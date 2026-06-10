---
tags:
  - jank
  - msys2
title: 260202 Jank on Windows MSYS2 に進展あり。ついに動作。
---
https://github.com/jank-lang/jank/issues/326#issuecomment-3832145310

JankのWindows対応、何かしらコミットしたいなと思いながら時間を割けないでいたら、他の方がコミットしてくれた様子。連絡いただけてありがたい限り。中身はまだ見れていないけれど、追って確認して動かしてみよう。動くとすれば楽しみ。
## 【追記】 無事動いた！

https://github.com/jank-lang/jank/issues/326#issuecomment-3850124650

上記にコメントした通り、無事にMSYS2 CLANG64上で動作させることができた。ただ依存関係やファイルパスの解決の問題で、通常のWindows上で動かすことはまだできていないけれど、MSYS2で動けば十分かなと思う。

注意点としては、LLVMの独自パッチ版が使われているのだけど、なぜか手元では必要な tar.zst が生成されなくて困ったので、コメント中にあるように以下の生成結果 (artifact) を利用してインストールした。（llvm-installに展開して、`./bin/build-clang -i` すればOK。）

https://github.com/ffunatsu/jank-win/actions/runs/21690298753/job/62548270230