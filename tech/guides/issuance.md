# Token issuance

One of the key advantages TokenD provides for is that users do not need to virtually hold the actual asset in order to possess it, they can simply buy a token backed by this asset.

Every token has its owner (creator), who defines its issuing policies. The easiest way to perform issuance is to transfer (issue) tokens directly to users' accounts (in fact, the token owner can issue tokens on their own account as well). 

### T balance comes first

The issuance is performed on the balance (not the account). Therefore, Bob will not be able to issue his `BNN` tokens to Alice if she does not have the `BNN` token on her balance. If Alice is interested in Bob's `BNN` token and would like to hold it, she will have to first of all add it to her balance. To do this, Alice needs to perform the [balance creation operation][manage_balance_op]: 

```javascript
async function createBalance () {
    const operation = base.operation.ManageBalance({
        destination: 'GBYMMGDOS32QIMZ2HX4DYVXNFVDEE4G3IUSKNLM44MCTOFSCYRPF7KDE', // Alice's account ID
        asset: 'BNN',
        action: xdr.ManageBalanceAction.create()
    })

    await horizon.transactions.submitOperations(operation)
}
```

After this, Bob will ask Alice to pass him her `BNN` balance ID, whereupon Bob can issue his `BNN` tokens to Alice.

> *Note:* The `ManageBalance` operation can not only be performed by Alice 
but by the master signer who has the [`BALANCE_MANAGER`][signer_types] right. 

## Issue tokens to a specific balance

Now that all organizational aspects are done, Bob is ready to issue tokens to Alice. 
Let's consider that `BNN` token has the following state and properties:

```json
{
  "code": "BNN",
  "owner": "GCXFATAJU2O2JRNQIXQY6IIXCVDFCENYG4WV43YSWEDYYBPRPMVAVWIE",
  "available_for_issuance": "500.000000",
  "preissued_asset_signer": "GAONUHILXGWHMQNWBE4GZ3GDS6A5KDTTUV7XBDNFRI2JKAPATWVYFEYJ",
  "max_issuance_amount": "500.000000",
  "issued": "0.000000",
  "pending_issuance": "0.000000",
  "policy": 0,
  "policies": []
}
```

Let's say that Bob wants to issue `100` tokens to Alice. The `available_for_issuance` field, which shows how much tokens are currently available for issuance, displays `500`, meaning that Bob won't have any problems with issuing `100` tokens to Alice.
At first, Bob needs to know Alice's BNN balance [by her account ID][get_account_balances], while the account ID can be [obtained by email][get_account_by_email]. After this, Bob will have to perform the following operation:

```javascript
async function performIssuance () {
    const operation = base.CreateIssuanceRequestBuilder({
      asset: 'BNN',
      amount: '100.000000',
      receiver: 'BBKVOTHCUDI4X5MFYNQN7YEAJYY7OPS3HO7J3BBESPQCV23MXW7LLMKR',
      reference: 'Some unique random string',
      externalDetails: {
        foo: 'bar'
      }
    })

    await horizon.transactions.submitOperations(operation)
}
```

[manage_balance_op]: /tech/operations/manage_balance.md
[signer_types]: /tech/key_entities/signer.md#signer-types
[get_account_balances]: https://tokend.gitlab.io/docs/#get-account-balances
[get_account_by_email]: https://tokend.gitlab.io/docs/#get-account-id-by-email

