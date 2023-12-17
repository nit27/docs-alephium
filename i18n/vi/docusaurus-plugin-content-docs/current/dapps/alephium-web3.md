---
sidebar_position: 25
title: Web3 SDK
sidebar_label: Web3 SDK
---

## Cài đặt

```
npm install --save @alephium/web3
```

## Kết nối đến Alephium

`NodeProvider` là một abstraction của việc kết nối đến mạng Alephium, bạn có thể lấy một `NodeProvider` bằng:

```typescript
const nodeProvider = new NodeProvider('http://localhost:22973')
```

Hoặc chỉ định `API_KEY` nếu bạn có `alephium.api.api-key` trong tệp cài đặt full node:

```typescript
const API_KEY = // alephium.api.api-key from your full node config
const nodeProvider = new NodeProvider('http://localhost:22973', API_KEY)
```

Thỉnh thoảng, sẽ thuận tiện hơn khi cài đặt sẳn global `NodeProvider` cho dự án của bạn:

```typescript
web3.setCurrentNodeProvider(<nodeURL>)
```

## Querying trên Blockchain

Một khi bạn đã có `NodeProvider`, bạn có thể kết nối đến blockchain để query cho contract state hiện tại, lấy các lịch sử của contract, tìm các deployed contract và hơn thế nữa.

```typescript
// Get the blockchain height from the given chain index
await nodeProvider.blockflow.getBlockflowChainInfo({
  fromGroup: 0,
  toGroup: 0
})
// { currentHeight: 315 }

// Get the block from the given block hash
await nodeProvider.blockflow.getBlockflowBlocksBlockHash('1ccfe845988ebf878384dd2dc9e55920261566c2ad9143963180222059ffd3b0')
// {
//   hash: '1ccfe845988ebf878384dd2dc9e55920261566c2ad9143963180222059ffd3b0',
//   timestamp: 1665207807876,
//   chainFrom: 0,
//   chainTo: 0,
//   height: 315,
//   deps: [
//     '5e9209fbf2b4c656b136933684b8606382575f16795ddc7a6317c5d4b7e378c5',
//     'b5d69f03d4d0eea5a4c7e0bc0e2e8c5a8f99306050d641b599991b441ea0f9ea',
//     'f344b3dd45f39d62cfd200cfa3312c080018102908787c5565b8c8af3647368f',
//     'fa359ce0b5accb1584cd280ae3c32f210424a9fd32cb736b6cdfe64882651010',
//     '8bc7d94ddfc66129049862e501ff5d5042e1e978b1a51d28e238321444a91071',
//     '5896ee3cbc8c4f4432a0b221039113061b6cf7eeb794b25758d247120d0df712',
//     'fc312b0c04a0eeb0767ec98137b740047d7c5e8f64463531828b7015cbeaeaa3'
//   ],
//   transactions: [
//     {
//       unsigned: [Object],
//       scriptExecutionOk: true,
//       contractInputs: [],
//       generatedOutputs: [],
//       inputSignatures: [],
//       scriptSignatures: []
//     }
//   ],
//   nonce: '5da513de7183d7eb1a90b46c4ffe6f7ae4fdf012f0018f41',
//   version: 0,
//   depStateHash: '2edccc3e717d38a6564fb970f6714f0c00c074226b2cb1ee6272781d3c9bc870',
//   txsHash: '738e879791e0c164a5a12b658e2b37e65ed9c415c2e152656c8469273f775f5a',
//   target: '20ffffff'
// }

// Get the transaction status
await nodeProvider.transactions.getTransactionsStatus({
  txId: 'f33da0d8f4c00d68e2d5818cb9617219a1108b801f387fc8d1595287e4dbf2aa'
})
// {
//   type: 'Confirmed',
//   blockHash: '47c95e02a7d7ca442ee1849d76a5987c7b72ddcad0b05e9f01df4bc4878f5980',
//   txIndex: 0,
//   chainConfirmations: 295,
//   fromGroupConfirmations: 295,
//   toGroupConfirmations: 295
// }

// Get the account balance
await nodeProvider.addresses.getAddressesAddressBalance('1DrDyTr9RpRsQnDnXo2YRiPzPW4ooHX5LLoqXrqfMrpQH')
// {
//   balance: '999972904436900000000000',
//   balanceHint: '999972.9044369 ALPH',
//   lockedBalance: '0',
//   lockedBalanceHint: '0 ALPH',
//   tokenBalances: [
//     {
//       id: 'ee682f8182f56b55a36a7f916a901685752e00d0e4b90de073e6658156ae82a9',
//       amount: '10000000000000000000'
//     }
//   ],
//   utxoNum: 1
// }
```

## Ghi vào trong blockchain

Các giao dịch sẽ được dùng để thay đổi trạng thái trên blockchain. Mỗi một giao dịch cần phải được ký xác nhận với private key, nó có thể thực hiện thông qua `SignerProvider`. Và hai `SignerProvider` trong `alephium/web3-wallet`.

### Cài đặt Ví Web3

```
npm install --save @alephium/web3-wallet
```

:::note
Tất cả các ví được sử dụng cho việc xây dựng và tích hợp vào contract, vui lòng không sử dụng chúng để lưu trữ số lượng lớn các token.
:::

### NodeWallet

Tham khảo thêm tại [hướng dẫn này](/wallet/node-wallet-guide) để tạo một full node wallet.

```typescript
// Create a node wallet by wallet name
const nodeWallet = new NodeWallet('alephium-web3-test-only-wallet', nodeProvider)

// Unlock the wallet with password
await nodeWallet.unlock('alph')

// Get accounts
await nodeWallet.getAccounts()
// [
//   {
//     publicKey: '0381818e63bd9e35a5489b52a430accefc608fd60aa2c7c0d1b393b5239aedf6b0',
//     address: '1DrDyTr9RpRsQnDnXo2YRiPzPW4ooHX5LLoqXrqfMrpQH',
//     group: 0
//   }
// ]

// Transfer 1 ALPH to 15Z54erRksUHb7qxegcKN5DePMv96tXdc1jW26fW3REwT
await nodeWallet.signAndSubmitTransferTx({
  signerAddress: '1DrDyTr9RpRsQnDnXo2YRiPzPW4ooHX5LLoqXrqfMrpQH',
  destinations: [{
    address: '15Z54erRksUHb7qxegcKN5DePMv96tXdc1jW26fW3REwT',
    attoAlphAmount: 10n ** 18n,
  }]
})
// {
//   txId: '7d11b0e88f0158cdd94fcf4e7af04a76be1101293a50050523f72422a9269f36',
//   fromGroup: 0,
//   toGroup: 0
// }

// Lock the wallet
await nodeWallet.lock()
```

### PrivateKeyWallet

```typescript
// Create a PrivateKeyWallet from private key
const wallet = new PrivateKeyWallet('a642942e67258589cd2b1822c631506632db5a12aabcf413604e785300d762a5', undefined, nodeProvider)

// Create a PrivateKeyWallet from mnemonic and group, here it will create an account on group 0
const wallet = PrivateKeyWallet.FromMnemonicWithGroup(
  'vault alarm sad mass witness property virus style good flower rice alpha viable evidence run glare pretty scout evil judge enroll refuse another lava',
  0,
  undefined,
  undefined,
  undefined,
  nodeProvider
)
console.log(wallet.account)
// {
//   address: '1DrDyTr9RpRsQnDnXo2YRiPzPW4ooHX5LLoqXrqfMrpQH',
//   publicKey: '0381818e63bd9e35a5489b52a430accefc608fd60aa2c7c0d1b393b5239aedf6b0',
//   group: 0
// }

// Transfer 1 ALPH to 15Z54erRksUHb7qxegcKN5DePMv96tXdc1jW26fW3REwT
await wallet.signAndSubmitTransferTx({
  signerAddress: wallet.account.address,
  destinations: [{
    address: '15Z54erRksUHb7qxegcKN5DePMv96tXdc1jW26fW3REwT',
    attoAlphAmount: 10n ** 18n
  }]
})
// {
//   txId: '9a23eab3796a56f538a4574d617b667fb9187721c8df9f39ac89d878cb9755a0',
//   fromGroup: 0,
//   toGroup: 0
// }
```

## Contract

Giống như Ethereum, một contract là một abstraction của program code, nó chạy ở trong Alephium blockchain. Hãy xem qua ví dụ minh hoạ để tìm hiểu về cách test, deploy và call một contract, hướng dẫn tại [đây](/dapps/getting-started) để tạo một project.

### Test một contract
#### Unit tests

SDK cung cấp các chức năng unit test, nó call các contract như một giao dịch bình thường, nhưng thay vì thay đổi các state trên blockchhain, nó trả về một contract state mới, những output của giao dịch, và các event.

```typescript
web3.setCurrentNodeProvider('http://localhost:22973')
const wallet = new PrivateKeyWallet('a642942e67258589cd2b1822c631506632db5a12aabcf413604e785300d762a5')
// Build the project first
await Project.build()

// Test the `withdraw` method of the `TokenFaucet` contract, it will NOT change the blockchain state
const testContractAddress = randomContractAddress()
// The `TokenFaucet` is generated in the getting-started guide
const result = await TokenFaucet.tests.withdraw({
  address: testContractAddress,
  // Initial state of the test contract
  initialFields: {
    symbol: Buffer.from('TF', 'utf8').toString('hex'),
    name: Buffer.from('TokenFaucet', 'utf8').toString('hex'),
    decimals: 18n,
    supply: 10n ** 18n,
    balance: 10n
  },
  // Assets owned by the test contract before a test
  initialAsset: {
    alphAmount: 10n ** 18n,
    tokens: [{
      id: binToHex(contractIdFromAddress(testContractAddress)),
      amount: 10n
    }]
  },
  // Arguments to test the target function of the test contract
  testArgs: { amount: 1n },
  // Assets owned by the caller of the function
  inputAssets: [{
    address: wallet.account.address,
    asset: { alphAmount: 10n ** 18n }
  }]
})

const contractState = result.contracts[0] as TokenFaucetTypes.State
expect(contractState.address).toEqual(testContractAddress)
```

Bạn nên tham khảo qua ví dụ hoàn chỉnh tại [`alephium-nextjs-template`](https://github.com/alephium/nextjs-template/blob/main/test/unit/token.test.ts)

#### Integration test

Bên cạnh các unit test, bạn có thể chạy vài integration test, hãy cẩn thận vì chúng nó có thể thay đổi blockchain state.

```typescript
web3.setCurrentNodeProvider('http://127.0.0.1:22973', undefined, fetch)
await Project.build()

const accounts = signer.getAccounts()
const account = accounts[0]
const testAddress = account.address
await signer.setSelectedAccount(testAddress)
const testGroup = account.group

const deployed = deployments.getDeployedContractResult(testGroup, 'TokenFaucet')
const tokenId = deployed.contractInstance.contractId
const tokenAddress = deployed.contractInstance.address

const faucet = TokenFaucet.at(tokenAddress)
const initialState = await faucet.fetchState()
const initialBalance = initialState.fields.balance

// Call `withdraw` function 10 times
for (let i = 0; i < 10; i++) {
  await Withdraw.execute(signer, {
    initialFields: { token: tokenId, amount: 1n },
    attoAlphAmount: DUST_AMOUNT * 2n
  })

  //!!! Blockchain state is changed !!!
  const newState = await faucet.fetchState()
  const newBalance = newState.fields.balance
  expect(newBalance).toEqual(initialBalance - BigInt(i) - 1n)
}
```

Thông tin tham khảo thêm tại [integration test folder](https://github.com/alephium/nextjs-template/blob/integration-test/test/integration)

### Deploy một contract

```typescript
web3.setCurrentNodeProvider('http://localhost:22973')
const wallet = new PrivateKeyWallet('a642942e67258589cd2b1822c631506632db5a12aabcf413604e785300d762a5')
await Project.build()

// Create a transaction to deploy the contract and submit the transaction to the Alephium network:
// `initialFields` is required if the contract has fields
// `initialAttoAlphAmount` must be greater than or equal to 1 ALPH, assets will be sent to the contract from the transaction sender account
// `issueTokenAmount` specifies the amount of tokens to issue
const issueTokenAmount = 10n
const deployResult = await TokenFaucet.deploy(wallet, {
  initialFields: {
    symbol: Buffer.from('TF', 'utf8').toString('hex'),
    name: Buffer.from('TokenFaucet', 'utf8').toString('hex'),
    decimals: 18n,
    supply: issueTokenAmount,
    balance: issueTokenAmount
  },
  initialAttoAlphAmount: 10n ** 18n,
  issueTokenAmount: issueTokenAmount
})
console.log(JSON.stringify(deployResult, null, 2))
// {
//   "signature": "ea95754bae7935311acf15d3323293f03bce89bb6c82939427da5e3074f0ada93b0cda24138dcec1de4e21a1a66dc1f0c8e99297e6ff8fe10587d1821cbae23f",
//   "fromGroup": 0,
//   "toGroup": 0,
//   "unsignedTx": "000401010103000000091500bee85f379545a2ed9f6cceb331288842f378cf0f04012ad4ac8824aae7d6f80a13c40de0b6b3a7640000a2144055050609121b4024402d404a010000000102ce0002010000000102ce0102010000000102ce0202010000000102ce0302010000000102a0000201020101001116000e320c7bb4b11600aba00016002ba10005b416005f14160403025446030b546f6b656e4661756365740212020a140301020a130aae188000dfdfc1174876e8000137a444479fa782e8b88d4f95e28b3b2417e5bc30d33a5ae8486d4a8885b82b224259c1e6000381818e63bd9e35a5489b52a430accefc608fd60aa2c7c0d1b393b5239aedf6b000",
//   "gasAmount": 57311,
//   "gasPrice": "100000000000",
//   "txId": "c9adbbc02f34d2b3f2db8790354bca9f3d6f7a1fde9b9269a393d6693663c084",
//   "contractAddress": "v8uU9yLfzUwqpJeS9Kd76DB75WJBcEuMdYgEQ2Gn8pLF",
//   "groupIndex": 0,
//   "contractId": "1581cc793fc7bafce6ef89eaf66404c9eec17561ef0e169ef2a4329c9f190c00",
//   "instance": {
//     "address": "v8uU9yLfzUwqpJeS9Kd76DB75WJBcEuMdYgEQ2Gn8pLF",
//     "contractId": "1581cc793fc7bafce6ef89eaf66404c9eec17561ef0e169ef2a4329c9f190c00",
//     "groupIndex": 0
//   }
// }

// Get the contract state
const tokenFaucet = deployResult.instance
const contractState = await tokenFaucet.fetchState()
console.log(JSON.stringify(contractState, null, 2))
// {
//   "address": "v8uU9yLfzUwqpJeS9Kd76DB75WJBcEuMdYgEQ2Gn8pLF",
//   "contractId": "1581cc793fc7bafce6ef89eaf66404c9eec17561ef0e169ef2a4329c9f190c00",
//   "bytecode": "050609121b4024402d404a010000000102ce0002010000000102ce0102010000000102ce0202010000000102ce0302010000000102a0000201020101001116000e320c7bb4b11600aba00016002ba10005b416005f",
//   "initialStateHash": "236a82352f5e34f813ecf274385912ed0ba67f1c305c24f7a6934c18d32213b1",
//   "codeHash": "641343b4f1c08b03969b127b452acc7535cad20231bc32af6c0b5f218dd8ff0c",
//   "fields": {
//     "symbol": "5446",
//     "name": "546f6b656e466175636574",
//     "decimals": "18",
//     "supply": "10",
//     "balance": "10"
//   },
//   "fieldsSig": {
//     "names": [
//       "symbol",
//       "name",
//       "decimals",
//       "supply",
//       "balance"
//     ],
//     "types": [
//       "ByteVec",
//       "ByteVec",
//       "U256",
//       "U256",
//       "U256"
//     ],
//     "isMutable": [
//       false,
//       false,
//       false,
//       false,
//       true
//     ]
//   },
//   "asset": {
//     "alphAmount": "1000000000000000000",
//     "tokens": [
//       {
//         "id": "1581cc793fc7bafce6ef89eaf66404c9eec17561ef0e169ef2a4329c9f190c00",
//         "amount": "10"
//       }
//     ]
//   }
// }
```

Dựa vào output cho ta thấy bạn đã thành công deploy contract, và có 10 token trong contract asset.

### Call một contract

Bạn có thể sử dụng script bên dưới để call các contract trên Alephium blockchain, script code sẽ được thực thi khi giao dịch được gửi vào trong mạng lưới của Alephium, nhưng script code sẽ không được lưu vào trong state của blockchain.

```typescript
web3.setCurrentNodeProvider('http://localhost:22973')
const wallet = new PrivateKeyWallet('a642942e67258589cd2b1822c631506632db5a12aabcf413604e785300d762a5')
await Project.build()

// Contract address from the deploy result
const contractAddress = deployResult.instance.address

// Contract id from the deploy result
const contractId = deployResult.instance.contractId

// Create a call contract transaction, `initialFields` is required if the script has fields
// The `Withdraw` is generated in the getting-started guide
const executeResult = await Withdraw.execute(wallet, {
  initialFields: {
    token: contractId,
    amount: 1n
  }
})
console.log(JSON.stringify(executeResult, null, 2))
// {
//   "signature": "16ec5eed788bfe7a803ec89f47d8ef7c1ac5f626ad88e4b46ecdde8be9fdef5719db49b09ef4e11a94901082e34c52629aafad4b9753483bf853ff695b19b9a4",
//   "fromGroup": 0,
//   "toGroup": 0,
//   "unsignedTx": "0004010101030000000513010d0c1440201581cc793fc7bafce6ef89eaf66404c9eec17561ef0e169ef2a4329c9f190c0001058000a447c1174876e8000137a44447f11525fe8e58af5a02b2f81bbbf35ea3c1bdf319ffe76794709c9e9d4e80c599000381818e63bd9e35a5489b52a430accefc608fd60aa2c7c0d1b393b5239aedf6b000",
//   "gasAmount": 42055,
//   "gasPrice": "100000000000",
//   "txId": "c29e9cb10b3e0b34979b9daac73151d98ee4de8f913e66aa0f0c8dc0cb99a617",
//   "groupIndex": 0
// }

// Get the account balance
const balance = await wallet.nodeProvider.addresses.getAddressesAddressBalance(wallet.account.address)
console.log(JSON.stringify(balance, null, 2))
// {
//   "balance": "999998990063400000000000",
//   "balanceHint": "999998.9900634 ALPH",
//   "lockedBalance": "0",
//   "lockedBalanceHint": "0 ALPH",
//   "tokenBalances": [
//     {
//       "id": "1581cc793fc7bafce6ef89eaf66404c9eec17561ef0e169ef2a4329c9f190c00",
//       "amount": "1"
//     }
//   ],
//   "utxoNum": 2
// }
```

### Query historic contract event

Contract event được index bởi địa chỉ contract với các offset, và bạn có thể query historic event của một địa chỉ contract by bằng cách thêm offset và limit (tuỳ chọn).

```typescript
const nodeProvider = new NodeProvider('http://localhost:22973')
// Contract address from the contract deploy result
const contractAddress = deployResult.instance.address

// Query contract events from index 0, and the `limit` cannot be greater than 100
const result = await nodeProvider.events.getEventsContractContractaddress(
  contractAddress, {start: 0, limit: 100}
)

// In the next query you can start with `result.nextStart`
console.log(JSON.stringify(result, null, 2))
// {
//   "events": [
//     {
//       "blockHash": "0c07e672c40629a5c943cc7e4ec677140cbd50fdc9dcacb6a1d30bc38c0e7b50",
//       "txId": "c29e9cb10b3e0b34979b9daac73151d98ee4de8f913e66aa0f0c8dc0cb99a617",
//       "eventIndex": 0,
//       "fields": [
//         {
//           "type": "Address",
//           "value": "1DrDyTr9RpRsQnDnXo2YRiPzPW4ooHX5LLoqXrqfMrpQH"
//         },
//         {
//           "type": "U256",
//           "value": "1"
//         }
//       ]
//     }
//   ],
//   "nextStart": 1
// }

// Sometimes, events might be emitted from non-canonical blocks because of block reorg, you can check if the block in the main chain
await nodeProvider.blockflow.getBlockflowIsBlockInMainChain({blockHash: events[0].blockHash})
// true

// Get the current contract event counter
await nodeProvider.events.getEventsContractContractaddressCurrentCount(contractAddress)
// 1

// You can also get events by transaction id if `alephium.node.event-log.index-by-tx-id` is enabled in your full node configuration file
await nodeProvider.events.getEventsTxIdTxid('c29e9cb10b3e0b34979b9daac73151d98ee4de8f913e66aa0f0c8dc0cb99a617')
// {
//   "events": [
//     {
//       "blockHash": "0c07e672c40629a5c943cc7e4ec677140cbd50fdc9dcacb6a1d30bc38c0e7b50",
//       "contractAddress": "v8uU9yLfzUwqpJeS9Kd76DB75WJBcEuMdYgEQ2Gn8pLF",
//       "eventIndex": 0,
//       "fields": [
//         {
//           "type": "Address",
//           "value": "1DrDyTr9RpRsQnDnXo2YRiPzPW4ooHX5LLoqXrqfMrpQH"
//         },
//         {
//           "type": "U256",
//           "value": "1"
//         }
//       ]
//     }
//   ]
// }
```

### Listening to events

Một cách khác để query event từng cái một, bạn có thể lấy các event bằng event subscription. Nó sẽ query một cách định kỳ và lấy event mới.

```typescript
web3.setCurrentNodeProvider('http://localhost:22973')
// The `TokenFaucet` contract instance from deploy result
const tokenFaucet = deployResult.instance
// The `TokenFaucetTypes.WithdrawEvent` is generated in the getting-started guide
const events: TokenFaucetTypes.WithdrawEvent[] = []
const subscribeOptions = {
  // It will check for new events from the full node every `pollingInterval`
  pollingInterval: 500,
  // The callback function will be called for each event
  messageCallback: (event: TokenFaucetTypes.WithdrawEvent): Promise<void> => {
    events.push(event)
    return Promise.resolve()
  },
  // This callback function will be called when an error occurs
  errorCallback: (error: any, subscription): Promise<void> => {
    console.log(error)
    subscription.unsubscribe()
    return Promise.resolve()
  }
}

// Subscribe the contract events from index 0
const subscription = tokenFaucet.subscribeWithdrawEvent(subscribeOptions, 0)
await new Promise((resolve) => setTimeout(resolve, 1000))
console.log(JSON.stringify(events, null, 2))
// [
//   {
//     "contractAddress": "v8uU9yLfzUwqpJeS9Kd76DB75WJBcEuMdYgEQ2Gn8pLF",
//     "blockHash": "0c07e672c40629a5c943cc7e4ec677140cbd50fdc9dcacb6a1d30bc38c0e7b50",
//     "txId": "c29e9cb10b3e0b34979b9daac73151d98ee4de8f913e66aa0f0c8dc0cb99a617",
//     "eventIndex": 0,
//     "name": "Withdraw",
//     "fields": {
//       "to": "1DrDyTr9RpRsQnDnXo2YRiPzPW4ooHX5LLoqXrqfMrpQH",
//       "amount": "1"
//     }
//   }
// ]

// Unsubscribe
subscription.unsubscribe()
```

## Tiện tích

### Conversion giữa contract id và contract address

```typescript
const contractId = 'bfc891f2f7fbb466bd7808f71cc022debb71fd3c1ceb752b623eb9c48ec4d165'
const contractAddress = addressFromContractId(contractId)
console.log(binToHex(contractIdFromAddress(contractAddress)) === contractId)
// true
```

### Lấy group của một address

```typescript
const group = groupOfAddress('1DrDyTr9RpRsQnDnXo2YRiPzPW4ooHX5LLoqXrqfMrpQH')
console.log(group)
// 0
```

### Lấy sub-contract id

```typescript
const contractId = 'bfc891f2f7fbb466bd7808f71cc022debb71fd3c1ceb752b623eb9c48ec4d165'
console.log(subContractId(contractId, '00'))
// 303483cfe0eaead281879233f884e8b64c2ecf26e368ccd4b05b2b5bda87ec3d
```
