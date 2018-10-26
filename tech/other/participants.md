# Participants

## Managing offers

Despite that manage offer operation is quite flexible, we need a way to somehow define all matches that were covered
by one `Manage offer` operation. 


### Who is who

Let's consider such example: Bob and Alice want to trade their `ETH` tokens for `BTC` tokens. We have an order book,
where base asset is `ETH` and quote is `BTC`. They both have made such offers: Bob is selling 1 `ETH` with 
price equal to 0.3 `BTC` and Alice is selling 1 `ETH` with price equal to 0.2 `BTC`. There are no opposite offers, 
so Bob and Alice should wait until somebody will come and fulfill their offers.

Now, John makes an offer that says that he wants to buy 2 `ETH` with the price `1 ETH = 0.3 BTC`. After John created this offer, the system automatically matched John's offer with those of Bob and Alice, so everybody gets what they want.

In this situation, we have a `manage offer` operation in the ledger, that has Bob's account as a source, 
3 participants (John, Alice, and Bob), and 2 matches (John's match with Alice's offer and John's match with Bob's offer).

### Detailed look

Let's consider the JSON representation of such `Manage offer` operation. Here is a legend:

Bob's details:

* Account ID is `GAYGENXYOLKC5GDRCZYICIUNAH4772FGOMLHODAYTUMKZ6I6HBPL3QRC`;
* ETH balance is `BBPD36ALD3TLJQNKEYR6EFCQKRBNBYTX3V3QS5ZT4A462SAX7RUMN636`;
* BTC balance is `BDK7B4LXL5IMQKS4FV5BWEKTAFS5DD6LGAMWALPZFOTMJKHAOKBEDSRF`;

Alice's details:

* Account ID is `GD75UE5WBHCCTWJZY3WIQAAYOHRKZ5JFNZL5P5BZJB5BPMWXKQZTVOAE`
* ETH balance is `BDJUAYQ3Z7C3WWFUKMR3OKJFTWP75SVWN7PKKTAFIICGZW2H747M44HH`
* BTC balance is `BAGUA7VCVYVB3D7C4UXCV2NIVPIP3LMPYDNLBNZW3XCCWCKBD2OX2FZ4`

John's details

* Account ID us `GDRTADZRZQMZFGU7VKJXHFLLYNQ4IAWG74QDNPTMDP3GDI3RPHN5PGZY`
* ETH balance is `BDBJBDMUPOX3FPNKHO2EEP4C2SJJVUBUNQ5WXWDV62WWXDYDUCRIZWQK`
* BTC balance is `BAQ2NISAFVXM23J52FLNZAG2GPP66CMEDMS6U367LUZIKBY7O6UOOSOD`

```json
{
     "id": "5471298708836353",
     "paging_token": "5471298708836353",
     "transaction_id": "5471298708836352",
     "source_account": "GAYGENXYOLKC5GDRCZYICIUNAH4772FGOMLHODAYTUMKZ6I6HBPL3QRC",
     "type": "manage_offer",
     "type_i": 16,
     "state_i": 7,
     "state": "fully matched",
     "identifier": "0",
     "ledger_close_time": "2018-10-02T14:34:47Z",
     "is_buy": false,
     "base_asset": "",
     "amount": "2.000000",
     "price": "0.300000",
     "fee": "0.000000",
     "offer_id": 0,
     "order_book_id": 0,
     "participants": [
     // JOHN ETH:
     {
        "account_id": "GDRTADZRZQMZFGU7VKJXHFLLYNQ4IAWG74QDNPTMDP3GDI3RPHN5PGZY", // john's account ID
        "balance_id": "BDBJBDMUPOX3FPNKHO2EEP4C2SJJVUBUNQ5WXWDV62WWXDYDUCRIZWQK", // john's ETH balance ID
        "effects": [
            {
              "base_asset": "ETH", // order book has it's base asset ETH
              "quote_asset": "BTC", // order book has it's qoute asset BTC
              "is_buy": true, // John bought base asset (ETH)
              "matches": [
                  {
                      "base_amount": "1.000000", // John bought 1 base (ETH) from Alice
                      "price": "0.200000", // With the price 0.2
                      "quote_amount": "0.200000", // Alice got 0.2 quote (BTC), John lost 0.2 quote (BTC)
                      "fee_paid": "0.000000" // John paid 0 fees
                  },
                  {
                      "base_amount": "1.000000", // John bought 1 base (ETH) from Bob
                      "price": "0.300000", // With the price 0.3
                      "quote_amount": "0.300000", // Bob got 0.3 quote (BTC), John lost 0.3 quote (BTC)
                      "fee_paid": "0.000000" // John paid 0 fees
                  }
              ]
            }
        ]
     },
     
     // JOHN BTC:
     {
        "account_id": "GDRTADZRZQMZFGU7VKJXHFLLYNQ4IAWG74QDNPTMDP3GDI3RPHN5PGZY", // john's account ID
        "balance_id": "BAQ2NISAFVXM23J52FLNZAG2GPP66CMEDMS6U367LUZIKBY7O6UOOSOD", // john's BTC balance ID
        "effects": [
            {
              "base_asset": "ETH", // order book has it's base asset ETH
              "quote_asset": "BTC", // order book has it's qoute asset BTC
              "is_buy": true, // John bought base asset (ETH)
              "matches": [
                  {
                      "base_amount": "1.000000", // John bought 1 base (ETH) from Alice
                      "price": "0.200000", // With the price 0.2
                      "quote_amount": "0.200000", // Alice got 0.2 quote (BTC), John lost 0.2 quote (BTC)
                      "fee_paid": "0.000000" // John paid 0 fees
                  },
                  {
                      "base_amount": "1.000000", // John bought 1 base (ETH) from Bob
                      "price": "0.300000", // With the price 0.3
                      "quote_amount": "0.300000", // Bob got 0.3 quote (BTC), John lost 0.3 quote (BTC)
                      "fee_paid": "0.000000" // John paid 0 fees
                  }
              ]
            }
        ]
     },
     
     // ALICE ETH:
     {
         "account_id": "GD75UE5WBHCCTWJZY3WIQAAYOHRKZ5JFNZL5P5BZJB5BPMWXKQZTVOAE", // alice's account ID
         "balance_id": "BDJUAYQ3Z7C3WWFUKMR3OKJFTWP75SVWN7PKKTAFIICGZW2H747M44HH", // alice's ETH balance ID
         "effects": [
            {
              "base_asset": "ETH", // order book has it's base asset ETH
              "quote_asset": "BTC", // order book has it's qoute asset BTC
              "is_buy": false, // Alice sold base asset (ETH)
              "matches": [
                  {
                      "base_amount": "1.000000", // Alice sold 1 base (ETH)
                      "price": "0.200000", // With the price 0.2
                      "quote_amount": "0.200000", // Alice got 0.2 quote (BTC)
                      "fee_paid": "0.000000" // Alice paid 0 fees
                  }
              ]
            }
         ]
     },
     // ALICE BTC
     {
         "account_id": "GD75UE5WBHCCTWJZY3WIQAAYOHRKZ5JFNZL5P5BZJB5BPMWXKQZTVOAE", // alice's account ID
         "balance_id": "BAGUA7VCVYVB3D7C4UXCV2NIVPIP3LMPYDNLBNZW3XCCWCKBD2OX2FZ4", // alice's BTC balance ID
         "effects": [
            {
              "base_asset": "ETH", // order book has it's base asset ETH
              "quote_asset": "BTC", // order book has it's qoute asset BTC
              "is_buy": false, // Alice sold base asset (ETH)
              "matches": [
                  {
                      "base_amount": "1.000000", // Alice sold 1 base (ETH)
                      "price": "0.200000", // With the price 0.2
                      "quote_amount": "0.200000", // Alice got 0.2 quote (BTC)
                      "fee_paid": "0.000000" // Alice paid 0 fees
                  }
              ]
            }
         ]
     },
     // BOB BTC:
     {
         "account_id": "GAYGENXYOLKC5GDRCZYICIUNAH4772FGOMLHODAYTUMKZ6I6HBPL3QRC", // bob's account ID
         "balance_id": "BBPD36ALD3TLJQNKEYR6EFCQKRBNBYTX3V3QS5ZT4A462SAX7RUMN636", // bob's BTC balance ID
         "effects": [
            {
              "base_asset": "ETH", // order book has it's base asset ETH
              "quote_asset": "BTC", // order book has it's qoute asset BTC
              "is_buy": false, // Bob sold base asset (ETH)
              "matches": [
                  {
                    "base_amount": "1.000000", // Bob sold 1 base (ETH)
                    "price": "0.300000", // With the price 0.3
                    "quote_amount": "0.300000", // Bob got 0.3 quote (BTC)
                    "fee_paid": "0.000000" // Bob paid 0 fees
                  }
              ]
            }
         ]
     },
     // BOB ETH:
     {
         "account_id": "GAYGENXYOLKC5GDRCZYICIUNAH4772FGOMLHODAYTUMKZ6I6HBPL3QRC", // bob's account ID
         "balance_id": "BDK7B4LXL5IMQKS4FV5BWEKTAFS5DD6LGAMWALPZFOTMJKHAOKBEDSRF", // bob's BTC balance ID
         "effects": [
            {
              "base_asset": "ETH", // order book has it's base asset ETH
              "quote_asset": "BTC", // order book has it's qoute asset BTC
              "is_buy": false, // Bob sold base asset (ETH)
              "matches": [
                  {
                    "base_amount": "1.000000", // Bob sold 1 base (ETH)
                    "price": "0.300000", // With the price 0.3
                    "quote_amount": "0.300000", // Bob got 0.3 quote (BTC)
                    "fee_paid": "0.000000" // Bob paid 0 fees
                  }
              ]
            }
         ]
     }
   ]
}
```

It looks a bit complicated, so let's look at this object in more details:

Take a look at the root fields: `source_account`, `price`, `amount`, and `is_buy`. We can make a conclusion 
that `source_account` (we know it's John) made an offer for selling `amount` (0.2) of base asset with some 
`price` (0.3) that is calculated in the quote asset. It doesn't mean that this price will be in the result. For 
now, we just know that John made such an offer.

Let's find all matches (that is, what exactly happened to John's account) by finding John's participation:

```javascript
const johnAccountId = `GDRTADZRZQMZFGU7VKJXHFLLYNQ4IAWG74QDNPTMDP3GDI3RPHN5PGZY`
const johnParticipants = json.participants.find(p => p.account_id === johnAccountId)
```

Because John's 2 balances participated in this operation, the code above will result in the following:

```json
 {
        "account_id": "GDRTADZRZQMZFGU7VKJXHFLLYNQ4IAWG74QDNPTMDP3GDI3RPHN5PGZY", // john's account ID
        "balance_id": "BDBJBDMUPOX3FPNKHO2EEP4C2SJJVUBUNQ5WXWDV62WWXDYDUCRIZWQK", // john's ETH balance ID
        "effects": [
            {
              "base_asset": "ETH", // order book has it's base asset ETH
              "quote_asset": "BTC", // order book has it's qoute asset BTC
              "is_buy": true, // John bought base asset (ETH)
              "matches": [
                  {
                      "base_amount": "1.000000", // John bought 1 base (ETH) from Alice
                      "price": "0.200000", // With the price 0.2
                      "quote_amount": "0.200000", // Alice got 0.2 quote (BTC), John lost 0.2 quote (BTC)
                      "fee_paid": "0.000000" // John paid 0 fees
                  },
                  {
                      "base_amount": "1.000000", // John bought 1 base (ETH) from Bob
                      "price": "0.300000", // With the price 0.3
                      "quote_amount": "0.300000", // Bob got 0.3 quote (BTC), John lost 0.3 quote (BTC)
                      "fee_paid": "0.000000" // John paid 0 fees
                  }
              ]
            }
        ]
     },
     
     // JOHN BTC:
     {
        "account_id": "GDRTADZRZQMZFGU7VKJXHFLLYNQ4IAWG74QDNPTMDP3GDI3RPHN5PGZY", // john's account ID
        "balance_id": "BAQ2NISAFVXM23J52FLNZAG2GPP66CMEDMS6U367LUZIKBY7O6UOOSOD", // john's BTC balance ID
        "effects": [
            {
              "base_asset": "ETH", // order book has it's base asset ETH
              "quote_asset": "BTC", // order book has it's qoute asset BTC
              "is_buy": true, // John bought base asset (ETH)
              "matches": [
                  {
                      "base_amount": "1.000000", // John bought 1 base (ETH) from Alice
                      "price": "0.200000", // With the price 0.2
                      "quote_amount": "0.200000", // Alice got 0.2 quote (BTC), John lost 0.2 quote (BTC)
                      "fee_paid": "0.000000" // John paid 0 fees
                  },
                  {
                      "base_amount": "1.000000", // John bought 1 base (ETH) from Bob
                      "price": "0.300000", // With the price 0.3
                      "quote_amount": "0.300000", // Bob got 0.3 quote (BTC), John lost 0.3 quote (BTC)
                      "fee_paid": "0.000000" // John paid 0 fees
                  }
              ]
            }
        ]
     },
     
```

However, we only need one participant to define what happened, because all the information they contain is
similar. Also, when parsing `manage offer`, we know for sure that the array of `effects` will always 
contain only 1 element. So, all the data we need about John's participation is stored in the first `effect`:

```javascript
const effect = johnParticipants[0].effects[0]
```


```json
{
   "base_asset": "ETH", // order book has it's base asset ETH
   "quote_asset": "BTC", // order book has it's qoute asset BTC
   "is_buy": true, // John bought base asset (ETH)
   "matches": [
       {
           "base_amount": "1.000000", // John bought 1 base (ETH) from Alice
           "price": "0.200000", // With the price 0.2
           "quote_amount": "0.200000", // Alice got 0.2 quote (BTC), John lost 0.2 quote (BTC)
           "fee_paid": "0.000000" // John paid 0 fees
       },
       {
           "base_amount": "1.000000", // John bought 1 base (ETH) from Bob
           "price": "0.300000", // With the price 0.3
           "quote_amount": "0.300000", // Bob got 0.3 quote (BTC), John lost 0.3 quote (BTC)
           "fee_paid": "0.000000" // John paid 0 fees
       }
   ]
}
```

And finally, we can find the exact values of matches that somehow affected John's balance. We can see, that 
John here obtained `2 ETH` and paid for them `0.5 BTC` without any fees.

The same works as well for Alice:

```javascript
const aliceAccountId = `GD75UE5WBHCCTWJZY3WIQAAYOHRKZ5JFNZL5P5BZJB5BPMWXKQZTVOAE`
const aliceParticipants = json.participants.find(p => p.account_id === aliceAccountId)
const aliceEffect = aliceParticipants[0].effects[0]
```

```json
{
    "base_asset": "ETH", // order book has it's base asset ETH
    "quote_asset": "BTC", // order book has it's qoute asset BTC
    "is_buy": false, // Alice sold base asset (ETH)
    "matches": [
        {
            "base_amount": "1.000000", // Alice sold 1 base (ETH)
            "price": "0.200000", // With the price 0.2
            "quote_amount": "0.200000", // Alice got 0.2 quote (BTC)
            "fee_paid": "0.000000" // Alice paid 0 fees
        }
    ]
}
```

and for Bob:

```javascript
const bobAccountId = `GAYGENXYOLKC5GDRCZYICIUNAH4772FGOMLHODAYTUMKZ6I6HBPL3QRC`
const bobParticipants = json.participants.find(p => p.account_id === bobAccountId)
const bobEffect = bobParticipants[0].effects[0]
```

```json
{
     "base_asset": "ETH", // order book has it's base asset ETH
     "quote_asset": "BTC", // order book has it's qoute asset BTC
     "is_buy": false, // Bob sold base asset (ETH)
     "matches": [
         {
           "base_amount": "1.000000", // Bob sold 1 base (ETH)
           "price": "0.300000", // With the price 0.3
           "quote_amount": "0.300000", // Bob got 0.3 quote (BTC)
           "fee_paid": "0.000000" // Bob paid 0 fees
         }
     ]
}
```
