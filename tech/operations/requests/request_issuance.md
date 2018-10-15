# Issuance Request

This operation creates new `issuance request`.

> *Note*: source account must be the owner of the asset to create issuance request.

## Review operation

To review issuance request, please use [Review issuance][2] operation.

## Source account requirements

| Property              | Value                                        |
|-----------------------|----------------------------------------------|
| Threshold             | `HIGH`                                         |
| Allowed account types | `MASTER`, `SYNDICATE`                        |
| Allowed signer types  | `ISSUANCE_MANAGER`, `SUPER_ISSUANCE_MANAGER` |

## Parameters

| Parameter       |  Type    |                              Description                               |
|-----------------|----------|------------------------------------------------------------------------|
| asset           | string   | Asset to be issued                                                     |
| amount          | string   | Amount to be issued                                                    |
| receiver        | string   | The valid `Balance ID` of the receiver                                 |
| externalDetails | object   | External details needed for PSIM to process withdraw operation         |
| reference       | string64 | Unique identifier of the issuance request                              |
| allTasks        | int32*   | Bit mask of all tasks which must be completed for the request approval |

## Tasks

The issuance requests comes up with the [Tasks][3] feature. It means that every 
request may contain set of pending tasks, that should be resolved by master.
The request may become approved only if all tasks are resolved. 

To make the request approved automatically, source needs to create with 
`allTasks` set to `0`. If source hasn't provided any tasks (the `allTasks` 
parameter is neither defined, nor being set to `0`), tasks will be taken
from the [Key Value storage][1] by key `issuance_tasks:{asset code}` 
(e.g. `issuance_tasks:BTC`);

### Possible tasks

Source is not allowed to set core flags.

| Name                                       | Value | Description                                                                                                                                                                                |
|--------------------------------------------|-------| ------------------------------------------------------------------------------------------------------------------------------------------------- |
| INSUFFICIENT_AVAILABLE_FOR_ISSUANCE_AMOUNT | 1     | Is being set automatically, if the amount for issuance is insufficient. Will be resolved automatically when reviewing. Source is NOT allowed to set this task | 
| ISSUANCE_MANUAL_REVIEW_REQUIRED            | 2     | Is being set automatically, if the amount for issuance is insufficient or asset has policy `MANUAL_REVIEW_REQUIRED`. Can be resolved by reviewer. Source is NOT allowed to set this task |
| DEPOSIT_VERIFY                             | 4     | Will verify if deposit limit of issuance destination is not exceeded |

## Examples

```javascript
const operation = base.CreateIssuanceRequestBuilder.createIssuanceRequest({
  asset: 'QTK',
  amount: '10.000000',
  receiver: 'BD2U4FYCQ6TEVXJZAFP2VB22NKFBLJKKVM625DEM4BXMQN6AOZFTHQAB',
  reference: 'qrhevwjkgbwhjejrktiwerlqwrewqrm',
  externalDetails: {},
  allTasks: 0 // will try to auto-approveg
})
```

## Possible errors

| Error                       | Code | Description                                                                              |
|-----------------------------|------|------------------------------------------------------------------------------------------|
| ASSET_NOT_FOUND             | -1   | Asset not found in core DB or asset code is invalid                                      |
| INVALID_AMOUNT              | -2   | Amount in request body is 0                                                              |
| REFERENCE_DUPLICATION       | -3   | Reference is empty or request with such reference already exists                         |
| NO_COUNTERPARTY             | -4   | Receiver balance not found or has different asset code                                   |
| NOT_AUTHORIZED              | -5   | Operation source account isn't the owner of asset                                        |
| EXCEEDS_MAX_ISSUANCE_AMOUNT | -6   | Asset max issuance amount is lower than the amount to issue                              |
| RECEIVER_FULL_LINE          | -7   | Issuance amount + receiver balance amount will overflow uint64                           |
| INVALID_EXTERNAL_DETAILS    | -8   | External details size is bigger than 1000 characters or external details is invalid JSON |
| FEE_EXCEEDS_AMOUNT          | -9   | Fee for the issuance request creation is bigger than the amount to issue                 |
| REQUIRES_KYC                | -10  | Asset requires receiver to have KYC                                                      |
| REQUIRES_VERIFICATION       | -11  | Asset requires receiver to be verified                                                   |
| ISSUANCE_TASKS_NOT_FOUND    | -12  | Issuance tasks have not been provided by the source and don't exist in `KeyValue` table  |
| SYSTEM_TASKS_NOT_ALLOWED    | -13  | Source is trying to set one of the core flags                                            |

[1]: https://tokend.gitlab.io/docs/#key-value-storage
[2]: /tech/operations/requests/review_issuance.md
[3]: review.md#tasks
