# md-node-attributes(3)

## NAME

`md_node_set_literal`, `md_heading_get_level`, `md_heading_set_level`, `md_list_get_attributes`, `md_list_set_attributes`, `md_list_set_tight`, `md_link_get_attributes`, `md_link_set_attributes`, `md_reference_definition_get_attributes`, `md_reference_definition_set_attributes` - ノード固有属性を取得または変更する

## SYNOPSIS

```c
md_status_t md_node_set_literal(md_node_t *node, const char *value, md_diag_t *diag);

md_status_t md_heading_get_level(const md_node_t *node, unsigned int *level, md_diag_t *diag);

md_status_t md_heading_set_level(md_node_t *node, unsigned int level, md_diag_t *diag);

md_status_t md_list_get_attributes(const md_node_t *node, md_list_kind_t *kind, unsigned long *start, md_list_delimiter_t *delimiter, int *tight, md_diag_t *diag);

md_status_t md_list_set_attributes(md_node_t *node, md_list_kind_t kind, unsigned long start, md_list_delimiter_t delimiter, md_diag_t *diag);

md_status_t md_list_set_tight(md_node_t *node, int tight, md_diag_t *diag);

md_status_t md_link_get_attributes(const md_node_t *node, const char **destination, const char **title, int *has_title, md_diag_t *diag);

md_status_t md_link_set_attributes(md_node_t *node, const char *destination, const char *title, int has_title, md_diag_t *diag);

md_status_t md_reference_definition_get_attributes(const md_node_t *node, const char **label, const char **destination, const char **title, int *has_title, md_diag_t *diag);

md_status_t md_reference_definition_set_attributes(md_node_t *node, const char *label, const char *destination, const char *title, int has_title, md_diag_t *diag);
```

## DESCRIPTION

`md_node_set_literal()` は literal を持つノードのテキストを設定する。heading の level は 1 から 6 とする。list の属性は kind、start、delimiter、tight である。bullet list は開始番号および区切り文字を持たず、ordered list の開始番号は 1 以上とする。

`md_list_set_tight()` は list の tight/loose 属性を変更する。複数のブロック子を持つ `list_item` を含む list を tight に設定する操作は `MD_INVALID_AST` で失敗する。子の追加が loose を必然とする場合、ライブラリは同じ操作で list を loose に更新する。子の削除では自動的に tight へ戻さない。

link および reference definition の title が未設定の場合、getter は対応する `has_title` を false とし、title ポインタを NULL とする。文字列属性へのポインタは所有権を移さず、ノードまたは祖先が破棄・切り離し・編集されるまでだけ有効である。

## PARAMETERS

必須の文字列引数は NULL 終端された有効な UTF-8 の非 NULL ポインタである。任意 title は `has_title` が false の場合にだけ NULL を許可する。getter の `node` および出力引数は非 NULL でなければならない。`diag` は NULL を許可する。

## RETURN VALUES

成功時は `MD_OK` を返す。ノード種別に適用できない getter または setter、範囲外の属性値、または AST 不変条件を破る変更は失敗し、AST と出力引数を変更しない。

## SEE ALSO

[md-node-traverse(3)](md-node-traverse.md)、[md-node-edit(3)](md-node-edit.md)
