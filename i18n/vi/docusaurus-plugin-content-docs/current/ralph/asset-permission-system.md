---
sidebar_position: 60
title: Asset Permission System (APS)
sidebar_label: Asset permission system (APS)
---

The Asset Permission System (APS) là một trong những chức năng độc đáo của Ralph. 
Nó quy định rõ ràng các flow của các tài sản ở cấp độ code,
hỗ trợ cho các lập trình viên và user của smart contract có thể dễ dàng di chuyển tài sản như mong muốn. 
Với những UTXOs model,
nó cũng hỗ trợ một trải nghiệm người dùng bảo mật và thân thiện hơn bằng 
việc loại bỏ đi các rủi ro của sự xác thực token không mong muốn trên toàn hệ thống, giống như EVM.

Alephium sử dụng
[sUTXO](https://medium.com/@alephium/an-introduction-to-the-stateful-utxo-model-8de3b0f76749)
model giúp các asset, bao gồm ALPH và các token khác 
được quản lý bởi các UTXOs cũng như các smart contract và các state của  nó được quản lý bởi 
việc sử dụng account-based model.

Điều này có nghĩa là:

1. Những giao dịch di chuyển tài  sản giữa các user chỉ yêu cầu các UTXOs mà
   đã được thử nghiệm một cách nghiêm ngặc cho tính năng bảo mật trong việc quản lý các tài sản. Lúc này, sẽ không liên quan gì đến việc sử dụng các 
   contract.
2. Khi các smart contract cần được dùng để di chuyển các tài sản thay cho các
   chủ sở hữu, không có giao dịch chấp thuận riêng biệt nào được yêu cầu. 
   Việc chấp thuận được thực hiện ngầm trong UTXO model: nếu giao dịch đó có chứa 
   một token cụ thể được xác thực để sử dụng cho giao dịch, 
   lúc này chủ sở hữu đã đồng ý cho việc sử dụng token đó
   trong trường hợp này, nghĩa là các smart
   contract mà được thu thập trên cùng một giao dịch
   có thể có khả năng di chuyển được các token.

Lúc này câu hỏi đặt ra là: ở một tình huống khác, làm sao để chúng ta biết rằng
các tài sản đã được chấp thuận trong một giao dịch đã sử dụng UTXO
model mà nó có thể đảm bảo tính bảo mật bởi các smart contract? Câu trả lời đó là 
Ralph's Asset Permission System (APS).

## Flow of Asset

Để tương tác với các the smart contract trong Alephium, một giao dịch cần 
thực thi một `TxScript`. Bên dưới là ví dụ về một giao dịch được thực hiện, 
có hai input, một fixed output và một `TxScript`:

```
                  ----------------
                  |              |
                  |              |
   1 Token A      |              |   1 ALPH (fixed output)
================> |              | ========================>
   6.1 ALPHs      |  <TxScript>  |   ??? (generated output)
================> |              | ========================>
                  |              | 
                  |              | 
                  |              |
                  ----------------
```

Có hai điều cần lưu ý ở đây:

1. Dù cho chỉ có một fixed output, sẽ có nhiều các
   outputs được tạo cho giao dịch này. Các generated outputs
   phụ thuộc vào kết quả của việc thực thi `TxScript`.
2. Tổng tài sản khả dụng cho `TxScript` (bao gồm các smart
   contract mà nó invoke) là `5.1` ALPHs và `1` Token A, bởi vì chúng ta cần subtract `1` ALPH trong fixed output.

Một `TxScript` có thể trông như thế này:

```rust
TxScript ListNFT(
    tokenAId: ByteVec,
    price: U256,
    marketPlace: NFTMarketPlace
) {
    let listingFee = marketPlace.getListingFee()
    let minimalAlphInContract = 1 alph
    let approvedAlphAmount = listingFee + minimalAlphInContract

    marketPlace.listNFT{callerAddress!() -> ALPH: approvedAlphAmount, tokenAId: 1}(tokenAId, price)
}
```

Bạn có thể đoán rằng, Token A là một NFT token và mục đích của
 `TxScript` bên trên là để list nó lên một marketplace smart contract.

Đoạn code dưới đây cần được lưu ý:

```rust
marketPlace.listNFT{callerAddress!() -> ALPH: approvedAlphAmount, tokenAId: 1}(tokenAId, price)
```

Đoạn code bên trong dấu ngoặc nhọn đã xác nhận rõ ràng rằng 
`approvedAlphAmount` ALPH và `1` token A được cho phép để gửi vào trong `marketPlace.listNFT` function, thậm chí tổng số lượng các
asset khả dụng cho `TxScript` là `5.1` và `1` cho ALPH và token A
tương ứng.

Tình huống sau đây có thể sẽ xãy ra:

1. Nếu `approvedAlphAmount` là nhiều hơn `5.1` ALPH, thì
   giao dịch sẽ thất bại với lỗi `NotEnoughBalance`.
2. Nếu `approvedAlphAmount` là ít hơn `5.1` ALPH, ví dụ là `1.1` ALPH,
   thì tổng số lượng các asset mà `marketPlace.listNFT` có thể giữ là
    `1.1` ALPHs và `1` token A. `marketPlace.listNFT` không có quyền truy cập cho `4` ALPH còn lại.
3. Nếu `marketPlace.listNFT` không sử dụng toàn bộ các 
   asset đã được chấp thuận, số lượng các asset còn lại sẽ được trả về chủ sở hữu của nó
   khi `marketPlace.listNFT` trả kết quả.

Hãy nhìn kỹ hơn vào `marketPlace.listNFT` function:

```rust
Contract NFTMarketPlace(
    nftListingTemplateId: ByteVec
) {
    // Other code are omitted for brevity

    pub fn getListingFee() -> U256 {
        return 0.1 ALPH
    }

    @using(preapprovedAssets = true, assetsInContract = true, updateFields = false)
    pub fn listNFT(
        tokenId: ByteVec,
        price: U256
    ) -> (Address) {
        assert!(price > 0, ErrorCodes.NFTPriceIsZero)

        // Only owner can list the NFT
        let tokenOwner = callerAddress!()

        let (encodeImmutableFields, encodeMutableFields) = NFTListing.encodeFields!(tokenId, tokenOwner, selfAddress!(), commissionRate, price)
        // Create the listing contract
        let nftListingContractId = copyCreateSubContract!{tokenOwner -> ALPH: 1 alph, tokenId: 1}(
            tokenId, nftListingTemplateId, encodeImmutableFields, encodeMutableFields
        )

        // Charge the listing fee
        transferTokenToSelf!(tokenOwner, ALPH, listingFee)

        return contractIdToAddress!(nftListingContractId)
    }
}
```

Điều cần chú ý đầu tiên là thông báo cho `listNFT` method:

```rust
@using(preapprovedAssets = true, assetsInContract = true, updateFields = false)
```

`preapprovedAssets = true` nói rằng VM mà nó `listNFT` method đang có ý định
sử dụng một vài asset và caller phải chấp thuận một chuỗi các
asset cần thiết nếu không một lỗi trong quá trình biên soạn sẽ bị chấm dứt và sẽ được thông báo. 
Trình biên soạn cũng sẽ báo lỗi nếu caller cố gắn chấp thuận các
asset cho một method khi `preapprovedAssets = false`.

`assetsInContract = true` chỉ ra cho VM rằng `listNFT`
method muốn cập nhật một asset của `NFTMarketPlace`
contract. Trình biên soạn sẽ cần phải xác nhận là `listNFT` method thật sự
làm công việc đó, nếu không một lỗi sẽ được cảnh báo. Trong trường hợp này,
`listNFT` cập nhật asset trong `NFTMarketPlace` contract bằng việc di chuyển `listingFee` vào trong nó:

```rust
// Charge the listing fee
transferTokenToSelf!(tokenOwner, ALPH, listingFee)
```

Chú thích `updateFields` nằm ngoài phạm vi của tài liệu này.

`marketPlace.listNFT` method được invoke bởi `TxScript` `ListNFT`, như ví
dụ bên dưới:

```rust
marketPlace.listNFT{callerAddress!() -> ALPH: approvedAlphAmount, tokenAId: 1}(tokenAId, price)
```

Khi `marketPlace.listNFT` được thực thi bởi VM, nó sẽ xác thực để
sử dụng `1.1` ALPH và `1` token từ caller của script. Nếu
`marketPlace.listNFT` lần lược call những method khác, nó có thể chấp thuận một
subset từ những asset đã được chấp thuận trước đó để đảm bảo rằng method đang hoạt động tốt. Ví dụ,
trong `marketPlace.listNFT` chúng ta có đoạn code bên dưới để tạo NFT
listing:

```rust
let nftListingContractId = copyCreateSubContract!{tokenOwner -> ALPH: 1 alph, tokenId: 1}(
    tokenId, nftListingTemplateId, encodeImmutableFields, encodeMutableFields
)
```

Như chúng ta có thể thấy, `marketPlace.listNFT` method chấp nhận `1` ALPH và `1`
Token A cho `copyCreateSubContract!` built-in function từ pool của nó nơi mà các asset đã được xác nhận (`1.1` ALPH và `1` Token A),trước khi nó được gửi 
 `listingFee` vào `NFTMarketPlace` contract của chín nó. Quá trình được mô tả như bên dưới:

```
  Caller of the TxScript
  (6.1 ALPH; 1 Token A)
           ||
           ||
           || Subtract assets in
           || Fixed outputs
           ||
           ||                    Approves                         Approves
           \/               (1.1 ALPH; 1 TokenA)              (1 ALPH; 1 TokenA)
  (5.1 ALPH; 1 Token A)  ========================> listNFT ========================> copyCreateSubContract!
                                                     ||
                                                     ||
                                                     || To self
                                                     ||
                                                     \/
                                                  (0.1 ALPH)
```

Ta có thể hình dung rằng, nếu chúng ta có một nhánh lớn hơn từ các method call, 
fund được chấp thuận sẽ đi như lần lượt từ gốc của các nhánh xuống 
các tâng thấp hơn. Asset Permission System làm cho quá trình 
gửi fund thông qua các method call được rõ ràng hơn và bắt buột hạn chế cho 
mỗi các method rằng các token nào và số lượng là bao nhiêu có thể được 
sử dụng.

Trở lại với giao dịch, sau khi `TxScript` được thực thi,
các generated output sẽ trông giống như bên dưới:

```
                        ----------------
                        |              |
                        |              |   1 ALPH (fixed output)
  1 Token A             |              | =========================================>
======================> |              |   1 ALPH, 1 Token A (NFTListing contract)
  6.1 ALPHs             |  <TxScript>  | =========================================>
======================> |              |   0.1 ALPH (NFTMarketPlace contract)
                        |              | =========================================>
                        |              |   4 ALPH - gas (change output)
                        |              | =========================================>
                        |              |
                        ----------------
```

## Tóm lại

Asset Permission System (APS) chỉ định một quá trình trong các asset của smart
contract. Các asset được xác nhận rõ ràng co mỗi method
invocation để đảm bảo các methods không bao giờ được sử dụng nhiều hơn 
số lượng mà chúng nó đã được xác thực trước đó. Cùng với UTXO model, nó cung cấp một 
giải pháp quản lý các tài sản đơn giản, tin cậy và bảo mật hơn.