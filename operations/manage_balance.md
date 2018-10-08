# Manage balance

Manage balance operations has it's goal to add balance to the account.

| Parameter    | Type   | Description           |
|--------------|--------|-----------------------|
| asset        | string | The balance will hold tokens for this code |
| destination  | string | The [Account id][account_id] that will own the balance
| action       | int    | The [XDR][xdr] manage balance action |

For example, we are the owner of `GANIVSARNMANTE7QRWZIB7543YHTF4243IHJGLZTSCJ4MRBWJZLPLG5J` account ID and we want to 
create the balance for holding `QTK` tokens. Simply doing this:

```javascript
const action = xdr.ManageBalanceAction.create()
const asset = 'QTK'
const destination = 'GANIVSARNMANTE7QRWZIB7543YHTF4243IHJGLZTSCJ4MRBWJZLPLG5J'

const operation = base.Operation.ManageBalance({
  action,
  asset,
  destination
})

await horizon.transaction.submitOperations(operation)
```

[account_id]: /coming_soon.md
[xdr]: /operations/xdr_enums.md