---
tags:
  - nim
title: 260322 awesome-nim がすごい
---
## awesome-nimを見ようと思ったきっかけ：nimらしさの探求

[昨日書いた記事](./260320_TrussCやoFのnim言語版で見据える未来) で、nimのフレームワークを2つ作った話を書いたのだけれど、その2種のフレームワークで少しずつnimらしさを出せるようなexampleを少しずつ試していて、[nimscripter](https://github.com/beef331/nimscripter)を使った[サンプル](https://github.com/ffunatsu/TrussC-nim/blob/main/examples/nimscripter_reload_test.nim)をあげたりしている。

![nimscripter_reload_test image](https://github.com/ffunatsu/TrussC-nim/raw/main/docs/reload.png)

これくらいだと正直LuaをC++に埋め込んだのとあんまり大差なくて、Rを押すとスクリプトがリロードされるとか、ファイルの更新を検知してリロードするくらいでは、それほどnimらしさは出ていないかもしれない。

もちろん、ホスト言語はnim、スクリプト言語もnim (script) という部分のシームレスさはあるけれども、[AngelScript](https://www.angelcode.com/angelscript/)等を利用すればそれはC++でも実現できる。もちろん[Sol2](https://github.com/ThePhd/sol2)等ほどの埋め込みのしやすさはあるかは微妙だろうけれど、nimscript自体もJITでも何でもなく動作速度の問題等あり、nimに対して制約も大きいことから、あまりこれくらいでnimの強みが出ているとは思えない。

やっぱり、**わざわざC++ではなくそのトランスパイラを使うなら、わざわざトランスパイラを使う所以がほしい**。それが他の人たちへの、わざわざトランスパイラを使っている説得力となる。ORCとか画期的な機能がついていることは肌感覚では知っていても、やっぱり目に見える実感があるくらいの大きなポイントをおさえておきたい。

## 10年ぶりくらいにawesome-nimに目を通す

そこで、nimならどんなことができるのかと改めて[awesome-nim](https://github.com/ringabout/awesome-nim)を、下手すると10年ぶりくらいに読み解いてみたら、だいぶ進化していて驚いた。さすが1.0に到達し、さらに2.0に到達した言語だけはあると思った。

<iframe width="426" height="162" scrolling="no" frameborder="0" src="https://matsubara0507.github.io/github-card/?target=ringabout/awesome-nim" ></iframe>

特にnimらしさがあるなと思ったのは、まだ自分も全部を読み込んでいるわけではないけれど、[nimpylib](https://github.com/nimpylib/nimpylib) はPythonの文法を def や class も含めて、stdlib丸ごとnimに持ち込んでいるプロジェクトで、しかもこれ系のライブラリが一つじゃなく存在する。マクロによって文法ごと変えてしまうのは、いかにもnimらしいという感じがするし、トランスパイラ型Lispの皮を被った変態言語の片鱗を垣間見れる。

以下、Pythonとしてシンタックスハイライトしているけれど、nimの内部DSLとなっている。末恐ろしい。

```python
class Example(object):  # Mimic simple Python "classes".
  """Example class with Python-ish Nim syntax!."""
  start: int
  stop: int
  step: int
  def init(self, start, stop, step=1):
    self.start = start
    self.stop = stop
    self.step = step

  def stopit(self, argument):
    """Example function with Python-ish Nim syntax."""
    self.stop = argument
    return self.stop

def exa():
  e = Example(5, 3)
  print(e.stopit(5))

exa()
```

[weave](https://github.com/mratsim/weave) も似たような部分があって、並列処理をstate-of-the-artで実行できると謳っているが、文法がすごい。parallelForという文法がそもそもnimにあったんじゃないかくらい溶け込んでいる。

```nim
parallelFor j in 0 ..< N:
    captures: {M, N, bufIn, bufOut}
    parallelFor i in 0 ..< M:
      captures: {j, M, N, bufIn, bufOut}
      bufOut[j*M+i] = bufIn[i*N+j]
```

[razaberi](https://github.com/Beglaa/razaberi) や[patty](https://github.com/andreaferretti/patty)によるパターンマッチも同じくらいnimと溶け込んでいて、pattyの場合以下のようなパターンマッチが可能。ここまでくると、`match` が予約語ではなくて良かったなと思うし、`match` はこのために残されていたんじゃないかくらいに思えてくる。

```nim
match makeRect(3, 4):
  Circle(radius):
    echo "it is a circle of radius ", radius
  Rectangle(width, height):
    echo "it is a rectangle of height ", height
```

DSLとして個人的に感動したのは [shell](https://github.com/Vindaar/shell) で、以下のようにあたかもシェルスクリプト自体なんじゃないかと思えるものをnimにDSLとして記載することができる。

```bash
shell:
  touch foo
  mv foo bar
  rm bar
```

もうここまでくると、`nim` としてシンタックスハイライトをかけても無意味なくらいに改変されていて（上は実際 `bash` としてシンタックスハイライトさせたほうがしっくりきたのでそうしている）、以下のように変数内包記法ですら改変されている。やっぱnimはPython風の記法という皮を被ったただのLispなんだろうなと思えてくる。

```nim
let fname = "myimage"
shell:
  convert ($fname).png ($fname).jpg
```

ここまでLispっぽいASTの改変が行えて、それでいて動的ではない静的なトランスパイラで、型付きで成功している言語は、実はnim以外にはあまりないんじゃないかと思えてくる[^1]。

V言語もCトランスパイラだけれど、方向性としては真逆で、V言語はGo言語ゆずりのシンプルさを謳っていて、nimはとにかく自由なRubyっぽい風土と思想を持っている。

そう思うと、やっぱり言語選びというのは、哲学と直結するのだなと思うし、自分のようにいろんな言語や環境を旅するように渡り歩いている人間としては、Lisp的なDSLを自由に内包してあれこれ変幻自在にあたかも別言語のように振る舞える言語をメインに選ぶというのは、ある意味で必然性があったのかもしれない。
## さらに10年後はどうなっているだろう ～ 『7つの言語 7つの世界』を1つの言語で…？

自分のライフワークとして、いろんな言語を探求する旅は、たぶん今後もずっと続く。葬送のフリーレンではないけれど、役に立たない民間魔法のようなマイナー言語をあれこれつまみ食いしては、時々そのエッセンスを活かしてDSLという魔法として実践に活用していく、そんな未来があるのかもしれない。

nimを見ていると、自分の座右の書である『7つの言語 7つの世界』を一つの言語で体現しているようにすら思えてきて、1つの言語にいくつの世界があるのだろうと思えてくる。前述のコードも、何通りのシンタックスハイライトを試せばいいのかわからないくらいで、きっとそれが、nimらしさなのだろう。

nimも自分も、またさらに10年後に何をしているかが楽しみだ。きっと、いつ見ても何をしているのかわからないくらいに、いろんなことをやっているのだろう。Lispと同じように、きっとそれが好かれたり嫌われたりするのだろうけれど、その多様性をnimはきっと今後も（マクロなどのメタな共通項を見出しながら）許容していくのだろうな。

[^1]: マクロのある静的言語でトランスパイラといえば、[Haxe](https://haxe.org/)があるけど、FlashもAdobe Animatorも公式に終了してしまったし、今は使われているのだろうか…？ある意味でActionScript（あるいはECMAScript4）という消えた遺産がHaxeという形で残っているのは嬉しくはあるし、Nekoに加えて新たにHashLinkというVMができたり、動きはあるっぽいけど…。（個人的にはHaxeの言語性も相まってC#/Mono/.NET Coreにいろんな意味で道を譲っているような気がするけれど、JSターゲット、ネイティブターゲットとかトランスパイラとしての違いもあるので、そうした部分で生き残っていくのかもしれない。そう思うとnimと似た立ち位置ではあるか…？）
