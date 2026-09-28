# ADR-014: image の alt を inline 子ノードとして表現する

## ステータス

承認

## 背景

Markdown の image は destination、任意の title および alt 内容を持つ。

既存設計の image は destination と title だけを持つ葉ノードであるため、`![alt](destination)` を解析すると alt が失われ、シリアライズ後に `![](destination)` へ変化する。

これは公開 AST のテキスト内容を保持できず、Markdown と AST の相互変換における意味的等価性を損なう。

alt を単一の文字列属性として保持するか、Markdown の inline 構造を表す子ノードとして保持するかを決定する必要がある。

## 決定

「alt を inline 子ノード列として保持する」を採用する。

alt を AST の inline 構造として表現することで、alt 内の意味構造を保持でき、既存の AST の走査・編集・所有権モデルを利用できるため。

## 検討案

### alt を単一の文字列属性として保持する

image に alt 用の文字列属性を追加し、alt の内容を文字列として保持する方式。

- 利点: AST の構造を単純に保てる。
- 利点: alt の取得・設定を文字列 API で容易に行える。
- 欠点: alt 内の inline 構造を AST として保持できない。
- 欠点: Markdown の意味構造を失う可能性がある。

### alt を inline 子ノード列として保持する

image を inline 子ノードを持つコンテナとして扱い、alt の内容を AST の子ノードとして保持する方式。

- 利点: alt 内の inline 構造を AST として保持できる。
- 利点: 既存の AST 走査・編集モデルを利用できる。
- 利点: Markdown と AST の相互変換において alt の意味を保持できる。
- 欠点: image の AST 構造が複雑になる。
- 欠点: image の親子関係、解析およびシリアライズに追加の規則が必要になる。

### alt を保持せず、空 alt としてシリアライズする

alt の内容を AST に保持せず、image を常に空の alt として扱う方式。

- 利点: AST の構造を変更する必要がない。
- 利点: image を葉ノードとして扱い続けられる。
- 欠点: 入力された alt の内容が失われる。
- 欠点: Markdown と AST の相互変換において意味を保持できない。
