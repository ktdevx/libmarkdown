# MADR（Markdown Architectural Decision Records）を使用する

## Context and Problem Statement

本プロジェクトにおいて、アーキテクチャに関するもの（「アーキテクチャ意思決定記録」）、コード、その他の分野を問わず、下されたアーキテクチャ上の決定を記録したいと考えている。
これらの記録は、どのような形式や構造に従うべきか？

## Considered Options

* [MADR](https://adr.github.io/madr/) 4.0.0 – Markdown Architectural Decision Records
* [Michael Nygard's template](http://thinkrelevance.com/blog/2011/11/15/documenting-architecture-decisions) – 「ADR」という用語の最初の形
* [Sustainable Architectural Decisions](https://www.infoq.com/articles/sustainable-architectural-design-decisions) – Y-Statements
* <https://github.com/joelparkerhenderson/architecture_decision_record> に掲載されているその他のテンプレート
* 無形式 – ファイル形式や構造に関する規約なし

## Decision Outcome

Chosen option: "MADR 4.0.0", because

* 暗黙の前提を明示できる。
  後から意思決定を理解できるようにするため、設計に関する文書化は重要である。
  詳細については、[A rational design process: How and why to fake it](https://doi.org/10.1109/TSE.1986.6312940) も参照のこと。
* MADRは、あらゆる意思決定を構造化して記録できる。
* MADRの形式は簡潔であり、私たちの開発スタイルに適している。
* MADRの構成は理解しやすく、利用と保守を容易にする。
* MADRプロジェクトは活発である。
