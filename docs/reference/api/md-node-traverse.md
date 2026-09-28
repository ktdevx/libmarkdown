# md-node-traverse(3)

## NAME

`md_node_get_type`, `md_node_get_parent`, `md_node_get_first_child`, `md_node_get_next_sibling`, `md_node_get_literal` - AST を走査して属性を取得する

## SYNOPSIS

```c
md_status_t md_node_get_type(const md_node_t *node, md_node_type_t *type, md_diag_t *diag);

md_status_t md_node_get_parent(const md_node_t *node, const md_node_t **parent, md_diag_t *diag);

md_status_t md_node_get_first_child(const md_node_t *node, const md_node_t **first_child, md_diag_t *diag);

md_status_t md_node_get_next_sibling(const md_node_t *node, const md_node_t **next_sibling, md_diag_t *diag);

md_status_t md_node_get_literal(const md_node_t *node, const char **value, md_diag_t *diag);
```

## DESCRIPTION

これらのアクセサはノード種別、親、最初の子、次の兄弟、または literal 属性を返す。image の最初の子および兄弟は alt を構成する inline 子ノードの走査に使用する。存在しない親、最初の子または次の兄弟は、対応する出力ポインタに NULL を設定する。

## PARAMETERS

`node` と出力引数は非 NULL でなければならない。`diag` は NULL を許可する。

## RETURN VALUES

成功時は `MD_OK` を返して出力引数を設定する。入力ノードまたは出力引数が NULL の場合、または対象ノードが literal を持たない場合は、それぞれ `MD_INVALID_ARGUMENT` または `MD_INVALID_AST` を返し、出力引数と AST を変更しない。

## OWNERSHIP

返されるノード参照および文字列属性へのポインタは所有権を移さない。そのノードまたは祖先が破棄・切り離し・編集されるまでだけ有効である。文字列属性は NULL 終端された読み取り専用ポインタである。

## SEE ALSO

[md-node-edit(3)](md-node-edit.md)、[md-node-attributes(3)](md-node-attributes.md)、[md-types(3)](md-types.md)
