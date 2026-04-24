# AZ-104: Prerequisites for Azure Administrators

This document explains the basic ideas you need to understand before you start managing Microsoft Azure. These are the building blocks of cloud computing and Azure's setup.

It is written in plain language to give you a clear, first-principles understanding.

---

## 📘 Table of Contents

- [☁️ What is Cloud Computing?](#️-what-is-cloud-computing)
- [🏢 Core Azure Architecture](#-core-azure-architecture)
- [💻 Using the Azure Portal](#-using-the-azure-portal)
- [🛠 Tools for the Job](#-tools-for-the-job)
- [🧠 Quick Summary](#-quick-summary)

---

## ☁️ What is Cloud Computing?

Cloud computing means renting computer power and storage over the internet instead of buying your own hardware. 

You only pay for what you use. This helps you:
- Keep costs low.
- Scale up quickly when you need more power.
- Focus on your business instead of hardware maintenance.

**Cloud Models:**
- **IaaS (Infrastructure as a Service):** You rent the raw servers and storage. You manage everything from the operating system up.
- **PaaS (Platform as a Service):** You rent a ready-to-use platform, like a database or web server. You just focus on your application.
- **SaaS (Software as a Service):** You use a fully finished product, like an email service (e.g., Microsoft 365).

---

## 🏢 Core Azure Architecture

Azure is built on a massive global network. 

### Regions
A region is a geographical area grouping multiple data centers. Examples are 'Australia East' or 'US West'. 
- You want to pick a region close to your users to make things fast.
- Some features are only available in certain regions.

### Availability Zones
Within a region, there are separate, physical data centers called Availability Zones. 
- They have their own power, cooling, and network.
- If one data center goes down, the others stay up. 

---

## 💻 Using the Azure Portal

The Azure Portal is the main website where you manage everything.

It lets you:
- Create new resources (like virtual machines or databases).
- See how much money you are spending.
- Manage who has access to what.
- Monitor the health of your services.

---

## 🛠 Tools for the Job

While the Portal is good for starting out, administrators use rules and code to manage things faster and without mistakes.

### Azure CLI
A tool to type commands on your screen to control Azure. It works on Windows, Mac, and Linux.

### Azure PowerShell
A powerful scripting tool. It uses commands called "cmdlets" specifically for Azure. It is great for automating tasks.

### Azure Cloud Shell
A built-in command-line tool right inside the Azure Portal. You do not need to install anything on your computer to use it.

---

## 🧠 Quick Summary

- **Cloud Computing** = Renting computer power over the internet.
- **IaaS / PaaS / SaaS** = Different levels of renting (from raw hardware to finished apps).
- **Regions** = Large geographical areas where data centers live.
- **Availability Zones** = Physically separate data centers in one region to prevent failures.
- **Azure Portal** = The website you use to point and click.
- **CLI & PowerShell** = The tools you use to type commands and automate tasks.
