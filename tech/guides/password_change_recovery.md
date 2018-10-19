# Password change / Recovery

Password change and recovery flows are based on updating user's signing keypair.
The general flow consists of the following steps:

1. Generate a new keypair
2. Get the [list of account signers](https://tokend.gitlab.io/docs/#get-account-signers) 
3. Create a transaction that drops all existing account signers and adds the new one which is defined by public key of the new keypair
4. Encrypt the secret seed of the new keypair with the new password
5. Perform a [wallet update](https://tokend.gitlab.io/docs/#update-wallet) with the new `keychain_data`, then the envelope of a created transaction is included as a `transaction` relation.

## Dropping account signers
In case of password change or recovery we suppose that the current keypair has been compromised and the list of account signers may be modified by the intruder, so we have to drop all existing account signers.

Normally, there are two signers for account: the active and the recovery one. Dont worry about dropping the recovery signer – it is protected and will remain untouched.

To drop a signer, you have to set its `weight` to `0` using `SetOptionsOp`:
 
* If signer's public key is equal to wallet's original account id, use SetOptionsOp with `masterWeight=0`
* In other cases, use SetOptionsOp with `signer` having `weight=0` created from current signer's public key 

**Important notice: drop the current signer last otherwise the transaction will fail.**

## Recovery
When there is no access to the current password, you have to obtain user's wallet in a special way:

1. Obtain [KDF parameters](https://tokend.gitlab.io/docs/#get-kdf-params) with `is_recovery` flag set to `true`
2. Use recovery seed entered by the user as a password to [derive wallet ID](https://tokend.gitlab.io/docs/#wallet-id-derivation)
3. Get the recovery wallet by the following wallet ID

From the recovery wallet you can obtain original account ID to load signers for. The same wallet ID should be used during the wallet update.

Sign the transaction and wallet update request with a keypair initialized from the recovery seed.

> __Important notice__: it is possible to perform a recovery before the account has been created. In this case, account signers request will return a 404 error, so all you need is, in the transaction, to add a new signer.
