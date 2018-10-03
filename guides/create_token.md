# Create your first token

Let's suppose, Bob has a box of bananas. He now wants to create a token that will represent the banana and start 
sharing them with everybody, or even trade his bananas on coconuts. Lets write the simple app that will allow Bob to
distribute his bananas to everyone in the world.

As you know, TokenD itself is a private blockchain, so not everyone is allowed to mutate the ledger. For example, to be
able to create tokens, user must pass KYC procedure and verify that he is a trusted authority. In technical terms,
the account who creates the token must have `Syndicate` [account type][1].  

For more technical details about KYC submission, read the [KYC docs][2]

## Request

OK, Bob already has a `Syndicate` account and can start to create his own tokens. He is able to do this using
[Asset Creation Request][3] operation:

```js
    async function createBobsBananaToken () {
      const operation = base.ManageAssetBuilder.assetCreationRequest({
        requestID: '0',
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

Now, Bob can see that his request is created and is waiting for admins review.

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

Now the asset creation request should be reviewed. To review the asset you need to be the [master signer][4] 
with the `Asset manager` policy. Approving or rejecting request can be performed by [Review Request][5] operation:

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

After the `Asset Manager` approved the Bob's bananas, he is able to start distributing them. If the request is rejected,
Bob will have to update his request so the procedure repeats.

[1]: /coming_soon.md 
[2]: /coming_soon.md
[3]: /coming_soon.md
[4]: /coming_soon.md
[5]: /coming_soon.md
[6]: /coming_soon.md
[7]: /coming_soon.md

<!--1: account types-->
<!--2: kyc-->
<!--3: Asset creation request-->
<!--4: Master signers-->
<!--5: Review request operation-->
<!--6: Horizon /requests-->
<!--7: Reviewable requests-->