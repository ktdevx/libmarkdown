# libmarkdown 公開 C API 設計書

## 1. 目的と位置付け

本書は libmarkdown の公開 C API に共通する方針と利用規則を定義し、詳細な API 契約を記載するリファレンスへの入口を提供する。

型、定数、関数宣言および個別操作の引数、戻り値、所有権、寿命、失敗時の契約は、[API リファレンス](#4-api-リファレンス) の各文書を正本とする。

## 2. 公開 API の方針

* 公開 API の型、関数および列挙型は `md_` 接頭辞を使用し、公開する列挙値、マクロおよび関連する定数は `MD_` 接頭辞を使用する。
* 失敗可能な操作は、戻り値としてステータスコードを返す。  
  必要に応じて、診断情報を引数を通じて受け取ることができる。

## 3. 共通規則

* 公開 API の必須文字列引数は NULL 終端された非 NULL ポインタで指定する。
* 空文字列は `""` で表し、文字列には埋め込み NULL を許可しない。
* 任意属性の文字列だけは、対応する存在フラグが false の場合に NULL を許可する。
* 出力文字列は NULL 終端する。
* ordered list の開始番号は `MD_ORDERED_LIST_START_MIN` から `MD_ORDERED_LIST_START_MAX` の範囲で指定する。個別の getter/setter における失敗時の状態は、対象 API リファレンスに従う。
* 解析入力、ノードのテキストおよび文字列属性、ならびにシリアライズ対象の AST は、有効な UTF-8 であることを呼び出し側が保証する。  
  ライブラリは UTF-8 の妥当性を検証、正規化または補正しない。この前提に違反した入力に対する解析結果、シリアライズ結果、診断および文字化けは保証しない。
* `diag` に非 NULL の `md_diag_t` を渡すと、ライブラリは各呼び出しの結果で内容を上書きする。  
  診断オブジェクトは呼び出し側が所有し、ライブラリは操作後に参照を保持しない。

## 4. API リファレンス

| 操作群 | 内容 |
| --- | --- |
| [md-types(3)](../reference/api/md-types.md) | 公開型、列挙値、定数、状態コードおよび診断。 |
| [md-allocator(3)](../reference/api/md-allocator.md) | グローバルアロケータの設定と適用範囲。 |
| [md-parse(3)](../reference/api/md-parse.md) | Markdown 文字列から document AST への解析。 |
| [md-serialize(3)](../reference/api/md-serialize.md) | document AST から Markdown 文字列へのシリアライズ。 |
| [md-node-lifecycle(3)](../reference/api/md-node-lifecycle.md) | AST ノードの生成と破棄。 |
| [md-node-edit(3)](../reference/api/md-node-edit.md) | 親子関係の接続と切り離し。 |
| [md-node-traverse(3)](../reference/api/md-node-traverse.md) | ノード種別、親子、兄弟および literal の取得。 |
| [md-node-attributes(3)](../reference/api/md-node-attributes.md) | literal、code block の info string、heading、list、link、image、参照定義の属性操作。 |

## 5. 文書の責務

本書は API 全体に適用する方針と共通規則の正本である。

各 API リファレンスは、対象シンボルの `SYNOPSIS`、引数、戻り値、診断、所有権および寿命の正本である。

AST の構造的不変条件、ノード種別の意味、解析規則およびシリアライズ形式は [設計書](design.md) の正本に従う。
