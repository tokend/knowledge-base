# Request \(Asset\)

This operation allows to provide the following actions with [asset](../key-entities/asset.md):

* Create asset creation request;
* Create asset update request;
* Cancel asset creation/update request;
* Change asset signer;
* Update max issuance amount;

## Review operations

To review asset creation request, use [Review asset creation](review_asset_creation.md) operation.  
To review asset update request, use [Review asset update](review_asset_update.md) operation.

## Source account requirements

| Property | Value |
| :--- | :--- |
| Threshold | `HIGH` |
| Allowed account types | `MASTER`, `SYNDICATE` |
| Allowed signer types | `ASSET_MANAGER` |

## Request asset creation

### Parameters

| Parameter | Type | Description |
| :--- | :--- | :--- |
| requestID | uint64 | Zero to create new request, request ID - to update |
| code | string | The code of the new asset, maximum 16 UTF-8 symbols |
| preissuedAssetSigner | string | Asset preissued signer |
| maxIssuanceAmount | uint64 | The maximum amount possible to issue |
| initialPreissuedAmount | uint64 | The initial amount to be issued |
| policies | uint64 | Bit mask of asset policies |
| details | longstring | Asset details object \(SDK will serialize it to JSON\) |

> IMPORTANT: asset code is immutable once the request is created. It means that if you want to update an existing `asset creation request` \(NOT the asset itself since asset does not exist at this moment\) it won't be possible to update the asset code.

### Examples

Create new `asset creation request`:

```javascript
const operation = base.ManageAssetBuilder.assetCreationRequest({
  requestID: '0', // it's a new request
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

Update pending/rejected `asset creation request` \(NOT the asset itself\):

```javascript
const operation = base.ManageAssetBuilder.assetCreationRequest({
  requestID: '213', // we will update existing request
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

## Request asset update

### Parameters

| Parameter | Type | Description |
| :--- | :--- | :--- |
| requestID | uint64 | Zero to create new request, request ID - to update |
| code | AssetCode | A new asset code to replace the old one |
| details | longstring | New details to replace the old ones |
| policies | uint64 | New policies to replace the old ones |

### Examples

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
  code: 'QTK',
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

## Cancel asset creation/update request

### Parameters

| Parameter | Type | Description |
| :--- | :--- | :--- |
| requestID | uint64 | ID of request to cancel |

## Change preissued asset signer

### Parameters

| Parameter | Type | Description |
| :--- | :--- | :--- |
| code | AssetCode | The code of preisssuer's asset |
| accountID | AccountID | The account id of a new preissuer |

### Examples

```javascript
const operation = base.ManageAssetBuilder.changeAssetPreIssuer({
  accountID: 'GDUV7QHM4SMHZXXFHN5QIOYWTPCGYEOR47NXE22DUJTGTLUHQZZUH7MJ', // new signer
  code: 'QTK'
})
```

## Update max issuance amount

| Parameter | Type | Description |
| :--- | :--- | :--- |
| assetCode | AssetCode | The code of asset to update issuance |
| maxIssuanceAmount | uint64 | A new max issuance amount |

## Possible errors

| Error | Code | Description |
| :--- | :--- | :--- |
| REQUEST\_NOT\_FOUND | -1 | You're trying to update the request which does not exist. |
| ASSET\_ALREADY\_EXISTS | -3 | The asset you are trying to create already exists in the system |
| INVALID\_MAX\_ISSUANCE\_AMOUNT | -4 | Max issuance amount should be greater than `0` |
| INVALID\_CODE | -5 | The asset code contains more than 16 UTF-8 symbols |
| INVALID\_POLICIES | -7 | Cannot validate the policies. |
| ASSET\_NOT\_FOUND | -8 | You are trying to update an asset that does not exist |
| REQUEST\_ALREADY\_EXISTS | -9 | The request on the creation of such asset already exists and has a  `pending`/`rejected` state. To create the new, fulfill the latest one \(approve or reject permanently\) |
| STATS\_ASSET\_ALREADY\_EXISTS | -10 | In the system, there can only be one asset with the `STATS_QUOTE_ASSET` policy |
| INITIAL\_PREISSUED\_EXCEEDS\_MAX\_ISSUANCE | -11 | Initial pre-issued amount exceeds max issuance amount. |
| INVALID\_DETAILS | -12 | Details must be a valid JSON string. If you see this error, it is probably a bug in the js-sdk code that serializes the object |

