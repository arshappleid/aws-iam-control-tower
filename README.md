**Author** : Prabhmeet Deol
# AWS Environment Governance with Control Tower and AWS Organizations Setup

## Overview
AWS Control Tower lets managers implement security boundaries for different compliance and management uses. 

### Sample Security Challenges it Solves
1. Ensuring only network engineers can modify networking components. Which ensures networking changes, suddenly do not take down multiple applications it supports.
2. Log Collection Compliance account with restricted access. Compliance requirements such as HIPAA, SOC tend to require organizations to store logs, at upto 7 years at times. Any violation of this could lead to fines upon Audits. Therefore ensures only specific accounts could send logs to specific s3 buckets within accounts, and only very specific accounts could delete them.
3. Audit Accounts - Allows Auditors to view (no modify or delete) all the Compliance Logs for past n Years. 
4. Ensuring Administrators performing patch work cannot delete EC2 Instances, atlthough have SSM access to perform patch work.
5. Seperate prod, dev, and test environments allow developers to Test and demo changes to clients. While allowing them to continuously iterate throughout the SDLC, and test changes before shipping them to production.

### Controlling permissions within Accounts.
Control Tower Makes use of [Service Control Policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_syntax.html), which only limit Access. They cannot grant IAM based permissions. Also called Guardrails. 

#### SCP Examples
**SCP evaluation follows a deny-by-default model**, meaning that any permissions not explicitly allowed in the SCPs are denied. To make sure this is enforced, make sure to have an Explicit Allow All at the root account level.

SCP for the Networking Account. The following policy denies access to every other resource accept Networking Resources (VPC, TGW , Network Firewalls, WAF , ELB). This strategy, allows to easily control access, and should be applied at each account level.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowOnlyNetworkAndIdentity",
      "Effect": "Allow",
      "Action": [
        "ec2:*Vpc*",
        "ec2:*Gateway*",
        "network-firewall:*",
        "iam:*",
        "sts:*",
        "logs:*"
      ],
      "Resource": "*"
    }
  ]
}
```


### How does this compare to Organizational Units

## References
1. [AWS Control Tower Pre-Requisites](https://docs.aws.amazon.com/organizations/latest/userguide/services-that-can-integrate-CTower.html#:~:text=AWS%20Control%20Tower%20applies%20preventive,AWS%20Control%20Tower%20user%20guide.)
2. [SCP Syntax](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_syntax.html)
3. 



