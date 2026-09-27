# image の alt を inline 子ノードとして表現する

## Context and Problem Statement

Markdown の image は destination、任意の title および alt 内容を持つ。既存設計の image は destination と title だけを持つ葉ノードであるため、`![alt](destination)` を解析すると alt が失われ、シリアライズ後に `![](destination)` へ変化する。これは公開 AST のテキスト内容保持と、シリアライズ後の意味的ラウンドトリップに反する。

alt を単一文字列属性として追加するか、CommonMark の inline 構造を表す子ノードとして追加するかを決定する必要がある。

## Considered Options

* alt を単一の文字列属性として保持する
* alt を inline 子ノード列として保持する
* alt を保持せず、空 alt としてシリアライズする

## Decision Outcome

Chosen option: "alt を inline 子ノード列として保持する", because alt 内の text、emphasis、strong、code、hard break などの意味構造を失わず、既存の AST 走査・編集・所有権モデルを利用できるため。

`image` は inline コンテナとなり、destination と任意の title を属性として持つ。alt は image の子ノード列で表現し、空 alt は子ノード 0 個で表現する。image の子には `link` と `image` を許可しない。子ノードは通常の親子関係、所有権、循環なし、共有なしおよび編集失敗時非変更の規則に従う。

image の destination/title は `md_image_get_attributes()` と `md_image_set_attributes()` で操作する。alt は専用の文字列 API を設けず、通常の子ノード走査・編集 API で操作する。シリアライザは image を常に inline 形式で出力し、alt 子ノードを image label 文脈でエスケープする。

### Consequences

* Good, because `![alt](destination)` の alt 内容を AST と正規形に保持できる。
* Good, because alt 内の inline 構造を roundtrip できる。
* Good, because alt の編集が既存の子ノード API と所有権契約に統合される。
* Bad, because image が葉ノードではなくなり、親子許可表、validator、parser、serializer およびテストを更新する必要がある。
* Bad, because image label 専用のエスケープ規則と、HTML の alt 属性へ変換するテスト用規則が必要になる。
