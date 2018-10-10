# Review request (default)

This operation allows review `reviewable request`. 

## Source account details

Depends on `reviewable request type`

## Parameters

| Parameter      |    Type               |       Description                                                                                          |
|:--------------:|:---------------------:|:----------------------------------------------------------------------------------------------------------:|
|  requestID     | uint64                | ID of request to be reviewed                                                                               |
|  requestHash   |   Hash                | Hash depends on reviewable request type and details (body)                                                 |
| requestDetails | union switch          | Depends on `reviewable request type` |
|   action       | ReviewRequestOpAction | One of the actions: `APPROVE`, `REJECT`, `PERMANENT_REJECT`                                                |
| reason         | longstring            | The reason why request was rejected or permanent rejected                                                  |
| reviewDetails  | ReviewDetails         | Details, that affect reviewable request                                                                    |

### Review Details fields

ReviewDetails has following fields:

* __Tasks to add__: bit masks of tasks, that will be added to request pending tasks.

* __Tasks to remove__: bit masks of tasks, that will be removed from request pending tasks.

* __External details__: string details, that will be added to request details.

## Possible errors

| Error                        | Code | Description                                                                                         |
|------------------------------|:----:|-----------------------------------------------------------------------------------------------------|
| INVALID_REASON               |  -1  | Reason must be empty if approving and not empty if rejecting.                                       |
| INVALID_ACTION               |  -2  | Action must be one of allowed actions: `APPROVE`, `REJECT`, `PERMANENT_REJECT`.                     |
| HASH_MISMATCHED    	       |  -3  | RequestHash from operation does not equal hash of existing request.				    |
| NOT_FOUND                    |  -4  | There is no request with such id for reviewer                                                       |
| TYPE_MISMATCHED              |  -5  | Request type (from request Details) from operation does not equal request type of existing request. |
| REJECT_NOT_ALLOWED           |  -6  | Use `PERMANENT_REJECT`.                                                                             |
| INVALID_EXTERNAL_DETAILS     |  -7  | External details has not represented as stringified json struct or details too long.                |
| REQUESTOR_IS_BLOCKED         |  -8  | Request cannot be approved while requestor has `SUSPICIOUS_BEHAVIOR` block reason.                  |
| PERMANENT_REJECT_NOT_ALLOWED |  -9  | Use `REJECT`.    									            |

## Successful result

Successful creation result has the following field:

* __Extended result__: 
	* __Fulfilled__: true if request does not have any pending tasks and now request has `APPROVED` state
	* __Type ext__: Depends on `reviewable request type`

