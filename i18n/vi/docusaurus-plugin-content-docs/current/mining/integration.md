---
sidebar_position: 50
title: Tích hợp
sidebar_label: Tích hợp 
---

Tài liệu này nhằm mục đích giúp các nhóm thợ đào (mining pools) và thợ đào tích hợp Alephium dễ dàng hơn. Tài liệu chủ yếu bao gồm:

* Hình thức liên lạc giữa các nhóm thợ đào và full node
* Cách để thợ đào tính toán block hash dựa trên nhiệm vụ khai thác.

Về việc triển khai hình thức liên lạc giữa nhóm và các cá nhân thợ đào, bạn có thể tham khảo giao thức theo tầng (stratum protocol) [tại đây](alephium-stratum.md). Lưu ý rằng các nhóm thợ đào không theo giao thức này một cách chính xác. 

Trong tài liệu này, chúng tôi sẽ tham khảo code của [mining-pool](https://github.com/alephium/mining-pool) và [gpu-miner](https://github.com/alephium/gpu-miner). 


## Nhóm các thợ đào (Mining Pool)

Nhóm các thợ đào cần kết nối với Alephium full node để nhận nhiệm vụ khai thác, và máy chủ api để khai thác đuợc mặc định là `localhost:10973`.

Nhóm các thợ đào liên lạc với full node thông qua giao thức binary và thông báo sẽ có format như sau:

```
MessageSize(4 bytes) + Message(1 byte MessageType + Payload)
```

### Nhận nhiệm vụ từ Full Node

Mỗi khi full node nhận một block mới, nó sẽ gửi một `Jobs` message (thông báo nhiệm vụ) đến nhóm các thợ đào. Bạn cũng có thể cài đặt khoảng thời gian trong [cấu hình khai thác](https://github.com/alephium/alephium/blob/master/flow/src/main/resources/system_prod.conf.tmpl#L6) của full node để gửi thông báo nhiệm vụ khi không có block mới. 

Vì hiện tại đang có 16 chain trên Alephium, nên sẽ có 16 block template trong mỗi thông báo nhiệm vụ và một block template này bao gồm các nội dung sau:

* `fromGroup` and `toGroup`: chain index của block template.
* `headerBlob`: dữ liệu nhị phân được tuần tự hoá (the serialized binary data) của [BlockHeader](https://github.com/alephium/alephium/blob/master/protocol/src/main/scala/org/alephium/protocol/model/BlockHeader.scala#L28), ngoại trừ 24 bytes(nonce) đầu tiên.
* `txsBlob`: dữ liệu nhị phân được tuần tự hoá của các giao dịch.
* `targetBlob`: dữ liệu nhị phân được tuần tự hoá của [Target](https://github.com/alephium/alephium/blob/master/protocol/src/main/scala/org/alephium/protocol/model/Target.scala#L32).

Bạn có thể tham khảo code được cung cấp [tại đây](https://github.com/alephium/mining-pool/blob/master/lib/messages.js) để tìm hiểu thêm về `Jobs` message và cách phân tích nó. 

Khi nhóm các thợ đào nhận được `Jobs` message từ full node, nó có thể gửi nhiệm vụ khai thác đến các thợ đào dựa trên hashrate của nó. Việc tính toán nonce cho mỗi chain chỉ yêu cầu `targetBlob` và `headerBlob`. Vì vậy, nhóm các thợ đào có thể tiết kiệm băng thông bằng cách bỏ đi `txsBlob` khi gửi đi nhiệm vụ khai thác cho các thợ đào. Bạn có thể tham khảo code được cung cấp [tại đây](https://github.com/alephium/mining-pool/blob/master/lib/blockTemplate.js#L51).


### Gửi các Block đến Full Node

Khi nhóm các thợ đào nhận được một `nonce` hợp lệ từ thợ đào, nó có thể gửi block đến full node, trong đó block bao gồm `nonce`, `headerBlob` và `txsBlob`, bạn có thể tham khảo code được cung cấp [tại đây](https://github.com/alephium/mining-pool/blob/master/lib/pool.js#L119).

Sau đó bạn có thể tham khảo code [tại đây](https://github.com/alephium/mining-pool/blob/master/lib/daemon.js#L49) để tạo một thông báo `SubmitBlock` hợp lệ và gửi đến full node.

Sau khi full node xác minh block đó, nó sẽ gửi một thông báo `SubmitBlockResult` để cho nhóm thợ đào biết block có hợp lệ hay không và bạn có thể tham khảo code [tại ](https://github.com/alephium/mining-pool/blob/master/lib/messages.js#L72) để phân tích thông báo `SubmitBlockResult`.

## Thợ đào 

### Tính toán BlockHash

Trong Alephium, kích thước của `nonce` là 24 bytes, và hash của block là: `blake3(blake3(serialize(blockHeader))`. Như đã đề cập từ trước, `blockBlob` trong mỗi nhiệm vụ là the serialized binary data của `BlockHeader` ngoại trừ `nonce`. Vì vậy, khi thợ đào tính toán block hash, nó cần thêm `nonce` vào phía trước `headerBlob`, bạn có thể tham khảo code tại [đây](https://github.com/alephium/gpu-miner/blob/master/src/worker.h#L135) và [đây](https://github.com/alephium/gpu-miner/blob/master/src/blake3/original-blake.hpp#L314).

### Kiểm tra ChainIndex

Ngoài việc kiểm tra target, thợ đào cũng cần kiểm tra chain index của block vì Alephium mã hoá chain index thành block hash. Bạn có thể tham khảo code [tại đây](https://github.com/alephium/gpu-miner/blob/master/src/blake3/original-blake.hpp#LL303C2-L303C2) để kiểm tra chain index của block hash đã đúng chưa.
