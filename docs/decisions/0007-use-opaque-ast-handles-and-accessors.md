# 不透明な AST ハンドルとアクセサ API を使用する

## Context and Problem Statement

libmarkdown は利用者が観察および編集できる公開 AST を提供する。一方、AST は親子関係、所有権、必須属性および UTF-8 に関する不変条件を満たし、失敗した編集操作は AST を変更してはならない。公開構造体を直接書き換え可能にすると、ライブラリがこれらの条件を維持できない。

利用者が AST を観察・編集する公開モデルと、AST の内部表現および不変条件の保護方法を決定する必要がある。

## Considered Options

* 読み取り専用の公開構造体と編集 API を使用する
* 不透明な AST ハンドルとアクセサ API を使用する
* 可変の公開構造体を使用する

## Decision Outcome

Chosen option: "不透明な AST ハンドルとアクセサ API を使用する", because 利用者がノード種別、属性および木構造を観察し、検証済みの操作で編集できる一方、内部表現の変更と不変条件の維持を両立できるため。

公開ヘッダは `md_node_t` および `md_markdown_t` を不完全型として宣言する。document root と構築用 fragment の双方を `md_node_t` で表し、利用者は、ノード種別、親、子、兄弟、テキストおよびノード固有属性をアクセサ API で取得する。`md_node_create(MD_NODE_DOCUMENT, ...)` で document root を生成し、その他のノードは document root を指定して生成する。生成、属性変更、子の追加、切り離しおよび破棄は、公開された検証付き API だけで行う。公開 API は可変なノード内部フィールドへのポインタを返さない。

### Consequences

* Good, because 全ての編集で不変条件の検査と失敗時の原子性を保証できる。
* Good, because ノードの内部表現を ABI に含めず、将来の実装変更の自由度を保てる。
* Good, because 所有権の移転を操作単位で明確に表現できる。
* Bad, because 利用者は単純なフィールド代入ではなく、アクセサおよび編集 API を呼ぶ必要がある。
* Bad, because 公開 API に観察・編集用の関数群を定義する必要がある。
