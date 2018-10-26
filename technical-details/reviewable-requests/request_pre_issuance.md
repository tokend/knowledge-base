# Request \(Pre issuance\)

The operation creates new `pre issuance request`.

## Review operation

To review pre issuance request, please use [Review pre issuance](https://github.com/tokend/knowledge-base/tree/26a801978c6d594d5250703fa23f110b4c026280/tech/requests_issuance.md) operation.

## Source account requirements

| Property | Value |
| :--- | :--- |
| Threshold | `HIGH` |
| Allowed account types | `MASTER`, `SYNDICATE` |
| Allowed signer types | `ISSUANCE_MANAGER` |

## Parameters

| Parameter | Type | Description |
| :---: | :---: | :---: |
| request | PreIssuanceRequest | Body of the request to be created |

## Possible errors

| Error | Code | Description |
| :--- | :---: | :--- |
| ASSET\_NOT\_FOUND | -1 | Asset not found in core database or asset code is invalid. |
| REFERENCE\_DUPLICATION | -2 | Request with such reference already exists. |
| NOT\_AUTHORIZED\_UPLOAD | -3 | Operation source account is not the owner of asset. |
| INVALID\_SIGNATURE | -4 | Signature of operation was not created by asset pre-issued signer. |
| EXCEEDED\_MAX\_AMOUNT | -5 | Amount to be pre-issued + available for issuance amount + issued amount more than max issuance amount. |
| INVALID\_AMOUNT | -6 | Amount to be pre-issued can not be zero. |
| INVALID\_REFERENCE | -7 | Reference can not be empty. |

