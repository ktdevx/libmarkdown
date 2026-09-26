# 固定の CommonMark AST ノードモデルを使用する

## Context and Problem Statement

libmarkdown は CommonMark Spec 0.31.2 のブロック要素とインライン要素を、利用者が編集可能な AST として公開する必要がある。任意の属性マップだけを持つ汎用ノードでは、許可される親子関係、必須属性およびシリアライズ規則を一貫して検証できない。

CommonMark 要素を固定ノード種別とするか、属性マップを持つ汎用ノードとするかを決定する必要がある。

## Considered Options

* 固定の CommonMark ノード種別とノード固有属性を使用する
* 属性マップを持つ汎用ノードを使用する
* ブロックとインラインで別の AST 型を使用する

## Decision Outcome

Chosen option: "固定の CommonMark ノード種別とノード固有属性を使用する", because CommonMark の意味を直接表現し、生成・編集・シリアライズで必要な属性と親子関係を決定的に検証できるため。

公開 API は、document、ブロック引用、リスト、リスト項目、コードブロック、HTML ブロック、段落、見出し、主題区切り、参照定義、および CommonMark のインライン要素を表すノード種別を定義する。

リスト種別、開始番号、tight/loose、見出しレベル、リンク先、タイトル、コードブロックの info string などの意味属性は、ノード種別に応じた専用アクセサおよび設定 API で扱う。

### Consequences

* Good, because 親子関係と必須属性をノード種別ごとに検証できる。
* Good, because AST 正規形とラウンドトリップ比較に必要な意味属性を固定できる。
* Good, because 対応しないノード種別を生成・入力・シリアライズで明確に拒否できる。
* Bad, because 将来の拡張構文には新しいノード種別と API の追加が必要になる。
* Bad, because 汎用属性を自由に追加する用途には適さない。
