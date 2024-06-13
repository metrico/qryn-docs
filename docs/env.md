# ⚙️ Configuration

**qryn** has no configuration files by design - just `ENV` variables

?> We're K8s and Docker container friendly 😉

### Resources

Each setup is different and requirements depend on the amount and volume of data and labels ingested. 

Make sure you have sufficient memory and disk resources allocated for your node service and clickhouse server when dealing with large amounts of data and fingerprints. We suggest 8GB RAM or higher for most setups with 100k-1M fingerprints. Observe your daily and weekly data consumption to forecast your disk usage requirements. Compression codecs and other optimizations can be performed at the ClickHouse level. When in doubt, ask the community for suggestions. 

#### Parameters

The following `ENV` parameters can be used to control **qryn** parameters and its backend settings

!> All parameters are _optional_, except for the **CLICKHOUSE_SERVER** related settings

| ENV   	                |Default   	|Usage   	|
|------------------------|---	    |---		|
| CLICKHOUSE_SERVER      | localhost   	| Clickhouse Server address  		|
| CLICKHOUSE_PORT  	     | 8123  	    | Clickhouse Server port  		|
| CLICKHOUSE_DB  	       | qryn  	    | Clickhouse Database Name  		|
| CLICKHOUSE_AUTH  	     | default:  	    | Clickhouse Authentication (user:password) |
| CLICKHOUSE_PROTO  	    | http  	    | Clickhouse Protocol (http, https) |
| CLICKHOUSE_TIMEFIELD   | record_datetime    | Clickhouse DateTime column for native queries |
| CLUSTER_NAME           | undefined | Clickhouse Cluster name |
| STORAGE_POLICY           | undefined | Clickhouse Cluster storage policy |
| BULK_MAXAGE  		        | 2000  	    | Max Age for Bulk Inserts  		|
| BULK_MAXSIZE  	        | 5000  	    | Max Size for Bulk Inserts  		|
| BULK_MAXCACHE  	       | 50000  	    | Max Labels in Memory Cache  		|
| LABELS_DAYS  		        | 7  	    	    | Max Days before Label rotation  		|
| SAMPLES_DAYS  	        | 7  	    	    | Max Days before Timeseries rotation  		|
| HOST 			               | 0.0.0.0 	    | HTTP API IP  		|
| PORT  		               | 3100 	            | HTTP API PORT  		|
| QRYN_LOGIN              | undefined             | Basic HTTP Username           |
| QRYN_PASSWORD         | undefined             | Basic HTTP Password           |
| READONLY  			          | false  	    | Readonly Mode, no DB Init  		|
| OMIT_CREATE_TABLES | false | Omit database provisioning on startup. Dangerous. |
| FASTIFY_BODYLIMIT      | 5242880   | API Maximum payload size in bytes |
| FASTIFY_REQUESTTIMEOUT | 0 | API Maximum Request Timeout in ms |
| FASTIFY_MAXREQUESTS    | 0 | API Maximum Requests per socket |
| FASTIFY_METRICS        | false | API /metrics exporter |
| ADVANCED_PROMETHEUS_MAX_SAMPLES | 5000000 | Max samples per a promql request |
| CORS_ALLOW_ORIGIN.     | * | CORS Allow Origin, default to any |
| TEMPO_SPAN             | 24 | Default span for Tempo queries in hours |
| TEMPO_TAGTRACE         | false | Optional tagging of TraceID (expensive) |
| DEBUG  			             | false  	    | Debug Mode (for backwards compatibility) 		|
| LOG_LEVEL  			         | info  	    | Log Level  		|
| HASH                   | xxhash64 | Hash function using for fingerprints. Currently supported `short-hash` and `xxhash64` (xxhash64 function)
| ALERTMAN_URL           | false      | Alertmanager API URL, ie: `http://my_alertmanager_url:1234` |
| ADVANCED_SAMPLES_ORDERING | timestamp_ns | Specify the 'ORDER BY' your samples table should use (for multiple use comma-separated list fingerprint,timestamp_ns) |
| BULK_MAX_SIZE_BYTE |  | max size of the bulk in bytes _(empty = infinite)_ |
| BULK_MAX_AGE_MS | 100  | max age of the bulk in milliseconds. Minimum value 100ms |
| MODE | all  | `all`, `writer`, `reader`, `init_only` _(init db and exit)_

#### Node Parameters
For memory intensive executions, fine tune the NodeJS settings using `NODE_OPTIONS  `

| ENV   	                |Default |Example  |
|------------------------|---	     |---		   |
| NODE_OPTIONS           |      	 | `-max-old-space-size=8192`  |
