# Atomic Swaps

One of the features of TokenD it that users can easily exchange tokens issued 
in the TokenD system with any other asset managed in external system. Such 
approach is highly useful for use cases where deposit of external system assets 
are not desirable due to legal limitations or UX requirements. 

Flow described 
in this document requires intermediate trusted entity, which will send 
confirmations of transfer in external system to the TokenD system.

1. Token issuer creates request to sell his tokens.
(see [ManageASSellRequest](/tech/sell_as.md)). 
He provides amount, token, price and identifier in external system. Amount of 
tokens specified in the request is locked on the issuer's balance. The 
issuer is able to cancel this request at any moment.
2. Another user willing to purchase token and creates request specifying amount 
of tokens to be brought (see using [CreateASBuyRequest](/tech/buy_as.md)). 
The corresponding amount of tokens is locked (locking period can be specified 
by admin of the system). Unique identifier is set for the request, which can be 
used to link transfer in external system with request in TokenD.
3. User performs transfer in external system specifying unique identifier of 
the request. (For systems, which use acquiring provider this step can be 
performed without need to provide user with ID of the request).
4. Intermediate system monitors requests from TokenD system 
using [/request](https://tokend.gitlab.io/horizon-docs/#reviewable-requests).
In case of corresponding transfer in external system, this module approves 
the request and corresponding amount of tokens is transferred to the user 
via [ReviewRequestOp](/tech/review_request_op.md). This module is also 
responsible for rejections of the request in case of expiration.
5. User is able to see purchased token in her account.
