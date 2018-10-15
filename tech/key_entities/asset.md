# Asset

Asset has such properties:

### Code

Code is a unique identifier for the asset. Valid asset code should not contain
more than 16 UTF-8 symbols.

### Owner

The account ID of account that created the asset.

### Issued

The amount of asset that is currently in circulation and is being hold by one or
more system accounts.

### Max issuance amount

The maximum amount of asset that can be issued.

### Available for issuance

The amount of asset that is not in a circulation but can be issued with 
[issuance][3] operation.

### Pre-issued asset signer

The public key of the only valid signer of the offline pre-issuance request. 

### Policies

*Asset policies* it the properties of asset that define it's behaviour. We can 
think of them as a configuration of asset. At this time TokenD assets may have
such policies: 

| Name                            | Description |
|:-------------------------------:|-------------|
| TRANSFERABLE                    | If set, asset can be transferred by [Payment][1] operation |
| BASE_ASSET                      | If set, every newly created [account][2] will have balance for such asset |
| STATS_QUOTE_ASSET               | If set, asset will be user for statistics calculations |
| WITHDRAWABLE                    | If set, asset can be withdrawn |
| REQUIRES_KYC                    | If set, all operations with asset requires all counterparties to be `general` or `syndicate` |
| ISSUANCE_MANUAL_REVIEW_REQUIRED | If set, all [issuance requests][3] should be reviewed by specific signer |



<!--1. Payment op-->

[1]: /coming_soon.md
[2]: /tech/key_entities/accounts.md
[3]: /tech/requests/request_issuance.md