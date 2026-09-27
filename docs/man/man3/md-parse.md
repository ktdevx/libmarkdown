# md-parse(3)

## NAME

`md_parse` - Markdown 文字列を document AST へ解析する

## SYNOPSIS

```c
md_status_t md_parse(const char *str, md_node_t **node, md_diag_t *diag);
```

## PARAMETERS

`str` は NULL 終端された有効な UTF-8 Markdown 文字列である。`node` は成功時の document root を受け取る非 NULL の出力ポインタである。`diag` は NULL を許可する。

## RETURN VALUES

成功時は `MD_OK` を返し、`node` に `MD_NODE_DOCUMENT` root の所有権を渡す。失敗時は document root を返さず、出力引数を変更しない。

## OWNERSHIP

生成された document root と接続済みの木は `md_node_destroy()` で破棄する。解析では操作時点のグローバルアロケータを使用する。

## SEE ALSO

[md-node-lifecycle(3)](md-node-lifecycle.md)、[md-allocator(3)](md-allocator.md)
