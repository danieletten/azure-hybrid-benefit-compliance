![MIT License](https://img.shields.io/badge/license-MIT-green)
![Azure Resource Graph](https://img.shields.io/badge/Source-Azure%20Resource%20Graph-blue)
![Workbook](https://img.shields.io/badge/Workbook-Azure%20Monitor-lightgrey)

# Azure Hybrid Benefit Compliance

An Azure Monitor Workbook for tracking Azure Hybrid Benefit (AHB) usage and Software Assurance (SA) entitlement consumption across Windows Server and SQL Server workloads.

**Use this workbook to:**
- Track core consumption against SA entitlements (2-core pack based)
- Identify pay-as-you-go workloads eligible for AHB cost savings
- Monitor AHB enablement status across your Azure estate

> **⚠️ Important:** This project is for informational and governance use only. No rights can be derived from the data presented. Always validate licensing compliance with official Microsoft documentation and your contractual agreements.

---

<details>
<summary><b>📘 What is Azure Hybrid Benefit?</b></summary>

**Azure Hybrid Benefit (AHB)** allows organizations to use on-premises Windows Server and SQL Server licenses with active Software Assurance to reduce Azure costs by up to 40-55%.

**Key benefits:**
- Pay only for base compute costs in Azure
- Use licenses in Azure, on-premises, or both (with mobility rights)
- Applies to Windows Server Datacenter/Standard and SQL Server Enterprise/Standard editions

**The challenge:** Organizations need to track which cores are actively using AHB, ensure they're within SA entitlements, and identify PAYG workloads that could benefit from AHB.

</details>

---

## Getting Started

### Installation

1. Download [`workbooks/Azure-Monitor-Workbook.json`](workbooks/Azure-Monitor-Workbook.json)
2. Open [Azure Portal](https://portal.azure.com) → **Azure Monitor** → **Workbooks** → **+ New**
3. Click **Advanced Editor** (toolbar icon `</>`)
4. Paste the downloaded JSON and click **Apply** → **Done Editing** → **Save**

### Configuration

After importing, configure your SA entitlements at the top of the workbook:
- Windows Server Datacenter 2-Core Packs
- Windows Server Standard 2-Core Packs (if applicable)
- SQL Server Enterprise 2-Core Packs
- SQL Server Standard 2-Core Packs
- Windows Server Allocation Mode (Datacenter First / Standard First / No Allocation)

The workbook will compare your actual core consumption against these entitlements.

---

## What This Workbook Provides

### Core-Based Entitlement Tracking

The workbook calculates core consumption using Microsoft licensing rules:
- **Windows Server**: vCore extraction from VM sizes (e.g., Standard_D4s_v3 → 4 cores)
- **SQL Server**: 4-core minimum per VM applied automatically
- **Real-time tracking**: Compare consumption against your 2-core pack entitlements

### Workload Coverage

- **Windows Server VMs** (Datacenter and Standard editions)
- **SQL Server on VMs** (Enterprise and Standard editions, requires SQL IaaS Agent)
- **Azure SQL Database**
- **Azure SQL Managed Instance**

### Windows Server Allocation Mode

**Important:** Azure Resource Graph cannot distinguish between Datacenter and Standard licenses (both use `licenseType = 'Windows_Server'`).

Choose an allocation assumption:
- **Datacenter First**: Allocate to DC pool first, overflow to Standard
- **Standard First**: Allocate to Standard pool first, overflow to DC
- **No Allocation**: Show total consumption only (informational)

<details>
<summary><b>📊 Technical Details</b></summary>

### Data Sources
All data retrieved via **Azure Resource Graph** (no billing data required):
- `microsoft.compute/virtualmachines`
- `microsoft.sqlvirtualmachine/sqlvirtualmachines`
- `microsoft.sql/servers/databases`
- `microsoft.sql/managedinstances`

### Features
- Subscription-based resource grouping
- SQL Edition filtering (Standard/Enterprise/Unknown)
- Status indicators: Within entitlement, Exceeds entitlement, Not configured
- Clear license state visibility: AHB enabled, AHB not enabled, AHB unknown
- Audit-safe terminology throughout

</details>

---

## Important Limitations and Disclaimers

**⚠️ Compliance Responsibility**

This workbook helps you **track and monitor** your Azure Hybrid Benefit usage, but you remain responsible for validating licensing compliance. This tool provides visibility and tracking insights to support your compliance efforts—it does not certify or guarantee compliance.

**Always validate your licensing compliance by:**
- Consulting official Microsoft licensing documentation and product terms
- Reviewing your contractual agreements with Microsoft or authorized partners
- Engaging with your license reseller or Microsoft licensing specialist
- Conducting regular licensing audits through appropriate channels

**Technical Limitations:**
- **Core calculations** are based on VM vCore counts from Azure Resource Graph
- **SQL Server**: Only VMs registered with SQL IaaS Agent are included
- **Windows Server allocation**: Cannot automatically determine DC vs Standard—allocation mode is a user assumption only
- **Indicative comparisons**: Results depend on user-provided entitlement data accuracy
- **No verification**: Does not verify licensing eligibility, mobility rights, or contractual terms
- **Unknown states preserved**: Must not be interpreted as compliant

---

## Inspiration and Attribution

This project is inspired by existing Azure Hybrid Benefit workbooks in the community, including work by Ryan Lowell. This implementation is a clean-room design extending the concept with additional workloads, licensing layers, SQL edition awareness, and audit-focused modeling.

## License

MIT License - See the `LICENSE` file for details.
