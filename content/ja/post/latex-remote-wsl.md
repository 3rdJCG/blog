---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Remote WSLを使ってたのしくLaTeX"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2021-04-24T09:40:40+09:00
lastmod: 2021-04-24T09:40:40+09:00
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

VSCodeのRemote WSLを使うとLaTeX執筆を行うとすごいべんりだったので設定方法を書いておく。

## 前提条件
以下の前提で進める
- そもそもWSLが使える
- WSLにtexlive-fullがインストールされている
- WindowsにVSCodeがインストールされている
- VSCodeに拡張機能Remote WSLとLaTeX Workshopがインストールされている

## VSCodeの設定
settings.jsonに以下の設定を追加/上書きする。

```json
"latex-workshop.latex.tools": [
    {
        "name" : "latexmk",
        "command" : "wsl.exe",
        "args" : [
            "latexmk",
            "%DOCFILE%.tex" 
        ]
    },
    {
        "name" : "uplatex",
        "command": "wsl.exe",
        "args": [
            "uplatex",
            "-kanji=utf8",
            "-interaction=nonstopmode",
            "-file-line-error",
            "--shell-escape",
            "%DOCFILE%.tex"
        ]
    },
    {
        "name" : "platex",
        "command": "wsl.exe",
        "args": [
            "platex",
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "-kanji=utf8",
            "-guess-input-enc",
            "--shell-escape",
            "%DOCFILE%.tex"
        ]
    },
    {
        "name" : "dvipdfmx",
        "command": "wsl.exe",
        "args": [
            "dvipdfmx",
            "%DOCFILE%"
        ]
    }
],
"latex-workshop.latex.recipes": [
    {
        "name": "latexmk",
        "tools": [
            "latexmk"
        ]
    },
    {
        "name": "uplatex",
        "tools": [
            "uplatex",
            "dvipdfmx"
        ]
    },
    {
        "name": "platex",
        "tools": [
            "platex",
            "dvipdfmx"
        ]
    },
],

"latex-workshop.latexindent.path": "latexindent",
```

Remote-WSLに必要な設定だけ書き出しているので、
他の設定はLaTeX Workshopの使い方などを解説したほかの記事を参考にしてほしい。

普段の設定と変わらないように見えるが、```"command": wsl.exe```を追加するのがミソである。

また```"latex-workshop.latexindent.path": "latexindent"```によってShift+Alt+Fを押下すると
WSL上でlatexindentが実行されて自動成形が可能になる。

latexindentの設定については以下の記事が詳しい。
- [latexindentの設定をカスタマイズして使う](https://qiita.com/3rdJCG/items/0e70cdcc03080d9b93f6)

## つかいかた
VSCodeをひらいて左下の```><```みたいなマークをクリックし、Remote-WSL: New Windowを選択する。
フォルダーを開くから好きなフォルダを開いたらあとはいつものLaTeX Workshopと変わりない。

また、TeXソース上でShift+Alt+Fを押すと自動成型が行われる。

## おわりに
もはやWindowsにTeXLiveを入れる必要は本当にないのではなかろうか。
WSLでもっと快適なTeXライフを送りましょう！
