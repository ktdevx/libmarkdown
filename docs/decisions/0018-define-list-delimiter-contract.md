# list delimiter の kind 別契約を定義する

## Context and Problem Statement

公開 API は bullet と ordered の両方に `md_list_delimiter_t` を渡すが、CommonMark の bullet list に区切り文字はない。kind と整合しない値の扱いを定義しないと、属性 setter の成功条件と getter の結果が実装依存になる。

## Considered Options

* bullet list では delimiter を無視する
* kind に関係なく delimiter を保持する
* bullet list は `MD_LIST_DELIMITER_NONE` 固定、ordered list は period または paren とする

## Decision Outcome

Chosen option: "bullet list は `MD_LIST_DELIMITER_NONE` 固定、ordered list は period または paren とする", because CommonMark の意味モデルと AST の不変条件を API の戻り値で一貫して表現できるため。

`md_list_set_attributes()` は bullet list に `MD_LIST_DELIMITER_NONE` 以外が渡された場合、`MD_INVALID_AST` を返し、list と出力引数を変更しない。`md_list_get_attributes()` は bullet list の delimiter として常に `MD_LIST_DELIMITER_NONE` を返す。ordered list では `MD_LIST_DELIMITER_PERIOD` または `MD_LIST_DELIMITER_PAREN` を受け付ける。

### Consequences

* Good, because getter の結果が常に AST の不変条件と整合する。
* Good, because bullet list の不要な記法属性を利用者が誤って保持できない。
* Bad, because API 呼び出し側は kind に応じて delimiter を選ぶ必要がある。
