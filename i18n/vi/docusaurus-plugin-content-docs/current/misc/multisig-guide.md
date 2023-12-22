---
sidebar_position: 30
title: Hướng dẫn Multisig 
sidebar_label: Hướng dẫn Multisig 
---

Alephium hỗ trợ `m-of-n` các địa chỉ multi-signature.

Bạn có thể tìm thấy lệnh liên quan dành cho multisig tại [http://127.0.0.1:12973/docs](http://127.0.0.1:12973/docs) trong mục `Multi-signature`. Hãy đảm bảo rằng full node của bạn đang chạy để bạn có thể truy cập Swagger UI. 

## Cách tạo địa chỉ Multisig

1. Thu thập các public key của các tài khoản cho multisig đó.

   Public key có thể được lấy từ ví bằng lệnh:

   ```
   GET /wallets/{wallet_name}/addresses/{address}
   ```

   Phản hồi:

   ```json
   {
     "address": "{address}",
     "publicKey": "d1b70d2226308b46da297486adb6b4f1a8c1842cb159ac5ec04f384fe2d6f5da28"
   }
   ```

2. Ví dụ, nếu bạn muốn tạo một tài khoản multisig với 3 tài khoản cần 2 chữ ký để mở khoá (2 trên 3), bạn có thể làm như sau:

   ```
   POST /multisig/address
   {
     "keys": [
       "d1b70d2226308b46da297486adb6b4f1a8c1842cb159ac5ec04f384fe2d6f5da28",
       "8c1842cb159ac5ec04f384fe2d6f5da2d1b70d2226308b46da297486adb6b41a8f",
       "e4a8c1842cb159ac5ec0b70d2226308b46da297486adb6b4f14f384fe2d6f5da31"
     ],
     "mrequired": 2
   }
   ```

   Phản hồi:

   ```json
   {
     "address": "1AujpupFP4KWeZvqA7itsHY9cLJmx4qTzojVZrg8W9y9n"
   }
   ```

   > ⚠️ CẢNH BÁO: Hãy chắc chắn rằng bạn đã ghi nhớ thứ tự của các public key, sau này bạn sẽ phải cung cấp thứ tự y như vậy. 
   Số dư hiện tại có thể được gửi đến địa chỉ đó.

3. Để sử dụng số dư, bạn cần thiết lập một giao dịch
multisig.   
   Chuyển các public key sẽ ký giao dịch, 2 trong các ví dụ của chúng tôi.  
   Hãy đảm bảo rằng chúng có thứ tự giống trong quá trình tạo địa chỉ:

   ```
   POST /multisig/build
   {
     "fromAddress": "1AujpupFP4KWeZvqA7itsHY9cLJmx4qTzojVZrg8W9y9n",
     "fromPublicKeys": [
       "d1b70d2226308b46da297486adb6b4f1a8c1842cb159ac5ec04f384fe2d6f5da28",
       "e4a8c1842cb159ac5ec0b70d2226308b46da297486adb6b4f14f384fe2d6f5da31"
     ],
     "destinations": [
       {
         "address": "1jVZrg8W9y9AujpupFP4KWeZvqA7itsHY9cLJmTonx4zq",
         "amount": "42 ALPH"
       }
     ]
   }
   ```

   Phản hồi:

   ```json
   {
     "unsignedTx": "0ecd20654c2e2be708495853e8da35c664247040c00bd10b9b13",
     "txId": "798e9e137aec7c2d59d9655b4ffa640f301f628bf7c365083bb255f6aa5f89ef",
     "fromGroup": 2,
     "toGroup": 1
   }
   ```

4. Hiện tại bạn có thể gửi `txId` cho những người cần ký giao dịch. Mọi người có thể ký bằng ví của mình:

   ```
   POST /wallets/{wallet_name}/sign
   {
     "data": "798e9e137aec7c2d59d9655b4ffa640f301f628bf7c365083bb255f6aa5f89ef"
   }
   ```

   Phản hồi:

   ```json
   {
     "signature": "9e1a35b2931bd04e6780d01c36e3e5337941aa80f173cfe4f4e249c44ab135272b834c1a639db9c89d673a8a30524042b0469672ca845458a5a0cf2cad53221b"
   }
   ```

5. Thu thập các chữ ký, ví dụ như 2 (vì `m=2`) và cuối cùng gửi đi giao địch:

   > LƯU Ý: Thứ tự của các chữ ký cần phải giống với thứ tự của các public key.
   ```
   POST /multisig/submit
   {
      "unsignedTx": "0ecd20654c2e2be708495853e8da35c664247040c00bd10b9b13",
      "signatures": [
      "9e1a35b2931bd04e6780d01c36e3e5337941aa80f173cfe4f4e249c44ab135272b834c1a639db9c89d673a8a30524042b0469672ca845458a5a0cf2cad53221b",
      "ab135272b834c1a639db9c89d673a8a30524042b0469672ca845458a5a0cf2cad53221b9e1a35b2931bd04e6780d01c36e3e5337941aa80f173cfe4f4e249c44"
      ]
   }

   ```

   Phản hồi:

   ```json
   {
     "txId": "503bfb16230888af4924aa8f8250d7d348b862e267d75d3147f1998050b6da69",
     "fromGroup": 2,
     "toGroup": 1
   }
   ```
