# Pre Issuance Request

The operation creates new `pre issuance request`.

## Review operation

To review pre issuance request, please use [Review pre issuance][1] operation.

## Source account requirements

| Property              | Value                 |
|-----------------------|-----------------------|
| Threshold             | `HIGH`                  |
| Allowed account types | `MASTER`, `SYNDICATE` |
| Allowed signer types  | `ISSUANCE_MANAGER`    |

## Parameters

| Parameter |       Type         | Description                       |
|:---------:|:------------------:|:---------------------------------:|
|  request  | PreIssuanceRequest | Body of the request to be created |

## Possible errors

| Error                 | Code | Description                                                                                            |
|-----------------------|:----:|--------------------------------------------------------------------------------------------------------|
| ASSET_NOT_FOUND       | -1   | Asset not found in core database or asset code is invalid.                                             |
| REFERENCE_DUPLICATION | -2   | Request with such reference already exists.                                                            |
| NOT_AUTHORIZED_UPLOAD | -3   | Operation source account isn't the owner of asset.                                                     |
| INVALID_SIGNATURE     | -4   | Signature of operation was not created by asset pre issued signer.                                     |
| EXCEEDED_MAX_AMOUNT   | -5   | Amount to be pre issued + available for issuance amount + issued amount more than max issuance amount. |
| INVALID_AMOUNT        | -6   | Amount to be pre issued can not be zero.                                                               |
| INVALID_REFERENCE     | -7   | Reference can not be empty.                                                                            |

[1]: /tech/operations/requests/review_pre_issuance.md
