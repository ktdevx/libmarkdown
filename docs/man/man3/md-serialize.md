# md-serialize(3)

## NAME

`md_serialize` - document AST を Markdown 文字列へシリアライズする

## SYNOPSIS

```c
md_status_t md_serialize(md_node_t *node, const char **str, md_diag_t *diag);
```

## PARAMETERS

`node` は document root でなければならない。`str` は成功時の NULL 終端 Markdown 文字列を受け取る非 NULL の出力ポインタである。`diag` は NULL を許可する。

## DESCRIPTION

`md_serialize()` は document 内部の出力バッファとは別の一時バッファへ Markdown 全体を構築する。AST 検証、シリアライズおよび終端処理が成功した場合だけ、一時バッファを document の出力バッファと交換し、`str` に読み取り専用ポインタを設定する。

## RETURN VALUES

成功時は `MD_OK` を返す。document root 以外は拒否する。AST が不変条件を満たさない場合は失敗し、AST を変更せず、部分的な出力を成功結果として返さない。確保または再確保に失敗した場合は document の AST と出力バッファを部分的な結果へ変更せず、失敗を返す。

## OWNERSHIP

`str` が指す文字列は document が所有する。利用者は解放してはならない。document の破棄時にバッファは解放され、破棄後に出力ポインタを参照してはならない。`md_serialize()` の次回呼び出し後は、成功または失敗にかかわらず、呼び出し前に取得した出力ポインタを参照してはならない。これは再確保によるアドレス変更の有無にかかわらず適用する。シリアライズ結果を複数世代にわたって保持することはできない。

## SEE ALSO

[md-node-lifecycle(3)](md-node-lifecycle.md)、[md-allocator(3)](md-allocator.md)
