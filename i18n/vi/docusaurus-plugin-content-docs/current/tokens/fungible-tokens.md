---
sidebar_position: 20
title: Fungible Tokens - Token có thể thay thế
sidebar_label: Fungible Tokens - Token có thể thay thế
---

### Tiêu chuẩn của Fungible Token 

Trên Alephium, các token mới có thể được phát hành khi triển khai các contract mới. Id của token mới được phát hành giống với Id của contract phát hành nó. Bạn có thể tham khảo hướng dần [tại đây](/dapps/build-dapp-from-scratch) để biết thêm chi tiết về cách phát hành token trên Alephium từ đầu.

Các token thường được liên kết với các thông tin như `tên`, `số thập phân`, `tổng cung`, etc. Mục đích của việc đưa ra tiêu chuẩn cho token là thiết lập các ràng buộc đối với hợp đồng phát hành token, để dApps và ví lưu trữ dễ dàng truy cứu các loại token và lấy thông tin từ đó.

Tiêu chuẩn [giao diện fungible token](https://github.com/alephium/alephium-web3/blob/master/packages/web3/std/fungible_token_interface.ral) xác định phương thức để thu thập `tên`, `ký tự`, `số thập phân`, cũng như là `tổng cung` của token. Nó cũng được ghi chú bằng chú thích `@std` với id `#0001`:

```rust
// Standard interface for fungible tokens
@std(id = #0001)
Interface IFungibleToken {
  pub fn getSymbol() -> ByteVec
  pub fn getName() -> ByteVec
  pub fn getDecimals() -> U256
  pub fn getTotalSupply() -> U256
}

// A `TokenFaucet` contract that implements the `IFungibleToken` interface
Contract TokenFaucet(
    symbol: ByteVec,
    name: ByteVec,
    decimals: U256,
    supply: U256
) implements IFungibleToken {
    pub fn getTotalSupply() -> U256 {
        return supply
    }

    pub fn getSymbol() -> ByteVec {
        return symbol
    }

    pub fn getName() -> ByteVec {
        return name
    }

    pub fn getDecimals() -> U256 {
        return decimals
    }
}
```

Sau khi một token contract triển khai giao diện [IFungibleToken](https://github.com/alephium/alephium-web3/blob/master/packages/web3/std/fungible_token_interface.ral), nó sẽ cho phép SDK lấy thông tin theo cách tiêu chuẩn, giống như hợp đồng `TokenFaucet` được đề cập ở trên:

```typescript
// Use SDK to call methods individually
const getDecimalResult = await tokenFaucet.methods.getDecimals()
const getTotalSupplyResult = await tokenFaucet.methods.getTotalSupply()
const getNameResult = await tokenFaucet.methods.getName()
console.log("TokenFaucet name, decimals, totalSupply", getNameResult.returns, getDecimalResult.returns, getTotalSupplyResult.returns)

// Use SDK to call all multiple methods at the same time
const multicallResult = await tokenFaucet.multicall({
  getDecimals: {},
  getTotalSupply: {},
  getName: {},
})
console.log("TokenFaucet name, decimals, totalSupply", multicallResult.getName.returns, multicallResult.getDecimal.returns, multicallResult.getTotalSupply.returns)
```

Trên thực tế, SDK đưa ra một phương pháp chuẩn để tìm nạp tất cả siêu dữ liệu cho fungible token.

```typescript
const metadata = await web3.getCurrentNodeProvider().fetchFungibleTokenMetaData(tokenFaucet.contractId)
console.log("TokenFaucet name, decimals, totalSupply", metadata.name, metadata.decimals, metadata.totalSupply)
```

[IFungibleToken](https://github.com/alephium/alephium-web3/blob/master/packages/web3/std/fungible_token_interface.ral)
cũng cho phép SDK đoán được loại token, nên dApps và ví lưu trữ có thể xử lý chúng một cách phù hợp:

```typescript
// Guess token type
const tokenType = await web3.getCurrentNodeProvider().guessStdTokenType(tokenFaucet.contractId)
expect(tokenType).toEqual('fungible')

// Guess token interface id
const tokenInterfaceId = await web3.getCurrentNodeProvider().guessStdInterfaceId(tokenFaucet.contractId)
expect(tokenInterfaceId).toEqual('0001')
```

Để có ví dụ đầy đủ hơn, vui lòng tham khảo repository [nextjs-template](https://github.com/alephium/nextjs-template).

### Hỗ trợ ví

Cả [Ví Desktop](/wallet/desktop-wallet/overview) và [Ví Mở rộng](/wallet/extension-wallet/overview) có hỗ trợ riêng cho fungible token.

Sau đây là ví dụ về cách hiển thị và di chuyển token `PACA` bằng ví mở rộng:

<img src={require("./media/transfer-alphpaca-1.png").default} alt="Token Overview" width="250"/>
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/transfer-alphpaca-2.png").default} alt="Send Token" width="250" />
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/transfer-alphpaca-3.png").default} alt="Sign Tx" width="250" />

### Danh sách Token

Ngoài những thông tin cơ bản như `tên`, `ký tự` và `số thập phân`, v,v. Fungible token thường chứa siêu dữ liệu khác như `mô tả` và `logoURI` để dApp và ví có thể hiển thị chúng một cách chính xác. 

Mục đích của [danh sách token](https://github.com/alephium/token-list) là trở thành nguồn tin cậy cho token id và siêu dữ liệu của những token có tiếng trong hệ sinh thái Alephium, vì thế ví và dApp có thể cảnh báo người dùng với những token chưa được xác minh. Dưới đây là cách ví mở rộng hiển thị token trước và sau khi được thêm vào danh sách token. 

<img src={require("./media/unverified-token.png").default} alt="Unverified" width="250"/>
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/verified-token.png").default} alt="Verified" width="250"/>

Hiện tại, cần có pull request để thêm siêu dữ liệu của token vào danh sách token. 
