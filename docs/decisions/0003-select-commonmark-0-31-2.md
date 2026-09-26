# CommonMark Spec 0.31.2 を準拠対象として選定する

## Context and Problem Statement

libmarkdown は、Markdown と編集可能な AST を相互変換する C99 ライブラリであり、入力の解釈とシリアライズ結果の基準となる Markdown 仕様を固定する必要がある。仕様が曖昧なままだと、パーサー、AST、シリアライザおよび適合性テストの期待する動作が一致しない。

どの Markdown 仕様を本ライブラリの準拠対象とするか決定する必要がある。

## Considered Options

* [CommonMark Spec 0.31.2](https://spec.commonmark.org/0.31.2/)
* [GitHub Flavored Markdown (GFM)](https://github.github.com/gfm/)
* CommonMark と独自拡張の組み合わせ
* Pandoc Markdown などの別の Markdown 方言

## Decision Outcome

Chosen option: "CommonMark Spec 0.31.2", because CommonMark のコア仕様に対象を限定することで、パーサー、公開 AST、シリアライザの責務と受け入れ条件を一貫させる。

### Consequences

* Good, because パーサーとシリアライザが従う構文および意味の基準を固定できる。
* Good, because [CommonMark Spec 0.31.2](https://spec.commonmark.org/0.31.2/) と公式テストケースを使い、実装の適合性を検証できる。
* Good, because GFM や個別の独自拡張を初期実装から除外し、公開 API と AST の範囲を抑えられる。
* Bad, because GFM や Pandoc Markdown 固有の構文は、別途対応を決定するまで利用できない。
* Bad, because CommonMark 0.31.2 以外の仕様バージョンとの挙動差を、本ライブラリの準拠対象としては保証しない。
