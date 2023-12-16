---
sidebar_position: 1
slug: /frequently-asked-questions
sidebar_label: FAQ
title: Những câu hỏi thường gặp
---

import UntranslatedPageText from "@site/src/components/UntranslatedPageText";

:::info
📚 Tìm hiểu nhanh tất cả về Alephium tại [5 Phút Tổng Quan](/).
:::

Trước khi bắt đầu, chúng tôi khuyến kích các bạn tham khảo qua những nguồn thông tin dưới đây:

- [Website chính thức](https://alephium.org)
- [X (Twitter) chính thức](https://twitter.com/alephium)
- [Discord chính thức](https://alephium.org/discord)
- [Telegram chính thức](https://t.me/alephiumgroup)
- [Reddit chính thức](https://reddit.com/r/Alephium)
- [Medium chính thức](https://medium.com/@alephium), nổi bật:
  - [Tokenomics của Alephium](https://medium.com/@alephium/tokenomics-of-alephium-61d59b51029c)
  - [Block Rewards của Alephium](https://medium.com/@alephium/alephium-block-rewards-72d9fb9fde33)
  - [Chương trình phần thưởng cho cộng đồng Alephium](https://medium.com/@alephium/introducing-community-rewards-f4638bbf14bf)
  - [Siêu hướng dẫn về Proof-of-Less-Work và những điều cần biết](https://medium.com/@alephium/tech-talk-1-the-ultimate-guide-to-proof-of-less-work-the-universe-and-everything-ba70644ab301)
  - [Giới thiệu về sUTXO](https://medium.com/@alephium/an-introduction-to-the-stateful-utxo-model-8de3b0f76749)
  - [ALPHred, Virtual Machine](https://medium.com/@alephium/meet-alphred-a-virtual-machine-like-no-others-85ce86540025)
  - [Cập nhật Leman Network Upgrade!](https://medium.com/@alephium/the-leman-network-upgrade-is-live-f52c89b7dd6a)

## Chain Data

### Có bao nhiêu ALPH đang lưu thông?

Bạn có thể tìm thông tin về cung lưu thông trên Alephium [Explorer](https://explorer.alephium.org) hoặc sử dụng [circulating ALPH endpoint](https://backend.mainnet.alephium.org/infos/supply/circulating-alph).

### Làm thế nào để tính cung lưu thông?

Nó được tính dựa trên cách tính của [CoinMarketCap](https://support.coinmarketcap.com/hc/en-us/articles/360043396252-Supply-Circulating-Total-Max-).

Số cung lưu thông là tổng số ALPH đang hiện hữu trừ cho:

- Số lượng token (đã khoá & khả dụng) của các thành viên từ private sale, dự án và những địa chỉ của đội ngũ quản lý. Theo CMC: _"tokens are generally only counted as circulating after they leave the original reserve wallet (i.e. outbound transfers are more representative of an intent to bring the coin into circulation rather than a mere unlock)"_.
- Tất cả ALPH đã khoá từ khắp các địa chỉ khác.

### Tôi có thể kiểm tra số lượng của các địa chỉ ví và những địa chỉ ví nào đang nằm trong top nắm giữ?

Để kiểm tra số lượng của bất kỳ một địa chỉ ví nào, hãy truy cập [Explorer](https://explorer.alephium.org).  
Ngoài ra, các thành viên trong cộng đồng cũng đã tự tạo ra một trang web có thể giúp bạn kiểm tra top các địa chỉ ví:

- https://alph-richlist.vercel.app/
- https://alph-top.web.app/

### Phí giao dịch tối thiểu là bao nhiêu?

Hiện tại, phí giao dịch tối thiểu được đặt ở mức is `0.002` ALPH để ngăn chặn tấn công mạng DoS. Trong tương lai, phí giao dịch tối thiểu có thể sẽ thấp hơn, với mức thấp nhất có thể là `0.00000000000001` ALPH trên Alephium. Con số phí giao dịch tối thiểu phụ chính xác sẽ thuộc vào số lượng UTXO được thêm vào và những người tham gia ký xác nhận giao dịch. 

### Tốc độ giao dịch tối đa (TPS) trên Alephium là bao nhiêu?

Tại mạng Mainnet của Alephium hiện tại có thể đặt hơn 400 TPS với 16 shard. Nó hoàn toàn có thể được nâng lên 10k TPS bằng việc gia tăng số shard cần thiết.
Đọc thêm về [the concept of TPS](https://medium.com/@alephium/transactions-per-second-tps-f13217a49e39).

### Con số biểu thị nhỏ nhất (denomination) của Alephium là bao nhiêu?

Alephium cho phép lên đến 18 chữ số thập phân và con số biểu thị nhỏ nhất đó (denomination) được gọi là Phi. Một Phi tương đương `0.000000000000000001` ALPH, hoặc `10^-18` ALPH, trong khi 1 ALPH bằng `10^18` Phi.

## dApps

### Có sàn DEX nào trên Alephium không?

Alephium cung cấp [DEX prototype](https://alephium.github.io/alephium-dex/#/swap) chạy trên Testnet. Tìm hiểu thêm về bài viết [DEX prototype](https://medium.com/@alephium/dex-prototype-live-on-testnet-bac5e7d095ce).

[DEX contracts](https://github.com/alephium/alephium-dex/tree/master/contracts) đã được thông thử nghiệm và kiểm tra độ tin cậy và hiệu quả. Nó dễ dàng có thể được fork và sử dụng cho bất kỳ dự án nào.

### CÓ dApp nào trên Alephium không?

Tất cả các ứng dụng được xây dựng trên Alephium đều được liệt kê trên [Awesome Alephium repository](https://github.com/alephium/awesome-alephium). Để tham gia vào đóng góp, hãy gửi Pull Request của bạn đến chúng tôi!

Alephium đang trong gia đoạn rất trẻ và những infrastructure của chúng tôi (bao gồm một [bridge](https://github.com/alephium/wormhole-fork)) cùng với những tài liệu có sẳn sẽ góp phần giúp mọi người phát triển các dApp trở nên dễ dàng hơn. Chúng tôi vẫn không ngừng liên tục cập nhật và phát triển.
Alephium có một số [prototype](https://docs.alephium.org/dapps/ecosystem#prototypes) luôn được duy trì cập nhật tốt nhất. Bạn có thể sử dụng chúng như kim chỉ nam cho dự án của mình.

Nếu bạn muốn xây dựng một dApp, hãy đọc qua tài liệu [Hướng Dẫn Bắt Đầu Xây Dựng dApps](https://docs.alephium.org/dapps/getting-started).

### Tại sao dApp chỉ có thể kết nối với một trong số địa chỉ ví?

Alephium vận hành như một blockchain với các shard, nơi mà những địa chỉ ví và các công đoạn của contract được tổ chức trong nhiều nhóm (group). Khi nó kết nối đến các dApp, chúng nó có thể được deploy vào bất kỳ nhóm nào. Tuy nhiên, có một nhược điểm — dApp chỉ có thể được sử dụng vào các địa chỉ trên cùng một group.

Vì vậy, khi kết nối đến một dApp, ứng dụng sẽ yêu cầu cụ thể được kết nối với những địa chỉ nào thuộc cùng chung một group giống như group của dApp đó.

Hãy lưu ý rằng đây là nhược điểm duy nhất khi sử dụng dApp. Những giao dịch này sẽ có trải nghiệm giống như một blockchain không có các shard. Đội ngũ phát triển chính cam kết. sẽ cải thiện việc quản lý tài sản đa group nhằm giúp người dùng sử dụng dApp dễ dàng hơn.

## Development

### Tôi có thể tìm thấy roadmap ở đâu?

Bạn có xem qua roadmap tại [website](https://alephium.org/#next) và trang tài liệu [docs](https://docs.alephium.org/#roadmap). Đừng quên theo dõi cập nhật hằng tuần của Alephium tại [Discord](https://alephium.org/discord), [Twitter](https://twitter.com/alephium) or [Reddit](https://www.reddit.com/r/Alephium/search?q=flair_name%3A%22Development%22&restrict_sr=1).

### Tôi có thể theo dõi trạng thái maintain công khai của Alephium ở đâu?

Bạn có thể theo dõi tại đây:

- Mạng mainnet https://status.mainnet.alephium.org
- Mạng testnet https://status.testnet.alephium.org

### Tôi có thể query một API ở đâu?

Để có thể query một API, bạn cần chạy full node ([hướng dẫn tại đây](https://wiki.alephium.org/full-node/Full-Node-Starter-Guide)).  
Alephium sử dụng OpenAPI để tương tác full node. Bạn có thể trực tiếp khởi chạy local Swagger UI thông qua `127.0.0.1:12973/docs` khi full node của bạn chạy thành công.  
Hoặc, hoặc bạn có thể sử dụng bất kỳ OpenAPI client nào. để thêm [openapi.json](https://raw.githubusercontent.com/alephium/alephium/master/api/src/main/resources/openapi.json) file từ repository của Alephium.

### Tôi muốn tìm hiểu về Chương trình Phần thưởng, Tài trợ và Đóng góp?

Alephium đang diễn ra các chương trình ấy tại [Grants and Rewards program](https://github.com/alephium/community/blob/master/Grant%26RewardProgram.md). Bạn có thể sẽ nhận được phần thưởng khi tham gia đóng góp ở bấy kỳ khía cạnh nào của chúng tôi.

### Những dự án nào đang được xây dựng trên Alephium?

Hầu hết các dự án đang được xây dựng trên Alephium cho đến nay đã được liệt kê trên [Awesome Alephium repository](https://github.com/alephium/awesome-alephium). Nếu bạn muốn cho chúng tôi biết dự án của mình, hãy tạo một Pull Request để thêm sản phẩm của bạn vào trong danh sách.

## Full node

### Có bất kỳ phần thưởng nào cho việc chạy full-node?

Alephium sử dụng cơ chế đồng thuận Proof of (Less) Work. Điều này có nghĩa là sẽ không có phần thưởng nào cho việc chạy full node, không như các blockchain trên Proof of Stake. Tuy nhiên, việc bạn chạy full node mang lại nhiều lợi ích như đóng góp vào sự phi tập trung, xác nhận các giao dịch một cách độc lập, bảo mật hơn, và tự chủ tài sản của mình. Cụm từ _"not your node, not your network"_ nhấn mạnh tầm quan trọng của việc tự chạy. full node, bởi vì phải đặt niềm tin vào các node do bên thứ ba nào đó để tương tác trong blockchain nghĩa là bạn phải đặt niềm tin vào ai đó. Mặc dù kết nối với các node được vận hành bởi các bên thứ ba nói chung là an toàn, một vài người vẫn thích tự thiết lập và chạy full node của họ nhằm tối ưu mức độ tin cậy và quyền riêng tư.

### Tôi cần những gì để chạy full-node?

Chạy full-node của Alephium rất nhẹ và có thể được chạy trên. hầu hết các thiết bị bao gồm Raspberry-Pi hoặc điện thoại. Để thiết lập và khởi chạy node của bạn, hãy xem qua [Hướng dẫn thiết lập Full-node](https://docs.alephium.org/full-node/getting-started/).

### Tôi có thể stake trên Alephium không?

Alephium không cung cấp chương trình staking trên blockchain, bởi vì nó không vận hành trên cơ chế đồng thuận PoS. Tuy nhiên, tham gia vào DeFi thông qua các liquidity pool có thể giúp bạn stake tài sản của mình trong tương lai.

## Niêm yết & Sàn giao dịch

### Mã thông báo là gì?

Mã thông báo (token ticker) của Alephium là [ALPH](https://medium.com/@alephium/introducing-alph-8381dbd9f88d).

### Mất bao lâu để các sàn giao dịch hiển thị số lượng tôi đã nạp vào?

Các sàn giao dịch yêu cầu số confirmation cao cho cơ chế PoW để đảm bảo an toàn. Hiện tại, một số sàn giao dịch yêu cầu từ 30 đến 60 confirmation cho giao dịch nạp Alephium, trong khoản 30 phút đến một giờ.

### Những sàn giao dịch nào đang niêm yết Alephium?

Bạn có thể tìm danh sách các sàn giao dịch đang niêm yết Alephium trên [CoinMarketCap](https://coinmarketcap.com/currencies/alephium/markets/) hoặc [CoinGecko](https://www.coingecko.com/en/coins/alephium).

## Mining

### Phần thưởng khai thái (Mining Reward) là gì?

Xem qua tài liệu [Giải thích chi tiết về block reward của Alephium](https://medium.com/@alephium/alephium-block-rewards-72d9fb9fde33).

### Tại sao phải giữ block reward trong 500 phút?, có phải block time chỉ có 64 giây?

Khoá 500-phút được thiết kế để chống lại tấn công re-org (re-org attack), với Bitcoin là khoảng 1000 minute bảo vệ khi một block reward được tìm thấy.

### Tại sao tôi phải có 4 địa chỉ ví cho việc khai thác khai thái (mining address)?

Alephium là một blockchain với các shard cụ thể là với các `G` group và các `G*G` shard. Bởi vì với thiết kế này, mỗi group yêu cầu các địa chỉ ví khai thác của chính nó.

Hiện tại, Alephium có 4 group và 16 shard on trên Mainnet. Vì vậy, cần phải có 4 địa chỉ ví khai thác, mỗi một địa chỉ cho mỗi group.

### Có bao nhiêu coin được khai thác mỗi ngày?

Để hiểu được có bao nhiêu coin được khai thác trong một ngày, bạn cần sử dụng công thức bên dưới. Bởi vì phần block reward thay đổi linh hoạt theo từng block nên công thức sẽ chỉ cung cấp cho bạn giá trị gần đúng.

```
3600 giây / 64 giây (Alephium block time) == 56.25 block trên một giờ, trên một shard.
56.25 x 16 shard == 900 block tổng công trên một giờ.
900 x 24 giờ == 21600 block trên một ngày.
21600 x  ALPH reward trên mỗi block ~= số ALPH được đào ra trong môt ngày.
```

Tại thời điểm bài viết này được cập nhật lần cuối, trung bình block reward là: `2.87` ALPH, nghĩa là khoảng `61'992` ALPH được khai thác trong một ngày.

### Những mỏ đào nào tôi có thể sử dụng để khai thác ALPH?

Bên dưới là danh sách những mỏ đào được biết đến của Alephium. Xin lưu ý danh sách này có thể sẽ không đầy đủ bởi vì các thợ đào liên tục xuất hiện, hãy gửi Pull Request để được thêm vào danh sách.

- https://www.bzminer.com/guides/how-to-mine-alephium/
- https://lolminer.site/
  https://github.com/Lolliedieb/lolMiner-releases
- https://www.srbminer.com/
  https://github.com/doktor83/SRBMiner-Multi/releases
- https://trex-miner.com/
  https://github.com/trexminer/T-Rex

Bạn cũng có thể sử. dụng tài liệu [Alephium's gpu-miner](https://github.com/alephium/gpu-miner) nhưng nó có thể không hiệu quả bằng danh sách bên trên.

## Tech

### Tại sao các bạn lại thiết kế thêm một blockchain L1? Đã có quá nhiều mô hình như vậy rồi?

Lịch sử cho thấy blockchain đã và đang không ngừng phát triển cải tiến để trở thành một giải pháp đủ đạo cho nhiều lĩnh vực. Với sự thay đổi không ngừng nghỉ, hầu hết các dự án đang mang lại những giá trị cốt lõi về sự phi tập trung, tự chủ tài sản, bảo mật và khả năng mở rộng mạng lưới để đáp ứng cho các nhu cầu cần thiết cho các ứng dụng. Alephium mang lại những giá trị tương tự đó, nó được thiết kế để thu hút sự quan tâm trong đa lĩnh vực với nền tảng (s)UTXO và Po(L)W. Chúng tôi dẫn đầu cho xu hướng kết hợp nền tảng DeFi với UTXO và ứng dụng phi tập trung với smart contract.

Thêm vào đó, Alephium có những đặc điểm công nghệ để thúc đẩy sự phát triển cho các dự án như:

1. Khả năng mở rộng lớn thông qua các shard
2. Có rất nhiều blockchain Layer 1 (L1) với việc sử dụng quá nhiều tài nguyên dẫn đến chi phí vận hành full node tăng cao và gián tiếp làm giảm sự phi tập trung. Cách vận hành của Alephium tương tự như của Bitcoin, người dùng có thể tự chạy full node và xác nhận trên mạng lưới. _"Don't trust, verify."_
3. Nhiều blockchain L1 sử dụng mô hình account (account model) hoặc là tương thích với EVM và điều này có thể thừa hưởng những yếu điểm. Alephium tạo ra một Virtual Machine (VM) mới với cách. hoạt động dựa trên mô hình Unspent Transaction Output (UTXO) nhằm cung cấp một mô hình lập trình mới với mức độ bảo mật cao cho các ứng dụng phi tập trung (dApps).
4. Hầu hết các blockchain L1 mới sử dụng cơ chế đồng thuận Proof of Stake (PoS). Alephium đã chọn xây dựng trên Proof of Work (PoW) đơn giản hơn, phù hợp hơn và cơ chế đồng thuận mạnh mẻ hơn để tối ưu tính phi tập trung.

### Alephium có hỗ trợ smart contract hay không?

Có, Alephium có hỗ trợ smart contract. Nó được đặc biệt thiết kế cho các dApp với sự mở rộng và an toàn trên mạng lưới.

### Tại sao blocktime là 64 giây? Có lý do cụ thể nào cho việc này?

Thực thi trên blockchain Proof-of-Work (PoW) được dựa vào khối lượng công việc được tính trên mỗi block mới thay vì là block time. Nghĩa là nếu một giao dịch cần N block thì sẽ cần được xác nhận với block time là T. Và giao dịch sẽ phải cần 2N block để được xác nhận nếu block time giảm xuống còn một nữa là T/2. Điều này sẽ dẫn đến cùng một khoảng thời gian để xác nhận giao dịch.

Trong khi khi block time ngắn hơn có thể mang lại trải nghiệm dùng tốt hơn, nhưng điều này cũng mang lại nhiều bất lợi:

- Nhiều orphan block được tạo ra. Tỷ lệ uncle block trên PoW của Ethereum là 10% hoặc cao hơn, trong khi tỉ lệ orphan block của Bitcoin là thấp hơn 1%.
- Tăng chi phí trên P2P network. Vấn đề này nghiêm trọng trong blockchain PoS vì các báo cáo cho thấy có hơn 90% giao dịch trên Solana là các thông báo xác thực của validator.

Để đảm bảo tiết kiệm dung lượng và đảm bảo hiệu quả cho các chain trong dài hạn, những đặc điểm này cần được phòng tránh. Nên vậy, Alephium bắt đầu với mỗi blocktime là 64 giây, nhằm tạo ra sự cân bằng giữ blockchain của Bitcoin với những blockchain mới với thời gian mỗi khối ngắn hơn.

Dành cho những người có sự ưu tiên cho block time và thực thi giao dịch, giải pháp Layer 2 có thể được xây dựng trên Alephium và block time có thể được giảm trong tương lai khi blockchain được tích cực phát triển và tốc độ internet được tăng lên.
Tối ưu, kích thước nhẹ, khả năng mở rộng, và sự hiểu quả cho Layer 1 là điều thiết yếu cho thế giới tài chính phi tập trung.

### Mất bao lâu cho một giao dịch được?

1 giây là đủ để thấy cho một giao dịch đang diễn ra ở trong mempool. Block time của Alephium hiện đang là 64 giây. Những quyết định mang tính chất kinh tế phụ thuộc vào số lượng. và khả năng quản lý rủi ro của bạn. Cho một giao dịch nhỏ, mempool có lẽ là vừa đủ, và 1-4 block sẽ đủ cho hầu hết các giao dịch. Tuy nhiên, nếu bạn là một người giao dịch với số lượng lớn, bạn cần phải chờ vài chục đến vài trăm block.

Bạn có thể tìm thêm thông tin chi biết về concept của block time và thời gian thực thi tại các bài viết này:

- [Block time & block size](https://medium.com/@alephium/block-time-and-block-size-16e37292444f)
- [Time to finality](https://medium.com/@alephium/time-to-finality-17d64eeffd25)

### Tại sao các bạn chọn POLW mà không phải là POS?

Công nghệ blockchain đang trong giai đoạn rất sớm và câu hỏi thường gặp là cấu trúc blockchain nào cần thiết cho 10 năm tiếp theo để có thể hỗ trợ tốt cho các dApp và DeFi.

Alephium được xây dựng với niềm tin rằng một blockchain có thể mở rộng với tốc độ cao, phí giao dịch thấp, khả năng có thể lập trình được như trên Ethereum và độ tin cậy cộng với bảo mật như Bitcoin. Mục tiêu của chúng tôi là tạo ra một giải pháp "mở rộng Bitcoin với smart contract tinh cậy".

Theo hiệu ứng Lindy, mặc dù cơ chế PoS đang rất thành công, mô hình của Bitcoin với các shard của PoW vẫn là giải pháp mạnh mẽ và mang lại sự phi tập trung cho việc xây dựng một blockchain có tín mở rộng. Những đặc điểm chính:

- PoW đơn giản, mạnh mẽ và dễ dàng thiết kế các thuật toán sharding
- PoS vẫn cần thời gian để thử nghiệm lại nhiều và tồn động nhiều vấn đề sau khi Ethereum chuyển sang PoS
- PoS có thể được cho rằng ít tính phi tập trung và làm giảm tính ẩn danh
- PoS có thể khiến người dùng phải tự tin tưởng vào những người chạy xác nhận giao dịch (validator) hơn bởi vì giá để chạy một node đang dần tăng lên cao
- PoS có thể sẽ làm giảm tính bảo mật cho DeFi bởi những cuộc tấn công như MEV

### Stateful UTXO là gì nó có những đặc điểm gì khác nhau so với những mô hình UTXO khác?

Có hai loại trạng thái (state) trong công nghệ blockchain: mutable state (như trên Ethereum) và immutable state (như trên UTXO hoặc eUTXO). Mutable state thì linh hoạt và dễ tương tác hơn, điều này được chứng tỏ thông qua sự phát triển tích cực của hệ sinh thái trên Ethereum. Tuy nhiên, UTXO model cung cấp những lợi ích bảo mật vốn có.

[Stateful UTXO model của](https://medium.com/@alephium/an-introduction-to-the-stateful-utxo-model-8de3b0f76749) kết hợp hai lợi ích bên trên lại. Nó hỗ trợ các mutable state như trên Ethereum và cho smart contract, trong khi vẫn thừa hưởng những lợi ích về bảo mật của UTXO model cho các loại tài sản.

### Alephium có gặp phải vấn đề tương tranh tương tự như mô hình UTXO cổ điển và mở rộng có thể dẫn đến TPS thấp không??

Không, Alephium không có những giới hạn đó. Stateful UTXO model của Alephium kết hợp UTXO model truyền thống với account model và hỗ trợ các mutable state. Điều này đảm bảo rằng các dApp có thể truy cập các mutable contract state một cách độc lập, loại bỏ mọi khả năng xảy ra sự cố đồng thời.

### Tại sao không phải là 1 triệu shard?

Mạng lưới chính có thể bị tắt nghẽn khi gia tăng số shard. Mỗi node cần sử dụng `2G - 1` shard cho sự ổn định. Nếu băng thông trung bình của mạng lưới ổn định, `G` có thể được điều chỉnh ở mức cao như 32. Khi một vài chi phí khác được tính vào, mạng lưới sẽ bị tắt nghẽn nhanh chóng.

### Làm gì để tăng số shard trên Alephium?

Một bản nâng cấp mạng lưới sẽ có thể gia tăng số shard. Bản nâng cấp có thể diễn ra nếu số shard đang tồn tại không đủ để duy trình băng thông trên toàn hệ thống.

### Có khả năng nào khiến cho một mạng lưới với các shard, như là Alephium, bị tấn công khi hashrate thấp hơn 51%? Ví dụ như bằng cách xâm phạm chỉ một group hoặc một shard?

Những lo ngại về bảo mật có thể nảy sinh trong các sharded blockchain nếu chúng không được thiết kế đúng cách, như Vitalik giải thích về định nghĩa "tấn công 1%" của anh ấy. Giải pháp sharding của Ethereum giải quyết vấn đề này bằng các trộn các xác nhận từ validator.

Mặc khác, Alephium giải quyết vấn đề này với thuật toán Blockflow. Việc khai thác trên các shard khác nhau được tích luỹ bởi sự độc lập của các block. Khi ai đó muốn tấn công, họ sẽ phải cố tổ chức lại shard đồng nghĩa rằng phải tổ chức lại toàn bộ các block độc lập.
Một cách trực quan và đơn giản nhất để xem điều này là hợp nhất tất cả các shard lại với nhau.

### Có bất kỳ cross shard atomicity nào cho các token và smart contract trên Alephium không?

Trên Alephium, token có thể kế hợp các across shard ở mức độ rất nhỏ, nghĩa là vẫn có khả năng để di chuyển một cách rất nhỏ các token từ một shard này sang một shard khác.
Tuy nhiên, trong các smart contract có token và các state component ở trong stateful UTXO model của Alephhium, chỉ có các  token mới có cross-shard atomicity; các state được phân chia và do đó chúng không thể kết hợp lại được. Thiết kế này phản ánh Alephium lấy token làm trung tâm và cho phép thiết kế các state đơn giản hơn giống như một database được phân vùng. Sự cân bằng này thuận lợi hơn so với xu hướng Layer 2 hiện tại các mà thiếu token atomicity và hiện không có giải pháp thực tế nào cho khả năng kết hợp các state hoàn chỉnh.

### Có flash loan nào khả dụng trên Alephium không?

Không, flash loan không khả dụng bởi thiết kế trên [Alephium's virtual machine, Alphred](https://medium.com/@alephium/meet-alphred-a-virtual-machine-like-no-others-85ce86540025).

### Địa chỉ ví trên Alephium được tạo ra như thế nào? Có các nào để phân biệt giữa địa chỉ Bitcoin legacy và địa chỉ Alephium?

Alephium sử dụng curve giống như trên Bitcoin (secp256k1 curve) để tạo ra các địa chỉ, nhưng khác thuật toán hash (blake2b). Tuy nhiên, địa chỉ ví trên Alephium thường sẽ dài hơn địa chỉ ví trên Bitcoin bởi vì nó sử dụng một hash 32-byte nâng cấp từ hash 20-byte.

### Tôi có thể sử dụng địa chỉ ví trên mainnet cho testnet?

Các địa chỉ ví được tự tạo ra bởi một thuật toàn và là mạng duy nhất (testnet, mainnet, devnet, v.v). Không cần thiết để kết nối đến một mạng lưới node (thậm chí là internet) để tạo ra một địa chỉ ví. Một cách thiết yếu, mỗi địa chỉ ví trên Alephium tồn tại trong tất cả các mạng, thậm khi khi chúng nó chưa được tạo hoặc tìm thấy.

Trong các mạng lưới cryoto trước đó, những giao dịch đã không chứa bất kỳ thông tin nào và có thể được "sử dụng lại" vào một mạng khác. Vì vậy, người ta không khuyến khích sử dụng một địa chỉ ví giống nhau cho các mạng khác nhau. Nhưng trong Alephium chứa network ID in trong chính các giao dịch nên có khả năng sử dụng cùng một địa chỉ ví cho nhiều mạng khác nhau.  
Khi bạn liên kết ví của bạn vào trong một mạng, như testnet, bạn có thể yêu cầu một testnet node để kiểm tra số dư trên địa chỉ ví. Nếu bạn thay đổi kết nối mạng trong phần cài đặt sang mainnet, một mainnet node sẽ hhieenr thị số dư địa chỉ ví trên mạng mainnet. Vì vậy, mỗi địa chỉ ví sẽ có số dư khác nhau trên mỗi mạng khác nhau, và bạn có thể xem số dư của địa chỉ ví trên từng mạng bằng cách kết nối đúng vào mạng đó.

### Tại sao Alephium chọn tự phát triển một virtual machine và ngôn ngữ lập trình riêng cho smart contract?

[stateful UTXO model](https://medium.com/@alephium/an-introduction-to-the-stateful-utxo-model-8de3b0f76749) nơi mà Alephium đặt nền tảng thì hoàn toàn theo lý thuyết và không. tương thích với những virtual machine đang có như EVM (cái được thiét kế cho account model). Điều này thúc đẩy cho quyết định tự tạo ra một virtual machine mới [Alphred](https://medium.com/@alephium/meet-alphred-a-virtual-machine-like-no-others-85ce86540025) và virtual machhine này mang lại nhiều thế mạnh từ sUTXO.

 Giống như trên EVM với ngôn ngữ là Solidity, Alphred có cấu trúc ngôn ngữ chủ đạo là Ralph. Ralph được thiết kế đặc biệt cho blockchain Alephium để cực kỳ tương thích và dễ sử dụng. Nó được tạo để gia tăng khả năng bảo mật và có những tính năng tương tự như của VM.

Bằng việc tự tạo ra VM và ngôn ngữ cho smart contract, Alephium có thể đề xuất một phiên bản thay thế giảm thiểu một vài vấn đề bảo mật được biết đến của Solidity và EVM. Thêm vào đó, trải nghiệm sử dụng cho các lập trình viên là tiêu chí hằng đầu khi thiết kế Alphred và Ralph nhằm hỗ trợ cho các lập trình viên dễ dàng bắt đầu xây dựng các dự án.

### Alephium có thể chống lại sự tấn công từ các máy tính lượng tử hay không?

Giống như Bitcoin và Ethereum, Alephium không cho rằng máy tính lượng tử là một vấn đề quan ngại cấp bách. Các thuật toán hash, sign và cấu trúc địa chỉ có thể được cập nhật. Vấn đề từ máy tính lượng tử có thể được giải quyết khi điều này thực sự diễn ra mà dần trở nên là một vấn đề gây hại đến người dùng.

## Tokenomics

### Giá GAS thấp nhất có thể là bao nhiêu?

Giá gas thấp nhất có thể hiện tại đang là `10^-7` ALPH hoặc `0.0000001` ALPH.

### Alephium có lịch trình giảm phát hay không? Alephium có halving hay không?

Alephium không có halving như Bitcoin. Lịch giảm phát phụ thuộc vào hasrate và timestamp của mạng lưới. Phần thưởng khai thác luôn được thay đổi với mỗi block. Bạn có thể tìm hiểu chi tiết qua bài viết này:

- [Block Reward](https://medium.com/@alephium/alephium-block-rewards-72d9fb9fde33)
- [Proof of Less Work](https://medium.com/@alephium/tech-talk-1-the-ultimate-guide-to-proof-of-less-work-the-universe-and-everything-ba70644ab301)

### Nếu các token được đốt (burn) thì trong tương lai có khi nào số lượng ALPH đang tồn tại sẽ về 0?

Theo lý thuyết là có. Dự đoán trong tương lai 10 năm tiếp theo là khó xãy ra và nếu nói 80 tiếp theo là rất khó. Blockchain như Alephium không có những chính sách thông thường như lịch giảm phát để phục vụ cho việc tiến hoá của công nghệ. Nếu một sự đồng thuận là cần thiết để thay đổi giảm phát, điều này mới có thể khiến cho các quyết định được thực thi.

### Giới hạn nguồn cung tối đa được tính như thế nào?

Giới hạn nguồn cung tối đa 1 tỷ ALPH là một con số ước tính. Giao thức đã dự tính ra số lượng cung có thể được sinh ra dựa vào timestamp sẽ rơi vào khoảng 80 năm nữa. Điều này do việc tính toán con số tổng cung cho một chuỗi shard trên toàn mạng lưới là rất tốn kém. Tốc độ phát hành cung được xác định theo thời gian và thay đổi tuỳ thuộc theo tốc độ của hash rate.
Điều đáng chú ý là giới hạn 1 tỷ đã được ước tính trước khi triển khai chính sách cải tiến [DAA](https://github.com/alephium/alephium/blob/master/docs/proposals/lemanDAA.md). Với mã nguồn hiện tại, số cung tối đa có thể dự tính ra được sẽ nhỏ hơn 1 tỷ trong 80 năm, thậm chí chưa đến việc đốt phí giao dịch của cơ chế POLW.

## Ví

### Alephium đã có những loại ví cá nhân nào rồi?

Alephium hiện tại đang cung cấp:

- một [desktop wallet](https://github.com/alephium/desktop-wallet/releases/latest)
- một [web extension wallet](https://github.com/alephium/extension-wallet) available on [Chrome](https://chrome.google.com/webstore/detail/alephium-extension-wallet/gdokollfhmnbfckbobkdbakhilldkhcj) and [Firefox](https://addons.mozilla.org/en-US/firefox/addon/alephiumextensionwallet/)
- một [mobile wallet](https://github.com/alephium/mobile-wallet) hiện đang được phát triển.

Ngoài những ví chính thức, có một số ví được phát triển thông qua bên thứ ba.

### Alephium đã có kế hoặch cho việc hỗ trợ hardware wallet chưa?

Hỗ trợ hardware wallet là ưu tiên của Alephium. Một tích hợp vào Ledger đang được phát triển và khả dụng cho chế độ developer với web extension wallet `v0.7.0`.
Phiên bản hỗ trợ chính thức cho Ledger đang được triển khai và mất nhiều giai đoạn thử nghiệm nhưng sẽ sớm được ra mắt.

### Khi nhập seed vào trong desktop wallet, có cách nào để tôi nhập lại tất cả những địa chỉ đã tạo trước đó?

Khi nhập lại ví thông qua cụm từ khôi phục, ví sẽ ngay lập tức kết nối với mạng để tìm tất cả các ví đã hoạt động trước đó. Ví đã hoạt động trước đó phải có ít nhất một giao dịch. Để thêm thủ công các ví bạn đã từng tạo trước đó, hãy tìm đến chổ Ví và nhấn vào biểu tượng cài đặt kế bên nút "+ Thêm địa chỉ ví mới" trên Desktop wallet, nó sẽ hiển thị lại tất cả địa chỉ ví đã hoạt động trước đó (được lưu trữ cùng với cụm từ khôi phục).

### Desktop wallet thu thập những thông tin nào?

Alephium luôn ưu tiên sự an toàn, bảo mật và trải nghiệm người dùng lên hằng đầu. Kích hoạt phân tích có thể giúp chúng tôi cải thiện trải nghiệm người dùng mà không làm ảnh hưởng đến thông tin cá nhân của bạn. Thông tin được thu thập bởi Desktop wallet là hoàn toàn ẩn danh. Khi lần đầu tiên bạn chạy ví của mình, một ID riêng biệt sẽ được sinh ra (ví dụ như, `vCJGCsDPrZ8WJaIKZMWjU`) là thông tin duy nhất được thu thập. Địa chỉ IP hoặc những thứ khác [thông tin cá nhân](https://posthog.com/blog/what-is-personal-data-pii) sẽ không được thu thập. Chỉ có những lần nhấn vào các nút, số lượng ví, địa chỉ, danh bạ và các thiết lập cho ví sẽ được ghi lại. Những thông tin này giúp chúng tôi cải thiện các tính năng và trải nghiệm người dùng.
Alephium là dự án có mã nguồn mở, nơi người dùng có thể vào để xem những gì đang diễn ra với các sự kiện được ghi lại bởi [searching for the `posthog?.capture` keyword](https://github.com/search?q=repo%3Aalephium%2Fdesktop-wallet+posthog?.capture&type=code).

### Tại sao có 0.001 ALPH token được thêm vào giao dịch của tôi mỗi khi tôi gửi các token?

`0.001` ALPH là yêu cầu tối thiểu trên một UTXO để tránh UTXO spamming. Số lượng này sẽ không được sử dụng bởi mạng lưới và nó sẽ xuất hiện cho địa chỉ đích đến, như các token.

### Tại sao việc sao lưu cụm từ khôi phục bí mật là rất quan trọng?

Sao lưu cụm từ khôi phục bí mật là điều cực kỳ cần thiết bởi vì cụm từ này như là chìa khoá cho ví của bạn. Nếu bạn mất khả năng truy cập vào ví (ví dụ như mất máy, có sự cố, hoặc xoá ứng dụng)  
Nếu không có cụm từ khôi phục, bạn sẽ không có khả năng try cập vào tất cả tài sản đã được lưu trữ trên ví đó vĩnh viễn.

## Những thứ khác

### Tôi có thể tìm thấy những nội dung được dịch sang ngôn ngữ khác ở đâu?

Bạn có thể tìm rất nhiều nội dung được dịch sang các ngôn ngữ khác của Medium, X(Twitter) và Youtube.

Trên X (Twitter), đây là những cộng đồng theo các quốc gia:

- [German](https://twitter.com/Alephiumde)
- [French](https://twitter.com/Alephiumfr)
- [Bulgarian](https://twitter.com/alephiumbg)
- [Indonesian](https://twitter.com/Alephium_id)

Người biên dịch được khuyến khích sử dụng các cấu trúc hashtag khi xuất bản nội dung của họ: #Alephium\[i18n\]
Bạn có thể tìm các phiên bản được dịch trên các nên tảng Medium, X (Twitter) và các channel khác với những hashtag:

- Tây Ban Nha: "#AlephiumES"
- Bồ Đào Nha: "#AlephiumPT"
- Pháp: "#AlephiumFR"
- Đức: "#AlephiumDE"
- Bulgarian: "#AlephiumBG"
- Tiếng Việt: "#AlephiumVN"

Trên [Discord server](https://alephium.org/discord), Alephium cũng có tạo những ngôn ngữ riêng để thảo luận.

Trên Telegram, những nhóm được quản lý các thành viên trong cộng đồng:

- [Đức](https://t.me/alphgermanofficial)
- [Việt Nam](https://t.me/alephiumvn)
- [Nga](https://t.me/alephiumgroup_ru)
- [Bồ Đào Nha](https://t.me/Alephium_pt)
- [Thổ Nhĩ Kỳ](https://t.me/alephiumturkiye)
- [Hà Lan](https://t.me/AlephiumgroupNL)
- [Trung Quốc](https://t.me/alephiumCN)

### Tin tức cập nhật?

Xem những cập nhật mới nhất trên channel [Discord](https://discord.gg/AFXKJNVFKJ) và [Telegram](https://t.me/Alephium_Announcement).
Những bản tin cập nhật hằng tuần trên [Discord](https://alephium.org/discord), [Reddit](https://www.reddit.com/r/Alephium) & [Twitter](https://twitter.com/alephium).

### Why is the project named Alephium?

Tên của Alephium xuất phát từ "Aleph". Theo Wikipedia: *"Aleph numbers are a sequence of numbers used to represent the cardinality of infinite sets that can be well-ordered. They were introduced by the mathematician Georg Cantor and are named after the symbol he used to denote them, the Hebrew letter aleph (ℵ)."*

Nếu bạn để ý, logo của Alephium được thiết kế dựa trên ký tự Aleph.

Như một sự bắt nhịp với Ethereum, Alephium được đặt để tạo sự thuận tiện cho việc gọi tên.

### Cập nhật Leman là gì?

Hoạt động vào ngày 30, tháng 3, năm 2023, [Cập nhật Leman](https://medium.com/@alephium/the-leman-network-upgrade-is-live-f52c89b7dd6a) là bản cập nhật hệ thống đầu tiên của Alephium. Đó là thành quả xứng đáng trong suốt một năm làm việc và đóng góp không ngừng nghỉ của đội ngũ và những tác giả đóng góp, và nó đại diện cho sự tăng trưởng qua các cột mốc của dự án. Đây là bước đi đầu tiên theo sau sự phát triển của hệ sinh thái Alephium, với nhiều tính năng mới, cung cấp trải nghiệm phát triển và sử dụng cho người dùng và lập trình viên để xây dựng những ứng dụng phi tập trung.

### Tôi có thể tìm hiểu mọi thứ về Alephium trong 5 phút hay không?

Bạn có thể tìm hiểu tổng quan thông qua tài liệu [docs](https://docs.alephium.org/) và những nguồn tham khảo thêm luôn có sẳn trên FAQ
### WHEN MOON?

1ALPH luôn là 1ALPH.
