---
tags:
  - fennel
  - lua
  - nrepl
aliases:
  - 260213_fennel_nrepl （と余談でtaskkkillとCtrl-Cの話）
---
[JeeJah](https://git.sr.ht/~technomancy/jeejah)という fennel (lua上のClojure方言) 用のnREPLがあるのだけれど、WindowsのVSCode nREPL環境でうまく動いていなかったので、フォークして動くようにした。

https://github.com/funatsufumiya/jeejah

自分はLuaJITで使っているのだけれど、READMEにあるように [luasocket](https://lunarmodules.github.io/luasocket/) のインストールが必要で、これがなかなか厄介だったりする。

## LuaJIT用のluasocketインストール

幸い、LuaJIT用にx86_64で事前コンパイルしたdllを配布しているものがあるので、ありがたく使わせてもらうことにする。

https://github.com/Corenale/luasocket_luajitx64_win_compiled

展開すると、mimeとsocketというフォルダがあるので、それをluajit.exeと同じフォルダにまず入れる。そして、それらを同じくコピーして、LUA_DIRのluaフォルダに入れる。

luajitのreplで `require('socket')` して何もエラーが出なければOK。エラーが出た場合は、探索候補になったパスが列挙されるので、何が足りないかをチェックして、パスに列挙されている bin (dll置き場) か lua (luaファイル置き場) に入れれば良い。

luasocketのテストができるスクリプトを以下に用意したので、以下のserverとclientを別窓のターミナルでそれぞれ起動して、通信ができればOK。

https://github.com/funatsufumiya/luasocket-test
## JeeJahが動かなかった理由

原因としてはいくつかあるようなのだけれど、一つは jeejah.fnl の register-session で返される session が単なるメッセージ送信用になっていて、そのあとで session として利用されている箇所（session-forで取得されている箇所）と齟齬が出ていたことと、あと `:op "describe"` の nREPLクライアントの命令に対して `ops` を返却するとき、配列が返却されていたのだけれど、実際には各命令がキーで説明がマップで登録されているものでなければいけなかった。幸い詳細は空配列で良いらしかったので、本当は `require` とか `optional` とかの説明が付与されるっぽいのだけれど、今回は割愛した。

## 今回 Fennel用 nREPL をいじった理由

これが本題なのだけど、nREPLの詳細を知りたかった。というのも、jank言語用のnREPLがまだ存在していないっぽくて、作ろうと思ったのだけれど、自分はnREPLが内部で何を通信しているのか知らなかったので、LLVMとかに近そうなLua実装を見れば何かわかるかなと思ったら動かなくて、結局、Python用のClojure方言であるhy用の[hy-nrepl](https://github.com/masatoi/hy-nrepl)のdebugモードに助けられたり、[python-nrepl-client](https://github.com/cemerick/nrepl-python-client) とかをテストに使ったりして、いろいろと遠回りにはなった。

でも結果としてnREPLはよくわかったし、Lua版も一応体裁上は動くようになったので、これで本格的にjank用のnREPLが作れるようになったかなという気がする。

## 余談: taskkill と Ctrl-C

最後に余談として、今回一番詰まったのは実は taskkill 関係だったりする。というのも、WindowsのCtrl-Cは、MacやLinuxのそれと同じようには動かず、バックグラウンドタスクが kill されないことが多い。そうすると不思議な挙動が起こり、TCPポート自体はListenできるのに、実際にメッセージを受信すると interrupt されて強制終了されるなどの妙な挙動を起こす。結果、別タスクが起動しっぱなしであると気づくのに半日かかったりした。

これはPython等を開発しているときも多いのだけれど、結局別窓から、 `taskkill /f /im luajit.exe` することになる。

なんで別窓からかというと、本窓で taskkill すると、そのあとにバックグラウンドタスクを終了させるかどうか Y/N で聞かれたときに、妙な挙動をしてしまって窓が使えなくなったりする。

本当はこの辺の完成度自体がWindows本体により改良されたらなと思うのだけれど、望み薄なので `taskkill /f /im luajit.exe` や `taskkill /f /im python.exe` 等は空で覚えとくのがWindowsシェルの嗜みなのかもしれない。