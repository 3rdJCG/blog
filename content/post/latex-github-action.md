---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "GitHub ActionsでLaTeXのビルドを自動化する"
subtitle: ""
summary: ""
authors: []
tags: []
categories: [Tech]
date: 2020-08-03T22:02:45+09:00
lastmod: 2020-08-03T22:02:45+09:00
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

GitHub Actionsというサービスが始まっていた。
LaTeXのビルドを自動化できたらいいなと思って使ってみようとしたが、
他人の作ったActionではいつも使っているフォントが使えないことが発覚した。
そのためDockerイメージとActionを自作し、解決することにした。

## Dockerイメージ
Dockerイメージ[latex-ci-notojp](https://hub.docker.com/repository/docker/3rdjcg/latex-ci-notojp)を作った。
LaTeXの日本語環境でのソースコード表示用にplistingsと日本語フォントとしてNotoフォントをイメージ内に組み込んだ。

## GitHub Action
[xu-cheng/latex-action](https://github.com/xu-cheng/latex-action)をもとに、作成したDockerイメージを使用するAction[3rdJCG/latex-build-langja](https://github.com/3rdJCG/latex-build-langja)を作成した。
本家同様引数やコンパイルオプションに対応したほか、
日本語環境特有の拡張機能の不具合などに対応するためTlmgrで拡張機能の追加でインストールできるようにしている。
