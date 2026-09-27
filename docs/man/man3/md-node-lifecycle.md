# md-node-lifecycle(3)

## NAME

`md_node_create`, `md_node_destroy` - AST ノードを生成および破棄する

## SYNOPSIS

```c
md_status_t md_node_create(md_node_type_t type, md_node_t **node, md_diag_t *diag);

md_status_t md_node_destroy(md_node_t *node, md_diag_t *diag);
```

## DESCRIPTION

`md_node_create(MD_NODE_DOCUMENT, ...)` は空の document root を生成する。その他のノード種別では document に未接続の fragment を生成する。document root は親を持たず、子として追加できず、切り離せない。fragment は許可された親子関係を満たす親への接続に成功するまで、利用者が所有する。

`md_node_destroy()` は切り離しノードとその子孫、または document root と接続済みの木を破棄する。接続済みの通常ノードは拒否する。document root の破棄は切り離し済み fragment を破棄しない。

## RETURN VALUES

`md_node_create()` は成功時に `MD_OK` を返し、`node` に生成物の所有権を渡す。`md_node_destroy()` は成功時に `MD_OK` を返す。失敗時の診断は `diag` に設定する。

## OWNERSHIP

親子関係の追加に成功した場合、親は子の所有権を持つ。親の破棄時には、その親が所有する子も破棄する。切り離しに成功した子の所有権は利用者に移る。

## SEE ALSO

[md-node-edit(3)](md-node-edit.md)、[md-parse(3)](md-parse.md)、[md-serialize(3)](md-serialize.md)、[md-allocator(3)](md-allocator.md)
