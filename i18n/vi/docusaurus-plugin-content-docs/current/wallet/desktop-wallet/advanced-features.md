---
sidebar_position: 40
title: Tính năng nâng cao
sidebar_label: Tính năng nâng cao
---

# Tính năng nâng cao

## 1. Hợp nhất UTXO

:::info

Do tính chất của UTXO, mỗi khi một giao dịch được thực hiện, một vài Unspent Transaction Outputs được tạo ra, mỗi output chứa số lượng ALPH khác nhau. Nếu thỉnh thoảng các UTXO không được hợp nhất thì một UTXO có thể đạt đến mức được gọi là "dust". Có nghĩa là nếu số lượng trong UTXO nhỏ hơn phí GAS để gửi ALPH mà nó có, thì những ALPH này sẽ không thể được di chuyển nữa. 

Để đảm bảo điều này không xảy ra, ví cho phép bạn dễ dàng hợp nhất các UTXO của mình chỉ bằng một nút bấm. 

:::

Trong mục `Địa chỉ ví`, nhấp vào biểu tượng `Tính năng nâng cao` và chọn `Hợp nhất UTXOs`.

<img src={require("./media/af1.png").default} alt="UTXO consolidation" width="auto" style={{ height: '200px' }} />

<img src={require("./media/af4.png").default} alt="UTXO consolidation" width="auto" style={{ height: '200px' }} />

Chọn địa chỉ ví mà bạn muốn hợp nhất các UTXO và chọn địa chỉ ví nhận (có thể cùng một địa chỉ). Chọn `Hợp nhất` và các UTXO của bạn sẽ được hợp nhất.

<img src={require("./media/af2.png").default} alt="Landing page" width="auto" style={{ height: '200px' }} />

## 2. Cụm từ khôi phục (Tính năng bảo mật nâng cao)

Được giới thiệu trong phiên bản 1.3.0

:::caution
Vui lòng đọc tài liệu sau và [bài viết](https://medium.com/@alephium/bip39-passphrase-implementation-f87adecd6f59) trước khi quyết định sử dụng tính năng này.
:::

### 1. Lưu ý quan trọng

- Cụm từ khôi phục (Passphrase) là một tính năng bảo mật nâng cao, nó bổ sung thêm một từ mà bạn chọn vào Cụm từ khôi phục bí mật (Secret Recovery Phrase) hiện có của mình. 
- Việc sử dụng Cụm từ khôi phục sẽ tạo ra một ví hoàn toàn mới, mà nó không thể được truy cập chỉ bằng Cụm từ khôi phục bí mật. 
- Mật khẩu ví desktop khác với Cụm từ khôi phục. Mật khẩu chỉ được sử dụng trên máy tính của bạn để mã hoá và lưu trữ Cụm từ khôi phục bí mật. Cụm từ khôi phục là một từ bổ sung cho Cụm từ khôi phục bí mật đó và không được lưu trữ trong ví. 
- Ngoài việc bổ sung thêm một lớp bảo mật, Cụm từ khôi phục còn cho bạn sự từ chối hợp lý khi bị cưỡng chế. 
- **Nếu bạn quyết định sử dụng Cụm từ khôi phục, điều quan trọng là bạn phải lưu trữ và sao lưu nó một cách an toàn ở một vị trí thực tế khác với nơi của Cụm từ khôi phục bí mật. Bạn phải nhớ Cụm từ khôi phục của mình một cách hoàn hảo. Việc thay đổi một ký tự đơn lẻ (thậm chí từ chữ thường sang chữ hoa) sẽ dẫn đến việc tạo ra một chiếc ví hoàn toàn mới.**

Giả sử bạn đã tạo ví bằng ví desktop, bạn có một danh sách gồm 24 từ được gọi là Cụm từ khôi phục bí mật. Cụm từ này có thể được sử dụng để khôi phục ví và truy cập vào số dư của bạn. Nếu Cụm từ khôi phục bí mật gồm 24 từ này bị đánh cắp, kẻ tấn công có thể lấy cắp tiền của bạn. Để tăng cường tính bảo mật cho người dùng ví desktop, và để tránh mất tiền do bị đánh cắp Cụm từ khôi phục bí mật, chúng tôi đã triển khai tính năng [BIP39 passphrase](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki#from-mnemonic-to-seed).

Passphrase là từ thứ 25 được tuỳ chọn bổ sung mà bạn có thể tự do lựa chọn cho mình. Nó có thể bao gồm bất kỳ ký tự chữ thường/chữ hoa, số và/hoặc biểu tượng nào và có độ dài tuỳ ý bạn. 

### 2. Cách sử dụng Cụm từ khôi phục

:::warning

Điều quan trọng cần nhớ: bất kỳ cụm từ khôi phục duy nhất nào cũng sẽ tạo và cấp quyền truy cậo vào một ví hoàn toàn mới. Bạn phải lưu trữ và sao lưu Passphrase một cách an toàn ở một vị trí thực tế khác với nơi của Cụm từ khôi phục bí mật. **Bạn phải nhớ rõ cụm từ khôi phục của mình. Việc thay đổi một ký tự (thậm chí từ chữ thường sang chữ hoa) sẽ tạo ra một ví hoàn toàn mới.**

:::

Để sử dụng cụm từ khôi phục, đơn giản chỉ cần chọn `Sử dụng cụm từ khôi phục tuỳ chỉnh (tính năng nâng cao)` và nhập vào cụm từ khôi phục mà bạn chọn.

<img src={require("./media/af5.png").default} alt="Landing page" width="auto" style={{ height: '200px' }} />

### 3. Những hạn chế của ví có sử dụng cụm từ khôi phục 

1. Bạn không (chưa) thể sử dụng nhãn màu cho địa chỉ ví đã tạo của mình. 
2. Mọi địa chỉ ví được tạo bổ sung sẽ cần được tạo lại sau mỗi lần đăng nhậpp. 

Điều này có thể được thay đổi trong tương lai. 
