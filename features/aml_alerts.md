# AML alerts

AML alert is a special operation that takes care of the situations when your user have got their cost in malicious way.
It may be caused by software bug, when costs were issued unexpectedly, or legal reasons so that user is not 
allowed to hold some tokens in his account. It completely destroys costs not only from the provided user’s balance but 
from the system itself

Consider such example

- There is 100,000 issued QTK tokens in the system
- Our user has on his balance 10 QTK - 5 valid QTK and 5 QTK that he received after some malicious actions
- We created and approved AML alert on 5 QTK for this user
- User now has only 5 QTK left on his balance
- There is now 99,995 QTK tokens issued in the system (edited)

To create a new AML alert, you need to specify such details:

- `amount` - the amount to charge from selected balance
- `reference` - any random string, but it should be unique for each request
- `reason` - reason of alerting

This action can be performed by admins with policy `AML alert manager`

At this step you've just created the request. Now any admin with policy `AML alert reviewer` can approve it or reject it 
(with providing the reason of rejection).
