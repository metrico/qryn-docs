# 📦 Installation

Let's install **qryn** in a snap. All you need is `NodeJS` [14.x-16.x](https://github.com/nodesource/distributions) or `Docker` installed.

Stack configuration is 100% based on [ENV](env.md) parameters passed to the process or container.

<!-- tabs:start -->

#### ** NPM **
<a id=npm name=npm></a>

![image](https://user-images.githubusercontent.com/1423657/184507827-9087fc2d-457b-42cd-ae2e-1c894160745b.png ':size=100')

Install `qryn` as global package on your system using `npm`
```bash
sudo npm install -g qryn
```

Start `qryn` using [ENV](env.md) variables for its settings:

```bash
cd $(dirname $(readlink -f `which qryn`)) \
  && CLICKHOUSE_SERVER="my.clickhouse.server" \
  CLICKHOUSE_AUTH="default:password" \
  CLICKHOUSE_DB="qryn" \
  qryn
```

?> That's it! You are ready to access the stack using qryn-view or Grafana


#### ** PM2 **
<a id=pm2 name=pm2></a>

![image](https://user-images.githubusercontent.com/1423657/184507827-9087fc2d-457b-42cd-ae2e-1c894160745b.png ':size=100')

Install `qryn` and `pm2` as global packages on your system using `npm`

```bash
sudo npm install -g qryn pm2
```

Start `qryn` using [ENV](env.md) variables for its settings:
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
<a id=git name=git></a>

![image](https://user-images.githubusercontent.com/1423657/184507866-9e43ae95-6974-4f0b-bdf3-7a8d05f43d8d.png ':size=100')

Clone the qryn repository, install with `npm`and run using `nodejs` 14.x *(or higher)*
```bash
git clone https://github.com/metrico/qryn && cd qryn
npm install
```

Start `qryn` using [ENV](env.md) variables for its settings:

```bash
CLICKHOUSE_SERVER="my.clickhouse.server" \
CLICKHOUSE_AUTH="default:password" \
CLICKHOUSE_DB="qryn" \
node qryn.js
```

?> That's it! You are ready to access the stack using qryn-view or Grafana

#### ** Docker **
<a id=docker name=docker></a>

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
<a id=k8s name=k8s></a>

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
  name: qryn
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
        qryn.cmd: qryn.dev
      creationTimestamp: null
      labels:
        io.metrico.service: qryn
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
  name: qryn
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

#### ** Bun **
<a id=bun name=bun></a>

![image](https://user-images.githubusercontent.com/1423657/185789758-d7366d2f-7b59-4cda-8bf4-198214581dd6.png ':size=100')

Use **bun** to install and run **qryn** 

?> Install [bun](https://bun.sh/) before proceeding!

Install `qryn` as global package on your system using `bun`
```bash
bun install -g qryn
```

Start `qryn` using [ENV](env.md) variables for its settings:

```bash
CLICKHOUSE_SERVER="my.clickhouse.server" \
CLICKHOUSE_AUTH="default:password" \
CLICKHOUSE_DB="qryn" \
qryn
```

?> That's it! You are ready to use **qryn**

#### ** RPi4/aarch64 **
<a id=bun name=raspberry></a>

<img src="https://j6z7x9q7.rocketcdn.me/wp-content/uploads/2019/09/Heatsink-case-goud-1.jpg" width=150 />

Install a full **qryn** stack on **aarch64** platforms

### Requirements
- RPI 4 (aarch64)
- ARMv8 64-Bit OS (DietPI or Raspbian)

#### Install ClickHouse (aarch64)
Install ClickHouse and define the default user `password`
```
curl -O 'https://builds.clickhouse.com/master/aarch64/clickhouse' && chmod a+x ./clickhouse
./clickhouse install
```

Start the ClickHouse service:
```
sudo clickhouse start
```

#### Install NodeJS
```
curl -sSL https://deb.nodesource.com/setup_16.x | sudo bash -
sudo apt install -y nodejs
```

#### Install qryn
Install **qryn** using the chosen ClickHouse `password`
```
npm install -g qryn @pastash/pastash @pastash/output_loki
cd $(dirname $(readlink -f `which qryn`)) && \
      CLICKHOUSE_AUTH="default:password" \ 
      CLICKHOUSE_SERVER="localhost" \
      CLICKHOUSE_DB="qryn" \
      pm2 start qryn --name "qryn"
```

#### Install Grafana
```
wget https://dl.grafana.com/oss/release/grafana_8.3.3_arm64.deb
sudo dpkg -i grafana_8.3.3_arm64.deb
```

Add a datasource for qryn via UI or by using this [provisioning example]([https://github.com/metrico/qryn-oss-demo/blob/main/grafana/provisioning/datasources/datasource.yml](https://gist.githubusercontent.com/lmangani/a4be2275731783b37e0fd6f67439e5d5/raw/5a4d193976142c729197ae175e59bb927820a58f/datasource.yml))

![image](https://user-images.githubusercontent.com/1423657/186918652-d37c11de-a7a8-4d2c-a104-4a63c2da51e5.png)


```yaml
        apiVersion: 1
        datasources:
          - name: Loki
            type: loki
            access: proxy
            url: http://localhost:3100
```

#### TLDR;

The above procedure is available as a [shell script](https://gist.githubusercontent.com/lmangani/a4be2275731783b37e0fd6f67439e5d5/raw/5a4d193976142c729197ae175e59bb927820a58f/rpi4_cloki_install.sh)


?> That's it! You are ready to use **qryn**

<!-- tabs:end -->
