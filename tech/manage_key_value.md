# Manage Key Value

The main idea of `ManageKeyValueOp` is to allow to pass through blockchain values. It can be used to configure core logic or to configure auxiliary modules.

## Reserved keys

|                            Key structure                           | Value type | Notes                                                                                                                                                                                                               |
|:------------------------------------------------------------------:|:----------:|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              `ext_sys_exp_period:<external_system_id>`             |  `UINT32`  | Specifies expiration period of external system account ID in seconds. **Warning:** core does not validate type of this entry; if invalid type will be specified, one day will be used as default expiration period. |
| `kyc_lvlup_rules:<current_account_type>:0:<account_type_to_set>:0` |  `UINT32`  | Used to specify default tasks for updating account from `current_account_type` to `account_type_to_set`.                                                                                                            |
|                   `issuance_tasks: <asset_code>`                   |  `UINT32`  | Used to specify default tasks for asset issuance.                                                                                                                                                                    |
|                   `atomic_swap_exp_period`                   |  `UINT32`  | Used to specify expiration period of atomic swap request                                                                                                                                                                  |