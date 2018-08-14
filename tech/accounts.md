# Accounts

Accounts are the central data structure in TokenD. Accounts are identified by a public key and saved in the ledger. Everything else in the ledger, such as assets, offers or KYC data, are owned by a particular account.

Accounts are created with the Create Account operation.

Account access is controlled by public/private key cryptography. For an account to perform a transaction–e.g., make a payment–the transaction must be signed by the private key that corresponds to that account’s public key. You can also set up more complicated multi-signature schemes for authorizing transactions on an account.

## Account Fields

Accounts have the following fields:

* **AccountID** &mdash; the public key that was first used to create the account. You can replace the key used for signing the account’s transactions with a different public key, but the original account ID will always be used to identify the account.
* **recoveryID** &mdash; the public key that is used to recover the accounts. Using the private key of this key it's possible to manage all the signers of the account, while signers are not able to change this key. It's possible to change `recoveryID` only by signing transaction with private key of current `recoveryID`.
* **Thresholds** Operations have varying levels of access. This field specifies thresholds for low-, medium-, and high-access levels, as well as the weight of the master key. For more info, see multi-sig.
* **Signers** Used for multi-sig. This field lists other public keys, their weights and policies, which can be used to authorize transactions for this account.
* **blockReasons** specifies blocking flags which have been set by the master of the system. Specific block reason disables specific set operations which account is able to perform. Example: `SUSPICIOUS_BEHAVIOR` disabled all operations for the account, while `WITHDRAWAL` disables withdrawal operation.
* **accountType** defines set of operations which are allowed to be performed by the account. Example: `MASTER` is able to perform all operations like `create account` and `review asset creation request`;  `NOT_VERIFIED` is not able to perform operations with assets which require KYC to be completed.
* **referrer** specifies account which brought user into the system.