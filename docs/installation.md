# Installation

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

![image](https://user-images.githubusercontent.com/1423657/184507855-772cd575-2f07-451a-9ab3-9e965132ac23.png ':height=100')

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

![image](https://user-images.githubusercontent.com/1423657/184507884-624b9598-62e1-413f-854e-8210ecac4e75.png ':height=100')

Each release is automatically pushed to [docker hub](https://hub.docker.com/r/qxip/qryn/tags) and [ghcr](ghcr.io/metrico/qryn:2.1.11)
```
qxip/qryn:latest
```
```
ghcr.io/metrico/qryn:latest
```

##### Compose
Use docker to get started in no time - use either a local or cloud ClickHouse instance.

```bash
git clone https://github.com/metrico/qryn && cd qryn/docker
docker-compose up -d
```

##### Sampling Demo

Follow this [guide](https://github.com/metrico/opentelemetry-trace-sampling-demo) to spin up a fully configured system w/ samples

?> That's it - demo logs included! Just access your stack using qryn-view or Grafana

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
Use K8s and helm to get started in no time - use either a local or cloud ClickHouse instance.

Follow this [guide](https://github.com/metrico/qryn-k8s) to get started.

?> That's it - demo logs included! Just access your stack using qryn-view or Grafana

<!-- tabs:end -->
