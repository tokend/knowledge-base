# Payment

Sends an amount from a source to destination Balance ID or Account ID.

## Source account details

| Property | Value |
| :--- | :--- |
| Threshold | `MEDIUM` |
| Account types | `GENERAL`, `NOT_VERIFIED`, `SYNDICATE` |
| Signer types | `BALANCE_MANAGER` |

## Parameters

| Parameter | Type | Description |
| :--- | :--- | :--- |
| source | string | Should be a valid `account ID`. Source account public key. |
| destination | string | Should be a valid `account ID` or `balance ID`. Payment recipient credential. |
| amount | string | Should be a valid `amount`. Payment amount to send. |
| fee | object |  |
| subject | string | Payment subject \(description\) |
| reference | string | Used to create unique payments. Payments with duplicated non-empty reference will be rejected. |

## Examples

```javascript
 const opertaion = PaymentV2Builder.paymentV2({
    sourceBalanceId: 'BCQOBAIMVNNH7RHZTD4OVSRUX2W575VUK4RUYELRHDPXSXJ5TMS2BHAV',
    destination: 'GBHL73YWIHZWBLFBCHEGGQZQ3WVCPW76DLO67HAITO3BXOGPLKPG7FRM',
    amount: '10.000000',
    feeData: {
      sourceFee: {
        maxPaymentFee: '0.100000',
        fixedFee: '3.000000',
        feeAsset: 'QTK'
      },
      destinationFee: {
        maxPaymentFee: '1.001',
        fixedFee: '0.001',
        feeAsset: 'QTK'
      }
    },
    subject: 'The description of the payment',
    reference: 'Some unique random string'
  })
```

## Possible errors

| Error | Code | Description |
| :--- | :--- | :--- |
| MALFORMED | -1 | Incorrect input. |
| UNDERFUNDED | -2 | Not enough funds in the source account. |
| LINE\_FULL | -3 | Destination is above their limit. |
| FEE\_MISMATCHED | -4 | Payment fee is not equal to expected fee. |
| BALANCE\_NOT\_FOUND | -5 | Destination balance not found. |
| BALANCE\_ACCOUNT\_MISMATCHED | -6 | There was an attempt to make payment using not your balance. |
| BALANCE\_ASSETS\_MISMATCHED | -7 | Source and destination balances have different assets. |
| SRC\_BALANCE\_NOT\_FOUND | -8 | Source balance not found. |
| REFERENCE\_DUPLICATION | -9 | There is already a payment with this reference \(the reference of the payment must be unique\). |
| STATS\_OVERFLOW | -10 | Statistics overflowed by the operation. |
| LIMITS\_EXCEEDED | -11 | The established limit on payments was exceeded. |
| NOT\_ALLOWED\_BY\_ASSET\_POLICY | -12 | The policy of the payment asset does not allow making payments. |
| INVOICE\_NOT\_FOUND | -13 | The invoice of the payment does not exist. |
| INVOICE\_WRONG\_AMOUNT | -14 | The invoice of the payment has wrong amount. |
| INVOICE\_BALANCE\_MISMATCH | -15 | The invoice of the payment has wrong receiver Balance ID. |
| INVOICE\_ACCOUNT\_MISMATCH | -16 | The invoice of the payment has wrong receiver Account ID. |
| INVOICE\_ALREADY\_PAID | -17 | The invoice of the payment has already been paid. |

