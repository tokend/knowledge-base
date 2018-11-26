# External Systems Interconnection

TokenD provides a consistent sequence of all operations occurring in the system. It can be used to easily integrate various sets of event tracking tools, CRMs, ERPs, and SCMs. Each operation also includes a set of changes applied to the ledger. They allow to easily calculate a partial state of the ledger at a specific moment in time. It can be used by the off-chain applications to calculate interest or demurrage, and perform real-time audit.

What is more, TokenD system has convenient methods to integrate external payment systems (core banking, fiat payment gateways, cryptocurrency networks). TokenD, on the blockchain level, ensures that there is no double spending on withdrawal and each issuance operation corresponds to one deposit operation in the external system. Reference implementation of integrations with popular cryptocurrencies and fiat payment processors and intuitive REST API will significantly reduce the time for custom integration.

Tokend supports the following deposit flows: BTC, DASH, ETH, ERC20
 
### Preparations

Assume that admin of the system did the following actions:

1. Create the token which represents a certain asset from external system;
1. Assign a specific `external system ID`, which will be used to link asset in the TokenD system and external system asset; For more details on how to perform this step, see `MANAGE_ASSET`;
1. Generate a set of addresses (identifiers) of the external system, which will allow automated module to link transfer performed in external system to user account in the TokenD system;
1. Submit a set of generated address to the core of the system with `external system ID` selected on second step using the `MANAGE_EXTERNAL_SYSTEM_ACCOUNT_ID_POOL_ENTRY` operation;
1. Specify the expiration period of the external system account id using the [MANAGE_KEY_VALUE](tech/manage_key_value.md) operation;

### Flow

1. User binds external system account id using `BIND_EXTERNAL_SYSTEM_ACCOUNT_ID`;
1. User initiates transfer in external system to the specified account id;
1. Automated `deposit` module monitors the external system for new incoming transfers. If a new one is confirmed, `deposit` crafts TokenD `create issuance request` transaction with corresponding details, unique reference of the deposit and `null` tasks, signs it, and sends to the `core`;
1. `core` creates an issuance request with default tasks taken from `KeyValue` table. Assume that the value of tasks is `DEPOSIT_VERIFY`;
1. User is now able to see deposit operations in the TokenD system;
1. `deposit_verify` retrieves request from the `core`, finds the corresponding transfer in the external system, ensures that they are matching, and submits the transaction that removes `DEPOSIT_VERIFY` flag to TokenD;
1. During the transaction processing, if tasks equal to `0`, the `core` tries to fulfill the issuance request. If the issuance amount is insufficient, the `core` will set `INSUFFICIENT_AVAILABLE_FOR_ISSUANCE_AMOUNT` flag in tasks bitmask. If asset to be issued has a `ISSUANCE_MANUAL_REVIEW_REQUIRED` policy, the `core` will set `ISSUANCE_MANUAL_REVIEW_REQUIRED` flag in tasks bitmask. The request will be fulfilled if, after the review, tasks are equal to `0`.
1. `funnel` module monitors the state of external system; on the new incoming transfers if total amount of funds does not exceed the threshold, it performs transfer to a Hot Wallet, otherwise to a Cold Wallet;

## Examples of audit log

Example of the operation sequence:

```json
{
  "_links": {
    "self": {
      "href": "https://api.alice.tokend.io/v2/transactions?order=asc\u0026limit=1\u0026cursor="
    },
    "next": {
      "href": "https://api.alice.tokend.io/v2/transactions?order=asc\u0026limit=1\u0026cursor=437197605965824"
    },
    "prev": {
      "href": "https://api.alice.tokend.io/v2/transactions?order=desc\u0026limit=1\u0026cursor=437197605965824"
    }
  },
  "_embedded": {
    "meta": {
      "latest_ledger": {
        "sequence": 101793,
        "closed_at": "2018-07-26T16:12:23Z"
      }
    },
    "records": [
      {
        "id": "8e9a121e63f86a1dad1971c1234a2b1b14cba88f567d4e24b951e5cc022177bb",
        "paging_token": "437197605965824",
        "hash": "8e9a121e63f86a1dad1971c1234a2b1b14cba88f567d4e24b951e5cc022177bb",
        "created_at": "2018-07-26T16:12:23Z",
        "ledger_seq": 101793,
        "envelope_xdr": "AAAAADotpAhI0YfGM89lMNDwuqh4/iE1R7nomm+ONx/1HTwMD5QsVK3lcBAAAAAAAAAAAAAAAABbYx9SAAAAAAAAAAEAAAAAAAAAAAAAAAAg6gq4ST/s/jrpkuotARntr+n98XSpVtJrryDUw+hiqAAAAAChiudBUvVtcQPUGvrosOXz6kOrnRbZOatzlp3NGbIOpgAAAAAAAAAFAAAAAAAAAAAAAAAAAAAAAfUdPAwAAABAR2KrbJ2OZ5TIWZL5p095XFm8NXFY8k13X4roBvZpa3kJ7bG2L/pHVneVj0dGtULuvOqJUHl7odWWUzhdACMRBA==",
        "result_xdr": "AAAAAAAAAAAAAAAAAAAAAQAAAAAAAAAAAAAAAAAAAAIAAAAAIOoKuEk/7P466ZLqLQEZ7a/p/fF0qVbSa68g1MPoYqgAAAABAAAAIjFBTVk1dkFONU1mVnF0cXVRZFdSdDdEdlpWQXF1WURXN3AAAAAAAAAAAAAAIOoKuEk/7P466ZLqLQEZ7a/p/fF0qVbSa68g1MPoYqgAAAACAAAAKjB4YjYwNjEwOTYwNDBhMDU1ZTg5MjNlZDIyRjUxMzU1YTE1NjYzQzhCNQAAAAAAAAAAAAAAAAAA",
        "changes": [
          {
            "effect": 0,
            "entry_type": 16,
            "payload": "AAGNoQAAABAAAAAAIOoKuEk/7P466ZLqLQEZ7a/p/fF0qVbSa68g1MPoYqgAAAABAAAAIjFBTVk1dkFONU1mVnF0cXVRZFdSdDdEdlpWQXF1WURXN3AAAAAAAAAAAAAA"
          },
          {
            "effect": 0,
            "entry_type": 16,
            "payload": "AAGNoQAAABAAAAAAIOoKuEk/7P466ZLqLQEZ7a/p/fF0qVbSa68g1MPoYqgAAAACAAAAKjB4YjYwNjEwOTYwNDBhMDU1ZTg5MjNlZDIyRjUxMzU1YTE1NjYzQzhCNQAAAAAAAAAAAAA="
          },
          {
            "effect": 0,
            "entry_type": 0,
            "payload": "AAGNoQAAAAAAAAAAIOoKuEk/7P466ZLqLQEZ7a/p/fF0qVbSa68g1MPoYqgAAAAAoYrnQVL1bXED1Br66LDl8+pDq50W2Tmrc5adzRmyDqYBAAAAAAAAAAAAAAAAAAAAAAAABQAAAAAAAAAAAAAAAAAAAAA="
          },
          {
            "effect": 0,
            "entry_type": 9,
            "payload": "AAGNoQAAAAkAAAAAIOoKuEk/7P466ZLqLQEZ7a/p/fF0qVbSa68g1MPoYqgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA="
          }
        ]
      }
    ]
  }
}
```

Example of parsed transaction envelope

```json
{
   "tx":{
      "sourceAccount":"GA5C3JAIJDIYPRRTZ5STBUHQXKUHR7RBGVD3T2E2N6HDOH7VDU6AYJOR",
      "salt":1122570949328465936,
      "timeBounds":{
         "maxTime":1533222738
      },
      "memo":{

      },
      "operations":[
         {
            "body":{
               "createAccountOp":{
                  "destination":"GAQOUCVYJE76Z7R25GJOULIBDHW272P56F2KSVWSNOXSBVGD5BRKRTQJ",
                  "recoveryKey":"GCQYVZ2BKL2W24ID2QNPV2FQ4XZ6UQ5LTULNSONLOOLJ3TIZWIHKNMH2",
                  "accountType":{
                     "int":5,
                     "string":"not_verified"
                  },
                  "ext":{

                  }
               }
            }
         }
      ],
      "ext":{

      }
   },
   "signatures":[
      {
         "hint":[
            245,
            29,
            60,
            12
         ],
         "signature":"R2KrbJ2OZ5TIWZL5p095XFm8NXFY8k13X4roBvZpa3kJ7bG2L/pHVneVj0dGtULuvOqJUHl7odWWUzhdACMRBA=="
      }
   ]
}
```

Example of parsed transaction result
```json
{
   "result":{
      "results":[
         {
            "tr":{
               "createAccountResult":{
                  "success":{
                     "externalSystemIDs":[
                        {
                           "accountID":"GAQOUCVYJE76Z7R25GJOULIBDHW272P56F2KSVWSNOXSBVGD5BRKRTQJ",
                           "externalSystemType":{
                              "int":1,
                              "string":"bitcoin"
                           },
                           "data":"1AMY5vAN5MfVqtquQdWRt7DvZVAquYDW7p",
                           "ext":{

                           }
                        },
                        {
                           "accountID":"GAQOUCVYJE76Z7R25GJOULIBDHW272P56F2KSVWSNOXSBVGD5BRKRTQJ",
                           "externalSystemType":{
                              "int":2,
                              "string":"ethereum"
                           },
                           "data":"0xb6061096040a055e8923ed22F51355a15663C8B5",
                           "ext":{

                           }
                        }
                     ],
                     "ext":{

                     }
                  }
               }
            }
         }
      ]
   },
   "ext":{

   }
}
```

Example of parsed transaciton meta
```json
{
   "operations":[
      {
         "changes":[
            {
               "created":{
                  "lastModifiedLedgerSeq":101793,
                  "data":{
                     "type":{
                        "int":16,
                        "string":"external_system_account_id"
                     },
                     "externalSystemAccountID":{
                        "accountID":"GAQOUCVYJE76Z7R25GJOULIBDHW272P56F2KSVWSNOXSBVGD5BRKRTQJ",
                        "externalSystemType":{
                           "int":1,
                           "string":"bitcoin"
                        },
                        "data":"1AMY5vAN5MfVqtquQdWRt7DvZVAquYDW7p",
                        "ext":{

                        }
                     }
                  },
                  "ext":{

                  }
               }
            },
            {
               "created":{
                  "lastModifiedLedgerSeq":101793,
                  "data":{
                     "type":{
                        "int":16,
                        "string":"external_system_account_id"
                     },
                     "externalSystemAccountID":{
                        "accountID":"GAQOUCVYJE76Z7R25GJOULIBDHW272P56F2KSVWSNOXSBVGD5BRKRTQJ",
                        "externalSystemType":{
                           "int":2,
                           "string":"ethereum"
                        },
                        "data":"0xb6061096040a055e8923ed22F51355a15663C8B5",
                        "ext":{

                        }
                     }
                  },
                  "ext":{

                  }
               }
            },
            {
               "created":{
                  "lastModifiedLedgerSeq":101793,
                  "data":{
                     "account":{
                        "accountID":"GAQOUCVYJE76Z7R25GJOULIBDHW272P56F2KSVWSNOXSBVGD5BRKRTQJ",
                        "recoveryID":"GCQYVZ2BKL2W24ID2QNPV2FQ4XZ6UQ5LTULNSONLOOLJ3TIZWIHKNMH2",
                        "thresholds":[
                           1,
                           0,
                           0,
                           0
                        ],
                        "accountType":{
                           "int":5,
                           "string":"not_verified"
                        },
                        "ext":{

                        }
                     }
                  },
                  "ext":{

                  }
               }
            },
            {
               "created":{
                  "lastModifiedLedgerSeq":101793,
                  "data":{
                     "type":{
                        "int":9,
                        "string":"statistics"
                     },
                     "stats":{
                        "accountID":"GAQOUCVYJE76Z7R25GJOULIBDHW272P56F2KSVWSNOXSBVGD5BRKRTQJ",
                        "ext":{

                        }
                     }
                  },
                  "ext":{

                  }
               }
            }
         ]
      }
   ]
}
```
