---
sidebar_position: 50
title: Devnet
sidebar_label: Devnet
---

# Tạo một local Devnet

## Sử dụng Docker

Nếu bạn muốn tạo một local development network với explorer, hãy dùng `docker-compose` và xem qua hướng dẫn [alphium-stack](https://github.com/alephium/alephium-stack#devnet).

## Sử dụng jar file

### Full node

Tải về `alephium-x.x.x.jar` từ [Github release](https://github.com/alephium/alephium/releases/latest) (đừng kích đúp chuột vào nó vì nó không thể chạy bằng cách này).

Tạo một configuration file tại `~/.alephium/user.conf`, đoạn mã phía dưới được trích ra từ [alephium-stack repo](https://github.com/alephium/alephium-stack/blob/master/devnet/devnet.conf)

```conf
# Import this mnemonic to have 4'000'000 token allocated for your addresses
#
# vault alarm sad mass witness property virus style good flower rice alpha viable evidence run glare pretty scout evil judge enroll refuse another lava

alephium.genesis.allocations = [
  {
    address = "1DrDyTr9RpRsQnDnXo2YRiPzPW4ooHX5LLoqXrqfMrpQH",
    amount = 1000000000000000000000000,
    lock-duration = 0 seconds
  },
  {
    address = "14UAjZ3qcmEVKdTo84Kwf4RprTQi86w2TefnnGFjov9xF",
    amount = 1000000000000000000000000,
    lock-duration = 0 seconds
  },
  {
    address = "15jjExDyS8q3Wqk9v29PCQ21jDqubDrD8WQdgn6VW2oi4",
    amount = 1000000000000000000000000,
    lock-duration = 0 seconds
  },
  {
    address = "17cBiTcWhung3WDLuc9ja5Y7BMus5Q7CD9wYBxS1r1P2R",
    amount = 1000000000000000000000000,
    lock-duration = 0 seconds
  }
]

alephium.consensus.num-zeros-at-least-in-hash = 0
alephium.consensus.uncle-dependency-gap-time = 0 seconds
alephium.network.leman-hard-fork-timestamp = 1643500800000 # GMT: 30 January 2022 00:00:00

alephium.network.network-id = 4
alephium.discovery.bootstrap = []
alephium.wallet.locking-timeout = 99999 minutes
alephium.mempool.auto-mine-for-dev = true
alephium.node.event-log.enabled=true
alephium.node.event-log.index-by-tx-id = true
alephium.node.event-log.index-by-block-hash = true

alephium.network.rest-port = 22973
alephium.network.ws-port = 21973
alephium.network.miner-api-port = 20973
alephium.api.network-interface = "0.0.0.0"
alephium.api.api-key-enabled = false
alephium.mining.api-interface = "0.0.0.0"
alephium.network.bind-address  = "0.0.0.0:19973"
alephium.network.internal-address  = "alephium:19973"
alephium.network.coordinator-address  = "alephium:19973"

# arbitrary mining addresses
alephium.mining.miner-addresses = [
  "1FsroWmeJPBhcPiUr37pWXdojRBe6jdey9uukEXk1TheA",
  "1CQvSXsmM5BMFKguKDPpNUfw1idiut8UifLtT8748JdHc",
  "193maApeJWrz9GFwWCfa982ccLARVE9Y1WgKSJaUs7UAx",
  "16fZKYPCZJv2TP3FArA9FLUQceTS9U8xVnSjxFG9MBKyY"
]
```

Lưu ý: Dãy mnemonic (24 từ) và the những địa chỉ tương ứng sẽ được tạo nhằm phục vụ cho việc khởi tạo ban đầu. Bạn có thể sử dụng chúng hoặc tự tạo những địa chỉ khác theo ý của mình, nhưng đừng bao giờ sử dụng nó trên `mainnet`. Bạn cũng có thể thêm nhiều địa chỉ nếu muốn. Nếu bạn muốn thay đổi các địa chỉ đó sau này, bạn sẽ cần phải xóa và khởi tạo lại devnet của mình.

Bạn có thể bắt đầu `devnet` với:

```sh
java -jar alephium-x.x.x.jar
```

Giờ đây bạn đã có thể truy cập vào API của full node tại: `http://localhost:22973/docs`

### Explorer-backend

Yêu cầu: https://www.postgresql.org/

Tải về `explorer-backend-x.x.x.jar` từ [Github release](https://github.com/alephium/explorer-backend/releases/latest)

Kết nối đến PostgreSQL và tạo một database cho devnet

```sql
CREATE DATABASE devnet;
```

Bạn có thể kiểm tra [configuration file](https://github.com/alephium/explorer-backend/blob/feature/contract-subcontract/app/src/main/resources/application.conf) để thấy những cài đặt nào có thể ghi đè lên. Vây lúc này bạn có thể tùy chỉnh và khởi chạy `explorer-backend` của bạn với:

```sh
export BLOCKFLOW_NETWORK_ID=2
export BLOCKFLOW_PORT=22973
export DB_NAME=devnet
java -jar explorer-backend-x.x.x.jar
```
