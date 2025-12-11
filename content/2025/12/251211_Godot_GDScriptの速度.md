---
tags:
  - godot
  - gdscript
---
https://www.youtube.com/watch?v=UgRHjLv3HbY

<iframe width="560" height="315" src="https://www.youtube.com/embed/UgRHjLv3HbY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

 こういう動画は今までいくつかあったけど、GDScript / C# / C++ の比較が主だった。RustやPython、Nimなども含めて比較している動画はなかったように思う。
 
 ちなみに今まで有名だった動画は以下等。
 
https://youtu.be/hZvCCvo3AHc

<iframe width="560" height="315" src="https://www.youtube.com/embed/hZvCCvo3AHc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

いずれにしても、プロトタイピング以外の用途でGDScriptを使うにはちょっと怖いなと思う結果が出ている。今まで体感として、ノードが増えると劇的に遅くなる印象があって、下記動画と同じような印象を持っている。

https://www.youtube.com/watch?v=og1htFnj3_8

<iframe width="560" height="315" src="https://www.youtube.com/embed/og1htFnj3_8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

ではどうすればいいか、というところだけれど、とりあえずPython使うっていうのはアリだと思う。実際 [py4godot](https://github.com/niklas2902/py4godot) 使って、あたかもPythonがGodotに組み込まれているんじゃないかと思うくらい使いやすかった。

次点としては [lua-gdextension](https://github.com/gilzoide/lua-gdextension) かな。LuaJITもあるし、こちらもPythonと同じくエディタとの統合性が高くて、luaを自然に扱えた。（他のNim等は、やはり別環境という印象が否めないし、動かない環境もあったりする。）

でも結局のところは、最初の動画でも結論づけているようにC#を使うのが無難だと思う。自分も結局はC#使っていくんじゃないかな。（他の人にアドオンを初期設定してもらう手間などもあるし、よほどPythonのライブラリが使いたい等がなければ。）