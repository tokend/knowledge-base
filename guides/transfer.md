# Transfers

Transferring tokens between the accounts is actually simple. All you need to do is check the fees and send the payment operation. Here is a simple guide on how to do this.

## Fees

Token transfers between the accounts can require paying fees \(both the sender and the recipient can choose to pay fees for the transfer\). The fees present on the platform are defined by the admin with the [Fee manager](../technical-details/key-entities/signer.md) right.

Admins can create custom fees, the imposition of which differ according to specific account, account type, types of operation, types of asset, specific amounts, etc. To get more detailed description about the fees management, see our [Horizon](https://tokend.gitlab.io/docs/#fees) reference.

### Fees example

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

```javascript
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

Here, you can see that for this operation, Alice will have to pay a `0.1 BNN` fixed fee and the `0.3` percentage fee \(of the transfer amount\): `10 * 0.3 = 3 BNN` as a percent fee.

Also, Jack may be required to pay certain fees as well. Let's check his fees for the required amount:

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

```javascript
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

As you can see, Jack is required to pay a `0.1 BNN` as a percent fee \(`10 * 0.01 = 0.1`\), while the fixed fee, fortunately, is `0`.

## Sending tokens

Sending tokens is available through the [Payment](../technical-details/operations/payment.md) operations.

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

## Other platforms

[![](https://kotlinlang.org/assets/images/favicon.ico) Transfers using Kotlin SDK](https://github.com/tokend/kotlin-sdk/wiki/Transfers)

