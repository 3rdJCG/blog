---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "WSLでFFmpeg+NVEncで快適エンコード"
subtitle: ""
summary: ""
authors: []
tags: [WSL, CUDA]
categories: [Tech]
date: 2021-05-23T07:15:26+09:00
lastmod: 2021-05-23T07:15:26+09:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

PCゲームをしているとNVIDIA ShadowPlayなどでプレイ中の動画を大量に録画するようになった。
しかしこれらのソフトで録画されたファイルはサイズがあまりにも巨大すぎるので、エンコードしたいと思った。
ファイルの数が膨大なので自動化したいが、
この手の自動化作業をWindowsでやるのは嫌だったのでWSLを使うことにした。

CPUエンコードだと時間がかかるのでGPUエンコードを用いることにしたが、
WSLから使用するのが少し難しかったので記事として残す。

## 前提条件
WSL Ubuntuがインストールされていて、使用できる状態である。

## セットアップ
各種必要ソフトウェアを導入していく

### NVIDIA Drivers for CUDA on WSLのインストール
まずNVIDIA Drivers for CUDA on WSLのインストーラを以下からダウンロードする。
NVIDIAのアカウントが必要。
- [CUDA on Windows Subsystem for Linux (WSL) - Public Preview](https://developer.nvidia.com/cuda/wsl)

ダウンロードされたインストーラを実行するとインストールが完了する。

### WSL側でのCUDAのインストール[^1]
以下の内容はこの記事を大変参考にさせて頂きました。
- [待ってました CUDA on WSL 2](https://qiita.com/ksasaki/items/ee864abd74f95fea1efa)

WSL側でCUDAソフトウェアをインストールする。
以下のコマンドでCUDAのリポジトリを登録して、インストールする。
```sh
sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
sudo sh -c 'echo "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda.list'
sudo apt-get update
sudo apt-get install -y cuda-toolkit-11-2
```

```sh
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.2.0/local_installers/cuda-repo-wsl-ubuntu-11-2-local_11.2.0-1_amd64.deb
sudo dpkg -i cuda-repo-wsl-ubuntu-11-2-local_11.2.0-1_amd64.deb
sudo apt-key add /var/cuda-repo-wsl-ubuntu-11-2-local/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda
```

### NVEncのビルド[^2]
以下の内容はこの記事を大変参考にさせて頂きました。
- [CUDA in WSL2を試す](https://rigaya34589.blog.fc2.com/blog-entry-1259.html)

NVEncに必要なFFmpeg4を導入する。
```sh
sudo add-apt-repository ppa:jonathonf/ffmpeg-4
sudo apt update
sudo apt install ffmpeg \
libavcodec-extra58 libavcodec-dev libavutil56 libavutil-dev libavformat58 libavformat-dev \
libswresample3 libswresample-dev libavfilter-extra7 libavfilter-dev libass9 libass-dev
```
以下の手順でコマンドを実行する。
```sh
sudo ln -f -s /usr/lib/wsl/lib/libcuda.so /usr/local/cuda/lib64/libcuda.so
git clone https://github.com/rigaya/NVEnc --recursive
cd NVEnc
./configure
make -j16
```
参考先の記事ではビルドに失敗してしまうとの内容だったが、
本記事で紹介した方法でCUDAをインストールするとビルドに成功した。

## エンコードを試す
試しにGPUを使用してエンコードを行う。
```sh
ffmpeg -i input.mp4 -vcodec h264_nvenc output.mp4
```

## エンコードを自動化する
以下のシェルスクリプトを書いた。
```sh
#!/bin/bash

OLDIFS=$IFS
IFS="
"
inputdir=$1
outputdir=$2

echo "[enclips] Encording clip videos..."
echo "[enclips] reading dir=$inputdir"
echo "[enclips] output dir=$outputdir"

count=1

for file in `\find $inputdir -maxdepth 1 -name '*.mp4'`; do
    filedate=`date "+%F-%H%M%S" -r $file`
    ffmpeg -i $file -n -b:v 8000k -s 1980x1080 -r 60 -vcodec h264_nvenc ""${outputdir%/}/${filedate}".mp4"
    count=`expr $count + 1`
done

IFS=$OLDIFS

echo "[enclips] Finished encording"
```

## 追記
普通に考えてWindowsにFFmpegのバイナリいれてPATH通して、
```sh
ffmpeg.exe -i $file ~~~~~~
```
したほうがパフォーマンス出るしWSLにCUDAいれる手間もいらない。
バッチファイルもPowerShellスクリプトも書きたくないとはいえなんでこんな方法にしたんだ。
何を考えていたんだ俺は。

いつかWSLでCUDA使うかもしれないので記事は残す。

[^1]: [待ってました CUDA on WSL 2](https://qiita.com/ksasaki/items/ee864abd74f95fea1efa)
[^2]: [CUDA in WSL2を試す](https://rigaya34589.blog.fc2.com/blog-entry-1259.html)