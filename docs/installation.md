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

## What is it?

The Azure Storage Connector is a Windows-based executable that OpCon calls when running Azure Storage jobs. Installing the connector involves extracting the distribution archive, registering the job subtype in Enterprise Manager, and creating the required global properties in OpCon.

To access the connector from Solution Manager, the ACS AzureStorage capability can be installed. This capability is a wrapper for the existing Azure Storage Connector providing Solution Manager screens for managing the connector.  

- Required before any Azure Storage jobs can run in OpCon
- Must be installed on each Windows agent that will run Azure Storage jobs
- Enterprise Manager must be restarted after the plugin is placed in the `dropins` directory
- Solution Manager implementation only works with Windows Azure Storage Connector installations when connector is installed on the OpCon or Relay server 

## How to implement it

### Prerequisites

- A Windows agent with network access to the Azure Storage account
- Java 11 (included in the connector distribution — no separate installation required)
- Administrative access to Enterprise Manager and OpCon
- Solution Manager Support requires OpCon Cloud or OpCon DataCenter 26.0.4 or greater

### Step 1 — Download and extract

To install the connector, complete the following steps:

1. Download `AzureStorage_Windows.zip` from the [Azure Storage Connector releases page](https://github.com/SMATechnologies/azure-storage-java/releases).
2. Extract the zip file to the installation directory on the Windows agent. All required files are located under the root folder of the extracted directory.

To install Solution Manager support, complete the following steps:

1. Download `ACSAzureStorage.zip` from the SMA FTP site /OpCon Releases/Integration/AzureStorage/
2. For OpCon DataCenter, extract the files into the /ProgramData/SAM/plugins directory
3. For OpCon Cloud, extract the files into the /Relay/plugins directory. 

### Step 2 — Install the Enterprise Manager plugin

To register the job subtype in Enterprise Manager, complete the following steps:

1. Copy the Enterprise Manager job subtype file from the `/emplugins` directory in the extracted folder.
2. Paste the file into the `dropins` directory of each Enterprise Manager installation that will create Azure Storage job definitions. Create the `dropins` directory if it does not exist.
3. Restart Enterprise Manager. The **Azure Storage** Windows job subtype is displayed in the job type list.

:::info Note

If the job subtype does not appear after restarting, right-click Enterprise Manager in the taskbar and select **Run as Administrator**, then restart again.

:::

### Step 3 — Create global properties in OpCon

To configure the required global properties, complete the following steps:

1. In OpCon, create a global property named `AzureStoragePath` and set its value to the full path of the connector installation directory (for example, `C:\ConnectorFiles\AzureStorage`).
2. Create an encrypted global property to store the Azure Storage connection string (access key). Reference this property in job definitions using the `[[property_name]]` token syntax.

### Step 4 — Create the definitions to support Solution Manager

This requires defining a script that will contain the contents of the Connector.config file used for the AzureStorage connector and defining an agent connection.

To define the configuration script in Solution Manager, complete the following steps.

First, create the script type:

1. Select **Library**, then select **Scripts**.
2. Select **Script types** from the upper right-hand corner.
3. Select **+Add**.
4. In the **Name** field, enter `ACSAzureStorage`.
5. In the **File Extension** field, enter `txt`.
6. In the **Description** field, enter `Used for ACSAzureStorage Integration`.
7. Select **Save**.

Next, create the script runner:

1. Select **Script Runners** from the upper right-hand corner.
2. Select **+Add**.
3. In the **Name** field, enter `ACSAzureStorage`.
4. In the **OS** field, select **ACSAzureStorage** from the list.
5. In the **Type** field, select **ACSAzureStorage** from the list.
6. In the **Command** field, enter `cmd.exe /c`.
7. Select **Save**.

Finally, create the `Connector.config` script:

1. Select **Scripts** from the upper right-hand corner.
2. Select **+Add**.
3. In the **Name** field, enter a name for the script. Using the proposed agent name with `_config` appended is suggested.
4. In the **Type** field, select **ACSAzureStorage** from the list.
5. Assign the required roles.
6. In the **Script** field, paste the contents of the `Connector.config` file you created. See [Configuration options](#configuration-options).
7. Select **Save**.

To define the ACS agent in Solution Manager, complete the following steps.

1. Select **Library**, then select **Agents**.
2. Select **+Add**.
3. In the **Name** field, enter the name of the ACS AzureStorage agent.
4. In the **Type** field, select **AzureStorage** from the list.
5. Select **General Settings**.
6. In the **NetCom** field, enter `<Default>`, or the name of a NetCom or Relay.
7. In the **AzureStorage Settings** section, under **Client Information**:
   - In the **Directory** field, enter the installation directory of the AzureStorage Connector.
   - In the **Name** field, enter `AzureStorage.exe`.
   - In the **Config File Name** field, enter `Connector.config`. This is the default value.
8. In the **Config Script** section:
   - In the **Script Runner** field, select **ACSAzureStorage** from the list.
   - In the **Script** field, select the config script you created above.
9. Select **Save**.
3. Now select **Communication Settings**
   Ensure that the Requires XML Escape Sequences: User-Defined field is set to True 
   If not change the field and save the definition changes
4. Select **CHANGE COMMUNICATION STATUS** and select **Enable Full Comm** to start the agent.    

## Configuration options

The connector reads its configuration from the `Connector.config` file in the installation directory.

| Setting | What it does | Default | Notes |
|---|---|---|---|
| `NAME` | Display name for the connector instance | None | Informational only. The connector does not read this value |
| `DEBUG` | Enables detailed debug logging | `OFF` | Set to `ON` to write verbose log output for troubleshooting |
| `[STORAGE ACCOUNTS]` | Section header | — | Present in the supplied `Connector.config`. See note below |
| `STORAGE` | Named storage account entry | None | Present in the supplied `Connector.config`. See note below |

:::info Note

The supplied `Connector.config` also contains a `[STORAGE ACCOUNTS]` section with a `STORAGE` entry. The storage account name and access key used by a job are supplied per job, through the **Account Name** and **Access Key** fields or the `-sa` and `-k` arguments — not through this file. Leave the section as supplied unless your component owner directs otherwise.

:::

**Connector.config example:**

```
[CONNECTOR]
NAME=Azure Storage Connector
DEBUG=OFF

[STORAGE ACCOUNTS]
STORAGE=<storage account entry>
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
