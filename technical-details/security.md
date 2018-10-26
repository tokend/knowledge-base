# Security

TokenD uses the [Ed25519 algorithm](https://ed25519.cr.yp.to/) for authorizing any request or read the private data. The use of such cryptographic proof allows building the system where transfer of secrets in an open form is not necessary, which significantly improves the security. Thus, an attacker who has access to one of the servers will not be able to change the state of the database or compromise the secrets of users.

## REST API

Request signature implementation is based on IETF HTTP Signatures draft RFC except of implicit headers parameter is not supported, clients must explicitly specify headers used for signing.

The signature algorithm supported is ed25519-sha256 which uses public signer key as keyId.

Both Signature and Authorization HTTP authentication schemas are supported.

The minimum recommended data to sign is the \(request-header\) and date.

For the following request:

```http
GET /users?type=2 HTTP/1.1
Host: api.tokend.io
Date: Sun, 05 Jan 2014 21:31:40 GMT
```

Signing string would be:

```text
date: Sun, 05 Jan 2018 21:31:40 GMT
(request-target): get /users?type=2
```

For `SCDMOOXVNMO6SA22AYUMZDIGLDJMBUTVEGB73FFNTLFJILBJWIU4NQ3D` private key gives the following signature string:

```text
keyId="GBLTOG6EJS5OWDNQNSCEAVDNMPBY6F73XZHHKR27YE5AKE23ZZEXOLBK",algorithm="ed25519-sha256",signature="0cvTqLDn+5i8pInkeSR833HrNSMI4xB9m1eN7rofiDVnoutKQJvpwB9hl2GhsMPcMbVXo4beUR96Stf/qU+iAg==",headers="date (request-target)"
```

## Transactions

Transactions always need authorization from at least one public key in order to be considered valid. Generally, transactions only need authorization from the public key of the source account.

Transaction signatures are created by cryptographically signing the transaction object contents with a secret key. TokenD uses the ed25519 signature scheme. A transaction with an attached signature is considered to have authorization from that public key.

The transaction may need more than one signature in two cases: 1\) if a transaction has operations that affect more than one account, it will need authorization from every account; 2\) if the account associated with the transaction has multiple public keys.

### Thresholds

Operations fall under a specific threshold category: low, medium, or high. The threshold for a given level can be set to any number from 0-255. This threshold is the amount of signature weight required to authorize an operation at that level.

Let’s say Diana sets the medium threshold on one of her accounts to 4. If that account submits a transaction that includes a payment operation \(medium security\), the transaction’s threshold is 4–the signature weights need to be greater than or equal to 4. If Diana’s master key \(the key corresponding to the public key that identifies the account she owns\) has a weight less than 4, she cannot authorize a transaction without other signers.

Once the signature threshold is met, if there are any leftover signatures, then the transaction is regarded as having too many signatures, which results in a failed transaction even if the remaining signatures are valid, i.e. for a transaction signed with N signatures, if the threshold is reached using K signatures, then the transaction will fail if N &gt; K.

Each account can set its own threshold values. By default all thresholds levels are set to 0, and the master key is set to weight 1. The Set Options operation allows you to change the weight of the master key and add other signing keys with different weights.

