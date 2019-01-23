# How to trade your token

Consider now Alice wants to sell her bananas and get some apples. One of the key benefits of the TokenD-driven platform is that it is equipped with an internal exchange. This means that users can exchange tokens issued on the platform without having to list them on any exernal exchanges. Example: Alice can buy apple-backed tokens with her banana-backed tokens (that's what the secondary market essentially is). So, let's go into the details.

## Asset pairs

Asset pair is one of the key entities in TokenD. There again, we have our `BNN` (banana) and `APL` (apple) tokens created and issued on the platform.

### Create an asset pair

First of all, we need to "declare" that `BNN` can now be traded with `APL` and the price is `1 BNN = 2 APL`.
Therefore, an admin with the `ASSET_MANAGER` right has to create an [Asset pair][2] using the [ManageAssetPair][4] operation

{% tabs %} {% tab title="JavaScript" %}
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
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
// Get network params for transaction
val netParams = api
        .general
        .getSystemInfo()
        .execute()
        .get()
        .toNetworkParams()

// Compose an operation
val operation = ManageAssetPairOp(
        action = ManageAssetPairAction.CREATE,
        base = "BNN",
        quote = "APL",
        policies = 0,
        physicalPrice = netParams.amountToPrecised(
                BigDecimal("2")
        ),
        physicalPriceCorrection = 0,
        maxPriceStep = 0,
        ext = ManageAssetPairOp.ManageAssetPairOpExt.EmptyVersion()
)

// Create a transaction
val transaction =
        TransactionBuilder(netParams, walletInfo.accountId)
                .addOperation(Operation.OperationBody.ManageAssetPair(operation))
                .build()
​
// Sign it with current account
transaction.addSignature(account)
​
// Submit the transaction
api.transactions.submit(transaction).execute()
```
{% endtab %} {% endtabs %}

The pair will be created once the `ASSET_MANAGER` master signer signs the transaction:

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

### Make the pair tradable

OK, now we know that 1 Banana Token is equal to 2 Apple Tokens. The fact is that, however, users are not yet able to trade apples for bananas. The trading pair should first have an "IsTradable" property enabled. This complementary regulatory mechanism is used to prevent unwanted or useless cases. So, this is where [Asset pair policies][3] are useful.

In this way, in order to make the asset pair available for trading, an admin with the `ASSET_MANAGER` right needs to update the `Asset Pair` properties using the [ManageAssetPair][4] operation:

{% tabs %} {% tab title="JavaScript" %}
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
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
// Compose an operation
val operation = ManageAssetPairOp(
        action = ManageAssetPairAction.UPDATE_POLICIES,
        base = "BNN",
        quote = "APL",
        policies = AssetPairPolicy.TRADEABLE_SECONDARY_MARKET.value,
        physicalPrice = netParams.amountToPrecised(
                BigDecimal("2")
        ),
        physicalPriceCorrection = 0,
        maxPriceStep = 0,
        ext = ManageAssetPairOp.ManageAssetPairOpExt.EmptyVersion()
)

// Create a transaction
val transaction =
        TransactionBuilder(netParams, walletInfo.accountId)
                .addOperation(Operation.OperationBody.ManageAssetPair(operation))
                .build()
​
// Sign it with current account
transaction.addSignature(account)
​
// Submit the transaction
api.transactions.submit(transaction).execute()
```
{% endtab %} {% endtabs %}

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

Alice can now trade her bananas to get some apples.

## Start trading

Same as token transfers, trading can require paying fees. Let's suppose Alice wants to sell 5 bananas (BNN) for apples (APL) at a price of 3 apples for banana. Trading fee is always calculated and paid in quote asset, apples in our case. For selling 5 bananas at selected price Alice expects to receive 15 apples (`Quote amount = Base amount * Price`). So the request will look as follows:

{% tabs %} {% tab title="JavaScript" %}
```javascript
async function getAliceFees() {
  const feeType = FEE_TYPES.offerFee

  const aliceFees = await horizon.fees.get(feeType, {
    asset: 'APL',
    subtype: 0,
    account: 'GALHS6JXCSMAGXIOCGQYYEJ7AKI3AXWXLRNNFN3EWAIQBHAN6YNRYUTK', // Alice's account ID
    amount: '15.000000'
  })

  return aliceFees
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val aliceFee = signedApi.fees.getByType(
                FeeType.OFFER_FEE.value,
                FeeParams(
                        asset = "APL",
                        // Alice's account ID
                        accountId = "GALHS6JXCSMAGXIOCGQYYEJ7AKI3AXWXLRNNFN3EWAIQBHAN6YNRYUTK",
                        amount = BigDecimal("15")
                )
).execute().get()
```
{% endtab %} {% endtabs %}

An account can make offers to buy or sell assets using the [ManageOffer][5] operation. Before Alice creates an offer, she has to make sure that she actually holds the asset she wants to sell. The offer creation is as follows:

{% tabs %} {% tab title="JavaScript" %}
```javascript
async function createOfferToSellBananas () {
    const operation = ManageOfferBuilder.manageOffer({
       baseBalance: 'BBKVOTHCUDI4X5MFYNQN7YEAJYY7OPS3HO7J3BBESPQCV23MXW7LLMKR', // Alice's bananas balance ID
       quoteBalance: 'BAEUBIHPHVI6X3PDC7HSIHVN5OUB7UU3AVTOGUOZHCRVUGPORPIIHS44', // Alice's apples balance ID
       isBuy: false, // we are going to sell the base asset (Bananas here)
       amount: '5.000000', // Alice wants to sell 5 Bananas
       price: '3.000000', // 1 Banana = 3 Apples
       orderBookID: '0',
       fee: await getAliceFees().percent
    })

    await horizon.transactions.submitOperations(operation)
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
// Compose an operation
val operation = CreateOfferOp(
        // Alice's bananas balance ID
        baseBalanceId = "BBKVOTHCUDI4X5MFYNQN7YEAJYY7OPS3HO7J3BBESPQCV23MXW7LLMKR",
        // Alice's apples balance ID
        quoteBalanceId = "BAEUBIHPHVI6X3PDC7HSIHVN5OUB7UU3AVTOGUOZHCRVUGPORPIIHS44",
        // We are going to sell the base asset (Bananas here)
        isBuy = false,
        // Alice wants to sell 5 Bananas
        baseAmount = netParams.amountToPrecised(
                BigDecimal("5")
        ),
        // 1 Banana = 3 Apples
        price = netParams.amountToPrecised(
                BigDecimal("3")
        ),
        fee = netParams.amountToPrecised(
                aliceFee.percent
        )
)

// Create a transaction
val transaction =
        TransactionBuilder(netParams, walletInfo.accountId)
                .addOperation(Operation.OperationBody.ManageOffer(operation))
                .build()
​
// Sign it with current account
transaction.addSignature(account)
​
// Submit the transaction
api.transactions.submit(transaction).execute()
```
{% endtab %} {% endtabs %}

Alice has created her offer and is now waiting for it to be fulfilled. 

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

Let's imagine that John saw her offer and agrees with the price. He now wants to get 2 bananas. John also must request the fee. At the price of 3 apple for banana quote amount will be 6 apples. The request will look as follows:

{% tabs %} {% tab title="JavaScript" %}
```javascript
async function getJohnFees() {
  const feeType = FEE_TYPES.offerFee

  const johnFees = await horizon.fees.get(feeType, {
    asset: 'APL',
    subtype: 0,
    account: 'GBHL73YWIHZWBLFBCHEGGQZQ3WVCPW76DLO67HAITO3BXOGPLKPG7FRM', // Johns's account ID
    amount: '6.000000'
  })

  return johnFees
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val johnFee = signedApi.fees.getByType(
                FeeType.OFFER_FEE.value,
                FeeParams(
                        asset = "APL",
                        // John's account ID
                        accountId = "GBHL73YWIHZWBLFBCHEGGQZQ3WVCPW76DLO67HAITO3BXOGPLKPG7FRM",
                        amount = BigDecimal("6")
                )
).execute().get()
```
{% endtab %} {% endtabs %}

John's creates his offer as follows: 

{% tabs %} {% tab title="JavaScript" %}
```javascript
async function createOfferToBuyBananas () {
    const operation = ManageOfferBuilder.manageOffer({
       baseBalance: 'BCQOBAIMVNNH7RHZTD4OVSRUX2W575VUK4RUYELRHDPXSXJ5TMS2BHAV', // John's bananas balance ID
       quoteBalance: 'BD5R7UV4PAVGPGU7BWKNKTOOEV45JD4TVVZ5GESMO4TBP6JA3W2L4HMP', // John's apples balance ID
       isBuy: true, // we are going to buy the base asset (Bananas here)
       amount: '2.000000', // John wants to buy 2 Bananas
       price: '3.000000', // 1 Banana = 3 Apples
       orderBookID: '0',
       fee: await getJohnFees().percent
    })

    await horizon.transactions.submitOperations(operation)
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
// Compose an operation
val operation = CreateOfferOp(
        // John's bananas balance ID
        baseBalanceId = "BCQOBAIMVNNH7RHZTD4OVSRUX2W575VUK4RUYELRHDPXSXJ5TMS2BHAV",
        // John's apples balance ID
        quoteBalanceId = "BD5R7UV4PAVGPGU7BWKNKTOOEV45JD4TVVZ5GESMO4TBP6JA3W2L4HMP",
        // We are going to buy the base asset (Bananas here)
        isBuy = true,
        // John wants to buy 2 Bananas
        baseAmount = netParams.amountToPrecised(
                BigDecimal("2")
        ),
        // 1 Banana = 3 Apples
        price = netParams.amountToPrecised(
                BigDecimal("3")
        ),
        fee = netParams.amountToPrecised(
                johnFee.percent
        )
)

// Create a transaction
val transaction =
        TransactionBuilder(netParams, walletInfo.accountId)
                .addOperation(Operation.OperationBody.ManageOffer(operation))
                .build()
​
// Sign it with current account
transaction.addSignature(account)
​
// Submit the transaction
api.transactions.submit(transaction).execute()
```
{% endtab %} {% endtabs %}


When the account creates an offer, it is checked against the orderbook. If the created offer matches with any existing ones, it will be fulfilled at the corresponding price. Since John's offer matches with that of Alice, it will be fulfilled automatically and John will eventually have 2 bananas, while Alice will have 6 apples and her 3 remaining bananas (that are still are about to be sold on secondary market).

[1]: /tech/key_entities/signer.md
[2]: /tech/key_entities/asset_pair.md
[3]: /tech/key_entities/asset_pair.md#policies
[4]: /coming_soon.md
[5]: /tech/operations/manage_offer.md
[6]: /tech/key_entities/signer.md

<!--2. Asset pairs-->
<!--3. Asset pair policies-->
<!--4. Manage asset pair op-->
