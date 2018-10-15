# Asset management requests

This operation allows such actions with the `asset`:

- Create asset creation request;
- Create asset update request;
- Cancel asset creation/update request;
- Change preissued asset signer;
- Update max issuance amount;

## Manage Sale Details

| Property              | Value                 |
| ---                   | ---                   |
| Threshold             | `HIGH`                |
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

## Examples

### Request asset creation

Create new `asset creation request`:

```javascript
const operation = base.ManageAssetBuilder.assetCreationRequest({
  requestID: '0', // saying it's a new request
  code: 'QTK',
  preissuedAssetSigner: 'GCDRDBHZNXS5ODJKSEXWRABKKHAGHJGOXWO75CMNEEUUQYFSDYARG5YP',
  maxIssuanceAmount: '1000.000000',
  policies: 4,
  initialPreissuedAmount: '1000.000000',
  details: {
    name: 'Q-Token',
    logo: {
      key: 'dmybfh4infrebjhcost7fvj5qeqmpjtujupkyzpvxhyrdbtujpd7r5n4'
    },
    terms: {
      key: 'dmybfh4infrebjhcost7fvj5qeqmpjtujupkyzpvxhyrdbtujpd7r5n4'
    }
  }
})
```

Update pending/rejected `asset creation request` (NOT the asset itself): 

```javascript
const operation = base.ManageAssetBuilder.assetCreationRequest({
  requestID: '213', // saying we will update existing request
  code: 'QTK',
  preissuedAssetSigner: 'GCDRDBHZNXS5ODJKSEXWRABKKHAGHJGOXWO75CMNEEUUQYFSDYARG5YP',
  maxIssuanceAmount: '1000.000000',
  policies: 4,
  initialPreissuedAmount: '1000.000000',
  details: {
    name: 'Q-Token',
    logo: {
      key: 'dmybfh4infrebjhcost7fvj5qeqmpjtujupkyzpvxhyrdbtujpd7r5n4'
    },
    terms: {
      key: 'dmybfh4infrebjhcost7fvj5qeqmpjtujupkyzpvxhyrdbtujpd7r5n4'
    }
  }
})
```

### Request asset update

Create new `asset update request`:

```javascript
const operation = base.ManageAssetBuilder.assetUpdateRequest({
  requestID: '0',
  policies: '0',
  details: {
    name: 'Q-Token',
    logo: {
      key: 'dmybfh4infrebjhcost7fvj5qeqmpjtujupkyzpvxhyrdbtujpd7r5n4'
    },
    terms: {
      key: 'dmybfh4infrebjhcost7fvj5qeqmpjtujupkyzpvxhyrdbtujpd7r5n4'
    }
  }
})
```

Update pending/rejected `asset update request`:

```javascript
const operation = base.ManageAssetBuilder.assetUpdateRequest({
  requestID: '321',
  policies: '0',
  details: {
    name: 'Q-Token',
    logo: {
      key: 'dmybfh4infrebjhcost7fvj5qeqmpjtujupkyzpvxhyrdbtujpd7r5n4'
    },
    terms: {
      key: 'dmybfh4infrebjhcost7fvj5qeqmpjtujupkyzpvxhyrdbtujpd7r5n4'
    }
  }
})
```

## Possible errors

| Error                                  | Code | Description                                            |
| ---                                    | ---  | ---                                                    |
| REQUEST_NOT_FOUND                      |  -1  | You're trying to update request which doesn't exist.   |
| ASSET_ALREADY_EXISTS                   |  -3  | The asset you're trying to create already exists in the system |
| INVALID_MAX_ISSUANCE_AMOUNT            |  -4  | Max issuance amount should be greated than `0`         |
| INVALID_CODE                           |  -5  | The asset code contains more than 16 UTF-8 symbols     |
| INVALID_POLICIES                       |  -7  | Cannot validate the policies.                          |
| ASSET_NOT_FOUND                        |  -8  | You're trying to update an asset that doesn't exist    |
| REQUEST_ALREADY_EXISTS                 |  -9  | The request to create such asset already exists and has `pending`/`rejected` state. To create the new, fulfill the latest one (approve or reject permanently) |
| STATS_ASSET_ALREADY_EXISTS             |  -10 | Asset with `STATS_QUOTE_ASSET` policy must be only one in the system |
| INITIAL_PREISSUED_EXCEEDS_MAX_ISSUANCE |  -11 | Initial pre issued amount exceeds max issuance amount. |
| INVALID_DETAILS                        |  -12 | Details must be a valid JSON string. If you see this error, it's probably a bug in js-base, which serializes the object |

## Successful result

Successful creation result has the following field:

- __Request ID__: id of the request generated in core.
- __Fullfilled__: boolean value representing whether the request is fullfilled.
