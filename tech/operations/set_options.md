# Set options  

Sets the options for an account. Mainly used for managing account signers. 

## Source account details

> TODO: add source account details

## Parameters

| Parameter             | Type | Description        |
|-----------------------|------|--------------------|
| masterWeight          |   int      | The weight of master signer (use it only if you want to remove master from signers) |
| signer                |   object   | Signer you're adding/removing (it's properties define the action) |
| signer.weight         |   int      | The weight of the new signer, 0 to delete old |
| signer.identity       |   int      | The identity of the new signer |
| signer.pubKey         |   string   | The public key of the new signer |
| signer.signerType     |   int      | The signer type (the bit sum of signer types) |
| signer.name           |   string   | The name of the sig ner |
| lowThreshold          |   int      | The minimum weight limit to submit low operations |
| medThreshold          |   int      | The minimum weight limit to submit medium operations |
| highThreshold         |   int      | The minimum weight limit to submit high operations |

## Use cases

### Admins management

Admin operations fall under a specific threshold category: low, medium, or high. The threshold for a given level 
can be set to any number from 0-255. This threshold is the amount of signature weight required to authorize an operation at 
that level.

To add a new admin, you'll simply need to add the new signer to master account. For example we want to add signer who will
manage assets:

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

To remove an admin, you'll need to delete the signer from the master account:

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

###  Change password

When an account is being created - it has 1 signer (master signer), which keypair is being stored encrypted with user's
password. So, when your user changes his password, it's a good idea to replace his signer with new one for safety.
You're simply creating a new signer and removing the old one (literally in this order). To add the new signer, just 
craft the `Set options` with one field - `signer` and provide his settings.

We will add the signer with all types, so we'll need to be sure it's an actual value every time we are updating signers.
We can get their values from XDR enums:

```javascript
function signerTypeAll () {
  return xdr.SignerType.values()
    .map(value => value.value)
    .reduce((total, value) => value | total)
}
```

Also we should know the maximum possible weight (for this case it's the only possible value):

```javascript
const maxWeight = 255
```

And use any number as signer `identity`:

```javascript
const identity = 1
```

And now we can simply add a new signer:

```javascript

const addSignerOperation = base.SetOptionsBuilder.setOptions({
  signer: {
    pubKey: 'GBOVLETLPZEAQZLH5LANCTAFTN3XCJ446KEHLCD3TNDPR6YDSPCBUMBW',
    signerType: signerTypeAll(),
    weight: 255,
    identity: '',
    name: ''
  }
})
```
and remove the old one:

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

## Possible errors

> TODO: add possible errors
