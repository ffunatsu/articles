---
title: 260309 Just use maps … を静的言語ではどうするか
tags:
  - clojure
  - cpp
  - nim
---
https://www.youtube.com/watch?v=aSEQfqNYNAc

<iframe width="560" height="315" src="https://www.youtube.com/embed/aSEQfqNYNAc?si=Y-NrTVVqX6A30LKh" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Clojureでは当たり前のことなのだけれど、じゃあnimとかの静的言語ではどうするかというのが今から書く内容。

Luaではテーブル、JavaScriptではオブジェクト（プロトタイプ）が、構造体として唯一必要なものとなっている。Clojureでもそう。

もちろんこれらは動的言語の特徴で、テストケースなどを網羅しないと問題が出るのは確か。ただ、ファスト・プロトタイピングにおいてこれほど強いものはないように思う。

C++やnim等の静的型付け言語で、型が複雑になって面倒で、同様のものがほしいとき、どうするか。
## JSONオブジェクトを使う

結論からいえば、jsonを使うとたいていうまくいく。

C++ならnlohmann json、nimならstd/json。

ちなみにnimで実際std/jsonを使うと、配列 (JArray) を破壊的変更しようとするとうまくいかなくて困るのだけれど、以下のフォーラムによるとelmsを使うとうまくいくとのこと。

https://forum.nim-lang.org/t/8753

```nim
import std/json

var j = %* [1,2,3,4,5]
j.elems[2] = % 10

echo j # [1,2,10,4,5]
```

JSONで表現できないデータ形式は少ないし、nimならmarshall/unmarshallで好きなオブジェクトを変換するのもうまくいく。C++のnlohmann jsonも、変換は定義が必要にはなるけどそこまで面倒ではない印象。

余談だけれどRustで同様のものを見つけるのが結構大変で、以下のrust_dynamicとかだろうか。Anyもちょっと使い勝手が悪かったりして、serde_jsonなんかは用途が違うし、Rustはあまり動的型との相性はよくない。

https://crates.io/crates/rust_dynamic

ちなみに全部JSONオブジェクトにすればいいかというと、Clojureのようにうまくいくわけではないので、実行効率が悪くなったり、書き心地が悪くなったりのオーバーヘッドがどうしても出る。これは静的言語の宿命か。
## (Tagged) Union 使う

次点として、Odinでいうところの [(Tagged) Union](https://odin-lang.org/docs/overview/#unions)を使う。Cのunionは違うものなので、typeidなど識別するものが必要になる。typeidがタグにあたるので、Tagged Unionと呼ばれているのだと思う。

https://zenn.dev/nekosato/books/816dcc4efcab67/viewer/a0dd6d

```c
struct object {
    enum {
        O_INT,
        O_LONG,
        O_DOUBULE,
    } type;
    union {
        int *o_int;
        long *o_long;
        double *o_double;
    } value;
};
```

Variant（バリアント）と呼ばれることもあり、RustのEnumは（上記実装とは異なるけれど）Tagged Unionそのものといってもいい。

Tagged Unionが標準で存在する言語は強い。RustとOdinが使いやすいのはその辺。Rustはパターンマッチが強いので、動的型がなくてもうまくいくのはこういうところにある。

ただ欠点としては、万能型というのがありえなくなるので、型だらけになる。でもTagged UnionやVariantは動的型よりも実行効率が良いことが多いので、一長一短といえる。

Tagged UnionはUnionに列挙したなかで最長のメモリサイズを事前に確保することが多いので、場合によっては無駄な実装になることもある。（上記の例のようにポインタ = `uintptr_t` という同じサイズのものであれば問題にはならないけれど、ポインタなら別に実体を置く場所が必要になる。）
## まとめ

他にも例えばCなら `void*` （つまり生ポインタ）を使うとか、動的型を表現する方法はいろいろある。結局、実行時にしかメモリサイズが決定されないことや、もしポインタを使えば実体をどこに置くかが問題になったり、あるいは余分なメモリ領域が使われたり、どこかにオーバーヘッドは出てくる。

ゼロコスト抽象化はこうしたオーバーヘッドを減らすので、静的言語で動的型を使うのを避けるのは、結局オーバーヘッドが嫌だというバイアスがだんだんかかるからだろうな。（それに結局は型システムあったほうが大規模プロジェクトではテストが楽だったりする。）