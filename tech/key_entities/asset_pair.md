# Asset pair

Asset pair has such properties:

## Base, quote and price

Every asset pair contains 3 key fields: `base_asset`, `quote_asset` and 
`price`. This data will be used to perform conversions between assets. 
The formula for such calculations is:

```javascript
price = quote/base
```

## Policies

*Asset pair policies* it the properties of asset that define it's behaviour. 
You can think of them as a configuration of the pair. At this time TokenD 
assets may have such policies:

| Name                        | Description |
|-----------------------------|-------------|
| TRADEABLE_SECONDARY_MARKET  | If set, it's allowed to trade asset pair on the secondary market |
