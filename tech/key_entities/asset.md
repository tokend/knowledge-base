# Asset

The asset has following properties:

### Code

Code is a unique identifier for the asset. Valid asset code should not contain
more than 16 UTF-8 symbols.

### Owner

The ID of the account that created the asset.

### Issued

The amount of asset that is currently in circulation and is being hold by one or
more system accounts.

### Max issuance amount

The maximum amount of asset that can ever be issued.

### Available for issuance

The amount of asset that is not in circulation but can be issued with the
[issuance][3] operation.

### Pre-issued asset signer

The public key of the valid signer of the pre-issuance request generated on offline device.

### Policies

*Asset policies*  – the properties of asset that define its behaviour. It is a kind of a configuration of the asset. Currently, TokenD assets have the following properties: 

| Name                            | Description |
|:-------------------------------:|-------------|
| TRANSFERABLE                    | If set, asset can be transferred by the [Payment][1] operation |
| BASE_ASSET                      | If set, every newly created [account][2] will automatically have this asset on their balance. |
| STATS_QUOTE_ASSET               | If set, this specific asset will be used for statistics calculation on user dashboard (Note: only one asset one the platform can be marked as STATS_QUOTE_ASSET.) |
| WITHDRAWABLE                    | If set, asset can be withdrawn |
| REQUIRES_KYC                    | If set, only `general` and `syndicate` accounts are able to operate with this asset|
| ISSUANCE_MANUAL_REVIEW_REQUIRED | If set, all [issuance requests][3] should be reviewed by a specific signer |

[1]: /tech/operations/payment.md
[2]: /tech/key_entities/accounts.md
[3]: /tech/requests/request_issuance.md
