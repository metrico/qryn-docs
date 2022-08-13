# Installation

Let's install **qryn** in a snap. All you need is `NodeJS` [14-16.x](https://github.com/nodesource/distributions) or 'Docker' installed.

<!-- tabs:start -->

#### ** NPM **
Install `qryn` as global package on your system using `npm`
```bash
sudo npm install -g qryn
```
```bash
cd $(dirname $(readlink -f `which qryn`)) \
  && CLICKHOUSE_SERVER="my.clickhouse.server" CLICKHOUSE_AUTH="default:password" CLICKHOUSE_DB="qryn" qryn
```

#### ** PM2 **
```bash
sudo npm install -g qryn pm2
```
```bash
cd $(dirname $(readlink -f `which qryn`)) \
  && CLICKHOUSE_SERVER="my.clickhouse.server" CLICKHOUSE_AUTH="default:password" CLICKHOUSE_DB="qryn" pm2 start qryn
pm2 save
pm2 startup
```

#### ** Docker **
Use docker to get started in no time - use either a local or cloud ClickHouse instance.

```bash
git clone https://github.com/metrico/qryn && cd qryn/docker
docker-compose up -d
```

#### ** GIT **
Clone the qryn repository, install with `npm`and run using `nodejs` 14.x *(or higher)*
```bash
git clone https://github.com/metrico/qryn && cd qryn
npm install
```
```bash
CLICKHOUSE_SERVER="my.clickhouse.server" CLICKHOUSE_AUTH="default:password" CLICKHOUSE_DB="qryn" node qryn.js
```


<!-- tabs:end -->
