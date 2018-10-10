# Set fees

> TODO: threshold

Manage fees that will be charged from TokenD accounts, includes:

* Creating new fee;
* Updating existing fee;
* Deleting existing fee.

You can define global fees as well as specific fees imposed on account/account type. The fee system is very flexible,
so you can set any fees on any operation or even at specific range.

Fee may be set to any operation that will somehow change someones balance. Such fees can have both
fixed and percent parts and the percent part of the fee will be dependable of amount.

## Source account details

| Property              | Value              |
|-----------------------|--------------------|
| Threshold             | high               |
| Allowed account types | `MASTER`	     |
| Allowed signer types  | `FEES_MANAGER`     |

## Parameters

| Parameter        | Type   | Description           |
|------------------|--------|-----------------------|
| fee              | object | Fixed fee that will be charged for every operation |
| fee.fixedFee     | string | Fixed fee that will be charged for every operation |
| fee.percentFee   | string | Percent fee that will be charged for every operation |
| fee.feeType      | int    | The [XDR][1] type of the fee |
| fee.subtype      | int    | The [XDR][1] subtype of the fee (some fees have their subtypes, for example `paymentV2` has `incoming` and `outgoing` subtypes) |
| fee.asset        | string | The operation asset (fees will apply to operations with specific asset) |
| fee.lowerBound   | string | The lower bound of the operation. It means fee rule won't be applied to operations with lower amount |
| fee.upperBound   | string | The upper bound of the operation. It means fee rule won't be applied to operations with higher amount |
| isDelete         | bool   | If set to `true`, the operation will delete the fee rule, if such exists |


## Examples

Let's, for example create global fees on withdrawals:

```javascript
const operation = base.Operation.SetFees({
  feeType: xdr.FeeType.withdrawalFee(), // every withdrawal
  asset: 'BTC', // of BTC
  lowerBound: '0.05', // that 0.05 BTC or higher
  upperBound: '4', // and 4 BTC or lower
  fixedFee: '0.100000', // will charge 0.1 BTC fee
  percentFee: '0.050000', // and (opAmount * 0.05) percent fee
  subtype: 0 // Withdrawal fees have no subtypes
})
```
Or some fees only on outgoing payments (it means we define fees paid by sender)

```javascript
const operation = base.Operation.SetFees({
  feeType: xdr.FeeType.paymentFee(), // every payment
  subtype: xdr.PaymentFeeType.outgoing().value, // that is outgoing,
  asset: 'ETH', // and is ETH payment
  fixedFee: '0.050000', // will charge 0.05 ETH fee
  percentFee: '0.001000' // and (opAmount * 0.001) ETH percent fee
})
```

## Possible errors

| Error                       | Code | Description                                                                               |
|-----------------------------|:----:|-------------------------------------------------------------------------------------------|
| INVALID_AMOUNT              |  -1  | Fixed fee or percent fee is invalid.                                                      |
| INVALID_FEE_TYPE            |  -2  | Fee type does not exist.                                                                  |
| ASSET_NOT_FOUND             |  -3  | There is no asset with such asset code of tokens for which the commision will be charged. |
| INVALID_ASSET               |  -4  | Asset code of tokens for which fee is created is invalid.                                 |
| MALFORMED                   |  -5  | Fee entry is invalid in some way                                                          |
| MALFORMED_RANGE             |  -6  | Fee with such type must use full range.                                                   |
| RANGE_OVERLAP    	      |  -7  | Range of new and old fees with diffenrent only bounds mustn't have overlaped bounds.      |
| NOT_FOUND          	      |  -8  | There is no such fee entry.                                                               |
| SUB_TYPE_NOT_EXIST   	      |  -9  | Fee entry must contains subtype equaled zero.                                             |
| INVALID_FEE_VERSION         | -10  | Ledger does not support such ledger version in extension of operation body.               |
| INVALID_FEE_ASSET  	      | -11  | Asset code of tokens for which will be charged is invalid.                                |
| FEE_ASSET_NOT_ALLOWED       | -12  | Not allowed to use fee asset for such fee type                                            |
| CROSS_ASSET_FEE_NOT_ALLOWED | -13  | Cross asset fee allowed only for outgoing payment fee.				         |
| FEE_ASSET_NOT_FOUND 	      | -14  | There is no asset with such asset code of tokens which will charged for commision.        |
| ASSET_PAIR_NOT_FOUND 	      | -15  | There is no asset pair for such asset and fee asset.		                         |
| INVALID_ASSET_PAIR_PRICE    | -16  | Price of asset pair can not be less than zero.		                                 |

[1]: /tech/operations/xdr_enums.md