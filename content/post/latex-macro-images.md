---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "LaTeXで連番画像の挿入をマクロ化する"
subtitle: ""
summary: ""
authors: []
tags: []
categories: [Tech]
date: 2020-12-08T17:50:23+09:00
lastmod: 2020-12-08T17:50:23+09:00
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

レポートで大量の連番画像を挿入する必要があった。
しかし```\includegraphics{1.png}```みたいなのを何個も書くのは嫌だったのでマクロ化した。
思ったより工夫が必要だったのでせっかくなので記事にした。

## やり方
以下のようにファイルを配置する。
```
  ┣ figure/
  ┃   ┣ 1.png 
  ┃   ┣ 2.png
  ┃   ┣ 3.png
  ┃   ┣  ・
  ┃   ┣  ・
  ┃   ┣  ・
  ┃   ┗ 10.png
  ┃
  ┗ main.tex
```

以下のコードをコピペする。
```LaTeX
\documentclass[uplatex,dvipdfmx]{jsarticle}
\usepackage{graphicx}

\newcommand{\filepath}[1]{#1}

\newcount\cnt
\def\includeFigLoop{
	\cnt=1 \loop\ifnum\cnt<11

	\section{連番\number\cnt }
	\begin{figure}[p]
		\centering
		\includegraphics[width=150mm]{figure/\filepath{\number\cnt}.png}
		\caption{連番\number\cnt の画像}
	\end{figure}

	\advance\cnt by1\repeat
}

\begin{document}

\includeFigLoop

\end{document}
```

## 解説
Qiitaの記事[^1]を参考にしてループを作成した。
このループ内で```\number\cnt```を参照するとそのときのループのカウントが返る。

しかし```\number\cnt```を
```LaTeX
\includegraphics{figure/\number\cnt.png}
```
のように```\includegraphics```内で参照するとエラーとなってしまう。

ここで```\newcommand```で
```LaTeX
\newcommand{\filepath}[1]{#1}
```
のように変数の値を返すコマンドを定義する。

このコマンドを以下のように```\includegraphics```で使うことで連番画像の挿入をマクロ化することができる。
```LaTeX
\includegraphics{figure/\filepath{\number\cnt}.png}
```

この手法は変数をそのまま挿入できない他の場面でも役立つかもしれない

[^1]: https://qiita.com/tttamaki/items/b0f735a8ab1a5c524130