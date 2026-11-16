# Account Receivables Agent Extension

Extend the Account Receivables Agent with additional AR tools and custom instructions for tenant-specific financial analysis workflows.

## Business challenge

Extend the existing Account Receivables Agent (`sap.agtreacan:agent:receivable-account-analysis:v1`) with additional tools and instructions to support richer, tenant-specific accounts receivable analysis — covering dispute management, payment advice handling, and customised collection workflow logic.

## Business Architecture (RBA)

### End-to-End Process

Finance — Invoice to Cash (generic)

### Process Hierarchy

```
Finance (E2E)
└── Invoice to Cash (generic)
    └── Manage customer invoices (generic) [BPS-363]
        └── Invoice customer
    └── Process accounts receivables and collect payment (generic) [BPS-366]
        └── Manage and process collections
└── Manage Treasury (generic)
    └── Manage payment, cash and risk (generic) [BPS-414]
        └── Manage cash and working capital
```

### Summary

The extension targets the accounts receivable and collections sub-processes within the Finance Invoice-to-Cash E2E, adding dispute management and payment advice tooling on top of the existing agent's open-item and dunning capabilities.

## Fit Gap Analysis

| Requirement (business) | Standard asset(s) found | API ORD ID | MCP Server ORD ID | MCP Server Version | Webhook API ORD ID | Data Product ORD ID | Gap? | Notes / assumptions |
|---|---|---|---|---|---|---|---|---|
| Dispute management tooling | SAP S/4HANA Cloud (Dispute Management SC5423/SC89) | `sap.s4:apiResource:CE_API_DISPUTE_MANAGE_0001:v1` | — | — | — | — | Yes | No MCP server; custom tool via API needed |
| Payment advice retrieval | SAP S/4HANA Cloud (Payment Processing SC5668/SC3313) | `sap.s4:apiResource:CE_PAYMENTADVICE_0001:v1` | — | — | — | — | Yes | No MCP server; custom tool via API needed |
| Contract accounting dispute case | SAP S/4HANA Cloud (Dispute Management FI-CA) | `sap.s4:apiResource:CE_CADISPUTECASE_0002:v2` | — | — | — | — | Yes | No MCP server; custom tool via API needed |
| Custom AR instructions & hooks | Account Receivables Agent (extensible) | — | — | — | — | — | No | Agent supports instruction overrides + before/after hooks |
| Collections strategy guidance | SAP S/4HANA Cloud (Collections Management SC5289/SC100) | — | — | — | — | — | No | Covered by base agent; can be enhanced via instructions |

### Key findings

- The Account Receivables Agent (`sap.agtreacan:agent:receivable-account-analysis:v1`) is confirmed extensible with support for tool additions, custom instructions, and pre/post hooks.
- No MCP servers are available for the discovered AR APIs (Dispute, Payment Advice, Contract Accounting Dispute Case) — tools must be built as custom API integrations.
- SAP S/4HANA Cloud covers all core AR sub-processes; the extension fills gaps in dispute tooling and payment advice retrieval.
- The extension capability `urn:sap:extension-capability:v1:default` supports both tool additions and instruction customisation in a single deployment unit.
- Custom before/after hooks enable workflow automation triggers (e.g. notifications, escalations) around agent execution.

## Recommendations

### Extend Account Receivables Agent with Dispute and Payment Tooling

#### Executive Summary

Add dispute, payment advice tools and custom AR instructions to the base agent.

#### Recommended Solution

Build an agent extension for `sap.agtreacan:agent:receivable-account-analysis:v1` that:
1. Adds a dispute management tool backed by `CE_API_DISPUTE_MANAGE_0001`
2. Adds a payment advice retrieval tool backed by `CE_PAYMENTADVICE_0001`
3. Injects custom instructions to guide tenant-specific collection strategy logic
4. Optionally configures pre/post hooks for escalation or notification workflows

#### Recommended solution category

Agent Extension

#### Intent fit
90%
