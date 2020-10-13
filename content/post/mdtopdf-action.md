---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Markdownから自動でPDFを出力するGitHub Actionを作成した"
subtitle: ""
summary: ""
authors: []
tags: []
categories: [Tech]
date: 2020-09-29T10:28:56+09:00
lastmod: 2020-09-29T10:28:56+09:00
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

LaTeXに詳しくない友人とちょっとした文書を共同で書くことになった。
お互いにちょうどよいのがMarkdownだと思ったのだが、いざPDF化してみようとするといろいろ難点があった。
そこで、GitHub ActionsでMarkdownを編集すると自動でLaTeX経由でPDF化するようなリポジトリを構築してGitHubのWebGUIでも編集でき、
もちろん従来のエディタでも編集できるようにした。

## 使用法
使用法について解説する。
まず、一般的なGitHub Actionsの使い方と同様に以下のようにファイルとディレクトリを配置する。
```
リポジトリのルートフォルダ/
  ┣ .github/
  ┃   ┗ workflows/
  ┃       ┗ main.yml
  ┃
  ┣ main.md
  ┗ .gitignoreとか
```

```main.yml```を以下のように記述する。
github-emailとgithub-usernameは自分のものに置き換える必要がある。
```yaml
# This is a basic workflow to help you get started with Actions
name: test

# Controls when the action will run. Triggers the workflow on push or pull request
# events but only for the master branch
on:
  push:
    branches: [ master ]

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
      with:
        persist-credentials: false # otherwise, the token used is the GITHUB_TOKEN, instead of your personal token
        fetch-depth: 0 # otherwise, you will failed to push refs to dest repo
    - name: Generate PDF documents
      uses: ./
      with:
        working_directory: test
        mode: mdtopdf
        root_file: test.md
    - name: Commit files
      run: |
        git add test/test.pdf
        git add test/test.tex
        git config --local user.email "github-email"
        git config --local user.name "github-username"
        git commit -m "Generate by GitHub Actions" -a
    - name: Push changes
      uses: ad-m/github-push-action@master
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}

```

## 実装
以下のように実装した。構造自体は[前回の記事](https://3rdjcg.dev/post/latex-github-action/)とほぼ同様である。

### Dockerイメージ
MarkdownをTeXに変換するソフトウェアとしてPandocが有名だが、これを直接GitHub Actionsで使用することはできない。
Pandocと周辺ソフトウェアをDockerイメージにまとめた[mdtopdf](https://github.com/p1ass/mdtopdf)があったのでこれを使用した。

### GitHub Actions
前に作ったAction[3rdJCG/latex-build-langja](https://github.com/3rdJCG/latex-build-langja)をもとに上述したDockerイメージを使用するように変更し、
mdtopdfのPDF以外のTeXやHTML出力機能も使えるようにオプションで対応したAction[3rdJCG/mdtopdf-action](https://github.com/3rdJCG/mdtopdf-action)を作成した。
