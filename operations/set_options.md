# Set options  

This operation sets the options for an account, Mainly used for managing account signers. 

The list of parameters:

| Parameter             | Type | Description        |
|-----------------------|------|--------------------|
| masterWeight          |   int   | The weight of master signer (use it only if you want to remove master from signers) |
| signer                |   object   | Signer you're adding/removing (it's properties define the action) |
| signer.weight         |   int   | The weight of the signer |
| signer.identity       |   int   | The identity of the signer |
| signer.pubKey         |   string   | The public key of the signer |
| signer.signerType     |   int   | The signer type (the bit sum of signer types) |
| signer.name           |   string   | The name of the sig ner |
| lowThreshold          |   int   | The minimum weight limit to submit low operations |
| medThreshold          |   int   | The minimum weight limit to submit medium operations |
| highThreshold         |   int   | The minimum weight limit to submit high operations |


## Admin thresholds and weights

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
    signerType: signerTypeAll(),
    weight: 255,
    identity: '',
    name: ''
  }
})
```




>You can omit this information when managing user signers, just always use











## Use cases

The `Set options` operation is mainly used to perform several actions.

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



Although you can as many signers as you want for your account, usually it's needed to have only one.  




/**
     * "set options" operations set or clear account flags,
     * set the account's inflation destination, and/or add new signers to the account.
     * The flags used in `opts.clearFlags` and `opts.setFlags` can be the following:
     *   - `{@link AuthRequiredFlag}`
     *   - `{@link AuthRevocableFlag}`
     *   - `{@link AuthImmutableFlag}`
     *
     * It's possible to set/clear multiple flags at once using logical or.
     * @param {object} opts
     * @param {number|string} [opts.masterWeight] - The master key weight.
     * @param {number|string} [opts.lowThreshold] - The sum weight for the low threshold.
     * @param {number|string} [opts.medThreshold] - The sum weight for the medium threshold.
     * @param {number|string} [opts.highThreshold] - The sum weight for the high threshold.
     * @param {object} [opts.signer] - Add or remove a signer from the account. The signer is
     *                                 deleted if the weight is 0.
     * @param {string} [opts.signer.pubKey] - The public key of the new signer (old `address` field name is deprecated).
     * @param {number|string} [opts.signer.weight] - The weight of the new signer (0 to delete or 1-255)
     * @param {number|string} [opts.signer.signerType] - The type of the new signer
     * @param {number|string} [opts.signer.identity] - The identity of the new signer
     * * @param {string} [opts.signer.name] - Name of the signer
     * @param {object} [opts.limitsUpdateRequestData] - required data for LimitsUpdateRequest creation
     * * @param {string} [opts.limitsUpdateRequestData.documentHash] - hash of the document to review
     * @param {string} [opts.source] - The source account (defaults to transaction source).
     * @returns {xdr.SetOptionsOp}
     * @see [Account flags](https://www.stellar.org/developers/guides/concepts/accounts.html#flags)
     */