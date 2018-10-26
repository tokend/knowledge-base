# Review update kyc request

This operation creates new or deletes existing `update_kyc_request`.

## Source account requirements

| Property              | Value                                |
|-----------------------|--------------------------------------|
| Threshold             | `HIGH`                                 |
| Allowed account types | `MASTER`                             |
| Allowed signer types  | `KYC_SUPER_ADMIN`, `KYC_ACC_MANAGER` |

## Request details - Update KYC Details

| Parameter       | Type                      | Description                                                           |
|-----------------|---------------------------|-----------------------------------------------------------------------|
| requestID       | uint64                    | ID of request to be reviewed                                          |
| requestHash     | string64                  | Hash depends on reviewable request type and details (body)            |
| action          | xdr.ReviewRequestOpAction | One of the actions: `APPROVE`, `REJECT`, `PERMANENT_REJECT`           |
| reason          | longstring                | The reason why request was rejected or permanent rejected             |
| tasksToAdd      | uint32                    | Bit masks of tasks that will be added to request pending tasks.      |
| tasksToRemove   | uint32                    | Bit masks of tasks that will be removed from request pending tasks.  |
| externalDetails | string                    | String details that will be added to request details.                |

## Examples

Review `update_kyc request`:

```javascript
const operation = base.ReviewRequestBuilder.reviewUpdateKYCRequest({
  requestID: '55',
  requestHash: '',
  action: xdr.ReviewRequestOpAction.approve().value,
  reason: '',
  tasksToAdd: 8,   
  tasksToRemove: 4,
  externalDetails: {}
})
```

## Possible errors

| Error                                | Code | Description                                                        |
|--------------------------------------|------|--------------------------------------------------------------------|
| NON_ZERO_TASKS_TO_REMOVE_NOT_ALLOWED | -60  | There is no sense to set zero `tasksToRemove` in `APPROVE` action. |

