## qryn:cloud

### Configuration

**qryn** has no configuration files by design - just `ENV` variables

?> K8s and Docker container friendly 😉

#### Parameters

The following `ENV` parameters can be used to configure and control **qryn:cloud**

!> These are licensed components. Please [contact us](mailto:info@qxip.net) to obtain your keys.

?> **qryn:cloud** provides healthy defaults, we recommend to focus on the DATABASE_DATA part to start with and let the defaults do the job for your deployment

<!-- tabs:start -->

#### ** qryn-go **
<a id=api name=api></a>

| ENV   	                |Default   	|Usage   	|
|------------------------|---	    |---		|
| QRYNCLOUD_LICENSE | None | License Key provided by QXIP Team |
| ---- | --- | ---- |
| QRYN_DATABASE_DATA_0_USER | default | Clickhouse Username |
| QRYN_DATABASE_DATA_0_PASS | qryn | Clickhouse Password |
| QRYN_DATABASE_DATA_0_NAME | qryn | Clickhouse Database Name |
| QRYN_DATABASE_DATA_0_HOST | "http://localhost" | Clickhouse Server address |
| QRYN_DATABASE_DATA_0_PORT | 9000 | Clickhouse Binary Port |
| QRYN_DATABASE_DATA_0_HTTP_PORT | 8123 | Clickhouse HTTP Port |
| QRYN_DATABASE_DATA_0_TABLE_PREFIX | "" | Prefix for tables (optional) |
| QRYN_DATABASE_DATA_0_STRATEGY | "failover" | Strategy for writing to multiple databases |
| QRYN_DATABASE_DATA_0_PRIMARY | true | Set to true to indicate this is primary database node |
| QRYN_DATABASE_DATA_0_DEBUG | false | Set to true to see debug messages about database in log |
| QRYN_DATABASE_DATA_0_SECURE | false | Set to true when Clickhouse is hosted on HTTPS (optional) |
| QRYN_DATABASE_DATA_0_HTTPS | false | Set to true, when Clickhouse is hosted on HTTPS (optional) |
| ---- | --- | ---- |
| QRYN_HTTP_SETTINGS_HOST | "0.0.0.0" | Listen Host Setting for Read API|
| QRYN_HTTP_SETTINGS_PORT | 3200 | Listen Port Setting for Read API |
| QRYN_HTTP_SETTINGS_PREFORK | false | Set to true to start reader in multi-process mode, does not support all functionality |
| QRYN_HTTP_SETTINGS_DEBUG | false | Set to true to see debug messages about database in log |
| ---- | --- | ---- |
| QRYN_HTTP_CONNECT_CONNECT_TIMEOUT | 10 | Seconds before timeout |
| ---- | --- | ---- |
| QRYN_LOG_SETTINGS_LEVEL | "trace" | Set log level "debug", "info", "trace" |
| QRYN_LOG_SETTINGS_STDOUT | false | Set to true to emit logs to stdout |
| QRYN_LOG_SETTINGS_SYSLOG | true | Set to true to emit logs to syslog |
| QRYN_LOG_SETTINGS_MAX_AGE_DAYS | 7 | Retention in Days for log data |
| QRYN_LOG_SETTINGS_ROTATION_HOURS | 24 | Rotation Interval in Hours for log data |

#### ** qryn-writer **
<a id=writer name=writer></a>

| ENV   	                |Default   	|Usage   	|
|------------------------|---	    |---		|
| QRYNCLOUD_LICENSE | None | License Key provided by QXIP Team |
| ---- | --- | ---- |
| QRYN_DATABASE_DATA_0_USER | default | Clickhouse Username |
| QRYN_DATABASE_DATA_0_NODE | clickhouse | Clickhouse Node Name |
| QRYN_DATABASE_DATA_0_PASS | qryn | Clickhouse Password |
| QRYN_DATABASE_DATA_0_NAME | qryn | Clickhouse Database Name |
| QRYN_DATABASE_DATA_0_HOST | "http://localhost" | Clickhouse Server address |
| QRYN_DATABASE_DATA_0_PORT | 9000 | Clickhouse Binary Port |
| QRYN_DATABASE_DATA_0_HTTP_PORT | 8123 | Clickhouse HTTP Port |
| QRYN_DATABASE_DATA_0_TABLE_PREFIX | "" | Prefix for tables (optional) |
| QRYN_DATABASE_DATA_0_STRATEGY | "failover" | Strategy for writing to multiple databases (optional) |
| QRYN_DATABASE_DATA_0_PRIMARY | true | Set to true to indicate this is primary database node (optional) |
| QRYN_DATABASE_DATA_0_DEBUG | false | Set to true to see debug messages about database in log (optional) |
| QRYN_DATABASE_DATA_0_SECURE | false | Set to true when Clickhouse is hosted on HTTPS (optional) |
| QRYN_DATABASE_DATA_0_HTTPS | false | Set to true, when Clickhouse is hosted on HTTPS (optional) |
| ---- | --- | ---- |
| QRYN_HTTP_SETTINGS_HOST | "0.0.0.0" | Listen Host Setting for Write API|
| QRYN_HTTP_SETTINGS_PORT | 3100 | Listen Port Setting for Write API |
| QRYN_HTTP_SETTINGS_PREFORK | false | Set to true to start reader in multi-process mode, does not support all functionality |
| QRYN_HTTP_SETTINGS_API_PREFIX | "loki" | Prefix for API e.g. /PREFIX/api/v1/labels |
| QRYN_HTTP_SETTINGS_DEBUG | false | Set to true to see debug messages about database in log |
| ---- | --- | ---- |
| QRYN_HTTPS_SETTINGS_ENABLE | false | Set to true to activate HTTPS protocol for writer |
| QRYN_HTTPS_SETTINGS_HOST | "0.0.0.0" | Listen Host for HTTPS for Write API|
| QRYN_HTTPS_SETTINGS_PORT | 3444 | Listen Port for HTTPS for Write API |
| QRYN_HTTPS_SETTINGS_MIN-TLS-VERSION | "TLS1.2" | Set to min TLS version to accept |
| QRYN_HTTPS_SETTINGS_MAX-TLS-VERSION | "TLS1.4" | Set to max TLS version to accept |
| QRYN_HTTPS_SETTINGS_HTTP_REDIRECT | "loki" | Prefix for API e.g. /PREFIX/api/v1/labels |
| QRYN_HTTPS_SETTINGS_CERT | false | Set to true to see debug messages about database in log |
| QRYN_HTTPS_SETTINGS_KEY | false | Set to true for websocket reader interface |
| ---- | --- | ---- |
| QRYN_HTTP_CONNECT_CONNECT_TIMEOUT | 10 | Seconds before timeout |
| ---- | --- | ---- |
| QRYN_LOG_SETTINGS_LEVEL | "trace" | Set log level "debug", "info", "trace" |
| QRYN_LOG_SETTINGS_STDOUT | false | Set to true to emit logs to stdout |
| QRYN_LOG_SETTINGS_SYSLOG | true | Set to true to emit logs to syslog |
| QRYN_LOG_SETTINGS_MAX_AGE_DAYS | 7 | Retention in Days for log data |
| QRYN_LOG_SETTINGS_ROTATION_HOURS | 24 | Rotation Interval in Hours for log data |

<!-- tabs:end -->

