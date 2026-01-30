---
title: 260130 catの代わりに bat -pP を使う
---
Windowsのcatは何かと使いづらい。UTF-8なのかShift JISなのかで挙動が変わるし、ターミナルの文字エンコーディングに左右される。

[bat](https://github.com/sharkdp/bat)を使うと、そうしたことは綺麗に解決する。 `cargo install bat` でインストールできるので手軽に使えるし、[Releases](https://github.com/sharkdp/bat/releases)よりバイナリを配置してインストールもできる。

https://github.com/sharkdp/bat

ただ、このままだとページャーが働いてしまってcatと挙動が異なってしまうので、`bat -pP` とするとcatに近い挙動になる。

シンタクスハイライトも効くし、パイプに渡してもcatと同じなので、例えばnushellなどでは `alias cat = bat -pP` とすれば、cat自体を置き換えることができる。

Powershellの場合は、以下のようにするとよいと思う。動作確認はしていないので注意。（[参考記事 (Qiita)](https://qiita.com/ikanamazu/items/5f29e09b80849a4efa58)）

```powershell
function cat(){
	bat -pP $args
}
```