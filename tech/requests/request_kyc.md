# Update kyc request

This operation creates new or update existing `update KYC request`.

## Review operation

To review update kyc request, please use [Review KYC][3] operation.

## Source account details

| Property              | Value                                                   |
|-----------------------|---------------------------------------------------------|
| Threshold             | `HIGH`                                                  |
| Allowed account types | `NOT_VERIFIED`, `GENERAL`, `SYNDICATE`, `MASTER`        |
| Allowed signer types  | `KYC_SUPER_ADMIN`, `KYC_ACC_MANAGER`, `ACCOUNT_MANAGER` |

## Parameters

| Parameter            | Type   |       Description                                             |
|----------------------|--------|-----------------------------------------------------------------------------|
| requestID            | uint64 | Zero to create, not zero to update existing                                 |
| accountToUpdateKYC   | string | The ID of account, which account type will be updated                       |
| accountTypeToSet     | string | New account type that will be set to account after request will be approved |
| kycLevelToSet        | number | New KYC level of account
| kycData              | object | Data that proves that user is allowed to update kyc
| kycData.blob_id      | string | The identifier of [blob][1], that stores user-provided KYC data
| allTasks             | uint64 | Tasks for kyc request, that can be set only by `KYC_SUPER_ADMIN`.

## Tasks

The KYC requests comes up with the [Tasks][4] feature. It means that every 
request may contain set of pending tasks, that should be resolved by master.
The request may become approved only if all tasks are resolved. 

To make the request approved automatically, source needs to create with 
`allTasks` set to `0`. If source hasn't provided any tasks (the `allTasks` 
parameter is neither defined, nor being set to `0`), tasks will be taken
from the [Key Value storage][2] by key `issuance_tasks:{asset code}` 
(e.g. `issuance_tasks:BTC`);

## Example

```javascript
const operation = CreateUpdateKYCRequestBuilder.createUpdateKYCRequest({
  accountToUpdateKYC: 'GCGR7MRRKUGOKRN7PY4R66LSKESGOAY4CEV6QQFFW4DE3TKQZIXW4RXG',
  accountTypeToSet: xdr.AccountType.general().value,
  kycLevelToSet: 0,
  kycData: {
    blob_id: '4NSI4HPWFHLZTUB3QPURTUPZMHID4OI6CDUZVJAREUIEAJMRLE6A'
  },
  allTasks: 0
})
```

## Possible errors

| Error                              | Code | Description                                                                           |
|------------------------------------|------|---------------------------------------------------------------------------------------|
| ACC_TO_UPDATE_DOES_NOT_EXIST       |  -1  | There is no account with such id.                                                     |
| REQUEST_ALREADY_EXISTS             |  -2  | KYC request for such account already exists.                                          |
| SAME_ACC_TYPE_TO_SET               |  -3  | Account type to set must be different from existing account type of account.          |
| REQUEST_DOES_NOT_EXIST             |  -4  | There is no request with such id.                                                     |
| PENDING_REQUEST_UPDATE_NOT_ALLOWED |  -5  | Only rejected kyc requests allowed to update.                                         |
| NOT_ALLOWED_TO_UPDATE_REQUEST      |  -6  | `MASTER` cannot update kyc reviewable requests.                                       |
| INVALID_UPDATE_KYC_REQUEST_DATA    |  -7  | Update kyc request allows update only `KYC data`.                                     |
| INVALID_KYC_DATA                   |  -8  | `KYC data` must be represended as stringified json struct.                            |
| KYC_RULE_NOT_FOUND                 |  -9  | There is no key value with rules for existing account type and `Account type to set`. |

[1]: https://tokend.gitlab.io/docs/#blobs
[2]: https://tokend.gitlab.io/docs/#key-value-storage
[3]: /tech/requests.md
[4]: review.md#tasks
