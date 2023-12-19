---
sidebar_position: 10
title: Bắt đầu
sidebar_label: Bắt đầu
---

## Yêu cầu

Java (11 hoặc 17) được cài đặt trên máy tính:

- Windows hoặc macOS: [https://adoptopenjdk.net/](https://adoptopenjdk.net/)
- Ubuntu: chạy `sudo apt install default-jdk` trong Terminal

## Tải về các tệp ứng dụng

Tải về `alephium-x.x.x.jar` từ [Github](https://github.com/alephium/alephium/releases/latest) (Đừng khích chuột 2 lần để mở nó vì nó sẽ không thể mở bằng cách này).

## Khởi chạy node của bạn

1. Mở khung search và gõ `Terminal` (trên Mac và Ubuntu) hoặc `Command Prompt` (trên Windows).
2. Trong Terminal/Command Prompt program, gõ `cd your-jar-file-path` để di chuyển vào trong thư mục chứa tệp **alephium-x.x.x.jar** đã tải về trước đó.
3. Gõ đoạn code bên dưới và nhấn Enter để khởi chạy full node:
   ```shell
   java -jar alephium-x.x.x.jar
   ```

🎉 _**Tada, node của bạn đã khởi chạy thành công**_

- Node của bạn sẽ đồng bộ với network. Sẽ mất chút thời gian cho lần đầu khởi chạy và nó sẽ hoàn toàn được đồng bộ khi block height trong terminal log giống với các log được tìm thấy trong các block cuối cùng trên [explorer].
- Nếu bạn đóng cửa sổ terminal, node của bạn sẽ tắt.
- Tất cả dữ liệu blockchain được lưu trữ trên `.alephium` trong home folder[^1].

### Swagger

Chúng tôi sử dụng OpenAPI để tương tác với full node. Bạn có thể mở trực tiếp Swagger UI thông qua [http://127.0.0.1:12973/docs](http://127.0.0.1:12973/docs).

Hoặc, bạn có thể sử dụng bất kỳ OpenAPI client nào để thêm tệp `openapi.json` từ repo của chúng tôi. ([tải về](https://github.com/alephium/alephium/raw/master/api/src/main/resources/openapi.json))

### Mining

Về hướng dẫn khai thác, bạn nên tham khảo qua [Hướng dẫn Solo Mining](mining/solo-mining-guide.md) hoặc [Hướng dẫn Pool Mining](mining/pool-mining-guide.md).

### Ví

Tải về Desktop Wallet tại [GitHub](https://github.com/alephium/desktop-wallet/releases/latest).

Hoặc, full node của bạn đã có sẳn ví với các tính năng nâng cao, tham khảo qua [Hướng dẫn sử dụng node wallet](wallet/node-wallet-guide.md) để biết cách sử dụng.

[^1]:home folder sẽ theo vào hệ thống của bạn: `C:\Users\<your-username>` trong Windows, `/Users/<your-username>` trong macOS và `/home/<your-username>` trong Linux.

[explorer]: https://explorer.alephium.org
