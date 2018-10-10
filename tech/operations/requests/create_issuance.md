# Issuance Request

This operation creates new `issuance request`.

> *Note*: source account must be the owner of the asset to create issuance request.
* If source has not provided any tasks, tasks will be taken from the 'KeyValue' table by key '`issuance_tasks:%asset code%'` (e.g. `issuance_tasks:BTC`);
* If tasks bitmask equals `0`, issuance request will be auto-reviewed, else - just created;
* If the amount for issuance is insufficient or issuance requires manual review, etc., the corresponding flag will be set to `allTasks`, see [Possible allTasks flags](##possible-alltasks-flags);

## Source account details

| Property              | Value                                        |
|-----------------------|----------------------------------------------|
| Threshold             | high                                         |
| Allowed account types | `MASTER`, `SYNDICATE`                        |
| Allowed signer types  | `ISSUANCE_MANAGER`, `SUPER_ISSUANCE_MANAGER` |

## Parameters

| Parameter |       Type      |                              Description                              |
|:---------:|:---------------:|:---------------------------------------------------------------------:|
|  request  | IssuanceRequest |                   Body of the request to be created                   |
| reference |     string64    |               Unique identifier of the issuance request               |
|  allTasks |     uint32*     | Bitmask of all tasks which must be completed for the request approval |

## Possible `allTasks` flags

### Core flags

Source is not allowed to set core flags.

| Name                                       | Value |
|--------------------------------------------|-------|
| INSUFFICIENT_AVAILABLE_FOR_ISSUANCE_AMOUNT | 1     |
| ISSUANCE_MANUAL_REVIEW_REQUIRED            | 2     |

### Other flags

This flags can be set by source

| Name                                       | Value |
|--------------------------------------------|-------|
| DEPOSIT_VERIFY                             | 4     |

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

## Successful result

Successful result has the following fields:

* __Request ID__: id of the request generated in core.

* __Receiver__: id of the balance to recieve issued amount of asset.

* __Fulfilled__: if true - request was created and reviewed right away.
