# Tokens Management

TokenD system can be used to track track, hold, and transfer any type of asset: dollars, euros, bitcoin, stocks, gold, and other tokens of value. Any asset on the network can be traded and exchanged with any other. What is more, TokenD supports system tokens created by admin of the system (signer of `MASTER` account) and user tokens created by accounts with type `SYNDICATE`. Each user token requires approval from the admin of the system to be created. It's possible to define custom rules for each tokens:

* should it be transferable;
* exchangeable with certain tokens;
* does user needs to complete KYC to be able to hold the token;
* what limitations for withdrawals, transfers or holdings should be applied;
* what king of fees should be applied for operations with that token.

## Token Creation Operation

* **code** &mdash; unique identifier of the token;
* **preissuedAssetSigner** &mdash; public key of the key pair which is allowed to authorize certain amount of tokens to be available for issuance. This key is only specified on asset creation; signers of the owner of the token are not able to change it.
* **maxIssuanceAmount** &mdash; maximum amount of tokens to be available in the system.
* **initialPreissuedAmount** &mdash; amount of tokens available for issuance right after creation of the token.
* **policies** &mdash; defines the rules of turnover of the token. (Can be update after creation). Following policies are available is the system:
    * TRANSFERABLE &mdash; can be transferred between accounts able to hold this token.
    * BASE_ASSET &mdash; one creation of new account balance for such token will be created automatically.
    * WITHDRAWABLE  &mdash; allows to create withdrawal request.
    * REQUIRES_KYC  &mdash; only accounts with type `GENERAL` or `SYNDICATE` are allowed to hold such token.
    * ISSUANCE_MANUAL_REVIEW_REQUIRED &mdash; issuance of such token requires additional confirmation from the admin.
    * REQUIRES_VERIFICATION  &mdash; only accounts with type `VERIFIED`, `GENERAL` or `SYNDICATE` are allowed to hold such token.
* **details** &mdash; json object. Can be used to store any additional information for the token. (Can be update after creation).

Example of created token

```json
{
   "code":"BTC",
   "owner":"GA4CZMOLWKO6RBKT77V2W4JGKVKYHSUAATVDJYPUYVGTYQ7BLTVH5CBP",
   "available_for_issuance":"999997769.920000",
   "preissued_asset_signer":"GBZHP5662QGSAQITTND2VRMMEH5LCTUMGKQWZGG2BJ54IKWXVVTBT2HD",
   "max_issuance_amount":"1000000000.000000",
   "issued":"2230.080000",
   "pending_issuance":"0.000000",
   "policy":27,
   "policies":[
      {
         "name":"AssetPolicyTransferable",
         "value":1
      },
      {
         "name":"AssetPolicyBaseAsset",
         "value":2
      },
      {
         "name":"AssetPolicyWithdrawable",
         "value":8
      },
      {
         "name":"AssetPolicyTwoStepWithdrawal",
         "value":16
      }
   ],
   "details":{
      "external_system_type":"",
      "logo":{
         "key":"dpurgh4infnubjhcost7fvpl45vrj3ctszl7z7kqnjk2eyf346b3s6xi",
         "name":"Bitcoin.png",
         "type":"image/png"
      },
      "name":"BTC",
      "terms":{
         "key":"",
         "name":"",
         "type":""
      }
   }
}
```