# CMake をビルドシステムとして選定する

## Context and Problem Statement

libmarkdown は C99 で実装するライブラリであり、Linux、Windows および macOS で configure、build、test を実行できるビルドシステムが必要である。ビルド設定とテスト実行方法を共通化し、開発環境と CI で同じ手順を利用できるようにする必要がある。

どのビルドシステムを libmarkdown の標準として採用するか決定する必要がある。

## Considered Options

* CMake
* Make
* Autotools
* Meson

## Decision Outcome

Chosen option: "CMake", because CMake の標準的な configure、build、test の流れを利用することで、C99 ライブラリのビルド手順を Linux、Windows および macOS に共通化できる。CI でも同じ CMake の手順を実行し、環境ごとの差異を各ジェネレータおよびツールチェーンに委ねる。

### Consequences

* Good, because Linux、Windows および macOS を対象とするビルド設定を共通の記述で管理できる。
* Good, because configure、build、test の手順をローカル環境と CI で統一できる。
* Good, because C99 の言語要件とライブラリのビルド設定を一つのビルド構成で管理できる。
* Bad, because 利用者の環境に CMake と、選択したジェネレータおよび C コンパイラが必要になる。
* Bad, because Makefile や IDE プロジェクトなどを直接管理する方式とは異なる設定と運用を学ぶ必要がある。