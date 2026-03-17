---
title: 251209 v-libui / V言語実装を通してCを理解する
tags:
  - v
  - vlang
---
libui-ngのV言語実装を作った。

https://github.com/funatsufumiya/v-libui/

<iframe width="426" height="162" scrolling="no" frameborder="0" src="https://matsubara0507.github.io/github-card/?target=funatsufumiya/v-libui" ></iframe>

ベースにしたのは、libui-ngのexamples。

https://github.com/funatsufumiya/libui_examples

<iframe width="426" height="162" scrolling="no" frameborder="0" src="https://matsubara0507.github.io/github-card/?target=funatsufumiya/libui_examples" ></iframe>

## 作った動機と、V言語実装を通してCを理解する過程の話（ロゼッタストーンとしてのC2V）

本来の目的は、一つ前の記事で書いたように、Odinでlibui-ng実装をしたいのが最終目標。 （ → 【追記】[odin-libui](https://github.com/funatsufumiya/odin-libui) も記事を書いたあとすぐに実装・公開しました。実装した感触は[次の記事](https://funatsufumiya.github.io/articles/2025/12/251209_odin-libui) にて。）

が、いきなりOdinでbindgenするとCヘッダーを書き直す過程で苦労しそうなのが目に見えているので、まずはV言語のC2Vトランスレータ（`v translate` や `v translate wrapper`）に頼ってみることにした。

既存ライブラリとしては、 [trufae/v-uing](https://github.com/trufae/v-uing)というものが元々あったのだけれど、動かなかったので、しぶしぶ自分で書いた。

V言語のいいところは、**出力されるC言語もきちんと読めるものであるということ**。これがNimとの大きな違いで、Nimはぶっちゃけ[C/C++を新しいアセンブリ環境くらいにしか見ていない](https://slides.com/onqtam/hello_nim) （注: おそらくコミッター間で個人差はあるが、トランスレータの出力結果としては事実）ということで、大きなスタンスの違いが出ている部分。

元々自分がZig以外のいわゆるBetter Cに手を出せなかった理由が、FFIのCラッパーを書くのが大変というところ。Nimも実際そうで、特にNimが出力するC/C++は非常に読みづらく、デバッグが恐ろしく大変。だからNimは結局書かないし、既に動いているライブラリを使う以外のことはぶっちゃけ諦めている。Haxe等も同じく。

V言語は、トランスレータとしては珍しく、出力結果としてのC言語の可読性がとても高い。出力されたC言語も、普通にC言語として読めるようになっている。GCが入っているはずなのだけれど、例えばヒープではなくスタックであるべきところはできるだけ元ソースに忠実にそのまま変換されるので、美しいだけでなく実用性も高い。

…ちょっと脱線したけど、このおかげで、**他のBetter C/C++を使うときにFFIラッパーを書くための基礎にできる**。C2Vによって出力されたコードをいじって、おかしなところを見つけたら、出力されたC言語（`-keepc` 等をうまく使う）を読みながら少しずつ定義を直していく。こうすることで、正しいFFI用のヘッダーが作られる。

これが結果的にロゼッタストーンとなってくれるので、次の別の言語に移植するときに大事な資産になる。これが狙い。

そして、C2Vのコードをいろいろいじっているうちに、理解が進んで、構造もわかってくる。個人的にはどちらかといえばこの工程のほうが大事かなと思っている。

こう考えると、トランスレータ言語が大好きな自分として、V言語は本当に大切な道具になっているというのが改めてわかるし、V言語があるおかげで他のBetter C/C++の世界に飛び出していくことができる。こうしたロゼッタストーンはとても大事だと思うし、本当に制作者とメンテナー陣に感謝。