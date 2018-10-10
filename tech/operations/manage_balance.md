# Manage balance

Adds a balance to the account.  

## Source account details

> TODO: add source account details

## Parameters

| Parameter    | Type   | Description           |
|--------------|--------|-----------------------|
| asset        | string | The balance will hold tokens for this code |
| destination  | string | The [Account id][1] that will own the balance
| action       | int    | The [XDR][2] manage balance action |

> Note: for now, the deletion of balance is not allowed despite the fact there is an XDR action for this.

## Examples

For example, we are the owner of `GANIVSARNMANTE7QRWZIB7543YHTF4243IHJGLZTSCJ4MRBWJZLPLG5J` account ID and we want to 
create the balance for holding `QTK` tokens. Simply doing this:

```javascript
const operation = base.Operation.ManageBalance({
  action: xdr.ManageBalanceAction.create(),
  asset: 'QTK',
  destination: 'GANIVSARNMANTE7QRWZIB7543YHTF4243IHJGLZTSCJ4MRBWJZLPLG5J'
})

await horizon.transaction.submitOperations(operation)
```

## Possible errors

> TODO: add possible errors

[1]: /coming_soon.md
[2]: /tech/operations/xdr_enums.md