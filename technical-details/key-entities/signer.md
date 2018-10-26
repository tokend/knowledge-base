# Signer

Signer is the main concept used for authorization. To be able to perform transaction on read private data of particular account from any of the services signature of one of the signers of that account must be provided.

## Signer Fields

* **pubKey** — public key of the ed25519 key pair. Used as identifier of the signer.
* **weight** — specifies weight of the signer.
* **signerType**  — bit mask of signer types; Defines set of operations allowed to be authorized by the signer in name of the account.
* **identity** — identity of the signer. Two signatures created by two different signers with same identity are handled as one.
* **name** — name of the signer.

## Signer types

Here is the list of all existing signer types at the moment:

| Name | Value | Rights |
| :--- | :--- | :--- |
| READER | 1 | Read data from API and Horizon |
| NOT\_VERIFIED\_ACC\_MANAGER | 2 | Manage `not verified` [accounts](accounts.md) and block/unblock `general` accounts |
| GENERAL\_ACC\_MANAGER | 4 | Create account, block/unblock `general` accounts |
| ASSET\_MANAGER | 16 | Create [assets](asset.md), create [asset pairs](../../guides/trading.md#asset-pairs) and update policies |
| ASSET\_RATE\_MANAGER | 32 | Set physical asset price |
| BALANCE\_MANAGER | 64 | Create [balances](../operations/manage_balance.md), spend assets from balances |
| ISSUANCE\_MANAGER | 128 | Create preissuance request |
| INVOICE\_MANAGER | 256 | Create and review invoice requests to other accounts |
| LIMITS\_MANAGER | 1024 | Change limits |
| ACCOUNT\_MANAGER | 2048 | Add/delete signers and trust |
| COMMISSION\_BALANCE\_MANAGER | 4096 | Spend from `commission` balances |
| OPERATIONAL\_BALANCE\_MANAGER | 8192 | Spend from `operational` balances |
| EVENTS\_CHECKER | 16384 | Check and trigger events |
| EXCHANGE\_ACC\_MANAGER | 32768 | Manage `exchange` account |
| SYNDICATE\_ACC\_MANAGER | 65536 | Manage `syndicate` account |
| USER\_ASSET\_MANAGER | 131072 | Review sale, asset creation/update requests |
| USER\_ISSUANCE\_MANAGER | 262144 | Review pre-issuance/issuance requests |
| WITHDRAW\_MANAGER | 524288 | Review withdraw requests |
| FEES\_MANAGER | 1048576 | Manage custom fees imposition rules |
| TX\_SENDER | 2097152 | Send tx |
| AML\_ALERT\_MANAGER | 4194304 | Manage AML alert requests |
| AML\_ALERT\_REVIEWER | 8388608 | Review aml alert requests |
| KYC\_ACC\_MANAGER | 16777216 | Manage kyc |
| KYC\_SUPER\_ADMIN | 33554432 | Create kyc requests with tasks and kyc requests for other users, manage kyc rule key value, review kyc requests create/update request |
| EXTERNAL\_SYSTEM\_ACCOUNT\_ID\_POOL\_MANAGER | 67108864 | Manage external system pools |
| KEY\_VALUE\_MANAGER | 134217728 | Manage keyValue |
| SUPER\_ISSUANCE\_MANAGER | 268435456 | Review issuance/pre-issuance requests |
| CONTRACT\_MANAGER | 536870912 | Create and review contract requests, manage contracts |
| ATOMIC\_SWAP\_MANAGER | 1073741824 | Create and review atomic swap bid requests, create and review atomic swap requests, cancel atomic swap bids |

