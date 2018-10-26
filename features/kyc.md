# Know Your Customer

TokenD system provides a powerful set of data agnostic tools for easy implementation of custom KYC procedure based on the regulations of a specific jurisdiction. Tasks based approach allows to do various verification steps which could include manual verification as well as integration with external system which can provide sanction lists screening, documents verification, etc.

Example of KYC request:

```json
{
   "id":"46",
   "paging_token":"46",
   "requestor":"GANY5G7RK644BVTMWIPFKFATM7NYAWUXSNI6I5CJIDEN2ORUAADSZ7UF",
   "reviewer":"GA4CZMOLWKO6RBKT77V2W4JGKVKYHSUAATVDJYPUYVGTYQ7BLTVH5CBP",
   "reference":"3b28a6c3766c6295adad976d349705ef49393fbfdebe4facb5862d2cdfa285d3",
   "reject_reason":"",
   "hash":"de74625001957dd6f0c161df4ea5d5359f74394125bb1b9547bcdabc70d9d50a",
   "details":{
      "request_type_i":9,
      "request_type":"update_kyc",
      "update_kyc":{
         "account_to_update_kyc":"GANY5G7RK644BVTMWIPFKFATM7NYAWUXSNI6I5CJIDEN2ORUAADSZ7UF",
         "account_type_to_set":{
            "int":2,
            "string":"general"
         },
         "kyc_level":0,
         "kyc_data":{
            "blob_id":"CUTGPFUJ6LYTMO4PLVGDDXQZRDZD244TAMZO54Z2DWMSBAMWSTUA"
         },
         "all_tasks":30,
         "pending_tasks":0,
         "sequence_number":0,
         "external_details":[
            {

            }
         ]
      }
   },
   "created_at":"2018-06-18T16:33:51Z",
   "updated_at":"2018-06-18T16:34:16Z",
   "request_state_i":3,
   "request_state":"approved"
}
```

__Note__: Detailed description of this section will be provided by 01/09/2018. If you have any questions, do not hesitate to contact dh@distributedlab.com