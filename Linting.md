# Linting

## Pre-issuance

Before issuing any publicly-trusted leaf certificate, Sectigo performs pre-issuance linting. We require that no errors or warnings are returned by any of the linters in order to proceed to issuance.

For server certificates, we use [Zlint](https://github.com/zmap/zlint), [x509lint](https://github.com/kroeckx/x509lint), and [Cablint](https://github.com/certlint/certlint). When we've already run these linters prior to issuing a CT precertificate, we don't run them again before issuing the corresponding certificate.

For non-server certificates, we use [Certlint](https://github.com/certlint/certlint).

For all OV and EV certificates (both server and non-server), we use [FTFY](https://github.com/rspeer/python-ftfy) to detect [mojibake](https://en.wikipedia.org/wiki/Mojibake) in certification request data supplied by customers. Apart from a curated list of false positives, we treat any FTFY finding as a linter error.

We review the changes introduced in each new linter release, and when any change seems relevant to our operations we make plans to deploy the linter update to our production environment.

We also have many pre-issuance checks implemented in our CA system, some of which we have documented at https://bugzilla.mozilla.org/show_bug.cgi?id=1724476.

## Post-issuance

We built and maintain crt.sh, which performs post-issuance linting using Zlint, x509lint, and cablint, for each newly logged certificate that it ingests from CT logs. Regularly updated summaries are provided at https://crt.sh/?zlint=1+week, https://crt.sh/?x509lint=1+week, and https://crt.sh/?cablint=1+week, enabling any CA to easily detect and start investigating any issues found. Sectigo staff members monitor these pages from time to time.

## Disabling lints

We currently do not disable any lints in any of the linters. In the past we had a short list of linter error/warning messages that we treated as false positives, but we worked with the linter maintainers to either downgrade to informational or remove those lints.

## Contributing

We have contributed to all of the linters, primarily by reporting bugs in existing lints and in most cases submitting pull requests to fix those bugs.

A Sectigo staff member is the current maintainer of Certlint.

## Linting our corpus of certificates

Whenever a linter update is deployed to crt.sh, a process that re-lints existing certificates is restarted. The re-linting occurs in reverse crt.sh ID order. Unfortunately, due to the vast quantity of certificates known to crt.sh and also due to the speed of the linters, the re-linting process rarely completes before the linter is updated again.

On the occasions that we have needed to scan our corpus of certificates as part of responding to an incident, our use of pre-issuance linting has meant that the linters are not the appropriate tool to use. Instead, we have invariably needed to write custom SQL scripts to run on crt.sh and/or our CA database.
