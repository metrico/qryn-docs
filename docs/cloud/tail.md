
# qryn.cloud

<a href="https://qryn.dev" target="_blank"><img src='https://user-images.githubusercontent.com/1423657/218816262-e0e8d7ad-44d0-4a7d-9497-0d383ed78b83.png' width=250></a>

# qryn-tail-proxy

Tiny **LogQL API** Proxy with **Live Tailing** capabilities for [qryn.cloud](https://qryn.cloud) integrators

<img src="https://user-images.githubusercontent.com/1423657/226195413-5a21cc5d-3f88-4f8f-a34d-c4586d36847d.png" width=600>



### Features
- Label Selection
- Basic filtering (`|=`, `!=`, `=` )

### Installation

#### Docker
```
ghcr.io/metrico/qryn-tail-proxy:latest
```

#### Usage

Run `qryntail` with the following `ENV` parameters:

| Name | Default | Dscription                                          |
| --- | --- |-----------------------------------------------------|
| HOST | 127.0.0.1 | API Host to listen                                      |
| PORT | 3000 | API Port to listen                                      |
| LOGLEVEL | INFO | loglevel to write console logs (INFO, DEBUG, ERROR) |
| APIURL | https://qryn.gigapipe.com/ | URL for qryn backend receiver for forward data |
