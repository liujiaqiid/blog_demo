### 1. 资源查询
<hr/>
Host:  Api Server

Path: /api/v1/resources

Params:

| param | usage | isRequired |
| ------------------------------------- |
| status | 是否已被订购，0-已订购 | 非必需|
| owner  | 资源的发布者UUID | 非必需|
| pageSize | 分页大小 | 非必需，默认100 |
| pageNum | 当前页数 | 非必需，默认为0 |

Method: GET
