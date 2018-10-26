# Request \(Issuance\)

This operation creates a new `issuance request`.

> _Note_: in order to create the issuance request, the source account must be the owner of the asset.

## Review operation

To review issuance request, please use [Review issuance](https://github.com/tokend/knowledge-base/tree/d0245307e292182ff962f32676d21d595fc6230a/tech/requestsuance.md) operation.

## Source account requirements

| Property | Value |
| :--- | :--- |
| Threshold | `HIGH` |
| Allowed account types | `MASTER`, `SYNDICATE` |
| Allowed signer types | `ISSUANCE_MANAGER`, `SUPER_ISSUANCE_MANAGER` |

## Parameters

| Parameter | Type | Description |
| :--- | :--- | :--- |
| asset | string | Asset to be issued |
| amount | string | Amount to be issued |
| receiver | string | The valid `Balance ID` of the receiver |
| externalDetails | object | External details needed for PSIM to process withdraw operation |
| reference | string64 | Unique identifier of the issuance request |
| allTasks | int32\* | Bit mask of all tasks which must be completed for the request approval |

## Tasks

The issuance requests comes with the [Tasks](review.md#tasks) features. It means that every request may contain a set of pending tasks that should be resolved by master. The request may become approved only if all tasks are resolved.

To make the request approved automatically, source needs to create it with `allTasks` set to `0`. If source hasn't provided any tasks \(the `allTasks` parameter is neither defined nor set to `0`\), tasks will be taken from the [Key Value storage](https://tokend.gitlab.io/docs/#key-value-storage) by key `issuance_tasks:{asset code}` \(e.g. `issuance_tasks:BTC`\);

### Possible tasks

Source is not allowed to set core flags.

| Name | Value | Description |
| :--- | :--- | :--- |
| INSUFFICIENT\_AVAILABLE\_FOR\_ISSUANCE\_AMOUNT | 1 | Is set automatically, if the amount for issuance is insufficient. Will be resolved automatically when reviewing. Source is NOT allowed to set this task |
| ISSUANCE\_MANUAL\_REVIEW\_REQUIRED | 2 | Is set automatically, if the amount for issuance is insufficient or asset has a `MANUAL_REVIEW_REQUIRED` policy. Can be resolved by the reviewer. Source is NOT allowed to set this task |
| DEPOSIT\_VERIFY | 4 | Will verify if deposit limit of issuance destination is not exceeded |

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

| Error | Code | Description |
| :--- | :--- | :--- |
| ASSET\_NOT\_FOUND | -1 | Asset not found in core DB or asset code is invalid |
| INVALID\_AMOUNT | -2 | Amount in request body is 0 |
| REFERENCE\_DUPLICATION | -3 | Reference is empty or request with such reference already exists |
| NO\_COUNTERPARTY | -4 | Receiver balance not found or has different asset code |
| NOT\_AUTHORIZED | -5 | Operation source account is not the owner of asset |
| EXCEEDS\_MAX\_ISSUANCE\_AMOUNT | -6 | Asset max issuance amount is lower than the amount to issue |
| RECEIVER\_FULL\_LINE | -7 | Issuance amount + receiver balance amount will overflow uint64 |
| INVALID\_EXTERNAL\_DETAILS | -8 | External details size is bigger than 1000 characters or external details is invalid JSON |
| FEE\_EXCEEDS\_AMOUNT | -9 | Fee for the issuance request creation is bigger than the amount to issue |
| REQUIRES\_KYC | -10 | Asset requires receiver to have KYC |
| REQUIRES\_VERIFICATION | -11 | Asset requires receiver to be verified |
| ISSUANCE\_TASKS\_NOT\_FOUND | -12 | Issuance tasks have not been provided by the source and do not exist in `KeyValue` table |
| SYSTEM\_TASKS\_NOT\_ALLOWED | -13 | Source is trying to set one of the core flags |

