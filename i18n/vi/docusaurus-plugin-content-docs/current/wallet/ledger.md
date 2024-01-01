---
sidebar_position: 50
title: Ledger
sidebar_label: Ledger
---

![IMG_8932-2](https://github.com/alephium/alephium/assets/88235023/010e915e-0ecd-4f8f-808e-4223202eaecd)

## Hướng dẫn cách cài đặt Alephium app lên trên thiết bị ledger và sử dụng nó để xác thực các giao dịch

🚨 *Lưu ý quan trọng: Phiên bản Alephium App cho các thiết bị Ledger là phiên bản được tùy chỉnh/đóng góp từ cộng đồng, được phát triển lên bởi Alephium. Nó chưa chính thức có sẳn trên Ledger Live! Và nó yêu cầu bạn tải về một phần mềm khác trên máy tính với một số bước tinh chỉnh kỹ thuật. Thực hiện hướng dẫn này nếu bạn chắc chắn hiểu về nó.*

🚨 *Bởi vì đây là một phiên bản Alpha, chúng tôi khuyến cáo sử dụng một thiết bị ledger mới hoàn toàn và chưa có bất kỳ coin nào được lưu trữ trên nó.*

🚨 *Ứng dụng Ledger chỉ có thể hoạt động với phiên bản mới nhất (từ v0.7.0) của phiên bản extension wallet cho đến thời điểm hiện tại.*

### Video hướng dẫn
Đây là video hướng dẫn mà bạn nên tham khảo  qua: https://www.youtube.com/watch?v=YBQy_siZh6w

### Tóm tắt các bước

**1 — Tải về phiên bản wallet từ**: Chrome — [Extension Wallet](https://chrome.google.com/webstore/detail/alephium-extension-wallet/gdokollfhmnbfckbobkdbakhilldkhcj) / Firefox — [Extension Wallet](https://addons.mozilla.org/en-US/firefox/addon/alephiumextensionwallet/)


**2 — Cài đặt các phần mềm cần thiết** (nếu máy của bạn đã cài PIP & Python, hãy đi tới bước 3)

Bạn sẽ phải cần Python và PIP được cài đặt trên máy tính để có thể sử dụng Alephium App trên Ledger:

* Python ([hướng dẫn trên Windows](https://www.simplilearn.com/tutorials/python-tutorial/python-installation-on-windows#:~:text=To%20download%20Python%2C%20you%20need,then%20select%20the%20Windows%20option.), [hướng dẫn trên Mac](https://docs.python.org/3/using/mac.html), [hướng dẫn trên Linux](https://docs.python-guide.org/starting/install3/linux/))
* PIP ([hướng dẫn trên Windows](https://www.dataquest.io/blog/install-pip-windows/), [hướng dẫn trên Mac](https://www.groovypost.com/howto/install-pip-on-a-mac/), [hướng dẫn trên Linux](https://docs.python-guide.org/starting/install3/linux/))


**3 — Cài đặt Ledger Python Library**

![image](https://github.com/alephium/docs/assets/88235023/fade8c08-f3a1-41b2-b7e9-9a3cd638a683)

Chúng ta sẽ sử dụng Ledger Python Library (bạn  có thể tìm hiểu thêm tại đây). Điều này là cần thiết bởi vì bạn sẽ cài đặt một App tùy chỉnh trên thiết bị Ledger của mình.

Để cài đặt, mở terminal và gõ:

**pip3 install — upgrade protobuf setuptools ecdsa**

**pip3 install ledgerwallet**

Nó sẽ cập nhật tất cả và cài đặt Ledger Wallet Library.
![Cài đặt Ledger App](https://github.com/alephium/docs/assets/88235023/f3f096e3-fb9b-4a8c-9a98-a060112b0f5f)

**4 — Tải về Alephium Ledger App**

Vào trang repo trên Github: https://github.com/alephium/ledger-alephium và tải về.

🚨*Tải tải về repository, nhấn vào nút “Code” và chọn “Download Zip.”*

![image](https://github.com/alephium/docs/assets/88235023/f699b669-1b00-4b2e-9649-5cedd221e0cb)

Tải về và giải nén nó trong một folder sẽ giúp bạn thao tác dễ dàng hơn để truy cập và thuận tiện hơn với read/write permission.

**5 — Cài đặt Alephium App trên thiết bị Ledger**

Thiết bị Ledger của bạn bây giờ sẽ cần được kết nối với máy tính và phải được mở khóa.

Đi đến repo trên Github (https://github.com/alephium/ledger-alephium/tree/master) và kéo xuống bên dưới để tìm câu lệnh thực thi cho phiên bản  Ledger:

![image](https://github.com/alephium/docs/assets/88235023/6c5df18d-c59f-4ae4-ad8c-3e7bceb65014)

Với thông tin trên, quay trở lại terminal và chạy câu lệnh để cài đặt Alephium App:

🚨 *Lưu ý quan trọng: Bạn cần phải chạy câu lệnh bên trong thư mục mà bạn đã tải các tệp trước đó từ Github.*

Trong ví dụ này, chúng tôi sử dụng một Ledger Nano S:

![image](https://github.com/alephium/docs/assets/88235023/d92896ef-5f9b-43a6-8f53-ab56f38c1700)

Sau khi chạy câu lệnh, bạn sẽ cần xác thự việc cài đặt của Alephium App trên thiết bị Ledger. Hãy cho phép tất cả và nhận pin để xác nhận quá trình cài đặt.

Khi thành công, một icon của Alephium sẽ xuất hiện trên thiết bị của bạn.

![image](https://github.com/alephium/docs/assets/88235023/7c41b2d3-ea5a-44ca-bd05-46338cf3274c)

Bây giờ bạn đã có thể sử dụng thiết bị Ledger này để ký xác thực cho các giao dịch trên Alephium! 🎉

**6 — Sử dụng Ledger với Extension Wallet**

Đi đến trình duyệt mà bạn đã cài phiên bản extension wallet và mở nó lên.

🚨 *Ưng dụng Ledger chỉ có thể hoạt động với phiên bản mới nhất (từ v0.7.0) của extension wallet. Nếu bạn chưa có, hãy cài đặt tại [đây (Chrome)](https://chrome.google.com/webstore/detail/alephium-extension-wallet/gdokollfhmnbfckbobkdbakhilldkhcj/related) hoặc [đây (Firefox)](https://addons.mozilla.org/en-US/firefox/addon/alephiumextensionwallet/).*

Tạo một địa chỉ trên extension wallet: Nhấn vào tên địa chỉ hiện tại sau đó nhấn vào nút "+". Nó sẽ dẫn bạn đế trang Ledger Connection:

* Cắm thiết bị Ledger của bạn vào và mở khóa nó;
* Mở Alephium App lên; (hãy chắc rằng bạn đã làm đúng các bước bên trên!)
* Chọn thiết bị Ledger của bạn trong danh sách;
* Kết thúc quá trình tùy chỉnh.

![install new wallet](https://github.com/alephium/alephium/assets/88235023/5fa7e000-2f77-4b44-9dfa-13b784e05eba)

**7 — Sử dụng thiết bị Ledger để thực hiện một giao dịch!**

Tất cả các bước tại đây là cơ bản trong quá trình sử dụng trên extension wallet:

* Nhấn vào nút "Send"

![image](https://github.com/alephium/docs/assets/88235023/17eaf25a-5629-48cb-bee7-996513e9a7b4)

* Chọn token bạn muốn gửi đi:

![image](https://github.com/alephium/docs/assets/88235023/60a3ed3b-04f7-447a-9472-886147d2b5d4)

* Chọn địa chỉ người nhận:

![image](https://github.com/alephium/docs/assets/88235023/b6b7aae2-4c9e-4048-934e-95caa93bf577)

* Xem lại thông tin chi tiết giao dịchh và nhấn vào nút “Sign with Ledger.”

![image](https://github.com/alephium/docs/assets/88235023/fde7b7c2-b864-468e-bb3f-66448fe8a4d2)

* Kỹ xác nhận giao dịch trên thiết bị Ledger và bạn có thể theo dõi quá trình giao dịch trên mục "Activity":

![image](https://github.com/alephium/docs/assets/88235023/efffc0de-01f8-48d7-a67c-ed1487c95483)

** 8 — Sử dụng thiết bị Ledger để tương tác với dApp trên Alephium** 

Bây giờ bạn đã có thể ký xác nhận một giao dịch trên thiết bị Ledger của mình, hãy thử qua chức năng kết nối đến một dApp. Các bước thực hiện đơn giản như sau.

Truy cập vào [Alephium DEX trên Testnet](https://alephium.github.io/alephium-dex). Nhấn vào nút “Connect Alephium” ở góc trên bên phải. Chọn extension wallet và tài khoản trên Ledger.

![connect with dex](https://github.com/alephium/alephium/assets/88235023/f3e6cf9e-e632-4bc0-84a8-67f38d067311)

Giờ đây, bạn đã kết nối với Alephium DEX. Tạo một giao dịch swap và sử dụng thiết bị Ledger để ký xác nhận. Các bước thực hiện giống như lúc thực hiện gửi một giao dịch thông thường.

![unnamed](https://github.com/alephium/alephium/assets/88235023/bb263f71-3801-4be3-86cd-d7a18b525e0a)

Nếu  bạn có câu hỏi hoặc đề xuất, hãy tham gia trên [Alephium’s Discord](http://alephium.org/discord), [Telegram](https://t.me/alephiumgroup), hoặc trên [Twitter](https://twitter.com/alephium)!
