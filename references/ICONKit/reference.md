---
title: "ICONKit, ICON sdk for Swift"
excerpt: ""
---

ICON supports SDK for 3rd party or user services development. You can integrate ICON SDK for your project and utilize ICON’s functionality.
This document provides you with an information of installation and API specification.

## Versions
Documentations for ICONKit 0.2.4

## Installation

### CocoaPods
[CocoaPods](https://cocoapods.org/) is a dependecy manager for Swift Cocoa projects.
```
$ sudo gem install cocoapods
```

To integrate ICONKit into your project, specify it in your `Podfile`

```
target '<<Your Target Name>>' do
    use_frameworks!
    ...
    pod 'ICONKit', '~> 0.2.4'
    ...
end
```

Now install the ICONKit
```
$ pod install
```

ICONKit uses [Result](https://github.com/antitypical/Result) framework. All functions of ICONService returns Result<T, ICONResult> which means `T` for successor, `ICONResult` for error.
Refer to [Result](https://github.com/antitypical/Result) for more detail.

## API Specification

## ICONService

`ICONService` is a class which provides APIs to communicate with ICON nodes. It enables you to easily use ICON JSON-RPC APIs (version 3). All methods of `IconService` returns a `Request<T, ICONResult>` instance. To execute the request and get the result value, you need to run `execute()` function of `Request`. 
Return value formed as `Result<T, ICONResult>`
All requests will be executed **synchronously**. Asynchronous request will be added soon.

### Initialize
```Swift
init(provider: String, nid: String)
```

#### Parameters
| Parameter | Type | Description |
| --------- | ---- | ----------- |
| provider | `String` | ICON node url |
| nid | String | Network ID |

#### Example
```Swift
    let iconService = ICONService(provider: "https://ctz.solidwallet.io/api/v3", nid: "0x1")
```

### getTotalSupply
get the total number of issued coins.
```Swift
func getTotalSupply() -> Request<Response.IntValue>
```

#### Parameters

None

#### Returns

`Request` - Request instance for `icx_getTotalSupply` JSON-RPC API request. if `execute()` successfully, returns `Result<Response.IntValue, ICONResult>`

#### Example
```Swift
// Returns total value of issued coins
let response = iconService.getTotalSupply().execute()
switch response {
case .success(let result): // result == Response.IntValue
    print(result.result)

case .error(let error):
    print(error)
}
```

### getBalance
Get the balance of the address.
```Swift
func getBalance(address: String) -> Request<Response.IntValue>
```

#### Parameters
| Parameter | Type | Description |
| --- | --- | --- |
| address | `String` | an EOA Address |

#### Returns
`Request` - Request instance for `icx_getBalance` JSON-RPC API request. if `execute()` successfully, returns `Result<Response.IntValue, ICONResult>`

#### Example
```Swift
// Returns the balance of specific address
let response = iconService.getBalance(address: "hx9d8a8376e7db9f00478feb9a46f44f0d051aab57").execute()
switch response {
case .success(let result): // result == Response.IntValue
    print(result.result)

case .error(let error):
    print(error)
}
```

### getBlock
Get the block information.
```Swift
func getBlock(height: UInt64) -> Request<Response.Block>
func getBlock(hash: String) -> Request<Resopnse.Block>
```

#### Parameters
| Parameter | Type | Description |
| --- | --- | --- |
| height | `UInt64` | Height value of block |
| hash | `String` | Hash value of block |

#### Returns
`Request` - Request instance for `icx_getBlockByHeight` | `icx_getBlockByHash`. if `execute()` successfully, returns `Result<Response.Block, ICONResult>`

#### Example
```Swift
// Returns block information by block height
let heightBlock = iconService.getBlock(height: 10532).execute()
// Returns block information by block hash
let hashBlock = iconService.getBlock(hash: "0xdb310dd653b2573fd673ccc7489477a0b697333f77b3cb34a940db67b994fd95")
switch response {
case .success(let result): // result == Response.Block
    print(result.result)

case .error(let error):
    print(error)
}
```

### getLasBlock
Get the last block information.
```Swift
func getLastBlock() -> Request<Response.Block>
```

#### Parameters
None

#### Returns
`Request` - Request instance for `icx_getLastBlock` JSON-RPC API request. if `execute()` successfully, returns `Result<Response.Block, ICONResult>`

#### Example
```Swift
// Returns last block
let response = iconService.getLastBlock().execute()
switch response {
case .success(let result): // result == Response.Block
    print(result.result)

case .error(let error):
    print(error)
}
```

### getScoreAPI
Get the SCORE API list.
```Swift
func getScoreAPI(scoreAddress: String) -> Request.ScoreAPI>
```

#### Parameters
| Parameter | Type | Description |
| --- | --- | --- |
| scoreAddress | `String` | A SCORE address |

#### Returns
`Request` - Request instance for `icx_getScoreApi` JSON-RPC API request. if `execute()` successfully, returns `Result<Response.ScoreAPI, ICONResult>`

#### Example
```Swift
// Returns the SCORE API list
let response = iconService.getScoreAPI(scoreAddress: "cx0000000000000000000000000000000000000001").execute()
switch response {
case .success(let result): // result == Response.ScoreAPI
    print(result.result)

case .error(let error):
    print(error)
}
```

### getTransaction
Get the transaction information.
```Swift
func getTransaction(hash: String) -> Request<Response.Transaction>
```

#### Parameters
| Parameter | Type | Description |
| --- | --- | --- |
| hash | `String` | Hash value of transaction |

#### Returns
`Request` - Request instnace for `icx_getTransactionByHash` JSON-RPC API request. if `execute()` successfully, returns `Result<Response.Transaction, ICONResult>`

#### Example
```Swift
// Returns transaction information
let response = iconService.getTransaction(hash: "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238").execute()
switch response {
case .success(let result): // result == Response.Transaction
    print(result.result)

case .error(let error):
    print(error)
}
```

### getTransactionResult
Get teh transaction result information.
```Swift
func getTransactionResult(hash: String) -> Request<Response.TransactionResult>
```

#### Parameters
| Parameter | Type | Description |
| --- | --- | --- |
| hash | `String` | a transaction hash |

#### Returns
`Request` - Request instance for `icx_getTransactionResult` JSON-RPC API request. if `execute()` successfully, returns `Result<Response.TransactionResult, ICONResult>`

#### Example
```Swift
// Returns transaction result information
let response = iconService.getTransactionResult(hash: "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238").execute()
switch response {
case .success(let result): // result == Response.TransactionResult
    print(result.result)

case .error(let error):
    print(error)
}
```

### sendTransaction
Send a transaction that changes the state of address.
```Swift
func sendTransaction(signedTransaction: SignedTransaction) -> Request<Response.TxHash>
```

#### Parameters
| Parameter | Type | Description |
| --- | --- | --- |
| signedTransaction | `SignedTransaction` | an instance of [SignedTransaction] class. |

#### Returns
`Request` - Request instance for `icx_sendTransaction` JSON-RPC request. if `execute()` successfully, returns `Result<Response.TxHash, ICONResult>`

#### Example
```Swift
let response = iconService.sendTransaction(signedTransaction: signedTransaction).execute()
switch response {
case .success(let result): // result == Response.TxHash
    print(result.result)

case .error(let error):
    print(error)
}
```

### call
Calls external function of SCORE.
```Swift
func call<T>(_ call: Call<T>) -> Request<T>
```

#### Parameters
| Parameter | Type | Description |
| --- | --- | --- |
| call | Call | an instance of Call class. |

#### Returns
`Request<T>` - Request instance for `icx_call` JSON-RPC request. if `execute()` successfully, returns `Result<T, ICONResult>`

#### Example
```Swift
let response = iconService.call(call).execute()
switch response {
case .success(let result): // result == Response.T
    print(result.result)

case .error(let error):
    print(error)
}
```

## Wallet
A class which provides EOA funcstions. It enables you to create, transform to Keystore or load from Keystore.

### Initialize
