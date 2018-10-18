# Tokens Management

TokenD system can be used to track, hold, and transfer any type of assets: dollars, euros, bitcoin, stocks, gold, and other tokens of value. Any asset on the network can be traded and exchanged with any other. What is more, TokenD supports system tokens created by admin of the system (signer of `MASTER` account) and user tokens created by accounts with `SYNDICATE` type. Each user token requires approval from the admin of the system to be created. It's possible to define custom rules for each tokens:

* should it be transferable/non-transferable;
* should it exchangeable/non-exchangeable with particular tokens;
* whether a user needs to complete KYC to be able to operate with the token or not;
* what limitations for withdrawals, transfers, or holdings should be applied;
* what kind of fees for operations with the token should be applied.

## Token Creation Operation

* **code** &mdash; unique identifier of the token;
* **preissuedAssetSigner** &mdash; public key of the key pair which is allowed to authorize certain amount of tokens to be available for issuance. This key is only specified on asset creation; token owner signers are not able to change it.
* **maxIssuanceAmount** &mdash; maximum amount of tokens to be available in the system.
* **initialPreissuedAmount** &mdash; amount of tokens which will be issued immediately after token creation.
* **policies** &mdash; defines the rules of turnover of the token. (can be updated after token creation). In the system, the folloing policies are available:
    * TRANSFERABLE &mdash; can be transferred between accounts that are able to operate with this token.
    * BASE_ASSET &mdash; one creation of new account balance for such token will be created automatically.
    * WITHDRAWABLE  &mdash; allows to create withdrawal requests.
    * REQUIRES_KYC  &mdash; only accounts with type `GENERAL` or `SYNDICATE` are allowed to operate with this token.
    * ISSUANCE_MANUAL_REVIEW_REQUIRED &mdash; issuance of such token requires additional admin confirmation.
    * REQUIRES_VERIFICATION  &mdash; only accounts with type `VERIFIED`, `GENERAL`, or `SYNDICATE` are allowed to operate with this token.
* **details** &mdash; json object. Can be used to store any additional information for the token. (Can be update after creation).

Example of a created token

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
