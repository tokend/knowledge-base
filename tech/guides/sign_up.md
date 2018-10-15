# Sign up

To have full access to the platform client should create own account entity. We 
suppose that when you reading this guide, you are familiar with [Wallets][2] 
and took a look our [API reference][3]

## Create wallet

First of all, you will need to create wallet. It's a quite complex process, but
we have encapsulated all the stuff in our javascript SDK, so all you need to
do is: 

```javascript
const { wallet, recoverySeed } = await api.wallets.create('vasil@tokend.io', 'p@ssw0rd')
```

By doing this, you're asking SDK to:

* Load the actual KDF parameters;
* Generate account [Ed25519][5] keys;
* Encrypt them using provided credentials;
* Generate [recovery][4] and 2FA public keys;
* Create new wallet entity and put it into the TokenD database.

> If you don't want to use the SDK or want to get deeper into how the things work,
 see [Wallet creation][1] in the TokenD API reference

## Verify email

Each wallet is bound to email address which is user to verify the wallet
before proceeding with sign up flow (the verification may be required 
depending on platform settings). Verification consists of submitting the token 
received in email to the API, for details visit [API docs][6]

```javascript
const encodedActionThatCameToEmail = 'eyAic3RhdHVzIjoyMDAsImF...'
await api.wallets.verifyEmail(encodedActionThatCameToEmail)
```

## Create user

After wallet is created and verified client should [create user][8] in the API 
database. This will also automatically trigger the creation of [account][7] with
same identifier on the ledger. Keys generated during wallet creation is now able
to manage all the three entities.

```javascript
const accountId = 'GBYMMGDOS32QIMZ2HX4DYVXNFVDEE4G3IUSKNLM44MCTOFSCYRPF7KDE'
await api.users.create(accountId) // will create both user and account
```

[1]: https://tokend.gitlab.io/docs#create-wallet
[2]: /tech/key_entities/wallet.md
[3]: https://tokend.gitlab.io/docs#wallets
[4]: /tech/guides/password_change_recovery.md
[5]: https://ed25519.cr.yp.to/
[6]: https://tokend.gitlab.io/docs#email-verification
[7]: /tech/key_entities/accounts.md
[8]: https://tokend.gitlab.io/docs#create-user
