---
title: 260413 ユグドラシル（世界樹）～ BinaryBuilder.jl
tags:
  - julia
---
先に書いた [Cxx.jlの代替手段に関する記事](260413_Cxx.jlの代替)に続いて、[JuliaPckaging/Yuggdrasil](https://github.com/JuliaPackaging/Yggdrasil)についてちょっと感激したので、それについて少しだけメモしておきたい。

https://terasakisatoshi.github.io/MathSeminar.jl/slideshow/binarybuilder/build/

<iframe width="612" height="426" scrolling="no" frameborder="0" src="https://terasakisatoshi.github.io/MathSeminar.jl/slideshow/binarybuilder/build/"></iframe>

JuliaのDLL管理 (JLLについて) は、上のごまふあざらしさんのスライドに綺麗にまとまっているので、現段階でこれ以上情報を持っているわけではないのだけれど、自分は純粋に「[ユグドラシル（世界樹）](https://github.com/JuliaPackaging/Yggdrasil)」の思想にちょっと感動した。

https://github.com/JuliaPackaging/Yggdrasil

<iframe width="426" height="162" scrolling="no" frameborder="0" src="https://matsubara0507.github.io/github-card/?target=JuliaPackaging/Yggdrasil" ></iframe>

[![Yuggdrasil-logo](https://user-images.githubusercontent.com/1282691/177174254-aa90664e-5c20-4ea3-9938-34de961dc198.png)](https://github.com/JuliaPackaging/Yggdrasil)

というのも、nim言語等でラッパーを書いたときに、dll関連が手に入らなかったりビルドできなかったりして使い物にならなくなってしまっているライブラリは実際多く存在しているし、RubyやPython、Node等で、特にnode-gyp等のビルドに失敗してしまったり、最近では少なくなったけれど、pipでインストールできなくてcondaに頼ったりなどは多くあって、いわゆるdll地獄は誰しもが経験していることだと思う。

それをDockerなどを使って事前にコンパイルして一箇所に集約して保持しておこうというのは、aptなどのLinux系のパッケージマネージャーなどではよく行われているように思うのだけれど、それを1言語で、しかも複数のプラットフォーム向けに網羅的に提供しているというのは結構珍しいかなと思った。

これと似たことができている言語はPythonのAnacondaやUVを除けば、あとはRustくらいだし、しかもRustは有名なパッケージかつ純粋なRust実装のみで、dll等を含むsysクレートは自前のビルド等に失敗しがちなので、そう考えるとどの言語にもできていない画期的な試みをLinuxパッケージマネージャ並に管理してマルチプラットフォーム展開しているのはすごいことだと思う。

これがあれば確かにnim言語のようなdllビルドできなくて困る問題は解決するし、最悪自分でビルド手順をみてビルドすればいいし、特に初心者にとってはありがたい選択肢だなと思った。

自分の最近試したC/C++呼び出しのテストも、dllを直接配置するのではなくてこのJLL(Wrappers)に早く移行できたらなと思うけれど、もう少しやり方などを調査してみてからにしよう。SDL3とかlibuiとかよく使うものはもう既にユグドラシル（世界樹）にストックされているかもしれないし、もし既にあったらありがたく使わせてもらおう。