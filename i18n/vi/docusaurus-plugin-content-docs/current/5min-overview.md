---
sidebar_position: 0
sidebar_label: Tổng quan
slug: /
title: Tổng quan về Alephium
---

import UntranslatedPageText from "@site/src/components/UntranslatedPageText";

## Alephium là gì?

**Là một blockchain có khả năng mở rộng cho lập trình viên. Bảo mật cho người dùng. Và phi tập trung cho mọi người.**

Hoạt động dựa trên sự kết hợp giữa công nghệ sharding thế hệ mới đáp ứng cho các sUTXO và thuật toán tiết kiệm năng lượng Proof-of-Less-Work. Với sự kết hợp này, mạng lưới có thể gia tăng khả năng mở rộng nhưng vẫn đảm bảo tính bảo mật cho các ứng dụng phi tập trung (dApps).

---

## Điều gì kiến Alephium đặc biệt?

Với sự hội tụ của những thành viên trong một cộng đồng xuất sắc, một đội ngũ tuyệt vời. Chúng tôi luôn tràn đầy năng lượng để tạo ra những giải pháp và các sản phẩm chất lượng. Dưới đây là những đặc điểm kỹ thuật làm nên sự đặc biệt của Alephium:

**Có khả năng mở rộng thông qua sharding**. Alephium được xây dựng trên thuật toán sharding mới và hoàn chỉnh, được gọi là BlockFlow. Nó cải thiện mô hình UTXO của BTC để có thể mở rộng và sử dụng cấu trúc dữ liệu DAG để đạt được sự đồng thuận giữa các phân đoạn (shard) khác nhau. Điều này sẽ cho phép tối đa 10'000 Transaction Per Second  (giao dịch mỗi giây). Hiện tại, con số này là hơn 400 TPS so với Bitcoin là 7 TPS).

**Có thể được lập trình & bảo mật**. Alephium đưa ra một mô hình stateful UTXO (kế hợp giữa tính bảo mật của mô hình UTXO truyền thống và mô hình account) hỗ trợ khả năng mở rộng layer-1 và mức độ lập trình tương tự như mô hình account được triển khai trên ETH, nhưng an toàn và bảo mật hơn.

**Tiêu thụ ít năng lượng hơn nhờ vào POLW**. Proof of Less Work là sự kết hợp giữa các công đoạn của việc khai thác và coin economic để điều chỉnh linh hoạt năng lượng trong quá trình khai thác các block mới. Trên cùng một điều kiện lý tưởng, Alephium chỉ sử dụng ⅛ năng lượng so với Bitcoin.

**Cải thiện cấu trúc on chain với VM (Alphred) của chính nó**. Nó giải quyết nhiều vấn đề quan trọng của các nền tảng dApp hiện tại với những cải tiến lớn về bảo mật, cung cấp những trải nghiệm quen thuộc trong việc phát triển phần mềm và giới thiệu các mô hình mới như các giao dịch trên trustless P2P smart contracts.

**Có ngôn ngữ lập trình riêng cho các dApp**. Đó chính là ngôn ngữ Ralph, có cú pháp tương tự như Rust. Nó cho phép xây dựng các smart contract dễ dàng và an toàn hơn so với Solidity. Ralph được thiết kế đặc biệt, giúp lập trình viên thuận tiện tạo ra các ứng dụng DeFi!

Kết hợp tất cả những yếu tố đổi mới này lại với nhau, Alephium cung cấp giải pháp được yêu cầu cao trong nhiều khía cạnh: một blockchain có khả năng mở rộng dựa trên ý tưởng vững chắc của Bitcoin để mang lại sự tin cậy, sức mạnh và bảo mật cho DeFi và dApp. Và chúng tôi đã triển khai chính thức!

**Xem ngay [whitepapers][whitepaper]!**

---

## Về tokenomic

Tổng cung của Alephium được giới hạn hardcap là 1 tỷ ALPH. Tại thời điểm Mainnet (08/11/2021), 140 triệu token từ nguồn cung đầu tiên (14% của hardcap) được đào với genesis block. Các token ALPH từ nguồn cung còn lại sẽ được đào hết trong dự kiến 80 năm nữa. Chi tiết về 140 triệu token:

- 80 triệu token (8%) 🤝 **Pre-sale và private sale**. Thời gian trả token trong vòng từ 2 đến 4 năm.

- 30 triệu token (3%) 💡 **Phát triển cộng đồng & hệ sinh thái**. Được khoá on-chain trong 4 năm và trả hằng quý. 

- 30 triệu token (3%) 🧑‍💻 **Ngân sách & Đội nhóm**. Được khoá on-chain trong 3 năm, và trả hằng quý.

860 triệu token còn lại (86%) 🌊 **Phần thưởng khai thác**. Số token này sẽ được sử dụng cho phần thưởng khai thác trong suốt khoảng 80 năm kế tiếp. Các thợ đào sẽ đảm nhận các công việc xác nhận các giao dịch và thực thi smart contract trên blockchain của Alephium.

Ngoài ra, một nữa chi phí của các giao dịch sẽ được đốt với mỗi block và Proof of Less Work sẽ kích hoạt chi phí internal mining thông qua việc đốt khi hashrate và năng lượng tiêu thụ trở nên tăng cao.


### Tổng cung và cung lưu thông

Tổng cung được tính bằng tổng các token được đào tại Genesis Block (đã đề cập bên trên) và phần thưởng khai thác kể từ khi khởi chạy Mainnet.

Alephium sử dụng phương pháp Coin Market Cap (CMC) để tính cung lưu thông: tổng số lượng ALPH hiện hữu không bao gồm số token (đã khoá và chưa khoá) từ private sale, ngân sách, hệ sinh thái, cố vấn/nhà đầu tư, các tài sản thuộc dự án được quản lý và tất cả ALPH bị khoá từ những địa chỉ ví khác.
Bạn có thể xem số cung lưu thông trên trang [explorer](https://explorer.alephium.org/#/blocks). Tìm hiểu kỹ hơn về CMC [tại đây.](https://support.coinmarketcap.com/hc/en-us/articles/360043396252-Supply-Circulating-Total-Max-)

Một cách tính đơn giản: [Tổng cung](https://mainnet-backend.alephium.org/infos/supply/total-alph) = [Cung lưu thông](https://mainnet-backend.alephium.org/infos/supply/circulating-alph) + [Nguồn cung dự trữ](https://mainnet-backend.alephium.org/infos/supply/reserved-alph) + [ALPH bị khoá](https://mainnet-backend.alephium.org/infos/supply/locked-alph)

**Tìm hiểu chi tiết về bài viết [Tokenomics trên Medium][tokenomics-medium].**

---

## Nếu bạn là một lập trình viên, hãy bắt đầu tại đây

Tìm [node release trên GitHub][node-release].

Khám phá và đóng góp vào các [dự án trên GitHub][github]:

- [Full node][full-node]
- [Desktop wallet][desktop-wallet]
- [Explorer][explorer]
- [Web3 SDK][web3-sdk]
- [Mobile Wallet][mobile-wallet-repo]
- [Extension Wallet][extension-wallet-repo]
- [Wallet Connect][walletconnect-repo]
- [Bridge][wormhole-fork-repo]
- [Wiki][wiki]
- [Awesome Alephium][awesome]

### Bạn muốn phát triển ứng dụng của mình trên Alephium?

Hãy bắt đầu với [Web3 SDK][web3-sdk] và nếu bạn muốn đang xây dựng một dApp, khám phá tại [đây](./dapps/Getting-Started). 
Sau khi hoàn thành đóng góp và dự án của bạn, hãy cho mọi người biết đến bằng việc tạo một PR để thêm dự án của bạn vào [Awesome Alephium][awesome]!
Cẩm nang về Alephium [Brand Guide][brand-guide] có thể giúp ích cho bạn.

Tìm hiểu về chương trình Phần thưởng & Tài trợ: [Reward & Grant Program][reward-grant]

## Nếu bạn là một thợ đào, hãy bắt đầu tại đây

Bắt đầu tham gia các kênh khai thác tại [Mining channel trên Discord][mining-discord].

Tìm hiểu về [Miner starter pack trên Github][miner-starter-pack].

và các repo về việc khai thác:

- https://github.com/alephium/gpu-miner
- https://github.com/alephium/fpga-miner
- https://github.com/alephium/mining-pool

Xem video hướng dẫn [cách bắt đầu solo mining][solo-mining-video].

Và nếu bạn muốn tham gia vào các pool đào, hãy xem qua danh sách các pool đang khả dụng tại [Pool Mining Guide](./mining/pool-mining-guide).

---

## Những cột mốc & Lộ trình phát triển

### Các cột mốc đã hoàn thành

**Nền tảng cốt lõi**

- 02.2019 - Phát hành Whitepaper
- Q1.2020 - Tích hợp phiên bản đầu tiên của giao thức sharding cốt lõi và thử nghiệm trên AWS
- 12.2020 - Khởi chạy Testnet
- Q1.2021 - Hỗ trợ Smart contract
- 09.2021 - Phát hành Desktop wallet & explorer
- **08.11.2021 - Khởi chạy Mainnet**
- 01.2022 - dApp prototype đầu tiên
- 06.2022 - Phiên bản beta của contract SDK (alephium-web3)
- 06.2022 - Nâng cấp Leman Network được triển khai trên testnet
- 11.2022 - Phiên bản đầu tiên của multi-guardians bridge trên Testnet
- Q4.2022 - Phát hành NFT prototype
- 03.2023 - Phát hành DEX prototyped
- 03.2023 - Phát hành Browser Wallet Extension
- **27.03.2023 - Nâng cấp Leman Network trên Mainnet**
- 03.2023 - Schnorr signatures và tương tác đa chuỗi (cross chain)
- 03.2023 - Asset Permission System mới & loạt hướng dẫn Virtual Machine (VM) và xây dựng các chức năng
- 03.2023 - Cải thiện node APIs and SDK
- 03.2023 - Cải thiện thuật toán điều chỉnh độ khó - difficulty adjustment algorithm (DAA)


**Hệ sinh thái**

- 08.11.2021 - Dịch vụ cloud mining bên thứ 3 tại khởi chạy Mainnet
- 11.2021 - Tham gia Liên minh UTXO & Hiệp hội Bitcoin Thuỵ Sỹ
- Q4.2021 - Phát triển hệ sinh thái khai thác: community pools, hội thợ đào, nhóm khai thác tham chiếu và nhóm khai thác tích hợp
- 12.01.2022 - ALPH được list trên sàn giao dịch đầu tiên: Gate.io
- Q3.2022 - Các hoạt động marketing đầu tiên ( được tài trợ từ ErgoHack, AMAs, đóng góp từ cộng đồng, các chiến dịch quảng bá, Tech Talk Series, v.v)
- Q3.2022 - dApps bên thứ 3 được triển khai trên Alephium
- 10.2022 - Tích hợp Dappnode
- 11.2022 - Tích hợp Flux
- 11.2022 - Phát hành Alephium Swag Shop
- H2.2022 - Sự đóng góp từ cộng đồng và các sáng kiến marketing với các thành viên trong Liên minh UTXO
- 12.2022 - Đạt hơn 265 sự đóng góp đến từ cộng đồng 


### Lộ trình phát triển

Mạng lưới của chúng ta dù mới nhưng phát triển rất nhanh. Chúng tôi sẽ liên tục cập nhật các lộ trình phát triển để thông báo các mục tiêu trọng điểm đến các bạn.

**Nền tảng cốt lõi**

- [Tính năng] Desktop Wallet v2.0
- [Phát hành] Mobile wallet (Android và iOS)
- [Phát hành] Alephium’s Bridge trên Mainnet
- [Tính năng & UX] Hỗ trợ streaming cho blockchain events (giao dịch, block, contract events)
- [Tính năng & Bảo mật] Typescript SDK cho giao dịch off-chain encoding/decoding
- [Tính năng] Giao dịch P2P endpoint và SDK
- [Cộng đồng] Giới thiệu AIP cho Alephium Improvement Proposal
- [Cải thiện] Cải thiện web3 SDK với DevX tốt hơn với nhiều chức năng
- [Tính năng] Hỗ trợ multisig giữa SDK và các ví
- [Bảo mật] Các tiêu chuẩn cho hiển thị thông tin giao dịch trên ví
- [Cải thiện] Cải thiện hệ thống database của full node mang lại hiệu suất tối ưu hơn
- [DevX & UX] Cải thiện các lỗi hệ thống cho full node và các endpoint
- [Tính năng] Thiết kế và tích hợp fast sync cho full node
- [Tính năng] Smart contracts hỗ trợ trên Explorer
- [Cải thiện] Cải thiện UX và các tính năng nâng cao cho Browser Wallet Extension
- [Cải thiện] Cải thiện tiêu chuẩn của NFT và prototype
- [Phát hành] dApps prototype mở rộng
- [DevX] Cải thiện trải nghiệm người dùng và các tính năng nâng cao cho ngôn ngữ Ralph
- [Cải thiện] Cải thiện các văn bản hướng dẫn 
- [Tính năng] Thiết kế và tích hợp light node


**Hệ sinh thái**

- ALPH được list trên các sàn giao dịch CEX khác
- Phát hành DEX 
- Phát hành các dApp bên thứ 3 (NFT, DEX, stablecoin, Alephium Name Services, oracle, v.v) trên Alephium
- Tích hợp Ledger wallet
- Bridge cho phép tương tác thêm với những hệ sinh thái khác
- Giới thiệu các video & và các bài đăng về hướng dẫn xây dựng các dApp trên Alephium
- Tích hợp ví bên thứ 3
- Alephium Hackathon đầu tiên
- Chương trình Tài trợ & Phần thưởng V2
- Cải thiện trải nghiệm làm quen cho lập trình viên
- Triển khai chương trình Người quảng bá
- Thúc đẩy sự đóng góp cộng đồng
- Đẩy mạnh marketing, kênhh đối tác và lan toả kiến thức đến các cộng đồng và các dự án
- Cải thiện website


---

## Mua/Bán

- [Gate.io - USDT][gateio]
- [TradeOgre - USDT][tradeogreUSDT]
- [TradeOgre - BTC][tradeogreBTC]

---

## Tham gia cộng đồng Alephium!

### Talk

- [Discord][discord]
- [Telegram][telegram]
- [Reddit][reddit]

### Connect 

- [Twitter][twitter]
- [LinkedIn][linkedin]
- [Facebook][facebook]

### Read, setup, explore, contribute

- [Website][website]
- [Whitepapers][whitepaper]
- [Medium][medium]
- [GitHub][github]

---

## Nội dung không chính thức & cộng đồng

:::info
Các nội dung này do các thành viên trong cộng đồng đóng. Không được kiểm duyệt và không thuộc sự quản lý chính thức bởi Alephium
:::

Tất cả danh sách các channel quốc tế tại [đây](./misc/Internationalization-and-Localization).

### Youtube

- [Youtube 🌎](https://www.youtube.com/playlist?list=PL8q8n0BHJS1Nd0nxGfsNJzNnAeHoXhezz)
- [Youtube 🇧🇷](https://www.youtube.com/playlist?list=PL8q8n0BHJS1PiisJCIWqeOsd20dsMtJIg)
- [Youtube 🇨🇳](https://www.youtube.com/playlist?list=PL8q8n0BHJS1O931vGMfFb0Qx3gFKhd4bD)
- [Youtube 🇩🇪](https://www.youtube.com/playlist?list=PL8q8n0BHJS1OtYdw8lKeke6nNSSfASzZq)
- [Youtube 🇮🇳](https://www.youtube.com/playlist?list=PL8q8n0BHJS1PBoCF0L2TfeWYC8b7DeTAn)
- [Youtube 🇮🇩](https://www.youtube.com/playlist?list=PL8q8n0BHJS1MEOKbcmicEO0uTuz67D5Fz)
- [Youtube 🇮🇹](https://www.youtube.com/playlist?list=PL8q8n0BHJS1O749KEPqfnwlr-RDlqJ20U)
- [Youtube 🇯🇵](https://www.youtube.com/playlist?list=PL8q8n0BHJS1PS9PGIYJd8pjK6fw8AKZO4)
- [Youtube 🇲🇾](https://www.youtube.com/playlist?list=PL8q8n0BHJS1OkFwspCxIVfFS2sVeGEC4K)
- [Youtube 🇷🇺](https://www.youtube.com/playlist?list=PL8q8n0BHJS1P4-22OaT_w3vwNZVwiQt6s)
- [Youtube 🇹🇭](https://www.youtube.com/playlist?list=PL8q8n0BHJS1MhpbWV3PI4xoXhjB06az_M)
- [Youtube 🇹🇷](https://www.youtube.com/playlist?list=PL8q8n0BHJS1OJIUOh0yANAEKdSUG8DdDG)
- [Youtube 🇻🇳](https://www.youtube.com/playlist?list=PL8q8n0BHJS1PJq68hRBfw3xeXGlfVDWVr)

---

## Đối tác

- [Bitcoin Association Switzerland](https://medium.com/@alephium/alephium-becomes-a-member-of-bitcoin-association-switzerland-2293fec16fc9)
- [Cetacean Capital](https://cetacean.capital/)
- [Crypto Valley Association](https://cryptovalley.swiss/)
- [Dappnode](https://dappnode.io)
- [Ergo](https://ergoplatform.org/)
- [Flux Labs](https://runonflux.io/fluxlabs.html)
- [Hodling SA](https://www.hodling.ch/)
- [UTXO Alliance](https://utxo-alliance.org/)


[whitepaper]: https://github.com/alephium/white-paper
[tokenomics-medium]: https://medium.com/@alephium/tokenomics-of-alephium-61d59b51029c
[website]: https://alephium.org/
[discord]: https://alephium.org/discord
[telegram]: https://t.me/alephiumgroup
[twitter]: https://twitter.com/alephium
[linkedin]: https://www.linkedin.com/company/alephium
[facebook]: https://www.facebook.com/alephium
[medium]: https://medium.com/@alephium
[github]: https://github.com/alephium
[gateio]: https://www.gate.io/fr/trade/ALPH_USDT
[tradeogreBTC]: https://tradeogre.com/exchange/BTC-ALPH
[tradeogreUSDT]: https://tradeogre.com/exchange/USDT-ALPH
[utxo-alliance]: https://utxo-alliance.org/
[bas]: https://medium.com/@alephium/alephium-becomes-a-member-of-bitcoin-association-switzerland-2293fec16fc9
[market-across]: https://marketacross.com/
[node-release]: https://github.com/alephium/alephium/releases/latest/
[full-node]: https://github.com/alephium/alephium
[desktop-wallet]: https://github.com/alephium/desktop-wallet
[explorer]: https://github.com/alephium/explorer
[web3-sdk]: https://github.com/alephium/alephium-web3
[wiki]: https://github.com/alephium/wiki
[awesome]: https://github.com/alephium/awesome-alephium
[mining-discord]: https://alephium.org/discord
[miner-starter-pack]: https://github.com/alephium/alephium-miner-getting-started
[solo-mining-video]: https://www.youtube.com/watch?v=hdPH6inWjhc
[reddit]: https://www.reddit.com/r/Alephium/
[mobile-wallet-repo]: https://github.com/alephium/mobile-wallet
[extension-wallet-repo]: https://github.com/alephium/extension-wallet
[walletconnect-repo]: https://github.com/alephium/walletconnect
[wormhole-fork-repo]: https://github.com/alephium/wormhole-fork
[brand-guide]: https://github.com/alephium/alephium-brand-guide
[reward-grant]: https://github.com/alephium/community/blob/master/Grant%26RewardProgram.md
