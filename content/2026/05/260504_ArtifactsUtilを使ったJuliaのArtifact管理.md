---
tags:
  - julia
---
[前の記事](260504_BinaryBuilder.jlその後)の続き。BinaryBuilder.jl を使わないArtifacts管理について。

まず先に、関連リポジトリを列挙して、後ほど構造を説明。
## SDL3

https://github.com/funatsufumiya/SDL3Test.jl

https://github.com/funatsufumiya/SDL3_prebuilt_jll

https://github.com/funatsufumiya/SDL3_prebuilt_jll_packager.jl

## libui-ng

https://github.com/funatsufumiya/LibUiTest.jl

https://github.com/funatsufumiya/libui_ng_prebuilt_jll

https://github.com/funatsufumiya/libui_ng_prebuilt_jll_packager.jl

## 解説

SDL3もlibui-ngも、似たようなリポジトリになっている。

（ちなみにこれがベストとかいうものではなく、これらの例はCのdllやsoを呼ぶ最低限の構成になっている点に注意。）

〇〇Test.jl というリポジトリが実際にArtifactsを利用しているリポジトリで、内部で 〇〇_jll に依存している。なお、公式パッケージ経由ではないのでgitで繋がっていて、パッケージ追加時にはJulia REPLから `] add https://github.com/xxx/yyy.git` で追加している点に注意。（Manifestを読まないと分かりにくいようになっているが、Manifestは自動生成なのでこの辺は多少不親切かも。）

〇〇_jll はArtifactsをまとめているだけのリポジトリになっていて、〇〇_jll_packager.jl の方で実際の生成物のアップロードを行なっている。この2種はたぶん分離しなくてよい部分もあると思うのだけれど、後者のgitに生成物を直接アップロードしているのに対して、それを deploy.jl を使って一度 gist に tar.gz としてアップロードしているので、個人的にgitが重くなるのを避けたくて分離している。

packager.jl では本当に単にgistにアップロードしているだけで、アップロード後、一時的にArtifacts.tomlに参考情報が書き出される。次のアップロードでは上書きされてしまう仕組みになっているので、READMEにメモを書いている。

〇〇_jll のArtifacts.tomlには、そのREADMEに書き残したメモをもとに、対応するOSやアーキテクチャを整理している。

今回はArtifactsUtilのサンプルにあったgistへのアップロードを使ったのだけれど、たぶん実際にはReleasesを使ったほうが管理はしやすいはず。その辺はArtifactsの仕組み上どうとでもなるので、HTTP(S)でアクセスできて権限のあるところにtar.gzがあれば良い。（gistの場合は個人使用ならPrivateでも問題ないし、認証系もシンプルなので、サンプルにgistが選ばれているのはそれが理由と思われる。）

Artifactsは、極端にいえばtar.gzのハッシュ値と、それがどのOSやアーキテクチャに対応しているかが書かれているだけのものなので、とてもシンプルな作りになっていて扱いやすい。

本当は先の記事のようにBinaryBuilder.jlでCIビルドできるほうが後世のためではあるのだけれど、例えばWindowsビルドやMacビルドが困難などの理由で、今回の方法を使ったり、あるいは併用するというのはアリだと思う。自分も今後できるだけ最初はBinaryBuilder.jlからクロスコンパイルできるよう努めつつ、難しい場合にこの方法をとりたいと思う。（次はReleasesをうまく使えるようにしたいな。）