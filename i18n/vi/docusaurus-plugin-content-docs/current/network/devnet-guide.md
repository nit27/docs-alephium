---
sidebar_position: 20
title: Hướng Dẫn Devnet
sidebar_label: Hướng dẫn Devnet
---

Để bắt đầu với devnet, chúng tôi đã xây dựng một môi trường rất thân thiện cho lập trình viên với empty block history với bất kỳ số lượng coin nào bạn muốn.

Hướng dẫn cài đặt full node trên devnet sẽ giống với cách cài đặt trên mainnet: [Hướng Dẫn Bắt Đầu Full Node](full-node/getting-started.md)

**Tệp tin `user.conf` phải được tinh chỉnh trước khi khởi chạy full node**.

Lưu ý: địa chỉ mặc định và port cho REST API là [http://127.0.0.1:12973/docs](http://127.0.0.1:12973/docs).

## Thiết lập

Tại tệp `$HOME/.alephium/user.conf` (`user.conf` nếu sử dụng docker) bạn sẽ cần phải thêm vào:

```
// in most cases, modify the following two lines
alephium.genesis.allocations = [{address = "<your-own-address>", amount = 1000000000000000000000000, lock-duration = 0 seconds}] // 1 million token allocated for your address
alephium.consensus.num-zeros-at-least-in-hash = 0

alephium.network.network-id = 4
alephium.discovery.bootstrap = []
alephium.wallet.locking-timeout = 99999 minutes
alephium.mempool.auto-mine-for-dev = true

// arbitrary mining addresses
alephium.mining.miner-addresses = [
"1FsroWmeJPBhcPiUr37pWXdojRBe6jdey9uukEXk1TheA",
"1CQvSXsmM5BMFKguKDPpNUfw1idiut8UifLtT8748JdHc",
"193maApeJWrz9GFwWCfa982ccLARVE9Y1WgKSJaUs7UAx",
"16fZKYPCZJv2TP3FArA9FLUQceTS9U8xVnSjxFG9MBKyY"
]
```

Hãy điền địa chỉ ví của bạn cho các genesis allocation. Bạn cũng có thể giảm `num-zeros-at-least-in-hash` để việc khai thác được diễn ra nhanh hơn.

Những thiết lập khác có thể được tìm thấy tại `$HOME/.alephium/network-4/` và các log tại `$HOME/.alephium/logs/`.

Nếu bạn tùy chỉnh tệp `user.conf` thì tốt hơn hết hãy xóa `$HOME/.alephium/network-4/` trước khi khởi chạy full node.

## Mining

Devnet với những tệp tùy chỉnh đơn giản có thể tự động đào các block mới cho tất cả các giao dịch. Không cần thiết phải sử dụng CPU cho việc khai thác.

Nếu bạn muốn sử dụng devnet cho việc thử nghiệm khai thác, hãy tăng độ khó lên như sau:

```
alephium.consensus.num-zeros-at-least-in-hash = 24
```
