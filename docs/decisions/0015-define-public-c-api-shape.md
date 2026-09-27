# C API の文字列を NULL 終端文字列として扱う

## Context and Problem Statement

libmarkdown の C API は Markdown、AST 属性およびシリアライズ結果のテキストを扱う。通常の Markdown テキストに埋め込み NULL は必要なく、公開 API で文字列の受け渡しを複雑にする理由もない。C API の文字列をどのように扱うかを決定する必要がある。

## Considered Options

* NULL 終端文字列だけを使う
* ポインタと明示的な長さを組み合わせて使う
* ライブラリ独自の文字列オブジェクトを使う

## Decision Outcome

Chosen option: "NULL 終端文字列だけを使う", because 通常のテキスト API として単純で、利用者が文字列長を別に管理する必要がなく、C99 の標準的な文字列規約に従えるため。

公開 API の文字列入力は NULL 終端を必須とし、埋め込み NULL を許可しない。必須の文字列引数は空文字列でも非 NULL ポインタを渡す。`NULL` ポインタは、対応する API が任意属性を表す場合だけ許可する。出力文字列もNULL 終端し、文字列の長さは終端から求める。

AST ハンドル、診断、アロケータおよび名前空間の規則は、それぞれ対応する ADR と設計書で定める。

### Consequences

* Good, because 文字列の長さを別の引数で管理する必要がなく、API を単純にできる。
* Good, because C99 の標準的な NULL 終端文字列として利用できる。
* Bad, because 埋め込み NULL を含むデータは扱えない。
* Bad, because 文字列長を得るために終端まで走査する必要がある。
