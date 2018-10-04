# Sign in


Sign in is basically a process of acquiring client's signing key from encrypted keychain data.

* First client needs to obtain [KDF parameters](https://tokend.gitlab.io/docs#get-kdf-parameters) used to derive wallet ID and keychain data, which includes unique salt generated during wallet creation.
* After [derive wallet ID](#wallet-id-derivation) using email and password, client is able to [get wallet](#get-wallet) with encrypted keychain data.
* [Decrypted](#keychain-derivation) keychain data should contain signing keypair which should be used to get access to user resources.
