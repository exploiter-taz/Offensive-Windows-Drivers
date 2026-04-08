# Offensive Windows Driver

## 📌 Overview

**Offensive Windows Driver** is a practical portfolio repository focused on Windows kernel‑mode research and offensive driver development.  
This project contains structured examples and documentation related to building custom drivers, interacting with IRP/IOCTL interfaces, manipulating kernel callbacks, and exploring defensive bypass techniques such as EDR/AV evasion and privilege escalation.

The purpose of this repository is to **showcase real hands‑on capabilities** in driver development and kernel security — suitable for technical evaluation by hiring managers, security teams, or collaborators.

## 🧪 Contents

This repo includes:

📁 **01‑Hello‑Driver/**  
&nbsp;&nbsp;– Basic driver example with setup and notes.

📁 **02‑IOCTL‑IRP/**  
&nbsp;&nbsp;– Handling IRP/IOCTL communication with the driver.

📁 **03‑Callback‑Manipulation/**  
&nbsp;&nbsp;– Experiments and write‑ups on kernel callback handling.

📁 **04‑EDR‑AV‑Evasion/**  
&nbsp;&nbsp;– Concepts related to defensive bypass using drivers.

📁 **tools/**  
&nbsp;&nbsp;– Supporting scripts and helper utilities.

## 🧠 What You’ll Find

Each module contains:
- Clear explanation of the goal and context.
- Sample source code in C/C++.
- Build and load instructions (Visual Studio + WDK).
- Observations, results, and relevant notes for learning.

## 📋 Prerequisites

To build and test drivers in this repo, you should have:
- Windows 10/11 Virtual Machine
- Visual Studio with Windows Driver Kit (WDK)
- Debugging tools (e.g., WinDbg)
- Test signing enabled on the VM

## 📘 How to Use

1. Open the appropriate folder for the experiment.
2. Follow the included steps in its README.
3. Build using the WDK and sign the driver (test mode).
4. Load the driver using a tool like `sc.exe` or custom loader.
5. Review debug output for behavior and results.

## ⚠️ Safety Notice

**Important:** Everything in this repository is intended strictly for **learning, research, and testing in isolated environments (VMs)**.  
Do **not** load or run these drivers on production machines or systems outside safe lab environments.

## 🏷️ Topics

Offensive security, Windows driver development, kernel‑mode, EDR/AV evasion, callback manipulation, IRP/IOCTL, security research

---

## 🗂️ Want to Contribute?

Contributions, write‑ups, and improvements are welcome!  
If you want to propose changes, add a module, or improve documentation, feel free to open a pull request.

---

## 👤 About

Maintained by **Your Name** — Security researcher focused on Windows kernel and offensive tooling.
