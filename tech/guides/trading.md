# How to trade your token

Consider now Alice wants to sell her bananas and get some apples. TokenDs secondary market is the good way to trade 
the tokens you hold. Let's go deeper to how does it work.

## Asset pairs

Asset pair is one of the key entities on TokenD. Consider we have our bananas tokens `BNN` in circulation as well as some 
apple tokens `APL` that were created and are used by other users of our app.

### Create asset pair

First, we want to declare that `BNN` now can be somehow sold for `APL` (and vice versa) and the price is `1 BNN = 2 APL`.
We will create an [Asset pair][2] for that by using the [ManageAssetPair][4] operation

> Only signer with `ASSET_MANAGER` policy is allowed to create/update asset pairs.

```javascript
async function createAssetPair() {
  const operation = base.operation.ManageAssetPair({
    base: 'BNN',
    quote: 'APL',
    policies: 0,
    physicalPriceCorrection: '0.000000',
    maxPriceStep: '0.000000',
    physicalPrice: '2.000000',
    action: xdr.ManageAssetPairAction.create()
  })

  await horizon.transactions.submitOperations(operation)
}
```

And if the transaction was signed by `ASSET_MANAGER` master signer, our pair should be created:

```json
[
  {
    "base": "BNN",
    "quote": "APL",
    "current_price": "2.000000",
    "physical_price": "2.000000",
    "physical_price_correction": "0.000000",
    "max_price_step": "0.000000",
    "policy": 0,
    "policies": []
  }
]
```

### Make pair tradable

OK, now we know, that 1 Banana Token equals to 2 Apple Tokens. It's a useful information, but it doesn't mean that 
anyone can start trading his apples for bananas right now. We must have some regulation mechanisms to prevent some
unwanted or useless cases. For example, it's not a good idea, to trade, for example, tickets to football matches, 
as well as lots of utility tokens. And here is where [Asset pair policies][3] are really useful.

`ASSET_MANAGER` can update `Asset Pair` policy to make it tradable by using the same [ManageAssetPair][4] operation:

```javascript
async function updatePairPolicy() {
  const operation = base.operation.ManageAssetPair({
    base: 'BNN',
    quote: 'APL',
    policies: 1,
    physicalPriceCorrection: '0.000000',
    maxPriceStep: '0.000000',
    physicalPrice: '2.000000',
    action: xdr.ManageAssetPairAction.updatePolicy()
  })

  await horizon.transactions.submitOperations(operation)
}
```

and after we have updated policy:

```json
[
  {
    "base": "BNN",
    "quote": "APL",
    "current_price": "2.000000",
    "physical_price": "2.000000",
    "physical_price_correction": "0.000000",
    "max_price_step": "0.000000",
    "policy": 0,
    "policies": [{
      "name": "AssetPairPolicyTradeableSecondaryMarket",
      "value": 1
    }]
  }
]
```

So now all preparations are done and Alice can finally trade her bananas to get some apples.

## Start trading

An account can make offers to buy or sell assets using the [ManageOffer][5] operation. In order to make an offer, 
the account must hold the asset it wants to sell. So here's how Alice can start selling her bananas for Apples:

```javascript
async function createOfferToSellBananas () {
    const operation = ManageOfferBuilder.manageOffer({
       baseBalance: 'BBKVOTHCUDI4X5MFYNQN7YEAJYY7OPS3HO7J3BBESPQCV23MXW7LLMKR', // Alice's bananas balance ID
       quoteBalance: 'BAEUBIHPHVI6X3PDC7HSIHVN5OUB7UU3AVTOGUOZHCRVUGPORPIIHS44', // Alice's apples balance ID
       isBuy: false, // we are going to sell the base asset (Bananas here)
       amount: '5.000000', // Alice wants to sell 5 Bananas
       price: '3.000000', // 1 Banana = 3 Apples
       orderBookID: '0'
    })

    await horizon.transactions.submitOperations(operation)
}
```

Alice created her offer and now waits for it to be filled. 

```json
{
   "offer_id": 2,
   "owner_id": "GALHS6JXCSMAGXIOCGQYYEJ7AKI3AXWXLRNNFN3EWAIQBHAN6YNRYUTK", // Alice's account id
   "base_asset_code": "BNN",
   "quote_asset_code": "APL",
   "is_buy": false, // Tells us Alice is selling her BNN
   "base_amount": "5.000000", // Amount in BNN
   "quote_amount": "15.000000", // Amount in APL
   "price": "3.000000",
   "created_at": "2018-10-03T12:56:10Z"
}
```

Now John saw her offer and agreed to Alice's price. He now wants to get 2 Bananas from Alice. John decides to cross
Alice's offer:

```javascript
async function createOfferToBuyBananas () {
    const operation = ManageOfferBuilder.manageOffer({
       baseBalance: 'BCQOBAIMVNNH7RHZTD4OVSRUX2W575VUK4RUYELRHDPXSXJ5TMS2BHAV', // John's bananas balance ID
       quoteBalance: 'BD5R7UV4PAVGPGU7BWKNKTOOEV45JD4TVVZ5GESMO4TBP6JA3W2L4HMP', // John's apples balance ID
       isBuy: true, // we are going to buy the base asset (Bananas here)
       amount: '5.000000', // Alice wants to sell 5 Bananas
       price: '3.000000', // 1 Banana = 3 Apples
       orderBookID: '0'
    })

    await horizon.transactions.submitOperations(operation)
}
```

When an account makes an offer, the offer is checked against the existing orderbook for that asset pair. If the 
offer crosses an existing offer, it is filled at the price of the existing offer. So John's offer was automatically 
matched with Alice's offer. Now John is got 2 Bananas, Alice got 6 Apples and she still waiting to sell her 3 Bananas
left.

[1]: /tech/key_entities/signer.md
[2]: /coming_soon.md
[3]: /coming_soon.md
[4]: /coming_soon.md
[5]: /tech/operations/manage_offer.md

<!--2. Asset pairs-->
<!--3. Asset pair policies-->
<!--4. Manage asset pair op-->
