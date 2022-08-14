# 📦 Installation

Let's install **qryn** in a snap. All you need is `NodeJS` [14.x-16.x](https://github.com/nodesource/distributions) or `Docker` installed.

Stack configuration is 100% based on [ENV](env.md) parameters passed to the process or container.

<!-- tabs:start -->

#### ** NPM **

![image](https://user-images.githubusercontent.com/1423657/184507827-9087fc2d-457b-42cd-ae2e-1c894160745b.png ':size=100')

Install `qryn` as global package on your system using `npm`
```bash
sudo npm install -g qryn
```

Start `qryn` using ENV variables for its settings:

```bash
cd $(dirname $(readlink -f `which qryn`)) \
  && CLICKHOUSE_SERVER="my.clickhouse.server" \
  CLICKHOUSE_AUTH="default:password" \
  CLICKHOUSE_DB="qryn" \
  qryn
```

?> That's it! You are ready to access the stack using qryn-view or Grafana


#### ** PM2 **

![image](https://user-images.githubusercontent.com/1423657/184507827-9087fc2d-457b-42cd-ae2e-1c894160745b.png ':size=100')

Install `qryn` and `pm2` as global packages on your system using `npm`

```bash
sudo npm install -g qryn pm2
```

Start `qryn` using ENV variables for its settings:
```bash
cd $(dirname $(readlink -f `which qryn`)) \
  && CLICKHOUSE_SERVER="my.clickhouse.server" \
  CLICKHOUSE_AUTH="default:password" \
  CLICKHOUSE_DB="qryn" \
  pm2 start qryn
```

Save your settings and install as a system service
```
pm2 save
pm2 startup
```

?> That's it! You are ready to access the stack using qryn-view or Grafana

#### ** GIT **

![image](https://user-images.githubusercontent.com/1423657/184507866-9e43ae95-6974-4f0b-bdf3-7a8d05f43d8d.png ':size=100')

Clone the qryn repository, install with `npm`and run using `nodejs` 14.x *(or higher)*
```bash
git clone https://github.com/metrico/qryn && cd qryn
npm install
```

Start `qryn` using ENV variables for its settings:

```bash
CLICKHOUSE_SERVER="my.clickhouse.server" \
CLICKHOUSE_AUTH="default:password" \
CLICKHOUSE_DB="qryn" \
node qryn.js
```

?> That's it! You are ready to access the stack using qryn-view or Grafana

#### ** Docker **

![image](https://user-images.githubusercontent.com/1423657/184507884-624b9598-62e1-413f-854e-8210ecac4e75.png ':size=300x100')

Each release is automatically pushed to [docker hub](https://hub.docker.com/r/qxip/qryn/tags) and [ghcr](ghcr.io/metrico/qryn:2.1.11)
```
qxip/qryn:latest
```
```
ghcr.io/metrico/qryn:latest
```

##### Compose
Use docker to get started in no time - use either a local or cloud ClickHouse instance.

```
  qryn:
    image: qxip/qryn:latest
    ports:
      - "3100:3100"
    environment:
      - CLICKHOUSE_SERVER=http://user:pass@clickhouse-server:8123
```

!> Refer to the [configuration](env.md) for a list of supported parameters

#### Sampling Demo

Follow this [guide](https://github.com/metrico/opentelemetry-trace-sampling-demo) for a demo including:

- [qryn](https://cloki.org) _(inserts and queries logs, metrics, telemetry)_
- [ClickHouse](https://clickhouse.com) _(data backend)_
- [Grafana](https://grafana.com/) _(preconfigured for visualizing data)_
- [Grafana Agent](https://grafana.com/docs/agent/latest/configuration/?src=li&mdm=social) _(processes trace data)_
- [Demo App](https://github.com/metrico/opentelemetry-trace-sampling-demo/tree/main/src) _(generate Zipkin traces and LogQL logs)_

![image](https://user-images.githubusercontent.com/1423657/183257423-59ac2648-0627-4edc-99b0-eea42abc3ca1.png)


?> That's it - demo logs included! Just access your stack using [qryn-view or Grafana](getting-started)

#### ** Kubernetes **

![image](https://user-images.githubusercontent.com/1423657/184507942-cb195a16-b6d1-451a-9d80-00550f261048.png ':size=100')

Each release is automatically pushed to [docker hub](https://hub.docker.com/r/qxip/qryn/tags) and [ghcr](ghcr.io/metrico/qryn:2.1.11)

```
qxip/qryn:latest
```
```
ghcr.io/metrico/qryn:latest
```

##### Helm
Use `Kubernetes` and `helm` to get started using either a local or cloud ClickHouse instance.

?> You need to have a Kubernetes cluster, and the kubectl command-line tool must be configured to communicate with your cluster. It is recommended to run this tutorial on a cluster with at least two nodes that are not acting as control plane hosts.

Follow this [guide](https://github.com/metrico/qryn-k8s) to get started.

##### Rapid Example
```bash
kubectl apply -f qryn-service.yaml,qryn-deployment.yaml
```

###### `qryn-deployment.yml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cloki
  labels:
    io.metrico.service: qryn
spec:
  replicas: 1
  selector:
    matchLabels:
      io.metrico.service: qryn
  strategy: {}
  template:
    metadata:
      annotations:
        cloki.cmd: cloki.org
      creationTimestamp: null
      labels:
        io.metrico.service: cloki
    spec:
      containers:
        - env:
            - name: CLICKHOUSE_AUTH
              value: "default:password"
            - name: CLICKHOUSE_PORT
              value: 8123
            - name: CLICKHOUSE_SERVER
              value: "clickhouse"
          image: qxip/qryn
          name: qryn
          ports:
            - containerPort: 3100
          resources: {}
      restartPolicy: Always
status: {}
```

###### `qryn-service.yml`
```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    io.metrico.service: qryn
  name: cloki
spec:
  ports:
    - name: "3100"
      port: 3100
      targetPort: 3100
  selector:
    io.metrico.service: qryn
status:
  loadBalancer: {}
```

?> That's it - demo logs included! Just access your stack using qryn-view or Grafana

<!-- tabs:end -->
