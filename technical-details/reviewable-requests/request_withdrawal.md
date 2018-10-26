# Request \(Withdrawal\)

This operation creates a new `withdrawal request`.

> Although it is technically possible to make cross-asset withdrawals, the feature is temporary disabled in TokenD.

## Review operation

To review the withdrawal request, use [Review withdrawal](https://github.com/tokend/knowledge-base/tree/b05843eec76dc0419e6c40b2af25520f7358034c/tech/requestshdrawal.md) operation.

## Source account requirements

| Property | Value |
| :--- | :--- |
| Threshold | `MEDIUM` |
| Allowed account types | `SYNDICATE`, `GENERAL`, `VERIFIED`, `NOT_VERIFIED` |
| Allowed signer types | `BALANCE_MANAGER` |

## Parameters

| Parameter | Type | Description |
| :--- | :--- | :--- |
| balance | string | `Balance ID` from which withdraw will be performed |
| amount | string | Amount to be charged from the specified balance \(excluding fees\) |
| fee | object | Fee to be charged |
| fee.fixed | string | Fixed fee to be charged |
| fee.percent | string | Percent fee to be charged |
| externalDetails | object | External details needed for PSIM to process withdraw operation |
| destAsset | string | Asset that requestor will receive in their external wallet \(for now, it is the same as `amount`\) |
| expectedDestAssetAmount | string | Amount that requestor will receive in their external wallet \(for now, use the asset specified `balanceID` belongs to\) |

## Examples

```javascript
const operation = base.CreateWithdrawRequestBuilder.createWithdrawWithAutoConversion({
    balance: 'BD2U4FYCQ6TEVXJZAFP2VB22NKFBLJKKVM625DEM4BXMQN6AOZFTHQAB', // BTC
    amount: '1.000',
    fee: {
      fixed: '0.010000',
      percent: '0.020000'
    },
    externalDetails: {},
    destAsset: 'BTC',
    expectedDestAssetAmount: '1.000'
})
```

## Possible errors

| Error | Code | Description |
| :--- | :--- | :--- |
| INVALID\_AMOUNT | -1 | Amount in request body is zero. |
| INVALID\_EXTERNAL\_DETAILS | -2 | External details are not stringified valid json struct or exceeded max permissible length. |
| BALANCE\_NOT\_FOUND | -3 | Balance with such id does not exist or does not belong to source account. |
| ASSET\_IS\_NOT\_WITHDRAWABLE | -4 | Asset does not have a `WITHDRAWABLE` policy. |
| CONVERSION\_PRICE\_IS\_NOT\_AVAILABLE | -5 | There is no asset pair for such asset from auotConvertionDetails and asset from source balance. |
| FEE\_MISMATCHED | -6 | Fixed fee or percent fee does not match the existing fee. |
| CONVERSION\_OVERFLOW | -7 | Converted amount exceeded max uint64 number. |
| CONVERTED\_AMOUNT\_MISMATCHED | -8 | Expected amount from auto conversion details must match actual converted amount. |
| BALANCE\_LOCK\_OVERFLOW | -9 | Total locked amount or sum of amount to be withdrawn and total fee exceeded max uint64. |
| UNDERFUNDED | -10 | There is no enough tokens on the source balance. |
| INVALID\_UNIVERSAL\_AMOUNT | -11 | Universal amount must be zero. |
| STATS\_OVERFLOW | -12 | Source stats exceeded max uint64 amount. |
| LIMITS\_EXCEEDED | -13 | Source account limits exceeded. |
| INVALID\_PRE\_CONFIRMATION\_DETAILS | -14 | Pre-confiramtion details must be empty. |
| LOWER\_BOUND\_NOT\_EXCEEDED | -15 | Amount to be withdrawn is less than the min withdrawn amount. |

