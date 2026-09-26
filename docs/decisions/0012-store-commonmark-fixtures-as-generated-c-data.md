# CommonMark フィクスチャを生成済み C データとして保持する

## Context and Problem Statement

CommonMark Spec 0.31.2 から抽出した example は、ネットワークに依存せずにテストで利用できる必要がある。テスト実行時に JSON や独自テキストを解析すると、出荷ライブラリとは別のパーサーや外部依存が必要になる。

抽出済み example をテストで読み込むファイル形式と、仕様書からの生成手順を決定する必要がある。

## Considered Options

* JSON または独自テキストのフィクスチャをテスト実行時に解析する
* 例ごとに Markdown と HTML の個別ファイルを保持する
* 生成済み C データとしてフィクスチャを保持する

## Decision Outcome

Chosen option: "生成済み C データとしてフィクスチャを保持する", because C のテスト実行ファイルが追加のランタイムパーサーや外部依存なしに、入力と期待 HTML のバイト列および長さを利用できるため。

`tools/extract_commonmark_examples.js` は、指定された CommonMark Spec 0.31.2 の仕様書から example を抽出し、`tests/fixtures/commonmark_0_31_2_examples.c` と対応ヘッダを生成する。

生成データは example 番号、Markdown バイト列と長さ、期待 HTML バイト列と長さを持つ。

生成ツールはテストの維持作業だけに使用し、CMake の configure、build および test の実行時に Node.js やネットワークを要求してはならない。

### Consequences

* Good, because テスト実行時にネットワーク、JSON パーサーおよび外部ツールを必要としない。
* Good, because バイト列の長さを明示でき、改行と末尾改行を正確に比較できる。
* Good, because 生成済みフィクスチャの差分をレビューできる。
* Bad, because フィクスチャの更新時に Node.js の生成ツールを実行する必要がある。
* Bad, because 生成 C ファイルが大きくなり、更新差分が読みづらくなる場合がある。
