---
sidebar_position: 30
sidebar_label: Chú thích
slug: /Chú thích
title: Chú thích
---

import UntranslatedPageText from "@site/src/components/UntranslatedPageText";

Đây là danh sách tổng hợp các khái niệm, thuật ngữ hữu ích cho việc tìm hiểu Alephium nói riêng và Blockchain nói chung.

## A

### Alephium

Alephium là blockchain L1 đầu tiên giúp mở rộng và nâng tầm các khái niệm PoW & UTXO. 
Ngoài các tính năng phi tập trung, tự chủ, bảo mật cao, dễ tiếp cận và tiết kiệm năng lượng, Alephium còn là một network thân thiện với lập trình viên, được tối ưu hoá cho các ứng dụng DeFi và hợp đồng thông minh (smart contract). 

Từ thiết kế kỹ thuật đến giao diện, Alephium đã được tạo ra để khắc phục các vấn đề về khả năng truy cập, khả năng mở rộng và bảo mật mà các ứng dụng phi tập trung hiện nay đang gặp phải. 


## B

### Thuật toán Blake 3 (Hash Function)
[Thuật toán Blake 3](https://github.com/BLAKE3-team/BLAKE3) là một hash function được mã hoá. Một hash function là một hàm toán học lấy chuỗi đầu vào có độ dài bất kỳ và chuyển đổi nó thành chuỗi đầu ra có độ dài cố định. Chuỗi đầu ra có độ dài cố định đó được gọi là hash value. 

Hash functions được sử dụng trong nhiều trường hợp trên một blockchain: trong [Merkle Tree](#merkle-tree), Proof of Work Consensus, digital signature, và trên chính Blockchain (vì mỗi block header của một block trong blockchain chứa hash của block header trước đó). Ví dụ như Bitcoin sử dụng [SHA-256.](https://en.wikipedia.org/wiki/SHA-2)

Alephium sử dụng thuật toán Blake 3 như là hash function được mã hoá cho công việc đào.

### Block Reward

Block reward là phần thưởng có giá trị kinh tế nhằm tạo động lực cho người đào tiếp tục công việc bảo vệ network. 

Người đào sẽ được trả bằng token của blockchain đó. Phần thưởng này thường sẽ nhiều hơn khi network còn nhỏ và mới, khi network đã phát triển thì phần thưởng sẽ ít lại. 
[Block Reward của Alephium trên GitHub](https://github.com/alephium/alephium/blob/master/protocol/src/main/scala/org/alephium/protocol/mining/Emission.scala)

### Block Size

Block size là giới hạn dữ liệu mà mỗi block có thể xử lý. 

Nó có thể được ước tính bằng nhiều cách. Trong một số blockchain, nó được biểu thị bằng lượng dữ liệu thực tế mà một block có thể chứa (ví dụ ở Zcash, block size là 2MB). Ở các blockchain khác, block size là giới hạn xử lý của máy tính mà nó có thể chiếm từ network (thường được biểu thị bằng gas). Block size của Etherium và Alephium được tính theo cách này. 

### Block Time

Block Time là thời gian cần thiết để tính toán các giao dịch trong một block và gửi nó lên network. 

Các giao dịch được tập hợp trong một block và được kiểm tra bởi các máy đào (hoặc các validators trên PoS blockchains). Thông thường block time bị ảnh hưởng bởi độ khó của việc đào, vì nó được điều chỉnh để phản ánh khả năng tính toán của network (hashrate) trong khoảng thời gian nhât định. 

Alephium network có sự điều chỉnh độ khó trên mỗi block và có block time được ước tính là **64 giây**. 

Tài liệu đọc thêm: [Block Time and Block Size Article](https://medium.com/@alephium/block-time-and-block-size-16e37292444f)

### Bridge

Bridge là một giao thức kết nối các blockchain riêng biệt để tạo ra sự tương tác giữa chúng. Mỗi blockchain thường có các tính năng công nghệ riêng, nhưng lại không có cách riêng để liên lạc với những giao thức khác. Vì thế bridge là một tập hợp các smart contract để liên kết các hệ sinh thái khác nhau này. 
 
Bridge có thể được chuyên biệt hơn bằng cách chỉ cho phép một loại tương tác (chằng hạn như di chuyển token), hoặc khái quát hơn khi cho phép bất kỳ loại truyền dữ liệu nào giữa các blockchain đã được bridged với nhau. 

## C

## D

## E

## F

## G

### Gas spent 

Gas Spent là số lượng tính toán mà máy đào dùng để thực hiện các giao dịch. Giao dịch nào càng có nhiều chức năng thì việc thực hiện nó càng phức tạp và càng tốn nhiều gas. 

Hiện tại, như một biện pháp anti-spam thì giá trị tối thiểu cho bất kỳ giao dịch nào trên Alephium là 20'000 gas, nghĩa là phí giao dịch của bạn có giá ít nhât là 0,002 ALPH. 

Khi network đã phát triển thì điều này sẽ được nới lỏng và thị trường sẽ xác định phí giao dịch. 

### Gas Price

Đây là giá trị tiền tệ của gas. Gas được định nghĩa là nỗ lực tính toán để thực thi một lệnh trong blockchain. Phí gas là tiền để trả cho công việc được thực hiện bởi người đào. 

### Genesis Block

Genesis Block là tên của block đầu tiên được đào trong một blockchain. Khi các block xếp chồng lên nhau thì Genesis Block là nền tảng hoặc là nơi bắt đầu của chúng. 

Đôi khi nó còn được gọi là Block 0 hay Block 1. Khi một block mới được thêm vào blockchain thi nó sẽ tham chiếu đến các block trước đó. Nhưng vì không có block nào ở trước, nên Genesis Block thường được hardcode vào phần mềm. 

Genesis Block của Alephium được đào vào ngày 8/11/2021. 

## H

### Hard Fork

Hard fork xảy ra khi có một nâng cấp lớn trên network's protocol làm cho các nodes hay người dùng sử dụng phiên bản cũ không thể gửi hoặc xác thực các giao dịch sau đó. 

Bởi việc nâng cấp là tuỳ chọn, nên đôi khi một số nodes hay người dùng quyết định không thực hiện, vì thế sẽ tạo ra một phiên bản blockchain khác từ đó, và điều này đã xảy ra với Ethereum và Ethereum Classic. 

## I

## J

## K

## L

## M

### Merkle Tree

Merkle tree [là một dạng cấu trúc](https://en.wikipedia.org/wiki/Merkle_tree) được sử dụng để nén dữ liệu một cách hiệu quả và an toàn hơn.

Khi blockchain gom các giao dịch vào các blocks thì mỗi block có một header, và header này có một hash; Hash này được lưu trữ trên Merkle Tree. Hash từ Merkle Tree đuợc dùng để xác minh rằng tệp dữ liệu giống với tập hợp giao dịch ban đầu mà không cần truy cập vào nội dung bên trong block. Khi hình dung, cấu trúc này giống một cái cây và có thể được gọi là một "binary hash tree". 

Ví dụ, Alephium sử dụng ba Merkle trees cho mỗi nhóm để lưu trữ tài sản - UTXOs, contract logic và contract state. 

### Maximal Extractble Value (MEV)

Người đào hay [Maximal Extractable Value (MEV)](https://ethereum.org/en/developers/docs/mev/) là giá trị thu được từ việc đào một block mà vượt quá tiêu chuẩn block reward và phí gas bằng cách thay đổi, bao gồm hoặc loại bỏ các giao dịch trong một block. 

Sự khác biệt này được đưa ra bởi các tác nhân được gọi là "searchers" nhằm phân tích nhóm tìm kiếm cơ hội lợi nhuận bằng cách thay thế thông tin trên một giao dịch, chẳng hạn như địa chỉ người gửi và người nhận. Để tăng xác suất giao dịch của họ được máy đào chọn trở thành một phần trong block tiếp theo được tạo ra, họ sẵn sàng trả phí gas cao hơn nhiều so với mức trung bình.

### Mining Reward

![](media/Block%20reward.png)

Mining reward là phần thưởng cho người đào cho việc tính toán cần thiết để xác thực các giao dịch và gom chúng vào một block. Trên Alephium, Mining reward có hai phần: [Phí giao dịch](#transaction-fee) và [Block Reward](#block-reward) hoặc new token emissions. Giao dịch trao thưởng cho người đào và phát hành đồng ALPH mới được đào, được gọi là một coinbase transaction. 

Công thức tính Mining reward:

Tổng Mining Reward = = Block Reward + min(max(Block Reward, 1 ALPH), transaction fees / 2)

![image](media/186885966-b8d746fb-612b-433e-8f79-47e5a87ea375.png)

Như một cơ chế giảm phát thì phân nửa phí giao dịch (transaction fees) sẽ được đốt. 

Đọc thêm: [Alephium Block Rewards](https://medium.com/@alephium/alephium-block-rewards-72d9fb9fde33)

### Multisig

Multisig hoặc Multisignature là quá trình yêu cầu hơn một private key để đồng ký tên một giao dịch để nó được đưa lên network. Đây chỉ là một bước bảo mật bổ sung. 

Usually, the multisig setup is done in a way that requires a minimal quorum of signers for a specific transaction to be approved and sent. For instance, a multisig of 5 out of 9 will require a quorum of 5 signers (among nine potential co-signers) to co-sign a transaction before it can be sent.

Alephium’s [Full Node Wallet](/wallet/node-wallet-guide) supports multisig addresses

## N

## O

## P

### Proof of Less Work (or PoLW)

Similar to Proof-of-Work for Bitcoin, or Proof-of-Stake for Ethereum (post-merge), PoLW is Alephium’s consensus algorithm. It optimizes the network's energy consumption without compromising its security & decentralisation. It is activated when the network surpasses 1 Eh/s of accumulated hashrate. 

After that, it partially internalizes the cost to mine a new block, by adding a coin-burning mechanism into the block validation process, incentivizing a cap on the processing power needed overall. Given the same network conditions, Alephium would only use ⅛ of the energy consumed by Bitcoin mining.

Additional resources: [TECH TALK #1 — The Ultimate guide to Proof-of-Less-Work, the universe and everything…](https://medium.com/@alephium/tech-talk-1-the-ultimate-guide-to-proof-of-less-work-the-universe-and-everything-ba70644ab301)

## Q

## R

## S

### Sharding 

Sharding is a strategy of database management that splits large databases into smaller, faster, more easily managed sections. 

These smaller parts are called [“shards”](https://en.wikipedia.org/wiki/Shard_(database_architecture)), which means "a small part of a whole." Sharding is used when the power needed to run the database exceeds the processing capacity of a single computer. Sharding becomes necessary when the size of the blockchain exceeds the processing power of the Virtual Machine and the network. Sharding breaks up the main blockchain into separate segments, and the nodes verify only a subset of transactions, allowing parallel transaction validation. This increases the network throughput. 

Alephium’s blockchain is sharded, and the Blockflow algorithm manages this. Currently, we have four groups with four shards in each one.

### Smart Contract 

[Smart Contract (SC)](https://en.wikipedia.org/wiki/Smart_contract) is a computer program that enables transactions to be executed by rules predefined, without needing to rely on a third party, central authority or external mechanisms. In the blockchain context, a smart contract is written using the native Programing Language or is compiled (translated) to it and usually runs on the blockchain’s [Virtual Machine.](#virtual-machine)

SCs on a blockchain can store arbitrary [state](#state) and execute arbitrary transactions. End clients also use transactions to interact with it. And the SC transactions can also invoke other SCs. These transactions might result in changing the state and sending coins from one smart contract to another or from one account to another.

In Alephium, the smart contracts are written using the Ralph language and run on Alphred Virtual Machine.

### State

The state is a [computer science concept](https://en.wikipedia.org/wiki/State_(computer_science)) where a machine can have multiple states, but only one at any given time.

A blockchain is considered to be a state machine. The state describes the system's current situation, and the transactions (inputs and outputs) trigger state transitions. As the transactions are bundled in blocks to make the process more efficient, the addition of a block is what changes the actual blockchain state.

Alephium uses the stateful UTXO model, which, compared to other UTXO accounting models, allows it to benefit from a full-featured state. 

## T

### Time to Finality

Time to Finality is the time between when a transaction is submitted to the network and when it’s considered final (and immutable). There are two main categories of finality: probabilistic finality and deterministic finality.

Most blockchain systems offer probabilistic transaction finality — this means that the probability that a transaction is valid and cannot be reversed increases with adding more blocks on the chain, but it’s never absolutely final. The network agrees that the transaction is final with enough time and blocks. This is how Bitcoin achieves finality, for example, a transaction is considered final after 6 blocks.

Other blockchains use a deterministic transaction finality (sometimes called absolute finality) — this means that the transaction is considered final when it is added to the blockchain. Fantom is one example of it.

Additional resource: [Time to Finality Article](https://medium.com/@alephium/time-to-finality-17d64eeffd25)

### Token

A token is a registry entry in a blockchain that follows a set of rules encoded by the smart contract issuing it. This definition makes it different from a cryptocurrency as the latter is the native asset of a blockchain like BTC or ETH, whereas tokens are built on an existing blockchain using smart contracts.

Tokens can be categorized as fungible or non-fungible. Fungible tokens are identical and can seamlessly replace one another. On the other hand, non-fungible tokens (NFTs) are unique and provably scarce, meaning their histories can be traced down to the individual level.

Tokens can also be categorized by their intended function: Utility, Security, or Currency Tokens. Currency tokens are created to be traded, like MakerDAO’s DAI or USDC. Utility tokens are focused on practical use, representing access to a given product or service. Security tokens are a digital representation of an underlying asset, such as a share in a company, voting right in a company or other centralized organization, or some tangible or digital article of value.

### Transaction Fee 

![image](media/186886291-79745fc1-25dc-4307-a752-400ce1ff2d31.png)

When someone does a transaction in Alephium, there’s a price to be paid to the miners for including it in a block. 

This price is composed of two elements: the [Gas Price](#gas-price) in the network’s native token and the [Gas Amount Spent](#gas-amount-spent) on this transaction processing and can be defined by this equation:

Transaction fee = Gas Price * Gas Amount Spent

Additional resources: [Transaction fee GitHub Implementation](https://github.com/alephium/alephium/blob/v1.4.2/protocol/src/main/scala/org/alephium/protocol/model/Transaction.scala#L230-L239)

### Transactions Per Second (TPS)

Transactions Per Second (TPS) is a measure that comes from the [database systems](https://en.wikipedia.org/wiki/Transactions_per_second) environment, and it means how many transactions theoretically can happen in one second in a given system.

In the blockchain context, it is used as a synonym for speed: how fast a transaction can be broadcasted to the network. The following equation calculates it:
 
TPS = (Block Size / Transaction Size ) /Block Time

Additional resources:[Transactions Per Second Article](https://medium.com/@alephium/transactions-per-second-tps-f13217a49e39)

## U

### UTXO

[UTXO](https://en.wikipedia.org/wiki/Unspent_transaction_output) (Unspent Transaction Output) is the term for the amount of a specific currency that remains unspent after a cryptocurrency transaction.

On a UTXO account model blockchain, the portion of what was sent and not spent in a transaction is used as an accounting method. Like double-entry accounting, each transaction has an input and output.

Improved versions were built over it, like eUTXO, Cell System, or Alephium’s sUTXO.

## V

### Virtual Machine

A Virtual Machine (VM) is a software emulation of a physical computer to run programs and deploy apps.

A virtual machine runs its own operating system and functions. Each node runs a copy of the VM to run the programs (smart contracts) and allow them to interact with each other and the blockchain itself. 

Alephium’s Virtual machine is called Alphred and has a lot of very [interesting properties.](https://www.youtube.com/watch?v=VVYH9rBJAdA&list=PLqL60kqgLPBBrc64K-1Gs771FBTiLtYZE&index=29)


## W

## X

## Y

## Z

