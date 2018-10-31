# Access management
TokenD is a tokenization platform that allows you to issue, transfer and exchange your assets with high level of privacy, security and auditability while following regulations of your jurisdiction. It provides flexible, user friendly and secure functionalities for managing the full lifecycle of tokens, which allows customers to implement MVP solutions for various use cases with low customization and fastest time to market.

TokenD is based on a blockchain technology that describes the way of keeping the actual copies of ledger between distributed network nodes that includes description of principles of reaching an agreement between these nodes about updating the ledger information. 

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

Imagine Alice wants to delegate her KYC management responsibilities to her assistant Bob.
Bob in order to be able to access and manage KYC data of users, needs `KYC_ACC_MANAGER` signer
Alice can set `KYC_ACC_MANAGER` signer to Bob, thus allowing him manage kyc

