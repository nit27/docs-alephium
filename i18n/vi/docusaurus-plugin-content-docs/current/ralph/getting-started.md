---
sidebar_position: 10
title: Bắt đầu
sidebar_label: Bắt đầu
---

## Giới thiệu

Ralph là một ngôn ngữ lập trình cho smart contract trên blockchain của Alephium và tập trung vào ba tiêu chí: bảo mật, đơn giản và hiểu quả. Hướng dẫn sau đây sẽ cung cấp các thủ thuật để viết rõ ràng, đúng cú pháp và bảo mật cho smart contract. Chúng tôi luôn bám sát các tiêu chí này khi thiết kế Ralph:
1. Giữ cho smart contract DSL đơn giản nhất có thể.
2. Luôn chỉ có một cách trực quan để sử dụng nó.
3. Tích hợp các chức năng thực tiễn.

## Biên soạn

Ralph là một ngôn ngữ gõ tĩnh, nhưng bạn không cần phải gõ cụ thể chi tiết cho các biến cụ bộ (local variable) và các  hằng số (constant) nhờ vào chức năng type inference.
Tất cả các type của Ralph đều là các valua type, ví dụ nó luôn luôn được sao chép khi nó được sử dụng như là các function argument hoặc được assign.
Hiện tại, Ralph chỉ hỗ trợ các kiểu dữ liệu sau đây:

### Primitive Types

#### U256

```rust
// The type of `a` ... `d` is U256.
let a = 10
let b = 10u
let c = 1_000_000_000
let d = 1e18
```

#### I256

```rust
// The type of `a` ... `d` is I256.
let a = -10
let b = 10i
let c = -1_000_000_000
let d = -1e18
```

#### Bool

```rust
// The type fo `a` and `b` is Bool.
let a = false
let b = true
```

#### ByteVec

```rust
// ByteVec literals must start with `#` followed by a hex string.
let a = #00112233
// ByteVec concatenation
let b = #0011 ++ #2233 // `b` is #00112233
// Empty ByteVec
let c = #
```

#### Address

```rust
// Address literals must start with `@` followed by a valid base58 encoded Alephium address.
let a = @1DrDyTr9RpRsQnDnXo2YRiPzPW4ooHX5LLoqXrqfMrpQH
```

### Fixed Size Array

Cú pháp cho các dãy fixed-size được lấy ý tưởng từ Rust.

```rust
// The type of `a0` is [U256; 4]
let a0 = [0, 1, 2, 3]

// The type of `a1` is [[U256, 2]; 2]
let a1 = [[0, 1], [2, 3]]

// The type of `a2` is [I256; 3]
let a2 = [0i; 3]

// The type of `a3` is [ByteVec; 4]
let a3 = [#00, #11, #22, #33]
```

### Mapping

Ralph sử dụng [subcontract](/ralph/getting-started#subcontract) thay vì cấu trúc map-like data để đưa ra chức năng map-like và làm giảm bớt các lỗi state bloat.

### Struct

Hiện tại, Ralph chưa hỗ trợ kiểu dữ liệu user-defined, nhưng chúng tôi sẽ cải thiện trong tương lai.

## Function

 Các function là những unit có thể được thực thi của code, bạn có thể định nghĩa các function bên trong một contract.

### Function Signature

```rust
// Public function, which can be called by anyone
pub fn foo() -> ()

// Private function, which can only be called inside the contract
fn foo() -> ()

// Function takes 1 parameter and has no return values
fn foo(a: U256) -> ()

// Function takes 2 parameters and returns 1 value
fn foo(a: U256, b: Boolean) -> U256

// Function takes 2 parameters and returns multiple values
fn foo(a: U256, b: Boolean) -> (U256, ByteVec, Address)
```

### Local Variable

Một function không thể chứa các định nghĩa duplocate variable và variable name trong function không thể giống các trường tên như contract.

```rust
fn foo() -> () {
  // `a` is immutable, and it cannot be reassigned
  let a = 10
  a = 9 // ERROR

  // `b` is mutable, and it can be reassigned
  let mut b = 10
  b = 9
}

fn bar() -> (U256, Boolean) {
  return 1, false
}

fn baz() -> () {
  // Both `a` and `b` are immutable
  let (a, b) = bar()
  // `c` is immutable, but `d` is mutable
  let (c, mut d) = bar()
  // Ignore the first return value of the function `bar`
  let (_, e) = bar()
}
```

### Control Structure

#### Return statement

```rust
fn foo() -> (U256, Boolean, ByteVec) {
  return 1, false, #00
}
```

#### If-else statement/expression

```rust
fn foo() -> ByteVec {
  // If else statement
  if (a == 0) {
    return #00
  } else if (a == 1) {
    return #01
  } else {
    return #02
  }
}

fn foo() -> ByteVec {
  return if (a == 0) #00 else if (a == 1) #01 else #02
}
```

#### For loop

```rust
// For loop
fn foo() -> () {
  for (let mut index = 0; index <= 4; index = index + 1) {
    bar(index)
  }
}
```

#### While loop

```rust
// While loop
fn foo() -> () {
  let mut index = 0
  while (index <= 4) {
    bar(index)
    index += 1
  }
}
```

:::note
`break` and `continue` statements are not supported in `for-loop` and `while-loop` because they may be bad practice in some cases. It's recommended to replace them with early `return` or [assert function](/ralph/built-in-functions#assert).
:::

:::note
Ở trong Ralph, mỗi một function chỉ có một scope, vì thế bạn không thể define các duplicated variable trong `while` hoặc `for` block:

```rust
let value = 0
while (true) {
  let value = 0 // ERROR, duplicated variable definitions
  // ...
}
```
Đây là một chức năng được thiết kế có mục đích vì variable shadowing thì không thực tế được sử dụng phổ biến.
:::

### Error Handling

Ralph cung cấp hai assertion function tích hợp sẳn cho error handling: [assert!](/ralph/built-in-functions#assert) và [panic!](/ralph/built-in-functions#panic). Assertion failure sẽ hoàn nguyên các thay đổi đã được tạo với world state bởi các giao dịch và dừng các giao dịch ngay lập tức.

```rust
enum ErrorCodes {
  InvalidContractState = 0
}

fn foo(cond: Boolean) -> () {
  // It will stop the transaction if `cond` is false.
  // The Alephium client will return the error code if the transaction fails.
  assert!(cond, ErrorCodes.InvalidContractState)
}

fn bar(cond: Boolean) -> U256 {
  if (!cond) {
    // The difference between `panic!` and `asset!` is that the return type of `panic!` is bottom type
    panic!(ErrorCodes.InvalidContractState)
  }
  return 0
}
```

### Function Call

Các function contract một contract hiện tại chỉ có thể được call trực tiếp ('bên trong') hoặc một cách đệ quy:

```rust
Contract Foo() {
  fn foo(v: U256) -> () {
    if (v == 0) {
      return
    }
    // Internal function call
    bar()
    // Recursive function call
    foo(v - 1)
  }

  fn bar() -> () {
    // ...
  }
}
```

Các function cũng có thể được call bên ngoài bằng cách sử dụng `bar.func()` notation, nơi mà `bar` là một contract instance và `func` là một function của `bar`:

```rust
Contract Bar() {
  pub fn func() -> U256 {
    // ...
  }
}

Contract Foo() {
  pub fn foo() -> () {
    // Instantiate the contract from contract id
    let bar = Bar(#15be9537456726c336a3cd1aa36074759c457f151ac253a500085920afe3838a)
    // External call
    let a = bar.func()
    // ...
  }
}
```

### Các function được tích hợp sẳn

Ralph cung cấp rất nhiều chức năng được tích hợp sẳn. Bạn có thể tham khảo tại [đây](/ralph/built-in-functions).

### Annotation

Trong Ralph cũng có hỗ trợ các annotation, hiện tại annotation khả dụng là `@using` annotation, và user-defined annotations sẽ được hỗ trợ trong tương lai.

`@using` annotation có bốn trường tùy chọn:

* `preapprovedAssets = true/false`: nếu function sử dụng user-approved assets. Giá trị mặc định là `false` cho các contract và `true` cho các script.
* `assetsInContract = true/false`: nếu function sử dụng contract assets. Giá trị mặc định sẽ là `false` cho các contract
* `checkExternalCaller = true/false`: nếu function kiểm tra các caller. Giá trị mặc định là `true` cho các contract
* `updateFields = true/false`: nếu function thay đổi các trường của contract. Giá trị mặc định là `false` cho các contract

#### Sử dụng Approved Assets

Trong Ralph, nếu một function sử dụng các asset, khi đó caller sẽ cần approve một cách rõ ràng các asset. và tất cả các function ở trong call stack phải được annotate với `@using(preapprovedAssets = true)`.

```rust
Contract Foo() {
  // Function `foo` uses approved assets, and it will transfer 1 ALPH and 1 token to the contract from the `caller`
  @using(preapprovedAssets = true)
  fn foo(caller: Address, tokenId: ByteVec) -> () {
    transferAlphToSelf!(caller, 1 alph)
    transferTokenToSelf!(caller, tokenId, 1)
  }

  @using(preapprovedAssets = true)
  fn bar(caller: Address, tokenId: ByteVec) -> () {
    // We need to explicitly approve assets when calling function `foo`
    foo{caller -> 1 alph, tokenId: 1}(caller, tokenId)
    // ...
  }
}
```

Về `preapprovedAssets` annotation, bạn sẽ cần kiểm tra như sau:

1. Nếu một function đã annotate `preapprovedAssets = true` nhưng không sử dụng các brace syntax, trình biên soan sẽ thông báo lỗi
2. Nếu một function call sử dụng các brace syntax nhưng function không được annotate `preapprovedAssets = true`, trình biên soạn sẽ thông báo lỗi

#### Sử dụng Contract Assets

```rust
Contract Foo() {
  // Function `foo` uses the contract assets, and it will transfer 1 alph to the caller
  @using(assetsInContract = true)
  fn foo(caller: Address) -> () {
    transferAlphFromSelf!(caler, 1 alph)
  }

  // Function `bar` must NOT be annotated with `@using(assetsInContract = true)`
  // because the contract assets will be removed after use
  fn bar(caller: Address) -> () {
    // ...
    foo(caller)
  }
}
```

Về `assetsInContract` annotation, trình biên soạn sẽ kiểm tra như sau:

1. Nếu function đã annotate `assetsInContract = true` nhưng không sử dụng contract assets, trình biên soạn sẽ thông báo lỗi

Bạn có thể tìm hiểu thêm về asset permission tại [đây](/ralph/asset-permission-system).

#### Update Field

Các function đã cập nhật các trường thông tin sẽ thay đổi các trường của contract. Nếu một function thay đổi các trường của contract mà không có `@using(updateFields = true)` annotation, trình biên soạn sẽ phát một cảnh báo; nếu một function không thay đổi các trường của contract nhưng đã annotate với `@using(updateFields = true)`, trình biên soạn cũng sẽ phát một cảnh báo.

```rust
Contract Foo(a: U256, mut b: Boolean) {
  // Function `f0` does not changes the contract fields
  fn f0() -> U256 {
    return a
  }

  // Function `f1` changes the contract fields
  @using(updateFields = true)
  fn f1() -> () {
    b = false
  }

  // Function f2 calls function f1, even if function f1 changes the contract fields,
  // function f2 still does not need to be annotated with `@using(updateFields = true)`,
  // because function f2 does not directly change the contract fields
  fn f2() -> () {
    f1()
  }
}
```

#### Kiểm tra External Caller

Trong smart contract, chúng ta cần thường xuyên kiểm tra các caller của contract function đã được xác thực hay chưa. Để tránh các bug được gây ra bởi các caller chưa được xác  nhận, trình biên dịch sẽ xuất hiện một cảnh báo cho tất cả các public function là không được kiểm tra cho các external caller. Cảnh báo có thể được loại bỏ với annotation `@using(checkExternalCaller = false)`.

Trình biên soạn sẽ bỏ qua việc kiểm tra cho các simple view function. Một simple view function phải thõa mãn các điều kiện sau:

1. Nó không thể thay đổi các contract field.
2. Nó không thể sử dụng bất kỳ asset nào.
3. Tất cả các sub-function call cũng phải là các simple view function.

Để kiểm tra caller của một function, built-in function [checkCaller!](/ralph/built-in-functions#checkcaller) phải được sử dụng.

```rust
Contract Foo(barId: ByteVec, mut b: Boolean) {
  enum ErrorCodes {
    InvalidCaller = 0
  }

  // We don't need to add the `@using(checkExternalCaller = true)` because
  // the `checkExternalCaller` is true by default for public functions.
  pub fn f0() -> () {
    // The `checkCaller!` built-in function is used to check if the caller is valid.
    checkCaller!(callerContractId!() == barId, ErrorCodes.InvalidCaller)
    b = !b
    // ...
  }

  // The compiler will report warnings for the function `f1`
  pub fn f1() -> () {
    b = !b
    // ...
  }

  // Function `f2` is a simple view function, we don't need to add the
  // `using(checkExternalCaller = false)` for simple view functions.
  pub fn f2() -> ByteVec {
    return barId
  }

  // The compiler will NOT report warnings because we checked the caller in function`f4`.
  pub fn f3() -> () {
    f4(callerContractId!())
    // ...
  }

  fn f4(callerContractId: ByteVec) -> () {
    checkCaller!(callerContractId == barId, ErrorCodes.InvalidCaller)
    // ...
  }
}
```

Có một trường hợp khác, trình biên soạn xuất hiện một cảnh báo khi một contract call một function thông qua một giao  diện. Điều này bởi vì chúng ta không biết liệu function đã tích hợp trước đó có cần kiểm tra external caller:

```rust
Interface Bar() {
  pub fn bar() -> ()
}

Contract Foo() {
  // The compiler will report warnings for the function `Foo.foo`
  pub fn foo(barId: ByteVec) -> () {
    Bar(barId).bar()
  }
}
```

## Contract

:::info
Mỗi một contract trên Alephium có 3 form định danh riêng biệt:
1. **Address**: mỗi contract có một địa chỉ duy nhất
2. **Contract ID**: mỗi contract có một contract ID duy nhất
3. **Token ID**: mỗi contract có thể phát hành một token với một ID giống như của contract ID

Trong Ralph, contract ID được sử dụng thường xuyên hơn. Các contract ID có thể được chuyển đổi từ/đến các form khác với chứng năng được tích hợp sẳn trong Ralph hoặc web3 SDK.
:::

Các contract trong Ralph thì giống với các class trong các ngôn ngữ hướng đối tượng. Mỗi một contract có thể chứa các thông tin khai báo của contract field, event, constant, enum và function. Tất cả thông tin khai báo phải được chứa bên trong một contract. Ngoài ra, các contract có thể được sử dụng lại các thông tin khai báo của những contract khác.

```rust
// This is a comment, and currently Ralph only supports line comments.
// Contract should be named in upper camel case.
// Contract fields are permanently stored in the contract storage.
Contract MyToken(supply: U256, name: ByteVec) {

  // Events should be named in upper camel case.
  // Events allow for logging of activities on the blockchain.
  // Applications can listen to these events through the REST API of an Alephium client.
  event Transfer(to: Address, amount: U256)

  // Constant variables should be named in upper camel case.
  const Version = 0

  // Enums can be used to create a finite set of constant values.
  enum ErrorCodes {
    // Enum constants should be named in upper camel case.
    InvalidCaller = 0
  }

  // Functions, parameters, and local variables should be named in lower camel case.
  pub fn transferTo(toAddress: Address) -> () {
    let payloadId = #00
    // ...
  }
}
```

### Field

Contract field được lưu trữ vĩnh viễn trong contract storage và các field có thể được thay đổ bởi contract code. Các app có thể lấy các contract field thông qua REST API từ một Alephium client.

```rust
// Contract `Foo` has two fields:
// `a`: immutable, it can not be changed by the contract code
// `b`: mutable, it can be changed by the contract code
Contract Foo(a: U256, mut b: Boolean) {
  // ...
}

// Contract fields can also be other contract.
// It will store the contract id of `Bar` in the contract storage of `Foo`.
Contract Foo(bar: Bar) {
  // ...
}

Contract Bar() {
  // ...
}
```

### Các function được tích hợp sẳn trong contract

Thỉnh thoãng, chúng ta cần tạo một contract ở trong một contract và trong trường hợp này chúng ta cần encode các contract field vào `ByteVec`. Ralph được tích hợp sẳn một chức năng được gọi là `encodeFields` rằng nó có thể để encode cho các contract field vào `ByteVec`.

Tham số của `encodeFields` function là một danh sách các kiểu của các contract field và được sắp xếp theo thứ tự đã được định nghĩa của nó. Function sẽ trả về hai giá trị `ByteVec`, giá trị đầu tiên là các encoded immutable field, và giá trị thứ hai là encoded mutable field.

Ví dụ:

```rust
Contract Foo(a: U256, mut b: I256, c: ByteVec, mut d: Bool) {
  // functions
}

Contract Bar() {
  @using(preapprovedAssets = true)
  fn createFoo(caller: Address, fooBytecode: ByteVec, a: U256, b: I256, c: ByteVec, d: Bool) -> (ByteVec) {
    let (encodedImmFields, encodedMutFields) = Foo.encodeFields!(a, b, c, d)
    return createContract!{caller -> 1 alph}(fooBytecode, encodedImmFields, encodedMutFields)
  }
}
```

### Event

Event là các tín hiệu được gửi đi mà các contracts có được kích hoạt. Các app có thể listen các event thông qua REST API của một Alephium client.

```rust
Contract Token() {
  // The number of event fields cannot be greater than 8
  event Transfer(to: Address, amount: U256)

  @using(assetsInContract = true)
  pub fn transfer(to: Address) -> () {
    transferTokenFromSelf!(selfTokenId!(), to, 1)
    // Emit the event
    emit Transfer(to, 1)
  }
}
```

### SubContract

Máy ảo của Alephium (Alephium's virtual machine) hỗ trợ subcontract. Các subcontract có thể được sử dụng như là một map-like data structure nhưng chúng nó ít xãy ra trong các vấn đề về state bloat. Một subcontract có thể được tạo ra bởi một parent contract với một subcontract path duy nhất.

```rust
Contract Bar(value: U256) {
  pub fn getValue() -> U256 {
    return value
  }
}

Contract Foo(barTemplateId: ByteVec) {
  event SubContractCreated(key: U256, contractId: ByteVec)

  @using(preapprovedAssets = true, checkExternalCaller = false)
  pub fn set(caller: Address, key: U256, value: U256) -> () {
    let path = u256To8Bytes!(key)
    let (encodedImmFields, encodedMutFields) = Foo.encodeFields!(value) // Contract `Bar` has only one field
    // Create a sub contract from the given key and value.
    // The sub contract id is `blake2b(blake2b(selfContractId!() ++ path))`.
    // It will fail if the sub contract already exists.
    let contractId = copyCreateSubContract!{caller -> 1 alph}(
      u256To8Bytes!(path),
      barTemplateId,
      encodedImmFields,
      encodedMutFields
    )
    emit SubContractCreated(key, contractId)
  }

  pub fn get(key: U256) -> U256 {
    let path = u256To8Bytes(key)
    // Get the sub contract id by the `subContractId!` built-in function
    let contractId =  subContractId!(path)
    return Bar(contractId).getValue()
  }
}
```

### Contract được tạo ra trong một contract

Ralph hỗ trợ việc tạo các contract có tính lập trình trong những contract khác. Nó cung cấp một vài function đã được tích hợp để tạo ra các contract, bạn có thể tìm hiểu thêm tại [đây](/ralph/built-in-functions#contract-functions).

Nếu bạn muốn tạo ra nhiều phiên bản của một contract, bạn nên sử dụng chức năng được tích hợp sẳn của `copyCreateContract!`, nó sẽ giảm rất nhiều dung lượng lưu trữ trên on-chain và phí gas của giao dịch.

```rust
Contract Foo(a: ByteVec, b: Address, mut c: U256) {
  // ...
}

// We want to create multiple instances of contract `Foo`.
// First we need to deploy a template contract of `Foo`, which contract id is `fooTemplateId`.
// Then we can use `copyCreateContract!` to create multiple instances.
TxScript CreateFoo(fooTemplateId: ByteVec, a: ByteVec, b: Address, c: U256) {
  let (encodedImmFields, encodedMutFields) = Foo.encodeFields!(a, b, c)
  copyCreateContract!(fooTemplateId, encodedImmFields, encodedMutFields)
}
```

### Migration

Các contract trên Alephium có thể được nâng cấo với hai chức năng migration: [migrate!](/ralph/built-in-functions#migrate) và [migrateWithFields!](/ralph/built-in-functions#migratewithfields). Đây là ba cách phổ biến để sử dụng chúng:

```Rust
fn upgrade(newCode: ByteVec) -> () {
  checkOwner(...)
  migrate!(newCode)
}

fn upgrade(newCode: ByteVec, newImmFieldsEncoded: ByteVec, newMutFieldsEncoded: ByteVec) -> () {
  checkOwner(...)
  migrateWithFields!(newCode, newImmFieldsEncoded, newMutFieldsEncoded)
}

fn upgrade(newCode: ByteVec) -> () {
  checkOwner(...)
  let (newImmFieldsEncoded, newMutFieldsEncoded) = ContractName.encodeFields!(newFields...)
  migrateWithFields!(newCode, newImmFieldsEncoded, newMutFieldsEncoded)
}
```

## Inheritance

Ralph cũng hỗ trợ multiple inheritance, khi một contract muốn inherit từ các contract khác, chỉ có một contract được tạo trên blockchain và code từ các parent contract sẽ được compile vào trong contract đã tạo.

```rust
Abstract Contract Foo(a: U256) {
  pub fn foo() -> () {
    // ...
  }
}

Abstract Contract Bar(b: ByteVec) {
  pub fn bar() -> () {
    // ...
  }
}

// The field name of the child contract must be the same as the field name of parnet contracts.
Contract Baz(a: U256, b: ByteVec) extends Foo(a), Bar(b) {
  pub fn baz() -> () {
    foo()
    bar()
  }
}
```

:::note
Trong Ralph, các abstract contract là không thể thực hiện được. Điều này có nghĩa là đoạn code bên dưới sẽ không khả dụng:

```rust
let bazId = // The contract id of `Baz`
Foo(bazId).foo() // ERROR
```
:::

## Interface

Các interface sẽ giống với các abstract contract với những điều kiện bên dưới:

* Nó không thể có bất cứ function nào đã được tích hợp.
* Nó không thể được thừa hưởng từ các contract khác, nhưng nó có thể thừa hưởng từ các interface khác.
* Nó không thể khai bác các contract field.
* Các contract chỉ có thể tích hợp trên một interface.

```rust
Interface Foo {
  event E(a: U256)

  @using(assetsInContract = true)
  pub fn foo() -> ()
}

Interface Bar extends Foo {
  pub fn bar() -> U256
}

Contract Baz() implements Bar {
  // The function signature must be the same as the function signature declared in the interface.
  @using(assetsInContract = true)
  pub fn foo() -> () {
    // Inherit the event from `Foo`
    emit E(0)
    // ...
  }

  pub fn bar() -> U256 {
    // ...
  }
}
```

Và bạn có thể khởi tạo một contract với interface:

```rust
let bazId = // The contract id of `Baz`
Foo(bazId).foo()
let _ = Bar(bazId).bar()
```

:::note
Khi deploy một contract bạn cần phải nạp vào một lượng đủ ALPH trong một contract (hiện tại là 1 alph). Vì vật tạo ra nhiều sub-contract là không thực tiễn.
:::

## TxScript

Một transaction script là những đoạn code để tương tác với các contract trên blockchain. Các transaction script có thể sử dụng các input asset từ một giao dịch. Một script chỉ có thể được sử dụng một lần và chỉ có thể được thực thi một lần cùng với giao dịch sở hữu.

```rust
Contract Foo() {
  pub fn foo(v: U256) -> () {
    // ...
  }
}

// The `preapprovedAssets` is true by default for `TxScript`.
// We set the `preapprovedAssets` to false because the script does not need assets.
@using(preapprovedAssets = false)
// `TxScript` fields are more like function parameters, and these
// fields need to be specified every time the script is executed.
TxScript Main(fooId: ByteVec) {
  // The body of `TxScript` consists of statements
  bar()
  Foo(fooId).foo(0)

  // You can also define functions in `TxScript`
  fn bar() -> () {
    // ...
  }
}
```
