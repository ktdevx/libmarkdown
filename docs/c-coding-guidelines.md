# C コーディング規約

## 1. 適用範囲

本規約は、libmarkdown の C ソースファイルおよび公開ヘッダーファイルに適用する。

## 2. 言語標準

### 2.1 C99

実装および公開 API は、最低限 C99 に準拠する。

### 2.2 C11 以降の機能

C11 以降で追加された言語機能または標準ライブラリ機能を前提にしてはならない。

## 3. 識別子の命名

### 3.1 公開識別子の接頭辞

公開する型、関数および列挙型には `md_` 接頭辞を付ける。例: `md_status_t`、`md_parse`

公開する列挙値、マクロおよび関連する定数には `MD_` 接頭辞を付ける。例: `MD_OK`、`MD_OFFSET_NONE`

この接頭辞規則は、将来追加する公開識別子にも適用する。

### 3.2 `typedef` 型のサフィックス

`typedef` で定義する型名には `_t` サフィックスを付ける。例: `md_status_t`

### 3.3 識別子の形式

列挙値および `#define` の名前は、大文字のスネークケースで命名する。例: `MD_INVALID_ARGUMENT`

変数名、`enum` タグ名、`struct` タグ名および `struct` メンバ名は、小文字のスネークケースで命名する。例: `child_count`、`md_node_type`、`md_node`、`first_child`

関数名は、小文字のスネークケースで命名する。関数名には処理の役割を示す動詞を含め、値の取得には `get`、値の設定には `set`、真偽値の問い合わせには `is` など、役割に応じた動詞を使用する。

例: `release_buffer`、`md_node_get_literal`、`md_heading_set_level`、`md_node_is_valid`

## 4. 構文と書式

### 4.1 波括弧

`if` 文では、実行文が一つだけの場合も含め、必ず波括弧を使用する。

```c
if (node == NULL)
{
    return MD_INVALID_ARGUMENT;
}
```

### 4.2 波括弧の位置

制御構文、関数定義、`struct` 定義、`union` 定義および `enum` 定義では、開始波括弧 `{` を制御構文または宣言の次の行に置く。

```c
while (node != NULL)
{
    node = node->next;
}
```

```c
struct md_node
{
};
```

```c
static void release_buffer(void *buffer)
{
    free(buffer);
}
```

`typedef` 宣言では、閉じ波括弧 `}` と typedef の型名を同じ行に置いてよい。

```c
typedef struct md_node
{
} md_node_t;
```

### 4.3 `goto` の使用

`goto` はエラー処理時のリソース解放など、クリーンアップ処理を一箇所に集約する目的に限り、`goto` を使用してよい。

```c
goto cleanup;

cleanup:
    free(buffer);
```

通常の制御フローを実現する目的では、`goto` を使用しない。

### 4.4 インデント

C ソースコードのインデントには、1段階あたり4個のスペースを使用する。

### 4.5 行の長さ

1行の長さは、原則として80カラム以内とする。80カラムを超える場合は、可能な限り適切な位置で改行する。

やむを得ない場合は100カラムを上限の目安とする。

## 5. ヘッダーファイルの構成

ヘッダーファイルには、インクルードガードを設ける。インクルードガードの名前は、対象となるヘッダーファイルを一意に識別できる大文字のスネークケースにする。

```c
#ifndef HEADER_H
#define HEADER_H

/* header contents */

#endif
```

## 6. エラー処理

### 6.1 戻り値によるエラー通知

失敗する可能性がある関数は、戻り値でエラー情報を返す。例: `md_status_t md_node_destroy(...)`
