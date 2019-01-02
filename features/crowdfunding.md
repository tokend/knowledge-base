# Crowdfunding (WIP TokenD v2.0)

TokenD has built in templed smartcontract which allows to perform onchain crowdfunding which various pricing mechanics. In general flow would be following:

1. Creator (actor who is creating asset sale) needs to have asset created in the system which sufficient number of assets available for issuance (defined in the next steps) and needs to have role (account type), which grants permission to create asset sales.
1. Company specifies tokens sale parameters (note: only common parameters of all sale types specified):
    1. `Start date` - point in time after which users are able to start investing;
    1. `End date` - point in time after which any changes to investment (creation of new, updating the size, canceling) is not allowed and settlement will be performed.
    1. `Base asset` - asset to be distributed in case of successful sale. Creator must be owner of this asset.
    1. `Default quote asset` - asset in which performance of the sale is calculated.
    1. `Quote assets` - list of assets in which investments are accepted.
    1. `Soft cap` - minimal amount in `default quote asset` of investments to be collected for successful sale.
    1. `Hard cap` - max amount in `default quote asset` to be accepted.
1. Depending on the default tasks specified by system administrator for `sale creation request`: sale will be created immediately or sale creation request will require some additional checks from the administrator.
1. Investor (must have corresponding role which grants permission to operate with `base asset`) performs investment in one of the quote assets.
1. On predefined schedule auxillary offchain module `sale checker` sends operation which checks current state of the sale and distributes the base asset among investors and transfers investments to the creator (if the current cap calculated based on the amount of investments converted into default quote asset using current price of corresponding asset pairs) or returns investments to the investors and unlocks corresponding amount of base tokens.

## Basic sale

Price for each base asset quote asset pair is fixed on the step of sale creation and cannot be changed. Number of tokens to be received by investor is known on the step of investment. Current cap fluctuates based on default quote asset quote assets pairs.
Amount of base asset to be distributed fluctuates (will not exceed `max amount ot be sold`).

## Crowdfunding

Amount of base asset to be distributed is fixed (equal to `max amount to be sold`). Price for `base asset`/`quote asset` pairs is defined by `current cap` and `max amount to be sold`. Number of tokens to be received by investor known on `check sale state` step.  

## Fixed price

`Base asset`/`default quote asset` price is fixed on creation step and cannot be changed (defined by `max amount to be sold` and `had cap`).  Number of tokens to be received by investor known on `check sale state` step.  `base asset`/`quote asset` price is calculated using `Base asset`/`default quote asset` and `default quote asset/quote asset`.
