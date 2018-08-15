# Tokens Issuance

TokenD system provides easy methods to issue tokens, which can be used to distributed the token or, in case of integration with external system token, manually or automatically process deposit operations. To be able to perform issuance of certain amount of tokens `available_for_issuance` must be equal or greater then that amount. There are two ways to make tokens available for issuance:

* specify amount available for issuance on token creation via `initialPreissuedAmount`;
* authorize amount to be issued via `CreatePreIssuanceRequestOp` operation. 

## `CreatePreIssuanceRequestOp`

To ensure high level of security TokenD allows tokens issuers to easily manage amount of tokens available for issuance in the system.
On token creation it is possible to specify `preissuedAssetSigner`. It is recommended to generate the keypair for `preissuedAssetSigner` on the offline device. There are only two ways to change this key: by signing corresponding with current `preissuedAssetSigner` of the token, or by creating fork of the network. `PreIssuanceRequest` create by `SYNDICATE` requires additional approval from the admin of the system.

### Fields of `CreatePreIssuanceRequestOp`

* **asset**  &mdash; code of token for which authorization should be performed;
* **amount**  &mdash; amount of tokens to be authorized to be issued;
* **reference**  &mdash; unique identifier of the pre issuance request;
* **signature**  &mdash; signaure of all fields above performed by `preissuedAssetSigner`.

## `CreationIssuanceRequestOp`

This operation creates new issuance request. Depending of default tasks specified for that token issuance can be performed right away or it might require additional confirmations from token owner.

__Note__: source account must be the owner of the asset to create issuance request.

* If source has not provided any tasks, tasks will be taken from the 'KeyValue' table by key '`create_issuance_tasks:%asset code%'` (e.g. `create_issuance_tasks:BTC`);
* If tasks bitmask equals `0`, issuance request will be auto-reviewed, else - just created;
* If the amount for issuance is insufficient or issuance requires manual review, etc., the corresponding flag will be set to `allTasks`, see [Possible allTasks flags](##possible-alltasks-flags);

### Fields of `CreationIssuanceRequestOp`

* **request**  &mdash; body of the request to be created
    * **asset**  &mdash; token code for which issuance will be performed
    * **amount**  &mdash; amount to be issued
    * **receiver**  &mdash; balance identifier of the receiver of the issuance
    * **externalDetails**  &mdash;  details of the issuance (External system id, etc.)
* **reference**  &mdash;  unique identifier of the issuance request
* **allTasks**  &mdash; bitmask of all tasks which must be completed for the request approval

### Reserved `allTasks` flags

| Name                                       | Value |Details|
|--------------------------------------------|-------|-------|
| INSUFFICIENT_AVAILABLE_FOR_ISSUANCE_AMOUNT | 1     |Set in case of issuance request which amount exceeds `available_for_issuance`|
| ISSUANCE_MANUAL_REVIEW_REQUIRED            | 2     |Set in case of policy `ISSUANCE_MANUAL_REVIEW_REQUIRED` been active|

### Possible errors

| Error                       | Code | Description                                                                              |
|-----------------------------|------|------------------------------------------------------------------------------------------|
| ASSET_NOT_FOUND             |  -1  | Asset not found in core DB or asset code is invalid                                      |
| INVALID_AMOUNT              |  -2  | Amount in request body is 0                                                              |
| REFERENCE_DUPLICATION       |  -3  | Reference is empty or request with such reference already exists                         |
| NO_COUNTERPARTY             |  -4  | Receiver balance not found or has different asset code                                   |
| NOT_AUTHORIZED              |  -5  | Operation source account isn't the owner of asset                                        |
| EXCEEDS_MAX_ISSUANCE_AMOUNT |  -6  | Asset max issuance amount is lower than the amount to issue                              |
| RECEIVER_FULL_LINE          |  -7  | Issuance amount + receiver balance amount will overflow uint64                           |
| INVALID_EXTERNAL_DETAILS    |  -8  | External details size is bigger than 1000 characters or external details is invalid JSON |
| FEE_EXCEEDS_AMOUNT          |  -9  | Fee for the issuance request creation is bigger than the amount to issue                 |
| REQUIRES_KYC                |  -10 | Asset requires receiver to have KYC                                                      |
| REQUIRES_VERIFICATION       |  -11 | Asset requires receiver to be verified                                                   |

### Successful result

Successful result has the following fields:

* __Request ID__: id of the request generated in core.

* __Receiver__: id of the balance to recieve issued amount of asset.

* __Fulfilled__: if true - request was created and reviewed right away.

## General Deposit Flow

High level description of deposit flow. For more details see:

* BTC Family Flow
* ETH Flow
* ERC20 Flow

### Preparations

1. Admin of the system have created token which represents asset from external system;
1. Admin of the system have assigned specific `external system ID`. Which will be used to link asset in the TokenD system and external system asset; For more details on how to perform this step see `MANAGE_ASSET`
1. Admin of the system generates set of address (identifiers) of the external system, which will allow automated module to link transfer performed in external system to user account in TokenD system;
1. Admin submits set of generated address to the core of the system with `external system ID` selected on second step using operation `MANAGE_EXTERNAL_SYSTEM_ACCOUNT_ID_POOL_ENTRY`
1. Admin specifies expiration period of the external system account id using operation [MANAGE_KEY_VALUE](tech/manage_key_value.md)

### Flow

1. User binds external system account id using `BIND_EXTERNAL_SYSTEM_ACCOUNT_ID`;
1. User initiates transfer in external system to specified account id;
1. Automated module `deposit` monitors the external system for new incoming transfer. If new one is confirmed, `deposit` crafts TokenD `create issuance request` transaction with corresponding details, unique reference of the deposit and `null` tasks, signs it and sends to `core`;
1. `core` creates an issuance request with default tasks taken from `KeyValue` table. Assume that the value of tasks is `DEPOSIT_VERIFY`;
1. User is now able to see deposit operation in TokenD system;
1. `deposit_verify` retrieves request from `core`, finds the corresponding transfer in the external system, ensures that they are matching and submits the transaction, that removes `DEPOSIT_VERIFY` flag, to TokenD;
1. While transaction being processed, if tasks equal to `0` - `core` tries to fulfill issuance request. If the amount for issuance is insufficient, `core` will set `INSUFFICIENT_AVAILABLE_FOR_ISSUANCE_AMOUNT` flag in tasks bitmask. If asset to be issued has policy `ISSUANCE_MANUAL_REVIEW_REQUIRED`, `core` will set `ISSUANCE_MANUAL_REVIEW_REQUIRED` flag in tasks bitmask. The request will be fulfilled iff tasks equal to `0` after review.
1. `funnel` module monitors state of external system; on new incoming transfer if total amount of funds does not exceeds threshold performs transfer to Hot Wallet otherwise to Cold Wallet;