# Markdown シリアライズは意味的等価性を保持する

## Context and Problem Statement

libmarkdown は Markdown 文字列を AST に解析し、編集可能な AST を Markdown へシリアライズする。入力 Markdown には、空白、改行形式、記法選択など、文書の意味を変えない複数の表現が存在するため、シリアライズ結果の一致基準を定める必要がある。

シリアライズにおいて、入力 Markdown の字句的な表現を完全に保持するのか、再解析したときの文書構造および意味を保持するのかを決定する必要がある。

## Considered Options

* 入力 Markdown の字句的表現を完全に保持する
* AST の意味的等価性を保持する
* 意味を保持せず、任意の Markdown を生成する

## Decision Outcome

Chosen option: "AST の意味的等価性を保持する", because AST を編集してから再シリアライズするという本ライブラリの目的に適合し、入力時の記法に依存せず、Markdown と AST の相互変換を検証できるため。

シリアライザは、有効な AST を UTF-8 の Markdown 文字列へ変換する。シリアライズ結果を再解析した AST は、入力 AST と意味的に等価でなければならない。ソース位置、入力時の記法、正規化された改行形式および記法上のみ必要な空白は保持対象としない。一方、コード内容、テキスト内容、インデントおよびハード改行など、意味に影響する内容は保持する。

### Consequences

* Good, because AST の編集結果を Markdown として再利用できる。
* Good, because Markdown の入力形式ではなく、再解析後の文書構造および意味を基準に検証できる。
* Good, because ソースの空白や記法を保持するためのソース保持機構を AST に追加する必要がない。
* Bad, because シリアライズ結果は入力 Markdown と字句的に一致しない場合がある。
* Bad, because 意味を保持する空白と、記法上のみ必要な空白をシリアライザが区別して扱う必要がある。
