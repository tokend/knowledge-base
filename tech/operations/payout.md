# Payout

This operation allows payout dividends to the asset holders.

Source - user which perform payout operation.

AssetHolder - user which has amount of specific asset.

Payout is distribution some amount of some asset from Source balance to AssetHolders balances.


PayoutAmount (for one user) = maxPayoutAmount * assetHolderAmount / issuedAmount. 

If assetHolderAmount < minAssetHolderAmount or payoutAmount < minAssetHolderAmount, assetHolder will not be participant of payout operation.

## Source account details

| Property              | Value                 |
|-----------------------|-----------------------|
| Threshold             | high                  |
| Allowed account types | `MASTER`, `SYNDICATE` |
| Allowed signer types  | `BALANCE_MANAGER`     |

## Parameters

| Parameter            |    Type   |       Description                                                                                   |
|:--------------------:|:---------:|:---------------------------------------------------------------------------------------------------:|
|   asset              | AssetCode | Asset code of tokens which holders will received dividends                                          |
|   sourceBalacneID    | BalanceID | ID of balance from which payout will be performed                                                   |
|   maxPayoutAmount    |  uint64   | Max tokens amount, that that will be charged from source balance                                    |
|   minPayoutAmount    |  uint64   | If calculated payout amount for holder less than minPayoutAmount, holder will not recieve dividends |
| minAssetHolderAmount |  uint64   | Min tokens amount on one Assetholder balance                                                        |
| fee 		       |   Fee     | Expected fee that will be charged from source balance                                               |

## Possible errors

| Error                     | Code | Description                                                                                                     |
|---------------------------|:----:|-----------------------------------------------------------------------------------------------------------------|
| INVALID_AMOUNT            |  -1  | Max payout amount can not be zero.                                                                              |
| INVALID_ASSET             |  -2  | Asset code invalid in some way.                                                                                 |
| ASSET_NOT_FOUND    	    |  -3  | There is no asset with such asset code and owner id (source accoount id).	                                     |
| ASSET_NOT_TRANSFERABLE    |  -4  | Asset must have transferable policy.                                                                            |
| BALANCE_NOT_FOUND         |  -5  | There is no balance with such balance id and account id (source account id).                                    |
| INSUFFICIENT_FEE_AMOUNT   |  -6  | Actual total fee amount more than expected total fee amount.                                                    |
| FEE_EXCEEDS_ACTUAL_AMOUNT |  -7  | Actual total fee amount more than actual payout total amount.                                                   |
| TOTAL_FEE_OVERFLOW        |  -8  | Unexpected state, sum of fixed and calculated percent fee exceeded max uint64 amount.                           |
| UNDERFUNDED               |  -9  | There is no enough amount on source balance to pay fee and perfrom payout. 			             |
| HOLDERS_NOT_FOUND         | -10  | There are no holders balances of such asset which have total amount equal or more than min asset holder amount. |
| MIN_AMOUNT_TOO_BIG        | -11  | There are no receivers which can recieve dividents amount equal or more min payout amount.			     |
| LINE_FULL                 | -12  | Destination balance amount exceeded max uint64 amount.							     |
| STATS_OVERFLOW            | -13  | Source stats exceeded max uint64 amount.									     |
| LIMITS_EXCEEDED           | -14  | Source account limits exceeded.									             |

## Successful result

Successful result has the following field:

* __Payout responses__: array of payout response.

* __Actual payout amount__: actual total amount that was payouted to asset holders.

* __Actual fee__: fee that was charged from source balance.

### PayoutResponse

* __Receiver ID__: id of receiver dividents account.

* __Receiver balance ID__: id of balance that was funded.

* __Received amount__: amount of source balance asset tokens received by asset holder.

