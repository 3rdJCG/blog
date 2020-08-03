---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Latex Github Action"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
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

# GitHub ActionsでLaTeXのビルドを自動化する

GitHub Actionsというサービスが始まっていた。
LaTeXのビルドを自動化できたらいいなと思って使ってみようとしたが、
他人の作ったActionではいつも使っているフォントが使えないことが発覚した。
そのためDockerイメージとActionを自作し、解決することにした。