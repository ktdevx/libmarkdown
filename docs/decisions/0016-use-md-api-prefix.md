# md 接頭辞を公開 API 名前空間として使用する

## Context and Problem Statement

libmarkdown の公開 C API には、型、関数、列挙値およびマクロの名前空間を識別する接頭辞が必要である。

公開 API の名前空間衝突を避け、将来の型、関数、列挙値およびマクロにも一貫して適用できる接頭辞を決定する必要がある。

## Considered Options

* `md_` / `MD_` を使用する
* 接頭辞を使用しない

## Decision Outcome

Chosen option: "`md_` / `MD_` を使用する", because libmarkdown の公開 API に専用の名前空間を与え、他の C API との識別子衝突を避けられるため。

公開する型、関数および列挙型は `md_` 接頭辞を使用する。公開する列挙値、マクロおよび関連する定数は `MD_` 接頭辞を使用する。

この接頭辞規則は、現在の API だけでなく、将来追加する公開識別子にも適用する。`libmarkdown` というプロジェクト名、CommonMark のノード種別名および一般的な Markdown 用語は変更しない。

### Consequences

* Good, because 公開 API の識別子を他の C ライブラリと区別できる。
* Good, because 型、関数、列挙値およびマクロへ同じ規則を適用できる。
* Good, because 実装前に命名を確定するため、互換層や二重の公開 API を持たずに済む。
