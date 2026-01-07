# Contributing

Thank you for your interest in contributing to this project.

This repository provides a **reference implementation** for tracking Azure Hybrid Benefit usage and Software Assurance entitlement consumption. Contributions are welcome, provided they align with the principles below.

## Guiding principles

Before contributing, please ensure that changes:

- Preserve separation between licensing domains
  - Windows Server licensing (Datacenter and Standard)
  - SQL Server licensing (Enterprise and Standard)
  - Azure SQL PaaS licensing
- Use core-based calculations aligned with Microsoft licensing rules
  - Windows Server: vCore extraction from VM sizes
  - SQL Server: 4-core minimum per VM
- Support user-provided entitlement tracking (2-core packs)
- Avoid certifying compliance or making licensing determinations
- Explicitly model unknown or incomplete states
- Remain compatible with Azure Resource Graph
- Are suitable for use in Azure Monitor Workbooks
- Do not rely on billing or Cost Management data

## Query guidelines

- Use Azure Resource Graph (KQL) only
- Avoid unsupported KQL features (e.g., `let` statements not supported in ARG)
- Prefer `summarize`, `count`, `countif`, `dcount`, `sumif`
- Use `toint()` and `tolong()` for numeric conversions
- Use consistent status values:
  - `AHB enabled`
  - `AHB not enabled`
  - `AHB unknown`
- Use `subscriptionName` for presentation
- Retain `subscriptionId` for traceability when appropriate

## Parameter guidelines

- Use clear, descriptive parameter labels
- Default values should be safe (e.g., "0" for entitlement packs)
- Support filtering where appropriate (SQL Edition, Allocation Mode)
- Document parameter behavior in workbook text sections

## Visualization guidelines

- Prefer Bar or Column charts with unstacked series
- Avoid more than two dimensions per visual
- Use consistent color semantics:
  - Green: AHB enabled, Within entitlement
  - Red: AHB not enabled, Exceeds entitlement
  - Yellow: AHB unknown
  - Grey/Info: Not configured
- Use tables for entitlement tracking with clear column headers
- Parameters may be used for filtering (SQL Edition, Subscription, Allocation Mode)

## Disclaimer and compliance language

- Always emphasize user responsibility for compliance validation
- Avoid language suggesting the tool certifies compliance
- Use "tracking", "monitoring", "indicative comparisons" rather than "compliance verification"
- Preserve all disclaimers about limitations and assumptions
- Document allocation modes as "modeling assumptions"

## Licensing considerations

- This project is MIT licensed
- Do not copy or adapt GPL-licensed content verbatim
- External inspiration is welcome, direct code reuse is not

## How to contribute

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Ensure queries, parameters, and visuals follow the principles above
5. Test in Azure Monitor Workbooks (validate KQL syntax)
6. Update standalone query files in `/queries` if applicable
7. Submit a pull request with a clear explanation of intent and scope

## What this project provides

- Core consumption tracking against user-provided SA entitlements
- Visibility into AHB enablement status across Azure resources
- Cost optimization opportunity identification (PAYG workloads)
- Audit-oriented licensing insights

## What this project does not provide

- Licensing compliance certification or validation
- Official Microsoft licensing guidance
- Automatic license assignment recommendations
- Contractual interpretation or legal advice

This is a **tracking and monitoring tool** that supports compliance efforts—users remain responsible for validating compliance with Microsoft and their license providers.

Thank you for helping keep this project accurate, useful, and trustworthy.
