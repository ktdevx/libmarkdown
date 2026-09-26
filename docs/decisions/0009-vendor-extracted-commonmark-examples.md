# 抽出済みの CommonMark example をテストフィクスチャとして固定する

## Context and Problem Statement

libmarkdown は CommonMark Spec 0.31.2 の公式 example を使って適合性を検証する。テストをネットワーク取得に依存させると、公式サイトの可用性や取得内容の変更によってローカルおよび CI の再現性が失われる。仕様書全体をリポジトリに保持しない場合も、example の抽出規則と対象バージョンを追跡できる必要がある。

公式 example をテスト実行時に取得するか、仕様書全体を固定するか、抽出済みケースを固定するかを決定する必要がある。

## Considered Options

* テスト実行時に CommonMark 仕様書をネットワークから取得する
* CommonMark 仕様書全体をリポジトリに固定する
* 抽出済みの CommonMark example をテストフィクスチャとして固定する

## Decision Outcome

Chosen option: "抽出済みの CommonMark example をテストフィクスチャとして固定する", because ネットワークを必要とせず再現可能なテストを実現しながら、出荷ライブラリに不要な仕様書全文を含めずに済むため。

テスト用フィクスチャには、CommonMark Spec 0.31.2 から出現順に抽出した example 番号、入力 Markdown および期待 HTML を格納する。抽出時は example フェンス内の単独の `.` 行より前を入力、後を期待 HTML とし、`→` をタブへ復元する。フィクスチャの生成手順は、使用した仕様バージョンと抽出規則を追跡可能な形でテストコードまたは開発文書に記録する。

### Consequences

* Good, because ローカルと CI のテストがオフラインで同じ入力を使用できる。
* Good, because 仕様書の変更やネットワーク障害がテスト結果に影響しない。
* Good, because テスト入力と期待 HTML をレビュー対象として管理できる。
* Bad, because CommonMark 仕様更新時にフィクスチャを再生成し、差分をレビューする必要がある。
* Bad, because フィクスチャだけでは仕様書本文の文脈を参照できない。
