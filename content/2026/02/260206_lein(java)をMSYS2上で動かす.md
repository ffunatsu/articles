---
title: 260206 lein (java) をMSYS2上で動かす
tags:
  - jank
  - java
  - clojure
  - msys2
---
[lein (leiningen)](https://leiningen.org/) をMSYS2上で動かす。スクリプト自体はMacやLinuxと同じものが使えるのだけれど、javaコマンドをどうするか。

たぶんもっといい方法はあると思うけれど、自分は単純に、`winget install Microsoft.OpenJDK.25` でWindows本体側にOpenJDKを入れて、`which java` (`where.exe java`) によって javaコマンドの場所を探す。自分の場合は `C:\Program Files\Microsoft\jdk-25.0.2.10-hotspot\bin\java.exe` だったので、以下のようなシェルスクリプトを書いて、`java` という名前で保存して使う。

```bash
#!/bin/bash

# C:\Program Files\Microsoft\jdk-25.0.2.10-hotspot\bin\java.exe
/C/Program\ Files/Microsoft/jdk-25.0.2.10-hotspot/bin/java.exe $@
```

これでleinは問題なく動く。

ちなみにMSYS2上で使うとトラブりがちなRust系コマンドも同じように使うことができて、例えばripgrepなどは、Windows側でインストールして、以下のような rg という名前のシェルスクリプトを作れば普通に使える。

```bash
#!/bin/bash

/C/Users/funat/.cargo/bin/rg.exe $@
```