## qryn:cloud

### Configuration

**qryn** has no configuration files by design - just `ENV` variables

?> K8s and Docker container friendly 😉

#### Parameters

The following `ENV` parameters can be used to configure and control **qryn:cloud**

<!-- tabs:start -->

#### ** qryn-go **
<a id=api name=api></a>

| ENV   	                |Default   	|Usage   	|
|------------------------|---	    |---		|
| QRYN_DATABASE_DATA_0_USER | default | Clickhouse Username |
| QRYN_DATABASE_DATA_0_NODE | clickhouse | Clickhouse Node Name |
| QRYN_DATABASE_DATA_0_PASS | qryn | Clickhouse Password |
| QRYN_DATABASE_DATA_0_NAME | qryn | Clickhouse Database Name |
| QRYN_DATABASE_DATA_0_HOST | "http://localhost" | Clickhouse Server address |
| QRYN_DATABASE_DATA_0_PORT | 9000 | Clickhouse Binary Port |
| QRYN_DATABASE_DATA_0_HTTP_PORT | 8123 | Clickhouse HTTP Port |
| QRYN_DATABASE_DATA_0_TABLE_PREFIX | "" | Prefix for tables (optional) |
| QRYN_DATABASE_DATA_0_READ_TIMEOUT | 30 | Read Timeout to Database (ms) |
| QRYN_DATABASE_DATA_0_WRITE_TIMEOUT | 30 | Write Timeout to Database (ms) |
| QRYN_DATABASE_DATA_0_STRATEGY | "failover" | Strategy for writing to multiple databases |
| QRYN_DATABASE_DATA_0_PRIMARY | true | Set to true to indicate this is primary database node |
| QRYN_DATABASE_DATA_0_DEBUG | false | Set to true to see debug messages about database in log |
| QRYN_DATABASE_DATA_0_SECURE | false | Set to true when Clickhouse is hosted on HTTPS |
| QRYN_DATABASE_DATA_0_HTTPS | false | Set to true, when Clickhouse is hosted on HTTPS |
| QRYN_HTTP_SETTINGS_HOST | "0.0.0.0" | Listen Host Setting for Read API|
| QRYN_HTTP_SETTINGS_PORT | 3200 | Listen Port Setting for Read API |
| QRYN_HTTP_SETTINGS_PREFORK | false | Set to true to start reader in multi-process mode, does not support all functionality |
| QRYN_HTTP_SETTINGS_ROOT | "/" | Path to root directory |
| QRYN_HTTP_SETTINGS_GZIP | true | Set to true if gzip compression should be used for transport |
| QRYN_HTTP_SETTINGS_GZIP_STATIC | true | Set to true if static content should be compressed |
| QRYN_HTTP_SETTINGS_API_PREFIX | "" | Prefix for API e.g. /PREFIX/api/v1/labels |
| QRYN_HTTP_SETTINGS_DEBUG | false | Set to true to see debug messages about database in log |
| QRYN_HTTP_SETTINGS_WEBSOCKET_ENABLE | false | Set to true for websocket reader interface |
| QRYN_READER_WRITER_PROXY | "http:localhost:3100" | Reader Proxy to Writer |
| QRYN_READER_VIEW_PATH | "/etc/qryn-view" | Path to QRYN-VIEW directory |
| QRYN_HTTP_CONNECT_CONNECT_TIMEOUT | 10 | Seconds before timeout |

-- WORK IN PROGRESS --

#### ** qryn-writer **
<a id=writer name=writer></a>

| ENV   	                |Default   	|Usage   	|
|------------------------|---	    |---		|
| CLICKHOUSE_SERVER      | localhost   	| Clickhouse Server address  		|

<!-- tabs:end -->

