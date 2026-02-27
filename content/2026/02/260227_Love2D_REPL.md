---
tags:
  - love2d
  - lovr
aliases:
---
https://github.com/funatsufumiya/love2d-tcp-repl

https://github.com/funatsufumiya/love2d-repl

スクリプト言語といえばREPLだし、ClojureScriptLuaやFennelなどのLispをnREPL等でゆくゆくは開発したいなと思い、とりあえずREPL作った。

Love2D / LoVR の両用できるように作っていて、グローバルな値を確認したり、関数を動的に変更したりなどいろいろ便利だと思う。tcp-replのほうはtelnet経由でアクセスするのでUIの制約を受けず、replのほうはLove2Dだけで完結するので、ちょっとした値の確認などに便利と思う。