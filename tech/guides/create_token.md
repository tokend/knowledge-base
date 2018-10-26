# Create your first token

Let's suppose, Bob has a box of bananas. He now wants to create a banana-backed token, issue a certain number of it,
and start sharing them with everybody or even trade. Let's write the simple app that will allow Bob to
distribute his bananas to everyone in the world.

Essentially, TokenD allows you to build a private blockchain, meanining not everyone is allowed 
to mutate the ledger. For example, to be able to create tokens, a user must 
pass the KYC procedure and verify that he is a trusted authority. In technical 
terms, the account managing to create its custom token must have the 
`SYNDICATE` [account type][1].  

For more technical details about KYC procedure, see the [KYC docs][2]

## Request

OK, Bob already has a `Syndicate` account and is now able to create his custom token. He will do this the using
[Asset Creation Request][3] operation:

```js
    async function createBobsBananaToken () {
      const operation = base.ManageAssetBuilder.assetCreationRequest({
        requestID: '0',__
        code: 'BNN',
        preissuedAssetSigner: 'GBT3XFWQUHUTKZMI22TVTWRA7UHV2LIO2BIFNRCH3CXWPYVYPTMXMDGC',
        maxIssuanceAmount: '1000.000000',
        policies: 0,
        initialPreissuedAmount: '1000.000000',
        details: {}
      })

      await horizon.transactions.submitOperations(operation)
    }
```

Now, Bob's request is created and is waiting for admins review.

```json
    {
       "id": "1",
       "requestor": "GD7AHJHCDSQI6LVMEJEE2FTNCA2LJQZ4R64GUI3PWANSVEO4GEOWB636",
       "reviewer": "GD7AHJHCDSQI6LVMEJEE2FTNCA2LJQZ4R64GUI3PWANSVEO4GEOWB636",
       "reference": "BTC",
       "hash": "2b973d51a10646505745aa7f1c860a383ea7361dac1b41779cc079dc385870dc",
       "details": {
         "request_type_i": 0,
         "request_type": "asset_create",
         "asset_create": {
           "code": "BNN",
           "pre_issued_asset_signer": "GBT3XFWQUHUTKZMI22TVTWRA7UHV2LIO2BIFNRCH3CXWPYVYPTMXMDGC",
           "max_issuance_amount": "1000.000000",
           "initial_preissued_amount": "1000.000000",
           "details": {}
         }
       },
       "created_at": "2018-10-07T17:40:11Z",
       "updated_at": "2018-10-07T17:40:11Z",
       "request_state_i": 1,
       "request_state": "pending"
    }
```

To find out more about reviewable requests, check our [Reviewable Requests documentation][7] and [Horizon reference][6]

## Review

Now, a particular admin should review the asset creation request. To be able to do this, an admin should be the [master signer][4] with the `ASSET_MANAGER` right. Approving or rejecting the request can be performed by the [Review Request][5] operation:

```javascript
    function getReviewOp (action, reason) {
      ReviewRequestBuilder.reviewRequest({
        requestID: '1',
        requestHash: '2b973d51a10646505745aa7f1c860a383ea7361dac1b41779cc079dc385870dc',
        requestType: xdr.ReviewableRequestType.assetCreate().value,
        action,
        reason
      })
    }

    async function approveBobsBananaToken () {
      const action = xdr.ReviewRequestOpAction.reject().value
      const operation = getReviewOp(action, '')
      
      await horizon.transactions.submitOperations(operation)
    }

    async function approveBobsBananaToken () {
      const action = xdr.ReviewRequestOpAction.reject().value
      const reason = 'The reason why is Bob\'s bananas are rejected'
      const operation = getReviewOp(action, reason)

      await horizon.transactions.submitOperations(operation)
    }
```

After the `Asset Manager` approved Bob's banana-backed token creation request, Bob is now able to distribute his tokens. If, however, the request is rejected, Bob will have to update it taking into consideration admin's reject reason and submit the request again.

[1]: /tech/key_entities/accounts.md#account-type
[2]: /tech/guides/kyc.md
[3]: /tech/requests/request_asset.md
[4]: /tech/key_entities/signer.md
[5]: /tech/requests/review.md
[6]: https://tokend.gitlab.io/docs/#reviewable-requests
[7]: /tech/requests/request_asset.md
