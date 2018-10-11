# Payment

Threshold: `Medium`

Sends an amount from a source to destination Balance ID or Account ID.

## Source account details

| Property              | Value              |
|-----------------------|--------------------|
| Threshold             | `MEDIUM`           |
| Account types         | `GENERAL`, `NOT_VERIFIED`, `SYNDICATE` |
| Signer types          | `BALANCE_MANAGER`  |

## Parameters

| Parameter   | Type   | Description                                                                                        |
|-------------|--------|----------------------------------------------------------------------------------------------------|
| source      | string | Should be a valid `account ID`. Source account public key.                                         |
| destination | string | Should be a valid `account ID` or `balance ID`. Payment recipient credential.                      |
| amount      | string | Should be a valid `amount`. Payment amount to send.                                                |
| fee         | object |                                                                                                    |
| subject     | string | Payment subject (description)                                                                      |
| reference   | string | Used to create unique payments. Payments with duplicated non-empty reference will be rejected.     | 

## Possible errors

| Error  | Code   | Description                                                                                        |
|--------|--------|----------------------------------------------------------------------------------------------------|
| MALFORMED                   | -1	| Bad input. |
| UNDERFUNDED                 | -2	| Not enough funds in source account. |
| LINE_FULL                   | -3	| Destination would go above their limit. |
| FEE_MISMATCHED              | -4	| Payment fee is not equal to expected fee. |
| BALANCE_NOT_FOUND           | -5	| Destination balance not found. |
| BALANCE_ACCOUNT_MISMATCHED  | -6	| There was an attempt to make payment using not your balance. |
| BALANCE_ASSETS_MISMATCHED   | -7	| Source and destination balances have different assets. |
| SRC_BALANCE_NOT_FOUND       | -8	| Source balance not found. |
| REFERENCE_DUPLICATION       | -9	| The reference of the payment must be unique. But there is already a payment with this reference. |
| STATS_OVERFLOW              | -10	| Statistics overflowed by the operation. |
| LIMITS_EXCEEDED             | -11	| The established limit on payments was exceeded. |
| NOT_ALLOWED_BY_ASSET_POLICY | -12	| The policy of the payment asset does not allow making payments. |
| INVOICE_NOT_FOUND           | -13	| The invoice of the payment does not exist. |
| INVOICE_WRONG_AMOUNT        | -14	| The invoice of the payment has wrong amount. |
| INVOICE_BALANCE_MISMATCH    | -15	| The invoice of the payment has wrong receiver Balance ID. |
| INVOICE_ACCOUNT_MISMATCH    | -16	| The invoice of the payment has wrong receiver Account ID. |
| INVOICE_ALREADY_PAID        | -17	| The invoice of the payment has already been paid. |


















