# Azure Identity & Resource Hierarchy — Explained from First Principles

This document explains the core building blocks of Azure’s identity and resource model: Tenant, Microsoft Entra ID, Management Groups, Subscriptions, Resource Groups, Resources, and how identities (users, apps, managed identities) access those resources.

It’s designed to be a clean, readable reference for engineers learning Azure fundamentals.

---

## 📘 Table of Contents

- [📚 AZ-104 Learning Paths](#-az-104-learning-paths)
- [💯 Practice Exam](#-practice-exam)
- [🏛 What is a Tenant?](#-what-is-a-tenant)
- [🔐 What is Microsoft Entra ID?](#-what-is-microsoft-entra-id)
- [🧩 Azure Hierarchy Overview](#-azure-hierarchy-overview)
- [🗂 Management Groups](#-management-groups)
- [💳 Subscriptions](#-subscriptions)
- [📦 Resource Groups](#-resource-groups)
- [⚙️ Resources](#️-resources)
- [👤 Identity Types](#-identity-types)
- [🔑 How Access Works](#-how-access-works)
- [🔐 Access Mechanisms](#-access-mechanisms)
- [🧠 Quick Summary](#-quick-summary)

---

## 📚 AZ-104 Learning Paths

- [01 - Prerequisites for Azure Administrators](./01-prerequisites.md)
- [02 - Manage Identities and Governance in Azure](./02-identity-governance.md)
- [03 - Configure and Manage Virtual Networks](./03-virtual-networks.md)
- [04 - Implement and Manage Storage in Azure](./04-storage.md)
- [05 - Deploy and Manage Azure Compute Resources](./05-compute.md)
- [06 - Monitor and Backup Azure Resources](./06-monitor-backup.md)

---

## 💯 Practice Exam

- [AZ-104 100-Question Scenario Practice Exam](./PRACTICE-EXAM.md)

---

## 🧪 Official Microsoft Lab Exercises

Hands-on lab exercises from Microsoft Learn, aligned to the AZ-104 exam objectives:

> 🔗 **[AZ-104 Microsoft Azure Administrator - Official Labs](https://microsoftlearning.github.io/AZ-104-MicrosoftAzureAdministrator/)**

These labs complement this study guide with guided, practical exercises in a real Azure environment. Work through them alongside each module for maximum retention.

---

## 🏛 What is a Tenant?

A tenant is your organisation’s dedicated, isolated space in Microsoft’s cloud.

It represents your company and contains:
- Users
- Groups
- Applications
- Service principals
- Domains (e.g., `yourcompany.onmicrosoft.com`)
- Security policies

> **Note:** A tenant is the top‑level boundary for identity and access.

---

## 🔐 What is Microsoft Entra ID?

Microsoft Entra ID (formerly Azure AD) is the identity platform that runs inside your tenant.

It provides:
- Authentication (sign‑in)
- Authorization (permissions)
- Token issuance
- App registrations
- MFA & Conditional Access

> **Tenant** = the container  
> **Entra ID** = the identity engine inside it

---

## 🧩 Azure Hierarchy Overview

Azure’s structure flows top‑down:

```text
Tenant (organisation boundary)
│
└── Microsoft Entra ID (identity system)
    │
    └── Management Groups (governance)
        │
        └── Subscriptions (billing + access boundary)
            │
            └── Resource Groups (logical folders)
                │
                └── Resources (VMs, Storage, DBs, etc.)
```

---

## 🗂 Management Groups

Management groups sit above subscriptions and allow you to apply:
- Azure Policies
- RBAC roles
- Governance rules

These settings inherit downward to all child subscriptions.

---

## 💳 Subscriptions

A subscription is a billing and access boundary.
- All Azure resources must live inside a subscription
- Each subscription is tied to one tenant
- A tenant can have many subscriptions

**Common patterns:**
- Prod / Dev / Test
- Department‑based
- Cost centre separation

---

## 📦 Resource Groups

A resource group (RG) is a logical container for related resources.

**Used for:**
- Lifecycle management
- RBAC scoping
- Organising workloads

**Example:**

```text
rg-webapp
├── App Service
├── Storage Account
├── SQL Database
└── Virtual Network
```

---

## ⚙️ Resources

These are the actual Azure services you deploy:
- Virtual Machines
- Storage Accounts
- SQL Databases
- VNets
- App Services
- Key Vaults

> **Note:** Resources live inside resource groups, which live inside subscriptions.

---

## 👤 Identity Types

### Users
Human identities authenticated by Entra ID.

### Service Principals
Application identities using:
- Client ID + Secret
- Client ID + Certificate

*Used by automation, CI/CD, backend services.*

### Managed Identities
Azure‑managed service principals:
- No secrets
- Auto‑rotating credentials

*Best for VMs, Functions, App Services.*

---

## 🔑 How Access Works

Azure uses a consistent flow:

```text
Identity → Requests Token → Entra ID Issues Token → ARM Validates Token → RBAC Decides → Resource
```

**Steps:**
1. Identity authenticates with Entra ID
2. Entra ID issues an OAuth 2.0 access token
3. Token is sent to Azure Resource Manager (ARM)
4. ARM checks RBAC at the correct scope
5. Access is allowed or denied

**RBAC scopes:**
- Management Group
- Subscription
- Resource Group
- Resource

*Permissions inherit downward.*

---

## 🔐 Access Mechanisms

### Access Keys
- Full‑control keys for storage accounts
- Long‑lived, risky
- **Avoid when possible**

### SAS Tokens
- Time‑limited
- Permission‑scoped
- Safer than access keys

### OAuth Access Tokens
- Issued by Entra ID
- Used for ARM, Graph API, Key Vault, etc.
- Short‑lived and secure

---

## 🧠 Quick Summary

- **Tenant** = your organisation’s boundary
- **Entra ID** = identity system inside the tenant
- **Management Groups** = governance at scale
- **Subscriptions** = billing + access boundary
- **Resource Groups** = logical workload folders
- **Resources** = actual Azure services
- **RBAC** = who can do what
- **Managed Identity** = best way for apps to authenticate
