# Lab 06 – Windows Authentication Monitoring and Event Analysis with Wazuh SIEM

## Overview

This lab demonstrates the deployment and configuration of **Wazuh SIEM** to monitor Windows authentication events and analyze security-related activity from a connected endpoint.

A Windows endpoint was integrated with a Wazuh server running on Ubuntu Server. Controlled authentication attempts were generated to verify that failed and successful login events were collected, classified, and displayed in the Wazuh Dashboard.

The exercise simulates a basic SOC Level 1 (SOC L1) monitoring and investigation workflow.

## Objectives

- Deploy and configure a Wazuh SIEM environment.
- Install and connect a Windows endpoint using the Wazuh Agent.
- Monitor Windows authentication events.
- Identify and analyze failed login attempts.
- Verify successful authentication events.
- Review security alerts and event details in Wazuh Threat Hunting.

## Lab Environment

| Component | Description |
|---|---|
| SIEM Platform | Wazuh 4.14.8 |
| Server Operating System | Ubuntu Server 24.04 LTS |
| Endpoint Operating System | Windows |
| Endpoint Agent | Wazuh Agent 4.14.8 |
| Virtualization | Oracle VirtualBox |
| Log Analysis | Windows Event Viewer |
| Additional Tools | PowerShell |

## Lab Architecture

The environment consisted of a Wazuh server running on an Ubuntu Server virtual machine and a Windows endpoint connected through the Wazuh Agent.

The Windows endpoint was registered with the Wazuh Manager and configured to forward security events for centralized monitoring and analysis.

**Main communication ports:**

- TCP 1514: Agent communication with the Wazuh Manager.
- TCP 1515: Agent registration and enrollment.
- TCP 55000: Wazuh API.

## Lab Procedure

### 1. Wazuh Deployment

Installed and configured the Wazuh components:

- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard

Verified that the services were running and that the dashboard was accessible.

### 2. Windows Endpoint Integration

Installed the Wazuh Agent on the Windows endpoint and configured its connection to the Wazuh Manager.

The endpoint was successfully registered with the following identification:

- **Agent name:** JUANP
- **Agent ID:** 001
- **Agent status:** Active

### 3. Authentication Event Simulation

Performed controlled authentication tests on the Windows endpoint:

1. Generated three failed login attempts using an incorrect password.
2. Performed a successful login afterward.
3. Reviewed the corresponding events in Windows Event Viewer.
4. Verified that Wazuh received and classified the events.

## Detection and Analysis

### 1. Failed Login Attempts

Windows Event Viewer recorded three failed authentication events.

| Field | Value |
|---|---|
| Windows Event ID | 4625 |
| Wazuh Rule ID | 60122 |
| Rule Description | Logon Failure - Unknown user or bad password |
| Alert Level | 5 |
| Logon Type | 2 |
| Status | 0xc000006d |
| SubStatus | 0xc000006a |
| Source IP | 127.0.0.1 |

The SubStatus value `0xc000006a` indicates an incorrect password.

Wazuh successfully collected and classified the three failed authentication events.

### 2. Successful Login

After the failed attempts, a successful authentication event was recorded.

| Field | Value |
|---|---|
| Windows Event ID | 4624 |
| Target User | Juan Pablo González |
| Logon Type | 2 |
| Source IP | 127.0.0.1 |
| Process | C:\Windows\System32\svchost.exe |

This event confirmed that the endpoint also generated successful authentication logs.

### 3. Wazuh Alert Verification

The Wazuh Threat Hunting interface was used to review the collected events.

**Failed authentication:**

- Rule ID: `60122`
- Description: `Logon Failure - Unknown user or bad password`
- Alert Level: `5`

**Successful authentication:**

- Rule ID: `60118`
- Description: `Windows Workstation Logon Success`
- Alert Level: `3`

The event details were reviewed to validate the endpoint, username, authentication type, and related fields.

## Key Findings

- The Windows endpoint was successfully integrated with Wazuh.
- The SIEM collected Windows authentication events.
- Three failed login attempts were identified and classified.
- A subsequent successful login event was also collected.
- Wazuh provided event details that supported the analysis and verification process.

The activity was generated intentionally in a controlled lab environment. It was not a real-world attack or confirmed security incident.

## Skills Demonstrated

- SIEM deployment and configuration.
- Endpoint monitoring and agent integration.
- Windows Security Event Log analysis.
- Authentication event investigation.
- Security alert identification and classification.
- Basic event correlation.
- SOC L1 monitoring and documentation.

## Evidence

Screenshots and supporting evidence are available in the `evidence/` directory.

| File | Description |
|---|---|
| `01-wazuh-dashboard.png` | Wazuh Dashboard |
| `02-wazuh-agent-active.png` | Active Windows endpoint |
| `03-windows-event-4625.png` | Failed authentication events in Event Viewer |
| `04-windows-event-4624.png` | Successful authentication event details |
| `05-wazuh-failed-logins.png` | Failed login alerts in Wazuh |
| `06-wazuh-successful-login.png` | Successful login alerts in Wazuh |

## Conclusion

This lab provided practical experience deploying a SIEM platform, integrating a Windows endpoint, and investigating authentication events.

It demonstrated how Wazuh can centralize endpoint security logs, identify failed and successful authentication activity, and provide relevant information for an initial security investigation.

These activities represent fundamental tasks performed by a **SOC Level 1 Analyst** during security monitoring and event triage.
