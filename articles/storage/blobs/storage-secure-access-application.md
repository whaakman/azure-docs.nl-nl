---
title: Met Azure Storage de toegang tot gegevens van een toepassing in de cloud beveiligen | Microsoft Docs
description: Gebruik SAS-tokens, versleuteling en HTTPS om de gegevens van een toepassing in de cloud te beveiligen.
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 05/30/2018
ms.author: tamram
ms.custom: mvc
ms.openlocfilehash: 71eec62f4468b6b74a15a30be3e472b41c4d45b0
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397769"
---
# <a name="secure-access-to-an-applications-data-in-the-cloud"></a>Toegang tot gegevens van een toepassing in de cloud beveiligen

Deze zelfstudie is deel drie van een serie. U leert hoe u informatie over toegang tot het opslagaccount kunt beveiligen. 

In deel drie van de serie leert u het volgende:

> [!div class="checklist"]
> * SAS-tokens gebruiken voor toegang tot miniatuurafbeeldingen
> * Versleuteling aan de serverzijde inschakelen
> * Alleen-HTTPS-transport inschakelen

[Azure Blob Storage](../common/storage-introduction.md#blob-storage) biedt een robuuste service voor het opslaan van bestanden voor toepassingen. Deze zelfstudie gaat nog een stapje verder dan [het vorige onderwerp][previous-tutorial] om te tonen hoe u toegang tot uw opslagaccount vanuit een webtoepassing kunt beveiligen. Wanneer u klaar bent, zijn de afbeeldingen versleuteld en gebruikt de web-app beveiligde SAS-tokens voor toegang tot de miniatuurafbeeldingen.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie moet u eerst de vorige zelfstudie over opslag hebben voltooid: [Het wijzigen van het formaat van geüploade afbeeldingen automatiseren met behulp van Event Grid][previous-tutorial]. 

## <a name="set-container-public-access"></a>Openbare toegang tot de container instellen

In dit deel van de serie zelfstudies worden SAS-tokens gebruikt voor toegang tot de miniaturen. In deze stap gaat u de openbare toegang tot de container _thumbnails_ instellen op `off`.

```azurecli-interactive 
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv) 

az storage container set-permission \ --account-name $blobStorageAccount \ --account-key $blobStorageAccountKey \ --name thumbnails  \
--public-access off
``` 

## <a name="configure-sas-tokens-for-thumbnails"></a>SAS-tokens voor miniaturen configureren

In deel één van deze serie zelfstudies, werden door de webtoepassing afbeeldingen van een openbare container weergegeven. In dit deel van de serie gaat u [Shared Access Signature-tokens (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md#what-is-a-shared-access-signature) gebruiken om de miniatuurafbeeldingen op te halen. Met SAS-tokens kunt u beperkte toegang verlenen tot een container of blob op basis van IP, protocol, tijdsinterval of toegestane rechten.

In dit voorbeeld maakt de opslagplaats van de broncode gebruik van de vertakking `sasTokens` die een bijgewerkt codevoorbeeld bevat. Verwijder de bestaande GitHub-implementatie met [az webapp deployment source delete](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_delete). Configureer vervolgens GitHub-implementatie naar de webtoepassing met de opdracht [az webapp deployment source config](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config).  

In de volgende opdracht is `<web-app>` de naam van uw web-app.  

```azurecli-interactive 
az webapp deployment source delete --name <web-app> --resource-group myResourceGroup

az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch sasTokens --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
``` 

De vertakking `sasTokens` van de opslagplaats werkt het bestand `StorageHelper.cs` bij. Dit vervangt de taak `GetThumbNailUrls` met het onderstaande codevoorbeeld. Met de bijgewerkte taak worden de miniatuur-URL's opgehaald door een [SharedAccessBlobPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy?view=azure-dotnet) in te stellen om de begintijd, verlooptijd en machtigingen voor het SAS-token op te geven. Zodra de web-app is geïmplementeerd, haalt deze met behulp van een SAS-token de miniaturen met een URL op. De bijgewerkte taak wordt in het volgende voorbeeld weergegeven:
    
```csharp
public static async Task<List<string>> GetThumbNailUrls(AzureStorageConfig _storageConfig)
{
    List<string> thumbnailUrls = new List<string>();

    // Create storagecredentials object by reading the values from the configuration (appsettings.json)
    StorageCredentials storageCredentials = new StorageCredentials(_storageConfig.AccountName, _storageConfig.AccountKey);

    // Create cloudstorage account by passing the storagecredentials
    CloudStorageAccount storageAccount = new CloudStorageAccount(storageCredentials, true);

    // Create blob client
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get reference to the container
    CloudBlobContainer container = blobClient.GetContainerReference(_storageConfig.ThumbnailContainer);

    BlobContinuationToken continuationToken = null;

    BlobResultSegment resultSegment = null;

    //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
    //When the continuation token is null, the last page has been returned and execution can exit the loop.
    do
    {
        //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
        //or by calling a different overload.
        resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);

        foreach (var blobItem in resultSegment.Results)
        {
            CloudBlockBlob blob = blobItem as CloudBlockBlob;
            //Set the expiry time and permissions for the blob.
            //In this case, the start time is specified as a few minutes in the past, to mitigate clock skew.
            //The shared access signature will be valid immediately.
            SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

            sasConstraints.SharedAccessStartTime = DateTimeOffset.UtcNow.AddMinutes(-5);

            sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);

            sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

            //Generate the shared access signature on the blob, setting the constraints directly on the signature.
            string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

            //Return the URI string for the container, including the SAS token.
            thumbnailUrls.Add(blob.Uri + sasBlobToken);

        }

        //Get the continuation token.
        continuationToken = resultSegment.ContinuationToken;
    }

    while (continuationToken != null);

    return await Task.FromResult(thumbnailUrls);
}
```

In de vorige taak zijn de volgende klassen, eigenschappen en methoden gebruikt:

|Klasse  |Eigenschappen| Methoden  |
|---------|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.windowsazure.storage.auth.storagecredentials?view=azure-dotnet)    |         |
|[CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)     | |[CreateCloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.createcloudblobclient?view=azure-dotnet#Microsoft_WindowsAzure_Storage_CloudStorageAccount_CreateCloudBlobClient)        |
|[CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)     | |[GetContainerReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getcontainerreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetContainerReference_System_String_)         |
|[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)     | |[SetPermissionsAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.setpermissionsasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_SetPermissionsAsync_Microsoft_WindowsAzure_Storage_Blob_BlobContainerPermissions_) <br> [ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_ListBlobsSegmentedAsync_System_String_System_Boolean_Microsoft_WindowsAzure_Storage_Blob_BlobListingDetails_System_Nullable_System_Int32__Microsoft_WindowsAzure_Storage_Blob_BlobContinuationToken_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_)       |
|[BlobContinuationToken](/dotnet/api/microsoft.windowsazure.storage.blob.blobcontinuationtoken?view=azure-dotnet)     |         |
|[BlobResultSegment](/dotnet/api/microsoft.windowsazure.storage.blob.blobresultsegment?view=azure-dotnet)    | [Results](/dotnet/api/microsoft.windowsazure.storage.blob.blobresultsegment.results?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobResultSegment_Results)         |
|[CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet)    |         | [GetSharedAccessSignature](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_GetSharedAccessSignature_Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_)
|[SharedAccessBlobPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy?view=azure-dotnet)     | [SharedAccessStartTime](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.sharedaccessstarttime?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_SharedAccessStartTime)<br>[SharedAccessExpiryTime](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.sharedaccessexpirytime?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_SharedAccessExpiryTime)<br>[Machtigingen](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.permissions?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_Permissions) |        |

## <a name="server-side-encryption"></a>Versleuteling aan de serverzijde

[Azure Storage Service Encryption (SSE)](../common/storage-service-encryption.md) helpt u bij het beschermen en beveiligen van uw gegevens. SSE-versleutelt data-at-rest, handelt de versleuteling, ontsleuteling en het sleutelbeheer af. Alle gegevens worden versleuteld met 256-bits [AES-versleuteling](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), een van de krachtigste blokversleutelingsmethoden die er bestaan.

SSE versleutelt automatisch gegevens in alle prestatielagen (Standaard en Premium), alle implementatiemodellen (Azure Resource Manager en het klassieke model) en alle services van Azure Storage (Blob, Queue, Table en File). 

## <a name="enable-https-only"></a>Alleen-HTTPS inschakelen

Om ervoor te zorgen dat aanvragen voor gegevens naar en van een opslagaccount beveiligd zijn, kunt u aanvragen beperken tot alleen-HTTPS. Werk het vereiste protocol voor het opslagaccount bij met de opdracht [az storage account update](/cli/azure/storage/account#az_storage_account_update).

```azurecli-interactive
az storage account update --resource-group myresourcegroup --name <storage-account-name> --https-only true
```

Test de verbinding met `curl` met behulp van het `HTTP`-protocol.

```azurecli-interactive
curl http://<storage-account-name>.blob.core.windows.net/<container>/<blob-name> -I
```

Nu veilige overdracht is vereist, wordt het volgende bericht weergegeven:

```
HTTP/1.1 400 The account being accessed does not support http.
```

## <a name="next-steps"></a>Volgende stappen

In deel drie van de serie, hebt u geleerd hoe u de toegang tot het opslagaccount beveiligt, om het volgende te doen:

> [!div class="checklist"]
> * SAS-tokens gebruiken voor toegang tot miniatuurafbeeldingen
> * Versleuteling aan de serverzijde inschakelen
> * Alleen-HTTPS-transport inschakelen

Ga verder met deel vier van de serie als u wilt weten hoe u een toepassing voor cloudopslag kunt bewaken en problemen ermee kunt oplossen.

> [!div class="nextstepaction"]
> [Toepassing voor cloudopslag bewaken en problemen ermee oplossen](storage-monitor-troubleshoot-storage-application.md)

[previous-tutorial]: ../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json