# Manage Balance

Adds a balance to the account.

## Source account details

| Property | Value |
| :--- | :--- |
| Threshold | `HIGH` |
| Account types | `GENERAL`, `NOT_VERIFIED`, `SYNDICATE` - if `source` and `destination` are the same, `MASTER` - if not |
| Signer types | `BALANCE_MANAGER` |

## Parameters

| Parameter | Type | Description |
| :--- | :--- | :--- |
| asset | string | The balance will hold tokens for this code |
| destination | string | The [Account id](../key-entities/accounts.md#account-id) that will own the balance |
| action | int | The [XDR](../xdr.md) manage balance action |

> Note: currently, deleting balance is not allowed despite there is an XDR action for this.

## Examples

For example, you are the owner of `GANIVSARNMANTE7QRWZIB7543YHTF4243IHJGLZTSCJ4MRBWJZLPLG5J` account ID and you want to create the balance in order to hold the `QTK` tokens. What you do is:

```javascript
const operation = base.Operation.ManageBalance({
  action: xdr.ManageBalanceAction.create(),
  asset: 'QTK',
  destination: 'GANIVSARNMANTE7QRWZIB7543YHTF4243IHJGLZTSCJ4MRBWJZLPLG5J'
})

await horizon.transaction.submitOperations(operation)
```

## Possible errors

| Error | Code | Description |
| :--- | :---: | :--- |
| MALFORMED | -1 | The action is temporarily disabled, |
| DESTINATION\_NOT\_FOUND | -3 | Destination `account ID` doesn't exist in the system |
| ASSET\_NOT\_FOUND | -4 | There is no such asset in the system |
| INVALID\_ASSET | -5 | Provided asset code is invalid |
| BALANCE\_ALREADY\_EXISTS | -6 | The balance for such asset already exists \(will be thrown only with `createUnique` action\) |

