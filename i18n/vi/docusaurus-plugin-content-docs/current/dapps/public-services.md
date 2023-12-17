---
sidebar_position: 50
title: Public Services
sidebar_label: Public services
---

## Testnet Faucet

Testnet Faucet là cách để nhận token từ testnet-v1 vào trong ví đã được kết nối.

### Thông qua HTTP API

Một cách khác để được nhận testnet-v16 token là thông qua việc call HTTP, đơn giản hãy để địa chỉ ví của bạn vào phần body của câu lệnh bên dưới:

```
curl -X POST -d '1H1GPLkoMGVUfxQcJgtjWTrKV1KJCQooEV5WxPMhP4Zjy' https://faucet.testnet.alephium.org/send
```

Lưu ý rằng faucet sẽ được điều tiết và cần vài phút để thực. hiện.

## Node và các Explorer API

Hiện tại, các API service bên dưới đang được maintain. Lưu ý rằng tất cả các API là rate limited để tránh việc spam.
* `https://wallet-v20.mainnet.alephium.org` cho mainnet với node v2.X ([Tài liệu](https://wallet-v20.mainnet.alephium.org/docs))
* `https://wallet-v20.testnet.alephium.org` cho testnet với node v2.X ([Tài liệu](https://wallet-v20.testnet.alephium.org/docs))
* `https://backend-v113.mainnet.alephium.org` Cho mainnet với explorer backend v1.13.X ([Tài liệu](https://backend-v113.mainnet.alephium.org/docs))
* `https://backend-v113.testnet.alephium.org` cho testnet với explorer backend v1.13.X ([Tài liệu](https://backend-v113.testnet.alephium.org/docs))

Bởi vì project vẫn đang trong quá trình phát triển, tất cả các API sẽ cần luôn được cập nhật theo các phiên bản. Thông thường, chỉ có những phiên bản mới nhất mới được maintain, nhưng bất kỳ cập nhật của các API sẽ được thông báo một cách chi tiết.

## API Aliases

Chúng tôi maintain các API aliases bên dưới để giúp user có thời gian trong việc di chuyển từ API cũ.

import aliases from "./api-aliases.json";

export const Aliases = ({aliases, type}) => (
    <Box>
        {aliases.length > 0 && <h3>{type} Aliases</h3>}
        <ul>{aliases && aliases.map((alias) => {
            const from = alias['from'];
            const to = alias['to'];
            const additionalPath = from.includes('wallet') ? '/infos/version' : from.includes('backend') ? '/infos' : '';
            return <li key={from}><code>{from}</code> (<a href={`${from}${additionalPath}`}>Test</a>) -> <code>{to}</code></li>;
        })}</ul>
    </Box>
)

<Aliases aliases={aliases['current']} type='Current' />
<Aliases aliases={aliases['deprecated']} type='Deprecated' />

## API rate limiting

Để chắn chắn trong việc có hiệu suất và bảo mật tốt, tất cả các public API của chúng tôi đều được tích hợp rate limiting. Điều này có nghĩa là sẽ có một giới hạn các con số được request mà bạn thực hiện trong một khoản thời gian nhất định. Bởi vì các service của chúng tôi liên tục được phát triển, nên rate limit có thể được điều chỉnh theo lưu lượng sử dụng.

Để đảm bảo cho trải nghiệm sử dụng được diễn ra trơn tru khi đạt đến rate limit, chúng tôi rất khuyến khích các bạn tích trữ cache và thử lại các công cụ khi thực hiện các yêu cầu đến dịch vụ API của chúng tôi. Phản hồi của cache có thể làm giảm số lượng call đến API. Cho nên rất có thể cố thử lại những yêu cầu bị lỗi sẽ dẫn đến các lỗi khác.

Nếu ứng dụng của bạn được tạo với framework của React, bạn có thể mượn chức năng ["SWR"](https://www.npmjs.com/package/swr) với package có sẳn trên npm. SWR cung cấp các hook tiện lợi cho việc fetch data và cache và giúp dễ dang hơn để làm việc với API. Đặc biệt, bạn có thể sử dụng hook `useSWR`  để xử lý các biến dữ liệu và hook `useSWRImmutable` cho các bất biến dữ liệu như là metadata của token. 