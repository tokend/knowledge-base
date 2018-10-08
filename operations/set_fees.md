# Set fees

Operation `Set fees` is used to manage fees imposed on accounts that are part of the TokenD system. You 
can define global fees as well as specific fees imposed on account/account type. The fee system is very flexible,
so you can set any fees on any operation or even at specific range.

First of all, there are 2 fee types existing in the system: payment operation fees and transaction fees 
(you can consider it a gas). 

By `payment operation` we mean any operation that will somehow change someones balance. This fees can have both
fixed and percent parts and the percent part of the fee will be dependable of amount.

Transaction fees, on the other hand, allow only fixed part to be set. 

| Parameter    | Type   | Description           |
|--------------|--------|-----------------------|
| fixedFee     | string | Fixed fee that will be charged for every operation |
| percentFee   | string | Percent fee that will be charged for every operation |
| feeType      | int    | The [XDR][xdr] type of the fee |
| subtype      | int    | The [XDR][xdr] subtype of the fee (some fees have their subtypes, for example `paymentV2` has `incoming` and `outgoing` subtypes) |
| asset        | string | Fee asset (is allowed to set only on `paymentV2` fees) |
| lowerBound   | string | The lower bound of the operation. It means fee rule won't be applied to operations with lower amount |
| upperBound   | string | The upper bound of the operation. It means fee rule won't be applied to operations with higher amount |
| isDelete     | bool   | If set to `true`, the operation will delete the fee rule, if such exists |


[xdr]: /operations/xdr_enums.md