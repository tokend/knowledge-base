# Review \(Issuance\)

This operation allows review `issuance request`.

## Source account details

| Property | Value |
| :--- | :--- |
| Threshold | `HIGH` |
| Allowed account types | `MASTER`, `SYNDICATE` |
| Allowed signer types | `USER_ISSUANCE_MANAGER`, `SUPER_ISSUANCE_MANAGER` |

## Possible errors

| Error | Code | Description |
| :--- | :--- | :--- |
| MAX\_ISSUANCE\_AMOUNT\_EXCEEDED | -40 | Sum of issued, pending issuance amount and amoun to be issued more than. |
| FULL\_LINE | -42 | Receiver can not be funded. |
| SYSTEM\_TASKS\_NOT\_ALLOWED | -43 | Not allowed set system tasks \(1 or 4\) to tasksToAdd or to tasksToRemove. |

