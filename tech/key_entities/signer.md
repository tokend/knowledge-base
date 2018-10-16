# Signers

Signer is the main concept used for authorization. To be able to perform transaction on read private data of particular account from any of the services signature of one of the signers of that account must be provided.

## Signer Fields

* **pubKey** &mdash; public key of the ed25519 key pair. Used as identifier of the signer.
* **weight** &mdash; specifies weight of the signer.
* **signerType**  &mdash; bit mask of signer types; Defines set of operations allowed to be authorized by the signer in name of the account.
* **identity** &mdash; identity of the signer. Two signatures created by two different signers with same identity are handled as one.
* **name** &mdash; name of the signer.

## Signer types

Here is the list of all existing signer types at the moment:

| Name                                    | Value      | Rights     |
|-----------------------------------------|------------|------------|
| READER                                  | 1          | Read data from API and Horizon
| NOT_VERIFIED_ACC_MANAGER                | 2          | Manage `not verified` [accounts][1] and block/unblock `general` accounts
| GENERAL_ACC_MANAGER                     | 4          | Create account, block/unblock `general` accounts, change limits on `general` account
| DIRECT_DEBIT_OPERATOR                   | 8          | Perform direct debit operation
| ASSET_MANAGER                           | 16         | Create [assets][2], create [asset pairs][3] and update policies, set [fees][4]
| ASSET_RATE_MANAGER                      | 32         | Set physical asset price
| BALANCE_MANAGER                         | 64         | Create [balances][5], spend assets from balances
| ISSUANCE_MANAGER                        | 128        | Make preissuance request
| INVOICE_MANAGER                         | 256        | Create payment requests to other accounts
| PAYMENT_OPERATOR                        | 512        | Review payment requests
| LIMITS_MANAGER                          | 1024       | Change limits
| ACCOUNT_MANAGER                         | 2048       | Add/delete signers and trust
| COMMISSION_BALANCE_MANAGER              | 4096       | Spend from commission balances
| OPERATIONAL_BALANCE_MANAGER             | 8192       | Spend from operational balances
| EVENTS_CHECKER                          | 16384      | Check and trigger events
| EXCHANGE_ACC_MANAGER                    | 32768      | Manage exchange account
| SYNDICATE_ACC_MANAGER                   | 65536      | Manage syndicate account
| USER_ASSET_MANAGER                      | 131072     | Review sale, asset creation/update requests
| USER_ISSUANCE_MANAGER                   | 262144     | Review pre-issuance/issuance requests
| WITHDRAW_MANAGER                        | 524288     | Review withdraw requests
| FEES_MANAGER                            | 1048576    | Set fee
| TX_SENDER                               | 2097152    | Send tx
| AML_ALERT_MANAGER                       | 4194304    | Manage AML alert request
| AML_ALERT_REVIEWER                      | 8388608    | Review aml alert requests
| KYC_ACC_MANAGER                         | 16777216   | Manage kyc
| KYC_SUPER_ADMIN                         | 33554432   |
| EXTERNAL_SYSTEM_ACCOUNT_ID_POOL_MANAGER | 67108864   |
| KEY_VALUE_MANAGER                       | 134217728  | Manage keyValue
| SUPER_ISSUANCE_MANAGER                  | 268435456  |
| CONTRACT_MANAGER                        | 536870912  | 
| ATOMIC_SWAP_MANAGER                     | 1073741824 |  

[1]: /tech/key_entities/accounts.md
[2]: /tech/key_entities/asset.md
[3]: /tech/guides/trading.md#asset-pairs
[4]: /tech/operations/set_fees.md
[5]: /tech/operations/manage_balance.md
