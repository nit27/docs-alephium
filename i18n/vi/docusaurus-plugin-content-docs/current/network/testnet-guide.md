---
sidebar_position: 10
title: Hướng Dẫn Testnet
sidebar_label: Hướng dẫn testnet
---

Cách cài đặt full node trên testnet sẽ giống với cách cài trên mainnet: [Hướng Dẫn Bắt Đầu Full Node](full-node/getting-started.md)

**Tệp tin `user.conf` phải được tùy tinh chỉnh trước khi khởi chạy full node**.

Lưu ý: Địa chỉ mặc địnhh và port cho REST API là [http://127.0.0.1:12973/docs](http://127.0.0.1:12973/docs).

## Thiết lập

Tại `$HOME/.alephium/user.conf` (`user.conf` nếu sử dụng docker) bạn sẽ cần phải thêm:

```
alephium.network.network-id = 1
alephium.discovery.bootstrap = ["testnet-bootstrap0.alephium.org:9973","testnet-bootstrap1.alephium.org:9973"]
```

## Mining

Trên testnet bạn có thể sử dụng [CPU Miner Guide](cpu-miner-guide.md) để khai thác vài ALPH

Thêm vào địa chỉ ví đào (mining address) tại `$HOME/.alephium/user.conf`:

```
alephium.mining.miner-addresses = [
"1FsroWmeJPBhcPiUr37pWXdojRBe6jdey9uukEXk1TheA",
"1CQvSXsmM5BMFKguKDPpNUfw1idiut8UifLtT8748JdHc",
"193maApeJWrz9GFwWCfa982ccLARVE9Y1WgKSJaUs7UAx",
"16fZKYPCZJv2TP3FArA9FLUQceTS9U8xVnSjxFG9MBKyY"
]
```

:::info 

Bạn có thể dễ dàng tạo ra các địa chỉ ví đào bằng cách cài đặt [desktop wallet](../wallet/desktop-wallet/configure-mining-wallet) và tạo một ví với 4 địa chỉ. Bây giờ bạn có thể sao chép và dán chúng vào trong tệp `user.conf` như đã đề cập bên trên.

:::

## Ví dụ về thiết lập

```
alephium.api.network-interface = "0.0.0.0"
alephium.mining.api-interface = "0.0.0.0"
alephium.network.network-id = 1
alephium.discovery.bootstrap = ["testnet-bootstrap0.alephium.org:9973","testnet-bootstrap1.alephium.org:9973"]
alephium.mining.miner-addresses = [
"1FsroWmeJPBhcPiUr37pWXdojRBe6jdey9uukEXk1TheA",
"1CQvSXsmM5BMFKguKDPpNUfw1idiut8UifLtT8748JdHc",
"193maApeJWrz9GFwWCfa982ccLARVE9Y1WgKSJaUs7UAx",
"16fZKYPCZJv2TP3FArA9FLUQceTS9U8xVnSjxFG9MBKyY"
]
alephium.api.api-key-enabled = false
```
