---
tags:
  - git
---


知らなかったけど、ghならありそうな気はしつつ調べてなくて、超簡単だった。

https://cli.github.com/manual/gh_label_clone

ラベルのExport/Importで調べると出てくるJSON系のスクリプトはこれがあれば不要で、`gh label list --json "name,color,description"` 等を使えば、ghだけでJSONをエクスポートすることも可能。