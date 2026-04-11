---
title: 260411 Love2D-GVPlayerとLove2D-NDIのGC圧迫を修正
tags:
  - love2d
  - lua
---
https://github.com/funatsufumiya/love2d_gv_video_player

<iframe width="426" height="162" scrolling="no" frameborder="0" src="https://matsubara0507.github.io/github-card/?target=funatsufumiya/love2d_gv_video_player" ></iframe>

https://github.com/funatsufumiya/love2d-ndi

<iframe width="426" height="162" scrolling="no" frameborder="0" src="https://matsubara0507.github.io/github-card/?target=funatsufumiya/love2d-ndi" ></iframe>

依然[『GC怖い病』 (26/03/06)](../03/260306_GC怖い病)として書いてたときに余裕がなくて直せなかったものを直した。内容としては、love2d側のAPIに `Object:release()` がきちんと存在していて、GCのタイミングまで待ってオブジェクトが溢れてFPSが低下する前に、きちんと毎フレーム `Object:release()` することでメモリプレッシャーを下げることができた。

本当は release ではなくてメモリを使いまわしたりできないか検討したのだけれど、どうやらそういうAPIはLove2D 11 には存在していないようだったけれど、Java等と違って手動でメモリ管理する術があるというのが勉強になった。

これがあれば、GCに頼ることはなく済むし、メモリプレッシャーが高いのはたいてい `ImageData` と `ByteData` 、`CompressedData` などと種類が限られているので、その辺をチェックして、単に変数に再代入している箇所できちんと古いオブジェクトを `release()` するだけで良い。

結構シンプルだけど効果が大きいので、もしかすると LWJGLなどにも似たようなAPIが準備されていたりするのかもなーと思った。以前 [minimaxをクロスプラットフォーム対応](https://funatsufumiya.github.io/qiita_exported_articles/items/minimax%20-%20Clojure%E3%81%A7%E6%9B%B8%E3%81%8B%E3%82%8C%E3%81%9F%E3%80%81DirectX11VulkanMetal%E5%AF%BE%E5%BF%9C%E3%81%AE%E3%83%9F%E3%83%8B%E3%83%9E%E3%83%AB%E3%81%AA%E3%82%B2%E3%83%BC%E3%83%A0%E3%83%95%E3%83%AC%E3%83%BC%E3%83%A0%E3%83%AF%E3%83%BC%E3%82%AF/)させたときに、試しに毎フレーム画像を読み込むテストをして似たようなトラブルに陥ったので、同様のAPIがないかをまた探ってみれば、ClojureのオリジナルであるJVM版でうまくgamedevしていくこともできるかもしれない。