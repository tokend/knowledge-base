# Reviewable request

The reviewable request is an entity, that is used when the user wants to perform an action, which must be applied by admin.

## Reviewable request fields

Reviewable request has the following fields:

* __Request ID__: id of the request. Request ID is generating in core during the new reviewable request creation.

* __Hash__:  hash of the request body.

* __Requestor__: the account which has created request.

* __Reject reason__: reject reason of the request. If the request is being rejected by admin, reject reason must be specified. Reject reason is an empty string by default.

* __Reviewer__: reviewer of the request.

* __Reference__: (optional) reference for the request which will act as a unique key for the request (will reject the request with the same reference from same requestor).

* __Created at__: when request was created (unix timestamp).

* __Body__: a body of the request depends on the reviewable request type.

* __Tasks ext__

    * __All tasks__: bitmask of all tasks which must be completed for the request approval. The value of this field is being set by user or core during the creation of reviewable request.

    * __Pending tasks__: bitmask of pending tasks which must be completed for the request approval. The value of this field initially equals to __All tasks__. During the request lifetime, it could be changed by reviewers. If __Pending tasks__ equals 0 - request should be approved.

    * __Sequence number__: initially equals 0. The value of this field increases, when the request is being rejected.

    * __External details__: this field is a list of comments added by reviewers during request review.