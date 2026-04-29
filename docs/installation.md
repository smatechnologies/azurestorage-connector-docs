---
title: Installation
sidebar_label: Installation
description: "Install and configure the Azure Storage Connector on a Windows agent so OpCon can schedule Azure Storage tasks."
tags:
  - Procedural
  - System Administrator
  - Installation
---

# Installation

**Theme:** Configure | **Audience:** System Administrators

## What is it?

The Azure Storage Connector is a Windows-based executable that OpCon calls when running Azure Storage jobs. Installing the connector involves extracting the distribution archive, registering the job subtype in Enterprise Manager, and creating the required global properties in OpCon.

- Required before any Azure Storage jobs can run in OpCon
- Must be installed on each Windows agent that will execute Azure Storage jobs
- Enterprise Manager must be restarted after the plugin is placed in the `dropins` directory

## How to implement it

### Prerequisites

- A Windows agent with network access to the Azure Storage account
- Java 11 (included in the connector distribution — no separate installation required)
- Administrative access to Enterprise Manager and OpCon

### Step 1 — Download and extract

To install the connector, complete the following steps:

1. Download `AzureStorage_Windows.zip` from the [Azure Storage Connector releases page](https://github.com/SMATechnologies/azure-storage-java/releases).
2. Extract the zip file to the installation directory on the Windows agent. All required files are located under the root folder of the extracted directory.

### Step 2 — Install the Enterprise Manager plugin

To register the job subtype in Enterprise Manager, complete the following steps:

1. Copy the Enterprise Manager job subtype file from the `/emplugins` directory in the extracted folder.
2. Paste the file into the `dropins` directory of each Enterprise Manager installation that will create Azure Storage job definitions. Create the `dropins` directory if it does not exist.
3. Restart Enterprise Manager. The **Azure Storage** Windows job subtype is displayed in the job type list.

**NOTE:** If the job subtype does not appear after restarting, right-click Enterprise Manager in the taskbar and select **Run as Administrator**, then restart again.

### Step 3 — Create global properties in OpCon

To configure the required global properties, complete the following steps:

1. In OpCon, create a global property named `AzureStoragePath` and set its value to the full path of the connector installation directory (for example, `C:\ConnectorFiles\AzureStorage`).
2. Create an encrypted global property to store the Azure Storage connection string (access key). Reference this property in job definitions using the `[[property_name]]` token syntax.

## Configuration options

The connector reads its configuration from the `Connector.config` file in the installation directory.

| Setting | What it does | Default | Notes |
|---|---|---|---|
| `NAME` | Display name for the connector instance | `Azure Storage Connector` | Informational only |
| `DEBUG` | Enables detailed debug logging | `OFF` | Set to `ON` to write verbose log output for troubleshooting |

**Connector.config example:**

```
[CONNECTOR]
NAME=Azure Storage Connector
DEBUG=OFF
```

## Exception handling

**Enterprise Manager shows no Azure Storage job subtype after restart**
The plugin file may not be in the correct `dropins` directory, or Enterprise Manager may not have restarted with sufficient permissions. Verify the file is in the `dropins` folder and restart Enterprise Manager using **Run as Administrator**.

**Connector fails with a Java error on startup**
The embedded Java Runtime Environment in the connector distribution may not be accessible. Verify that the connector was fully extracted and that the `/java` directory exists under the installation root.

**Job fails with exit code 1 and no detailed error**
Enable debug logging by setting `DEBUG=ON` in `Connector.config`, run the job again, and review the log file in the installation directory for the specific error message.

## Administration

- To enable or disable the connector, stop or start the OpCon job that calls it — the connector has no persistent service of its own
- Only System Administrators with access to the OpCon Library and the Windows agent file system can install or update the connector
- To update the connector, extract the new version over the existing installation directory and restart any open Enterprise Manager instances

## Security considerations

- **Authentication:** The connector authenticates to Azure using the connection string (access key) passed at job runtime. Store the access key as an encrypted global property in OpCon.
- **Authorization:** Only OpCon users with access to the job definition can view or modify the connection string property reference. The underlying encrypted value is not exposed in the OpCon interface.
- **Data security:** The connection string is passed as a command-line argument to the connector process. Use encrypted global properties to prevent the key from appearing in plain text in OpCon logs.
- **Sensitive data:** The Azure Storage access key grants full access to the storage account. Rotate keys according to your organization's credential management policy.

## Operations

- **Monitoring:** OpCon reports job status (Finished OK or Failed) and the connector exit code for each Azure Storage job. Review the job output in Solution Manager or Enterprise Manager for task-specific results.
- **Alerts:** Configure OpCon events or notifications on job failure to alert operations staff when an Azure Storage task does not complete successfully.
- **Performance and scaling:** The connector is a single-process executable with no built-in concurrency limits. If high volumes of concurrent Azure Storage jobs are required, distribute them across multiple Windows agents.

## FAQs

**Do I need to install the connector on every agent?**

Yes. The connector executable must be present on each Windows agent that runs Azure Storage jobs. The `AzureStoragePath` global property must point to the correct installation path for each agent.

**Can I use the same connector installation for multiple storage accounts?**

Yes. The storage account name and access key are provided at the job level, not in the connector configuration. A single connector installation can access multiple storage accounts.

**Does the connector run as a Windows service?**

No. The connector is a command-line executable that OpCon starts on demand for each job. It exits after the task completes.

**What happens if Connector.config is missing?**

The connector uses built-in defaults for all settings. The `Connector.config` file is only required if you need to change the connector name or enable debug logging.

## Glossary

**AzureStoragePath** — A global property in OpCon that stores the full path to the Azure Storage Connector installation directory. Required for constructing the connector command line in job definitions.

**Dropins directory** — A folder within the Enterprise Manager installation where job subtype plugin files are placed. Enterprise Manager scans this folder at startup to register available job subtypes.

**Encrypted global property** — A global property in OpCon whose value is stored in an encrypted format. The decrypted value is passed to jobs at runtime but is never displayed in plain text in the OpCon interface.

**Connector.config** — The configuration file for the Azure Storage Connector. Located in the connector installation directory. Controls the connector display name and debug logging behavior.
