---
sidebar_position: 5
title: Bắt đầu
sidebar_label: Bắt đầu
---

import UntranslatedPageText from "@site/src/components/UntranslatedPageText";

## Tổng quan

Alephium cung cấp đầy đủ nhiều công cụ và các package để giúp bạn có thể xây dựng các dApp của mình.

Hướng dẫn này giúp bạn cài đặt với những setup được khuyến khích bởi chúng tôi.

Điều kiện cần có:

- Viết code bằng [Typescript](https://www.typescriptlang.org/)
- Chạy trên [terminal](https://en.wikipedia.org/wiki/Terminal_emulator)
- [nodejs](https://nodejs.org/en/) phiên bản >= 16
- `npm` phiên bản >= 8 

## Tại một project dApp

Để tạo một project hướng dẫn, mở terminal và chạy:

```
npx @alephium/cli@latest init alephium-tutorial
```

Nó sẽ tạo một directory mới `alephium-tutorial` và đưa những project mẫu vào trong directory.

## Khởi chạy mạng local

Để compile và kiểm tra các contract, rất cần thiết để chạy một phiên bản mạng local, bạn có thể tham khảo [hướng dẫn này](/full-node/devnet) để chạy devnet.

Mạng mới của bạn sẽ được khởi chạy và sử dụng [tuỳ chỉnh này](https://github.com/alephium/alephium-stack/blob/master/devnet/devnet.conf) và tạo ra các địa chỉ trong 4 group với vừa đủ ALPH cho mục đích thử nghiệm.

Typescript SDK bây giờ sẽ có thể tương tác với mạng thông qua REST endpoints.

## Compile contract của bạn

Tiếp theo, di chuyển vào tutorial project:

```
cd alephium-tutorial
```

Hãy nhìn vào thư mục `contracts/`, bạn sẽ tìm thấy `token.ral`:

```rust
import "std/fungible_token_interface"

// Defines a contract named `TokenFaucet`.
// A contract is a collection of fields (its state) and functions.
// Once deployed, a contract resides at a specific address on the Alephium blockchain.
// Contract fields are permanently stored in contract storage.
// A contract can issue an initial amount of token at its deployment.
Contract TokenFaucet(
    symbol: ByteVec,
    name: ByteVec,
    decimals: U256,
    supply: U256,
    mut balance: U256
) implements IFungibleToken {

    // Events allow for logging of activities on the blockchain.
    // Alephium clients can listen to events in order to react to contract state changes.
    event Withdraw(to: Address, amount: U256)

    enum ErrorCodes {
        InvalidWithdrawAmount = 0
    }

    // A public function that returns the initial supply of the contract's token.
    // Note that the field must be initialized as the amount of the issued token.
    pub fn getTotalSupply() -> U256 {
        return supply
    }

    // A public function that returns the symbol of the token.
    pub fn getSymbol() -> ByteVec {
        return symbol
    }

    // A public function that returns the name of the token.
    pub fn getName() -> ByteVec {
        return name
    }

    // A public function that returns the decimals of the token.
    pub fn getDecimals() -> U256 {
        return decimals
    }

    // A public function that returns the current balance of the contract.
    pub fn balance() -> U256 {
        return balance
    }

    // A public function that transfers tokens to anyone who calls it.
    // The function is annotated with `updateFields = true` as it changes the contract fields.
    // The function is annotated as using contract assets as it does.
    @using(assetsInContract = true, updateFields = true, checkExternalCaller = false)
    pub fn withdraw(amount: U256) -> () {
        // Debug events can be helpful for error analysis
        emit Debug(`The current balance is ${balance}`)

        // Make sure the amount is valid
        assert!(amount <= 2, ErrorCodes.InvalidWithdrawAmount)
        // Functions postfixed with `!` are built-in functions.
        transferTokenFromSelf!(callerAddress!(), selfTokenId!(), amount)
        // Ralph does not allow underflow.
        balance = balance - amount

        // Emit the event defined earlier.
        emit Withdraw(callerAddress!(), amount)
    }
}
```

và `withdraw.ral` :

```rust
// Defines a transaction script.
// A transaction script is a piece of code to interact with contracts on the blockchain.
// Transaction scripts can use the input assets of transactions in general.
// A script is disposable and will only be executed once along with the holder transaction.
TxScript Withdraw(token: TokenFaucet, amount: U256) {
    // Call token contract's withdraw function.
    token.withdraw(amount)
}
```

Để compile các contract của bạn, chạy:

```
npx @alephium/cli@latest compile
```

Các artifact đã compile sẽ ở trong thư mục `artifacts`.

Dòng lệnh này cũng sẽ tạo ra một code typescript dự trên các artifact. Typescript code đã được tạo sẽ nằm trong thư mục `artifacts/ts`. Bạn có thể dễ dàng tương tác với blockchain của Alephium và sử dụng thuận tiện hơn với typescript code đã tạo.

## Test your contract

Project mẫu sẽ xuất hiện với các test `test/unit/token.test.ts` cho contract của bạn:

```typescript
import { web3, Project, TestContractParams, addressFromContractId, AssetOutput, DUST_AMOUNT } from '@alephium/web3'
import { expectAssertionError, randomContractId, testAddress, testNodeWallet } from '@alephium/web3-test'
import { deployToDevnet } from '@alephium/cli'
import { TokenFaucet, TokenFaucetTypes, Withdraw } from '../artifacts/ts'

describe('unit tests', () => {
  let testContractId: string
  let testTokenId: string
  let testContractAddress: string
  let testParamsFixture: TestContractParams<TokenFaucetTypes.Fields, { amount: bigint }>

  // We initialize the fixture variables before all tests
  beforeAll(async () => {
    web3.setCurrentNodeProvider('http://127.0.0.1:22973')
    await Project.build()
    testContractId = randomContractId()
    testTokenId = testContractId
    testContractAddress = addressFromContractId(testContractId)
    testParamsFixture = {
      // a random address that the test contract resides in the tests
      address: testContractAddress,
      // assets owned by the test contract before a test
      initialAsset: { alphAmount: 10n ** 18n, tokens: [{ id: testTokenId, amount: 10n }] },
      // initial state of the test contract
      initialFields: {
        symbol: Buffer.from('TF', 'utf8').toString('hex'),
        name: Buffer.from('TokenFaucet', 'utf8').toString('hex'),
        decimals: 18n,
        supply: 10n ** 18n,
        balance: 10n
      },
      // arguments to test the target function of the test contract
      testArgs: { amount: 1n },
      // assets owned by the caller of the function
      inputAssets: [{ address: testAddress, asset: { alphAmount: 10n ** 18n } }]
    }
  })
  //See more test in `test/unit/token.test.ts`
})
```

Bạn có thể chạy với câu lệnh:

```
npm run test
```

hoặc

```
npx @alephium/cli@latest test
```

## Deploy contract của bạn

Tiếp theo, để deploy contract chúng ta sẽ sử dụng Alephium CLI và một script để deploy `scripts/0_deploy_faucet.ts`:

```typescript
import { Deployer, DeployFunction, Network } from '@alephium/cli'
import { Settings } from '../alephium.config'
import { TokenFaucet } from '../artifacts/ts'

// This deploy function will be called by cli deployment tool automatically
// Note that deployment scripts should prefixed with numbers (starting from 0)
const deployFaucet: DeployFunction<Settings> = async (
  deployer: Deployer,
  network: Network<Settings>
): Promise<void> => {
  // Get settings
  const issueTokenAmount = network.settings.issueTokenAmount
  const result = await deployer.deployContract(TokenFaucet, {
    // The amount of token to be issued
    issueTokenAmount: issueTokenAmount,
    // The initial states of the faucet contract
    initialFields: {
      symbol: Buffer.from('TF', 'utf8').toString('hex'),
      name: Buffer.from('TokenFaucet', 'utf8').toString('hex'),
      decimals: 18n,
      supply: issueTokenAmount,
      balance: issueTokenAmount
    }
  })
  console.log('Token faucet contract id: ' + result.contractInstance.contractId)
  console.log('Token faucet contract address: ' + result.contractInstance.address)
}

export default deployFaucet
```

Bạn có thể khởi chạy nó bằng:

```
npx @alephium/cli@latest deploy
```

Nó sẽ deploy token faucet vào group 0 của devnet. Để deploy trên testnet (hoặc bất kỳ mạng nào), cập nhật `alephium.config.ts` của bạn và theo sau đó là `--network`:

```
npx @alephium/cli@latest deploy --network testnet
```

## Tương tác với contract đã được deploy

Bây giờ, bạn có thể build source code `src/token.ts` :

```typescript
import { Deployments } from '@alephium/cli'
import { DUST_AMOUNT, web3, Project } from '@alephium/web3'
import { testNodeWallet } from '@alephium/web3-test'
import configuration from '../alephium.config'
import { TokenFaucet, Withdraw } from '../artifacts/ts'

async function withdraw() {
  web3.setCurrentNodeProvider('http://127.0.0.1:22973')
  // Compile the contracts of the project if they are not compiled
  Project.build()

  // Attention: test wallet is used for demonstration purpose
  const signer = await testNodeWallet()

  const deployments = await Deployments.load(configuration, 'devnet')

  // The test wallet has four accounts with one in each address group
  // The wallet calls withdraw function for all of the address groups
  for (const account of await signer.getAccounts()) {
    // Set an active account to prepare and sign transactions
    await signer.setSelectedAccount(account.address)
    const accountGroup = account.group

    // Load the metadata of the deployed contract in the right group
    const deployed = deployments.getDeployedContractResult(accountGroup, 'TokenFaucet')
    if (deployed === undefined) {
      console.log(`The contract is not deployed on group ${account.group}`)
      continue
    }
    const tokenId = deployed.contractInstance.contractId
    const tokenAddress = deployed.contractInstance.address

    // Submit a transaction to use the transaction script
    await Withdraw.execute(signer, {
      initialFields: { token: tokenId, amount: 1n },
      attoAlphAmount: DUST_AMOUNT
    })

    const faucet = TokenFaucet.at(tokenAddress)
    // Fetch the latest state of the token contract
    const state = await faucet.fetchState()
    console.log(JSON.stringify(state.fields, null, '  '))
  }
}

withdraw()

```

Đơn giản là chạy:

```
npm run build
```

và tương tác với token faucet đã được deploy:

```
node dist/src/token.js
```

## Kết nối vào ví

dApp yêu cầu sự tương tác của ví cho các user của dApp để xác nhận và có thể sử dụng trên blockchain của Alephium, ví dụ như ký (sign) vào các giao dịch. Hiện tại dApps có thể được tương tác với cả hai [Extension Wallet](../wallet/extension-wallet/dapp)
và [WalletConnect](../wallet/walletconnect). Xin hãy tham khảo các trang hướng dẫn để biết thêm chi tiết.

## Tìm hiểu thêm

- Tìm hiểu về hệ sinh thái, truy cập [Tổng quan về hệ sinh thái](/dapps/ecosystem).
- Tìm hiểu về web3 SDK, truy cập [hướng dẫn web3 SDK](/dapps/alephium-web3).
- Tìm hiểu về ngôn ngữ Ralph, truy cập [Hướng dẫn ngôn ngữ lập trình Ralph](/ralph/getting-started).
- Tìm hiểu về cách xây dựng một Nextjs dApp, truy cập [Xây dựng dApp với Nextjs](/dapps/build-dapp-with-nextjs.md)
