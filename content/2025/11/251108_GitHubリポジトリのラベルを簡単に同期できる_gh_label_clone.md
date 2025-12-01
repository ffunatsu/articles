---
tags:
  - git
---


知らなかったけど、ghならありそうな気はしつつ調べてなくて、超簡単だった。

https://cli.github.com/manual/gh_label_clone

```bash
$ gh label clone owner/your_repo_name #--force

# または

$ gh label clone owner/from_repo_name --repo owner/to_repo_name #--force
```

`--force` があると上書きするらしい。

`gh label list --json "name,color,description"` 等を使えば、JSONとしてエクスポートすることも可能。