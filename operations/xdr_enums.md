XDR, also known as External Data Representation, is used throughout the TokenD network. 
The ledger, transactions, results, history, and even the messages passed between computers running 
TokenD-core are encoded using XDR.

XDR is specified in RFC 4506 and is similar to tools like Protocol Buffers or Thrift. XDR provides a 
few important features:

* It is very compact, so it can be transmitted quickly and stored with minimal disk space.
* Data encoded in XDR is reliably and predictably stored. Fields are always in the same order, 
which makes cryptographically signing and verifying XDR messages simple.
* XDR definitions include rich descriptions of data types and structures, which is not possible 
in simpler formats like JSON, TOML, or YAML.
* Since XDR is a binary format and not as widely known as simpler formats like JSON, the TokenD 
SDKs all include tools for parsing XDR and will do so automatically when retrieving data.

In addition, the TokenD API server generally exposes the most important parts of the XDR data in JSON, so 
they are easier to parse if you are not using an SDK. The XDR data is still included (encoded as a base64 string) 
inside the JSON in case you need direct access to it.

In most cases, when building client-side part of an application, you'll need only the values of [XDR enums][2] 
that are exposed by javascript SDK's.

[1]: https://tools.ietf.org/html/rfc4506.html
[2]: /coming_soon.md
[3]: /coming_soon.md