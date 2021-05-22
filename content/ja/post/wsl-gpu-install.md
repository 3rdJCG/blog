---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "WSLでffmpeg+nvencで快適エンコード"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2021-05-23T07:15:26+09:00
lastmod: 2021-05-23T07:15:26+09:00
featured: false
draft: true

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

