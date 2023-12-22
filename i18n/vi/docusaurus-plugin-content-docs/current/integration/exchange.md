---
sidebar_position: 40
title: Sàn giao dịch
sidebar_label: Sàn giao dịch
---

Hướng dẫn này sẽ giải thích cơ bản các API và thông tin cần thiết để tích hợp Alephium với một sàn giao dịch điện tử.

## Bắt đầu

### Local development network

Để tích hợp Alephium, một exchange (sàn giao dịch) phải chạy full node. Hơn nữa, explorer-backend có thể chạy để debug và index

Để tạo một local development network với explorer, tham khảo qua hướng dẫn trong [alephium-stack](https://github.com/alephium/alephium-stack#devnet) repository. Khi đã chạy, Swagger UI có thể truy cập vào API interface của full node và explorer backend.

* Full node Swagger UI: [http://127.0.0.1:22973/docs](http://127.0.0.1:22973/docs)
* Explorer backend Swagger UI: [http://127.0.0.1:9090/docs](http://127.0.0.1:9090/docs)
* Explorer front-end: [http://localhost:23000](http://localhost:23000)

### APIs

Để hướng dẫn được ngắn gọn, các relevant API queries sẽ được cung cấp ở trong tài liệu hướng dẫn thay vì các ảnh chụp màn hình của Swagger UI.

[web3 SDK](https://github.com/alephium/alephium-web3#packages) chứa các Typescript APIs đã tạo cho cả [full node](https://github.com/alephium/alephium-web3/blob/master/packages/web3/src/api/api-alephium.ts) và [explorer backend](https://github.com/alephium/alephium-web3/blob/master/packages/web3/src/api/api-explorer.ts).

### Test wallet

Hãy khôi phục test wallet bằng cách thi hành API bên dưới. Test wallet có 1 triệu ALPH cho địa chỉ `1DrDyTr9RpRsQnDnXo2YRiPzPW4ooHX5LLoqXrqfMrpQH`.

```shell
curl -X 'PUT' \
  'http://127.0.0.1:22973/wallets' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "password": "test",
  "mnemonic": "vault alarm sad mass witness property virus style good flower rice alpha viable evidence run glare pretty scout evil judge enroll refuse another lava",
  "walletName": "test"
}'

# Response
# {
#   "walletName": "test"
# }
```

Lấy public key cho địa chỉ bằng:

```shell
curl -X 'GET' \
  'http://127.0.0.1:22973/wallets/test/addresses/1DrDyTr9RpRsQnDnXo2YRiPzPW4ooHX5LLoqXrqfMrpQH' \
  -H 'accept: application/json'

# Response
# {
#   "address": "1DrDyTr9RpRsQnDnXo2YRiPzPW4ooHX5LLoqXrqfMrpQH",
#   "publicKey": "0381818e63bd9e35a5489b52a430accefc608fd60aa2c7c0d1b393b5239aedf6b0",
#   "group": 0,
#   "path": "m/44'/1234'/0'/0/0"
# }
```

## Transaction APIs

### Tạo một giao dịch

Hãy tạo một giao dịch và gửi `1.23 ALPH` vào địa chỉ `1C2RAVWSuaXw8xtUxqVERR7ChKBE1XgscNFw73NSHE1v3`.

```shell
# `fromPublicKey` is the public key of the wallet address

curl -X 'POST' \
  'http://127.0.0.1:22973/transactions/build' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "fromPublicKey": "0381818e63bd9e35a5489b52a430accefc608fd60aa2c7c0d1b393b5239aedf6b0",
  "destinations": [
    {
      "address": "1C2RAVWSuaXw8xtUxqVERR7ChKBE1XgscNFw73NSHE1v3",
      "attoAlphAmount": "1230000000000000000"
    }
  ]
}'

# Response:
# {
#   "unsignedTx": "00040080004e20c1174876e8000137a444479fa782e8b88d4f95e28b3b2417e5bc30d33a5ae8486d4a8885b82b224259c1e6000381818e63bd9e35a5489b52a430accefc608fd60aa2c7c0d1b393b5239aedf6b002c41111d67bb1bb000000a3cd757be03c7dac8d48bf79e2a7d6e735e018a9c054b99138c7b29738c437ec00000000000000000000c6d3c20ab5db74a5b8000000bee85f379545a2ed9f6cceb331288842f378cf0f04012ad4ac8824aae7d6f80a00000000000000000000",
#   "gasAmount": 20000,
#   "gasPrice": "100000000000",
#   "txId": "a6c14ad03597ce96ebf78b336aded654395f38e0274c810183c4847d9af3d617",
#   "fromGroup": 0,
#   "toGroup": 1
# }
```

### Sign a transaction

Bây giờ hãy ký (sign) vào transaction id:

```shell
curl -X 'POST' \
  'http://127.0.0.1:22973/wallets/test/sign' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "data": "a6c14ad03597ce96ebf78b336aded654395f38e0274c810183c4847d9af3d617"
}'

# Response
# {
#   "signature": "78a607ec26165b5a63d7e30a0c85657e8a0fe3b7efccdba78166e51b52c32c9020f921e0a29b6a436ec330c3b3eb2222ee851e718e3504b1a70d73ba45cd503c"
# }
```

### Submit a transaction

Hãy gửi (submit) giao dịch vào network:

```shell
# `unsignedTx` is from the response of transaction building
# `signature` is from the response of transaction signing

curl -X 'POST' \
  'http://127.0.0.1:22973/transactions/submit' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "unsignedTx": "00040080004e20c1174876e8000137a444479fa782e8b88d4f95e28b3b2417e5bc30d33a5ae8486d4a8885b82b224259c1e6000381818e63bd9e35a5489b52a430accefc608fd60aa2c7c0d1b393b5239aedf6b002c41111d67bb1bb000000a3cd757be03c7dac8d48bf79e2a7d6e735e018a9c054b99138c7b29738c437ec00000000000000000000c6d3c20ab5db74a5b8000000bee85f379545a2ed9f6cceb331288842f378cf0f04012ad4ac8824aae7d6f80a00000000000000000000",
  "signature": "78a607ec26165b5a63d7e30a0c85657e8a0fe3b7efccdba78166e51b52c32c9020f921e0a29b6a436ec330c3b3eb2222ee851e718e3504b1a70d73ba45cd503c"
}'

# Response
# {
#   "txId": "a6c14ad03597ce96ebf78b336aded654395f38e0274c810183c4847d9af3d617",
#   "fromGroup": 0,
#   "toGroup": 1
# }
```

## Block APIs

### Lấy block hash với transaction ID

Để lấy block hash của một giao dịch đã xác nhận, bạn có thể sử dụng full node API:

```shell
curl -X 'GET' \
  'http://127.0.0.1:22973/transactions/status?txId=a6c14ad03597ce96ebf78b336aded654395f38e0274c810183c4847d9af3d617' \
  -H 'accept: application/json'

# Response
# {
#   "type": "Confirmed",
#   "blockHash": "1d616d33a7aadc3cf49f5db1cc484b22a642140673f66020c13dc7648b9382d1",
#   "txIndex": 0,
#   "chainConfirmations": 1,
#   "fromGroupConfirmations": 1,
#   "toGroupConfirmations": 0
# }
```

### Lấy block với block hash

```shell
curl -X 'GET' \
  'http://127.0.0.1:22973/blockflow/blocks-with-events/ecbc7a3115eb0da1f82902db226b80950e861ef8cbb6623ed02fc42a6eeb69cb' \
  -H 'accept: application/json'

# Response
# {
#   "block": {
#     "hash": "ecbc7a3115eb0da1f82902db226b80950e861ef8cbb6623ed02fc42a6eeb69cb",
#     "timestamp": 1231006505000,
#     "chainFrom": 2,
#     "chainTo": 3,
#     "height": 0,
#     ...
#   },
#   "events": []
# }
```

### Polling for blocks

Trong Alephium, bạn có thể fetch tất cả các block từ tất cả các chain trong một khoảng thời gian nhất định bởi vì nó là một sharded blockchain với các multiple chains và vận hành đồng thời ở các height khác nhau.

```shell
curl -X 'GET' \
  'http://127.0.0.1:22973/blockflow/blocks?fromTs=0&toTs=30' \
  -H 'accept: application/json'

# Response: there are 16 chains, therefore 16 lists of block hashes
# {
#   "blocks": [
#     [],
#     ...
#     []
#   ]
# }
```

Bạn có thể khôi phục các block ở mỗi chain:
```shell
curl -X 'GET' \
  'http://127.0.0.1:22973/blockflow/chain-info?fromGroup=2&toGroup=3' \
  -H 'accept: application/json'

# Response
# {
#   "currentHeight": 0
# }

curl -X 'GET' \
  'http://127.0.0.1:22973/blockflow/hashes?fromGroup=2&toGroup=3&height=0' \
  -H 'accept: application/json'

# Response
# {
#   "headers": [
#     "ecbc7a3115eb0da1f82902db226b80950e861ef8cbb6623ed02fc42a6eeb69cb"
#   ]
# }
```

## UTXO Management

### Tại sao phải quản lý UTXO?

Trong thực tế, một vài thợ đào có khuynh hướng gửi các mining reward trực tiếp vào trong địa chỉ ví trên sàn giao dịch, điều này vô tình tạo ra nhiều các small-valued UTXO vào trong hot wallet của địa ví sàn. Tuy nhiên, vì có một con số giới hạn các input để nó có thể được đi chung với mỗi giao dịch nên việc rút về có thể sẽ thất bại nếu hot wallet nhận các phần nhỏ UTXO đó.

### Làm sao để hợp nhất các small-valued UTXO?

Nếu sàn giao dịch bạn đang sử dụng đã có sẳn UTXO management framework, điều này rất tốt. Nhưng nếu chưa, bạn vẫn có giải pháp đơn giản để khắc phục nó. Bạn có thể sử dụng chức năng sweep để hợp nhất các small-valued UTXO của một địa chỉ nhất định. Lưu ý chức năng này chỉ khả dụng từ bản full node `2.3.0`.

```shell
# `maxAttoAlphPerUTXO` refers to the maximum amount of ALPH in the UTXOs to be consolidated.

curl -X 'POST' \
  'http://127.0.0.1:22973/transactions/sweep-address/build' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "fromPublicKey": "0381818e63bd9e35a5489b52a430accefc608fd60aa2c7c0d1b393b5239aedf6b0",
  "toAddress": "1DrDyTr9RpRsQnDnXo2YRiPzPW4ooHX5LLoqXrqfMrpQH",
  "maxAttoAlphPerUTXO": "100 ALPH"
}'
```

### Làm sao để sử dụng designated UTXOs?

Để các giao dịch trở nên hiệu quả hơn, chúng tôi khuyến khích các bạn lưu trữ dãy các UTXO của hot wallets và đưa ra các UTXO cụ thể thông qua API.

```shell
curl -X 'POST' \
  'http://127.0.0.1:22973/transactions/build' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "fromPublicKey": "0381818e63bd9e35a5489b52a430accefc608fd60aa2c7c0d1b393b5239aedf6b0",
  "destinations": [
    {
      "address": "1C2RAVWSuaXw8xtUxqVERR7ChKBE1XgscNFw73NSHE1v3",
      "attoAlphAmount": "230000000000000000"
    }
  ]
  "utxos": [
    {
        "hint": 714242201,
        "key": "3bfdeea82a5702cdd98426546d9eeecd744cc540aaffc5ec8ea998dc105da46f"
    }
  ]
}'
```

`hint` và `key` cho UTXO được fetch từ output đầu tiên trên giao dịch đầu tiên chúng ta đã thực hiện. `key` là độc nhất và có thể được sử dụng để index cho UTXO.

```shell
curl -X 'GET' \
  'http://127.0.0.1:22973/transactions/details/a6c14ad03597ce96ebf78b336aded654395f38e0274c810183c4847d9af3d617' \
  -H 'accept: application/json'

# Response
# {
#   "unsigned": {
#     "txId": "a6c14ad03597ce96ebf78b336aded654395f38e0274c810183c4847d9af3d617",
#     ...
#     "fixedOutputs": [
#       {
#         "hint": 714242201,
#         "key": "3bfdeea82a5702cdd98426546d9eeecd744cc540aaffc5ec8ea998dc105da46f",
#         "attoAlphAmount": "1230000000000000000",
#         "address": "1C2RAVWSuaXw8xtUxqVERR7ChKBE1XgscNFw73NSHE1v3",
#         ...
#       },
#       {
#         "hint": 933512263,
#         "key": "087ee967733900cc7f7beada612ba514dd134ddffc2ad1b6ad8b6998915089c4",
#         "attoAlphAmount": "999998768000000000000000",
#         "address": "1DrDyTr9RpRsQnDnXo2YRiPzPW4ooHX5LLoqXrqfMrpQH",
#         ...
#       }
#     ]
#   },
#   ...
# }
```

## Các thông tin khác

### Tạo Ví

Để tạo nhiều địa chỉ ví cho các user, bạn có thể sử dụng [HD-wallet trong web3 SDK](https://github.com/alephium/alephium-web3/blob/master/packages/web3-wallet/src/hd-wallet.ts#L112-L185).

### Sharding

Alephium là một sharded blockchain các địa chỉ ví trên nó được chia thành 4 group trên mainnet. Tuy nhiên, bạn có thể: 
- Gửi ALPH đến nhiều các địa chỉ ví mà chúng nó ở trong cùng một địa chỉ group trong một giao dịch đơn. Tất cả các địa chỉ đích đến phải nằm trong cùng một group.
- Gửi ALPH từ nhiều địa chỉ ví mà nó nằm trong cùng một địa chỉ group trong một giao dịch đơn. Tất cả các địa chỉ dùng để gửi phải nằm trong cùng một group.
- Gửi ALPH từ nhiều các địa chỉ mà nó nằm trong cùng một group đến nhiều địa chỉ ví khác mà nó nằm trong một group khác. Tất cả các địa chỉ dùng để gửi đi phải nằm trong cùng một group và tất cả các địa chỉ đích đến cũng phải nằm trong cùng một group.

Để lấy group của một địa chỉ, bạn có thể tham khảo chức năng của web3 SDK [groupOfAddress(address)](https://github.com/alephium/alephium-web3/blob/master/packages/web3/src/utils/utils.ts#L85-L103).

### Gas computation

Phí giao dịch trên Alephium được xác định bằng số lượng gas đã phân bổ và giá gas. Số gas tố đa là 625,000 và nó có thể được phân bổ cho mỗi giao dịch. Giá gas mặc định được đặt tại `1e11` attoALPH trên một đơn vị gas. Khi tiến hành một giao dịch di chuyển tài sản, lượng gas có thể  được tính toán bằng cách sử dụng đoạn code bên dưới:

```Typescript
txInputBaseGas = 2000
txOutputBaseGas = 4500
inputGas = txInputBaseGas * tx.inputs.length
outputGas = txOutputBaseGas * tx.outputs.length

txBaseGas = 1000
p2pkUnlockGas = 2060 // Currently there is only one signature

txGas = inputGas + outputGas + txBaseGas + p2pkUnlockGas
minimalGas = 20000

gas = max(minimalGas, txGas)
```
