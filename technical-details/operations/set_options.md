# Set options

Sets the options for an account. Mainly used for managing account signers.

## Source account details

| Property | Value |
| :--- | :--- |
| Threshold | `HIGH` |
| Account types | `GENERAL`, `NOT_VERIFIED`, `SYNDICATE`, `MASTER` |
| Signer types | `ACCOUNT_MANAGER` |

## Parameters

| Parameter | Type | Description |
| :--- | :--- | :--- |
| masterWeight | int | The weight of master signer \(use it only if you want to remove master from signers\) |
| signer | object | Signer you are adding/removing \(the action performed relies on the properties\) |
| signer.weight | int | The weight of the new signer, 0 to delete old |
| signer.identity | int | The identity of the new signer |
| signer.pubKey | string | The public key of the new signer |
| signer.signerType | int | The signer type \(the bit sum of signer types\) |
| signer.name | string | The name of the signer |
| lowThreshold | int | The minimum weight limit to submit low operations |
| medThreshold | int | The minimum weight limit to submit medium operations |
| highThreshold | int | The minimum weight limit to submit high operations |

## Use cases

### Admins management

Admin operations fall under a specific threshold category: low, medium, or high. The threshold for a given level can be set to any number from 0-255. This threshold is the amount of signature weight required to authorize an operation at that level.

To add a new admin, you simply need to add the new signer to master account. For example, you want to add a signer who will manage assets:

```javascript
const signerTypeAssetManager = xdr.SignerTypes.AssetManager().value
const operation = base.SetOptionsBuilder.setOptions({
  signer: {
    pubKey: 'GBOVLETLPZEAQZLH5LANCTAFTN3XCJ446KEHLCD3TNDPR6YDSPCBUMBW',
    signerType: signerTypeAssetManager,
    weight: 255,
    identity: '',
    name: ''
  }
})
```

To remove an admin, you will need to delete the signer from the master account:

```javascript
const signer = {
  pubKey: 'GBOVLETLPZEAQZLH5LANCTAFTN3XCJ446KEHLCD3TNDPR6YDSPCBUMBW',
  weight: 255,
  identity: 1,
  signerType: 16
}

const operation = base.Operation.setOptions({
  ...signer,
  weight: 0
})
```

To remove the master craft `setOptions` operation with masterWeight param:

```javascript
const operation = base.Operation.setOptions({
  masterWeight: 0
})
```

### Change password

When an account is created, it has 1 signer \(master signer\), the keypair of which is stored encrypted with user's password. So, when user changes their password, it is recommended to to replace their signer with the new one \(for the safety reasons\). You create a new signer and remove the old one \(specifically in this order\). To add a new signer, you just need to craft the `Set options` with one field - `signer` and provide his settings.

We will add the signer with all types. That is why we need to be sure that it is an actual value every time we are updating the signers. We can get their values from XDR enums:

```javascript
function signerTypeAll () {
  return xdr.SignerType.values()
    .map(value => value.value)
    .reduce((total, value) => value | total)
}
```

Also, we should know the maximum possible weight \(for this case, it is the only possible value\):

```javascript
const maxWeight = 255
```

And use any number as a signer `identity`:

```javascript
const identity = 1
```

Now, we can simply add a new signer:

```javascript
const addSignerOperation = base.SetOptionsBuilder.setOptions({
  signer: {
    pubKey: 'GBOVLETLPZEAQZLH5LANCTAFTN3XCJ446KEHLCD3TNDPR6YDSPCBUMBW',
    signerType: signerTypeAll(),
    weight: 255,
    identity: 1,
    name: 'my signer'
  }
})
```

and remove the old one:

```javascript
const signer = {
  pubKey: 'GBOVLETLPZEAQZLH5LANCTAFTN3XCJ446KEHLCD3TNDPR6YDSPCBUMBW',
  weight: 255,
  identity: 1,
  signerType: signerTypeAll()
}

const operation = base.Operation.setOptions({
  ...signer,
  weight: 0
})
```

## Possible errors

| Error | Code | Description |
| :--- | :---: | :--- |
| TOO\_MANY\_SIGNERS | -1 | Max number of signers is already reached. |
| THRESHOLD\_OUT\_OF\_RANGE | -2 | Incorrect value for weight or threshold. |
| BAD\_SIGNER | -3 | The operations signer is a master key but signer cannot be a master key. |
| INVALID\_SIGNER\_VERSION | -7 | The signer version is higher than the ledger version. |

