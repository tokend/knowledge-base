# Architecture

![Architecture of TokenD system](https://lh4.googleusercontent.com/_ELztJW0hlR4-b6Og0mXEcFnX2O_5trs1P75pxxGvI3OuzGZZ0AxhZE0XEfjYpucadE20M8OHkbgQtadu7hN-1bkaS-7lv2D4I1LM1U5cHCMalzmVXdjh_NcuLPiPfCKepxIpaQv)

## Core Cluster

Min number of instances: 4;
Modules: Core, Horizon;

### Core

Core is a replicated state machine that maintains a local copy of a cryptographic ledger and processes transactions against it, in consensus with a set of peers. It implements the federated consensus protocol. It is written in C++11. Requires PostgreSQL database to store the most recent state of the ledger and AWS S3 to store full history of transactions.

### Horizon

Horizon is the client-facing API server. It acts as an interface between the core and applications that want to access the network. It allows submitting transactions to the network, checking the status of accounts, and viewing transaction history. Requires PostgreSQL database to store history of transactions. Horizon is an entry point into the instance.

## Payment Services Integration Module (PSIM) Cluster

Min number of instances: 2;
Modules: deposit, funnel, withdraw.

### Deposit

Deposit is a module which monitors the state of external systems (e.g. BTC and ETH blockchains, banks, payment gateways) and reflects deposit operations (using multisignature) into the TokenD system. It does not require any additional storage and guarantees that all deposit operations are processed no more than once. Requires access to external systems (does not require local instance). No publically available entry points.

### Funnel

Funnel is a module which is only required for systems working with BTC, ETH. It monitors the state of HD wallets (provided to users to perform deposits) and transfers all available assets to hot or cold wallet (depending on the state of hot wallet); Requires access to external systems. In case of BTC and ETH, it does not require local instances. No publically available entry points.

### Withdraw

Withdraw is a module which monitors the state of TokenD system and reflects withdrawal operations into external system. Confirmation of withdrawal requests and actual transactions into external systems are performed using multisig. Guarantees that withdrawal operation review is atomic. Signing the review of request and actual withdrawal in case of BTC and ETH is performed in two steps. First, approve/reject the request, approve/reject the payload of transactions and submit not signed transactions into the Core of TokenD. Second, sign a transaction, add signatures into the Core of TokenD, confirm the request, and submit a signed transaction into external system. In case of Payment Gateways, withdrawal is guaranteed to be atomic by means of idempotent ID of the request (functionality provided by Payment Gateway). Requires access to external systems (does not require local instance). Does not require any additional storage. No publically available entry points.

## RateSync Cluster

Min number of instances: 1;
Recommended number of instances: 2;
RateSync is a module which pulls exchange rates data from external systems (Poloniex, CoinmarketCap, etc.). Finds correspondence between exchange rate providers and using multisignature sends exchange rates to the core of the system. Does not require any additional storage; No publically available entry points.

## API Cluster

Min number of instances: 1;
Recommended number of instances: 2;
API is a module which stores user-related data: users' KYC and project specific data, AccountID-Email linking. Requires S3 for the KYC docs storage and Postgres RDS to store KYC data; Has a publically available entry point.

## KeyChain Cluster

Min number of instances: 1;
Recommended number of instances: 2;
KeyChain is a module which stores private keys used to encrypt KYC data of users. Gives keys only to admins or owners of keys based on data retrieved from Horizon. Has a publically available entry point. Requires Postgres RDS to store private keys (completely separate from API); API and KeyChain must be connected to different Horizon-Core pairs.

## KeyServer Cluster 

 Min number of instances: 1;
Recommended number of instances: 2;
KeyServer is a module which stores client-side encrypted private keys of users. Gives encrypted payload only to users who provided the proof of knowing the encryption secret. Allows to enable 2FA (Google Authenticator).

## Notificator Cluster

Min number of instances: 1;
Recommended number of instances: 2;
Notificator is a middleware module between all modules of the system and external providers (mailgun, twilio, etc.). Ensures delivery of messages. Has no publically available entry points. Requires Postgres RDS to store the already sent messages.
