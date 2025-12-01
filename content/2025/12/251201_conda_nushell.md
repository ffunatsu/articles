Nu (nushell) でcondaのactivate / deactivateの初期設定がいつもわからなくなるのでメモ。

## TL;DR

```bash
$ cd $nu.default-config-dir

$ http get https://raw.githubusercontent.com/nushell/nu_scripts/refs/heads/main/modules/virtual_environments/conda.nu | save -f conda.nu
```

その後 `nvim $nu.config-path` 等で以下を追記

```nu
source ($nu.default-config-dir | path join 'conda.nu')
activate base # 自動でbaseをactivateしたい場合のみ。
```

## Starshipも併せて設定（オプション）

オプションとして、[starship](https://starship.rs/ja-JP/) も設定しておくと、gitの状態などが詳細にわかって便利。

```bash
# windowsの場合
$ winget install starship

# macの場合は
$ brew install starship
```

その後 `nvim $nu.config-path` 等で以下を追記

```nu
mkdir ~/.cache/starship
starship init nu | save -f ~/.cache/starship/init.nu
```

なお、プリセット等を設定しないと何も出ないので、例えば以下のnerd-font-symbolsを初期設定するなら、初回だけ以下を実行。

https://starship.rs/ja-JP/presets/nerd-font

```bash
$ starship preset nerd-font-symbols -o ~/.config/starship.toml
```

## 以下、TL;DRに至った経緯をメモ

いつも確認するのは次のIssue（nushellのconda統合ってどうするのっていう主旨）。

https://github.com/nushell/nushell/issues/2272

いくつか例が上がっているなかで、nu_scripts が紹介されている。

https://github.com/nushell/nu_scripts

そのなかに conda.nu というものがある。

https://github.com/nushell/nu_scripts/blob/main/modules/virtual_environments/conda.nu

あとは、これを読み込む設定をすれば完了。