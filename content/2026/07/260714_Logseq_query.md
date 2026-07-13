---
tags:
  - logseq
aliases:
  - 260714 Logseqのresult-transformの個人的ベスト
---
優先度のついたものは先に表示しつつ、それ以外は日付順にソートして、直近30個を取得する result-transform。長らくどうすればベストか悩んでいたけれど、これで良さそう。

```clojure
:result-transform  (fn [result]
                    (let [rev-pri (fn [p] (case p "A" "Z" "B" "Y" "C" "X" "Z" "B" "A"))]
                     (->> result
                      (sort-by (fn [r]
                                [
                                 (str (rev-pri (get r :block/priority "Z"))
                                      "_"
                                      (get (get r :block/page) :block/journal-day "19000101"))
                                ]))
                      reverse
                      (take 30)
                      (map (fn [m] (update m :block/properties
                                    (fn [u]
                                      (assoc u
                                             :journal-day (get (get m :block/page) :block/journal-day 
```

使い方は例えばこんな感じで、以下は直近30のソート済みTODO（やらないもの以外）を取得するもの。

```clojure
#+BEGIN_QUERY
{:title "TODO (直近30)"
 :query (and (todo TODO) (not [[stale]]) (not [[やらない]]))
 :result-transform (fn [result]
                    (let [rev-pri (fn [p] (case p "A" "Z" "B" "Y" "C" "X" "Z" "B" "A"))]
                     (->> result
                      (sort-by (fn [r]
                                [
                                 (str (rev-pri (get r :block/priority "Z"))
                                      "_"
                                      (get (get r :block/page) :block/journal-day "19000101"))
                                ]))
                      reverse
                      (take 30)
                      (map (fn [m] (update m :block/properties
                                    (fn [u]
                                      (assoc u
                                             :journal-day (get (get m :block/page) :block/journal-day "19000101")))))))))
 :breadcrumb-show? false}
#+END_QUERY
```

日付順だけで並べるならぶっちゃけどうやってもOK。

ちなみに journal-day はデバッグ用で、テーブル表示にして設定（歯車）から表示させると、どんなデータがとれているかのデバッグができる。