# Sign up

To have full access to the platform a client should create their own account entity. This guide supposes that reader is already familiar with [Wallets][2] and got acquainted with our [API reference][3].

## Create a wallet

Wallet creation is a quite complex process, but
we have encapsulated all the stuff in our javascript SDK, so all you need to
do is: 

```javascript
const { wallet, recoverySeed } = await api.wallets.create('vasil@tokend.io', 'p@ssw0rd')
```

By doing this, you ask SDK to:

* Load the actual KDF parameters;
* Generate account [Ed25519][5] keys;
* Encrypt keys using the provided credentials;
* Generate [recovery][4] and 2FA public keys;
* Create new wallet entity and put it into the TokenD database.

> If you don't want to use SDK or want to get deeper into how things work,
 see [Wallet creation][1] in the TokenD API reference,

## Verify an email

Each wallet is bind to email address of the user, which is to be verified
before proceeding with the sign up flow (whether the verification is obligatory or not depends on the platform settings). Verification implies submitting the token received in email via API (for details, see [API docs][6]).

```javascript
const encodedActionThatCameToEmail = 'eyAic3RhdHVzIjoyMDAsImF...'
await api.wallets.verifyEmail(encodedActionThatCameToEmail)
```

## Create a user

After wallet is created and verified, the client should [create user][8] in the API 
database. This will also automatically trigger the creation of [account][7] with the
same identifier on the ledger. Keys generated during wallet creation are now able
to manage all the three entities.

```javascript
const accountId = 'GBYMMGDOS32QIMZ2HX4DYVXNFVDEE4G3IUSKNLM44MCTOFSCYRPF7KDE'
await api.users.create(accountId) // will create both user and account
```

## Other platforms

[<img src="https://kotlinlang.org/assets/images/favicon.ico" height="16"/> Sign up using Kotlin SDK][9]

[1]: https://tokend.gitlab.io/docs#create-wallet
[2]: /tech/key_entities/wallet.md
[3]: https://tokend.gitlab.io/docs#wallets
[4]: /tech/guides/password_change_recovery.md
[5]: https://ed25519.cr.yp.to/
[6]: https://tokend.gitlab.io/docs#email-verification
[7]: /tech/key_entities/accounts.md
[8]: https://tokend.gitlab.io/docs#create-user
[9]: https://github.com/tokend/kotlin-sdk/wiki/Sign-up
