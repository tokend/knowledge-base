# Sign in

Signing in is basically a process of acquiring client's signing key from encrypted 
keychain data. As well as in [Wallet Creation][1], we have encapsulated all
the tricky actions in our javascript SDK, so all you need to do here is just to
get your [wallet][2]:

```javascript
const wallet = api.wallets.get('foo@bar.com', 'p@ssw0rd')
```

By doing this, you're asking SDK to perform such actions:

* Check out actual KDF parameters;
* Calculate `wallet ID` and `wallet key`;
* Get `wallet` by it's `id`;
* Decrypt account keys using `wallet key`;
* Return error if wallet is not verified;

As a result, you'll get [Wallet][3] instance, that will contain decrypted keys,
so you can start use them for signing requests and transactions.


```javascript
console.log(wallet.id) // => 4aadcd4eb44bb845d828c45dbd68d5d1196c3a182b08cd22f05c56fcf15b153c
console.log(wallet.email) // => foo@bar.com
// seed for signing keypair:
console.log(wallet.secretSeed) // => SAPSZ3ODNFYAESQEPGL72MAXFNL7RQTFBLYDD32DICJHYQNA36LGNGBN
// The account identifier, NOT the PK from signing keypair: 
console.log(wallet.accountId) // => GAIEBMXUPSGW2J5ELJFOY6PR5IWXXJNHIJSDKTDHK76HHRNYRL2QYU4O
```

To start using wallet through SDK, you can do the following:

```javascript
sdk.useWallet(wallet)
```

After this, SDK will automatically sign every request and operations with your 
signing keypair. That means now you can provide your user with all the 
functionality TokenD has.

[1]: /guides/sign_in.md
[2]: /other/wallets.md
[3]: https://tokend.gitlab.io/new-js-sdk/Wallet.html
