---
sidebar_position: 10
title: Tổng quan
sidebar_label: Tổng quan
---

Các token là những 'công dân hạng nhất' trên Alephium khi có được những tính năng và hỗ trợ tối ưu nhất. Giống như token ALPH, tất cả token trên Alephium được quản lý bởi UTXOs, và được sở hữu trực tiếp bởi các địa chỉ ví.

Thiết kế này có một số ưu điểm vượt trội hơn các blockchain khác:

- Việc di chuyển token giữa những người dùng chỉ yêu cầu UTXO, và UTXO đã được thử nghiệm trong 'cuộc chiến' về tính bảo mật trong việc quản lý tài sản. 
- Dễ dàng hơn cho các ví lưu trữ và dApp khám phá token của người dùng, bao gồm các token có thể thay thế (fungible) và không thể thay thế (non-fungible).
- Khi smart contract cần di chuyển các token, nó không yêu cầu thêm giao dịch phê duyệt vì việc phê duyệt đã được thiết lập sẵn trong mô hình UTXO. Alephium tận dụng [Asset Permission System](/ralph/asset-permission-system) của nó để đảm bảo rằng các token được xử lý an toàn bởi các smart contract.  
- Việc di chuyển token có khả năng mở rộng rất cao vì chúng có thể tận dụng tối đa thiết kế [Sharding](/glossary.md#sharding) của Alephium. 

Những thông tin sau đây sẽ giúp bạn làm việc dễ dàng với token trên hệ sinh thái của Alephium: 

- [Các tiêu chuẩn Token](https://github.com/alephium/alephium-web3/tree/master/packages/web3/std) được giới thiệu trong SDK để thiết lập giao diện tiêu chuẩn cho cả fungible và non-fungible tokens. 
- Các chức năng tiện ích được cài đặt trong SDK để giảm bớt các tác vụ thông thường cho các dApp và ví lưu trữ khi tương tác với token, chẳng hạn như đoán các loại token và trích xuất siêu dữ liệu token. 
- [Danh sách Token](https://github.com/alephium/token-list) được sử dụng để cung cấp danh sách đáng tin cậy cho các fungible token và các bộ sưu tập NFT có tiếng. 
- Hỗ trợ riêng cho cả fungible và non-fungible token được lưu trữ trong ví và trên trình duyệt web. 
- Công cụ giúp triển khai đợt public sale của NFT theo phong cách [Opensea Drop](https://docs.opensea.io/docs/drops-on-opensea) và nó được gọi `Flow`.

Trên trang [Fungible Tokens](/tokens/fungible-tokens), bạn sẽ tìm hiểu về tiêu chuẩn của fungible token, cách phát hành fungible token, cách tìm kiếm siêu dữ liệu token và cách để di chuyển các fungible token trong ví, v.v...

Trên trang [Non-fungible Tokens](/tokens/non-fungible-tokens), bạn sẽ tìm hiểu về tiêu chuẩn của non-fungible token, cách để tạo bộ sưu tập NFT của riêng bạn và triển khai đợt public sale NFT đầu tiên của bạn có tên là `Flows` trên [NFT marketplace](https://testnet.nft.alephium.org/).
