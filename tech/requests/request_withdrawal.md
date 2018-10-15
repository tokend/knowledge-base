# Withdrawal Request

This operation creates new `withdrawal request`.

> Although it's technically possible to make cross-asset withdrawals, the feature
is temporary disabled in TokenD.

## Review operation

To review withdrawal request, please use [Review withdrawal][1] operation.

## Source account requirements

| Property              | Value                                              |
|-----------------------|----------------------------------------------------|
| Threshold             | `MEDIUM`                                           |
| Allowed account types | `SYNDICATE`, `GENERAL`, `VERIFIED`, `NOT_VERIFIED` |
| Allowed signer types  | `BALANCE_MANAGER`                                  |

## Parameters

| Parameter               | Type   | Description                                                                                                          |
|-------------------------|--------|----------------------------------------------------------------------------------------------------------------------|
| balance                 | string | `Balance ID` from which withdraw will be performed                                                                   |
| amount                  | string | Amount to be charged from specified balance (does not include fees)                                                  |
| fee                     | object | Fee to be charged                                                                                                    |
| fee.fixed               | string | Fixed fee to be charged                                                                                              |
| fee.percent             | string | Percent fee to be charged                                                                                            |
| externalDetails         | object | External details needed for PSIM to process withdraw operation                                                       |
| destAsset               | string | Asset that requestor will receive in his external wallet (for now, use the same as `amount`)                         |
| expectedDestAssetAmount | string | Amount that requestor will receive in his external wallet (for now, use the asset specified `balanceID` belongs to)  |

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

| Error                             | Code | Description                                                                                     |
|-----------------------------------|------|-------------------------------------------------------------------------------------------------|
| INVALID_AMOUNT                    | -1   | Amount in request body is zero.                                                                 |
| INVALID_EXTERNAL_DETAILS          | -2   | External details is not stringified valid json struct or exceeded max permissible length.       |
| BALANCE_NOT_FOUND                 | -3   | Balance with such id does not exist or does not belong to source account.                       |
| ASSET_IS_NOT_WITHDRAWABLE         | -4   | Asset has not `WITHDRAWABLE` policy.                                                            |
| CONVERSION_PRICE_IS_NOT_AVAILABLE | -5   | There is no asset pair for such asset from auotConvertionDetails and asset from source balance. |
| FEE_MISMATCHED                    | -6   | Fixed fee or percent fee does not match existing fee.                                           |
| CONVERSION_OVERFLOW               | -7   | Converted amount exceeded max uint64 number.                                                    |
| CONVERTED_AMOUNT_MISMATCHED       | -8   | Expected amount from auto conversion details must match actual converted amount.                |
| BALANCE_LOCK_OVERFLOW             | -9   | Total locked amount or sum of amount to be withdrawn and total fee exceeded max uint64.         |
| UNDERFUNDED                       | -10  | There is no enough tokens on source balance.                                                    |
| INVALID_UNIVERSAL_AMOUNT          | -11  | Universal amount must be zero.                                                                  |
| STATS_OVERFLOW                    | -12  | Source stats exceeded max uint64 amount.                                                        |
| LIMITS_EXCEEDED                   | -13  | Source account limits exceeded.                                                                 |
| INVALID_PRE_CONFIRMATION_DETAILS  | -14  | Pre confiramtion details must be empty.                                                         | 
| LOWER_BOUND_NOT_EXCEEDED          | -15  | Amount to be withdrawn is less than min withdrawn amount.                                       |

[1]: /tech/requestshdrawal.md
