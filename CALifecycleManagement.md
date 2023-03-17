# CA Lifecycle Management

## Current active CA hierarchy
The following table lists the Root CA certificates we operate that are currently included in the Apple root store.

| Common Name | Expiration Date | Key Size | Trust Purpose | 
|---|---|---|---|
| AAA Certificate Services | 2028-12-31 | 2048 RSA | Multi-Purpose, currently spanning TLS Server Authentication, Client Authentication, Code Signing and Email Protection |
| COMODO Certification Authority | 2030-12-31 | 2048 RSA | Multi-Purpose, currently spanning TLS Server Authentication, Client Authentication, Email Protection |
| COMODO ECC Certification Authority | 2038-01-18 | 384 ECC | Multi-Purpose for ECC certificates, currently spanning TLS Server Authentication, Client Authentication, Email Protection | 
| COMODO RSA Certification Authority | 2038-01-18 | 4096 RSA | Multi-Purpose for RSA certificates, currently spanning TLS Server Authentication, Client Authentication, Code Signing and Email Protection | 
| USERTrust ECC Certification Authority | 2038-01-18 | 384 ECC | Multi-Purpose for ECC certificates, currently spanning TLS Server Authentication, Client Authentication, Code Signing and Email Protection. Currently our main Root CA for ECC certificates | 
| USERTrust RSA Certification Authority | 2038-01-18 | 4096 RSA | Multi-Purpose for RSA certificates, currently spanning TLS Server Authentication, Client Authentication, Code Signing, Document Signing and Email Protection. Currently our main Root CA for RSA certificates | 

In summary there are currently 6 Root CA certificates in active operation. All of these are multi-purpose with the last ones set to expire on 2038-01-18.

## Planned CA hierarchy (in progress)
The following table lists our next generation of Root CA certificates, each of which is intended to enable a single-purpose hierarchy.

| Common Name | Expiration Date | Key Size | Signing Algorithm | Trust Purpose | 
|---|---|---|---|---|
| Sectigo Public Server Authentication Root E46 | 2046-03-21 | 384 ECC | ecdsa-with-SHA384 | TLS Server Authentication for ECC certificates |
| Sectigo Public Server Authentication Root R46 | 2046-03-21 | 4096 RSA | sha384WithRSAEncryption | TLS Server Authentication for RSA certificates |
| Sectigo Public Email Authentication Root E46 | 2046-03-21 | 384 ECC | ecdsa-with-SHA384 | Email Protection for ECC certificates |
| Sectigo Public Email Authentication Root R46 | 2046-03-21 | 4096 RSA | sha384WithRSAEncryption | Email Protection for RSA certificates |
| Sectigo Public Code Signing Root E46 | 2046-03-21 | 384 ECC | ecdsa-with-SHA384 | Code Signing for ECC certificates |
| Sectigo Public Code Signing Root R46 | 2046-03-21 | 4096 RSA | sha384WithRSAEncryption | Code Signing for RSA certificates |
| Sectigo Public Time Stamping Root E46 | 2046-03-21 | 384 ECC | ecdsa-with-SHA384 | Time Stamping for ECC certificates |
| Sectigo Public Time Stamping Root R46 | 2046-03-21 | 4096 RSA | sha384WithRSAEncryption | Time Stamping for RSA certificates |
| Sectigo Public Document Signing Root E46 | 2046-03-21 | 384 ECC | ecdsa-with-SHA384 | Document Signing for ECC certificates |
| Sectigo Public Document Signing Root R46 | 2046-03-21 | 4096 RSA | sha384WithRSAEncryption | Document Signing for RSA certificates |

## Key size and algorithm support

| Common Name | Intended Subordinate CA Signing Algorithm | Intended Subordinate CA Key Size and Curve | Intended Leaf Certificate Signing Algorithm | Intended Leaf Certificate Key Size and Curve |
| --- | --- | --- | --- | --- |
| Sectigo Public Server Authentication Root E46 | ecdsa-with-SHA384 | P-256 or P-384 | ecdsa-with-SHA256 OR ecdsa-with-SHA384| P-256 OR P-384 |
| Sectigo Public Server Authentication Root R46 | sha384WithRSAEncryption | RSA 3072 or RSA 4096 | sha256WithRSAEncryption OR sha384WithRSAEncryption | At least RSA 2048 and a multiple of 8 |
| Sectigo Public Email Authentication Root E46 | ecdsa-with-SHA384 | P-256 or P-384 | ecdsa-with-SHA256 OR ecdsa-with-SHA384 | P-256 OR P-384 |
| Sectigo Public Email Authentication Root R46 | sha384WithRSAEncryption | RSA 3072 or RSA 4096 | sha256WithRSAEncryption OR sha384WithRSAEncryption | At least RSA 2048 and a multiple of 8 |
| Sectigo Public Code Signing Root E46 | ecdsa-with-SHA384 | P-256 or P-384 | ecdsa-with-SHA256 OR ecdsa-with-SHA384 | P-256 OR P-384 |
| Sectigo Public Code Signing Root R46 | sha384WithRSAEncryption | RSA 3072 or RSA 4096 | sha256WithRSAEncryption OR sha384WithRSAEncryption | At least RSA 3072 and a multiple of 8 |
| Sectigo Public Time Stamping Root E46 | ecdsa-with-SHA384 | P-256 or P-384 | ecdsa-with-SHA256 OR ecdsa-with-SHA384 | P-256 OR P-384 |
| Sectigo Public Time Stamping Root R46 | sha384WithRSAEncryption | RSA 3072 or RSA 4096 | sha256WithRSAEncryption OR sha384WithRSAEncryption | At least RSA 3072 and a multiple of 8 |
| Sectigo Public Document Signing Root E46 | ecdsa-with-SHA384 | P-384 | ecdsa-with-SHA256 OR ecdsa-with-SHA384 | P-256 OR P-384 |
| Sectigo Public Document Signing Root R46 | sha384WithRSAEncryption | RSA 4096 | sha256WithRSAEncryption OR sha384WithRSAEncryption | At least RSA 2048 and a multiple of 8 |

**Note:** For the issuance of SubCAs and Leaf Certificates, Sectigo will at a minimum follow recommendations and requirements from NIST, the CA/B Forum Guidelines as well as requirements imposed by Root Store Operators.  
**Note 2:** While we intend to only issue ECC based certificates under our \*E46 Root CAs and only RSA based certificates under our \*R46 Root CAs, we may choose to deviate from this if we are presented with any customer use cases for hybrid certificate chains.

## Root CA Lifecycle management and planning
Historically we have planned to create a new generation of Root CA certificates approximately 7 to 10 years before the expiry dates of the previous generation. However, in practice over the last couple of decades, external factors (such as shifts in industry requirements relating to key sizes and signature algorithms) have resulted in us typically creating new generations of Root CA certificates ahead of that planned schedule.

The [desire](https://wiki.mozilla.org/CA/Required_or_Recommended_Practices#CA_Hierarchy) from multiple root store operators to transition into single purpose hierarchies, tasked us to look at, issue and start the root inclusion process for our new generation of Root CA certificates.
While this was ongoing, Mozilla also announced the [draft idea](https://wiki.mozilla.org/CA/Root_CA_Lifecycles) for shortening Root CA lifecycles. Due to this change, we expect to see the Websites Trust Bit for at least one of our existing Root CA certificates be removed from the Mozilla root store on April 15, 2025, with all our other Root CA certificates following, with all our other Root CA certificates similarly affected between then and 2028"

The removal of trust bits does not automatically mean we will stop issuing certificates using the hierarchy in question. We have customers in several areas around the world that require the use of legacy Root CA certificates. While we do talk to and push these customers to move to more modern systems, we cannot in every case change their requirements. 

In order to provide a seamless transition for Subscribers, we plan on cross-signing our new generation of Root CA certificates from one or more of our legacy Root CAs. This is something we have started by cross-signing both single-purpose Code Signing CA certificates. The validity of this cross-signed CA is less than 6 years. 

We have not yet set a date for cross-signing any of the single-purpose Root CA certificates that we have requested to be included in the Apple root store, but expect to do so before the end of 2023. We believe this will lead to a more seamless transition for Subscribers. As such it is our expectation to start issuing the first TLS Server certificates using our new generation before the end of 2023. For Code Signing, we have already started active issuance.

With the new plans for Root CA lifecycles from both Mozilla and Google, we expect to be performing new root submissions every 5 years going forward, potentially earlier, depending on the final lifecycle expectations set by all Root Store operators.

We do not have any concrete plan on when we will ask for our multi-purpose hierarchies to be removed from the Apple Root Store. This will depend on several factors including expected timeline for Root CA inclusion across several Root Store operators as well as requirements from Subscribers who, while we push for them to become more agile, also have use-cases in which they depend on our legacy multi-purpose Roots. 
