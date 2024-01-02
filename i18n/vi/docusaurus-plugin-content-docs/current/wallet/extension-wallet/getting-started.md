---
sidebar_position: 20
title: Bắt đầu
sidebar_label: Bắt đầu
---

### Cài đặt 

Ví mở rộng của Alephoium đã có mặt trên [Chrome](https://chrome.google.com/webstore/detail/alephium-extension-wallet/gdokollfhmnbfckbobkdbakhilldkhcj) và [Firefox](https://addons.mozilla.org/en-US/firefox/addon/alephiumextensionwallet/).

### Tạo ví

<img src={require("./media/new-wallet-1.png").default} alt="Create wallet"/>

1. Chọn biểu tượng Ví mở rộng Alephium trên trình duyệt của bạn.
2. Trên trang hiển thị, nhấn nút `Ví mới`. 
3. Nhập mật khẩu để bảo vệ ví của bạn. 
4. Hoàn tất! Ví của bạn đã được tạo. 

Hiện tại ví của bạn không có bất kỳ tài sản nào. Trong buớc tiếp theo, hãy chuyển một số ALPH sang ví này. 

### Di chuyển tài sản

#### ALPH Token Faucet

Đối với `devnet` và `testnet`, ví mở rộng có tích hợp `ALPH` token faucet. Bạn có thể nhận mỗi lần lần lượt `1000` `ALPH` cho `devnet` và `12` `ALPH` cho `testnet`.

<img src={require("./media/token-faucet-1.png").default} alt="Receive Fund" width="250"/>
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/token-faucet-2.png").default} alt="Token Faucet" width="250" />
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/token-faucet-3.png").default} alt="Token Received" width="250" />

#### Di chuyển ALPH

Bạn cũng có thể di chuyển `ALPH` từ tài khoản của mình sang tài khoản khác:

<img src={require("./media/transfer-alph-1.png").default} alt="Overview page" width="250"/>
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/transfer-alph-2.png").default} alt="Transfer page" width="250" />
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/transfer-alph-3.png").default} alt="Review transfer" width="250" />

Sau khi ký giao dịch chuyển khoản, bạn có thể theo dõi trạng thái giao dịch từ mục `Activity`. Ngay sau khi giao dịch được xác nhận, số lượng `ALPH` được chuyển sẽ hiển thị trên tài khoản nhận.

<img src={require("./media/received-alph-1.png").default} alt="Pending Tx" width="250"/>
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/received-alph-2.png").default} alt="Confirmed Tx" width="250" />
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/received-alph-3.png").default} alt="ALPH Received" width="250" />

Chỉ đơn giản vậy thôi! Bạn đã chuyển thành công một số `ALPH`.

#### Di chuyển Tokens

Quá trình chuyển fungible token về cơ bản giống như di chuyển token `ALPH`:

<img src={require("./media/transfer-alphpaca-1.png").default} alt="Transfer Token 1" width="250"/>
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/transfer-alphpaca-2.png").default} alt="Transfer Token 2" width="250" />
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/transfer-alphpaca-3.png").default} alt="Transfer Token 3" width="250" />

Vui lòng đọc huớng dẫn [Fungible Tokens](/tokens/fungible-tokens) để biết thêm thông tin về fungible token.

### Hỗ trợ NFT 

Ví mở rộng Alephium cũng hỗ trợ hiển thị và di chuyển NFT như bên dưới đây:

<img src={require("./media/display-nft-collections.png").default} alt="Display Collections" width="250"/>
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/display-nft-collection.png").default} alt="Display Collection" width="250" />
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/transfer-nft.png").default} alt="Transfer NFT" width="250" />

Vui lòng tham khảo hướng dẫn [Non-fungible Tokens (NFTs)](/tokens/non-fungible-tokens) để biết thêm thông tin về NFT.

### Quản lý tài khoản 

Tài khoản trong ví mở rộng Alephium đại diện cho nơi lưu trữ kỹ thuật số chứa các địa chỉ ví công khai và private key tương ứng, nó cho phép người dùng nhận, lưu trữ và di chuyển tài sản trên blockchain Alephium. 

Ví mở rộng Alephium cho phép người dùng quản lý nhiều tài khoản cùng một lúc. Ví dụ, Alice có thể có một tài khoản cho `Tiền lương`, một tài khoản cho `Tiết kiệm` và một tài khoarn khác cho `Du lịch 2023`. 

#### Tạo tài khoản 
Để tạo thêm một tài khoản nữa, vui lòng làm theo các bước sau:

<img src={require("./media/manage-accounts-1.png").default} alt="Overview" width="250"/>
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/manage-accounts-2.png").default} alt="Account List" width="250"/>
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/manage-accounts-3.png").default} alt="Add Account" width="250"/>

Có một số tuỳ chọn mà chúng ta có thể cân nhắc trước khi tạo tài khoản:

- `Nhóm`: Các tuỳ chọn có sẵn là `0`, `1`, `2` or `3`. Nếu `số bất kỳ` được chọn, nhóm chứa tài khoản được tạo sẽ là ngẫu nhiên. 
- `Sign`: Các tuỳ chọn có sẵn là `mặc định` và `schnorr`. `Mặc định` đại diện cho loại chữ ký Alephium mặc định đuợc sử dụng để (ví dụ) ký tên các giao dịch Alephium; trong khi `schnorr` đại diện cho loại chữ ký `BIP340` Schnorr, nó rất hữu ích khi tương tác với các protocol như [Nostr](https://nostr.com/)
- `Loại tài khoản`: Các tuỳ chọn có sẵn là `Tài khoản Alephium` hoặc là `Tài khoản Ledger`. Để biết thêm chi tiết về Ledger, vui lòng tham khảo hướng dẫn [Ledger](/wallet/ledger).

Sau khi chọn các tuỳ chọn, một tài khoản mới sẽ được tạo và sẵn sàng để sử dụng.

#### Chỉnh sửa tài khoản 

Bạn có thể chỉnh sửa tên của tài khoản hiện có, xuất private key, xoá tài khoản, v.v.:

<img src={require("./media/manage-accounts-4.png").default} alt="Account List" width="250"/>
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/manage-accounts-5.png").default} alt="Edit Account" width="250"/>
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/manage-accounts-6.png").default} alt="Export Private Key" width="250"/>
