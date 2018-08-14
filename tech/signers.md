# Signers

Signer is the main concept used for authorization. To be able to perform transaction on read private data of particular account from any of the services signature of one of the signers of that account must be provided.

## Signer Fields

* **pubKey** &mdash; public key of the ed25519 key pair. Used as identifier of the signer.
* **weight** &mdash; specifies weight of the signer.
* **signerType**  &mdash; bit mask of signer types; Defines set of operations allowed to be authorized by the signer in name of the account.
* **identity** &mdash; identity of the signer. Two signatures created by two different signers with same identity are handled as one.
* **name** &mdash; name of the signer.