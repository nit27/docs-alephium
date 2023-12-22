---
sidebar_position: 30
title: Khắc phục lỗi 
---

# Khắc phục lỗi  

#### Tại sao tôi chỉ có thể khôi phục 1 trong 4 địa chỉ ví khai thác của tôi?

Bạn phải chỉ định `isMiner = true` khi khôi phục địa chỉ ví khai thác. Vui lòng xem ví dụ tại đây: [Khôi phục ví khai thác](solo-mining-guide.md#restore-your-miner-wallet)

#### Làm sao để kết nối máy đào với full node trên một máy tính khác trong cùng một subnet?

1. Thêm thông tin sau vào `user.conf` của bạn và khởi động lại full node.

```
alephium.mining.api-interface = "0.0.0.0"
```

2. Chạy máy đào của bạn với `-a IP`, trong đó IP là IP của full node trong subnet.

#### Cách sử dụng Swagger UI của VPS chạy bằng full node

Chuyển tiếp cổng SSH được khuyến nghị:

```
ssh user@server  -L 12973:127.0.0.1:12973
```

#### Cách để truy cập Swagger UI của full node trên một máy tính khác trong subnet ?

1. Thêm thông tin sau vào `user.conf` của bạn và khởi động lại full node.

```
alephium.api.network-interface = "0.0.0.0"
```

2. Thay đổi `host` của Swagger UI thành IP của full node.

#### Máy đào của tôi (thông qua run-miner.sh) không thể kết nối với full node trên một máy tính khác 

Lệnh `run-miner.sh` kết nối với `127.0.0.1` theo mặc định. Bạn cần phải thêm `-a IP` vào `run-miner.sh`.

#### Tại sao máy đào của tôi lại chiếm lượng bộ nhớ lớn trên HiveOS?

Bạn nên tắt `log to write in RAM` bằng lệnh `logs-on`.

#### Làm sao để tuỳ chỉnh thời gian chờ tự động khoá (auto-lock timeout) cho ví?

Bạn có thể tahy đổi auto-lock timeout của ví với cấu hình sau:

```
alephium.wallet.locking-timeout = 10 minutes
```
