---
sidebar_position: 15
title: Tạo dApp đầu tiên
sidebar_label: Tạo dApp đầu tiên
---

import UntranslatedPageText from "@site/src/components/UntranslatedPageText";

Trong hướng dẫn này sẽ hướng dẫn bạn tạo một project dApp cơ bản.

Yêu cầu:

- Viết code trong [Typescript](https://www.typescriptlang.org/)
- Sử dụng [terminal](https://en.wikipedia.org/wiki/Terminal_emulator)
- [nodejs](https://nodejs.org/en/) phiên bản >= 16
- `npm` phiên bản >= 8 

## Tạo một dApp project mới: Token Faucet

Trong hướng dẫn này, chúng ta sẽ viết dApp đầu tiên: Token faucet.

Code ở bên dưới được lấy từ trang [Bắt đầu](/dapps/getting-started), đừng lo lắng vì chúng tôi sẽ hướng dẫn cụ thể từng bước

Tạo một thư mục cho project và di chuyển vào nó:

```sh
mkdir alephium-faucet-tuto
cd alephium-faucet-tuto
```

Bây giờ hãy tạo một thư mục tên là `contracts` chứa tất cả các contract:

```sh
mkdir contracts
```

Contract đầu tiên của chúng ta sẽ là `token.ral` bạn có thể tìm nó ở [đây](https://github.com/alephium/nextjs-template/blob/main/contracts/token.ral). Bạn có thể copy tất cả file vào trong thư mục `contracts` của mình.

Hãy quan sát kỹ đoạn code dưới đây:

```rust
import "std/fungible_token_interface"

Contract TokenFaucet(
    symbol: ByteVec,
    name: ByteVec,
    decimals: U256,
    supply: U256,
    mut balance: U256
) implements IFungibleToken {
```

Bốn trường đầu tiên sẽ là các giá trị bất biến lưu trữ dữ liệu cần thiết để phục vụ [IFungibleToken interface](https://github.com/alephium/alephium-web3/blob/master/packages/web3/std/fungible_token_interface.ral).
`mut balance` là giá trị biến, có thể thay đổi để theo dõi số lượng các token còn lại trong faucet.

Bạn có thể thấy contract tạo ra một `event` là đưa ra thông báo `error`. Tìm hiểu thêm thông tin về [events](https://wiki.alephium.org/ralph/getting-started#events) và [error handling](https://wiki.alephium.org/ralph/getting-started#error-handling).

Dưới đây là 5 cách truy cập vào các contract argument.

Bạn sẽ thấy điều kỳ diệu ở cách cuối cùng:

```rust
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
```

Với `assert!` chúng ta hãy chắc chắn rằng ko có cái nào chiếm hơn 2 token tại cùng một thời điểm.  
Tại trường `transferTokenFromSelf` sẽ thực hiện việc di chuyển các token.  
Chúng ta cập nhật trường `mut balance` với một số dư mới. Trong trường hợp của bước underflow, một lỗi có thể sẽ xuất hiện và giao dịch sẽ không được thực thi.
`callerAddress!()` và `selfTokenId!()` là những chức năng được xây dựng sẳn, tìm hiểu thêm tại trang [built-in functions](/ralph/built-in-functions).

## Compile contract của bạn

Bạn cần phải tương tác với full node để có thể compile contract, và cần sử dụng thông tin đã định nghĩa chính xác khi [tạo devnet của bạn](/full-node/devnet). Nếu bạn chưa bắt đầu nó, hãy khởi tạo.
Chúng tôi định nghĩa node URL bằng cách sử dụng file đã config: `alephium.config.ts`. 
Tạo file này tại thư mục project và dán đoạn code bên dưới vào:

```typescript
import { Configuration } from '@alephium/cli'

export type Settings = {}

const configuration: Configuration<Settings> = {
  networks: {
    devnet: {
      //Make sure the two values match what's in your devnet configuration
      nodeUrl: 'http://localhost:22973',
      networkId: 2
    }
  }
}

export default configuration
```

Bây giờ hãy compile:

```sh
npx @alephium/cli@latest compile
```

Nó có thể yêu cầu bạn xác nhận vài thứ để bản package mới nhất `@alephium/cli`. Chọn yes để thực thi.

Khi câu lệnh bên trên chạy xong, bạn sẽ thấy một thư mục mới được tạo có tên là `artifacts`. Nó chứa nhiều file liên quan đến contract của bạn. Ví dụ như, `artifacts/ts/TokenFaucet.ts` tạo ra rất nhiều chức năng helper như `at`, `fetchState`, `call*`, v.v, và rất nhiều chức năng test.

## Test contract của bạn
SDK cung cấp các chức năng kiểm tra các unit. Nó sẽ call contract bằng cách gửi một giao dịch, nhưng thay vì thay đổi trạng thái của blockchain, nó sẽ trả về một trạng thái contract, các output của giao dịch, và các event.

Cài đặt test framework:

```sh
npm install ts-jest @types/jest
```

Bạn cũng cần package của chúng tôi `@alephium/web3`:

```sh
npm install @alephium/web3 @alephium/web3-test
```

Tạo một `test` folder:

```sh
mkdir test
```

và tạo file `test/token.test.ts` đơn giản chứa nội dung bên dưới:

```typescript
import { web3, Project, addressFromContractId } from '@alephium/web3'
import { randomContractId, testAddress } from '@alephium/web3-test'
import { TokenFaucet } from '../artifacts/ts'

describe('unit tests', () => {
  it('Withdraws 1 token from TokenFaucet', async () => {

    // Use the correct host and port
    web3.setCurrentNodeProvider('http://127.0.0.1:22973')
    await Project.build()

    const testContractId = randomContractId()
    const testParams = {
      // a random address that the test contract resides in the tests
      address: addressFromContractId(testContractId),
      // assets owned by the test contract before a test
      initialAsset: { alphAmount: 10n ** 18n, tokens: [{ id: testContractId, amount: 10n }] },
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

    const testResult = await TokenFaucet.tests.withdraw(testParams)
    console.log(testResult)
  })
})
```

Nếu bạn muốn một bài test phức tạp hơn, hãy tham khảo qua [template](https://github.com/alephium/nextjs-template/blob/main/test/unit/token.test.ts) này.

Không cần phải nhận quá nhiều thứ, hãy tạo một file TypeScript với một vài tuỳ chỉnh, đặt nó tên là `tsconfig.json` và lưu ở thư mục gốc của project:

```json
{
  "compilerOptions": {
    "outDir": "dist",
    "target": "es2020",
    "esModuleInterop": true,
    "module": "commonjs",
    "resolveJsonModule": true
  },
  "exclude": ["node_modules"],
  "include": ["src/**/*.ts", "test/**/*.ts", "scripts/**/*.ts", "alephium.config.ts", "artifacts/**/*.ts"]
}
```

Bây giờ bạn có thể chạy để test:

```sh
npx @alephium/cli@latest test
```

Bạn sẽ thấy cửa sổ terminal cho ra output của việc call withdraw method.

🎉 Xin chúc mừng! Bạn đã vừa mới tạo thành công contract đầu tiên và viết ra một test để call nó và test nó trên local! Bây giờ hãy tiếp tục deploy contract của bạn.

## Deploy your contract

Bây giờ mọi thứ sẽ thú vị hơn , chúng ta sẽ deploy contract trên `devnet` :rocket:

Câu lệnh `deploy` sẽ thực thi tất cả các script mà nó tìm thấy trong thư mục `scripts`. Hãy tạo thư mục `scripts` ngay trong thư mục gốc của dự án:

```sh
mkdir scripts
```

Tạo một deployment script tên là `0_deploy_faucet.ts` trong thư mục `scripts` và dán những dòng code bên dưới vào.  
Lưu ý: deployment script phải luôn có tiền tố với một con số (bắt đầu từ `0`).

```typescript
import { Deployer, DeployFunction, Network } from '@alephium/cli'
import { Settings } from '../alephium.config'
import { TokenFaucet } from '../artifacts/ts'

// This deploy function will be called by cli deployment tool automatically
// Note that deployment scripts should prefixed with numbers (starting from 0)
const deployFaucet: DeployFunction<Settings> = async (
  deployer: Deployer
): Promise<void> => {
  const issueTokenAmount = 100n
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

[deployContract](https://github.com/alephium/alephium-web3/blob/d2b5b63cae015e843aa77b4cf484bc62a070f1d5/packages/cli/src/types.ts#L133-L137) của `Deployer` sẽ lấy contract và deploy nó với một argument chính xác. Bạn có thêm một argument `taskTag` để tag deployment của bạn với một tên cho trước. Mặc định, nó sẽ sử dụng tên contract, nhưng nếu bạn deploy cùng một contract nhiều lần với các trường ban đầu khác nhau, tệp tin `.deployment`  sẽ bị ghi đè. Sử. dụng một `taskTag` để giải quyết vấn đề này.

Từ giao diện [DeployContractParams](https://github.com/alephium/alephium-web3/blob/d2b5b63cae015e843aa77b4cf484bc62a070f1d5/packages/web3/src/contract/contract.ts#L1286-L1293), Chúng ta thấy `initialFields` là trường bắt buộc bởi vì nó chứa các argument cho `TokenFaucet` contract.

Với `issueTokenAmount` bạn có thể chỉnh bao nhiêu token mà bạn muốn phát hành, điều này là bắt buộc nếu bạn muốn tạo một token. Nếu không ghi số lượng token muốn phát hành, sẽ không có token-id nào được tạo ra.

Bây giờ hãy deploy!

```sh
npx @alephium/cli@latest deploy
```

...UH-OH... Nó không chạy???

Nếu xuất hiện lỗi `The node chain id x is different from configured chain id y`, hãy kiểm tra `networkId` ở trong devnet configuration và file `alephium.config.ts`.

`No UTXO found` ???

Tất nhiên rồi, chúng ta chưa tạo ra `how-to-use-my-utxos`, nên cần phải tạo ra [privateKeys](https://github.com/alephium/alephium-web3/blob/d2b5b63cae015e843aa77b4cf484bc62a070f1d5/packages/cli/src/types.ts#L39-L46).

Bạn cần phải xuất private key từ wallet extension (có thể làm sau), hãy chắc chắn rằng ví của bạn có đủ fund, như là một trong những genesis allocation ở trên devnet. 
Nếu bạn sử dụng docker để khởi chạy devnet, nó cũng có thể sẽ chạy thành công bởi vì chúng đã được làm ra [a default private key in our cli package](https://github.com/alephium/alephium-web3/blob/d2b5b63cae015e843aa77b4cf484bc62a070f1d5/packages/cli/src/types.ts#L75) dựa trên genesis allocation.

Hãy cập nhật `alephium.config.ts`

```typescript
const configuration: Configuration<void> = {
  networks: {
    devnet: {
      nodeUrl: 'http://localhost:22973',
      networkId: 2,
      //The private key of my genesis address 132mqFF2BuxGigdaMTGSruuW29kmEs2eEGcpquG4YZRNh
      privateKeys: ['672c8292041176c9056bb0dd1d91d34711ceed2493b5afc83f2012b27df2c559']
    }
  }
}
```

:::caution
Real applications should use environment variables or similiar techniques for senstivie settings like `privateKeys`.
Do not commit your private keys to source control.
:::

và thử lại để deploy:

```sh
npx @alephium/cli@latest deploy
```

```sh
Contracts are compiled already. Loading them from folder "artifacts"
Deploying contract TokenFaucet
Deployer - group 1 - 132mqFF2BuxGigdaMTGSruuW29kmEs2eEGcpquG4YZRNh
Token faucet contract id: d00e9c788ddd572b0c186f0599a264f4c79f009c632c8040b7c5f71bfc0ec301
Token faucet contract address: 28h7qSmkAAeNyoBuQKGyp1WG8VfdKPePCCFGKwp2Y8yyA
✅ Deployment scripts executed!
```

Chúc mừng! Contract của bạn đã được deploy. Chúng ta có thể kiểm tra số dư của contract. Sử dụng `curl` và thay đổi contract address tuỳ theo kết quả đã deploy:

```sh
curl 'http://localhost:22973/addresses/28h7qSmkAAeNyoBuQKGyp1WG8VfdKPePCCFGKwp2Y8yyA/balance'
```

Kết quả trả về nên giống như sau:

```json
{
  "balance": "1000000000000000000",
  "balanceHint": "1 ALPH",
  "lockedBalance": "0",
  "lockedBalanceHint": "0 ALPH",
  "tokenBalances": [
    {
      "id": "d00e9c788ddd572b0c186f0599a264f4c79f009c632c8040b7c5f71bfc0ec301",
      "amount": "100"
    }
  ],
  "utxoNum": 1
}
```

Chúng ta có thể thấy token id, với 100 token chúng ta đã phát hành trước đó.

Hãy kiểm tra contract state bằng cách lấy group của địa chỉ: 

```sh
curl 'http://localhost:22973/addresses/28h7qSmkAAeNyoBuQKGyp1WG8VfdKPePCCFGKwp2Y8yyA/group'
curl 'http://localhost:22973/contracts/28h7qSmkAAeNyoBuQKGyp1WG8VfdKPePCCFGKwp2Y8yyA/state?group=1'
```


Contract state phản hồi:
```json
{
  "address": "28h7qSmkAAeNyoBuQKGyp1WG8VfdKPePCCFGKwp2Y8yyA",
  "bytecode": "050609121b4024402d404a010000000102ce0002010000000102ce0102010000000102ce0202010000000102ce0302010000000102a0000201020101001116000e320c7bb4b11600aba00016002ba10005b416005f",
  "codeHash": "641343b4f1c08b03969b127b452acc7535cad20231bc32af6c0b5f218dd8ff0c",
  "initialStateHash": "06595afa695949e915dfc1220dfb47125b01751d9e193f4c5fa1c7fc3566673d",
  "immFields": [
    {
      "type": "ByteVec",
      "value": "5446"
    },
    {
      "type": "ByteVec",
      "value": "546f6b656e466175636574"
    },
    {
      "type": "U256",
      "value": "18"
    },
    {
      "type": "U256",
      "value": "100"
    }
  ],
  "mutFields": [
    {
      "type": "U256",
      "value": "100"
    }
  ],
  "asset": {
    "attoAlphAmount": "1000000000000000000",
    "tokens": [
      {
        "id": "d00e9c788ddd572b0c186f0599a264f4c79f009c632c8040b7c5f71bfc0ec301",
        "amount": "100"
      }
    ]
  }
}
```

Ở trong `immFields` ta có thể thấy argument của `TokenFaucet` là (`symbol`, `name`, `decimals`, `supply`). Cũng như trường `mutFields` chứa số dư hiện tại. Chúng ta sẽ kiểm tra trường này sau khi call faucet.

Câu lệnh `deploy` cũng tạo một tệp `.deployments.devnet.json`, thông qua kết quả của việc deploy. Rất cần thiết để lưu giữ tệp này để sau này dễ dàng tương tác với contract hơn mặc dù tất cả các thông tin có thể được tìm thấy trên blockchain.

# Tương tác với deployed contract

Có được token faucet là rất tốt, thậm chí lấy dc các token từ nó thì càng tuyệt vời hơn.

Bây giờ chúng ta sẽ viết vài dòng code để tương tác với faucet contract.

Ta cần cài đặt một gói cài đặt `cli` và `typescript` dependency nếu chưa cài:

```
npm install @alephium/cli typescript
```

Bạn sẽ thấy các tuỳ chọn khác nhau để tương tác với blockchain. Trước đó ta đã sử dụng `DeployFunction` với tệp `scripts/<number>_*` nên nó sẽ tự động deploy với công cụ CLI.

Một cách khác để tạo một project cơ bản là sử dụng TypeScript. Tạo một thư mục và một tệp tên là `src` trong folder gốc của dự dán và đặt tên nó là `tokens.ts`, sau đó dán đoạn mã bên dưới vào trong tệp `src`:

```typescript
import { Deployments } from '@alephium/cli'
import { DUST_AMOUNT, web3, Project, NodeProvider } from '@alephium/web3'
import { PrivateKeyWallet} from '@alephium/web3-wallet'
import configuration from '../alephium.config'
import { TokenFaucet, Withdraw } from '../artifacts/ts'

async function withdraw() {

  //Select our network defined in alephium.config.ts
  const network = configuration.networks.devnet

  //NodeProvider is an abstraction of a connection to the Alephium network
  const nodeProvider = new NodeProvider(network.nodeUrl)

  //Sometimes, it's convenient to setup a global NodeProvider for your project:
  web3.setCurrentNodeProvider(nodeProvider)

  //Connect our wallet, typically in a real application you would connect your web-extension or desktop wallet
  const wallet = new PrivateKeyWallet({privateKey: '672c8292041176c9056bb0dd1d91d34711ceed2493b5afc83f2012b27df2c559' })

  // Compile the contracts of the project if they are not compiled
  Project.build()

  //.deployments contains the info of our `TokenFaucet` deployement, as we need to now the contractId and address
  //This was auto-generated with the `cli deploy` of our `scripts/0_deploy_faucet.ts`
  const deployments = await Deployments.from('.deployments.devnet.json')

  //Make sure it match your address group
  const accountGroup = 1

  const deployed = deployments.getDeployedContractResult(accountGroup, 'TokenFaucet')

  if(deployed !== undefined) {
    const tokenId = deployed.contractInstance.contractId
    const tokenAddress = deployed.contractInstance.address

    // Submit a transaction to use the transaction script
    // It uses our `wallet` to sing the transaction.
    await Withdraw.execute(wallet, {
      initialFields: { token: tokenId, amount: 1n },
      attoAlphAmount: DUST_AMOUNT
    })

    // Fetch the latest state of the token contract, `mut balance` should have change
    const faucet = TokenFaucet.at(tokenAddress)
    const state = await faucet.fetchState()
    console.log(state.fields)

    // Fetch wallet balance see if token is there
    const balance = await wallet.nodeProvider.addresses.getAddressesAddressBalance(wallet.account.address)
    console.log(balance)
  } else {
    console.log('`deployed` is undefined')
  }
}

// Let's perform one withdraw
withdraw()
```

Nếu để ý kỹ, bạn sẽ thấy một vài thứ đang xuất hiện trong `artifacts`: [`Withdraw`](https://github.com/alephium/nextjs-template/blob/main/contracts/withdraw.ral) là một [`TxScript`](https://wiki.alephium.org/ralph/getting-started#txscript), nó yêu cầu để tương tác với `TokenFaucet` contract. Code của nó rất đơn giản. Tạo một tệp tên là `withdraw.ral` nằm trong thư mục `contracts` và dán đoạn code bên dưới:

```rust
TxScript Withdraw(token: TokenFaucet, amount: U256) {
    token.withdraw(amount)
}
```

Bây giờ bạn cần compile lại contract để lấy artifact cho `Withdraw`:

```sh
npx @alephium/cli@latest compile
```

Tiếp theo, compile đoạn mã TypeScript đến JavaScript:

```sh
npx tsc --build .
```

Ồh, bạn có thể thấy lỗi từ `alephium.config.ts`, bởi vì những thiết đang sử dụng một JSON đơn giản, nhưng bây giờ `TypeScript` muốn nó tương tác với chính [interface](https://github.com/alephium/alephium-web3/blob/d2b5b63cae015e843aa77b4cf484bc62a070f1d5/packages/cli/src/types.ts#L48-L62). Đặc biệt là `networks` là một bản ghi cần chứa 3 loại `NetworkType`. Bạn cần fix nó hoặc cập nhật tệp `alephium.config.ts` với:

```typescript
import { Configuration } from '@alephium/cli'

export type Settings = {}

const configuration: Configuration<Settings> = {
  defaultNetwork: 'devnet',
  networks: {
    devnet: {
      nodeUrl: 'http://localhost:22973',
      networkId: 2, //Use the same as in your devnet configuration
      privateKeys: ['672c8292041176c9056bb0dd1d91d34711ceed2493b5afc83f2012b27df2c559'],
      settings: {}
    },
    testnet: {
      nodeUrl: '',
      privateKeys: [],
      settings: {}
    },
    mainnet: {
      nodeUrl: '',
      privateKeys: [],
      settings: {}
    }
  }
}

export default configuration
```

Bây giờ compile lại

```
npx tsc --build .
```

Một thư mục `dist` sẽ được tạo ra, tiếp tục bằng cách tương tác với deployed token faucet:

```
node dist/src/token.js
```

Bây giờ bạn sẽ tự hào vì là chủ của token vừa được tạo ra.


## Tiếp theo là gì?

Bạn nên tham khảo thêm ví dụ phức tạp hơn về token faucet ở hướng dẫn dự án [alephium/nextjs-template](https://github.com/alephium/nextjs-template).

## Kết nối đến các ví

dApp sẽ yêu cầu sự tích hợp từ ví cho các user để xác thực và tương tác với Alephium blockchain,
như là ký vào giao dịch. Hiện tại, các dApp có thể được tích hợp với cả [Extension Wallet](../wallet/extension-wallet/dapp)
và [WalletConnect](../wallet/walletconnect). Vui lòng tham khảo qua để thêm thông tin chi tiết.

## Tìm hiểu thêm

- Tìm hiểu về hệ sinh thái, vui lòng truy cập [tổng quan về hệ sinh thái](/dapps/ecosystem).
- Tìm hiểu thêm về web3 SDK, vui lòng truy cập [hướng dẫn web3 SDK](/dapps/alephium-web3).
- Tìm hiểu thêm về ngôn ngữ Ralph, vui lòng truy cập [hướng dẫn về ngôn ngữ lập trình Ralph](/ralph/getting-started).
- Tìm hiểu thêm về cách tạo dApp trên Nextjs, vui lòng truy cập [Xây dựng dApp với Nextjs](/dapps/build-dapp-with-nextjs.md)
