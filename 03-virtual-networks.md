# AZ-104: Configure and Manage Virtual Networks

This document explains how your different Azure services talk to each other and the internet.

It is written in plain language to give you a clear, first-principles understanding.

---

## 📘 Table of Contents

- [🕸 What is a Virtual Network (VNet)?](#-what-is-a-virtual-network-vnet)
- [🛤 Subnets](#-subnets)
- [🚦 Network Security Groups (NSGs)](#-network-security-groups-nsgs)
- [🌍 IP Addressing](#-ip-addressing)
- [🤝 VNet Peering](#-vnet-peering)
- [🧠 Quick Summary](#-quick-summary)

---

## 🕸 What is a Virtual Network (VNet)?

A Virtual Network (VNet) is your private slice of the cloud. It is essentially a secure bubble.

When you create a server (Virtual Machine) in Azure, it needs a VNet so it can talk to other resources safely. 
- VNets are isolated from each other. 
- By default, things inside a VNet can talk to the internet, but the internet cannot reach inside.

---

## 🛤 Subnets

A VNet is usually very large. You break it down into smaller, manageable sections called Subnets.

Think of a VNet as a large office building, and Subnets as different rooms inside that building.
- You might put your web servers in one room (Subnet A).
- You might put your private databases in another room (Subnet B).
- This separation makes your network more organised and secure.

---

## 🚦 Network Security Groups (NSGs)

A Network Security Group (NSG) is a digital bouncer for your network. It acts as a basic firewall.

You use an NSG to create strict rules about what data can go in (inbound) and out (outbound).
- You can say, "Allow web traffic (Port 80) to enter the web server subnet."
- You can say, "Block all internet traffic from reaching the database subnet."

---

## 🌍 IP Addressing

Devices need addresses to find each other on a network. 

### Private IP Addresses
These are hidden inside your VNet. Your servers use them to talk to each other safely. The outside world cannot see these addresses.

### Public IP Addresses
These face the internet. If you host a public website, you need a Public IP address so the world can visit it.

---

## 🤝 VNet Peering

Usually, VNets are completely isolated. Traffic in VNet A cannot reach VNet B.

VNet Peering is a way to seamlessly connect two VNets together. 
- The networks behave as if they are one big safe network.
- The traffic stays on Microsoft's private network instead of going out to the public internet, making it fast and secure.

---

## 🧠 Quick Summary

- **VNet** = Your private, secure network bubble in the cloud.
- **Subnet** = A smaller section inside your VNet to organise resources.
- **NSG** = The bouncer that decides what traffic is allowed in or out.
- **Private IP** = Internal network address.
- **Public IP** = External internet address.
- **VNet Peering** = Connecting two VNets together securely.
