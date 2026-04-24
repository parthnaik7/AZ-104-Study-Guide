# AZ-104: Implement and Manage Storage in Azure

This document explains how Azure stores your data, files, and information safely and cost-effectively.

It is written in plain language to give you a clear, first-principles understanding.

---

## 📘 Table of Contents

- [🗄 Storage Accounts](#-storage-accounts)
- [📦 Azure Blob Storage](#-azure-blob-storage)
- [📁 Azure Files](#-azure-files)
- [🛡 Storage Security](#-storage-security)
- [🔄 Redundancy (Backups)](#-redundancy-backups)
- [🧠 Quick Summary](#-quick-summary)

---

## 🗄 Storage Accounts

A Storage Account is a massive bucket in the cloud where you put your data. 

Before you can store anything, you must create a Storage Account. It gives you a unique internet address (URL) so you can access your data from anywhere in the world.

---

## 📦 Azure Blob Storage

Blob stands for "Binary Large Object".

This is used for storing massive amounts of unstructured data. Unstructured data means things like text files, photos, videos, and backups. 
- **Hot Tier:** For data you access all the time.
- **Cool Tier:** For data you access once a month. It costs less to store, but more to read.
- **Archive Tier:** For data you keep for years but rarely touch (like old legal documents). It is the cheapest to store, but takes hours to retrieve.

---

## 📁 Azure Files

While Blobs are for raw data, Azure Files acts like a traditional shared hard drive. 

You can mount an Azure File share directly onto your Windows or Mac computer, just like a USB drive or a network folder at work.
- It uses standard file-sharing protocols (like SMB).
- It is great for moving older, on-premise file servers to the cloud easily.

---

## 🛡 Storage Security

You must lock down your storage so only the right people can see it.

- **Storage Account Keys:** These are like master passwords. They give full control over everything in the account. You should rarely use them.
- **Shared Access Signatures (SAS):** This is a temporary, restricted pass. You can give someone a SAS token that says, "You can read this one photo, but only for the next hour."
- **Entra ID (RBAC):** The best and safest way to manage access. You use your normal identity rules to control who sees the data.

---

## 🔄 Redundancy (Backups)

You do not want to lose your data if a data center breaks. Redundancy means making copies.

- **LRS (Locally Redundant Storage):** Keeps 3 copies of your data inside a single physical building. Cheap, but risky if the whole building loses power.
- **ZRS (Zone-Redundant Storage):** Keeps 3 copies of your data across 3 separate buildings (Availability Zones) in the same region.
- **GRS (Geo-Redundant Storage):** Keeps copies in your main region, and also sends copies to a completely different region hundreds of miles away. Use this for disaster recovery.

---

## 🧠 Quick Summary

- **Storage Account** = The master bucket you must create first.
- **Blob Storage** = Massive, unstructured storage for things like photos and videos.
- **Azure Files** = A traditional network drive you can mount on your PC.
- **SAS Tokens** = Temporary, restricted access tickets to share data safely.
- **Redundancy (LRS, ZRS, GRS)** = Storing multiple copies of your data across buildings and regions to prevent loss.
