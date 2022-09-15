# Collecting Evidence

During incident response activities, organisations may need to share evidence. Below is a process using [Azure Storage Explorer](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer) to do this securely.

## Creating a [blob container](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal#create-a-container) to receive evidence

1. Review the following documentation in the creation of a [blob storage](https://docs.microsoft.com/en-us/azure/storage/blobs/quickstart-storage-explorer).

2. Once the follow [step](https://docs.microsoft.com/en-us/azure/storage/blobs/quickstart-storage-explorer#generate-a-shared-access-signature) is completed, share with relevant external organisation.

## Uploading evidence to a [blob container securely using a SAS url](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=windows#attach-to-an-individual-resource)

1. In the Select **Resource panel** of the Connect to **Azure Storage dialog** ![resource dialog](/images/resource-dialog.png), select the **Blob Container** resource.

![blob resource](/images/Blob%20Resource.png)

2. Select **Shared access signature url (SAS)** and select **Next**.

![SaS Token](/images/SaS%20Token.png)

3. Enter a display name for your connection and the SAS URI for the resource. Select Next.

![SaS Input](/images/SaS-Url-Input.png)

4. Review your connection information in the Summary panel. If the connection information is correct, select **Connect**.

5. Once connected, select **Upload** to upload the evidence to blob.

![blob upload](/images/blob-upload.png)