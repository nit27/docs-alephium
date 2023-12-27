---
sidebar_position: 30
title: Non-fungible Tokens (NFTs) - Token không thể thay thế
sidebar_label: Non-fungible Tokens (NFTs) - Token không thể thay thế
---

Non-fungible tokens (NFTs) trên Alephium có một số tính năng độc đáo hơn so với NFT trên các blockchain khác:

- Quyền sở hữu thực sự dựa trên mô hình UTXO: giống như các token các trên Alephium, NFT được sở hữu trực tiếp bởi các địa chỉ. Vì UTXO được bảo vệ bằng private keys của người dùng nên ngay cả khi có lỗi trong NFT contract, tài sản của người dùng vẫn được an toàn. 

- Hỗ trợ bậc nhất cho NFT: token là tài sản gốc trên Alephium. Nhờ vậy mà NFT của nguời dùng có thể dễ dàng được tìm kiếm và hiển thị trên ví, trình duyệt, và dApp mà không cần dựa vào các tiện ích của bên thứ ba. 

- Bảo mật cao hơn nhờ VM và ngôn ngữ hợp đồng của Alephium: virtual machine (VM) và ngôn ngữ hợp đồng của Alephium loại bỏ giao dịch phê duyệt riêng cho quá trình giao dịch NFT, từ đó hạn chế những rủi ro liên quan. Điều này giúp đơn giản hoá việc viết hợp đồng NFT bảo mật cho các lập trình viên với sự hỗ trợ của các công cụ như [Asset Permission System](/ralph/asset-permission-system).

- Hệ thống Hợp đồng phụ: Trên Alephium, không có cấu trúc dữ liệu [mapping](https://docs.soliditylang.org/en/v0.8.7/types.html#mapping-types). Bộ sưu tập được tạo bằng hợp đồng gốc (bộ sưu tập) và [hợp đồng phụ](http://localhost:3000/ralph/built-in-functions#subcontract-functions) (the items). Mỗi hợp đồng phụ đại diện cho một NFT trong bộ sưu tập này, và tất cả siêu dữ liệu được gắn với nó. Đây là tính năng riêng trên Blockchain của Alephium, nhờ vậy mà NFT của Alephium là độc nhất (một token cho mỗi hợp đồng phụ) hoặc semi-fungible (bán độc-nhất), vì cùng một hợp đồng phát hành có thể tạo ra nhiều token.

- Phân nhóm giao dịch hiệu quả: nhiều NFT và người dùng có thể tham gia vào một giao dịch.

- Phí giao dịch rẻ hơn và băng thông cao hơn: các giao dịch NFT được hưởng lợi từ thuật toán sharding của Alephium.

- Sự khan hiếm NFT: nguồn cung NFT trên Alephium là hữu hạn, vì mỗi NFT yêu cầu triển khai hợp đồng phụ riêng, do đó sẽ yêu cầu đặt cọc ALPH - hiện tại là 1 `ALPH`. Cách vận hành độc nhất này đã đặt ra giới hạn trong việc tạo ra NFT trên platform, do đó gia tăng sự khan hiếm của NFT trên Alephium. 
  
### Tiêu chuẩn của Non-fungible token

 Bộ sưu tập NFT và các NFT riêng lẻ đều có siêu dữ liệu được gắn vói chúng, chẳng hạn như `collectionUri`, `totalSupply` và `tokenUri`, v.v. Giao diện [INFTCollection](https://github.com/alephium/alephium-web3/blob/master/packages/web3/std/nft_collection_interface.ral) và [INFT](https://github.com/alephium/alephium-web3/blob/master/packages/web3/std/nft_interface.ral) chuẩn hoá phương pháp tìm nạp siêu dữ các liệu này. 
```rust
// Standard interface for NFT collection
@std(id = #0002)
Interface INFTCollection {
   pub fn getCollectionUri() -> ByteVec
   pub fn totalSupply() -> U256
   pub fn nftByIndex(index: U256) -> INFT
   pub fn validateNFT(nftId: ByteVec, nftIndex: U256) -> () // Validates that the NFT is part of the collection, otherwise throws exception.
}

// Standard interface for NFT
@std(id = #0003)
Interface INFT {
   pub fn getTokenUri() -> ByteVec
   pub fn getCollectionIndex() -> (ByteVec, U256) // Returns collection id and index of the NFT in the collection.
}
```

Chúng cũng được ghi chú bằng chú thích `@std` để tạo điều kiện cho dApp và ví truy xuất loại hợp đồng/token của chúng.

```typescript
// Guess NFT token type
const nftTokenType = await web3.getCurrentNodeProvider().guessStdTokenType(nft.contractId)
expect(nftTokenType).toEqual('non-fungible')

// Check if a contract is a NFT collection
const isNFTCollection = await web3.getCurrentNodeProvider().guessFollowsNFTCollectionStd(nftCollection.contractId)
console.log("Is NFT collection", isNFTCollection)
```

Đối với các hợp đồng triển khai [INFTCollection](https://github.com/alephium/alephium-web3/blob/master/packages/web3/std/nft_collection_interface.ral) và [INFT](https://github.com/alephium/alephium-web3/blob/master/packages/web3/std/nft_interface.ral), SDK cung cấp phương pháp chuẩn để tìm nạp siêu dữ liệu tương ứng của chúng:

```typescript
// NFT Collection Metadata
const collectionMetadata = await web3.getCurrentNodeProvider().fetchNFTCollectionMetaData(nftCollection.contractId)
console.log("NFT Collection URI, totalSupply", collectionMetadata.collectionUri, collectionMetadata.totalSupply)

// NFT Metadata
const nftMetadata = await web3.getCurrentNodeProvider().fetchNFTMetadata(nft.contractId)
console.log("NFT Token URI, collection address", nftMetadata.tokenUri, nftMetadata.collectionAddress)
```

Đối với các bộ sưu tập NFT, một trong các siêu dữ liệu là `collectionUri`, đây là URI dẫn đến tài liệu JSON theo cơ chế sau

```typescript
interface NFTCollectionUriMetaData {
  name: string            // Name of the NFT collection
  description: string     // General description of the NFT collection
  image: string           // A URI to the image that represents the NFT collection
}
```

Đối với NFT riêng lẻ, một trong những siêu dữ liệu là `tokenUri`, đây là URI dẫn đến tài liệu JSON document với cơ chế sau:

```typescript
interface NFTTokenUriMetaData {
  name: string                           // Name of the NFT
  description?: string                   // General description of the NFT
  image: string                          // A URI to the image that represents the NFT
  attributes?: [                         // Attributes of the NFT
    {
      trait_type: string
      value: string | number | boolean
    }
  ]
}
```

### Sàn giao dịch AlephiumNFT 

Sàn giao dịch [AlephiumNFT](https://github.com/alephium/alephium-nft) là một sàn giao dịch NFT theo proof-of-concept để thể hiện năng lực của NFT trên Alephium. Tại đây, bạn có thể tạo các bộ sưu tập NFT, khám phá, phát hành và giao dịch NFT. Bạn cũng có thể triển khai chiến dịch public sale theo [Opensea Drop](https://docs.opensea.io/docs/drops-on-opensea) cho bộ sưu tập NFT của bạn. Những chiến dịch này được gọi là `Flows` trên sàn giao dịch `AlephiumNFT`.

Việc tạo bộ sưu tập NFT của riêng bạn khá đơn giản. Theo dõi [Twitter thread](https://twitter.com/alephium/status/1674397159947649030) để biết thêm chi tiết. Nếu bạn muốn tạo `Flow` trên sàn `AlephiumNFT`, [@alephium/cli](https://www.npmjs.com/package/@alephium/cli) có một sub-command `nft` có thể hỗ trợ bạn.

#### Cách tạo Flow

Giả sử bạn muốn mở một đợt public sale cho bộ sưu tập NFT của bạn, trong đó có `5` NFT riêng lẻ. Trước khi bạn tạo `Flow` cho nó, bạn nên chuẩn bị sẵn `5` hình ảnh. Nếu không, `@alephium/cli` sẽ đưa ra lệnh để bạn tạo hình ảnh bằng OpenAI's [DALL.E](https://openai.com/research/dall-e):

```bash
export OPENAI_API_KEY=xxxx-xxxx-xxxx-xxxx
npx @alephium/cli@latest nft generate-images-with-openai --number 5 -d /tmp/imagine "imagine all the people, living life in peace"
```

Việc này sẽ tạo ra `5` hình ảnh với chú thích `imagine all the
people, living life in peace` và lưu trữ chúng trong thư mục `/tmp/imagine`. Vui lòng bỏ qua bước này nếu bạn đã thiết kế hình ảnh cho bộ sưu tập của mình rồi. 

Giả sử hình ảnh đã sẵn sàng trong thư mục `/tmp/imagine`. Bước tiếp theo là tạo tệp siêu dữ liệu ở định dạng YAML cho bộ sưu tập của bạn. Đây là một ví dụ về tệp YAML có tên `imagine.yaml`:

```bash
> ls /tmp/imagine
0.jpg  1.jpg  2.jpg  3.jpg  4.jpg

> cat imagine.yaml
0.jpg:                              # File name of the image
  attributes:                       # Attributes of the NFT, optional
    - color: blue                   # Value of the attributes can be `number`, `boolean` or `string`
    - is_outdoor: true
1.jpg:
  description: Imagine is too naive # Description of the NFT, optional
2.jpg:
  name: Imagine in Asia             # Name of the NFT, optional
  attributes:
    - color: blue
    - is_outdoor: false
3.jpg:                              # Name is auto-generated as #${index} if not specified, e.g. #04
4.jpg:
```

Nếu bạn đã hài lòng về hình ảnh và siêu dữ liệu của bộ sưu tập của mình, hãy chạy lệnh sau để gửi hình ảnh và siêu dữ liệu lên IPFS:

```bash
> export IPFS_INFURA_PROJECT_ID=xxxx-xxxx-xxxx-xxxx
> export IPFS_INFURA_PROJECT_SECRET=xxxx-xxxx-xxxx-xxxx
> npx @alephium/cli@latest nft upload-images-and-metadata-to-ipfs -m imagine.yaml -d /tmp/imagine -i imagine
NFTBaseUri:
https://ipfs.io/ipfs/QmaTXEGJQe5ZLg9TVEBJEpz3dwbzG9m7b6NWVogxnYgnbJ/
```

`NFTBaseUri` dẫn đến thư mục IPFS trong đó `5` tài liệu được đặt tên và lưu trữ theo trình tự của chúng trong file `imagine.yaml`:

<img src={require("./media/ipfs-imagine-directory.png").default} alt="IPFS Imagine Directory"/>

Mỗi tài liệu đều dẫn đến siêu dữ liệu của NFT và có thể được tham chiếu theo index của chúng. Ví dụ, `https://ipfs.io/ipfs/QmaTXEGJQe5ZLg9TVEBJEpz3dwbzG9m7b6NWVogxnYgnbJ/2` dẫn đến siêu dữ liệu của NFT thứ 3:

```bash
> curl https://ipfs.io/ipfs/QmaTXEGJQe5ZLg9TVEBJEpz3dwbzG9m7b6NWVogxnYgnbJ/2 | jq
{
  "name": "Imagine in Asia",
  "image": "https://ipfs.io/ipfs/QmbLevU4kVnQCCoYt23mKhdowJ7TnNNT9dRyVw9AyQDJty/2.jpg",
  "attributes": [
    {
      "trait_type": "color",
      "value": "blue"
    },
    {
      "trait_type": "is_outdoor",
      "value": false
    }
  ]
}
```

Bạn có thể xác thực xem `NFT BaseUri` có hợp lệ hay không bằng lệnh sau:

```bash
> npx @alephium/cli@latest nft validate-enumerable-nft-base-uri --nftBaseUri https://ipfs.io/ipfs/QmbLevU4kVnQCCoYt23mKhdowJ7TnNNT9dRyVw9AyQDJty/ --maxSupply 5
Token Metadataz:
[
  {
    name: '#0',
    ....
  },
  ....
]
```

Sau khi `NFTBaseUri` được tạo, chúng ta đã sẵn sàng để khởi chạy `Flow` trên sàn `AlephiumNFT`:

<img src={require("./media/create-flow-page.png").default} alt="Create FLow Page"/>

Như đã trình bày ở trên, bạn có thể gán cho hình ảnh trong bộ sưu tập: the max batch mint size, the mint price, tên và mô tả của bộ sưu tập, và quan trọng nhất là NFT base URI that we created in the last step. After you click the `Create NFT Collection` button and sign the transaction, you will successfully create your first `Flow`, share the link and start to launch the public sale of your NFT collection!

<img src={require("./media/flow-page.png").default} alt="FLow Page"/>

### Hỗ trợ ví

Cả [Ví Desktop](/wallet/desktop-wallet/overview) và [Ví mở rộng](/wallet/extension-wallet/overview) có hỗ trợ riêng cho non-fungible tokens.

Sau đây là ví dụ về cách hiển thị và di chuyển NFT trong `Bộ sưu tập hình ảnh` trong ví mở rộng:

<img
src={require("./media/show-nft-collection-extension-wallet.png").default} alt="Show collection" width="250"/>
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/transfer-nft-collection-extension-wallet.png").default} alt="Transfer NFT" width="250" />

### Danh sách Token 

Hiện nay, việc làm giả các bộ sưu tập NFT và lừa đảo người dùng không quá khó. Vì vậy [Danh sách Token](https://github.com/alephium/token-list) cho phép các bộ sưu tập NFT nổi tiếng trong hệ sinh thái Alephium được đưa vào danh sách trắng, để các dApp và ví lưu trữ có thể cảnh báo người dùng về các bộ sưu tập NFT chưa được xác minh. Dưới đây là cách ví mở rộng hiển thị bộ sưu tập NFT truớc và sau khi nó được thêm vào danh sách token. 

<img src={require("./media/unverified-nft-collection.png").default} alt="Unverified" width="250"/>
&nbsp;&nbsp;&nbsp;&nbsp;
<img src={require("./media/verified-nft-collection.png").default} alt="Verified" width="250"/>

Hiện tại, bạn cần pull request để thêm bộ sưu tập NFT vào danh sách token. 

