---
tags:
  - jank
  - msys2
---
[先の記事](./260202_Jank_on_Windows_MSYS2に進展あり。ついに動作。)でJankがMSYS2で動いたという報告をしたが、実際にjankで遊び始めた。

まずはGetting Startedをこなしてみている。

https://book.jank-lang.org/getting-started/02-hello-world.html

leiningen 等は特に問題なく動作していて、それだけでもすごいなと思って、`cpp/raw` が動いているのがすごく感動。JITでC++が動くというのは [CL-CXX-JIT](https://github.com/Islam0mar/CL-CXX-JIT) 以来なので、とても楽しい。型とか普通に解決して利用できているのがなんとも不思議でならない。
## leinからdllを呼び出すとき、エラーが発生する。

で、少し詰まっているのが https://book.jank-lang.org/cpp-interop/native-libs.html でleinからdllを呼び出すところ。MSYS2では拡張子を `.so` ではなく `.dll` を使う点にも注意が必要だけれど、`jank` コマンドから例えば `native-lib` にある `libcompress.dll` を呼び出すには `jank -lcompress -I ./native-lib -L ./native-lib run src/native_lib_tutorial/main.jank` などとすればOKなのだけれど、Getting Startedによると

```clojure
  :jank {:include-dirs ["native-lib"]
         :library-dirs ["native-lib"]
         :linked-libraries ["compress"]}
```

を project.clj にかけばいいよ、と書いてあるものの、最新のjankとミスマッチしているのか、惜しい感じで今ひとつ動かない。

サンプル自体も、C++ JITのコードを何も前置き無しに呼び出しているので当然コンパイルエラーが出ていて、最新のjankでは `cpp/` を関数名の前につけるようになっているなど、ドキュメントが追いついてない点も少しあったりする。

たぶん、`lein new org.jank-lang/jank native-lib-tutorial` で呼び出されている org.jank-lang/jank という名前のテンプレートを、独自にフォークした最新に追従するものに更新する必要があるんだろうな。
## 【追記】原因判明、run-mainの呼び出し方がおかしいっぽい。

エラーの原因は、`run-main` および `compile` の引数の順番だった。元は、`jank run-main xxxx yyyy ....` と呼ばれているのだけれど、最新のjankなのか、MSYS2版のjankなのかはわからないが、run-main xxxx はすべてのOption引数の後（かつ `--` の前）になければ正しく動作しない。

これはテンプレートを変更することでも対応可能だが、以下のようなシェルスクリプトを書くことでも対応できる。

```bash
#!/bin/bash
 
# echo "orig cmd: jank $@"

jank="$HOME/dev/jank-win/compiler+runtime/build/jank"
# 引数を -- で分割
pre_ddash=()
post_ddash=()
found_ddash=0
for arg in "$@"; do
    if [[ $found_ddash -eq 0 && "$arg" == "--" ]]; then
        found_ddash=1
        continue
    fi
    if [[ $found_ddash -eq 0 ]]; then
        pre_ddash+=("$arg")
    else
        post_ddash+=("$arg")
    fi
done
 
# run-main, compile を探して削除
special_cmds=("run-main" "compile")
special_idx=-1
special_cmd=""
for idx in "${!pre_ddash[@]}"; do
    for cmd in "${special_cmds[@]}"; do
        if [[ "${pre_ddash[$idx]}" == "$cmd" ]]; then
            special_idx=$idx
            special_cmd="${pre_ddash[$idx]}"
            break 2
        fi
    done
done
if [[ $special_idx -ge 0 ]]; then
    # 配列から削除
    tmp=()
    for idx in "${!pre_ddash[@]}"; do
        if [[ $idx -ne $special_idx ]]; then
            tmp+=("${pre_ddash[$idx]}")
        fi
    done
    pre_ddash=("${tmp[@]}")
    # 最後のポジション引数（ハイフンなし）の直前を探す
    last_pos_idx=-1
    for idx in "${!pre_ddash[@]}"; do
        if [[ ! "${pre_ddash[$idx]}" == -* ]]; then
            last_pos_idx=$idx
        fi
    done
    # 直前にspecial_cmdを挿入
    if [[ $last_pos_idx -ge 0 ]]; then
        tmp=()
        for idx in "${!pre_ddash[@]}"; do
            if [[ $idx -eq $last_pos_idx ]]; then
                tmp+=("$special_cmd")
            fi
            tmp+=("${pre_ddash[$idx]}")
        done
        pre_ddash=("${tmp[@]}")
    else
        pre_ddash=("$special_cmd" "${pre_ddash[@]}")
    fi
fi
 
#echo "cmd: $jank ${pre_ddash[@]} -- ${post_ddash[@]}"  # デバッグ用（普段はコメントアウト）
 
"$jank" "${pre_ddash[@]}" -- "${post_ddash[@]}"
```

これを jank としてパスに登録しつつ、元のjankをパスから外せばシェル的にはOKなのだけれど、leinテンプレート中で使われているbabashkaのwhichが、シェルスクリプトをうまく判定してくれないので、苦肉の策で、以下のような `main.c` を作り、`gcc main.c -o jank.exe` として、さっきのシェルスクリプトと同じパスに置けば、解決する。

```c
#include <windows.h>
#include <stdio.h>

int main(int argc, char *argv[]) {
    // MSYS2 bash.exeのパス
    const char *bash_path = "C:\\msys64\\usr\\bin\\bash.exe";
    // jankシェルスクリプトのパス（スラッシュ区切り）
    const char *jank_sh = "C:/msys64/home/funatsu/bin/jank";

    // コマンドライン構築
    char cmd[4096] = {0};
    snprintf(cmd, sizeof(cmd), "\"%s\" \"%s\"", bash_path, jank_sh);
    for (int i = 1; i < argc; ++i) {
        strcat(cmd, " \"");
        strcat(cmd, argv[i]);
        strcat(cmd, "\"");
    }

    // 実行
    STARTUPINFOA si = {0};
    PROCESS_INFORMATION pi = {0};
    si.cb = sizeof(si);

    if (CreateProcessA(NULL, cmd, NULL, NULL, FALSE, 0, NULL, NULL, &si, &pi)) {
        WaitForSingleObject(pi.hProcess, INFINITE);
        CloseHandle(pi.hProcess);
        CloseHandle(pi.hThread);
        return 0;
    } else {
        fprintf(stderr, "Failed to launch: %s\n", cmd);
        return 1;
    }
}
```

あくまで元のleinテンプレートをいじらずに動かす苦肉の策なので、本当はテンプレート ([jank/lein-jank](https://github.com/jank-lang/jank/tree/main/lein-jank)) の下記の該当部から、`run-main` だけ (?) を書き換えたフォークを作って使うのが一番良いのかもしれない。

https://github.com/jank-lang/jank/blob/main/lein-jank/src/leiningen/jank.clj

どこかで暇を見てフォーク作ってClojarsに登録して