# Microsoft-AI-Fest-1
Review a cybersecurity scenario at Litware Inc. Then work through key phases, like understanding the context, evaluating threats, proposing a solution architecture, and reviewing implementation steps. Throughout this journey, you'll make decisions aligned with Zero Trust principles to address business and security needs

Litware Inc. is a robotics and automation supplier for the automotive industry, operating over 40 global sites with cloud and IOT-driven manufacturing processes. Litware runs a hybrid IT environment(Hybrid microsoft cloud with regional autonomy) with microsoft azure supporting engineering collaberation. Regional teams independently manage Azure subscription, creating inconsistent security configurations across business units. Factory production system includes Windows 11 workstations, Red hat Linux gateways, IoT sensors, and Ubuntu edge computing nodes taht processes machine data locally before sending insights to Azure. Operational and ioT devices share networks to engeneering stations for efficiency, but many legacy systems use default configurations and proprietary protocols, operating well beyond their service life. Local IT teams manage devices using different tools and schedules, leading to inconsistent patching and delayed vulnerabilities remediation. Recent incidents involved unpatched engineering laptops remaining exposed for weeks. Infrastructure services like SQL Server databases are migrating to Azure VMs, while security telemetry is processed regionally using varied security information and event management tools. As the security architect, i must analyse litweare's current posture and design a comprehensive zero trust architecture that secures endpoints and infrastructure security while maintaining operational continuity.



# Step 1: Identify the Major Security Risks

## Risk 1: Inconsistent Azure Security Governance

### Evidence

> Regional teams independently manage Azure subscriptions

### Problem

* Different security baselines
* Different policies
* Different compliance standards
* Security drift across regions

### Zero Trust Principle

✅ **Assume Breach**

Need centralized governance and policy enforcement.

### Likely Microsoft Solutions

* Azure Policy
* Management Groups
* Defender for Cloud
* Azure Arc

---

## Risk 2: Mixed IT + OT Networks

### Evidence

> Operational and IoT devices share networks with engineering stations

### Problem

If an engineering laptop is compromised:

```text
Laptop
 ↓
Engineering Network
 ↓
OT Network
 ↓
Production Systems
```

This creates lateral movement opportunities.

### Zero Trust Principle

✅ **Assume Breach**

### Likely Solutions

* Network Segmentation
* Zero Trust Network Access
* Microsegmentation
* Defender for IoT

---

## Risk 3: Legacy Industrial Systems

### Evidence

> Default configurations
> Proprietary protocols
> Beyond service life

### Problem

Legacy systems often:

* Cannot run modern security agents
* Cannot be patched
* Use insecure protocols

### Zero Trust Principle

✅ **Assume Breach**
✅ **Least Privilege**

### Likely Solutions

* Defender for IoT
* Network Isolation
* Compensating Controls
* Continuous Monitoring

---

## Risk 4: Inconsistent Patching

### Evidence

> Different tools and schedules

> Unpatched engineering laptops exposed for weeks

### Problem

This is a major endpoint security gap.

### Zero Trust Principle

✅ **Verify Explicitly**

Device health should influence access.

### Likely Solutions

* Microsoft Intune
* Defender for Endpoint
* Conditional Access
* Update Management

---

## Risk 5: Hybrid Infrastructure

### Evidence

> SQL Server databases migrating to Azure VMs

### Problem

Need consistent visibility across:

* On-prem servers
* Azure VMs
* Linux systems
* Windows systems

### Likely Solutions

* Defender for Cloud
* Azure Arc
* Microsoft Sentinel

---

## Risk 6: Fragmented Security Monitoring

### Evidence

> Security telemetry processed regionally using varied SIEM tools

### Problem

No centralized SOC visibility.

```text
Region A → SIEM A
Region B → SIEM B
Region C → SIEM C
```

Threats may go unnoticed.

### Zero Trust Principle

✅ **Assume Breach**

### Likely Solutions

* Microsoft Sentinel
* Defender XDR
* Azure Monitor

---

# Step 2: Asset Inventory

Microsoft often asks which assets require protection.

### User Devices

* Windows 11 engineering workstations
* Engineering laptops

### Servers

* Azure VMs
* SQL Server

### Linux Systems

* Ubuntu edge nodes
* Red Hat gateways

### IoT / OT

* Industrial sensors
* Factory equipment
* Manufacturing controllers

### Cloud Resources

* Azure subscriptions
* Azure collaboration services

---

# Step 3: Expected Microsoft Security Stack

Based on the scenario, I would expect these technologies to appear repeatedly:

| Security Area            | Likely Solution               |
| ------------------------ | ----------------------------- |
| Azure governance         | Azure Policy                  |
| Hybrid management        | Azure Arc                     |
| Endpoint protection      | Defender for Endpoint         |
| IoT security             | Defender for IoT              |
| Cloud posture management | Defender for Cloud            |
| Device compliance        | Intune                        |
| Identity security        | Entra ID + Conditional Access |
| Monitoring               | Sentinel                      |
| Incident response        | Defender XDR                  |
| Privileged access        | PIM                           |

---

# Step 4: Zero Trust Mapping

| Zero Trust Principle | Litware Example                            |
| -------------------- | ------------------------------------------ |
| Verify Explicitly    | Conditional Access + Device Compliance     |
| Least Privilege      | PIM + RBAC                                 |
| Assume Breach        | Network Segmentation + Sentinel + Defender |

---

## My Prediction

The first Litware questions will likely focus on:

1. **How to centrally govern Azure subscriptions**

   * Azure Policy
   * Management Groups
   * Defender for Cloud

2. **How to secure IoT and OT environments**

   * Defender for IoT
   * Network Segmentation

3. **How to address unpatched engineering laptops**

   * Intune
   * Defender for Endpoint
   * Conditional Access

4. **How to consolidate regional monitoring**

   * Microsoft Sentinel
   * Defender XDR





### Thanks for sharing the architecture diagram. This confirms many of the risks we identified from the text.

## What the Current Architecture Shows

### 1. Multiple Independent Plants and Facilities

On the left side:

* Litware Plant
* Litware Facility

Each site contains:

* Windows engineering workstations
* Linux systems
* SQL servers
* IoT devices
* Cameras and sensors
* Edge computing nodes
* Local SIEM instances

This indicates **regional autonomy** and decentralized management.

---

### 2. Separate Local Monitoring

Notice:

* One SIEM at the plant
* Another SIEM at the facility

This matches the scenario statement:

> "security telemetry is processed regionally using varied SIEM tools"

### Security Risk

```text
Plant SIEM
     ↘
      (not correlated)
     ↗
Facility SIEM
```

An attack spanning multiple sites may never be detected as a single incident.

### Likely Future Solution

✅ Microsoft Sentinel

to centralize monitoring.

---

### 3. Edge Computing Nodes

The architecture shows several:

* Ubuntu edge nodes
* Industrial gateways
* Local processing before Azure upload

### Security Risk

Edge devices often:

* Run outdated Linux
* Receive infrequent patches
* Operate in remote locations

### Likely Future Solution

✅ Azure Arc

✅ Defender for Cloud

✅ Defender for IoT

---

### 4. Mixed IT and OT Environment

The plant network contains:

* User workstations
* IoT sensors
* Industrial devices
* Cameras
* Production systems

all connected through the same local infrastructure.

### Security Risk

```text
Compromised Laptop
        ↓
Plant Network
        ↓
IoT Device
        ↓
Production System
```

This violates Zero Trust segmentation principles.

### Likely Future Solution

✅ Defender for IoT

✅ Network Segmentation

---

### 5. Entra Tenant Is Centralized

On the right side:

Litware has:

* Entra Tenant
* Enterprise Applications
* App Registrations
* User Portal
* MFA

This is good because identity is already centralized.

### Security Observation

Identity exists centrally, but device and infrastructure management appear decentralized.

Microsoft will likely strengthen:

* Conditional Access
* Device Compliance
* PIM
* Defender XDR

---

### 6. Azure VMs Across Multiple Subscriptions

The diagram shows:

* Plant Azure Subscription
* Facility Azure Subscription
* Azure VMs

### Security Risk

Different subscriptions may have:

* Different security baselines
* Different policies
* Different monitoring settings

### Likely Future Solution

✅ Azure Policy

✅ Management Groups

✅ Defender for Cloud

---

### 7. Azure Log Analytics Is Already Present

Interesting observation:

The architecture already sends some telemetry to Azure Log Analytics.

```text
Sites
   ↓
Azure Log Analytics
```

This suggests Microsoft may ask:

> How do we move from Log Analytics to a centralized SOC?

Expected answer:

✅ Microsoft Sentinel

because Sentinel sits on top of Log Analytics and adds:

* SIEM
* SOAR
* Analytics rules
* Incident correlation
* Playbooks

---

## Zero Trust Gaps Visible in the Diagram

| Area              | Current State             | Risk            |
| ----------------- | ------------------------- | --------------- |
| Identity          | MFA exists                | Good foundation |
| Devices           | Managed inconsistently    | High            |
| Patching          | Regional processes        | High            |
| IoT               | Mixed with user devices   | High            |
| Monitoring        | Multiple SIEMs            | High            |
| Azure Governance  | Independent subscriptions | Medium-High     |
| Linux/Edge Nodes  | Decentralized management  | High            |
| Incident Response | Regional visibility       | High            |

---

## My Prediction for the Upcoming Litware Questions

Based on the architecture, Microsoft will probably ask you to choose solutions for:

1. **Centralized governance across subscriptions**

   * Azure Policy
   * Management Groups
   * Defender for Cloud

2. **Unpatched Windows and Linux devices**

   * Defender for Endpoint
   * Intune
   * Azure Arc

3. **IoT and OT protection**

   * Defender for IoT

4. **Centralized monitoring**

   * Microsoft Sentinel

5. **Hybrid/edge device management**

   * Azure Arc

6. **Incident detection and response**

   * Defender XDR












### 1st Risk AREA

This hotspot identifies the **Endpoint Security and Management** risk area.

Let's map each bullet point to the underlying Zero Trust problem.

| Risk Identified                                       | Zero Trust Issue                   | Likely Microsoft Solution              |
| ----------------------------------------------------- | ---------------------------------- | -------------------------------------- |
| Inconsistent patching schedules                       | Devices not managed uniformly      | Microsoft Intune                       |
| Laptops connect without health verification           | No device trust validation         | Conditional Access + Device Compliance |
| Vulnerability remediation delays                      | No centralized endpoint management | Intune + Defender for Endpoint         |
| Unpatched laptops exposed for weeks                   | Weak vulnerability management      | Defender Vulnerability Management      |
| Limited integration between detection and remediation | Siloed security operations         | Defender XDR                           |

---

## What Microsoft is really testing

The scenario contains two major weaknesses:

### 1. Device Compliance Gap

Current state:

```text
User
 ↓
Laptop
 ↓
Network Access
```

No verification of:

* Patch level
* Antivirus status
* Device health
* Compliance state

Zero Trust requires:

```text
User
 ↓
Device Compliance Check
 ↓
Conditional Access
 ↓
Resource Access
```

Expected technologies:

✅ Microsoft Intune
✅ Microsoft Entra Conditional Access

---

### 2. Endpoint Detection and Response Gap

Current state:

```text
Threat Detected
      ↓
Manual Investigation
      ↓
Manual Remediation
```

Microsoft prefers:

```text
Defender for Endpoint
          ↓
Defender XDR
          ↓
Automated Investigation
          ↓
Automated Remediation
```

Expected technologies:

✅ Microsoft Defender for Endpoint
✅ Microsoft Defender XDR

---

## Likely Future Design Recommendation

If Microsoft asks how to solve this risk area, the architecture will probably include:

### Device Management

* Microsoft Intune
* Device Compliance Policies
* Automated Patch Management

### Endpoint Protection

* Microsoft Defender for Endpoint
* Vulnerability Management
* Attack Surface Reduction

### Access Control

* Entra ID Conditional Access
* Require compliant devices
* Require MFA

### Detection & Response

* Defender XDR
* Automated investigation and remediation

---

## Exam Cheat Sheet

When you see:

> Unpatched laptops

Think:
✅ Intune
✅ Defender Vulnerability Management

When you see:

> Devices connect without health verification

Think:
✅ Conditional Access
✅ Device Compliance Policies

When you see:

> Threat detection not linked to remediation

Think:
✅ Defender for Endpoint
✅ Defender XDR

This hotspot is essentially a textbook **"Verify Explicitly"** Zero Trust scenario: *don't trust a laptop merely because it can connect; verify that it is healthy, compliant, and continuously monitored before granting access.*











### 2nd Key Risk Area

This hotspot is the **OT (Operational Technology) and IoT Infrastructure** risk area.

Unlike the previous hotspot (which focused on endpoint management), this one is primarily testing the Zero Trust principle:

> **Assume Breach**

Microsoft assumes that one compromised device should **not** be able to move freely across the manufacturing environment.

---

## Risk Analysis

### 1. OT and IoT share networks with IT assets

**Problem**

```text
Engineering Laptop
        ↓
Shared Network
        ↓
IoT Sensor
        ↓
PLC / Controller
        ↓
Production Line
```

If an attacker compromises a laptop, they can potentially move laterally into industrial systems.

### Zero Trust Violation

No segmentation between:

* IT assets
* OT assets
* IoT devices

### Likely Solution

✅ Defender for IoT
✅ Network Segmentation
✅ Microsegmentation

---

### 2. Legacy Firmware and Default Credentials

The scenario states:

> static firmware, legacy protocols, and default credentials

These are common OT security problems because:

* Devices cannot easily be patched
* Many run proprietary protocols
* Some cannot support modern security agents

### Likely Solution

✅ Defender for IoT

Defender for IoT can:

* Discover unmanaged devices
* Identify default credentials
* Detect vulnerable firmware
* Monitor industrial protocols

---

### 3. Limited Visibility into Controllers

The scenario says:

> difficult to verify software integrity

This means the organization lacks asset inventory and monitoring.

### Risk

Unknown devices become blind spots.

### Likely Solution

✅ Defender for IoT

Key capability:

```text
Discover
Identify
Classify
Monitor
Alert
```

for industrial assets.

---

### 4. Edge Nodes Bypass Security Controls

Current state:

```text
Edge Node
      ↓
Azure
```

Telemetry bypasses centralized inspection.

### Risk

* Malicious activity may go undetected
* Security controls are inconsistent
* Monitoring gaps exist

### Likely Solution

✅ Centralized monitoring through:

* Microsoft Sentinel
* Defender for IoT
* Defender XDR (where applicable)

---

### 5. Lack of IoT Identity and Access Controls

The scenario says:

> limits identity, filtering, and access control

This is a major Zero Trust issue.

Current model:

```text
Device connects
      ↓
Trusted automatically
```

Zero Trust model:

```text
Device identity verified
      ↓
Access controlled
      ↓
Traffic filtered
      ↓
Continuous monitoring
```

---

# Expected Microsoft Security Stack

If a future question asks how to address this risk area, the strongest answers will likely be:

| Requirement                | Solution             |
| -------------------------- | -------------------- |
| OT/IoT visibility          | Defender for IoT     |
| Industrial asset discovery | Defender for IoT     |
| Legacy protocol monitoring | Defender for IoT     |
| Lateral movement reduction | Network Segmentation |
| Centralized monitoring     | Microsoft Sentinel   |
| Threat correlation         | Defender XDR         |
| Hybrid/edge governance     | Azure Arc            |

---

# Exam Shortcut

When you see:

* Industrial devices
* Sensors
* Controllers
* PLCs
* Manufacturing systems
* Legacy protocols
* OT/IoT environments

Think:

✅ **Microsoft Defender for IoT**

When you see:

* Shared IT and OT networks
* Lateral movement

Think:

✅ **Segmentation / Zero Trust Network Design**

This hotspot is essentially telling us:

> "Litware's factory floor is highly connected but insufficiently segmented and monitored."

Microsoft's primary answer for that type of risk is usually **Defender for IoT**, supported by **network segmentation** and **centralized monitoring through Sentinel**.















### 3rd Key Risk Area

This third hotspot is about **Infrastructure Operations and Visibility**.

If Hotspot 1 was about **devices** and Hotspot 2 was about **OT/IoT**, then Hotspot 3 is about **governance, telemetry, and centralized security operations**.

---

# Risk Analysis

## 1. Independent Azure Provisioning

> Azure provisioned independently by regions

### Problem

```text
Region A Azure Subscription
Region B Azure Subscription
Region C Azure Subscription
```

Each region may have:

* Different security settings
* Different RBAC assignments
* Different monitoring configurations
* Different compliance baselines

This creates **configuration drift**.

### Likely Microsoft Solution

✅ Azure Management Groups

✅ Azure Policy

✅ Defender for Cloud

---

## 2. Regional Telemetry Processing

> Regional telemetry processing reduces multi-site anomaly detection and unified alert correlation

### Current State

```text
Plant A → Local SIEM
Plant B → Local SIEM
Plant C → Local SIEM
```

### Problem

An attacker moving between sites may appear as unrelated events.

### Likely Solution

✅ Microsoft Sentinel

Sentinel enables:

```text
All Regions
      ↓
Microsoft Sentinel
      ↓
Correlated Incidents
      ↓
Unified Investigation
```

---

## 3. No Intermediary Telemetry Control

> Lack of intermediary telemetry control limits routing and policy enforcement

### Meaning

Telemetry flows directly from systems to destinations without centralized governance.

This limits:

* Filtering
* Inspection
* Routing policies
* Data governance

### Likely Solution

✅ Azure Monitor

✅ Log Analytics

✅ Sentinel

---

## 4. Public Endpoints for Edge Telemetry

> Edge telemetry uses public endpoints

### Current Risk

```text
Edge Node
    ↓
Internet
    ↓
Azure
```

This increases:

* Internet exposure
* Attack surface
* Data interception opportunities

### Likely Solution

✅ Private Endpoints

✅ Private Link

✅ VPN Connectivity

---

## 5. No Azure Management Groups

The scenario explicitly says:

> Fragmented policy enforcement without Azure Management Groups

This is a huge clue.

Microsoft is practically telling you one answer already.

### Azure Management Groups

Provide:

```text
Management Group
        ↓
Subscriptions
        ↓
Resource Groups
        ↓
Resources
```

Benefits:

* Centralized governance
* Consistent policy assignment
* Standardized compliance controls

---

# Zero Trust Principles Involved

### Verify Explicitly

* Consistent policy enforcement
* Centralized governance

### Assume Breach

* Unified monitoring
* Cross-region threat detection
* Centralized incident correlation

---

# Expected Microsoft Technologies

| Problem                      | Solution                         |
| ---------------------------- | -------------------------------- |
| Multiple Azure subscriptions | Azure Management Groups          |
| Configuration drift          | Azure Policy                     |
| Weak cloud posture           | Defender for Cloud               |
| Fragmented monitoring        | Microsoft Sentinel               |
| Multi-site correlation       | Sentinel                         |
| Public telemetry paths       | Private Link / Private Endpoints |
| Central log storage          | Log Analytics                    |

---

# Exam Cheat Sheet

When you see:

> Independent Azure subscriptions

Think:

✅ Azure Management Groups

---

When you see:

> Inconsistent configurations

Think:

✅ Azure Policy

---

When you see:

> Multiple SIEMs

Think:

✅ Microsoft Sentinel

---

When you see:

> Regional telemetry processing

Think:

✅ Centralized monitoring with Sentinel

---

When you see:

> Public telemetry endpoints

Think:

✅ Private Link / Private Endpoints

---

### Likely Final Architecture for Litware

If Microsoft asks you to redesign this area, expect something close to:

```text
Azure Management Groups
          ↓
Azure Policy
          ↓
Defender for Cloud
          ↓
Azure Monitor / Log Analytics
          ↓
Microsoft Sentinel
          ↓
Automated Response
```

This would solve nearly every risk listed in this hotspot:

* Governance
* Visibility
* Correlation
* Policy enforcement
* Centralized operations
* Reduced internet exposure.













### 4rth Key risk area


This fourth hotspot is about **Security Operations (SecOps)**, and it's really testing Microsoft's vision of a **modern SOC (Security Operations Center)**.

---

# Risk Analysis

## 1. Telemetry Integration Gaps

> Security operations centralized but limited by telemetry integration gaps and inconsistent data formats.

### Current State

```text
Windows Logs
Linux Logs
IoT Logs
SIEM Data
Azure Logs
     ↓
Different Formats
     ↓
Difficult Correlation
```

### Problem

The SOC cannot easily correlate events across:

* Endpoints
* Servers
* IoT devices
* Cloud resources
* Identities

### Likely Solution

✅ Microsoft Sentinel

Sentinel normalizes and correlates data from many sources.

---

## 2. Ad Hoc Local Tooling

> Local teams use ad hoc tooling

### Problem

Different sites use:

* Different workflows
* Different response methods
* Different priorities

This causes:

* Slow investigations
* Inconsistent response quality
* Duplicate work

### Likely Solution

✅ Microsoft Sentinel Playbooks

✅ Defender XDR

---

## 3. No Unified View Across Devices

> Lack of unified view across devices impedes AI-driven threat detection

This is a huge clue.

Microsoft's AI-driven detection depends on seeing the entire attack chain.

### Current State

```text
Endpoint Alert
      X
Identity Alert
      X
Cloud Alert
```

Separate investigations.

### Desired State

```text
Identity
Endpoint
Cloud
IoT
Email
     ↓
Defender XDR
     ↓
Single Incident
```

### Likely Solution

✅ Microsoft Defender XDR

---

## 4. Skill Gaps in Security Teams

> Teams unsure how to act on threat intelligence

### Problem

Organizations often have:

* Too many alerts
* Too few analysts
* Limited expertise

### Microsoft's Approach

Use:

* AI-assisted investigations
* Automated response
* Threat intelligence correlation
* Playbooks

### Likely Solutions

✅ Microsoft Sentinel

✅ Defender XDR

---

# What Microsoft Is Testing

This hotspot combines two major Microsoft security platforms:

### Defender XDR

Provides:

* Unified incidents
* AI correlation
* Cross-domain visibility

Sources:

* Endpoints
* Identity
* Email
* Applications
* Cloud workloads

---

### Microsoft Sentinel

Provides:

* SIEM
* SOAR
* Automation
* Playbooks
* Threat hunting

---

# Likely Target Architecture

```text
Defender for Endpoint
Defender for Identity
Defender for Cloud
Defender for IoT
Entra ID
Azure Logs
      ↓
Microsoft Defender XDR
      ↓
Microsoft Sentinel
      ↓
Analytics Rules
      ↓
Automated Playbooks
      ↓
Incident Response
```

---

# Zero Trust Principle

This hotspot is mostly:

✅ **Assume Breach**

Because the organization assumes attacks will occur and focuses on:

* Detecting them quickly
* Correlating signals
* Automating response
* Reducing attacker dwell time

---

# Litware Hotspot Summary

| Hotspot                                | Main Microsoft Solutions                          |
| -------------------------------------- | ------------------------------------------------- |
| Endpoint Security & Management         | Intune, Defender for Endpoint, Conditional Access |
| OT & IoT Infrastructure                | Defender for IoT, Segmentation                    |
| Infrastructure Operations & Visibility | Azure Management Groups, Azure Policy, Sentinel   |
| Security Operations (SecOps)           | Defender XDR, Microsoft Sentinel                  |

At this point, Microsoft has effectively revealed the four pillars of the future Litware architecture. The next phase will likely ask you to map specific Microsoft solutions to each of these risk areas, similar to the Fabrikam exercise.
















































