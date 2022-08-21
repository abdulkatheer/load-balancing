NOTE: This is tested with Envoy version c919bdec19d79e97f4f56e4095706f8e6a383f1c/1.22.2/Modified/RELEASE/BoringSSL

## Install Envoy
brew install envoy
## Start envoy
envoy --config-path http.yaml
# HTTP/ Layer 7
## All backend
## Conditional splitting
## Conditional blocking/response

# TCP/ Layer 4
## Just All backend
## Brower
reuses same TCP connection (long-lived, so sticky)
## cURL
New TCP connection everytime, but still not load-balanced as expected. This is bcz of Threading model of Envoy.
## Threads don't share info with each other, that leads to this inconsistent loab-balancing.
Disabling Multithreading
envoy --config-path tcp.yaml --concurrency 1
Still browser may not work as expected, bcz of TCP connection reuse
But cURL will work as expected.

# TLS
## Create key-pairs
Obtain cert and private key in PEM format and store it as cert.pem and private.pem file in root directory
then set permissions to private key
chmod 755 private.pem

Disable TLS 1.1 and lower
Enable H2 and HTTP 1.1 and disable rest (ALPN will offer these two)
Downstream still don't have TLS and H2 enabled.