---
sidebar_position: 40
title: Node Wallet
sidebar_label: Node wallet
---

Wallet API có thể được call bằng cách sử dụng Swagger UI tại [http://127.0.0.1:12973/docs](http://127.0.0.1:12973/docs) hoặc sử dụng `curl`. Hãy chắc chắn rằng full node của bạn đang chạy is để có thể truy cập vào Swagger UI.

## Tạo một wallet mới

Bạn có thể tạo một wallet mới bằng một POST với dữ liệu trong `/wallets`.

```json
{
  "password": "123456",
  "walletName": "foo" //optional (wallet-x) by default
}
```

Server sẽ phản hồi và sẽ cho bạn thông tin của wallet mnemonic.

```json
{
  "walletName": "foo",
  "mnemonic": "laptop tattoo torch range exclude fuel bike menu just churn then busy century select cactus across other merge vivid alarm asset genius mountain transfer"
}
```

Fetch địa chỉ của wallet bằng `GET /wallets/{wallet_name}/addresses`

```json
{
  "activeAddress": "T1J2yrmQrNwuFW8z2W6xXFLtJoBCWEm7gLg9BuY8tzKjxw",
  "addresses": ["T1J2yrmQrNwuFW8z2W6xXFLtJoBCWEm7gLg9BuY8tzKjxw"]
}
```

Nếu bạn đã tạo một wallet rồi nhưng nó đã bị xóa hoặc bạn không nhớ mật khẩu, bạn có thể khôi phục wallet đó bằng `mnemonic`:

```
PUT /wallets
{
    "password": "123456",
    "mnemonic": "laptop tattoo torch range exclude fuel bike menu just churn then busy century select cactus across other merge vivid alarm asset genius mountain transfer",
    "walletName": "foo" //optional
}
```

## Lock/Unlock

Wallet của bạn sẽ tự động được khóa bảo vệ lại trong một khoảng thời gian, bạn sẽ cần phải mở khóa nó để có thể sử dụng:

```
POST /wallets/{wallet_name}/unlock
{
    "password": "123456"
}
```

Bạn cũng có thể khóa ví thủ công bằng cách:

```
POST /wallets/{wallet_name}/lock
```

## Query for balance

Bạn có thể kiểm tra số dư hiện tại bằng `GET /wallets/{wallet_name}/balances`
:

```json
{
  "totalBalance": 0,
  "balances": [
    {
      "address": "T1J2yrmQrNwuFW8z2W6xXFLtJoBCWEm7gLg9BuY8tzKjxw",
      "balance": 0
    }
  ]
}
```

## Transfering funds

Bạn có thể thực hiện một giao dịch từ một wallet đến một địa chỉ bằng cách:

```
POST /wallets/{wallet_name}/transfer
{
    "destinations ": [{
        "address": "<the destination address>",
        "amount ": "42 ALPH"
    }]
}
```

Server sẽ phản hồi với thông tin transaction id và thông tin của group.

```json
{
  "txId": "50318e5bfd56796690890f4a9c5aae2725629a15a71cad909bbf4a669c32c2f4",
  "fromGroup": 0,
  "toGroup": 3
}
```
