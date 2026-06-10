---
tags:
  - julia
---
[260416_BinaryBuilder.jl実験の途中経過](260416_BinaryBuilder.jl実験の途中経過) で途中になっていたものを少し進めた。（URLは前と同じ。）

https://gist.github.com/ffunatsu/f4534e38b9de47c4c37ff02e11955a92

（追記: 今後の管理がしやすいようにリポジトリも一応作った: https://github.com/ffunatsu/libui_ng_jll/）

Linux x86_64 の musl / glibc についてはビルドすることに成功したものの、それ以外のプラットフォームについてはそれぞれの理由で惨敗。

BinaryBuilder.jl が使っているコンテナ（イメージ）の仕様にもう少し仲良くならないと、ビルドを通すのは難しい様子。libui-ngのビルド、実機でのビルドはそんなに難しくないだけに、無念。

やっぱり、BinaryBuilder.jl やユグドラシル (Yggdrasil) を使うのは諦めて、Artifactだけ置いた独自gitを使いそうな予感がする。Juliaで新しいライブラリのサポートが結構ゆっくりなのは、どうしてもこの辺に由来している気がする。古い昔のライブラリは、過去にこのスクリプトを書いてくれている人がいれば保守は簡単でも、どうしても新しいライブラリは（CIで完全に再現性を保って保守するには）サポートが大変そうな予感。

(ArtifactsUtil.jl を使った方法については後日 [追記](260504_ArtifactsUtilを使ったJuliaのArtifact管理)。)