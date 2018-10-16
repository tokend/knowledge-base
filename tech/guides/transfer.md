# Transfers

Transferring tokens between is quite simple on TokenD. You need to do just one things: check the fees and send 
the payment operation. Here is the simple guide on how to do this.

## Fees

Transfers between accounts may require to pay some fees from sender as well as from recipient. All the fees can be
defined by the [Fee manager][1].

Fees may differ for specific accounts account types or specific amounts. To get more detailed description about how 
to manage fees, look to our [Fees][2] documentation and [Horizon][3] reference

### Get concrete fees

Let's suppose Alice wants to send Jack 10 Banana Tokens. Let's check how much fees should Alice pay in this case:

```javascript
async function getAliceFees () {
  const feeType = FEE_TYPES.paymentFee
  const feeSubtype = PAYMENT_FEE_SUBTYPES.outgoing // Alice will send this payment

  const aliceFees = await horizon.fees.get(feeType, {
    asset: 'BNN',
    subtype: feeSubtype,
    account: 'GAMHW7REYETRP55IQMYTO5NWKEPF3LRV4F32C5KQSB32E6YPZEYYASXS', // Alice's account ID
    amount: '10.000000'
  })
  
  console.log(fees.data)
}
```

```json
{
  "asset": "BNN",
  "fixed": "0.100000",
  "percent": "0.300000",
  "fee_type": 0,
  "subtype": 1,
  "account_id": "GAMHW7REYETRP55IQMYTO5NWKEPF3LRV4F32C5KQSB32E6YPZEYYASXS",
  "account_type": 0,
  "lower_bound": "0.000000",
  "upper_bound": "20.000000",
  "fee_asset": "BNN",
  "exists": true
}
```

Here we can see that for this fee Alice will required to pay `0.1 BNN` fixed fee and `0.3` share from payment amount:
`10 * 0.3 = 3 BNN` as a percent fee.

Also we know that Jack may be required to pay some fees too. Let's check his fees for required amount:

```javascript
async function getJackFees () {
  const feeType = FEE_TYPES.paymentFee
  const feeSubtype = PAYMENT_FEE_SUBTYPES.incoming // Jack will receive this payment

  const jackFees = await horizon.fees.get(feeType, {
    asset: 'BNN',
    subtype: feeSubtype,
    account: 'GBHL73YWIHZWBLFBCHEGGQZQ3WVCPW76DLO67HAITO3BXOGPLKPG7FRM', // Jack's account ID
    amount: '10.000000'
  })
  
  console.log(fees.data)
}
```

```json
{
  "asset": "BNN",
  "fixed": "0.000000",
  "percent": "0.010000",
  "fee_type": 0,
  "subtype": 2,
  "account_id": "GBHL73YWIHZWBLFBCHEGGQZQ3WVCPW76DLO67HAITO3BXOGPLKPG7FRM",
  "account_type": 0,
  "lower_bound": "0.000000",
  "upper_bound": "40.000000",
  "fee_asset": "BNN",
  "exists": true
}
```

As we see, Jack is required to pay some fees to, he will pay `0.1 BNN` as a percent fee (`10 * 0.01 = 0.1`), and,
fortunately, `0` fixed fee.

## Send tokens

Sending tokens is available through [Payment v2][4] operation. For now, [Payment v1][5] is available, but it doesn't 
support sending tokens by account ID, [Fee assets][6], and will be deprecated soon.

```javascript
function sendTokens () {
  const opertaion = PaymentV2Builder.paymentV2({
    sourceBalanceId: 'BCQOBAIMVNNH7RHZTD4OVSRUX2W575VUK4RUYELRHDPXSXJ5TMS2BHAV', // Alice's Bananas balance ID
    destination: 'GBHL73YWIHZWBLFBCHEGGQZQ3WVCPW76DLO67HAITO3BXOGPLKPG7FRM', // Jack account ID
    amount: '10.000000',
    feeData: {
      sourceFee: {
        maxPaymentFee: '0.100000',
        fixedFee: '3.000000',
        feeAsset: 'BNN'
      },
      destinationFee: {
        maxPaymentFee: '1.001',
        fixedFee: '0.001',
        feeAsset: 'BNN'
      }
    },
    subject: 'Take my bananas, Jack!',
    reference: 'Some unique random string'
  })
  
  await horizon.transactions.submitOperations(operation)
}
```

[1]: /coming_soon.md
[2]: /coming_soon.md
[3]: https://tokend.gitlab.io/docs/#fees
[4]: /tech/operations/payment.md
[5]: /tech/operations/payment.md
[6]: /coming_soon.md