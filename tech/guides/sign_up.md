# Sign up

To have full access to the platform client should create own account entity. We 
that when you reading this guide, you are familiar with [Wallets][2] and have
read our [API reference][3]

## Create wallet

First of all, you will need to create wallet. It's a quite complex process, but
we have encapsulated all the stuff in our javascript sdk, so all you need to
do is: 

```javascript
const { wallet, recoverySeed } = await api.wallets.create('vasil@tokend.io', 'p@ssw0rd')
```

By doing this, you're telling SDK to:
* Check out actual KDF parameters;
* Generate account keys;
* Encrypt them using provided credentials; 
* Generate [recovery][4] and 2FA public keys;
* Create new wallet entity and put it into database.

> If you don't want to use the SDK or want to get deeper into how the things work,
 see [Wallet creation][1] in the TokenD API reference

## Verify email

Each wallet is bound to email address which, depending on platform settings, 
may require verification before proceeding with sign up flow.
Verification consists of submitting token received in email to Key Server, 
for details see [KS docs](https://tokend.gitlab.io/docs#email-verification)

```javascript
const encodedActionThatCameToEmail = 'eyAic3RhdHVzIjoyMDAsImF...'
await api.wallets.verifyEmail(encodedActionThatCameToEmail)
```

## Create user

After wallet is created and verified client should 
[create user](https://tokend.gitlab.io/docs#create-user) in Key Server which 
will also automatically trigger creation of account resource with same 
identifier on the ledger. Keypair generated during wallet creation is now able 
to manage all three resources.

```javascript
const accountId = 'GBYMMGDOS32QIMZ2HX4DYVXNFVDEE4G3IUSKNLM44MCTOFSCYRPF7KDE'
await api.users.create(accountId) // will create both user and account
```

[1]: https://tokend.gitlab.io/docs#create-wallet
[2]: /tech/key_entities/wallet.md
[3]: https://tokend.gitlab.io/docs#wallets
[4]: /tech/guides/password_change_recovery.md
