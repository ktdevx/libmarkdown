# md-node-edit(3)

## NAME

`md_node_insert_before`, `md_node_detach` - AST の親子関係を編集する

## SYNOPSIS

```c
md_status_t md_node_insert_before(md_node_t *parent, md_node_t *child, const md_node_t *before, md_diag_t *diag);

md_status_t md_node_detach(md_node_t *node, md_node_t **out_node, md_diag_t *diag);
```

## DESCRIPTION

`md_node_insert_before()` は、親を持たない `child` を `parent` の直接の子として接続する。`before` が NULL の場合は最後の子として追加する。非 NULL の `before` は `parent` の直接の子でなければならない。成功時に `child` とその子孫の所有権は `parent` へ移る。親が document に接続済みの場合は child も接続済み部分木となり、親が未接続の場合は child も未接続部分木にとどまる。

`md_node_detach()` は接続済みの通常ノードを親から切り離し、`out_node` にその所有権を渡す。接続済みの `list`、`list_item`、`block_quote`、`paragraph`、`emphasis` または `strong` から最後の子を切り離す操作は失敗する。document root は切り離せない。

## RETURN VALUES

成功時は `MD_OK` を返す。接続に失敗した場合、`child` の未接続状態、親子関係および所有権は変更しない。切り離しに失敗した場合、子は親の所有のままとする。許可されない親子関係、最小子数または木構造の不変条件を破る操作は失敗する。

## DIAGNOSTICS

無効な親子関係、既に親を持つ子、循環または共有を作る操作は失敗し、`diag` が非 NULL なら対応する診断を設定する。

## SEE ALSO

[md-node-lifecycle(3)](md-node-lifecycle.md)、[md-node-traverse(3)](md-node-traverse.md)、[md-types(3)](md-types.md)
