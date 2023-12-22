---
sidebar_position: 20
title: Hướng dẫn khai thác theo nhóm
sidebar_label: Hướng dẫn khai thác theo nhóm  
---

# Hướng dẫn khai thác theo nhóm (Pool Mining) 

Bạn sẽ tìm thấy một danh sách hoàn chỉnh của các nhóm thợ đào (mining pools) có tiếng [tại đây](#community-pools)

### Tạo nhóm riêng của bạn 

Nếu bạn muốn tổ chức nhóm các thợ đào của riêng bạn, vui lòng tham khảo [repo tại đây](https://github.com/alephium/mining-pool/). Nếu bạn tạo một nhóm mới, vui lòng gửi Pull Request để thêm nhóm của bạn vào [danh sách](#community-pools).

### Mẫu cấu hình node cho nhóm thợ đào:

> Phần này dành cho người quản lý nhóm, không dành cho thợ đào.

```
// more connections for better block propagation
alephium.network.external-address = "<public IP for discovery>:9973"
alephium.network.max-outbound-connections-per-group = 48
alephium.network.max-inbound-connections-per-group  = 256

alephium.mining.miner-addresses = [4 miner addresses]

// comment out the following 2 lines if you don't access rest api from external network
alephium.api.network-interface = "0.0.0.0"
alephium.api.api-key = "<api key>"
```

## Các nhóm của cộng đồng 

### ⚠️ Cảnh báo
Đây là danh sách không đầy đủ các nhóm khai thác được quản lý bởi cộng đồng. Những nhóm này không được kiểm định bởi Alephium và Âlephium không chịu trách nhiệm về sự lựa chọn nhóm của bạn. Mặc dù các nhóm được liệt kê ở đây cho đến nay vẫn hoạt động tốt như mong đợi, nhưng hãy lưu ý rằng việc lựa chọn nhóm yêu cầu bạn phải thực hiện một số nghiên cứu về tính bảo mật, danh tiếng và sự an toàn chung của nhóm. Bạn nên bắt đầu từ việc [tham khảo cộng đồng trên Discord](https://alephium.org/discord)

### Sử dụng nhóm và cách để nhận hỗ trợ 

Chúng tôi rất vui khi thấy danh sách nhóm ngày càng phát triển và đa dạng hoá. Hãy tham gia và thử trải nghiệm! Nếu nhóm đã quá đông thành viên, hãy cân nhắc chuyển sang một khác để gia tăng tính phi tập trung. 

Alephium vẫn còn rất mới và điều quan trọng là bạn phải theo dõi các cập nhật từ Alephium, cũng như các phần mềm cần thiết để tham gia các nhóm khai thác. Hãy nhớ rằng mỗi một nhóm đều có một cộng đồng phát triển trên các nền tảng khác nhau luôn sẵn sàng tiếp nhận các yêu cầu và câu hỏi của bạn. Thế nên, tốt nhất bạn nên đặt các câu hỏi liên quan đến nhóm của mình trong nhóm có liên quan. Và bạn cũng có thể tìm thấy hướng dẫn bằng ngôn ngữ của bạn tại một trong những [playlist cộng đồng](https://www.youtube.com/channel/UCIX9Eww2Kch7sc0E6gCmEdg/playlists) của chúng tôi. 

### Các nhóm hiện đang hoạt động và được biết đến

Dưới đây là danh dách các nhóm thợ đào được sắp xếp theo bảng chữ cái. Chúng tôi khuyến khích bạn [gửi pull request](https://github.com/alephium/wiki/tree/master/docs/mining/pool-mining-guide.md) để thêm các nhóm sắp ra mắt vào wiki này và/hoặc để báo cáo sự biến mất của chúng, cũng như các hành vi có nguy cơ sai phạm quy định. 

Bạn cũng có thể tham khảo [https://miningpoolstats.stream/alephium](https://miningpoolstats.stream/alephium).

#### Alephium-pool (Nhóm cộng đồng)

- Website: https://alephium-pool.com/
- Telegram: https://t.me/alephium_pool
- Discord: https://discord.gg/ZXYU2NGx

#### ALPH.city

- Website: https://alph.city/
- Telegram: https://t.me/alphcity

#### ALPH-pool.com

- Website: https://alph-pool.com/
- Telegram: https://t.me/ALPH_pool_chat

#### Alph2Mine.com

- Website: https://alph2mine.com/
- Email: alph2mine@gmail.com

#### Coinhunters Pool

- Website: https://alph.coinhunters.space
- Telegram (EN): https://t.me/alph_coinhunters_en
- Telegram (RU): https://t.me/alph_gravitsapapool_ru

#### e4pool ALPH Pool 

- Website: https://e4pool.com/alph
- Telegram: https://t.me/E4piko
- Support: https://t.me/e4pool_howto
- Forum: https://forum.e4pool.com/

#### Enigma Pool

- Website: https://enigmapool.com/
- Discord: https://discord.com/invite/enigmapool
- Calculator: https://enigmapool.com/tools/calculator

#### Herominers Pool

- Website: https://alephium.herominers.com/
- Discord: https://discord.com/invite/gvWSs84
- Telegram: https://t.me/HeroMinersPool

#### Metapool (Community pool)

- Website: https://www.metapool.tech
- Calculator: https://metapool.tech/dashboard#calculator
- Telegram: https://t.me/metapool1
- Discord: https://discord.gg/5TTzMDzJ

#### Okminer 

- Website (CN): https://okminer.com
- Calculator: https://okminer.com/tools
- Telegram (CN): https://t.me/okminer_CN
- Telegram (EN): https://t.me/okminer_support

#### p1pool.com

- Website: https://p1pool.com/
- Telegram: https://t.me/p1pool_com
- Discord: https://discord.gg/U8dh97XHk8
- Email: info@p1pool.com

#### Soloblocks

- Website: https://soloblocks.org/alph/

#### Solopool.org

- Website: https://alph.solopool.org/
- Telegram: https://t.me/solopool_org
- Twitter: https://twitter.com/solopool_org
- Email: support@solopool.org

#### Wooly Pooly

- Website: https://woolypooly.com/en/coin/alph
- Calculator: https://woolypooly.com/en/calc/what-to-mine-gpu
- Discord: https://woolypooly.com/discord
- Telegram: https://woolypooly.com/telegram

⚠️ **Hãy đảm bảo rằng bạn cập nhật phiên bản mới nhất của phần mềm khai thác**

### Alephium GPU-Miner

- Download: [https://github.com/alephium/gpu-miner](https://github.com/alephium/gpu-miner)
- Support: [https://alephium.org/discord](https://alephium.org/discord)

### bzMiner

- Download: [https://www.bzminer.com/](https://www.bzminer.com/)
- Support: [https://discord.gg/NRty3PCVdB](https://discord.gg/NRty3PCVdB)

### lolMiner

- Download: [https://lolminer.site/download/](https://lolminer.site/download/)

### T-Rex

- Download: [https://trex-miner.com/](https://trex-miner.com/)

### SRBMiner

- Download: [https://www.srbminer.com/download.html](https://www.srbminer.com/download.html)

## Nếu bạn có những câu hỏi liên quan đến HiveOS or RaveOS, những thông tin sau sẽ giúp bạn

### Hive OS

- Website: [https://hiveos.farm](https://hiveos.farm)
- Forum: [https://hiveon.com/forum/](https://hiveon.com/forum/)
- Telegram: [https://t.me/hiveoschat_en](https://t.me/hiveoschat_en)
- Discord: [https://discord.gg/CVZeZdn](https://discord.gg/CVZeZdn)

### Rave OS

- Mail support: support@raveos.com
- Website: [https://raveos.com/](https://raveos.com/)
- Telegram: [https://t.me/raveossupport](https://t.me/raveossupport)
- Discord: [https://discord.gg/Dcdadz2](https://discord.gg/Dcdadz2)

### OKMiner Mobile OS 

- iOS: [https://apps.apple.com/ru/app/okminer-os/id1494087547](https://apps.apple.com/ru/app/okminer-os/id1494087547)
- Android: [https://downap.okminer.com/hashapk/okminer.apk](https://downap.okminer.com/hashapk/okminer.apk)
- Website (CN): https://okminer.com
- Calculator: https://okminer.com/tools
- Telegram (CN): https://t.me/okminer_CN
- Telegram (EN): https://t.me/okminer_support
