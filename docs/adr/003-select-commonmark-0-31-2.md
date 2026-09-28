# ADR-003: CommonMark Spec 0.31.2 を準拠対象として選定する

## ステータス

承認

## 背景

libmarkdown は、Markdown と編集可能な AST を相互変換する C99 ライブラリであり、入力の解釈とシリアライズ結果の基準となる Markdown 仕様を固定する必要がある。仕様が曖昧なままだと、パーサー、AST、シリアライザおよび適合性テストの期待する動作が一致しない。

どの Markdown 仕様を本ライブラリの準拠対象とするか決定する必要がある。

## 決定

「CommonMark Spec 0.31.2」を採用する。

CommonMark のコア仕様に対象を限定することで、パーサー、公開 AST、シリアライザの責務と受け入れ条件を一貫させる。

## 検討案

### CommonMark Spec 0.31.2

[CommonMark Spec 0.31.2](https://spec.commonmark.org/0.31.2/) は、Markdown の構文と解析結果を明確に定義する仕様である。0.31.2 を本ライブラリが準拠する仕様バージョンとして固定し、仕様に定義された構文と意味を実装対象とする。

- 利点: パーサーとシリアライザが従う構文および意味の基準を固定できる。
- 利点: CommonMark Spec 0.31.2 と公式テストケースを使い、実装の適合性を検証できる。
- 利点: GFM や個別の独自拡張を初期実装から除外し、公開 API と AST の範囲を抑えられる。
- 欠点: GFM や Pandoc Markdown 固有の構文は、別途対応を決定するまで利用できない。
- 欠点: CommonMark 0.31.2 以外の仕様バージョンとの挙動差を、本ライブラリの準拠対象としては保証しない。

### GitHub Flavored Markdown (GFM)

[GitHub Flavored Markdown (GFM)](https://github.github.com/gfm/) は CommonMark を基盤とし、テーブル、タスクリスト、取り消し線、URL 自動リンクなどの拡張構文を追加した Markdown 方言である。

- 利点: CommonMark に加えて、GitHub で広く利用されている Markdown 拡張を扱える。
- 利点: GitHub 上の Markdown 文書との互換性を高められる。
- 欠点: CommonMark に加えて GFM 固有の構文と意味を実装する必要がある。
- 欠点: GFM 固有の構文を表現するため、AST および公開 API の範囲が広がる。
- 欠点: GitHub の Markdown 処理との互換性を考慮する必要がある。

### CommonMark と独自拡張の組み合わせ

CommonMark を基本仕様とし、libmarkdown 独自の構文や意味を追加する方式。独自拡張を標準仕様とは分離して定義し、必要に応じて利用できる構成とする。

- 利点: CommonMark の仕様準拠を維持しながら、ライブラリ固有の機能を追加できる。
- 利点: 将来的な用途に応じて独自の Markdown 拡張を追加できる。
- 欠点: 独自拡張の仕様、AST 表現およびシリアライズ規則を別途定義する必要がある。
- 欠点: 独自拡張を追加するほど、CommonMark だけを対象とする場合より実装およびテストの範囲が広がる。
- 中立: 独自拡張の具体的な内容は、個別の要件に応じて別途決定できる。

### Pandoc Markdown などの別の Markdown 方言

Pandoc Markdown など、CommonMark や GFM とは異なる Markdown 方言を準拠対象とする方式。対象とする方言の仕様に合わせてパーサー、AST およびシリアライザを設計する。

- 利点: 対象とする Markdown 方言固有の機能を利用できる。
- 利点: 特定の Markdown 処理系との互換性を重視したライブラリとして設計できる。
- 欠点: 対象とする方言ごとに構文や意味が異なるため、仕様の選定と実装範囲の定義が必要になる。
- 欠点: CommonMark を基準とする場合と比較して、仕様や実装の差異を管理する必要がある。
