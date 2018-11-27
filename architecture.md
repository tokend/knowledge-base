# Architecture

TokenD can be divided into two parts: DLT-based logic (node) responsible for 
the key functionalities such as tokens management and distribution, rights 
management, etc.; auxiliary modules, which interconnect DLT with external 
systems, store user data, etc. A detailed overview of these modules and their 
connections is specified in Figure 2. 

![Architecture of TokenD system](https://gitlab.com/tokend/diagrams/raw/feature/test/png/ecosystem_a4.png)

### Node

**_Node_** is a key component of the platform. It processes transactions, manages 
history, and provides an easy to use API to access the blockchain data. It 
consists of two modules:

* **Core** — a replicated state machine that maintains a local copy of  
cryptographic ledger and processes transactions against it in consensus with 
a set of peers. It implements the federated consensus protocol and is 
responsible for tokens accounting and roles management.

**_Horizon_** is the client-facing REST API server. It acts as an interface 
between the core and applications that want to access the network. It allows 
submitting transactions to the network, checking the status of accounts, and 
viewing transaction history. 

### PSIM

**_PSIM_** (Payment Services Integration Module) is a set of modules that 
play the role of a bridge between TokenD and cryptocurrencies’ public 
blockchains, banks, payment gateways, exchanges. They reflect corresponding 
operations like deposit, withdrawal and exchange rate changes in another system.

### ESIM

**_ESIM_** (External System Integration Module) is a set of modules that 
interconnects TokenD with various external systems. They are responsible for 
a wide range of functionalities: from transfer notifications to 
automatization of user identity verification.

### KYC Storage

**_KYC Storage_** is a GDPR compliant module which stores data collected 
during the KYC (know your customer) procedure. To access the data, a user or 
admin needs to provide the digital signature, which is verified against 
the most rest state of the ledger. Such an approach provides a high level of 
security that can be further improved with full data encryption at rest and 
transit.

### Key-server

**_Key-server_** is a module which stores client-side-encrypted private 
keys of users and admins. This prevents a malicious actor from getting access 
to the accounts of the system even having a full access to the storage.

### Web, iOS, Android wallets

**_Web, iOS, Android wallets_** are client facing applications that 
provide a wide range of functionalities: from storage of encrypted private 
keys on the device, to token transfers, withdrawals, and trading. They 
interact with the core of the system directly through the Horizon module and 
signs all transactions and requests locally. Such an approach ensures that 
users’ private keys are safe even in case of MITM (man in the middle) attacks.
