---
tags:
  - jank
  - cpp
---
Clojurians Slackを毎日眺めていると、jankが意外に活発に開発されてエラー報告も積極的になされていて、自分も触発されて少し使ってみた。

stb_imageについてはClojurians Slackで上がっていたエラー報告の検証用、GLFWはシンプルにOpenGLでウィンドウ出して遊びたかった。

https://github.com/funatsufumiya/jank-stb_image-test/tree/dev

<iframe width="426" height="162" scrolling="no" frameborder="0" src="https://matsubara0507.github.io/github-card/?target=funatsufumiya/jank-stb_image-test" ></iframe>

https://github.com/funatsufumiya/jank-glfw-test/

<iframe width="426" height="162" scrolling="no" frameborder="0" src="https://matsubara0507.github.io/github-card/?target=funatsufumiya/jank-glfw-test" ></iframe>

stb_imageのほうはClojurians Slackで報告があがっているように妙なエラーが出ていて、工夫をしないと動かなかったけれど、一応無事動いた。（devブランチの方がストレート。mainにマージしたいけれどSlackのコメントとの流れを鑑みて一旦保留。）

GLFWの方はまぁ普通で、とはいえOpenGLのウィンドウが出るとやっぱちょっと感動した感じはある。あとこれが普通にWin/Mac/Linuxでクロスプラットフォームで動いていることは、C++では当たり前なのだけれどjankがそうであることにちょっと感激してしまう。

そもそもC++を動的にダイナミックに扱えるという経験が少ないので、自分はまだnREPLをうまく使えていないけれど、nREPLと組み合わせて対話的に開発できれば、結構面白い開発経験になるかもしれない。

GLFWのexampleは、正直現時点ではLispとC++の文法の違いから移植コストの方が高く付くだけになってしまっているので、それ以上の楽しさを見出したいなと思うところ。前なら素直に楽しくコーディングしていったところだろうけれど、自然とnimと比較してしまうので、基本的にはnimメインで、暇をみて遊んでみよう。（正直現時点では、nimの方がC++の文法と親和性が高くてコピペしやすくて助かる。）

ちなみにjankに移植して一番大変だったのは、jankの関数呼び出しが型にかなり厳密だということ。例えばC++なら`NULL`で呼び出せるところが`nullptr`じゃないとポインタ型の呼び出しだと判定されずに型エラーで関数が呼び出せなかったりなどは日常茶飯事。`not`と`cpp/!` が違う（特に`cpp`の値を渡したときに`not`だと反転されない）のには戸惑った。たぶんだけど `not` な `cpp-value` というのは、何らかのC++の値があれば true 判定でそれが not で false になるっぽくて、極端なことをいえば `cpp/false` でも `(cpp/int 0)`  でも not すると true になって頭がおかしくなりそうだった。

まぁ終始こんな感じで、jankではjank objectの世界とcpp valueの世界がほぼ完全に分離されていて、あまりシームレスに行き来できるとは言い難い状況なので、nim以上に大変な部分はある。

それでも自分は結構楽しいなぁと思いながら書き進めていて、現時点でjankは個人的には実用レベルにようやく達したと感じているので、もう少し時間をかけて遊んでみよう。