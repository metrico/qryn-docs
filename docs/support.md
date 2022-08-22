# Supported API Features

At the time of writing this doc, **qryn** supports the following APIs are supported:

## LogQL API

🟢 INSERT 🟢 QUERY

| **Name**             | **Type**         | **Support**         | **APIs.**               | **Docs**    
|----------------------|------------------|---------------------|-------------------------|--------------
| Push                 | POST             | **Yes**             | `/loki/api/v1/push`        | [View Docs](https://github.com/metrico/qryn/wiki/HTTP-API#logql-http-api)
| Query                | GET              | **Yes**             | `/loki/api/v1/query`       | [View Docs](https://github.com/metrico/qryn/wiki/HTTP-API#logql-http-api)
| Query Range          | GET              | **Yes**             | `/loki/api/v1/query_range` | [View Docs](https://github.com/metrico/qryn/wiki/HTTP-API#logql-http-api)
| Labels               | GET              | **Yes**             | `/loki/api/v1/label`       | [View Docs](https://github.com/metrico/qryn/wiki/HTTP-API#logql-http-api)
| Label Values         | GET              | **Yes**             | `/loki/api/v1/label/name/values` | [View Docs](https://github.com/metrico/qryn/wiki/HTTP-API#logql-http-api)
| Tail                 | GET              | **Yes**             | `/loki/api/v1/tail`        | [View Docs](https://github.com/metrico/qryn/wiki/HTTP-API#logql-http-api)
| Ready                | GET              | **Yes**             | `/ready`                   | [View Docs](https://github.com/metrico/qryn/wiki/HTTP-API#logql-http-api)


## Tempo API

🟢 INSERT 🟢 QUERY

| **Name**             | **Type**           | **Support**         | **APIs.**                         | **Docs**    
|----------------------|--------------------|---------------------|-----------------------------------|--------------
| Push                 | POST               | **Yes**             | `/tempo/api/push`                                | [View Docs](/api/tempo.md)
| Query Spans          | GET                | **Yes**             | `/api/v2/spans`                                | [View Docs](/api/tempo.md)
| Query Traces         | GET                | **Yes**             | `/api/traces/{traceId}`                                | [View Docs](/api/tempo.md)
| Labels               | GET                | **Yes**             | `/api/search/tags`                                | [View Docs](/api/tempo.md)
| Label Values         | GET                | **Yes**             | `/api/search/tag/{tags}/values`                                | [View Docs](/api/tempo.md)


 
 /api/v2/spans
 /api/traces/{traceId}
 /api/search/tags
 /api/search/tag/{tags}/values


## Prometheus API

🟢 INSERT 🟡 QUERY


| **Name**             | **Type**           | **Support**         | **APIs.**                         | **Docs**    
|----------------------|--------------------|---------------------|-----------------------------------|--------------
| Remote Write         | POST               | **Yes**             | `/api/v1/prom/remote/write`       | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L388)
| Remote Write         | POST               | **Yes**             | `/api/prom/remote/write`          | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L388)
| Query Range          | GET                | **Yes**             | `/api/v1/query_range`             | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L392)
| Query                | GET                | **Yes**             | `/api/v1/query`                   | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L392)
| Labels                | GET/POST           | **Yes**             | `/api/v1/labels`                 | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L392)
| Label Values          | GET/POST           | **Yes**             | `/api/v1/label/:name/values`     | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L392)




## Elastic API

🟢 INSERT

| **Name**             | **Type**           | **Support**         | **APIs.**                         | **Docs**    
|----------------------|--------------------|---------------------|-----------------------------------|--------------
| Index                | POST               | **Yes**             | `/:target/_doc`                   | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L318)
| Index                | POST               | **Yes**             | `/:target/_create/:id`            | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L318)
| Index                | PUT                | **Yes**             | `/:target/_doc/:id`               | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L318)
| Index                | PUT                | **Yes**             | `/:target/_create/:id`            | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L318)
| Bulk                 | POST               | **Yes**             | `/_bulk`                          | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L318)
| Bulk                 | POST               | **Yes**             | `/:target/_bulk`                  | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L318)

## Influx API

🟢 INSERT

| **Name**             | **Type**           | **Support**         | **APIs.**                         | **Docs**    
|----------------------|--------------------|---------------------|-----------------------------------|--------------
| Write                | POST               | **Yes**             | `/influx/api/v2/write`            | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L388)

