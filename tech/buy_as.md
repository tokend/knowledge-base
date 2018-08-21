# Create Atomic Swap Buy Request

This operation allows to create `ASBuyRequest`.

## Source account details

| Property              | Value                                        |
|-----------------------|----------------------------------------------|
| Threshold             | high                                         |
| Allowed account types | `MASTER`, `SYNDICATE`, `GENERAL`, `NOT_VERIFIED`                        |
| Allowed signer types  | `BALANCE_MANAGER` |

## Parameters

| Parameter |       Type      |                              Description                              |
|:---------:|:---------------:|:---------------------------------------------------------------------:|
|  requestID  | UINT64 |                   Identifier of the request from which base asset will be purchased                   |
| amount |    UINT64    | Amount of tokens to be brought               |
|  balanceID |     BalanceID    | ID of the balance to which brought tokens will be transferred |

## Possible errors

| Error                       | Code | Description                                                                              |
|-----------------------------|------|------------------------------------------------------------------------------------------|
| INVALID_AMOUNT              |  -1  | Amount in request body is 0                                                              |
| BALANCE_NOT_FOUND       |  -2  | Specified balance not found or sender of request is not owner of the balance                         |
| SWAP_REQUEST_NOT_FOUND             |  -3  | Swap request does not exist                                    |
| MALFORMED              |  -4  | Operation is invalid in some way                                                      |
| PAIR_NOT_TRADED              |  -5  | Atomic swaps for specified asset pair is not allowed                                        |

## Successful result

Successful result has the following fields:

* __Request ID__: id of the request generated in core.