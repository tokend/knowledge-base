# Access management
TokenD is based on a permissioned blockchain that limit the parties (it can be wether another person or external service) who can transact on the blockchain and that set who can serve the network by writing new blocks into the chain.


### Accounts

Accounts are the central data structure in TokenD. Accounts are identified by a public key and saved in the ledger. Another data in the ledger, such as assets, offers, or KYC data are owned by a particular account.

Account access is controlled by a public/private key cryptography. For an account to perform a transaction (e.g. make a payment) the transaction must be signed with the private key that corresponds to the account’s public key. You can also set up more complicated multi-signature schemes for authorizing transactions on the account.

Detailed description of system's security is available [here](technical-details/security).

### Signers

Signer is the main concept used for authorization. To be able to perform transaction on read private data of particular account from any of the services signature of one of the signers of that account must be provided. For more info, see [here](technical-details/key-entities/signer).

### Thresholds

Operations have different levels of access. This field specifies thresholds for low, medium, and high access levels, as well as the weight of the master key. For more info, see [here](technical-details/security#thresholds).

### Master account 
Every account has a set of properties that make certain impact on account rights and the behaviour. Master account though is allowed to perform any operation in the system, also can add or remove signers

### Example

Imagine Charlie wants to delegate his KYC management responsibilities to his assistant Dan.
Dan in order to be able to access and manage KYC data of users, needs `KYC_ACC_MANAGER` signer
Charlie can set `KYC_ACC_MANAGER` signer to Dan, thus allowing him manage kyc
