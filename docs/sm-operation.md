# Solution Manager Operation

## AzureStorage Job Type

When using the Job Type, select the required Task Type from the drop-down list and fill in the fields.

Integration supports the following task types:

- **Create Container**    used to create a container in Azure Storage.
- **Delete Container**    used to delete a container in Azure Storage.
- **File Arrival**        used to monitor for the arrival of a file in a container in Azure Storage.
- **File Delete**         used to delete a file within a container in Azure Storage.
- **File Download**       used to download a file from Azure Storage to a local environment.
- **File Upload**         used to upload a file from a local environment to Azure Storage.
- **List Containers**     used to obtain a list of containers in Azure Storage.
- **List Files**          used to list files with a container in Azure Storage.

### Integration Selection

**Integrations or Integration Group**: Defines the AzureStorage machine that is associated with the connector.

### Task Details

### Create Container
Can be used to create a new container within the storage account.

**Account Name**           Required field that contains the name of the Azure Storage account to perform the task on.
**Access Key**             Required field that contains The Access Key associated with the Storage Account. The key value is the Connection String value that can be found in the Access keys section of the storage account.
**Container Name**         Required field that contains the name of the container to create.

### Delete Container
Can be used to delete a container within the storage account.

**Account Name**           Required field that contains the name of the Azure Storage account to perform the task on.
**Access Key**             Required field that contains The Access Key associated with the Storage Account. The key value is the Connection String value that can be found in the Access keys section of the storage account.
**Container Name**         Required field that contains the name of the container to delete.

### File Arrival
Can be used to monitor for the arrival of a file in a specific container. It should be noted that before starting the task, any previous existing versions of the file must be removed from the container.
Wild cards are not supported for this function.

**Account Name**           Required field that contains the name of the Azure Storage account to perform the task on.
**Access Key**             Required field that contains The Access Key associated with the Storage Account. The key value is the Connection String value that can be found in the Access keys section of the storage account.
**Container Name**         Required field that contains the name of the container to check.
**Container Folder**       Optional field that consists of the folder where the file will be placed in within the container.
**Container File Name**    Required field that consists of the name of the file.
**Wait Time**              Required field that consists of the maximum time in minutes to wait for the file. A value of 0 will wait indefinitely for the file to arrive.
**Static File Size Time**  Required field that consists of the time in seconds for the file size to be static to determine if the file arrival is complete. Default value is 5 seconds.
**Poll Delay**             Required field that consists of the time in seconds to wait before the initial check. Default value is 5.
**Poll Interval**          Required field that consists of the time in seconds between checks. Default value is 3. 

### File Delete
Can be used to delete file(s) in a container.
Wild cards are not supported for this function.

**Account Name**           Required field that contains the name of the Azure Storage account to perform the task on.
**Access Key**             Required field that contains The Access Key associated with the Storage Account. The key value is the Connection String value that can be found in the Access keys section of the storage account.
**Container Name**         Required field that contains the name of the container that contains the file(s) to delete.
**Container Folder**       Optional field that consists of the folder of the file(s) to delete.
**Container File Name**    Required field that consists of the name of the file to delete.

### Fle Download
Can be used to download files from a container within the storage account. The files are downloaded to locations relative to the azure-storage connector installation. 
Before downloading files, the files must not exist in the target directory. When container and local filename definitions are provided, wild cards are not supported. 

**Account Name**           Required field that contains the name of the Azure Storage account to perform the task on.
**Access Key**             Required field that contains The Access Key associated with the Storage Account. The key value is the Connection String value that can be found in the Access keys section of the storage account.
**Container Name**         Required field that contains the name of the container that contains the file(s) to download.
**Container Folder**       Optional field that consists of the folder of the file(s) to download.
**Container File Name**    Required field that consists of the name of the file to download.
**Directory Name**         Required field that consists of the full path of the directory to download the files to.
**Local File Name**        Optional field that consists of the name of the target file. When this option is used, wild cards are not supported.

### Fle Upload
Can be used to upload files from a directory to a container within the storage account. The files are uploaded from locations relative to the azure-storage connector installation. 
Before downloading files, the files must not exist in the target directory. When container and local filename definitions are provided, wild cards are not supported.

**Account Name**           Required field that contains the name of the Azure Storage account to perform the task on.
**Access Key**             Required field that contains The Access Key associated with the Storage Account. The key value is the Connection String value that can be found in the Access keys section of the storage account.
**Container Name**         Required field that contains the name of the container where the file must be uploaded to.
**Container Folder**       Optional field that consists of the folder within the container where the file must be uploaded to..
**Container File Name**    Optional field that consists of the name of the target file. When this option is used, wild cards are not supported.
**Directory Name**         Required field that consists of the full path of the directory to upload the file(s) from.
**Local File Name**        Required field that consists of the name of the file(s) to upload. 
**Overwrite**              Optional field for fileupload and indicates if existing files can be overwritten.

### List Containers
Can be used to list files associated with container(s) within the storage account.

**Account Name**           Required field that contains the name of the Azure Storage account to perform the task on.
**Access Key**             Required field that contains The Access Key associated with the Storage Account. The key value is the Connection String value that can be found in the Access keys section of the storage account.
**Container Name**         Required field that contains the name of the container to list. Supports wild cards (? and *).

### List Files
Can be used to list files associated with container(s) within the storage account.

**Account Name**           Required field that contains the name of the Azure Storage account to perform the task on.
**Access Key**             Required field that contains The Access Key associated with the Storage Account. The key value is the Connection String value that can be found in the Access keys section of the storage account.
**Container Name**         Required field that contains the name of the container to list. Supports wild cards (? and *).
**Container Folder**       Optional field that consists of the folder to check for files within the container.
**Container File Name**    Required field that consists of the name of the file to list. Supports wild cards (? and *).
