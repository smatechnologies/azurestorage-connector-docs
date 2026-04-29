---
title: Solution Manager operation
sidebar_label: Solution Manager operation
description: "Configure and run Azure Storage tasks using the AzureStorage job type in Solution Manager."
tags:
  - Reference
  - Procedural
  - Automation Engineer
---

# Solution Manager operation

**Theme:** Configure | **Audience:** Automation Engineers

## What is it?

The Solution Manager operation page describes how to define Azure Storage jobs using the AzureStorage job type in Solution Manager.

- Use this interface when creating or managing Azure Storage jobs from the browser-based Solution Manager
- Provides the same task coverage as the Enterprise Manager job subtype in a modern, web-based interface
- Select the task type from a list — only fields relevant to the selected task are displayed

## AzureStorage job type

To configure an Azure Storage job in Solution Manager, select the **AzureStorage** job type and complete the following steps:

1. In the **Integrations** or **Integration Group** field, select the AzureStorage machine associated with the connector.
2. Select the task type from the **Task Type** list.
3. Enter the required values for the selected task type.
4. Select the **Save** button.

### Supported task types

| Task type | Description |
|---|---|
| **Create Container** | Creates a container in Azure Storage |
| **Delete Container** | Deletes a container from Azure Storage |
| **File Arrival** | Monitors a container for the arrival of a specified file |
| **File Delete** | Deletes a file within a container in Azure Storage |
| **File Download** | Downloads a file from Azure Storage to a local directory |
| **File Upload** | Uploads a file from a local directory to Azure Storage |
| **List Containers** | Returns a list of containers in Azure Storage |
| **List Files** | Returns a list of files within a container in Azure Storage |

## Task details

### Integration selection

**Integrations or Integration Group** — Defines the AzureStorage machine associated with the connector.

### Create container

Creates a new container in the storage account.

| Field | Required | Description |
|---|---|---|
| **Account Name** | Yes | The name of the Azure Storage account |
| **Access Key** | Yes | The connection string for the storage account. The key value is the connection string found in the **Access keys** section of the storage account in the Azure portal |
| **Container Name** | Yes | The name of the container to create |

### Delete container

Deletes a container from the storage account.

| Field | Required | Description |
|---|---|---|
| **Account Name** | Yes | The name of the Azure Storage account |
| **Access Key** | Yes | The connection string for the storage account |
| **Container Name** | Yes | The name of the container to delete |

### File arrival

Monitors a container for the arrival of a specified file. Remove any existing versions of the file from the container before starting this task. Wildcards are not supported.

| Field | Required | Description |
|---|---|---|
| **Account Name** | Yes | The name of the Azure Storage account |
| **Access Key** | Yes | The connection string for the storage account |
| **Container Name** | Yes | The name of the container to monitor |
| **Container Folder** | No | The folder path within the container where the file will arrive |
| **Container File Name** | Yes | The name of the file to monitor for |
| **Wait Time** | Yes | Maximum time in minutes to wait for the file. Enter `0` to wait indefinitely |
| **Static File Size Time** | Yes | Time in seconds the file size must remain static before arrival is considered complete. Default: `5` |
| **Poll Delay** | Yes | Time in seconds to wait before the initial check. Default: `5` |
| **Poll Interval** | Yes | Time in seconds between checks. Default: `3` |

### File delete

Deletes a file or files from a container. Wildcards are not supported.

| Field | Required | Description |
|---|---|---|
| **Account Name** | Yes | The name of the Azure Storage account |
| **Access Key** | Yes | The connection string for the storage account |
| **Container Name** | Yes | The name of the container that contains the files to delete |
| **Container Folder** | No | The folder path within the container where the files reside |
| **Container File Name** | Yes | The name of the file to delete |

### File download

Downloads files from a container to a local directory. Files are downloaded relative to the connector installation directory. Target files must not exist in the target directory before downloading. Wildcards are not supported when specific source and target filenames are provided.

| Field | Required | Description |
|---|---|---|
| **Account Name** | Yes | The name of the Azure Storage account |
| **Access Key** | Yes | The connection string for the storage account |
| **Container Name** | Yes | The name of the container from which to download files |
| **Container Folder** | No | The folder path within the container where the files reside |
| **Container File Name** | Yes | The name of the file to download |
| **Directory Name** | Yes | The full path of the local directory to download the files to |
| **Local File Name** | No | The target filename. When specified, wildcards are not supported |

### File upload

Uploads files from a local directory to a container. Files are uploaded relative to the connector installation directory. Target files must not exist in the container before uploading unless **Overwrite** is selected. Wildcards are not supported when specific source and target filenames are provided.

| Field | Required | Description |
|---|---|---|
| **Account Name** | Yes | The name of the Azure Storage account |
| **Access Key** | Yes | The connection string for the storage account |
| **Container Name** | Yes | The name of the container to upload files to |
| **Container Folder** | No | The folder path within the container where the files will be placed |
| **Container File Name** | No | The target filename. When specified, wildcards are not supported |
| **Directory Name** | Yes | The full path of the local directory to upload files from |
| **Local File Name** | Yes | The name of the files to upload |
| **Overwrite** | No | When selected, existing files in the container are overwritten |

### List containers

Returns a list of containers in the storage account. Supports wildcards.

| Field | Required | Description |
|---|---|---|
| **Account Name** | Yes | The name of the Azure Storage account |
| **Access Key** | Yes | The connection string for the storage account |
| **Container Name** | Yes | The name of the container to list. Supports wildcards (`?` and `*`). Enter `*` to list all containers |

### List files

Returns a list of files within a container. Supports wildcards.

| Field | Required | Description |
|---|---|---|
| **Account Name** | Yes | The name of the Azure Storage account |
| **Access Key** | Yes | The connection string for the storage account |
| **Container Name** | Yes | The name of the container from which to list files. Supports wildcards (`?` and `*`) |
| **Container Folder** | No | The folder path within the container to check for files |
| **Container File Name** | Yes | The name of the file to list. Supports wildcards (`?` and `*`) |

## FAQs

**Can I use global property tokens in Solution Manager job fields?**

Yes. Reference global properties using the `[[property_name]]` syntax in any field that accepts text input. Use an encrypted global property to store the **Access Key** value.

**What is the difference between Container Folder and Container Name?**

The **Container Name** identifies the top-level container in the storage account. The **Container Folder** is an optional virtual directory path within that container where blobs are organized. Not all tasks require a container folder.

**Does Solution Manager support wildcard patterns for file operations?**

Wildcards (`?` and `*`) are supported for List Containers and List Files tasks. They are not supported for File Arrival or when specific local and container filenames are both provided for download and upload tasks.

**What format is the Access Key?**

The access key is the full connection string value from the **Access keys** section of the storage account in the Azure portal. It includes the account name, key, and endpoint information in a single string beginning with `DefaultEndpointsProtocol=`.

## Glossary

**AzureStorage job type** — The Solution Manager job type that provides a form-based interface for configuring Azure Storage connector tasks. Replaces the need to enter command-line arguments manually.

**Container Name** — The name of an Azure Blob Storage container. Containers are top-level groupings of blobs within a storage account.

**Container Folder** — An optional virtual directory path within an Azure Storage container. Used to organize blobs into a hierarchical structure within a container.

**Access Key** — The connection string for an Azure Storage account. Obtained from the **Access keys** section of the storage account in the Azure portal. Store as an encrypted global property in OpCon.

**Poll Interval** — The time in seconds between each check when the connector is monitoring for file arrival. A shorter interval increases the number of checks but also increases API calls to Azure.

**Static File Size Time** — The time in seconds that a file's size must remain unchanged before the connector considers the file transfer complete. Prevents the connector from detecting a file that is still being written.
