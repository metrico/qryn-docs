<!-- ![image](https://user-images.githubusercontent.com/1423657/197589119-bb790fba-dd50-412c-92c1-033675fa980a.png ':size=200') -->
<a href="https://app.gigapipe.com/signup?ref=qxip" target="_blank">
  <!-- <img src="https://user-images.githubusercontent.com/1423657/200078144-5d0b0960-2ad8-4b0c-9cdd-b7f8f9f516ae.png" width=500 /> -->
  <img src="https://user-images.githubusercontent.com/1423657/200078554-f8352174-9a6b-4f4a-90fc-1c6521d46c5b.png" width=600 />  
</a>

# ☁️  [qryn:cloud](/cloud)

!> qryn:cloud requires activation licenses. please [contact us](mailto:info@qxip.net) to obtain your keys.

## 📦 Configuration

Let's configure **qryn:cloud** for our deployment. 

Stack configuration is 100% based on [ENV](env.md) parameters passed to the process or container.

<!-- tabs:start -->

#### ** qryn-ctrl **
<a id=ctrl name=ctrl></a>

See an example, where you use ENV vars as defined on the URL above:
```
  qryn-ctrl:
    image: ${version_qryn_ctrl}
    container_name: qryn-ctrl
    depends_on:
      clickhouse-server:
        condition: service_healthy
    environment:
      - QRYN_DATABASE_DATA_0_NAME=${qryn_db_name:-qryn}
      - QRYN_DATABASE_DATA_0_HOST=${clickhost:-clickhouse-server}
      - QRYN_DATABASE_DATA_0_PORT=${clickhouse_port:-9000}
      - QRYN_DATABASE_DATA_0_USER=${clickuser:-default}
      - QRYN_DATABASE_DATA_0_PASS=${clickpass:-clickPass}
      - QRYN_DATABASE_DATA_0_DEBUG=${qryn_debug:-true}
      - QRYN_DATABASE_DATA_0_TTL_DAYS=${qryn_db_ttl:-7}
      - QRYN_DATABASE_DATA_0_SECURE=${qryn_db_secure:-false}
      - QRYN_DATABASE_DATA_0_ASYNC_INSERT=${qryn_db_async_insert:-false}
    command:
      - sh
      - -c
      - ./qryn-ctrl -initialize_db
```

#### ** qryn-go **
<a id=api name=api></a>

See an example, where you use ENV vars as defined on the URL above:
```
  qryn-go:
    image: ${version_qryn_reader}
    container_name: qryn-go
    restart: unless-stopped
    volumes:
      - qryn_reader_data:/var/lib/.prof
      - qryn_reader_data:/var/lib/.local
    expose:
      - ${qryn_go_port}
    ports:
      - 3200:${qryn_go_port}
    environment:
      - QRYNCLOUD_LICENSE=${qryn_license}
      - QRYN_SYSTEM_SETTINGS_LICENSE_AUTO_SHUTDOWN=${qryn_license_auto_shutdown:-true}
      - QRYN_DATABASE_DATA_0_NAME=${qryn_db_name:-qryn}
      - QRYN_DATABASE_DATA_0_HOST=${clickhost:-clickhouse-server}
      - QRYN_DATABASE_DATA_0_PORT=${clickhouse_port:-9000}
      - QRYN_DATABASE_DATA_0_DEBUG=${qryn_debug:-true}
      - QRYN_DATABASE_DATA_0_USER=${clickuser:-default}
      - QRYN_DATABASE_DATA_0_PASS=${clickpass:-clickPass}
      - QRYN_HTTP_SETTINGS_PORT=${qryn_go_port:-3200}
      - QRYN_HTTP_SETTINGS_DEBUG=${qryn_http_debug:-true}
      - QRYN_LOG_SETTINGS_STDOUT=${qryn_log_stdout:-true}
      - QRYN_LOG_SETTINGS_LEVEL=${qryn_log_lvl:-debug}
    depends_on:
        clickhouse-server:
          condition: service_healthy
        qryn-ctrl:
          condition: service_completed_successfully
    logging:
      driver: "local"
      options:
        max-size: "10m"  # Rotate logs when they reach 10MB
        max-file: "5"    # Keep up to 5 rotated log files
```

#### ** qryn-writer **
<a id=writer name=writer></a>

See an example, where you use ENV vars as defined on the URL above:
```
  qryn-writer:
    image: ${version_qryn_writer}
    container_name: qryn-writer
    restart: unless-stopped
    volumes:
      - qryn_writer_data:/var/lib/.prof
      - qryn_writer_data:/var/lib/.local
    expose:
      - ${qryn_writer_port}
    ports:
      - 3100:${qryn_writer_port}
    environment:
      - QRYNCLOUD_LICENSE=${qryn_license}
      - QRYN_SYSTEM_SETTINGS_LICENSE_AUTO_SHUTDOWN=${qryn_license_auto_shutdown:-true}
      - QRYN_DATABASE_DATA_0_NAME=${qryn_db_name:-qryn}
      - QRYN_DATABASE_DATA_0_HOST=${clickhost:-clickhouse-server}
      - QRYN_DATABASE_DATA_0_PORT=${clickhouse_port:-9000}
      - QRYN_DATABASE_DATA_0_DEBUG=${qryn_debug:-true}
      - QRYN_DATABASE_DATA_0_USER=${clickuser:-default}
      - QRYN_DATABASE_DATA_0_PASS=${clickpass:-clickPass}
      - QRYN_HTTP_SETTINGS_PORT=${qryn_writer_port:-3100}
      - QRYN_LOG_SETTINGS_STDOUT=${qryn_log_stdout:-true}
      - QRYN_LOG_SETTINGS_LEVEL=${qryn_log_lvl:-debug}
    command:
      - sh
      - -c
      - ./cloki-writer
    depends_on:
      clickhouse-server:
        condition: service_healthy
      qryn-ctrl:
          condition: service_completed_successfully
    logging:
      driver: "local"
      options:
        max-size: "10m"  # Rotate logs when they reach 10MB
        max-file: "5"    # Keep up to 5 rotated log files
```

<!-- tabs:end -->
