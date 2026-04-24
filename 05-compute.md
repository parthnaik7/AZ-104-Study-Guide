# AZ-104: Deploy and Manage Azure Compute Resources

This document explains the "engine room" of Azure—the services that actually run your applications and code.

It is written in plain language to give you a clear, first-principles understanding.

---

## 📘 Table of Contents

- [🖥 Virtual Machines (VMs)](#-virtual-machines-vms)
- [⚖️ Availability Sets and VM Scale Sets](#️-availability-sets-and-vm-scale-sets)
- [🌐 Azure App Service](#-azure-app-service)
- [🐳 Containers (ACI and AKS)](#-containers-aci-and-aks)
- [🧠 Quick Summary](#-quick-summary)

---

## 🖥 Virtual Machines (VMs)

A Virtual Machine (VM) is a computer running in the cloud. It acts exactly like a physical laptop or server.

You use VMs when you need full control over the operating system (Windows or Linux).
- You choose the "size" (how much memory and CPU power it has).
- You attach virtual hard disks to it.
- You are responsible for installing updates, antivirus protection, and software.

---

## ⚖️ Availability Sets and VM Scale Sets

Servers can fail. You need systems to keep your apps running if a server dies.

### Availability Sets
This makes sure your VMs are not sharing the same power supply or network switch inside the Microsoft data center. 
- If you have two web servers, put them in an Availability Set. 
- If a rack of servers loses power, only one of your VMs goes down. The other stays up.

### Virtual Machine Scale Sets (VMSS)
This allows you to create and manage a group of identical, load-balanced VMs.
- It can automatically add more VMs when your website gets busy (scaling out).
- It can automatically remove VMs when things quiet down, saving you money (scaling in).

---

## 🌐 Azure App Service

Azure App Service is a Platform as a Service (PaaS). 

Instead of building a VM and installing a web server on it yourself, you just give your application code to Azure, and Azure runs it for you.
- You do not worry about the underlying operating system.
- It is much faster and easier for web developers to use.
- It scales up and down very easily.

---

## 🐳 Containers (ACI and AKS)

Containers are a modern way to package software. They bundle your app and all its dependencies into one small container.

It guarantees the app will run the same way on your laptop as it does in the cloud.

### Azure Container Instances (ACI)
The fastest and simplest way to run a container in Azure. You just upload the container and press start. Great for simple jobs.

### Azure Kubernetes Service (AKS)
A powerful system for running hundreds or thousands of containers at once. It manages scaling, health checks, and networking for massive applications.

---

## 🧠 Quick Summary

- **Virtual Machine (VM)** = A full computer in the cloud. You manage the operating system.
- **Availability Sets** = Splitting your VMs across different hardware racks to avoid failures.
- **VM Scale Sets (VMSS)** = Automatically adding or removing identical VMs based on demand.
- **App Service** = A fully managed platform to host websites without worrying about servers.
- **Containers (ACI / AKS)** = Packaging apps neatly so they run consistently anywhere.
