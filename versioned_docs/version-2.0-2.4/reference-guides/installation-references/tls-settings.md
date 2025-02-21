---
title: TLS Settings
---

In Rancher v2.1.7, the default TLS configuration changed to only accept TLS 1.2 and secure TLS cipher suites. TLS 1.3 and TLS 1.3 exclusive cipher suites are not supported.

# Configuring TLS settings

The Audit Log is enabled and configured by passing environment variables to the Rancher server container. See the following to enable on your installation.

- [TLS settings in Docker options](../single-node-rancher-in-docker/advanced-options.md#tls-settings)

- [TLS settings in Helm chart options](helm-chart-options.md#tls-settings)

# TLS Environment Variables

| Parameter | Description | Default | Available options |
|-----|-----|-----|-----|
| `CATTLE_TLS_MIN_VERSION` | Minimum TLS version | `1.2` | `1.0`, `1.1`, `1.2` |
| `CATTLE_TLS_CIPHERS` | Allowed TLS cipher suites | `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,`<br/>`TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,`<br/>`TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305,`<br/>`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,`<br/>`TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,`<br/>`TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305` | See [Golang tls constants](https://golang.org/pkg/crypto/tls/#pkg-constants) |


# Legacy configuration

If you need to configure TLS the same way as it was before Rancher v2.1.7, please use the following settings:


| Parameter | Legacy value |
|-----|-----|
| `CATTLE_TLS_MIN_VERSION` | `1.0` |
| `CATTLE_TLS_CIPHERS` | `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,`<br/>`TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,`<br/>`TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,`<br/>`TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,`<br/>`TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305,`<br/>`TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305,`<br/>`TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,`<br/>`TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA,`<br/>`TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA,`<br/>`TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA,`<br/>`TLS_RSA_WITH_AES_128_GCM_SHA256,`<br/>`TLS_RSA_WITH_AES_256_GCM_SHA384,`<br/>`TLS_RSA_WITH_AES_128_CBC_SHA,`<br/>`TLS_RSA_WITH_AES_256_CBC_SHA,`<br/>`TLS_ECDHE_RSA_WITH_3DES_EDE_CBC_SHA,`<br/>`TLS_RSA_WITH_3DES_EDE_CBC_SHA`
