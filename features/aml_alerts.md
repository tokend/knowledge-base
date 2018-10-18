# AML alerts

AML alert is a special operation that deals with situations when your a user received tokens in an unexpected way.
It may be caused by software bug, when tokens were issued unexpectedly, or legal reasons - user is not 
allowed to hold certain tokens on their account. It completely destroys tokens not only from the balance of a corresponding user but from the system itself (similar to withdrawal). 

Consider such example

* There are 100,000 issued QTK tokens in the system.
* The user has 10 QTK on their balance. Five of them are valid and 5 – user received after performing certain malicious actions.
* Admin created and approved the AML alert on 5 QTK of the user.
* These particular tokens are locked (destroyed).
* User has only 5 QTK left on their balance.
* There is now 99,995 QTK tokens issued in the system.

To create a new AML alert, you need to specify the following details:

* `amount` - the amount of tokens to charge from the selected balance
* `reference` - any random string (it should be unique for each request)
* `reason` - reason of alert

This action can be performed by admins with the `AML alert manager` policy.

Any admin with the `AML alert reviewer` policy will later approve/reject this action (and provide the reject reason).
