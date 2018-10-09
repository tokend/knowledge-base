# Set fees

Manage fees that will be charged from TokenD accounts.

You can define global fees as well as specific fees imposed on account/account type. The fee system is very flexible,
so you can set any fees on any operation or even at specific range.

Fee may be set to any operation that will somehow change someones balance. This fees can have both
fixed and percent parts and the percent part of the fee will be dependable of amount.

Transaction fees, on the other hand, allow only fixed part to be set. 

| Parameter    | Type   | Description           |
|--------------|--------|-----------------------|
| fixedFee     | string | Fixed fee that will be charged for every operation |
| percentFee   | string | Percent fee that will be charged for every operation |
| feeType      | int    | The [XDR][xdr] type of the fee |
| subtype      | int    | The [XDR][xdr] subtype of the fee (some fees have their subtypes, for example `paymentV2` has `incoming` and `outgoing` subtypes) |
| asset        | string | The operation asset (fees will apply to operations with specific asset) |
| lowerBound   | string | The lower bound of the operation. It means fee rule won't be applied to operations with lower amount |
| upperBound   | string | The upper bound of the operation. It means fee rule won't be applied to operations with higher amount |
| isDelete     | bool   | If set to `true`, the operation will delete the fee rule, if such exists |

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
[xdr]: /operations/xdr_enums.md