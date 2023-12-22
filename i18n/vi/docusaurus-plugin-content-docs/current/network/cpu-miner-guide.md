---
sidebar_position: 30
title: Hướng Dẫn Đào Bằng CPU
sidebar_label: Hướng dẫn đào bằng CPU
---

:::info

Việc khai thác bằng CPU chỉ nhằm mục đích thử nghiệm. Để sử dụng testnet, hãy tham khảo qua [Hướng Dẫn Testnet](network/testnet-guide.md).

Nếu bạn thật sự muốn khai thác, hãy xem qua [Hướng Dẫn Solo Mining](mining/solo-mining-guide.md) hoặc [Hướng Dẫn Pool Mining](mining/pool-mining-guide.md).

:::

Trước tiên bạn nên tham khảo qua [Hướng Dẫn Khởi Chạy Full-node](full-node/getting-started.md) để tải về, thiết lập và bắt đầu node của bạn và sử dụng Swagger (hoặc bất kỳ các OpenAPI client).

Lưu ý: địa chỉ mặc định và port cho REST API là [http://127.0.0.1:12973/docs](http://127.0.0.1:12973/docs).

## Bắt đầu khai thác

Hãy chắc chắn rằng local node của bạn đã được đồng bộ hoàn toàn trước khi tiến hành việc khai thác. Chúng tôi sẽ thêm chức năng xác thực cho việc nào ở các bản release kế tiếp.

Bạn có thể **bắt đầu** đào trên local node bằng một POST trên `/miners/cpu-mining?action=start-mining`.

Server sẽ đơn giản trả lời với trạng thái `true` để xác nhận tiến trình khai thác đã chắn chắn bắt đầu.

Lưu ý rằng bạn sẽ cần phải tùy chỉnh các địa chỉ ví đào như đã giải thích ở [Tạo ví dành cho thợ đào](mining/solo-mining-guide.md#create-a-new-miner-wallet) trong phần Hướng Dẫn Khai Thác Bằng GPU.

## Dừng khai thác

Một cách đơn giản, bạn có thể **dừng** đào trên local node bằng một POST trên `/miners/cpu-mining?action=stop-mining`.

## Điều chỉnh CPU

Bạn có thể điều chỉnh dành bao nhiêu tài nguyên của CPU cho việc khai thác bằng 2 mã bên dưới:

    akka.actor.mining-dispatcher.fork-join-executor.parallelism-min = 1 // the minimal number of threads for mining
    akka.actor.mining-dispatcher.fork-join-executor.parallelism-max = 4 // the maximal number of threads for mining
