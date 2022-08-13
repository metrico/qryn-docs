# Motivations

**qryn** _(formerly known as cLoki)_ is a project incubated at _qxip_ build by ClickHouse users for ClickHouse users.

### Background

**qryn** implements a complete LogQL API buffered by a fast bulking **LRU** sitting on top of **ClickHouse** tables and relying on its *columnar search and insert performance alongside solid distribution and clustering capabilities* for stored data. qryn does not parse or index incoming logs, but rather groups log streams using the same label system as Prometheus and supports LogQL, PromQL and Tempo APIs for querying data back.


#### Contributors

<a href="https://github.com/lmangani/qryn/graphs/contributors">
  <img src="https://contributors-img.web.app/image?repo=lmangani/cloki" />
</a>

#### Disclaimers

©️ QXIP BV, released under the GNU Affero General Public License v3.0. See [LICENSE](LICENSE) for details.

[^1]: qryn is not affiliated or endorsed by Grafana Labs or ClickHouse Inc. All rights belong to their respective owners.

[^2]: qryn is a 100% clear-room api implementation and does not fork, use or derivate from Grafana Loki code or concepts.

[^3]: Grafana®, Loki™ and Tempo® are a Trademark of Raintank, Grafana Labs. ClickHouse® is a trademark of ClickHouse Inc. Prometheus is a trademark of The Linux Foundation.
