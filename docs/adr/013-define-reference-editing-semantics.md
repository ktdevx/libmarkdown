# ADR-013: 解析済み参照リンクの属性を編集後も固定する

## ステータス

承認

## 背景

Markdown の参照リンクは、解析時に reference definition を参照して `link` または `image` として AST に変換される。

AST 編集後に reference definition の変更を既存の `link` や `image` に反映すると、あるノードの編集が別のノードの意味を暗黙的に変更することになる。

解析後の AST における参照リンクの扱いを、reference definition の編集から独立させるか、編集のたびに再解決するかを決定する必要がある。

## 決定

「解析時に解決した link と image の属性を編集後も固定する」を採用する。

解析時に決定した `link` と `image` の意味をその後の reference definition の変更から独立させることで、AST の編集操作による影響範囲を明確にし、既存ノードが別の編集操作によって暗黙的に変化することを防げるため。

## 検討案

### reference definition の編集ごとに既存の link と image を再解決する

reference definition の追加、削除、移動または属性変更に応じて、既存の `link` と `image` を再解決する方式。

- 利点: reference definition の変更を既存の参照リンクへ自動的に反映できる。
- 欠点: reference definition の編集によって、離れた位置にある既存ノードの意味が変化する。
- 欠点: 編集操作の影響範囲を予測しにくくなる。

### 次回シリアライズ時だけ参照を再解決する

AST 上では参照リンクの情報を保持し、シリアライズ時に最新の reference definition を参照してリンク先を決定する方式。

- 利点: reference definition の変更をシリアライズ結果に反映できる。
- 欠点: AST の状態とシリアライズ結果の意味が一致しない可能性がある。
- 欠点: シリアライズが AST の状態だけで決まらず、別の AST 要素にも依存する。

### 解析時に解決した link と image の属性を編集後も固定する

解析時に reference definition から解決した `link` と `image` の属性を AST に保持し、その後の reference definition の変更では既存ノードを変更しない方式。

- 利点: 既存ノードの意味が他の編集操作によって暗黙的に変化しない。
- 利点: AST の状態と各ノードの意味を独立して扱える。
- 欠点: reference definition の変更を既存の link や image に反映するには、利用者が対象ノードを明示的に編集する必要がある。
