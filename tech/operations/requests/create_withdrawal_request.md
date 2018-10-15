# Create Issuance Request

This operation creates new `withdrawal request`.

## Source account details

| Property              | Value                                                                                                                          |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------|
| Threshold             | `MEDIUM`                                                                                                                         |
| Allowed account types | `SYNDICATE`, `GENERAL`, `ACCREDITED_INVESTOR`, `INSTITUTIONAL_INVESTOR`, `VERIFIED`, `NOT_VERIFIED`, `OPERATIONAL`, `EXCHANGE` |
| Allowed signer types  | `BALANCE_MANAGER`                                                                                                              |

## Parameters

| Parameter |       Type        | Description                       |
|:---------:|:-----------------:|:---------------------------------:|
|  request  | WithdrawalRequest | Body of the request to be created |

## Possible errors

| Error                             | Code | Description                                                                                     |
|-----------------------------------|:----:|-------------------------------------------------------------------------------------------------|
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

## Successful result

Successful result has the following fields:

* __Request ID__: id of the request generated in core.

