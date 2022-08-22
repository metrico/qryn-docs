# Supported Features

At the time of writing this doc, **qryn** supports the following features:

## LogQL API

| **Name**             | **Type**           | **Support**         | **APIs.**                         | **Docs**    
|----------------------|--------------------|---------------------|-----------------------------------|--------------
| Name.                | Type.              | **Yes**             | `/something`                      | [View Docs](/feature/name.md)


## Tempo API

| **Name**             | **Type**           | **Support**         | **APIs.**                         | **Docs**    
|----------------------|--------------------|---------------------|-----------------------------------|--------------
| Name.                | Type.              | **Yes**             | `/something`                      | [View Docs](/feature/name.md)


## Prometheus API

| **Name**             | **Type**           | **Support**         | **APIs.**                         | **Docs**    
|----------------------|--------------------|---------------------|-----------------------------------|--------------
| Remote Write         | POST               | **Yes**             | `/api/v1/prom/remote/write`       | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L388)
| Remote Write         | POST               | **Yes**             | `/api/prom/remote/write`          | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L388)
| Query Range          | GET                | **Yes**             | `/api/v1/query_range`             | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L392)
| Query                | GET                | **Yes**             | `/api/v1/query`                   | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L392)
| Labels                | GET/POST           | **Yes**             | `/api/v1/labels`                 | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L392)
| Label Values          | GET/POST           | **Yes**             | `/api/v1/label/:name/values`     | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L392)




## Elastic API

| **Name**             | **Type**           | **Support**         | **APIs.**                         | **Docs**    
|----------------------|--------------------|---------------------|-----------------------------------|--------------
| Index                | POST               | **Yes**             | `/:target/_doc`                   | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L318)
| Index                | POST               | **Yes**             | `/:target/_create/:id`            | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L318)
| Index                | PUT                | **Yes**             | `/:target/_doc/:id`               | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L318)
| Index                | PUT                | **Yes**             | `/:target/_create/:id`            | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L318)
| Bulk                 | POST               | **Yes**             | `/_bulk`                          | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L318)
| Bulk                 | POST               | **Yes**             | `/:target/_bulk`                  | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L318)

## Influx API

| **Name**             | **Type**           | **Support**         | **APIs.**                         | **Docs**    
|----------------------|--------------------|---------------------|-----------------------------------|--------------
| Write                | POST              | **Yes**             | `/influx/api/v2/write`             | [View Docs](https://github.com/metrico/qryn/blob/master/qryn.js#L388)

