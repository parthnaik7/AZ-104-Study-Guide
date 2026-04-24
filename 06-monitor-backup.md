# AZ-104: Monitor and Backup Azure Resources

This document explains how to keep an eye on your cloud systems and how to fix things when they break.

It is written in plain language to give you a clear, first-principles understanding.

---

## 📘 Table of Contents

- [👁 Azure Monitor](#-azure-monitor)
- [📈 Metrics and Logs](#-metrics-and-logs)
- [🚨 Alerts](#-alerts)
- [💾 Azure Backup](#-azure-backup)
- [🌍 Azure Site Recovery](#-azure-site-recovery)
- [🧠 Quick Summary](#-quick-summary)

---

## 👁 Azure Monitor

Azure Monitor is the central hub for collecting, analysing, and acting on data from your cloud applications.

It helps you understand how your applications are performing. It actively identifies problems affecting your users.

---

## 📈 Metrics and Logs

Azure Monitor collects two main types of data.

### Metrics
Metrics are numbers measured over time.
- E.g., "CPU usage is at 85%."
- E.g., "There are 500 network connections right now."
Metrics are great for seeing real-time performance and building charts.

### Logs
Logs are detailed records of events that happened.
- E.g., "User John signed in at 9:00 AM."
- E.g., "Server crashed due to memory exhaustion."
Logs are great for deep investigation and figuring out *why* something broke. Logs are stored in a tool called **Log Analytics**.

---

## 🚨 Alerts

Nobody wants to sit and stare at charts all day. Alerts do the watching for you.

You instruct Azure Monitor to watch the data and notify you when something happens.
- "Send an email to the IT team if CPU usage goes over 90% for 5 minutes."
- "Send a text message if a server stops responding."
Alerts ensure you fix problems before your customers even notice them.

---

## 💾 Azure Backup

Data gets deleted on accident, or hit by ransomware. Azure Backup protects your data.

It is a service that creates safe, automated copies of:
- Virtual Machines
- SQL Databases
- Azure File Shares
Backups are stored securely in a **Recovery Services Vault**.

---

## 🌍 Azure Site Recovery

While Backup protects against deleted files, Site Recovery protects against entire regions going offline.

It is a Disaster Recovery (DR) tool. 
- It constantly copies your entire running Virtual Machine to a different region (e.g., from 'Australia East' to 'Australia Southeast').
- If a hurricane takes out your main data center, you click a button and your servers boot up in the safe region within minutes.

---

## 🧠 Quick Summary

- **Azure Monitor** = The main hub for watching your resources.
- **Metrics** = Numbers that track performance (like CPU %).
- **Logs** = Detailed records used to investigate problems.
- **Alerts** = Automated notifications when something goes wrong.
- **Azure Backup** = Protecting servers and files from deletion and ransomware.
- **Azure Site Recovery** = Protecting your entire business from massive regional disasters.
