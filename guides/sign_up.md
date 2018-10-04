# Sign up

To have full access to the platform client should create own account entity.

To achieve that following steps must be completed:

* Wallet should be [created](https://tokend.gitlab.io/docs#create-wallet) using Key Server API.
* Depending on platform settings additional verification may be required, see [email verification](#email-verification) for details.
* After wallet is created and verified client should [create user](https://tokend.gitlab.io/docs#create-user) in Key Server which will also trigger creation of account resource with same identifier on the ledger.
  Keypair generated during wallet creation is now able to manage all three resources.


## Email verification

Each wallet is binded to email address which, depending on platform settings, may require verification before proceeding with sign up flow.
Verification consists of submitting token received in email to Key Server, for details see [KS docs](https://tokend.gitlab.io/docs#email-verification)
