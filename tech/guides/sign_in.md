# Sign in

The sign in process essentially is a process of acquiring user's signing key from encrypted 
keychain data. As well as in [Wallet Creation][1], we have encapsulated all
the tricky actions in our javascript SDK, so all you need to do here is just
get your [wallet][2]:

{% tabs %} {% tab title="JavaScript" %}
```javascript
const wallet = api.wallets.get('foo@bar.com', 'p@ssw0rd')
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val keyServer = KeyServer(api.wallets)
try {
    val walletInfo = keyServer.getWalletInfo(email, password)
} catch (e1: EmailNotVerifiedException) {
    // Email is not verified
} catch (e2: InvalidCredentialsException) {
    // Invalid credentials
}
```
{% endtab %} {% endtabs %}

By doing this, you ask SDK to perform such actions:

* Check out actual KDF parameters;
* Calculate `wallet ID` and `wallet key`;
* Get `wallet` by its `id`;
* Decrypt account keys using `wallet key`;
* Return error if wallet is not verified;

As a result, you get a [Wallet][3] instance that will contain decrypted keys,
so you can start using them for signing requests and transactions.

{% tabs %} {% tab title="JavaScript" %}
```javascript
console.log(wallet.id) // => 4aadcd4eb44bb845d828c45dbd68d5d1196c3a182b08cd22f05c56fcf15b153c
console.log(wallet.email) // => foo@bar.com
// seed for signing keypair:
console.log(wallet.secretSeed) // => SAPSZ3ODNFYAESQEPGL72MAXFNL7RQTFBLYDD32DICJHYQNA36LGNGBN
// The account identifier, NOT the PK from signing keypair: 
console.log(wallet.accountId) // => GAIEBMXUPSGW2J5ELJFOY6PR5IWXXJNHIJSDKTDHK76HHRNYRL2QYU4O
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
// Hex-encoded wallet ID
System.out.println(walletInfo.walletIdHex) 
// => 4aadcd4eb44bb845d828c45dbd68d5d1196c3a182b08cd22f05c56fcf15b153c

// Email
System.out.println(walletInfo.email)
// => foo@bar.com

// Seed for signing keypair
System.out.println(walletInfo.secretSeed.joinToString(""))
// => SAPSZ3ODNFYAESQEPGL72MAXFNL7RQTFBLYDD32DICJHYQNA36LGNGBN

// Original account ID, NOT the account ID of signing keypair
System.out.println(walletInfo.accountId)
// => GAIEBMXUPSGW2J5ELJFOY6PR5IWXXJNHIJSDKTDHK76HHRNYRL2QYU4O
```
{% endtab %} {% endtabs %}

To start using wallet through SDK, you can do the following:

{% tabs %} {% tab title="JavaScript" %}
```javascript
sdk.useWallet(wallet)
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val account = Account.fromSecretSeed(walletInfo.secretSeed)
val signedApi = TokendApi(ROOT_URL, AccountRequestSigner(account))
```
{% endtab %} {% endtabs %}

After this, SDK will automatically sign every request and operations with your 
signing keys. Subsequently, new account is created. That means that you can provide your new user with all the TokenD functionalities.

## Other platforms

[<img src="https://kotlinlang.org/assets/images/favicon.ico" height="16"/> Sign in using Kotlin SDK][4]

[1]: /tech/guides/sign_in.md
[2]: /tech/key_entities/wallet.md
[3]: https://tokend.gitlab.io/new-js-sdk/Wallet.html
[4]: https://github.com/tokend/kotlin-sdk/wiki/Sign-in
