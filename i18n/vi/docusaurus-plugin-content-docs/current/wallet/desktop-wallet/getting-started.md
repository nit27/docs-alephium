---
sidebar_position: 20
title: Bắt đầu
sidebar_label: Bắt đầu
---

# Bắt đầu

## 1. Cài đặt

1. Download file phù hợp với hệ điều hành của bạn (macOS, Windows, Linux) với [bản cập nhật mới nhất](https://github.com/alephium/desktop-wallet/releases/latest) và double click để cài đặt ứng dụng.
2. Trên Linux và Windows, double click ứng dụng để mở ví desktop. Trên macOS, bạn cần phải đi tới Application folder, nhấp chuột phải ứng dụng Alephium rồi nhấp vào _Open_ (đừng double click, bạn sẽ không mở được trừ khi bạn cài đặt như vậy từ System Preferences).

## 2. Tạo ví 

:::info

Theo mặc định, ví sẽ được kết nối với public node của Alephium. Nếu bạn muốn sử dụng node cá nhân hoặc tạo ví offline, bạn có thể tạo bằng cách nhấp vào bánh răng cài đặt ở trên cùng góc phải của ứng dụng và thay đổi node host thành localhost của bạn hoặc chỉ cần để trống để tạo ví offline. 

:::

<img src={require("./media/gs1.png").default} alt="Desktop Wallet Landing page" width="auto" style={{ height: '200px' }} />

<img src={require("./media/gs2.png").default} alt="Prompt to create new wallet" width="auto" style={{ height: '200px' }} />

<img src={require("./media/gs3.png").default} alt="Prompt to chose a name and set a password" width="auto" style={{ height: '200px' }} />

1. Nhấp chuột vào `Create/Import a new wallet`, rồi chọn `New wallet`.

2. Chọn tên ví và mật khẩu để bảo vệ ví của bạn trên máy tính. Mật khẩu này không thay thế cho Cụm từ khôi phục bí mật (Secret Recovery Phrase) gồm 24 từ cho ví của bạn. Nó chỉ được dùng để khoá và mở khoá ví mới được tạo. Nhấp vào `Continue`.

<img src={require("./media/gs4.png").default} alt="Display of the 24 secret words" width="auto" style={{ height: '200px' }} />

<img src={require("./media/gs5.png").default} alt="Prompt to verify annotation of the 24 secret words was correct" width="auto" style={{ height: '200px' }} />

<img src={require("./media/gs6.png").default} alt="Landing page of a brand new wallet" width="auto" style={{ height: '200px' }} />

3. Trong khung được highlight, bạn sẽ thấy 24 từ. Đây là Cụm từ khôi phục bí mật gồm 24 từ cho ví của bạn. Đây là thông tin cực kì quan trọng và quý giá nhất của bạn, bạn phải lưu trữ nó một cách khôn ngoan, an toàn và cẩn thận. 

4. Tiếp tục, bạn sẽ được yêu cầu xác minh rằng bạn đã hiểu đúng Cụm từ khôi phục bí mật gồm 24 từ. Nhấp vào `Ready` và chọn các từ theo đúng thứ tự. Nếu bạn chọn chính xác, các từ sẽ chuyển sang màu xanh. Ngược lại, nó sẽ chuyển sang màu đỏ: nhưng không sao, bạn có thể sắp xếp lại các từ cho đến khi bạn chọn đúng. 

5. Mọi thứ đẫ sẵn sàng! chào mừng đến ví lưu trữ mới của bạn.

## 3. Di chuyển ALPH

<img src={require("./media/gs7.png").default} alt="Landing page of Desktop Wallet" width="auto" style={{ height: '200px' }} />

<img src={require("./media/gs8.png").default} alt="Prompt to enter destination address and amount of transaction" width="auto" style={{ height: '200px' }} />

<img src={require("./media/gs9.png").default} alt="Prompt to enter destination address and amount of transaction" width="auto" style={{ height: '200px' }} />

1. Để gửi ALPH, đơn giản chỉ cần nhấp nút `Send` trong danh mục bên trái. 

2. Nhập vào số luợng ALPH (vd: 100) và địa chỉ nguời nhận.

3. (Không bắt buộc) Bạn có thể đặt thời gian khoá (lock time) bằng cách tick vào ô tương ứng. Trong trường hợp đó, ALPH sẽ được gửi đến địa chỉ người nhận nhưng sẽ bị khoá on-chain cho đến ngày bạn đã chỉ định. Lưu ý rằng, bạn không thể thay đổi thời gian khoá sau khi giao dịch đã được gửi. 

4. Chọn `Check` và kiểm tra cẩn thận chi tiết giao dịch của bạn. Khi bạn nhấp vào `Send`, ALPH sẽ được chuyển đến người nhận. 

:::info

Giao dịch có thể được tìm thấy trong mục `Overview`, cũng như mục `Addresses` khi nhấp vào địa chỉ cụ thể.

:::

## 4. Quản lý các địa chỉ

Bạn có thể xem tất cả các địa chỉ hiện có được hiển thị trong danh sách ở mục `Addresses` với các thông tin như nhãn mác, số dư, số nhóm, v.v.

<img src={require("./media/gs10.png").default} alt="Address page" width="auto" style={{ height: '200px' }} />

Chọn một trong các nút `+ New address` sẽ chuyển hướng bạn đến trang `New address`, nơi mà bạn có thể tạo và gắn nhãn địa chỉ mởi. Theo mặc định, địa chỉ được tạo theo nhóm ngẫu nhiên. Bạn có thể chọn một nhóm cụ thể theo cách thủ công trong phần `Advanced options`.

<img src={require("./media/gs11.png").default} alt="Prompt to create a new address" width="auto" style={{ height: '200px' }} />

Khi nhấo vào một địa chỉ cụ thể, bạn sẽ thấy chi tiết địa chỉ bao gồm lịch sử giao dịch của địa chỉ đó. 

<img src={require("./media/gs12.png").default} alt="View of a singel address" width="auto" style={{ height: '200px' }} />

Bạn có thể chỉnh sửa nhãn địa chỉ, chọn địa chỉ đó làm địa chỉ mặc định, hoặc chuyển hết tất cả số dư không bị khoá đến địa chỉ bạn chọn trong phần Cài đặt ở đầu ứng dụng. 

<img src={require("./media/gs13.png").default} alt="Prompt to configure address options" width="auto" style={{ height: '200px' }} />
