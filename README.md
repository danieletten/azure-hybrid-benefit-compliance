# Azure Hybrid Benefit & Software Assurance Entitlement Tracker

This repository contains an Azure Monitor Workbook that provides visibility into Azure Hybrid Benefit (AHB) usage and Software Assurance entitlement consumption across Azure subscriptions.

The workbook is designed as a **reference implementation** for architects, FinOps teams, SAM (Software Asset Management) professionals, and governance or audit stakeholders who need transparent insight into how Microsoft licenses are consumed in Azure.

## Primary Use Cases

1. **Track Software Assurance entitlement consumption** (core-based) against available 2-core packs
2. **Identify AHB opportunities**: Find VMs eligible for Azure Hybrid Benefit currently using pay-as-you-go licensing
3. **Monitor AHB enablement status** across Windows Server, SQL Server, and Azure SQL Platform Services

## What this workbook covers

The workbook models licensing using **core-based calculations** aligned with Microsoft licensing rules:

- **Windows Server licensing** on Azure Virtual Machines
  - Datacenter and Standard editions (separate 2-core pack tracking)
  - Core consumption based on VM vCore counts
  - User-configurable allocation mode (Datacenter First, Standard First, or No Allocation)
- **SQL Server licensing** on Azure Virtual Machines (SQL IaaS Agent)
  - Enterprise and Standard editions (separate 2-core pack tracking)
  - 4-core minimum per VM licensing rule applied
  - Core consumption with edition-aware calculations
  - SQL Edition filtering (Standard / Enterprise / Unknown)
- **Azure SQL Database licensing**
- **Azure SQL Managed Instance licensing**

Each licensing domain is treated as a **separate compliance layer**, which avoids double counting and aligns with how Microsoft licensing is audited in practice.

## Key features

### Core-Based Calculations
- Windows Server: vCore extraction from VM sizes (e.g., Standard_D4s_v3 → 4 cores)
- SQL Server: 4-core minimum applied per VM as per Microsoft licensing rules
- Real-time core consumption tracking against user-provided 2-core pack entitlements

### Software Assurance Entitlement Tracking
- Input your 2-core pack quantities for:
  - Windows Server Datacenter
  - Windows Server Standard
  - SQL Server Enterprise
  - SQL Server Standard
- Compare consumption against available cores
- Status indicators: Within entitlement, Exceeds entitlement, Not configured

### Windows Server Allocation Mode
**Important limitation**: Azure Resource Graph cannot distinguish between Datacenter and Standard licenses (both use `licenseType = 'Windows_Server'`).

The workbook provides three allocation modes as **user-defined assumptions**:
- **Datacenter First**: Allocate consumption to Datacenter pool first, overflow to Standard
- **Standard First**: Allocate consumption to Standard pool first, overflow to Datacenter
- **No Allocation**: Show total consumption without allocation (informational only)

This approach is transparent about the limitation while providing practical tracking options.

### Licensing Visibility
- Clear distinction between:
  - **AHB enabled**: Actively using Azure Hybrid Benefit
  - **AHB not enabled**: Pay-as-you-go licensing (opportunity for AHB)
  - **AHB unknown**: License status cannot be determined
- Subscription-based presentation with clear resource grouping
- Interactive filtering using Workbook parameters (SQL Edition selection)
- Audit-safe terminology throughout

## Data sources

All data is retrieved using **Azure Resource Graph**, including:

- `microsoft.compute/virtualmachines`
- `microsoft.sqlvirtualmachine/sqlvirtualmachines`
- `microsoft.sql/servers/databases`
- `microsoft.sql/managedinstances`
- `microsoft.resources/subscriptions`

No billing data or Cost Management exports are required.

## Important limitations and disclaimers

- **Core consumption calculations** are based on VM vCore counts from Azure Resource Graph
- **SQL Server**: Only VMs registered with the SQL IaaS Agent are included. VMs running SQL Server without this agent will not appear.
- **Windows Server allocation**: Azure cannot automatically determine which VMs use Datacenter vs Standard licenses. Allocation mode is a user-selected assumption for tracking purposes only.
- **Not a licensing compliance tool**: This workbook provides **indicative comparisons** based on user-provided entitlement data
- Results should **not be used as the sole basis** for licensing compliance decisions
- Licensing compliance must always be validated against official Microsoft documentation and your contractual agreements
- This tool does not verify licensing eligibility, mobility rights, or contractual terms
- Unknown states are intentionally preserved and must not be interpreted as compliant

## How to use

1. **Deploy the workbook** to your Azure Monitor Workbooks
2. **Enter your entitlements**:
   - Windows Server Datacenter 2-Core Packs
   - Windows Server Standard 2-Core Packs (if applicable)
   - SQL Server Enterprise 2-Core Packs
   - SQL Server Standard 2-Core Packs
3. **Select Windows Server Allocation Mode** based on your tracking preference
4. **Filter SQL Edition** if you want to focus on specific SQL Server versions
5. **Review consumption tables** to see how cores are consumed against your entitlements
6. **Identify AHB opportunities** by reviewing PAYG VMs that could use Azure Hybrid Benefit
7. **Export or screenshot** results for documentation or SAM reviews

## Disclaimer

This workbook is provided for **informational and tracking purposes only**.

While care has been taken to ensure technical accuracy, no rights can be derived from the information presented. Core consumption calculations are based on Azure Resource Graph data and user-provided entitlement information. The workbook provides indicative comparisons and does not constitute official licensing guidance or compliance validation.

Licensing compliance must always be validated against official Microsoft documentation, licensing statements, and your contractual agreements with Microsoft or authorized partners.

## Inspiration and attribution

This project is inspired by existing Azure Hybrid Benefit workbooks in the community, including work by Ryan Lowell.  
This implementation is a clean-room design that extends the concept with additional workloads, licensing layers, SQL edition awareness, and audit-focused modeling.

## License

This project is licensed under the **MIT License**.  
See the `LICENSE` file for details.
