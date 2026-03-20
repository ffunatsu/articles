---
tags:
  - nim
  - openFrameworks
  - trussc
---
![trussc-nim-logo](https://github.com/funatsufumiya/TrussC-nim/raw/main/docs/trussc-nim-logo.png)

https://github.com/funatsufumiya/TrussC-nim

![of-nim-logo](https://github.com/funatsufumiya/of-nim/raw/main/docs/of-nim-logo-s.png)

https://github.com/funatsufumiya/of-nim

さて、各種アドオンでの検証も少しずつ進んできて、完成度の上がってきた感のあるoFとTrussCのnimバインディング。

自分もnimを久々に再開して、これほど良い成果がすぐに出るとは思っていなかったので感無量。それほど、nimlineやcppstl、そしてconfig.nimsやnim.cfgなどのnimのエコシステムが完成してきているということなのだと思う。実際、nimline等のラッパーレスなバインディングができるとは自分は思っていなかったので、これほどC/C++とnimがスムーズに連携できるようになっているのは知らなかった。

余談として、本当はnimを中心にした今回のものと別に、[loaf](https://github.com/danomatika/loaf)に対する[ofxLua](https://github.com/danomatika/ofxLua)のように、of-nimに対するofxNimと、trussc-nimに対するtcxNimを作りたいなぁとは思っている。

けど、そっちはCMakeやMakefileに基本的にnimのソースを混ぜることになるのだけれど、それはあんまりnimの開発陣やユーザ層は興味がないようで、大変そうな感じがするので今はちょっと保留している。まぁC++のソースをnimから出力したり、静的ライブラリ + .h を出力したりは普通にできるので、また気が向いたらやろう。そう思うとやっぱりnimはPythonみたいな位置付けのホスト言語寄りのグルー言語なのかな。

## nimバインディングで見据える未来

さて本題なのだけど、どうしてわざわざnimバインディングを作ったのかについて。

個人的に、C/C++は初心者には辛いものがあると思う。Zigもそうなのだけれど、なにか作りたいものがあるときに、C/C++やZig、Rustは、ちょっとシステムやコンピュータのことに目が向きすぎる。

よく、ビジネスロジックのことを考えるときに、システムやプログラムなどの知識やこだわりが足枷（あしかせ）になることに似ている。

そういう意味で自分が好きなのはやっぱりopenFrameworksでもTrussCでもなくProcessingやp5.js、[wonderfl](https://www.amazon.co.jp//dp/4862671098) が好きだし、実際TouchDesignerや[TiXL](https://tixl.app/)が好きな人は多いだろうし、UnityやUE、Godotなどを使う層も実際それほど（よほどのことがない限りは）コンピュータのことを気にしはしないはず。

でもC++やRustで書いていると、なんとなく自然にコンピュータのことに目が向きすぎるきらいがある。nimや[jank](https://github.com/jank-lang/jank)によってそれを払拭したかったし、何よりProcessingやwonderflのようなライトな手軽さをoFやTrussCにも持ってきたかった。

そうすることで、ライトに書きながらも自然と実行速度も最適化されるというnimの利点が最大限生かされ、UnityやUE、Godotに近い感触に近づくはずと思うし、[NimForUE](https://github.com/jmgomez/NimForUE)や[gdext-nim](https://github.com/godot-nim/gdext-nim)、[UnLua](https://github.com/Tencent/UnLua)などが目指す理想像もきっとそういうところにあるのだと思う。

<iframe width="426" height="162" scrolling="no" frameborder="0" src="https://matsubara0507.github.io/github-card/?target=jmgomez/NimForUE" ></iframe>

<img alt="nim-ue-logo" src="https://github.com/jmgomez/NimForUE/raw/devel/logo.png" width="300px">

自分もホントはLuaとLove2DやLoVRでそれを目指したかったけど、やっぱ（普通の）GC言語ではちょっと限界があったし、ポインタ等が触れてORCのようなリアルタイム特化GCのあるnimならではの強みはこういう分野にあると改めて思う。

[nelua](https://nelua.io/)もゆくゆくはこういう分野に使われていくのだろうけれど、まだ少し時期尚早っぽいので様子見かな。jankも同じく。

of-nimやTrussC-nimによって、Processingのようにコンピュータのことを気にしないで楽しくクリエイティブ・コーディングができる環境が整えばなと思う。そうやって欲が出てきたら、本家に触れてみたり、C/C++探求の長く険しい旅に足を踏み入れてもらってもそれはそれでいいんじゃないだろうか。（実際、nimのビルドエラーと戦うには同じくらいのC/C++の知識は必然的に必要になるけれども・・・。）