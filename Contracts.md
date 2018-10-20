# Create your first contract

Let's suppose, Bob wants to make a deal with Alice, while using John as escrow. Bob creates a contract and send it for review to Alice

Contract is an entity that is used when two users want to make a deal with using third participant as escrow.
Both users can start dispute that will call the escrow.
Manage contract operation allows performing actions with a contract.

## Request

OK, Bob already has a Syndicate account and is now able to create сontract. He will do this the using Create Contract Request operation:

 ``` 
    async function createBobsContractRequest () {
      const operation = base.ManageAssetBuilder.createContractRequest({
        customer: 'GBT3XFWQUHUTKZMI22TVTWRA7UHV2LIO2BIFNRCH3CXWPYVYPTMXMDGC', // Alice's account ID
        escrow: 'GBT3XFWQUHUTKZMI22TVTWRA7UHV2LIO2BIFNRCH3CXWPYVYPTMXJOHN', // John's account ID
        startTime: 'start time in timestamp',
        endTime: 'end time in timestamp',
        details: {}
      })
      
      await horizon.transactions.submitOperations(operation)
    }
 ``` 

Now, a Alice should review the contract request. Approving or rejecting the request can be performed by the Review Contract Request operation:

     function signBobsContractRequest () {
        const operation = ReviewRequestBuilder.reviewContractRequest({
          requestID
          requestHash
          requestType
          source: 'GBT3XFWQUHUTKZMI22TVTWRA7UHV2LIO2BIFNRCH3CXWPYVYPTMXMDGC', // Alice's account ID
          action
          reason
          details
        })
        await horizon.transactions.submitOperations(operation)
    }

## Create contract invoice request 

After the Alice approved Bob's contract request, Bob is now able to create invoice request to Alice.

```
    async function createBobsContractInvoiceRequest () {
      const operation = base.ManageInvoiceRequestBuilder.createInvoiceRequest({
          sender: 'GBT3XFWQUHUTKZMI22TVTWRA7UHV2LIO2BIFNRCH3CXWPYVYPTMXMDGC', // Alice's account ID,
          contractID: '0001',
          asset: 'BNN',
          amount: '100',
          details
      })
      
      await horizon.transactions.submitOperations(operation)
    }
```

## Invoice request review

Now, a Alice should review the contract invoice request. Approving or rejecting the request can be performed by the Review Invoice Request operation:

```
    function getReviewOp (action, reason) {
      ManageInvoiceRequestBuilder.reviewRequest({
        requestID: '1',
        requestHash: '2b973d51a10646505745aa7f1c860a383ea7361dac1b41779cc079dc385870dc',
        requestType
        source: 'GBT3XFWQUHUTKZMI22TVTWRA7UHV2LIO2BIFNRCH3CXWPYVYPTMXMDGC', // Alice's account ID,
        action,
        billPayDetails: {
            sourceBalanceId
            destination
            amount: '200'
            subject: '',
            feeData
          },
        reason
      })
    }
​
    async function approveBobsInvoiceRequest () {
      const action = xdr.ReviewRequestOpAction.approve().value
      const operation = getReviewOp(action, '')
​
      await horizon.transactions.submitOperations(operation)
    }
​
    async function rejectBobsInvoiceRequest () {
      const action = xdr.ReviewRequestOpAction.reject().value
      const reason = 'The reason why is Bob\'s bananas are rejected'
      const operation = getReviewOp(action, reason)
​
      await horizon.transactions.submitOperations(operation)
    }
```
## Start dispute
Beside approving or rejecting the invoice request, Alice can start dispute, if she doesnt satisfied with Bob's invoice request

Alice and Bob are able to start the dispute.

 ``` 
    async function startDispute () {
      const operation = base.ManageContractBuilder.startDispute({
        contractId: '0001',
        reason
      })
      
      await horizon.transactions.submitOperations(operation)
    }
 ``` 
 
 When dispute started Escrow is able to resolve dispute with revert all invoices amounts to Customer balances or confirm approved invoices that means invoices amounts will be unlocked on Contractor balances. All pending invoices will be in a permanent reject state.

 ``` 
    async function resolveDispute () {
      const operation = base.ManageContractBuilder.resolveDispute({
        contractId: '0001',
        isRevert: true // if true all invoice payment will be reverted
      })
      
      await horizon.transactions.submitOperations(operation)
    }
 ``` 

## Confirm contract completion 
After the Alice approved Bob's invoice request, Bob and Alice can confirm the contract completed.

 ``` 
    async function confirmContractCompleted () {
      const operation = base.ManageContractBuilder.confirmCompleted({
        contractId: '0001'
      })
      
      await horizon.transactions.submitOperations(operation)
    }
 ``` 