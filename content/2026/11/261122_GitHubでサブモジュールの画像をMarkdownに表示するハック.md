---
tags:
  - git
---
参考
https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#images

GitHubの執筆時点での仕様で、README等にはサブモジュールの画像を貼ることはできない。

一方、

`![](/../../../../<user>/<repo>/blob/main/path/to/image.png)`

の記法を使うとサブモジュール（厳密には外部リポジトリ）への画像を貼ることができる。

ただ、これではローカルでREADMEを見たときに画像が表示されない。そこで、

```markdown
![](/../../../../<user>/<repo>/blob/main/path/to/image.png)
![](submodule/image.png)
```

のように併記すると、他方は GitHub.com 上だけで動き、他方はローカルのみで動くので都合が良い。

Altを表示させたい場合は、以下のように単純に画像へのリンクを貼ると良い。（厳密には2つ貼らないといけないが、 `![]()` 画像記法の方にaltをつけると読むときにややこしい。 ）

```markdown
[https://github.com/<user>/<repo>/blob/main/oath/to/image.png](https://github.com/<user>/<repo>/blob/main/oath/to/image.png)

![](/../../../../<user>/<repo>/blob/main/path/to/image.png)
![](submodule/image.png)
```