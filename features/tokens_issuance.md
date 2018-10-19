# Tokens Issuance

TokenD system provides easy methods to issue tokens. It is possible to distribute tokens or, in case of integration with external systems, process deposit operations manually or automatically. To be able to perform token issuance of certain amount, `available_for_issuance` must be equal or greater then that amount. There are two ways to make tokens available for issuance:

* specify amount available for issuance on token creation via `initialPreissuedAmount`;
* authorize amount to be issued via `CreatePreIssuanceRequestOp` operation. 

## `CreatePreIssuanceRequestOp`

To ensure high level of security TokenD allows token issuers to easily manage amount of tokens available for issuance in the system.
During token creation, it is possible to specify `preissuedAssetSigner`. It is recommended to generate the keypair for `preissuedAssetSigner` on the offline device. There are only two ways to change this key: by signing the corresponding transaction with current `preissuedAssetSigner` of the token, or by creating the fork of the network. `PreIssuanceRequest` created by `SYNDICATE` requires additional approval from the admin of the system.

### Fields of `CreatePreIssuanceRequestOp`

* **asset**  &mdash; code of token for which authorization should be performed;
* **amount**  &mdash; amount of tokens authorized for issuance;
* **reference**  &mdash; unique identifier of the pre-issuance request;
* **signature**  &mdash; signaure of all above listed fields performed by `preissuedAssetSigner`.

## `CreationIssuanceRequestOp`

This operation creates new issuance request. Depending on the default tasks specified, token issuance can either be performed immediately or require additional confirmations from token owner.

__Note__: To create issuance requests, the source account must be the owner of the asset.

* If source has not provided any tasks, they will be taken from the 'KeyValue' table by the key '`create_issuance_tasks:%asset code%'` (e.g. `create_issuance_tasks:BTC`);
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
| INSUFFICIENT_AVAILABLE_FOR_ISSUANCE_AMOUNT | 1     |Set in case the issuance amount in request exceeds the  `available_for_issuance`| amount
| ISSUANCE_MANUAL_REVIEW_REQUIRED            | 2     |Set in case the `ISSUANCE_MANUAL_REVIEW_REQUIRED` policy is active|

### Possible errors

| Error                       | Code | Description                                                                              |
|-----------------------------|------|------------------------------------------------------------------------------------------|
| ASSET_NOT_FOUND             |  -1  | Asset not found in core DB or asset code is invalid                                      |
| INVALID_AMOUNT              |  -2  | Amount in request body is 0                                                              |
| REFERENCE_DUPLICATION       |  -3  | Reference is empty or request with such reference already exists                         |
| NO_COUNTERPARTY             |  -4  | Receiver balance is not found or has different asset code                                   |
| NOT_AUTHORIZED              |  -5  | Operation source account is not the owner of asset                                        |
| EXCEEDS_MAX_ISSUANCE_AMOUNT |  -6  | Asset max issuance amount is lower than the amount to issue                              |
| RECEIVER_FULL_LINE          |  -7  | Issuance amount + receiver balance amount will overflow uint64                           |
| INVALID_EXTERNAL_DETAILS    |  -8  | External details are invalid or their size exceeds 1000 characters
JSON |
| FEE_EXCEEDS_AMOUNT          |  -9  | Fee for the issuance request creation is bigger than the amount to issue                 |
| REQUIRES_KYC                |  -10 | The receiver of the asset is required to have a KYC                                                      |
| REQUIRES_VERIFICATION       |  -11 | The receiver of the asset is required to pass verification                                                   |

### Successful result

Successful result has the following fields:

* __Request ID__: id of the request generated in core.

* __Receiver__: id of the balance to receive issued amount of asset.

* __Fulfilled__: if true - request was created and reviewed right away.
