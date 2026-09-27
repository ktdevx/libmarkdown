# リストの tightness を明示属性として編集する

## Context and Problem Statement

CommonMark の tight/loose はリストの意味属性であり、ラウンドトリップ比較の対象である。一方、入力の空行情報は AST に保持しないため、子ノード構造だけから常に tight/loose を復元することはできない。編集時の値の更新規則を定めないと、シリアライザと AST 検証が矛盾する。

## Considered Options

* 子ノード構造から tightness を常に再計算する
* tightness を利用者が設定する明示属性として保持する
* tightness を AST の比較対象から除外する

## Decision Outcome

Chosen option: "tightness を利用者が設定する明示属性として保持する", because パースした空行の意味を保持しつつ、編集後に利用者が出力形式を選べるため。

パーサーは CommonMark の規則に従って tightness を計算し、`list` 属性へ設定する。利用者は `md_list_set_tight()` で設定を変更できる。複数のブロック子を持つ `list_item` を含む list を tight に設定する操作は `MD_INVALID_AST` で失敗する。子の追加が tightness を loose にすることを必然とする場合、ライブラリは同じ編集操作の一部として list を loose に更新する。子の削除では自動的に tight へ戻さず、利用者が明示的に設定する。

### Consequences

* Good, because 入力時の空行が表す tight/loose を AST に保持できる。
* Good, because 編集後の出力形式を利用者が決定できる。
* Bad, because list 属性の更新と子構造の整合性を検証する必要がある。
