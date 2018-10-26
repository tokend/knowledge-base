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

## Action

Every review operation may be used to perform any the actions, that update request
state:

* Approve
* Reject
* Reject Permanently

Every action is taken from corresponding XDR enum, to read more, visit the
[XDR][14] documentation.

Here are some key moments about review actions:

* Rejected requests are the only one that can be updated;
* Permanently rejected requests can't be updated at all;
* Pending requests must be rejected first, before requestor can update it;
* Approving pending request will left it's state `pending` if there are any pending tasks left;
* It's not allowed to have 2 pending [KYC requests][4] from one requestor;
* It's not allowed to have 2 rejected (not permanently) [KYC requests][4] from one requestor;
* It's not allowed to have 2 pending [Asset requests][1] for one asset;
* It's not allowed to have 2 rejected [Asset requests][1] for one asset;

## Reason

To reject/reject permanently any request, you'll need to provide the `rejectReason`.
It is a simple string with a human-readable text with max-length 256 symbols.
When approving request, the `reason` must be empty.

## Tasks

Some of the reviewable requests has the `tasks` property, that is actually 
a bit mask, where every bit of defines the task that is linked an action 
performed by the system module, external service or a real person. Here is
some tasks we used for [KYC requests][4]:

* Verify that documents provided during verification process is valid;
* Receive the confirmation from external service that verifies if user is 
accredited investor;
* Wait for super admin review.

Every task may be unset by the [signer][12] of master account. To set or unset 
tasks you will need to perform a review operation with the required signer type. 
Request can not be approved until all of it's pending tasks are resolved.

To try to make the request approved automatically, the requestor needs to create 
request with `allTasks` set to `0`. If source hasn't provided any tasks (the `allTasks` 
parameter is neither defined, nor being set to `0`), tasks will be taken
from the [Key Value storage][key_value] by key `{request_type}_tasks:{asset code}` 
(e.g. `issuance_tasks:BTC`);

Here is the list of requests types that are currently come up with `Tasks` feature:

* [Issuance request][2]
* [KYC request][4]

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
[12]: /tech/key_entities/signer.md
[13]: https://tokend.gitlab.io/docs/#key-value-storage
[14]: /tech/xdr.md
[key_value]: https://tokend.gitlab.io/docs/#key-value-storage