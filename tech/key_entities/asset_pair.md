# Asset pair

Asset pair has the following properties:

## Base, quote, and price

Every asset pair contains 3 key fields: `base_asset`, `quote_asset`, and 
`price`. This data will be used to perform exchanges between assets. 
The formula for such calculations is:

```javascript
price = quote/base
```

## Policies

*Asset pair policies* are the properties of the pair that define its behaviour. 
It is a kind of configuration of the pair. Currently, TokenD 
asset pairs have the following properties:

| Name                        | Description |
|-----------------------------|-------------|
| TRADEABLE_SECONDARY_MARKET  | If set, a particular asset pair can be traded on the secondary market |
