# Links

[IRAP Reference Architecture](https://github.com/aws-quickstart/quickstart-compliance-irap-protected)
[SSAM webinar](https://www.youtube.com/watch?v=uP0SvWaARUY)
[SSAM standard](https://www.dspanz.org/industry-standards/addon-security-standard/ABSIA-Security-Standard-for-Add-on-Marketplaces.pdf)
[SSAM guidelines](https://www.dspanz.org/industry-standards/addon-security-standard/)

# Definitions

## DPO - Digital Partnership Office

## DSP - Digital Service Provider

Company that directly integrates with the ATO

## SSAM - Security Standard for Add-on Marketplaces

Requirements set forth by the ATO for addons to DSP applications.
Applies to any addon with more than 1,000 DSP connections or that connects to a tax practitioner.

Any software that stores TFN's _may_ be required to follow the operational framework.

# SSAM Compliance

SSAM self certification is provided to the DSP which then provides their own certification to the ATO.

## Violation

Violation of SSAM compliance results in 30 days notice to provide a fix plan and 60 days to implement.

## Requirements

### Encryption Key Management

#### Requirement

Relates specifically to OAuth tokens.

1. Do not display tokens to the end user
2. Store refresh tokens in persistent memory
3. Encrypt the access key
4. Store the encryption key in a file

#### Solution

1. Keys are kept inside SSE RDS
2. User keys are not exposed to end users

### Encryption in Transit

#### Requirement

1. App server uses TLSV1.1 or higher
2.

#### Solution

1. API Gateway mandatory HTTPS 1.2+

### Authentication

#### Requirement

1. Require strong password
2. Implement 2FA or use DSP SSO

#### Solution

### Indirect access to data

#### Requirement

1. Make clear where data is shared to users
2. Justify sending data internally

#### Solution

### App server configuration

#### Requirement

1. NIST general server security

#### Solution

### Vulnerability Management

#### Requirement

1. Follow OWASP top 10

#### Solution

### Encryption at Rest

#### Requirement

1. Data repositories must be encrypted at rest

#### Solution

### Audit Logging

#### Requirement

1. Must include both application and event level
2. Must include following where applicable

- Date and time of event
- Relevant user or process
- Event description
- Success or failure
- Event source

3. Logs must be retained for a minimum of 12 months
4. Logs must be immutable and secure

#### Solution

### Data hosting

#### Requirement

1. Store data in australia
2. (weaker) Do not store data in north korea, china etc.

#### Solution

### Monitoring and breach reporting

#### Requirement

1. Demonstrate you scan for threats
2. Monitoring at network/infrastructure/application level
3. Detected anomalies must be reported to DSP

#### Solution

1. Implement Amazon Guard Duty
