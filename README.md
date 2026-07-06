# HashiCorpQ Platforms

Shared, customer-agnostic infrastructure modules for all HashiCorpQ customer deployments.
Customer repositories (`HashiCorpQ/<customer>`) consume these modules; nothing deploys
directly from this repository.

## Structure
- `aws/modules/` — CloudFormation modules, one directory per AWS technology domain (Phase 1)
- `hashicorp/` — Terraform modules (Phase 2)
- `azure/bicep/`, `azure/arm/` — Azure modules (future)

## Module contract
Every AWS module accepts the standard parameters `NamePrefix`, `Environment`,
`CostCenter`, `DataClassification`, `ComplianceProfile` and exports under the
`${NamePrefix}-*` namespace. Stacks are named `<customer>-<program>-<module>`.
All ARNs use `${AWS::Partition}` — never hardcoded `arn:aws:` — for GovCloud portability.

## Status
| Module | Status |
|---|---|
| networking | v1.0 — generalized from the proven iQ deployment (see nQ runbook) |
| compute | planned (ECS cluster, instance roles, auto scaling) |
| storage | planned |
| database | planned |
| security | planned (KMS, Config, Security Hub) |
| governance | planned (CloudTrail, budgets, tag enforcement) |
| ml-ai | planned |
