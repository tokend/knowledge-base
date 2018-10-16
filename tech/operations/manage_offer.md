## Manage offer

Manage offer adds an offer to secondary market order book.

## Source account details

| Property              | Value              |
|-----------------------|--------------------|
| Threshold             | `MEDIUM`           |
| Account types         | `GENERAL`, `NOT_VERIFIED`, `SYNDICATE` |
| Signer types          | `BALANCE_MANAGER`  |

## Parameters

| Parameter    | Type   | Description           |
|--------------|--------|-----------------------|
| baseBalance  | string | The base balance of your account. Should be valid `Balance ID` |
| quoteBalance | string | The quote balance of your account. Should be valid `Balance ID` |
| orderBookID  | string | The ID of the order book, always `'0'` for secondary market |
| isBuy        | string | If true, offer is about buying base asset, if false - about selling |
| amount       | string | The amount of `base asset`, no matter if offer is buy of sell |
| price        | string | The price in `quote asset`: `1 base = 1 quote * price` |

## Examples

We want to sell 3 `ETH` for `BTC` with price `1 ETH = 0.25 BTC`:

```javascript
const operation = base.ManageOfferBuilder.manageOffer({
  baseBalance: 'BBPD36ALD3TLJQNKEYR6EFCQKRBNBYTX3V3QS5ZT4A462SAX7RUMN636', // ETH balance
  quoteBalance: 'BDK7B4LXL5IMQKS4FV5BWEKTAFS5DD6LGAMWALPZFOTMJKHAOKBEDSRF', // BTC balance
  orderBookID: '0', // always 0 for secondary market
  isBuy: false, // we want to sell base asset(ETH)
  amount: '3', // we want to sell 3 ETH
  price: '0.25' // we want 0.25 BTC for each ETH
})

await horizon.transactions.submitOperations(operation)
```

## Possible errors

| Error | Code | Description|
|-------|------|------------|
| MALFORMED | -1 | Generated offer would be invalid.
| PAIR_NOT_TRADED | -2 | It's not allowed to trade with this pair.
| BALANCE_NOT_FOUND | -3 | Does not own balance for buying or selling.
| UNDERFUNDED | -4 | Doesn't hold what it's trying to sell.
| CROSS_SELF | -5 | The account has opposite offer of equal or lesser price active, so the account creating this offer would immediately cross itself.
| OFFER_OVERFLOW | -6 | Overflow occurred during the performing an offer.
| ASSET_PAIR_NOT_TRADABLE | -7 | It's not allowed to trade between this asset pair.
| PHYSICAL_PRICE_RESTRICTION | -8 | The offer price violates physical price restriction.
| CURRENT_PRICE_RESTRICTION | -9 | The offer price violates the current price restriction.
| NOT_FOUND | -10 | Offer ID does not match an existing offer.
| INVALID_PERCENT_FEE | -11 | The fee percentage is not valid.
| INSUFFICIENT_PRICE | -12 | The offer price is not insufficient.
| ORDER_BOOK_DOES_NOT_EXISTS | -13 | Specified order book does not exists.
| SALE_IS_NOT_STARTED_YET | -14 | Sale is not started yet.
| SALE_ALREADY_ENDED | -15 | Sale has already ended.
| ORDER_VIOLATES_HARD_CAP | -16 | After sending the offer, hard cap will be exceeded. cap.
| CANT_PARTICIPATE_OWN_SALE | -17 | It's not allowed to participate in own sale.
| ASSET_MISMATCHED | -18 | Sale assets does not match assets for specified balances.
| PRICE_DOES_NOT_MATCH | -19 | Price does not match sale price.
| PRICE_IS_INVALID | -20 | Price must be positive.
| UPDATE_IS_NOT_ALLOWED | -21 | Update of the offer is not allowed.
| INVALID_AMOUNT | -22 | Amount must be positive.
| SALE_IS_NOT_ACTIVE | -23 | An attempt to access a sale that is inactive.
| REQUIRES_KYC | -24 | Source must have KYC in order to participate
