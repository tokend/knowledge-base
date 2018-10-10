# Update kyc request

This operation creates new or update existing `update KYC request`.

## Source account details

| Property              | Value                                                                                                         |
|-----------------------|---------------------------------------------------------------------------------------------------------------|
| Threshold             | high                                                                                                          |
| Allowed account types | `NOT_VERIFIED`, `VERIFIED`, `GENERAL`, `INSTITUTIONAL_INVESTOR`, `ACCREDITED_INVESTOR`, `SYNDICATE`, `MASTER` |
| Allowed signer types  | `KYC_SUPER_ADMIN`, `KYC_ACC_MANAGER`, `ACCOUNT_MANAGER`                                                       |

## Parameters

| Parameter            |       Type           |       Description                           |
|:--------------------:|:--------------------:|:-------------------------------------------:|
| requestID            | 	 uint64       | Zero to create, not zero to update existing |
| updateKYCRequestData | UpdateKYCRequestData | Details of kyc request                      |

### Update KYC request data
UpdateKYCRequestData has the following fields:

* __Account to update KYC__: account id of user, which account type will be updated.

* __Account type to set__: new acccount type for such user.

* __KYC level to set__: new kyc level of account.

* __KYC data__: data that proves that user is allowed to udpate kyc.

* __All tasks__: (optional) tasks for kyc request, that can be setted only by `KYC_SUPER_ADMIN` (if not setted use default from key value).

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


## Successful result

Successful result has the following field:

* __Request ID__: id of the request generated in core.

* __Fulfilled__: true if request was auto approved.

