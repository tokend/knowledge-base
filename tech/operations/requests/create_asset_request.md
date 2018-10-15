# Manage Asset

This operation allows such actions with the `asset`:

- Create asset creation request;
- Create asset update request;
- Cancel asset request;
- Change preissued signer;
- Update max issuance;

## Manage Sale Details

| Property              | Value                 |
| ---                   | ---                   |
| Threshold             | high                  |
| Allowed account types | `MASTER`, `SYNDICATE` |
| Allowed signer types  | `ASSET_MANAGER`       |

## Parameters

| Action                        | Parameter              | Type       | Description                                |
| ---                           | ---                    | ---        | ---                                        |
| Any                           | requestID              | uint64     | Zero to create new request, else to update |
| Create asset creation request | code                   | AssetCode  | The code of asset to create                |
| Create asset creation request | preissuedAssetSigner   | AccountID  | The created asset's preissued signer       |
| Create asset creation request | maxIssuanceAmount      | uint64     | The created asset's max issuance amount    |
| Create asset creation request | initialPreissuedAmount | uint64     | The created asset's initial amount         |
| Create asset creation request | policies               | uint64     | Bit mask of existing asset policies        |
| Create asset creation request | details                | longstring | The created asset's details                |
| Create asset update request   | code                   | AssetCode  | A new asset code to replace the old one    |
| Create asset update request   | details                | longstring | New details to replace the old ones        |
| Create asset update request   | policies               | uint64     | New policies to replace the old ones       |
| Cancel asset request          | -                      | -          | -                                          |
| Change preissued signer       | code                   | AssetCode  | The code of preisssuer's asset             |
| Change preissued signer       | accountID              | AccountID  | The account id of a new preissuer          |
| Update max issuance           | assetCode              | AssetCode  | The code of asset to update issuance       |
| Update max issuance           | maxIssuanceAmount      | uint64     | A new max issuance amount                  |

## Possible errors

| Error                                  | Code | Description                                            |
| ---                                    | ---  | ---                                                    |
| REQUEST_NOT_FOUND                      |  -1  | There is no request with such id.                      |
| ASSET_ALREADY_EXISTS                   |  -3  | Cannot find asset with such a code.                    |
| INVALID_MAX_ISSUANCE_AMOUNT            |  -4  | Cannot be 0.                                           |
| INVALID_CODE                           |  -5  | Cannot validate the asset code.                        |
| INVALID_POLICIES                       |  -7  | Cannot validate the policies.                          |
| ASSET_NOT_FOUND                        |  -8  | Cannot find asset with the code                        |
| REQUEST_ALREADY_EXISTS                 |  -9  | The reference is already exists.                       |
| STATS_ASSET_ALREADY_EXISTS             |  -10 | Statistics quote asset already exists.                 |
| INITIAL_PREISSUED_EXCEEDS_MAX_ISSUANCE |  -11 | Initial pre issued amount exceeds max issuance amount. |
| INVALID_DETAILS                        |  -12 | Details must be a valid json.                          |

## Successful result

Successful creation result has the following field:

- __Request ID__: id of the request generated in core.
- __Fullfilled__: boolean value representing whether the request is fullfilled.
