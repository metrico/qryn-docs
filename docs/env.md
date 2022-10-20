## qryn
### Configuration

**qryn** has no configuration files by design - just `ENV` variables

?> K8s and Docker container friendly 😉

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
| FASTIFY_BODYLIMIT      | 5242880   | API Maximum payload size in bytes |
| FASTIFY_REQUESTTIMEOUT | 0 | API Maximum Request Timeout in ms |
| FASTIFY_MAXREQUESTS    | 0 | API Maximum Requests per socket |
| FASTIFY_METRICS        | false | API /metrics exporter |
| TEMPO_SPAN             | 24 | Default span for Tempo queries in hours |
| TEMPO_TAGTRACE         | false | Optional tagging of TraceID (expensive) |
| DEBUG  			             | false  	    | Debug Mode (for backwards compatibility) 		|
| LOG_LEVEL  			         | info  	    | Log Level  		|
| HASH                   | short-hash | Hash function using for fingerprints. Currently supported `short-hash` and `xxhash64` (xxhash64 function)


#### Node Parameters
For memory intensive executions, fine tune the NodeJS settings using `NODE_OPTIONS  `

| ENV   	                |Default |Example  |
|------------------------|---	     |---		   |
| NODE_OPTIONS           |      	 | `-max-old-space-size=8192`  |
