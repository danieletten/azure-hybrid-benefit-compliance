# Contributing

Thank you for your interest in contributing to this project.

This repository aims to remain a **clear, audit-aware reference implementation** for Microsoft licensing compliance in Azure. Contributions are welcome, provided they align with the principles below.

## Guiding principles

Before contributing, please ensure that changes:

- Preserve separation between licensing domains
  - Windows Server licensing
  - SQL Server licensing
  - Azure SQL PaaS licensing
- Avoid implicit assumptions or inferred compliance
- Explicitly model unknown or incomplete states
- Remain compatible with Azure Resource Graph
- Are suitable for use in Azure Monitor Workbooks
- Do not rely on billing or Cost Management data

## Query guidelines

- Use Azure Resource Graph (KQL) only
- Avoid unsupported KQL features
- Prefer `summarize`, `count`, `countif`, and `dcount`
- Use consistent status values:
  - `AHB enabled`
  - `AHB not enabled`
  - `AHB unknown`
- Use `subscriptionName` for presentation
- Retain `subscriptionId` for traceability when appropriate

## Visualization guidelines

- Prefer Bar or Column charts with unstacked series
- Avoid more than two dimensions per visual
- Use consistent color semantics:
  - Green: AHB enabled
  - Red: AHB not enabled
  - Yellow: AHB unknown
- Parameters may be used for filtering (for example SQL Edition or Subscription)

## Licensing considerations

- This project is MIT licensed
- Do not copy or adapt GPL-licensed content verbatim
- External inspiration is welcome, direct code reuse is not

## How to contribute

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Ensure queries and visuals follow the principles above
5. Submit a pull request with a clear explanation of intent and scope

## What this project is not

- A licensing entitlement calculator
- A replacement for Microsoft licensing guidance
- A cost optimization tool

It is a **technical compliance visibility tool**, by design.

Thank you for helping keep this project clean, useful, and trustworthy.
