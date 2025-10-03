---
slug: '/'
sidebar_label: 'Azure-Storage Connector'
---

# Azure-Storage Connector

Latest version : 2.0.2-12.14.6

Azure Storage is an OpCon Connector for Windows that uses the Azure Java SDK to interact with Azure storage. 

The job definitions are entered either as Windows jobs using the Azure Storage job sub-type or Solution Manager using the AzureStorage job type. When the job is scheduled by OpCon, the definitions are passed as arguments to the AzureStorage Connector.

![MSAzure Component Overview](../static/img/msazure-component-overview.png)

Provides tasks to manage containers and blobs (files).

- **list**              Provides list of containers and blobs.    
- **container create**  Create a container.
- **container delete**  Delete a container.
- **delete file**       Delete a file (blob) within a container.
- **download file**     Download a file (blob) from a container.
- **upload file**       Upload a file to a container.
- **file arrival**      Wait for a file (blob) to arrive in a container.

