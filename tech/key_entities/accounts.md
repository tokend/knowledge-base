# Account

Accounts are the central data structure in TokenD. Accounts are identified by 
a public key and saved in the ledger. Another data in the ledger, such as 
assets, offers, or KYC data are owned by a particular account.

Accounts are created using the `Create Account` operation.

Account access is controlled by a public/private key cryptography. For an 
account to perform a transaction (e.g. make a payment) the transaction must 
be signed with the private key that corresponds to the account’s public key. 
You can also set up more complicated multi-signature schemes for authorizing 
transactions on the account.

## Account ID

*Account ID* is the public key that was first used to create the account. 
You can replace the key used for signing the account’s transactions with a 
different public key, but the original account ID will always be used to 
identify the account.

The base32 encoded account ID looks as follows: `GBNB7XNMFRHFAHBIIU6BECH6E62K5X2GY3EQZTYMO4S7AU6NUPGEZ5SQ`

## Account secret seed

By the *Account Seed* we actually mean the [ed25519](https://ed25519.cr.yp.to/) 
private key of one of the account [signers][2].

The base32 encoded secret seed looks as follows: `SBFIZIILH442RMEZDZXEGFLV7IONXQBCUM62B44H6FHYP6U3DKWIMRHA`

When creating the account, the ed25519 public key is generated according to its first signer, but
while the account ID is permanent, the signer of account could be replaced using the
[Set options][3] operation. Also, it is allowed to have multiple
signers (each of which has its own secret seed) on one account.

## Other account properties

Every account has a set of properties that make certain impact on 
account rights and the behaviour.

### Account type

*Account type* tells us what the account itself is about. 


The main account types in TokenD are:

| Name          | Description |
|:-------------:|-------------|
|`AccountTypeNotVerified`| Any user who registered in the TokenD system but did not pass the KYC procedure yet. |
|`AccountTypeGeneral`| `General` accounts represent trusted users, who passed the KYC procedure. Certain actions on the platform require verification, correspondingly only `general` accounts will be allowed to perform such actions  |
|`AccountTypeSyndicate`| is allowed to perform [token creation requests][5]. Usually, it is used for corporate users, who have already passed the KYC procedure and became trusted authorities |
|`AccountTypeMaster`| is allowed to perform any operation in the system. The ability of doing an action is restricted only with the policies of the transaction signer [type][4]. |

### Signers

Used for multi-sig. This field lists other public keys, their weights, and 
policies, which can be used to authorize transactions for this account.

### Thresholds

Operations have different levels of access. This field specifies thresholds for 
low, medium, and high access levels, as well as the weight of the master 
key. For more info, see multi-sig.

### Recovery ID

The public key that is used to recover the accounts. Using the private key of 
this key it is possible to manage all the signers of the account, while signers 
are not able to change this key. It is possible to change `recoveryID` just by 
signing the transaction with a private key of the current `recoveryID`.

### Block Reasons

Specifies blocking flags which have been set by the master of the system. 
Specific block reason disables specific set operations which account is able 
to perform. Example: `SUSPICIOUS_BEHAVIOR` disabled all operations for the 
account, while `WITHDRAWAL` disables the withdrawal operation.


### Account referrer

Specifies an account which brought the user into the system.


[2]: /tech/key_entities/signer.md
[3]: /tech/operations/set_options.md
[4]: /tech/key_entities/signer.md#signer-types
[5]: /tech/requests/request_asset.md
