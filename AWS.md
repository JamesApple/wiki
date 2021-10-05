* [AWS services in plain english](https://www.web3us.com/how-guides/amazon-web-services-plain-english)

# IAM
Account Root User (Principal) -> Creates IAM Entities

# Regions, AZs and Edge Infrastructure

Regions: us-east-1, us-west-1, ap-southeast-1
Availability Zones: us-east1a, us-east1b, us-east1c

Availability zones are typically fault isolaters
Regions are typically governance or security based

VPC's map to regions. Subnets map to AZ's

# HA, FT, DR

## High Availability

Active-Down Configuration

A replaceable tire on the back of a 4x4. You have to get out and fix the tire
but you can carry on.

Solutions that are quickly recoverable. Does not mean there will be no pause in
service. 

Place instances in an austoscaling group that spans AZ's

## Fault Tolerant

Multiple Redundancies

Active-Active configuration

Needs to survive a drop in service.

4x engines on a jet that needs 2x engines to operate.

For business cases where no gap in service can occur

## Disaster Recovery

Ejector seats in a plane.

```
- Backup
|
|
| RPO
|
- DISASTER
|
|
| RTO
|
- Restoration
```

### RPO Recovery Point Objective

Time between when a disaster occurs and the last recoverable copy of key data was created
24 hours for keyswipe would be acceptable
10 minutes for sales would be acceptable

### RTO Recovery Time Objective

Time for normal operations to be resumed.

# Storage Systems

## Ephemeral

Data that is local to a resource and lost when that resource goes down

### Amazon Elasticache

Redis

### Instance Store Volume

Fastest store possible in IOPS

## Transient

Data that exists until it is passed between sources

### SQS

## Persistent

### EBS Elastic Block Storage

Network file system that is persisted. Data on block will last between boots
Used to store database volumes

Never included by default. Must be configured

### Amazon EFS

# IAM
* Users
* Groups
* Roles
* IAM Policies
* Authentication Attributes (MFA, SSH keys for code commit)

> Groups are not a real identity. They are a way of applying policies to
> multiple users

DENY always overrides an ALLOW

## Inline Policy
JSON attached directly to a user / group
## Attached Policy
Self managed policy objects

## Credential report
Details about all IAM users that have ever existed


## Password Policies
You can configure length, expiry and more 

By default all new accounts have no permission policies attached

All permission policies have `Implicit Deny`

```sh
aws configure # Type in access and secret
```

IAM provides some identity services but can be configured to provide identity
through other means such as active directory

