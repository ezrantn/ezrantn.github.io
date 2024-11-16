---
title: Introducing Macshift
date: '2024-11-14'
author: Ezra Natanael
categories:
  - Projects
slug: introducing-macshift
---

I created Macshift as a fun project to help people quickly change their MAC addresses on Windows. It’s a command-line tool written in Go, designed to give users better control over their network adapters without getting too technical. Here’s a simple rundown of how it works and why it might be useful!

---

## What is a MAC Address?

A **MAC address** (short for Media Access Control address) is like a unique ID for each device that connects to a network. It’s made up of 6 bytes (or 48 bits) and is often shown as six pairs of characters, like `00:1A:2B:3C:4D:5E`.

### How a MAC Address Works

- **First 3 bytes**: These identify the manufacturer of the device (like Intel or Dell).
- **Last 3 bytes**: These are unique to each device from that manufacturer.

So, a MAC address helps identify who made the device and makes sure each device is unique.

### Why is a MAC Address Useful?

1. **Device Identification**: It’s how devices on the same network (like in your home or office) know who’s who. The router can tell your laptop from your phone by their MAC addresses.

2. **Network Data Delivery**: When you send data (like loading a website), the MAC address helps make sure it goes to the right device on your network.

3. **Network Access Control**: Some networks only let devices with certain MAC addresses connect, so changing a MAC address can sometimes help with network access or troubleshooting.

4. **Privacy on Public Networks**: If you’re using public Wi-Fi, changing your MAC address can make it harder to track your device as you connect to different networks.

In short, a MAC address is an important part of how networks identify and send data to devices, and Macshift gives you the power to change it when needed.

---

## What Macshift Does

Macshift allows you to change the MAC (Media Access Control) address of your network adapters with just a few commands. There are various reasons someone might want to alter their MAC address, from improving privacy on public networks to troubleshooting connectivity issues. Sometimes, networks use MAC filtering, so changing yours can even help you bypass certain restrictions.

---

## How Macshift Works

Macshift operates by interacting directly with your network adapter’s settings in the Windows registry, which stores configuration data for Windows. When you choose to change your MAC address, Macshift generates a new, random address. It then saves this new MAC in the registry, replacing the original, and disables and re-enables the adapter to apply the change. If you want to return to the original MAC, Macshift has a command to restore it, removing the custom address and re-enabling the adapter with the original one.

Macshift has three main commands:

1. **List Adapters**:  Displays all your network adapters with their names and current MAC addresses.
2. **Change MAC Address**: Generates a random MAC address for a selected adapter and updates the system.
3. **Restore Original MAC**: Brings back the original MAC address if you need to undo the change.

---

## Built for Simplicity

To make Macshift easy to use, I built it with an interactive command system using [Cobra](https://cobra.dev/), so each command feels straightforward and natural. You just pick an adapter, run a command, and Macshift handles the rest!

So, if you ever need to change a MAC address on Windows, give Macshift a try. It was a fun project to work on, and it’s open-source, so feel free to check it out or suggest improvements!

---

## Try Macshift on GitHub

You can find Macshift on [GitHub](https://github.com/ezrantn/macshift), where it's available for easy installation. Just run a simple command to get started, and you'll be able to manage your MAC addresses in no time. Contributions are welcome, so if you have ideas for improvements or new features, feel free to jump in and submit a pull request!