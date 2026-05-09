<div align="center">

# 🤝 Contributing to Network Practical

**Thank you for taking the time to contribute!**
Every lab, fix, diagram, or documentation improvement makes this resource better for the entire networking community.

[![Stars](https://img.shields.io/github/stars/hackThacker/network-practical?style=flat-square)](https://github.com/hackThacker/network-practical/stargazers)
[![Forks](https://img.shields.io/github/forks/hackThacker/network-practical?style=flat-square)](https://github.com/hackThacker/network-practical/network/members)
[![Issues](https://img.shields.io/github/issues/hackThacker/network-practical?style=flat-square)](https://github.com/hackThacker/network-practical/issues)

[Code of Conduct](#-code-of-conduct) • [Ways to Contribute](#-ways-to-contribute) • [Prerequisites](#-prerequisites) • [Getting Started](#-getting-started) • [Lab Standards](#-lab-submission-standards) • [Naming Conventions](#-naming-conventions) • [Pull Request Process](#-pull-request-process) • [Issue Reporting](#-issue-reporting) • [Checklist](#-contribution-checklist)

</div>

---

## 📜 Code of Conduct

By participating in this project, you agree to maintain a respectful and inclusive environment. All contributors are expected to:

- Use welcoming and inclusive language
- Respect differing viewpoints and experience levels
- Accept constructive criticism gracefully
- Focus on what is best for the community
- Show empathy toward other contributors

Violations may result in your contribution being rejected or your access being revoked.

---

## 💡 Ways to Contribute

There are many ways you can add value to this project — you don't have to be an expert:

| Contribution Type | Description |
|---|---|
| 🆕 **New Lab** | Submit a brand-new Cisco Packet Tracer scenario not already covered |
| 🐛 **Bug Fix** | Fix broken links, incorrect configs, or misconfigured topologies |
| 📄 **Documentation** | Improve or add README/documentation for existing labs |
| 🖼️ **Topology Diagrams** | Add missing or higher-quality network topology images |
| 🔧 **Config Improvement** | Optimize or correct existing `.pkt` configurations |
| 💬 **Issue Reporting** | Report broken links, outdated content, or unclear documentation |
| ⭐ **Star & Share** | Help others discover the repo by starring and sharing it |

---

## 🛠️ Prerequisites

Before contributing, make sure you have the following ready:

### Required Tools

```
✅ Cisco Packet Tracer  →  Version 8.0 or above (recommended: latest stable)
✅ Git                  →  For cloning, branching, and submitting pull requests
✅ GitHub Account       →  For forking and submitting contributions
✅ Text Editor          →  VS Code, Notepad++, or any Markdown-capable editor
```

### Knowledge Requirements

```
✅ Basic understanding of Cisco IOS CLI commands
✅ Familiarity with network protocols (IP, DHCP, RIP, EIGRP, VoIP, etc.)
✅ Ability to write clear and simple English documentation
✅ Basic understanding of GitHub workflow (fork → branch → PR)
```

> 💡 **Note:** You can download Cisco Packet Tracer for free from the [Cisco Networking Academy](https://www.netacad.com/courses/packet-tracer).

---

## 🚀 Getting Started

Follow these steps to set up your environment and submit a contribution:

### Step 1 — Fork the Repository

Click the **Fork** button on the top-right of the repo page to create your own copy:

```
https://github.com/hackThacker/network-practical
```

### Step 2 — Clone Your Fork

```bash
git clone https://github.com/<your-username>/network-practical.git
cd network-practical
```

### Step 3 — Create a New Branch

Always work on a dedicated branch — never commit directly to `main`:

```bash
git checkout -b feature/your-lab-name
```

**Branch naming examples:**

```bash
feature/university-network-design
fix/soho-dhcp-config
docs/update-hospital-readme
improvement/eigrp-topology-diagram
```

### Step 4 — Add Your Contribution

Follow the [Lab Submission Standards](#-lab-submission-standards) and [Naming Conventions](#-naming-conventions) below before adding files.

### Step 5 — Commit Your Changes

Write clear, meaningful commit messages:

```bash
git add .
git commit -m "feat: add University Network Design lab with OSPF routing"
```

**Commit message prefixes:**

| Prefix | Use For |
|---|---|
| `feat:` | Adding a new lab or feature |
| `fix:` | Fixing a broken config, link, or topology |
| `docs:` | Documentation changes only |
| `improve:` | Enhancing an existing lab or diagram |
| `chore:` | Maintenance tasks (renaming files, cleanup) |

### Step 6 — Push and Open a Pull Request

```bash
git push origin feature/your-lab-name
```

Then go to your fork on GitHub and click **"Compare & pull request"**.

---

## 📐 Lab Submission Standards

Every lab submitted to this repository **must** include all three of the following components:

### 1. 📁 Folder Structure

Each lab must live inside its own numbered and named folder:

```
<number> <Lab Name>/
├── README.md               ← Documentation (required)
├── <Lab Name>.png          ← Topology diagram image (required)
└── <Lab Name>.pkt          ← Cisco Packet Tracer file (required)
```

**Example:**

```
9 University Network Design/
├── README.md
├── 9 University Network Design.png
└── 9 University Network Design.pkt
```

---

### 2. 📄 README.md — Documentation Format

Every lab folder must contain a `README.md` with the following sections:

```markdown
# <Lab Name>

## Overview
A brief description of what this lab covers and what the learner will achieve.

## Network Topology
Describe the network topology — number of routers, switches, PCs, VLANs, etc.

## IP Addressing Table
| Device     | Interface | IP Address     | Subnet Mask     | Gateway       |
|------------|-----------|----------------|-----------------|---------------|
| Router0    | Fa0/0     | 192.168.1.1    | 255.255.255.0   | —             |
| PC0        | NIC       | 192.168.1.10   | 255.255.255.0   | 192.168.1.1   |

## Configuration Steps
Step-by-step CLI commands or configuration instructions.

## Verification Commands
Commands to verify the configuration is working correctly.

## Learning Outcomes
What concepts does this lab reinforce?
```

> ⚠️ Labs submitted **without** proper documentation will not be merged.

---

### 3. 🖼️ Topology Diagram Standards

The topology image must meet the following standards:

```
✅ Format      →  PNG or JPG (PNG preferred for clarity)
✅ Resolution  →  Minimum 800 x 600 pixels
✅ Content     →  Must show all devices, connections, and IP labels
✅ Source      →  Screenshot directly from Cisco Packet Tracer (Logical View)
✅ Naming      →  Must match the folder name exactly
```

---

### 4. 📦 Packet Tracer File Standards

```
✅ Version       →  Saved in Packet Tracer 8.0 or above
✅ Completion    →  All devices must be fully configured and tested
✅ Connectivity  →  All nodes must be able to ping each other (where applicable)
✅ Naming        →  File name must match the folder name
✅ No errors     →  No red/orange connection indicators on final save
```

---

## 🏷️ Naming Conventions

Consistent naming keeps the repository clean and navigable. Follow these rules strictly:

### Folder Naming

```
Format : <number> <Lab Name>
Example: 9 University Network Design
```

- Use the next available number in sequence
- Capitalize each word (Title Case)
- No underscores — use spaces only
- No special characters except hyphens where necessary

### File Naming

```
README.md          →  Always lowercase, always README.md
Image file         →  Must exactly match the folder name  (e.g. 9 University Network Design.png)
Packet Tracer file →  Must exactly match the folder name  (e.g. 9 University Network Design.pkt)
```

### Branch Naming

```
feature/<short-lab-name>         →  New lab addition
fix/<what-is-being-fixed>        →  Bug or config fix
docs/<lab-name-being-updated>    →  Documentation update
improve/<what-is-improved>       →  Enhancement to existing content
```

---

## 🔁 Pull Request Process

When opening a Pull Request, please follow this checklist to speed up the review process:

### PR Title Format

```
[TYPE] Short description of what was added or changed

Examples:
[NEW LAB] University Network Design with OSPF
[FIX] Correct IP addressing in Banking Network Design
[DOCS] Add IP table to Hospital Network README
[IMPROVE] Replace low-res SOHO topology image
```

### PR Description Template

When opening your PR, fill in the following:

```markdown
## What does this PR do?
<!-- Briefly describe what you've added or changed -->

## Type of Contribution
- [ ] New Lab
- [ ] Bug Fix
- [ ] Documentation Update
- [ ] Topology Diagram Improvement
- [ ] Config Improvement

## Lab Details (if new lab)
- Lab Name:
- Lab Number:
- Protocols Used:
- Devices Included:

## Checklist
- [ ] Folder follows naming convention
- [ ] README.md included with all required sections
- [ ] Topology diagram included (PNG/JPG, 800x600+)
- [ ] .pkt file included and saved in Packet Tracer 8.0+
- [ ] All devices ping successfully in the lab
- [ ] No red connection indicators on final save
- [ ] Main README.md updated to include the new lab entry
```

### Review Timeline

| Stage | Expected Time |
|---|---|
| Initial review | Within 3–5 days |
| Feedback / revision requests | Within 2–3 days of response |
| Merge after approval | Within 1–2 days |

> PRs that do not follow the standards above will be labelled `needs-revision` and returned for corrections before review.

---

## 🐛 Issue Reporting

Found a broken link, misconfigured topology, or unclear documentation? Open an issue!

### Issue Types & Labels

| Label | Use When |
|---|---|
| `bug` | A `.pkt` file is broken, a link is dead, or a config doesn't work |
| `documentation` | A README is missing, incomplete, or unclear |
| `enhancement` | You have a suggestion to improve an existing lab |
| `new-lab-request` | You'd like to request a new lab topic |
| `question` | You need help understanding a lab |

### Issue Template

When creating an issue, please include:

```markdown
## Issue Type
<!-- bug / documentation / enhancement / new-lab-request / question -->

## Affected Lab
<!-- Which lab or file is this related to? -->

## Description
<!-- Clear and concise description of the issue -->

## Steps to Reproduce (for bugs)
1. Open file '...'
2. Navigate to '...'
3. See error / issue

## Expected Behavior
<!-- What should happen? -->

## Actual Behavior
<!-- What actually happens? -->

## Screenshots (if applicable)
<!-- Attach screenshots if helpful -->
```

---

## ✅ Contribution Checklist

Before submitting your PR, run through this complete checklist:

```
FOLDER & FILES
──────────────────────────────────────────────────────
[ ] Folder is named correctly  (<number> <Lab Name>)
[ ] README.md is present inside the lab folder
[ ] Topology image (.png or .jpg) is present
[ ] Packet Tracer file (.pkt) is present
[ ] All file names match the folder name exactly

DOCUMENTATION (README.md)
──────────────────────────────────────────────────────
[ ] Overview section is written
[ ] Network topology is described
[ ] IP addressing table is included
[ ] Configuration steps are documented
[ ] Verification commands are listed
[ ] Learning outcomes are stated

PACKET TRACER FILE (.pkt)
──────────────────────────────────────────────────────
[ ] Saved in Packet Tracer 8.0 or above
[ ] All devices are fully configured
[ ] All nodes can communicate (ping test passed)
[ ] No red or orange link indicators
[ ] VLANs, routing, DHCP, etc. verified as applicable

TOPOLOGY IMAGE
──────────────────────────────────────────────────────
[ ] Image is PNG or JPG format
[ ] Resolution is at least 800 x 600 pixels
[ ] All devices and IP labels are visible
[ ] Taken from Packet Tracer Logical View

MAIN REPOSITORY README
──────────────────────────────────────────────────────
[ ] New lab entry added to the main README.md
[ ] View Documentation link is correct
[ ] View Images link is correct
[ ] Download link is correct

PULL REQUEST
──────────────────────────────────────────────────────
[ ] Branch name follows naming convention
[ ] PR title follows [TYPE] format
[ ] PR description template is filled out
[ ] No direct commits to main branch
```

---

## 🙏 Thank You

Every contribution — big or small — helps build a better learning resource for networking students and professionals worldwide. Your efforts are genuinely appreciated.

If you have any questions before contributing, feel free to open a [Discussion](https://github.com/hackThacker/network-practical/issues) or reach out via [hackthacker.blogspot.com](https://hackthacker.blogspot.com).

### Prataince
---

<div align="center">

Made with ❤️ by [hackthacker](https://github.com/hackThacker)

</div>
