---
title: 251209 odin-libui
tags:
  - odin
---
[前の記事](https://ffunatsu.github.io/articles/2025/12/251209_v-libui_V%E8%A8%80%E8%AA%9E%E5%AE%9F%E8%A3%85%E3%82%92%E9%80%9A%E3%81%97%E3%81%A6C%E3%82%92%E7%90%86%E8%A7%A3%E3%81%99%E3%82%8B) では [v-libui]([https://github.com/funatsufumiya/v-libui/](https://github.com/ffunatsu/v-libui/)) として libui-ng のV言語バインディングを行ったけれど、その後すぐに [odin-libui](https://github.com/funatsufumiya/odin-libui/) としてOdin言語実装を完成させた。

https://github.com/funatsufumiya/odin-libui/

<iframe width="426" height="162" scrolling="no" frameborder="0" src="https://matsubara0507.github.io/github-card/?target=funatsufumiya/odin-libui" ></iframe>

[前の記事](https://ffunatsu.github.io/articles/2025/12/251209_v-libui_V%E8%A8%80%E8%AA%9E%E5%AE%9F%E8%A3%85%E3%82%92%E9%80%9A%E3%81%97%E3%81%A6C%E3%82%92%E7%90%86%E8%A7%A3%E3%81%99%E3%82%8B)で結構大々的にV言語の良さとかC2Vのロゼッタストーン的な良さを書いててあれだけど、[odin-c-bindgen](https://github.com/karl-zylinski/odin-c-bindgen)が優れていたため、ぶっちゃけV言語よりも簡単で、特にbindgen部分に触ることはなかった。

というかそれが当たり前なのかもしれない。bindgenはうまくいかないのが普通っぽく思ってたけど（LLVMでCヘッダをパースするのは限界があると思っていたので）、実はそうではないのかもしれない。

ちなみに odin-c-bindgen 以外に [runic](https://github.com/Samudevv/runic)というものもあるようで、こちらはOdinで書いたものをCから呼べるようにしたりなど、相互に変換できるらしい。これも追々使ってみよう。

V言語は、C言語の領域を呼ぶときには `C.xxx` という記法を使うので、モジュールがあるのにないような感じになってなんだかモヤっとしてたけど、Odinはそういうこともなく、シームレスに繋がっていて心地がよかった。`core:c/libc` からCの関数群も当たり前のように呼べるので、正直Cトランスパイラよりも不思議とCっぽいのは、Zigと同じ感想かもしれない。

ということで、Odinからクロスプラットフォーム (Win/Mac/Linux) なGUIライブラリが呼べるようになったので幅が広がり、前に少し書いた 

- [odin-webui](https://github.com/webui-dev/odin-webui) : Electron/Tauriライクなウェブ系UI
- [orui](https://github.com/andzdroid/orui): Raylib上でImGui的に即時UI

と合わせて、GUIの幅がすごく広がった。Odin、思ったよりもCとシームレスにかつ簡単に繋ぐことができるとわかったし、まるでスクリプト言語のような楽しい書き心地なので、これはもうV言語をすっ飛ばしてOdinばっかり書いていくかもしれない。
