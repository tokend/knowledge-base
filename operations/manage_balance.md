# Manage balance

Adds a balance to account.  

| Parameter    | Type   | Description           |
|--------------|--------|-----------------------|
| asset        | string | The balance will hold tokens for this code |
| destination  | string | The [Account id][account_id] that will own the balance
| action       | int    | The [XDR][xdr] manage balance action |

> Note: for now, the deletion of balance is not allowed despite the fact there is an XDR action for this.

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

[account_id]: /coming_soon.md
[xdr]: /operations/xdr_enums.md