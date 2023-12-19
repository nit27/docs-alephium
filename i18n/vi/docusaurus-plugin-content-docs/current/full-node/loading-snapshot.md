---
sidebar_position: 35
title: Loading from a snapshot
sidebar_label: Loading from snapshot
---

# Full node

Một trong những điều đầu tiên của việc chạy full node, như đã mô tả ở [Bắt đầu](./getting-started) sẽ mất một khoảng thời gian để hoàn tất quá trình đồng bộ với các node khác trên network. Nói một cách khác, bạn cần phải đợi đến khi quá trình tải xuống một bản sao của toàn bộ blockchain.

Để có thể rút ngắn giai đoạn của quá trình đầu tiên này, những phiên bản snapshot [đã được đóng góp trên Alephium Archives repository](https://archives.alephium.org). Một quá trình tự động tải lên các bản snapshot cho cả [testnet](https://archives.alephium.org/#testnet/) và [mainnet](https://archives.alephium.org/#mainnet/).

## Tải về một snapshot

Trước khi khởi chạy full node ở lần đầu tiên, bạn có thể tải về một bản snapshot gần nhất và giải nén nó (một tệp `tar`) vào thư mục của bạn. Đoạn mã dưới đây sẽ hỗ trợ bạn làm việc đó mà không yêu cầu bộ nhớ còn trống quá nhiều để có thể tải về và giải nén snapshot:

```shell
ALEPHIUM_HOME=~/.alephium
ALEPHIUM_NETWORK=mainnet
curl -L "$(curl -s https://archives.alephium.org/archives/$ALEPHIUM_NETWORK/full-node-data/_latest.txt)" | tar xf - -C "$ALEPHIUM_HOME/"
```

Một tệp tên là `_latest.txt` được cập nhật để bạn tiện theo dõi hơn. Nó luôn cho bạn biết phiên bản snapshot được cập nhật mới nhất.

## Sử dụng một bản script có sẳn

Mặc dù các câu lệnh bên trên có thể chạy ổn, nhưng đôi khi nó có thể xãy ra lỗi không mong muốn ví dụ như làm cho database của full node không được trong trạng thái cập nhật mới nhất. Cho nên bạn có thể sử dụng đoạn script sẳn bên dưới để hạn chế xãy ra lỗi.

```shell
ALEPHIUM_HOME=/tmp
ALEPHIUM_NETWORK=mainnet
curl -L https://github.com/touilleio/alephium-standalone/raw/main/snapshot-loader.sh | env ALEPHIUM_HOME=${ALEPHIUM_HOME} ALEPHIUM_NETWORK=${ALEPHIUM_NETWORK} sh
```

## Khởi chạy một container độc lập

Và nếu bạn muốn thử những cài đặt này một cách nhanhh chóng, một bản OCI image (đơn giản nó là bản image mở rộng của `alephium/alephium`), sẽ tự động cài đặt những thứ cần thiết: `touilleio/alephium-standalone`. [Source code của nó](https://github.com/touilleio/alephium-standalone) sẽ cung cấp những hướng dẫn sử dụng chi tiết cần thiết .

Đoạn mã bên dưới là một ví dụ tham khảo để chạy một standalone container:

```
ALEPHIUM_HOME=/tmp
ALEPHIUM_NETWORK=mainnet
docker run -p 39973:39973 -p 127.0.0.1:12973:12973 \
  -v ${ALEPHIUM_HOME}:/alephium-home/.alephium \
  -e ALEPHIUM_NETWORK=${ALEPHIUM_NETWORK} touilleio/alephium-standalone:latest
```

# Explorer database

[Alephium Archives repository](https://archives.alephium.org) là repo được biết đến rộng rãi với trong việc sử dụng explorer database snapshot. Các snapshot có thể được tải về trong postgresql database của explorer backend ở lần chạy đầu tiên, bạn có thể chạy lệnh bên dưới:

```shell
ALEPHIUM_NETWORK=mainnet
curl -L $(curl -L -s https://archives.alephium.org/archives/${ALEPHIUM_NETWORK}/explorer-db/_latest.txt) | gunzip -c | psql -U $pg_user -d $database
```
