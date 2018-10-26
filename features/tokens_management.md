# Advanced token management

TokenD system can be used to track, hold, and transfer any type of asset: 
dollars, euros, bitcoin, stocks, gold, and other tokens of value. Any asset on 
the network can be traded and exchanged with any other. What is more, TokenD 
supports system tokens created by the admin of the system and user tokens 
created by trusted users. By trusted we mean that such user token passed the KYC
verification procedure and is considered verified.

TokenD system provides easy methods to issue tokens, which can be used to 
distributed the token or, in case of integration with external system token, 
manually or automatically process deposit operations. To be able to perform 
issuance of certain amount of tokens `available_for_issuance` must be equal or 
greater then that amount.

To ensure high level of security TokenD allows tokens issuers to easily 
manage amount of tokens available for issuance in the system.

So, to summarize, with TokenD your users will be able to do such actions with tokens:

* Issue and distribute tokens within a few API calls.
* Easily manage risks by using offline application for pre-authorization of a 
certain amount of tokens to be issued.
* Interconnect your token with the tokens from external systems like ERC20 
tokens in the Ethereum Network or Stellar Tokens.
* Define and change the properties of the token in a few API calls: 
    * should the token be fungible or non-fungible;
    * should a user initially complete KYC process before holding the tokens;
    * should the token be transferable or tradeable;
    * should the token be divisible or indivisible. 
* Integrate your existing system using REST API to process withdrawal or 
redemption requests.
* Define the limits on holdings and transfers to be compliant with regulations.
* Define the fee rules for the specific account and account type for various 
range of operations.
* Securely exchange tokens with assets from external systems using atomic swaps.
