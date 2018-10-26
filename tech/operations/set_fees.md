# Set fees

The process of fees management in TokenD includes the following operations:

* Creating new fee;
* Updating existing fee;
* Deleting existing fee.

TokenD is provided with a flexible and feature-rich set of tools for fees management, which allows creating custom fees imposition rules by choosing and combining the following parameters:
* Account (types of accounts, any specific account, or all)
* Asset (any specific asset-backed token on the platform issued both by admins and by users)
* Operation type (withdrawal, invest, issuance, payment (outgoing, incoming), and order match)
* Flat fee (a fixed fee)
* Percentage fee 
* Boundaries (define the boundaries that apply the imposition of different fee-charging scenarios. Example: you can specify that 2% fee will be imposed exclusively on the  USD withdrawals in the range between $100 and $3000.)

## Source account details

| Property              | Value              |
|-----------------------|--------------------|
| Threshold             | `HIGH`               |
| Allowed account types | `MASTER`	     |
| Allowed signer types  | `FEES_MANAGER`     |

## Parameters

| Parameter        | Type   | Description           |
|------------------|--------|-----------------------|
| fee              | object | Fixed fee that will be charged for every operation |
| fee.fixedFee     | string | Fixed fee that will be charged for every operation |
| fee.percentFee   | string | Percentage fee that will be charged for every operation |
| fee.feeType      | int    | The [XDR][1] type of the fee |
| fee.subtype      | int    | The [XDR][1] subtype of the fee (some fees have their subtypes, for example `paymentV2` has `incoming` and `outgoing` subtypes) |
| fee.asset        | string | The operation asset (fees will apply to operations with specific assets) |
| fee.lowerBound   | string | The lower bound of operation. This means that the fee rule won't be applied to operations with lower amount |
| fee.upperBound   | string | The upper bound of operation. This means that the fee rule won't be applied to operations with higher amount |
| isDelete         | bool   | If set `true`, the operation will delete the fee rule, if such exists |


## Examples

Let's create a global fee on all withdrawals in the system:

```javascript
const operation = base.Operation.SetFees({
  fee: {
    feeType: xdr.FeeType.withdrawalFee(), // every withdrawal
      asset: 'BTC', // of BTC
      lowerBound: '0.05', // that 0.05 BTC or higher
      upperBound: '4', // and 4 BTC or lower
      fixedFee: '0.100000', // will charge 0.1 BTC fee
      percentFee: '0.050000', // and (opAmount * 0.05) percent fee
      subtype: 0 // Withdrawal fees have no subtypes
  }
})
```
Now, let's create a fee only for outgoing payments (it means that we define fees paid by the sender)

```javascript
const operation = base.Operation.SetFees({
  fee: {
    feeType: xdr.FeeType.paymentFee(), // every payment
    subtype: xdr.PaymentFeeType.outgoing().value, // that is outgoing,
    asset: 'ETH', // and is ETH payment
    fixedFee: '0.050000', // will charge 0.05 ETH fee
    percentFee: '0.001000' // and (opAmount * 0.001) ETH percent fee
  }
})
```

## Possible errors

| Error                       | Code | Description                                                                               |
|-----------------------------|:----:|-------------------------------------------------------------------------------------------|
| INVALID_AMOUNT              |  -1  | Fixed fee or percentage fee is invalid.                                                      |
| INVALID_FEE_TYPE            |  -2  | Fee type does not exist.                                                                  |
| ASSET_NOT_FOUND             |  -3  | There is no asset (with such asset code) for which the fee will be charged. |
| INVALID_ASSET               |  -4  | Token asset code for which the fee is created is invalid.                                 |
| MALFORMED                   |  -5  | Fee entry is invalid due to certain reasons.                                                           |
| MALFORMED_RANGE             |  -6  | Fee with such type must use full range.                                                   |
| RANGE_OVERLAP    	      |  -7  | Range of new and old fees with different bounds have overlaped.      |
| NOT_FOUND          	      |  -8  | There is no such fee entry.                                                               |
| SUB_TYPE_NOT_EXIST   	      |  -9  | Fee entry must contain a subtype equal to zero.                                             |
| INVALID_FEE_VERSION         | -10  | Ledger does not support such ledger version in extension of operation body.               |
| INVALID_FEE_ASSET  	      | -11  | Asset code of token for which the fee will be charged is invalid.                                |
| FEE_ASSET_NOT_ALLOWED       | -12  | It is not allowed to use the fee asset for such fee type                                            |
| CROSS_ASSET_FEE_NOT_ALLOWED | -13  | Cross asset fee allowed only for outgoing payment fees.				         |
| FEE_ASSET_NOT_FOUND 	      | -14  | There is no asset with such asset code of tokens which will charged for the fee.        |
| ASSET_PAIR_NOT_FOUND 	      | -15  | There is no asset pair for such asset and fee asset.		                         |
| INVALID_ASSET_PAIR_PRICE    | -16  | Price of asset pair can not be less than zero.		                                 |

[1]: /tech/xdr.md
