# qryn:cloud 

!> qryn:cloud requires activation tokens. please [contact us](mailto:info@qxip.net) to obtain your QRYNCLOUD_LICENSE

## 📦 Installation

Let's install **qryn cloud** in a snap. All you need is a supported OS or `Docker` installed.

Stack configuration is 100% based on [ENV](/cloud/env.md) parameters passed to the process or container.

<!-- tabs:start -->

#### ** Docker **

Install `qryn` on your system using `docker` and `docker-compose`

?> qryn:cloud provides separately scalable `reader` and `writer` components

##### Log into the Container Hub
```
docker login ghcr.io
```

!> A valid user with permission to access the container packages is required. Please [contact us](mailto:info@qxip.net) to obtain permission and a token access.

##### Pull the containers
```
docker pull ghcr.io/metrico/qryn-writer-cloud
docker pull ghcr.io/metrico/qryn-go-cloud
```

##### Create Docker Compose File

Define a docker compose file to configure each element.

?> Adjust the clickhouse server and authentication details to match your setup

```yml
version: '3.1'

services:
  qryn-writer:
    image: ghcr.io/metrico/qryn-writer-cloud:latest
    container_name: qryn-writer
    restart: unless-stopped
    expose:
      - 3100
    ports:
      - 3100:3100
    environment:
      - CLICKHOUSE_SERVER=clickhouse-server
      - QRYN_DATABASE_DATA_0_NAME=qryn
      - QRYN_DATABASE_DATA_0_HOST=clickhouse.server.com
      - QRYN_DATABASE_DATA_0_PORT=9000
      - QRYN_DATABASE_DATA_0_USER=default
      - QRYN_DATABASE_DATA_0_PASS=qryn1234
      - QRYN_DATABASE_DATA_0_DEBUG=true
      - QRYN_DATABASE_DATA_0_SECURE=false
      - QRYN_DATABASE_DATA_0_ASYNC_INSERT=false
      - QRYN_HTTP_SETTINGS_PORT=3100
      - QRYN_SYSTEM_SETTINGS_DB_TIMER=0.5
      - QRYN_SYSTEM_SETTINGS_DB_BULK=1000
      - QRYN_LOG_SETTINGS_STDOUT=true
      - QRYNCLOUD_LICENSE=XXXXXXXXX-XXXXXXXXXXXX-XXXXXXXX
    command:
      - sh
      - -c
      - ./cloki-writer -initialize_db && ./cloki-writer

  qryn-go:
    image: ghcr.io/metrico/qryn-go-cloud:latest
    container_name: qryn-go
    restart: unless-stopped
    expose:
      - 3200
    ports:
      - 3200:3200
    environment:
      - CLICKHOUSE_SERVER=clickhouse-server
      - QRYN_DATABASE_DATA_0_NAME=qryn
      - QRYN_DATABASE_DATA_0_HOST=clickhouse.server.com
      - QRYN_DATABASE_DATA_0_PORT=9000
      - QRYN_DATABASE_DATA_0_HTTP_PORT=443
      - QRYN_DATABASE_DATA_0_USER=default
      - QRYN_DATABASE_DATA_0_PASS=qryn1234
      - QRYN_DATABASE_DATA_0_SECURE=false
      - QRYN_DATABASE_DATA_0_HTTPS=false
      - QRYN_DATABASE_DATA_0_DEBUG=true
      - QRYN_HTTP_SETTINGS_PORT=3200
      - QRYN_SYSTEM_SETTINGS_DB_TIMER=0.5
      - QRYN_SYSTEM_SETTINGS_DB_BULK=1000
      - QRYN_LOG_SETTINGS_STDOUT=true
      - QRYNCLOUD_LICENSE=XXXXXXXXX-XXXXXXXXXXXX-XXXXXXXX
```

##### Run Compose
```bash
docker compose up -d
docker compose ps
```

?> That's it! You are ready to access the stack using [qryn-view or Grafana](getting-started.md)

?> For an overview of all enviroment settings go [here](/cloud/env.md) and for configurations go [here](/cloud/config.md)

#### ** APT **
Install `qryn` on your system using `deb` on a Debian system _(or derivate)_

?> qryn:cloud provides separately scalable `reader` and `writer` components

##### Download
```bash
# Use provided download link from QXIP
```
##### Install
```bash
dpkg -i qryn-writer-cloud_vx.x.x_amd64.deb
dpkg -i qryn-go-cloud_vx.x.x_amd64.deb
```
##### Configure
```bash
```
##### Run
```bash
# Execute init DB on first time run
/usr/bin/qryn-writer/ -initialize_db

systemctl enable qryn-writer
systemctl start qryn-writer

systemctl enable qryn-go
systemctl start qryn-go
```

#### ** RPM **

Install `qryn` on your system using `rpm` on a RHEL based OS _(or derivate)_

?> qryn:cloud provides separately scalable `reader` and `writer` components

##### Download
```bash
# Use provided download link from QXIP
```
##### Install
```bash
dpkg -i qryn-writer-cloud_vx.x.x_x86_64.rpm
dpkg -i qryn-go-cloud_vx.x.x_x86_64.rpm
```
##### Configure
Configure per [here](/cloud/config.md)
```bash
vim /etc/qryn-writer/qryn-writer.json
vim /etc/qryn-go/qryn-go.json
```
##### Run
```bash
# Execute init DB on first time run
/usr/bin/qryn-writer/ -initialize_db

systemctl enable qryn-writer
systemctl start qryn-writer

systemctl enable qryn-go
systemctl start qryn-go
```

<!-- tabs:end -->
