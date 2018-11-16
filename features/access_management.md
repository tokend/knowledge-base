# Access management
TokenD is based on a permissioned blockchain that limit the parties (it can be wether another person or external service) who can transact on the blockchain and that set who can serve the network by writing new blocks into the chain.

Therefore Tokend has pack of functionalities that help us to distribute the rights. In order to use them, you need to be familiar with 2 key entities: **accounts** and **signers**

### Accounts

Accounts are the central data structure in TokenD. First of all, an account is a human-readable name that is stored on the blockchain. An account is required to transfer or otherwise push a transaction to the blockchain. Accounts are identified by a public key and saved in the ledger. Another data in the ledger, such as assets, offers, or KYC data are owned by a particular account.

TokenD differs the type of account to master's and user's [accounts](/tech/key_entities/accounts#account-type). The owner of master account is allowed to perform a lot of operations that will have impact on the whole system.

Detailed description of system's security is available [here](/tech/security).

### Signers

The account owner can assign signers to his employees or some services. Signer is the main concept used for authorization. Therefore, the signer will be able to perform transaction on read private data of this account from any of the services signature of one of the signers of that account must be provided. For more info, see [here](/tech/key_entities/signer).

Signer access is controlled by a public/private key cryptography. For an account to perform a transaction (e.g. make a payment) the transaction must be signed with the private key that corresponds to the account’s public key. You can also set up more complicated multi-signature schemes for authorizing transactions on the account.

Signer is an entity that has several properties like: 
   * #### Public key— public key of the ed25519 key pair. Used as identifier of the signer. The secret seed from such keypair is owned only by the person/service who represents signer and is not available to the account owner.
   * #### Signer type  — the list of [types](/tech/key_entities/signer), each of them defines the set of operations signer can perform
   * #### Identity — identity of the signer. Two signatures created by two different signers with same identity are considered as one.
   * #### Weight — the weight the access level of signer. It means that signer's weight, in order to perform the operation, must be higher than the operation threshold level
