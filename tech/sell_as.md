# Manage Atomic Swap Sell Request

This operation allows to create or cancel `ASSellRequest`.

## Source account details

| Property              | Value                                        |
|-----------------------|----------------------------------------------|
| Threshold             | high                                         |
| Allowed account types | `MASTER`, `SYNDICATE`, `GENERAL`, `NOT_VERIFIED`                        |
| Allowed signer types  | `BALANCE_MANAGER` |

## Parameters

| Parameter |       Type      |                              Description                              |
|:---------:|:---------------:|:---------------------------------------------------------------------:|
|  requestID  | UINT64 |                   Identifier of the request. `0` to create new one                   |
| action |     `CREATE`, `CANCEL`    | Action to be performed against request               |
|  createBody |     *CreateASSellRequest    | Body of the request for creation. Can only be set with `CREATE` action |

### `CreateASSellRequest` fields

| Parameter |       Type      |                              Description                              |
|:---------:|:---------------:|:---------------------------------------------------------------------:|
| amount |    UINT64    | Amount of tokens to be sold               |
|  balanceID |     BalanceID    | ID of the balance from which corresponding amount of tokens will be charged |
|  quoteAsset |     AssetCode    | Unique identifier of the token to be brought |
|  price |     UINT64    | Price per one base token in terms of quote |
|  details |     longstring    | JSON object with max length of 1000 characters. Can be used to pass additional details like unique identifier of seller in external system |

## Possible errors

| Error                       | Code | Description                                                                              |
|-----------------------------|------|------------------------------------------------------------------------------------------|
| INVALID_AMOUNT              |  -1  | Amount in request body is 0                                                              |
| BALANCE_NOT_FOUND       |  -2  | Specified balance not found or sender of request is not owner of the balance                         |
| UNDERFUNDED             |  -3  | not enough funds                                    |
| INVALID_PRICE              |  -4  | Price is 0                                                              |
| REQUEST_NOT_FOUND              |  -5  | Request with specified ID does not exists (only for `CANCEL` action)                                                            |
| MALFORMED              |  -6  | Operation is invalid in some way                                                      |
| PAIR_NOT_TRADED              |  -7  | Atomic swaps for specified asset pair is not allowed                                        |

## Successful result

Successful result has the following fields:

* __Request ID__: id of the request generated in core.