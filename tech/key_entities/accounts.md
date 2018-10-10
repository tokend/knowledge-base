# Account

Accounts are the central data structure in TokenD. Accounts are identified by 
a public key and saved in the ledger. Everything else in the ledger, such as 
assets, offers or KYC data, are owned by a particular account.

Accounts are created with the [Create Account][1] operation.

Account access is controlled by public/private key cryptography. For an 
account to perform a transaction – e.g., make a payment – the transaction must 
be signed by the private key that corresponds to that account’s public key. 
You can also set up more complicated multi-signature schemes for authorizing 
transactions on an account.

## Account ID

*Account ID* is the public key that was first used to create the account. 
You can replace the key used for signing the account’s transactions with a 
different public key, but the original account ID will always be used to 
identify the account.

The base32 encoded account ID looks like: `GBNB7XNMFRHFAHBIIU6BECH6E62K5X2GY3EQZTYMO4S7AU6NUPGEZ5SQ`

## Account secret seed

By *Account Seed* we actually mean the [ed25519](https://ed25519.cr.yp.to/) 
private key of one of the account [signers][2].

The base32 encoded secret seed looks like: `SBFIZIILH442RMEZDZXEGFLV7IONXQBCUM62B44H6FHYP6U3DKWIMRHA`

When creating the account, the ed25519 public key from his first signer, but
while account ID is permanent, the signer of account could be replaced using 
[Set options][3] operation. Also it's allowed to have multiple
signers (every has it's own secret seed) on one account.

## Other account properties

Every account has a set of properties that make some impact on 
account rights and behaviour.

### Account type

*Account type* tells us what the account itself is about. 


The main account types in TokenD are:

| Name          | Description |
|:-------------:|-------------|
|`AccountTypeNotVerified`| Any user who have registered in the TokenD system and have not passed KYC procedure yet. |
|`AccountTypeGeneral`| `General` accounts represent trusted users, who have passed KYC procedure. Some actions require verification to be passed, so only `general` accounts will be allowed to perform such actions  |
|`AccountTypeSyndicate`| Is allowed to perform [token creation requests][5]. Usually it's used for corporate users, who have already passed KYC procedure and became trusted authorities |
|`AccountTypeMaster`| Is allowed to perform any operation in the system. The ability of doing an action is restricted only with the policies of the transaction signer [type][4]. |

### Signers

Used for multi-sig. This field lists other public keys, their weights and 
policies, which can be used to authorize transactions for this account.

### Thresholds

Operations have varying levels of access. This field specifies thresholds for 
low-, medium-, and high-access levels, as well as the weight of the master 
key. For more info, see multi-sig.

### Recovery ID

The public key that is used to recover the accounts. Using the private key of 
this key it's possible to manage all the signers of the account, while signers 
are not able to change this key. It's possible to change `recoveryID` only by 
signing transaction with private key of current `recoveryID`.

### Block Reasons

Specifies blocking flags which have been set by the master of the system. 
Specific block reason disables specific set operations which account is able 
to perform. Example: `SUSPICIOUS_BEHAVIOR` disabled all operations for the 
account, while `WITHDRAWAL` disables withdrawal operation.


### Account referrer

Specifies account which brought user into the system.


<!--1. Create account-->
<!--5. Asset creation request-->
<!--6. Asset policies-->


[1]: /coming_soon.md
[2]: /tech/signers.md
[3]: /tech/operations/set_options.md
[4]: /tech/signers.md#signer-types
[5]: /coming_soon.md
[6]: /coming_soon.md