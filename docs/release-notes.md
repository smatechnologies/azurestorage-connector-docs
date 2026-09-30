---
sidebar_label: 'Release notes'
title: Azure Storage Connector release notes
description: "Version history and change details for the Azure Storage Connector, including new features, improvements, and bug fixes."
tags:
  - Reference
  - System Administrator
  - Installation
---

# Azure Storage Connector release notes

## 26

### 26.0.0

The AzureStotage connector has been moved from the SMA Innovation Lab to fully supported within the Continuous Connector environment.
Release 26.0.0 is the first release that must be downloaded from the Continuous FTP Site under **/OpCon Releases/Connectors/AzureStorage**. 


This version should be used in conjunction with the ACS AzureStorage Solution Manager add on. 

2026 September

#### What's new

:eight_spoked_asterisk: All libraries upgraded to latest versions and support for Java 11. 

### 2.0.2

2024 December

#### What's new

:eight_spoked_asterisk: Added support for ACS (Azure Container Storage) Azure Storage job subtype in Solution Manager, providing a new interface for configuring Azure Storage jobs.

:eight_spoked_asterisk: Updated connector to use Azure Java SDK for all Azure Storage interactions, improving reliability and compatibility with current Azure APIs.

#### Why this matters

The new ACS Azure Storage job subtype in Solution Manager gives operations teams a modern, browser-based interface for configuring Azure Storage jobs without requiring access to Enterprise Manager. Updating to the Azure Java SDK ensures the connector stays compatible with current Azure APIs and security requirements.

### 2.0.1

2023 July

#### Fixes

:eight_spoked_asterisk: Fixed an issue where file arrival monitoring did not correctly detect file completion when static file size time was set to values other than the default.

:eight_spoked_asterisk: Fixed an issue where wildcard patterns for container delete and container list tasks did not match containers with certain naming conventions.

### 2.0.0

2022 January

#### What's new

:eight_spoked_asterisk: Rewrote connector using the Azure Java SDK, replacing the previous REST API implementation for improved reliability and Azure API compatibility.

:eight_spoked_asterisk: Added support for the AzureStorage job subtype in Enterprise Manager, providing a graphical interface for configuring Azure Storage tasks.

:eight_spoked_asterisk: Added the file arrival task, which monitors a container for the arrival of a specified file and waits up to a configurable timeout.

:eight_spoked_asterisk: Added support for folder paths within containers for file delete, file download, file list, and file upload tasks.

#### Why this matters

Version 2.0.0 replaces the previous REST-based implementation with the Azure Java SDK, improving compatibility with Azure's evolving APIs. The new Enterprise Manager job subtype eliminates the need to construct command-line arguments manually. File arrival monitoring enables event-driven automation that responds to file delivery rather than running on a fixed schedule.
