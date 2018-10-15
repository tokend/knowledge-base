# Review withdrawal request

This operation allows review `withdrawal request`.

__Note__: Review withdrawal request is the final operation to perform withdraw.

## Source account details

| Property              | Value                 |
|-----------------------|-----------------------|
| Threshold             | high                  |
| Allowed account types | `MASTER`, `SYNDICATE` |
| Allowed signer types  | `WITHDRAW_MANAGER`    |

## Request details - Withdrawal Details

| Parameter       | Type   |       Description             |
|:---------------:|:------:|:-----------------------------:|
| externalDetails | string | Stringified json struct data. |

## Possible errors

Only general errors can be returned

