# Installation

Let's install **qryn** in a snap. All you need is `NodeJS` [14-16.x](https://github.com/nodesource/distributions) or `Docker` installed.

Configuration is 100% based on [ENV](env.md) parameters passed to the process or container.

<!-- tabs:start -->

#### ** NPM **
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

That's it! You are ready to access the stack using qryn-view or Grafana


#### ** PM2 **
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

That's it! You are ready to access the stack using qryn-view or Grafana


#### ** Docker **
Use docker to get started in no time - use either a local or cloud ClickHouse instance.

```bash
git clone https://github.com/metrico/qryn && cd qryn/docker
docker-compose up -d
```

That's it - demo logs included! Just access your stack using qryn-view or Grafana

#### ** GIT **
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

That's it! You are ready to access the stack using qryn-view or Grafana


<!-- tabs:end -->
