---
sidebar_position: 30
title: Dapp Recipes
sidebar_label: Dapp Recipes
---

## Contract

### Fetch contract state

Khi sử dụng lệnh `npx @alephium/cli compile`  để compile một contract, nó sẽ tạo ra một TypeScript code dự theo code của contract.
Lấy [TokenFaucet](https://github.com/alephium/nextjs-template/blob/main/contracts/token.ral) contract như ví dụ,
[này](https://github.com/alephium/nextjs-template/blob/main/artifacts/ts/TokenFaucet.ts) sẽ tạo ra TypeScript code.
Bạn có thể sử dụng TypeScript code để fetch cho contract state:

```typescript
import { TokenFaucet } from 'artifacts/ts' // Note that you may need to change the import path according to your project directory structure
import { web3, NodeProvider } from '@alephium/web3'

const nodeUrl = 'http://127.0.0.1:12973'
const nodeProvider = new NodeProvider(nodeUrl)
web3.setCurrentNodeProvider(nodeProvider)

const tokenFaucetAddress = 'y1btMZHTvMvHEqLTdx1JHvEXq3tmVfqsY2rwM669upiT'
const tokenFaucet = TokenFaucet.at(tokenFaucetAddress)
const contractState = await tokenFaucet.fetchState()

// The names in `contractState.fields` are the same as the field names in the TokeFaucet contract
const { symbol, name, decimals, supply, balance  } = contractState.fields

// You can also get the assets owned by the contract
const { alphAmount, tokens } = contractState.asset
```

### Call contract method

Bạn có thể sử dụng TypeScript code để call những contract method, nó giống như `eth_call` của Ethereum:

```typescript
import { TokenFaucet } from 'artifacts/ts'
import { web3, NodeProvider } from '@alephium/web3'

const nodeUrl = 'http://127.0.0.1:12973'
const nodeProvider = new NodeProvider(nodeUrl)
web3.setCurrentNodeProvider(nodeProvider)

const tokenFaucetAddress = 'y1btMZHTvMvHEqLTdx1JHvEXq3tmVfqsY2rwM669upiT'
const tokenFaucet = TokenFaucet.at(tokenFaucetAddress)
const totalSupply = await tokenFaucet.methods.getTotalSupply()
```

### Subscribe to contract events

Trong [TokenFaucet](https://github.com/alephium/nextjs-template/blob/main/contracts/token.ral) contract,
chúng ta đã định lượng [Withdraw](https://github.com/alephium/nextjs-template/blob/c846a675235198045cdf91ba0304aa287f2fc68d/contracts/token.ral#L18) event.
Mỗi khi chức năng `withdraw` được gọi, contract sẽ sinh ra một `Withdraw` event.
Chúng ta có thể subscribe vào withdraw event sử dụng phương pháp sau:

```typescript
import { TokenFaucet, TokenFaucetTypes } from 'artifacts/ts'
import { EventSubscribeOptions } from '@alephium/web3'

// `TokenFaucetTypes.WithdrawEvent` is a generated TypeScript type
const options: EventSubscribeOptions<TokenFaucetTypes.WithdrawEvent> = {
  // We specify the pollingInterval as 4 seconds, which will query the contract for new events every 4 seconds
  pollingInterval: 4000,
  // The `messageCallback` will be called every time we recive a new event
  messageCallback: (event: TokenFaucetTypes.WithdrawEvent): Promise<void> => {
    console.log(`Withdraw(${event.fields.to}, ${event.fields.amount})`)
    return Promise.resolve()
  },
  // The `errorCallback` will be called when an error occurs, here we unsubscribe the subscription and log the error
  errorCallback: (error, subscription): Promise<void> => {
    console.error(error)
    subscription.unsubscribe()
    return Promise.resolve()
  },
  // The `onEventCountChanged` callback is an optional parameter that will be called when the contract event count changes
  onEventCountChanged: (eventCount): Promise<void> => {
  },
}

// We subscribe to contract events starting from event count 0.
// We can also persist the current event count within the `onEventCountChanged` callback,
// allowing us to subscribe from the last event count for the next subscription.
const fromEventCount = 0
const subscription = tokenFaucet.subscribeWithdrawEvent(options, fromEventCount)

// Unsubscribe the subscription
subscription.unsubscribe()
```

## Giao dịch

### Query transaction status

Bạn có thể query một trạng thái giao dịch bằng cách sử dụng đoạn mã bên dưới:

```typescript
import { NodeProvider } from '@alephium/web3'

const nodeUrl = 'http://127.0.0.1:12973'
const nodeProvider = new NodeProvider(nodeUrl)

const txId = '919d4e4b1080d74beb56a1f78ea7c0569a358e3ea3988058987cc1addf4b93cc'
const txStatus = await nodeProvider.transactions.getTransactionsStatus({ txId })
```

Bạn có thể làm các trạng thái giao dịch khác đi bằng cách sử dụng `txStatus.type`:

1. `MemPooled`: có nghĩa là giao dịch đang trong mempool
2. `Confirmed`: giao dịch đã được xác nhận, và bạn có thể lấy các xác nhận bằng `txStatus.chainConfirmations`
3. `TxNotFound`: giao dịch không tồn tại

## Hooks

`@alephium/web3-react` package cung cấp một vài hook để tạo điều khiện cho việc phát triển giao diện frontend cho người dùng.

### useWalletConfig

```typescript
import { useWalletConfig } from '@alephium/web3-react'

export function Component() {
  const { network, setNetwork, addressGroup, setAddressGroup } = useWalletConfig()

  return <div>
    <button onClick={() => setNetwork('testnet')}>Network: {network}</button>
    <button onClick={() => setAddressGroup(3)}>Address group: {addressGroup}</button>
  </div>
}
```

`useWalletConfig` trả về những thiết lập của nút connect và chức năng tiện ích để cập nhật cho những thiết lập đó.

### useWallet

```typescript
import { useWallet, Wallet } from '@alephium/web3-react'

function Component() {
  const { account, connectionStatus } = useWallet()

  if (connectionStatus === 'connecting') return <div>Connecting</div>
  if (connectionStatus === 'disconnected') return <div>Disconnected</div>

  // connected
  return <div>{account}</div>
}
```

Nếu giá trị kết quả trả về `undefined`, nó cho thấy rằng ví đang chưa được kết nối. Kết quả của ví nên được trả về với những trường thông tin sau:

* `wallet.signer`: bạn có thể sử dụng signer để ký cho các giao dịch
* `wallet.account`: đây là tài khoản hiện tại đang được kết nối
* `wallet.nodeProvider`: bạn có thể sử dụng node provider để giao tiếp với full node, lưu ý giá trị được trả về có thể là `undefined`

### useBalance

```typescript
import { useBalance } from '@alephium/web3-react'

const { balance, updateBalanceForTx } = useBalance()
```

`useBalance` hook trả về hai giá trị:

1. `balance`: số dư hiện tại của tài khoản đang được kết nối
2. `updateBalanceForTx`: được dùng để cập nhật số dư khi user thực hiện một giao dịch. Nó sẽ lấy id của giao dịch như là một tham số và nó sẽ cập nhật số dư khi giao dịch được xác nhận.

### useTxStatus

```typescript
import { useState } from 'react'
import { useTxStatus } from '@alephium/web3-react'

const { txStatus } = useTxStatus(txId)
const confirmed = useMemo(() => {
  return txStatus?.type === 'Confirmed'
}, [txStatus])
```

`useTxStatus` hook cũng chấp nhận một tham số optional callback của `(txStatus: node.TxStatus) => Promise<any>`, nó sẽ được call sau mỗi trạng thái giao dịch dc query.

## Tiện ích

### Rate limit

`NodeProvider` được dùng để giao tiếp với full node khi tạo dApp,
và bạn có thể sử dụng public [API services](./public-services.md) được cung cấp bởi Alephium. 
Nhưng tất cả các API đều có chỉ số giới hạn để ngăn chặn việc spam. Vì vậy nếu client gửi quá nhiều request cho một số lượng nhất định tại cùng một thời điểm, nó sẽ trả về HTTP 429 error.

Bạn có thể sử dụng [fetch-retry](https://github.com/jonbern/fetch-retry) để giải quyết vấn đề này:

```typescript
import * as fetchRetry from 'fetch-retry'

// We specify up to 10 retries, with 1 second retry delay
const retryFetch = fetchRetry.default(fetch, {
  retries: 10,
  retryDelay: 1000
})
const nodeProvider = new NodeProvider('node-url', undefined, retryFetch)
```

### Tuỳ chỉnh nút kết nối của ví

`@alephium/web3-react` cung cấp `AlephiumConnectButton` component đê tạo điều kiện cho việc xây dựng giao diện người dùng,
bạn cũng có thể sử dụng `AlephiumConnectButton.Custom` để tuỳ chỉnh kiểu của nút connect:

```typescript
import { AlephiumConnectButton } from '@alephium/web3'

function CustomWalletConnectButton = () => {
  return (
    <AlephiumConnectButton.Custom>
      {({ isConnected, disconnect, show, account }) => {
        return isConnected ? (
          <button onClick={disconnect}>
            Disconnect
          </button>
        ) : (
          <button onClick={show}>
            Connect
          </button>
        )
      }}
    </AlephiumConnectButton.Custom>
  )
}
```
