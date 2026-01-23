
ずっとできなくて困っていたら、以下のIssueに解決策が上がっていた。

https://github.com/msys2/MSYS2-packages/issues/38

以下のコメントにある通り：

https://github.com/msys2/MSYS2-packages/issues/38#issuecomment-150131217

```zsh
# complete hard drives in msys2
drives=$(mount | sed -rn 's#^[A-Z]: on /([a-z]).*#\1#p' | tr '\n' ' ')
zstyle ':completion:*' fake-files /: "/:$drives"
unset drives
```

これを .zshrc 等に記載すればOK。これで補完が効くようになる。理由はわからないけど動いたのでいいや。たぶんWindows固有のCドライブとかのドライブ名を解決しているんだろうとは思う。