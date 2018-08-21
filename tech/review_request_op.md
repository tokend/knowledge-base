# Review Request

This operation allows to approve, reject or permanently reject [ReviewableRequest](/tech/reviewable_request.md).

## Parameters

| Parameter |       Type      |                              Description                              |
|:---------:|:---------------:|:---------------------------------------------------------------------:|
|  requestID  | UINT64 |                  ID of request to be reviewed                  |
| requestHash |    Hash    | Hash of the request to be review|
|  action |     ReviewRequestOpAction: `APPROVE`, `REJECT`, `PERMANENT_REJECT`    | Action to be performed |
|  reason |     longstring    | Reject reason. Must be specified for `REJECT`, `PERMANENT_REJECT` |

## Possible errors

| Error                       | Code | Description                                                                              |
|-----------------------------|------|------------------------------------------------------------------------------------------|
|INVALID_REASON | -1|         reason must be empty if approving and not empty if rejecting|
|INVALID_ACTION | -2||
|HASH_MISMATCHED | -3||
|NOT_FOUND | -4||
|TYPE_MISMATCHED | -5||
|REJECT_NOT_ALLOWED | -6|  reject not allowed| use permanent reject|
|INVALID_EXTERNAL_DETAILS | -7||
|REQUESTOR_IS_BLOCKED | -8||
|PERMANENT_REJECT_NOT_ALLOWED | -9|  permanent reject not allowed| use reject|
|ASSET_ALREADY_EXISTS | -20||
|ASSET_DOES_NOT_EXISTS | -21||
|MAX_ISSUANCE_AMOUNT_EXCEEDED | -40||
|INSUFFICIENT_AVAILABLE_FOR_ISSUANCE_AMOUNT | -41||
|FULL_LINE | -42|  can't fund balance - total funds exceed UINT64_MAX|
|SYSTEM_TASKS_NOT_ALLOWED | -43||
|Sale creation requests|
|BASE_ASSET_DOES_NOT_EXISTS | -50||
|HARD_CAP_WILL_EXCEED_MAX_ISSUANCE | -51||
|INSUFFICIENT_PREISSUED_FOR_HARD_CAP | -52||
|NON_ZERO_TASKS_TO_REMOVE_NOT_ALLOWED | -60||
|SALE_NOT_FOUND | -70||
|INVALID_SALE_STATE | -80|  sale state must be "PROMOTION"|
|INVALID_SALE_NEW_END_TIME | -90|  new end time is before start time or current ledger close time|
|AMOUNT_MISMATCHED | -101|  amount does not match|
DESTINATION_BALANCE_MISMATCHED | -102|  invoice balance and payment balance do not match|
NOT_ALLOWED_ACCOUNT_DESTINATION | -103||
REQUIRED_SOURCE_PAY_FOR_DESTINATION | -104|  not allowed shift fee responsibility to destination|
 SOURCE_BALANCE_MISMATCHED | -105|  source balance must match invoice sender account|
 CONTRACT_NOT_FOUND | -106||
 INVOICE_RECEIVER_BALANCE_LOCK_AMOUNT_OVERFLOW | -107||
INVOICE_ALREADY_APPROVED | -108||
PAYMENT_V2_MALFORMED | -110|  bad input| requestID must be > 0|
UNDERFUNDED | -111|  not enough funds in source account|
LINE_FULL | -112|  destination would go above their limit|
DESTINATION_BALANCE_NOT_FOUND | -113||
BALANCE_ASSETS_MISMATCHED | -114||
SRC_BALANCE_NOT_FOUND | -115|  source balance not found|
REFERENCE_DUPLICATION | -116||
STATS_OVERFLOW | -117||
LIMITS_EXCEEDED | -118||
NOT_ALLOWED_BY_ASSET_POLICY | -119||
INVALID_DESTINATION_FEE | -120||
INVALID_DESTINATION_FEE_ASSET | -121|  destination fee asset must be the same as source balance asset|
FEE_ASSET_MISMATCHED | -122||
INSUFFICIENT_FEE_AMOUNT | -123||
BALANCE_TO_CHARGE_FEE_FROM_NOT_FOUND | -124||
PAYMENT_AMOUNT_IS_LESS_THAN_DEST_FEE | -125||
DESTINATION_ACCOUNT_NOT_FOUND | -126