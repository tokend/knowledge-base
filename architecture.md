# Architecture

![Architecture of TokenD system](https://lh4.googleusercontent.com/_ELztJW0hlR4-b6Og0mXEcFnX2O_5trs1P75pxxGvI3OuzGZZ0AxhZE0XEfjYpucadE20M8OHkbgQtadu7hN-1bkaS-7lv2D4I1LM1U5cHCMalzmVXdjh_NcuLPiPfCKepxIpaQv)

## Core Cluster

Min number of instances: 4;
Modules: Core, Horizon;

### Core

Core is a replicated state machine that maintains a local copy of a cryptographic ledger and processes transactions against it, in consensus with a set of peers. It implements the federated consensus protocol. It is written in C++11. Requires PostgreSQL database to store most recent state of the ledger; and AWS S3 to store full history of transactions.

### Horizon

Horizon is the client facing API server. It acts as the interface between core and applications that want to access the network. It allows to submit transactions to the network, check the status of accounts, view transaction history. Requires PostgreSQL database to store history of transactions. Horizon is only entrypoint into instance.

## Payment Services Integration Module (PSIM) Cluster

Min number of instances: 2;
Modules: deposit, funnel, withdraw.

### Deposit

Deposit is module which monitors state of external system (BTC, ETH blockchains, Banks, Payment Gateway) and reflects deposit operations (using multisignature) into TokenD system. Guaranties that all deposit operations are processed and processed only once. Does not require any additional storage. Requires access to external system (does not require local instance). No publically available entry points. 

### Funnel

Funnel is module which is only required for the systems working in BTC, ETH. Monitors state of HD wallets (provided to users to perform deposits) and transfers all available assets to hot or cold wallet (depending on state of the hot wallet); Requires access to external system. In case of BTC and ETH does not require local instance. No publically available entry points.

### Withdraw

Withdraw is module which monitors state of TokenD system and reflects withdrawal operations into external system. Confirmation of withdrawal request and actual transaction into external system are performed using multisig. Guaranties that review of withdrawal operation is atomic. Signature of review of the request and actual withdrawal in case of (BTC and ETH) is performed in two steps. First: agree on approval/rejection of the request and on payload of transaction and submit not signed transaction into Core of TokenD. Second: sign transaction, add signatures into Core of TokenD and confirm request, submit signed transaction into external System. In case of Payment Gateways withdrawal is guaranteed to be atomic by idempotent ID of the request (functionality provided by Payment Gateway). Requires access to external system (does not require local instance). Does not require any additional storage. No publically available entry points.

## RateSync Cluster

Min number of instances: 1;
Recommended number of instances: 2;
RateSync is module which pulls exchange rates data from External Systems (Poloniex, CoinmarketCap, etc.). Finds consens between providers on current exchange rate and using multisignature sends exchange rates to core of the system. Does not require any additional store; No publically available entry points.

## API Cluster

Min number of instances: 1;
Recommended number of instances: 2;
API is module which stores users related data: KYC data of the users and project specific data, AccountID-Email linking. Requires S3 to KYC docs storage and Postgres RDS to store KYC data; Has publically available entry point.

## KeyChain Cluster

Min number of instances: 1;
Recommended number of instances: 2;
KeyChain is module which stores private keys used to encrypt KYC data of the users. Gives keys only to admins or owner of keys based on data retrieved from Horizon. Has publically available entry point. Requires Postgres RDS to store private keys (Completely separate from API); API and KeyChain must be connected to different Horizon-Core pairs.

## KeyServer Cluster 

 Min number of instances: 1;
Recommended number of instances: 2;
KeyServer is module which stores client side encrypted private keys of the users. Gives encrypted payload only to users who provided proof of knowing encryption secret, allows to enable 2FA (Google Authenticator).

## Notificator Cluster

Min number of instances: 1;
Recommended number of instances: 2;
Notificator is middleware module between all modules of the system and external providers (mailgun, twilio, etc.). Ensures delivery of the messages. Has no publically available entry point.Requires Postgres RDS to store already sent messages.
