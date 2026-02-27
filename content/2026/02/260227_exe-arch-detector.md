---
tags:
  - windows
  - windows-arm
---
https://github.com/funatsufumiya/exe-arch-detector

Windows ARM、いわゆるSnapdragon XのノートPCをここ半年ほど愛用しているのだけれど、exeが x86_64 用なのか ARM64 用なのかの判定はすごく大事になる。これをしないとdllの依存関係などがうまく解決できない。

簡単な方法としては git bash 上で file コマンドを使う方法がよく出てくるものの、例えば普通のexeに画像ファイルなどを埋め込んだ lovr などのいわゆるzip形式のexeはうまく判定されずzipであると表示されてしまう。

```bash
$ file "/c/Users/fu/apps/lovr/lovr"
/c/Users/fu/apps/lovr/lovr: Zip archive, with extra data prepended
```

そこで、以下StackOverflowの回答をもとに、使いやすいよう少しコードを修正した [`exe-arch-detector`](https://github.com/funatsufumiya/exe-arch-detector) を作った。GitHub上にexeを同梱しているので、ダウンロードして実行すれば使える。Releasesのやつも中身は同じ。

https://stackoverflow.com/questions/54834984/how-do-i-determine-the-architecture-of-an-executable-binary-on-windows-10/54835107#54835107

実行すると、exeのCOFF headerというものを読み取った結果が表示されて、x64なのかARMなのかがわかる。

一応それ以外のアーキテクチャ、例えばPowerPC用とかでも読み取れるらしいけど、例えば i386 用のexeだと以下のように出る。Windowsはもともと32bitのexeも64bit上で問題なく実行できるので、実はいろんなアーキテクチャのものが混在していて複雑なのだと思い知らされる。

```bash
$ exe-arch-detector.exe projectGenerator.exe
# Machine: 0x014c
# IMAGE_FILE_MACHINE_I386
# Intel 386
```

なお先ほど判定に失敗していた lovr をこれでやり直すと以下のようにちゃんと判定できる。

```bash
$ exe-arch-detector.exe C:\Users\fu\apps\lovr\lovr.exe
# Machine: 0x8664
# IMAGE_FILE_MACHINE_AMD64
# x64
```