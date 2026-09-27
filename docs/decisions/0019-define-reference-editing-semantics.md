# 解析済み参照リンクの編集後挙動を固定する

## Context and Problem Statement

解析時の参照リンクは、参照ラベルを保持せず destination と title を持つ `link` または `image` へ変換される。解析後に reference definition を編集した場合の再解決規則を定義しないと、既存 AST の意味が編集操作の順序に依存する。

## Considered Options

* reference definition の編集ごとに既存の link と image を再解決する
* 次回シリアライズ時だけ参照を再解決する
* 解析時に解決した link と image の属性を編集後も固定する

## Decision Outcome

Chosen option: "解析時に解決した link と image の属性を編集後も固定する", because AST 編集を局所的かつ原子的にでき、reference definition の移動や削除で無関係なノードが暗黙に変化しないため。

reference definition の追加、削除、移動または属性変更は、既存の `link` と `image` の destination、title およびノード種別を変更しない。新たに解析される Markdown では、CommonMark の文書順に従って参照を解決する。シリアライザは既存の link と image を常にインライン形式で出力する。

### Consequences

* Good, because AST 編集操作の成功・失敗と変更範囲が明確になる。
* Good, because シリアライズ結果を再解析しても、解決済みリンクの意味を保持できる。
* Bad, because reference definition の変更で既存の参照リンクを更新したい利用者は、link/image 属性を明示的に変更する必要がある。
