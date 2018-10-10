# Review update kyc request

This operation creates new or deletes existing `update_kyc_request`.

## Source account details

| Property              | Value                                |
|-----------------------|--------------------------------------|
| Threshold             | high                                 |
| Allowed account types | `MASTER`                             |
| Allowed signer types  | `KYC_SUPER_ADMIN`, `KYC_ACC_MANAGER` |

## Request details - Update KYC Details

| Parameter       | Type   |       Description                                                    |
|:---------------:|:------:|:--------------------------------------------------------------------:|
|   tasksToAdd    | uint32 | Bit masks of tasks, that will be added to request pending tasks.     |
|  tasksToRemove  | uint32 | Bit masks of tasks, that will be removed from request pending tasks. |
| externalDetails | string | String details, that will be added to request details.               |

## Possible errors

> TODO: add possible errors

## Successful result

Successful creation result has the following field:

* __Request ID__: id of the request generated in core.
