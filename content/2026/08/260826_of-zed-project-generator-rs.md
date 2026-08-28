---
title: 260826_openFrameworksのZedプロジェクトジェネレータ
tags:
  - Zed
  - openFrameworks
---
https://github.com/ffunatsu/of-zed-project-generator-rs/

<iframe width="426" height="162" scrolling="no" frameborder="0" src="https://matsubara0507.github.io/github-card/?target=ffunatsu/of-zed-project-generator-rs" ></iframe>

前々からやりたいなーと思っていた、[openFrameworksのVSCode用プロジェクトジェネレータ](https://github.com/ffunatsu/of-vscode-project-generator-rs)のZedへの移植について、ようやく達成できた。

思えば、このアドオンを作った頃はCodyというAIアシスタントを愛用していて、自分の手でスクラッチで書いた[シェルスクリプト](https://github.com/ffunatsu/of-vscode-project-generator-sh)を、CodyにRust化してもらっていたのを思い出す（***追記*** : READMEを確認したらCodyではなくGitHub Copilotを使ってました）。今回同じような感じでAntigravityにZed対応してもらった。

Codyとのやりとりは、結構大変だったような印象があったけれど、Antigravityはかなり進化していて、ほとんど手間がかかることなく、意図もすんなり読み取って実行に移してくれるのですごい。

やっぱり、生成AIは既存のものを翻訳したり翻案するのにはとても向いている。新規に作るものについては、自分の楽しみもあって自分でクリエイティブ・コーディングしながら書いていきたいけど、今回のように既存のものを何かに移すときは、費用対効果を考えながらうまく使っていければと思う。