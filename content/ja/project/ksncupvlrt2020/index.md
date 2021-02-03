---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "KOSENation CUP VALORANTのデザイン"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2021-02-01T12:18:17+09:00
lastmod: 2021-02-01T12:18:17+09:00
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

高専生・OBOG限定のesports大会KOSENation CUP VALORANTを開催した。

代表としての仕事以外にWebサイトやルールブックの制作・各種デザインなどを行った。
一つのイベントの各種広報用画像を効率的に制作する方法に関していい知見が得られたので記事としてまとめる。

---
## 統一デザインの決定
---
この大会の広報向けに統一したデザインが必要だと思ったので、まず配色とフォントを決めることにした。

### 配色
インターンに行った時に他のインターン生から色は極限まで少なくして統一すべきという教えを受けたのでこれを実践した。

まずその配色を決めなければいけないのだが、配色はcoolorsで自動的に生成したものを使った。
coolorsはスペースキーを押すだけでいい感じの配色を自動生成してくれるサイトだ。[^1]
参考までに今大会で使った色は以下の通りだ。

https://coolors.co/e75a7c-fcf7ff-2c363f-2a9d8f

### フォント
同様にフォントも統一した。[^2]
ロゴ用にDirectors Gothic 220 SemiBold Obl、通常フォントにはNotoフォントを使用した。

---
## 広報戦略
---
### Twitter
大会を開催するためには当然参加者=選手の応募が必要不可欠だ。
狙う年齢層的に広報をTwitterに絞って行うこととした。

他のメンバーと方法について掘り下げていくとTwitterを使った広報は画像を中心にする方針が決まり、
僕はひたすら1920x1080の画像を作る作業にいそしむことになった。

こうして大まかな方針が決まったのだが、一番重要な一番初めの大会告知をどのようにするか悩んだ。
Twitterは4枚画像が貼れるので、これを全部使うこととして何が必要なのか考え、以下の結論に至った。
- 気分を盛り上げるキャッチーなキーアート
- 1番伝えたい広報内容
- 参加欲を煽る賞品
- 参加の手順

これらを踏まえて画像を作って、以下のようなツイートになった。

{{< tweet 1312693947047841797 >}}

これらの画像はAdobe Photoshopで作った。
(後述するが以降は画像の作成がとんでもない量になって大変だったのでAdobe XDを用いている)

### Webサイト
小規模な大会だとTwitterとGoogleフォームとDiscordだけで広報と応募受付をすましている場合もあったが、
今後を見据えてWebサイトを作りたいと思った。

しかし、お金がかかるといろいろめんどくさい[^3]のでお金がかかる方法は嫌だった。
そこで当サイトでも使っているHUGO+Netlifyの組み合わせでサイト構築をすることとした。
こうするとドメインしかお金がかからないし、Netlifyのドメインならタダにできる。

特にHUGOは記事の書きやすさという点でもメリットが多かった。
運営メンバー全員がプログラミング経験があるわけではないので、Markdownで書けるのはかなり便利だった。

またGitHubにソースを置いてNetlifyに自動デプロイさせるようにしていたので、
GitHubのWebGUI上で書いた内容が自動的に反映できるようにもできた。
かなり情報系じゃない人向けの環境にできたとおもう。[^4]

完成したサイト : [KOSENation CUP VALORANT](https://kosenation.netlify.app/event/kosenationcup/)

[^1]:これも他のインターン生から教えてもらった
[^2]:といいつつ日本語フォントはちょくちょく変えて試してみたりしたので違うのが混じっているかもしれない
[^3]:メンバー3人で割り勘なのであんまりお金かけるのはきまずかった
[^4]:でも結局おしえるのはちょっと苦戦した、もう少しいい方法があるかもしれない
