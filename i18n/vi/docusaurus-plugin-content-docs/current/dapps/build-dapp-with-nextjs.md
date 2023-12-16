---
sidebar_position: 10
title: Xây dựng dApp với Nextjs
sidebar_label: Xây dựng dApp với Nextjs
---

import UntranslatedPageText from "@site/src/components/UntranslatedPageText";

Đây là sự tiếp nối của hướng dẫn [Bắt đầu](/dapps/getting-started.md). Trong hướng dẫn này, bạn sẽ có thể xây dựng một [Nextjs](https://nextjs.org/) dApp đơn giản và có thể tương tác với token faucet smart contract. Xem lại giải thích tại hướng dẫn [Bắt đầu](/dapps/getting-started.md).

Yêu cầu:

- Kiến thức cơ bản về [Typescript](https://www.typescriptlang.org/)
  và [Nextjs](https://nextjs.org/)
- [npm](https://www.npmjs.com/) và
  [npx](https://www.npmjs.com/package/npx) đã được cài đặt
- Làm quen với hướng dẫn token faucet tutorial project tại trang [Bắt đầu](/dapps/getting-started.md).
- Cài đặt [extension wallet](/wallet/extension-wallet/overview)
- Cài đặt docker và docker-compose

## Tạo một project dApp sử dụng temple của Nextjs

```sh
npx @alephium/cli@latest init alephium-nextjs-tutorial --template nextjs
```

Nó sẽ tạo một thư một mới tên là `alephium-nextjs-tutorial` và
chứa một dự án mẫu Nextjs bên trong thư mục.


## Khởi chạy một mạng local

Đi vào thư mục `alephium-nextjs-tutorial/docker` và chạy

```sh
cd alephium-nextjs-tutorial/docker
docker-compose up -d
```

Nó sẽ bắt đầu cả Alephium full node và [explorer
backend](https://github.com/alephium/explorer-backend) on
`devnet`. Explorer backend cần extension wallet để hoạt động.

Bây giờ bạn có thể [compile](/dapps/getting-started.md#compiling-your-contract),
[test](/dapps/getting-started.md#testing-your-contract) and
[deploy](/dapps/getting-started.md#deploying-your-contract) token faucet contract như đã giải thích ở trang [Bắt đầu](/dapps/getting-started.md).

Hãy chắc chắn rằng contract đã được deploy trước khi thực hiện những bước tiếp theo.

```sh
npx @alephium/cli@latest deploy
```

## Tương tác token faucet với the Nextjs dApp

Đi đến thư mục gốc của dự án và chạy

```sh
npm install
npm run dev
```

Mở trang [http://localhost:3000](http://localhost:3000) bằng trình duyệt để thấy ứng dụng token faucet.

<img src={require("./media/nextjs-template-connect.png").default}
alt="Connect button" width="300"/>

Như hình minh hoạ, token facet dApp hiển thị một nút `Connect Alephium` trước khi được kết nối vào ví. Nhấn vào nút đó và chọn `Extension Wallet` để mở extension wallet. `WalletConnect` sẽ sớm được hỗ trợ.

<img src={require("./media/nextjs-template-open-connect.png").default} alt="Landing page" width="250"/>
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/nextjs-template-connect-click-extensonwallet.png").default} alt="Create wallet" width="250" />

Xem lại màn hình xác nhận trên extension wallet và nhấn vào nút
`Connect`, token faucet dApp sẽ được kết nối với extension
wallet. 

<img src={require("./media/nextjs-template-connected.png").default} alt="Landing page" width="520"/>

Điền số token muốn rút ra (tối đa là 2), và nhấn nút `Send Me Token`. Xem lại chi tiết giao dịch và nhấn
`Confirm`.

<img src={require("./media/nextjs-template-send-token.png").default} alt="Landing page" width="520"/>

Chúc mừng, Bạn vừa mới thành công thực hiện một giao dịch di chuyển một vài token từ token faucet đến tài khoản của mình!

## Tích hợp

Mục tiêu của [nextjs
template](https://github.com/alephium/nextjs-template) project là để minh hoạ cách tương tác với blockchain trên Alephium bằng Nextjs.

Xác thực có thể được hoàn thành với một vài dòng lệnh sử dụng
[@alephium/web3-react](https://github.com/alephium/alephium-web3/tree/master/packages/web3-react)
component:

```tsx
<AlephiumWalletProvider>
  <AlephiumConnectButton />
  // Your logic
</AlephiumWalletProvider>
```

`<AlephiumWalletProvider>` tạo một react
[context](https://reactjs.org/docs/context.html) và chuyển nó qua component tree của ứng dụng. Context
sẽ chứa
[SignerProvider](https://github.com/alephium/alephium-web3/blob/8cf20fee4c16091cf581518e9f411e31ec37955e/packages/web3-react/src/contexts/alephiumConnect.tsx#L56)
nơi mà chưa các thông tin cần thiết để tương tác với blockchain của Alephium, chẳn hạn như ký vào giao dịch, v.v.

Sau khi user đã được kết nối vào ví, chúng ta có thể tương tác với blockchain của Alephium bằng cách sử dụng dãy react hook được cung cấp bởi
[@alephium/web3-react](https://github.com/alephium/alephium-web3/tree/master/packages/web3-react). Ví dụ, lấy thông tin [ví đã được kết nối hiện tại](https://github.com/alephium/alephium-web3/blob/master/packages/web3-react/src/hooks/useWallet.tsx),
[số dư](https://github.com/alephium/alephium-web3/blob/master/packages/web3-react/src/hooks/useBalance.tsx)
và [trạng thái của giao dịch](https://github.com/alephium/alephium-web3/blob/master/packages/web3-react/src/hooks/useTxStatus.tsx),
v.v.

Khi một user thực hiện một giao dịch, bạn có thể cập nhật số dư của user bằng `updateBalanceForTx`.
Đây là ví dụ:

```typescript
// The useBalance hook returns two values:
// 1. balance: the current balance
// 2. updateBalanceForTx: used to update the balance when the user makes a transaction.
const { balance, updateBalanceForTx } = useBalance()

const withdrawCallback = useCallback(async () => {
  const result = await withdraw(...)
  updateBalanceForTx(result.txId)
}, [updateBalanceForTx])
```

Thêm thông tin chi tiết về việc tích, xin hãy xem qua
[code](https://github.com/alephium/nextjs-template). 

## Tìm hiểu thêm

- Nextjs template được deploy trên testnet [https://alephium.github.io/nextjs-template](https://alephium.github.io/nextjs-template/)
- Tìm hiểu thêm về hệ sinh thái, truy cập [Tổng quan về hệ sinh thái](/dapps/ecosystem).
- Tìm hiểu thêm về web3 SDK, truy cập [Hướng dẫn web3 SDK](/dapps/alephium-web3).
- Tìm hiểu thêm về ngôn ngữ Ralph, truy cập [Hướng dẫn ngôn ngữ lập trình Ralph](/ralph/getting-started).
