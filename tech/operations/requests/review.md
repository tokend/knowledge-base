# Review request (default)

This operation is used to review `reviewable request` of any type. However, almost
every request operation has the corresponding review operation that is much 
easier to use because of encapsulating composing request and review details.

Such reviewable requests has the specific review operation: 

[Asset create/update requests][1] - Use [Review asset creation][6] or [Review asset update][7] to review  
[Issuance requests][2] - Use [Review issuance][8]  
[Pre issuance requests][3] - Use [Review pre issuance][9]  
[KYC requests][4] - Use [Review KYC][10]  
[Withdrawal requests][5] - Use [Review withdrawal][11]  

## Source account details

Depends on `reviewable request type`

## Parameters

| Parameter      |    Type                   |       Description                                                                                          |
|----------------|---------------------------|------------------------------------------------------------------------------------------------------------|
| requestID      | uint64                    | ID of request to be reviewed                                                                               |
| requestHash    | string64                  | Hash depends on reviewable request type and details (body)                                                 |
| action         | xdr.ReviewRequestOpAction | One of the actions: `APPROVE`, `REJECT`, `PERMANENT_REJECT`                                                |
| reason         | longstring                | The reason why request was rejected or permanent rejected                                                  |
| requestDetails | object                    | Depends on `reviewable request type`                                                                       |
| reviewDetails  | object                    | Details, that affect reviewable request                                                                    |

### Review Details fields

ReviewDetails has following fields:

* __Tasks to add__: bit masks of tasks, that will be added to request pending tasks.
* __Tasks to remove__: bit masks of tasks, that will be removed from request pending tasks.
* __External details__: string details, that will be added to request details.

## Possible errors

| Error                        | Code | Description                                                                                         |
|------------------------------|------|-----------------------------------------------------------------------------------------------------|
| INVALID_REASON               |  -1  | Reason must be empty if approving and not empty if rejecting.                                       |
| INVALID_ACTION               |  -2  | Action must be one of allowed actions: `APPROVE`, `REJECT`, `PERMANENT_REJECT`.                     |
| HASH_MISMATCHED    	       |  -3  | RequestHash from operation does not equal hash of existing request.				    |
| NOT_FOUND                    |  -4  | There is no request with such id for reviewer                                                       |
| TYPE_MISMATCHED              |  -5  | Request type (from request Details) from operation does not equal request type of existing request. |
| REJECT_NOT_ALLOWED           |  -6  | Use `PERMANENT_REJECT`.                                                                             |
| INVALID_EXTERNAL_DETAILS     |  -7  | External details has not represented as stringified json struct or details too long.                |
| REQUESTOR_IS_BLOCKED         |  -8  | Request cannot be approved while requestor has `SUSPICIOUS_BEHAVIOR` block reason.                  |
| PERMANENT_REJECT_NOT_ALLOWED |  -9  | Use `REJECT`.    									            |


[1]: request_asset.md
[2]: request_issuance.md
[3]: request_pre_issuance.md
[4]: request_kyc.md
[5]: request_withdrawal.md
[6]: review_asset_creation.md
[7]: review_asset_update.md
[8]: review_issuance.md
[9]: review_pre_issuance.md
[10]: review_kyc.md
[11]: review_withdrawal.md
