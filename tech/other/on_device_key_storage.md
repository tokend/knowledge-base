# TokenD on-device key storage

## The problem
Despite TokenD uses a public-key cryptography, security of the system can be compromised while keys are being derived from a password. Users are comfortable having one password for many services so the risk of password comprometation is high. Attacker with a database of stolen email-password pairs can go over it making requests to the keyserver and get access to some accounts.

## Proposed solution
To get rid of passwords key storage should be moved to the client side. Master key of the account can be stored on a user’s mobile device using special app. Then to access user’s account client application must generate a keypair and ask user to authorize it using master key.

## Auth flow
![Auth flow sequence chart](https://docs.google.com/uc?export=download&id=1lOqTB3IQ19ULT0fiJ78qC0prYGW0baRQ "Auth flow")

## Auth URI
Auth request can be represented by the URI with **tdauth** scheme.
### General params
|Name|Description|Value|
|---|---|---|
|action|Describes required action|**auth** - used to authorize public key|
|api|API root URL|HTTP URL, urlencoded|

### Auth-specific params
|Name|Description|Value|
|---|---|---|
|app|Name of the app requesting access|String, urlencoded|
|pubkey|Base32-encoded public key of the signer, basically an AccountID|String|
|scope|Bitmask representing requested permissions|Unsigned integer|
|expires_at|Access expiration date|Unix timestamp (UTC) - for a finite sessionn<br>0 - permanent access until revoked by user|
### Examples
Authorize client app for a session:
`tdauth://action=auth&api=https%3A%2F%2Fapi.testnet.tokend.org&app=TokenD+web&pubkey=GAUL...H23T&scope=268435455&expires_at=1541930400`

Authorize Telegram bot for a permanent access with limited scope:
`tdauth://action=auth&api=https%3A%2F%2Fapi.testnet.tokend.org&app=TokenD+bot&pubkey=GBQG...ZWSN&scope=4&expires_at=0`

## Requirements
### Account signer
Extra fields are required in the account signer structure:

* expires_at - will be used to limit access time for specific signer

### Wallet
Encrypted seed is no more needed in the wallet structure. To derive wallet ID secret seed bytes should be used instead of password. With this approach genuine wallet ID can be derived only in the authenticator app and so it need to be passed to clients requesting access to the account.

Wallet request must be signed. This will help to obtain auth status in the client app.

### Authenticator-client communication
We need to set up communication between the client app and the authenticator. After the auth request client app need to be notified about the authentication result. For example it can poll an URL based on newly generated account ID.

Endpoint to poll:

`GET: /authresult/GDQN...EPAB`

While auth request is pending result will be 404.
After user made a decision the authenticator will set auth status and related wallet ID using the same endpoint:

`POST: /authresult/GDQN...EPAB`

<pre>
{  
   "success":"true",
   "wallet_id":"e1e8...4c40"
}
</pre>

As soon as the client app received auth status and wallet ID during polling it can perform further required actions. 