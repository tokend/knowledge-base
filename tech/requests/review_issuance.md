# Review issuance request

This operation allows review `issuance request`.

## Source account details

| Property              | Value                                             |
|-----------------------|---------------------------------------------------|
| Threshold             | `HIGH`                                              |
| Allowed account types | `MASTER`, `SYNDICATE`                             |
| Allowed signer types  | `USER_ISSUANCE_MANAGER`, `SUPER_ISSUANCE_MANAGER` |

## Possible errors

| Error                        | Code | Description                                                              |
|------------------------------|------|--------------------------------------------------------------------------|
| MAX_ISSUANCE_AMOUNT_EXCEEDED | -40  | Sum of issued, pending issuance amount and amoun to be issued more than. |
| FULL_LINE                    | -42  | Receiver can not be funded.                                              |
| SYSTEM_TASKS_NOT_ALLOWED     | -43  | Not allowed set system tasks (1 or 4) to tasksToAdd or to tasksToRemove. |

