## Manage offer

Manage offer adds an offer to secondary market order book.


| Parameter    | Type   | Description           |
|--------------|--------|-----------------------|
| baseBalance  | string | The base balance of your account. Should be valid [Balance ID][1]|
| quoteBalance | string | The quote balance of your account. Should be valid [Balance ID][1] |
| orderBookID  | string | The ID of the order book, always `'0'` for secondary market |
| isBuy        | string | If true, offer is about buying base asset, if false - about selling |
| amount       | string | The amount of `base asset`, no matter if offer is buy of sell |
| price        | string | The price in `quote asset`: `1 base = 1 quote * price` |

Example. We want to sell 3 `ETH` for `BTC` with price `1 ETH = 0.25 BTC`:

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


[1]: /coming_soon.md
