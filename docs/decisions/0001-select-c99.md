# C99 を実装言語標準として選定する

## Context and Problem Statement

libmarkdown は、Markdown と編集可能な AST を扱う C 言語ライブラリであり、公開 API と実装が依存する言語標準を固定する必要がある。言語標準を固定しない場合、利用可能な構文、標準ライブラリ、コンパイラおよび対象環境の前提が実装ごとに異なる可能性がある。

どの C 言語標準を libmarkdown の実装および公開 API の基準とするか決定する必要がある。

## Considered Options

* C99
* C11 以降
* C90
* C++

## Decision Outcome

Chosen option: "C99", because C99 を実装および公開 API の最低言語標準とすることで、C 言語ライブラリとしての API を維持しながら、C90 より新しい標準機能を利用できる。C11 以降の機能への依存は、C99 に対応する利用者およびビルド環境を不要に制限するため、必須としない。

### Consequences

* Good, because 公開 API を C99 から利用可能な C 言語インターフェースとして設計できる。
* Good, because CMake の configure、build、test において C99 を共通の言語要件として検証できる。
* Good, because C11 以降の機能に依存せず、対象となるコンパイラおよび環境の前提を明確にできる。
* Bad, because C11 以降で追加された言語機能や標準ライブラリ機能を必須の実装基盤として利用できない。
* Bad, because C++ 固有の型安全機能や標準ライブラリを実装および公開 API の前提にできない。