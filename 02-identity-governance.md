# AZ-104: Manage Identities and Governance in Azure

This document explains how to control who can use your Azure resources and what they are allowed to do. 

It is written in plain language to give you a clear, first-principles understanding.

---

## 📘 Table of Contents

- [👤 Microsoft Entra ID](#-microsoft-entra-id)
- [👥 Users and Groups](#-users-and-groups)
- [🛡 Role-Based Access Control (RBAC)](#-role-based-access-control-rbac)
- [📜 Azure Policy](#-azure-policy)
- [📏 Resource Locks](#-resource-locks)
- [🧠 Quick Summary](#-quick-summary)

---

## 👤 Microsoft Entra ID

Microsoft Entra ID is the system that checks who you are (authentication) and what you can do (authorization). 
- It is the front door to your Azure account.
- It stores the usernames, passwords, and security rules for your organisation.
- It was previously called Azure Active Directory.

---

## 👥 Users and Groups

To manage access efficiently, you work with Users and Groups.

### Users
A user is a single person. Every person in your organisation who needs to use Azure should have their own user account. 

### Groups
A group is a collection of users. 
- You put users into a group (like "IT Team" or "Finance").
- You give the group access, instead of giving it to each person one by one.
- This saves time and stops mistakes.

---

## 🛡 Role-Based Access Control (RBAC)

Role-Based Access Control (RBAC) is the system Azure uses to strictly control what actions users can take.

You need three things to give someone access:
1. **Who? (Security Principal):** The user or group.
2. **What can they do? (Role Definition):** The job title, like 'Reader' (can only look) or 'Contributor' (can make changes but not give access to others).
3. **Where? (Scope):** The specific part of Azure they can touch, like a single Resource Group or an entire Subscription.

---

## 📜 Azure Policy

While RBAC manages *who* can do things, Azure Policy manages *what* things are allowed to exist at all.

Azure Policy sets the rules for your organisation.
- You can force people to only build servers in specific countries.
- You can stop anyone from building huge, expensive servers by accident.
- If a resource breaks a rule, Azure will flag it or simply block it from being made.

---

## 📏 Resource Locks

A Resource Lock is a safety switch. It prevents accidents.

Even if you have full permission to delete things, a lock will stop you.

- **CanNotDelete:** People can still read and change the resource, but they cannot delete it.
- **ReadOnly:** People can only look at the resource. They cannot change or delete it.

---

## 🧠 Quick Summary

- **Entra ID** = The system that stores identities and checks passwords.
- **Groups** = The smart way to handle users in bulk.
- **RBAC** = Defines who has permission to do specific jobs in certain areas.
- **Azure Policy** = Sets the rules for what is allowed to be built.
- **Resource Locks** = Safety switches that stop accidental changes or deletions.
