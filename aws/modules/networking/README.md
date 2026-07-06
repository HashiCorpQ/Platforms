# networking module v1.0

3-AZ VPC foundation: sixteen /20 subnets derived from a /16 via Fn::Cidr
(web/app/db/reserved tiers x3 AZs + AI training/inference pairs), stateless-correct
public and private NACLs, S3 Gateway Endpoint, VPC Flow Logs, and four tier
security groups. Pure networking domain — compute, instance IAM, and budgets
live in their own modules.

## Deploy (example: webIQ pilot)
```
aws cloudformation deploy \
  --stack-name webiq-core-networking \
  --template-file template.yaml \
  --capabilities CAPABILITY_IAM \
  --parameter-overrides NamePrefix=webiq-core VpcCidr=10.16.0.0/16 Environment=dev \
  --no-fail-on-empty-changeset
```

## Key parameters
`NamePrefix` (drives all names and exports), `VpcCidr` (/16 required),
`AllowedSshCidr`, `ComplianceProfile` (commercial|government),
`FlowLogRetentionDays` (90+ for government profiles).

## Exports
`${NamePrefix}-vpc`, `-vpc-cidr`, `-rtb-web`, `-rtb-private`,
`-subnet-{web|app|db|reserved}-{a|b|c}`, `-subnet-ai-{training|inference}-{a|b}`,
`-sg-{bastion|web|db|ai}`.

## Inherited fixes (see nQ deployment runbook)
Public NACL egress includes ephemeral 1024-65535 and UDP 53 (stateless return
traffic). Private NACL ingress allows ephemeral from 0.0.0.0/0 (S3 Gateway
Endpoint responses arrive from S3 public ranges). No explicit ECS
service-linked role anywhere.
