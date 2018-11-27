# External Systems Interconnection

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