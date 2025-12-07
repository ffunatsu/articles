---
title: 251207 バイナリを古いサーバで動かす (muslを使ってlibc依存をなくす方法)
tags:
  - c
  - odin
  - musl
  - llvm
---
[先日書いた記事](https://funatsufumiya.github.io/articles/2025/12/251206_git-lfs-rust-cgi-server_%E4%BD%9C%E3%81%A3%E3%81%9F)のなかで、古いレンタルサーバでRust等のビルド済みバイナリ（実行ファイル）を動かすとき、Rust musl以外はGLIBCのバージョンが噛み合わずに（ `libc` が動的リンクされているために）動かないという話を書いた。かつ、muslであっても、 `/lib/ld-musl-x86_64.so.1` に動的リンクされるようになっていると、ルート権限のないレンタルサーバでは当然動かない。

先日は諦めてRustを使ったのだけれど、Odin言語で遊んでいたところ、以下の記事によって解決した。すごく嬉しい。

https://gaultier.github.io/blog/odin_and_musl.html

流れとしては、

- muslを先にビルドする
- いきなり exe 等を出力せず、まず `.o` （オブジェクトファイル）を出力する
- リンカを使って、できあがった `.o` とmuslのオブジェクトファイルをリンクさせて exe 等を作る

という流れになる。以下、一つ一つ説明する。**今回はOdinを例にとっているが、Odinでなくても良い**。（普通のC言語でもなんでも、`.o` が出力できるものなら可。）

## muslで lib/crt1.o と lib/libc.a を作る手順

[先の記事](https://gaultier.github.io/blog/odin_and_musl.html)中では arm ターゲットになってしまっているので、x86_64ターゲット、つまり非クロスコンパイルの場合、以下のようになる。

※ 実行前に `sudo apt install llvm-dev` 等が必要。前記事中にあるようにLLVMではなくGNU GCCを使うようにもできるらしいが割愛。

```bash
$ git clone --recurse --depth 1 https://git.musl-libc.org/git/musl
$ cd musl
$ RANLIB=llvm-ranlib AR=llvm-ar CC=clang ./configure --disable-shared
$ make -j 12
```

こうすると、`lib/crt1.o` と `lib/libc.a` ができる。この2つが超重要。

## オブジェクトファイルをリンクする (Odinの場合)

実際、Odinの場合はどうなるかを説明する。以下のHello Worldを例にとる。それでもOdinの場合は `.o` (オブジェクトファイル) は大量にできるので、フォルダを分けて出力させている。

```odin
package main

import "core:fmt"

main :: proc() {
    fmt.printf("hello\n")
}
```

上の `hello.odin` が、`hello/main.odin` として配置されていると仮定して進める。

```bash
$ mkdir build
$ cd build
$ odin build ../hello -build-mode=object
$ ls

# hello-runtime-error_checks.o
# hello-bufio.o                                 hello-runtime-heap_allocator.o
# hello-builtin.o                               hello-runtime-heap_allocator_unix.o
# hello-bytes.o                                 hello-runtime-internal.o
# hello-fmt.o                                   hello-runtime-os_specific.o
# hello-io.o                                    hello-runtime-os_specific_linux.o
# hello-main.o                                  hello-runtime-print.o
# hello-math.o                                  hello-runtime-procs.o
# hello-mem.o                                   hello-runtime-random_generator_chacha8.o                               hello-runtime-random_generator_chacha8_simd128.o
# hello-os.o                                    hello-runtime-udivmod128.o
# hello-reflect.o                               hello-strconv.o
# hello-runtime-core.o                          hello-strconv_decimal.o
# hello-runtime-core_builtin.o                  hello-strings.o
# hello-runtime-default_temp_allocator_arena.o  hello-time.o
# hello-runtime-default_temporary_allocator.o   hello-unix.o
# hello-runtime-dynamic_map_internal.o          hello-utf16.o
# hello-runtime-entry_unix.o                    hello-utf8.o

$ ld.lld *.o ~/dev/musl/lib/libc.a ~/dev/musl/lib/crt1.o -o hello-odin-musl
```

補足として、`ld.lld` は LLVMのリンカで、`sudo apt install lld` 等でインストールできる。ただ、LLVMである必要はないはず (GNUの普通の `ld` でも良いはず) だが、先の手順でLLVMで作っているので、GNUの場合はそちらもあわせたほうがいいのかもしれない。

また、先でできた `lib/libc.a` と `lib/crt1.o` のパスは別の場所から呼んでいる (今回は `~/dev/musl` に配置。)

これで、libc が静的リンクされたバイナリが完成するので、古いレンタルサーバや古いPCでも問題なく動作する。（このためだけにわざわざRust muslを使う必要がなくなって、とても嬉しい。）