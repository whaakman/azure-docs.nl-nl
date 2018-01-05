---
title: Beveiligde toegang tot de gegevens van een toepassing in de cloud met Azure Storage | Microsoft Docs
description: SAS-tokens, versleuteling en HTTPS gebruiken voor het beveiligen van gegevens van uw toepassing in de cloud
services: storage
documentationcenter: 
author: georgewallace
manager: timlt
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 09/19/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: c43165e230a00b6a4408637fd2290a21800d07b9
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="secure-access-to-an-applications-data-in-the-cloud"></a>Veilige toegang tot de gegevens van een toepassing in de cloud

Deze zelfstudie maakt deel uit drie van een serie. U informatie over het beveiligen van toegang tot het opslagaccount. 

Deel 3 van de reeks, leert u hoe:

> [!div class="checklist"]
> * SAS-tokens gebruiken voor toegang tot van miniatuurafbeeldingen
> * Server-side '-versleuteling inschakelen
> * Alleen HTTPS-transport inschakelen

[Azure-blobopslag](../common/storage-introduction.md#blob-storage) biedt een robuuste service voor het opslaan van bestanden voor toepassingen. Deze zelfstudie wordt uitgebreid [het vorige onderwerp] [ previous-tutorial] laten zien hoe u veilige toegang tot uw storage-account van een webtoepassing. Wanneer u klaar bent met de afbeeldingen zijn versleuteld en de web-app beveiligde SAS-tokens gebruikt voor toegang tot de miniatuurafbeeldingen.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie u moet de zelfstudie hebt voltooid vorige opslag: [automatisch vergroten/verkleinen geüpload met behulp van gebeurtenis raster installatiekopieën][previous-tutorial]. 

## <a name="set-container-public-access"></a>Set container openbare toegang

In dit gedeelte van de zelfstudie reeks worden SAS-tokens gebruikt voor toegang tot de miniaturen. In deze stap, stelt u de openbare toegang tot de _duim_ container `off`.

```azurecli-interactive 
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv) 

az storage container set-permission \ --account-name $blobStorageAccount \ --account-key $blobStorageAccountKey \ --name thumbs  \
--public-access off
``` 

## <a name="configure-sas-tokens-for-thumbnails"></a>SAS-tokens voor miniaturen configureren

Deel een van deze zelfstudie reeks, werd de webtoepassing installatiekopieën van een openbare container weergegeven. In dit deel van de reeks u [Secure Access Signature (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md#what-is-a-shared-access-signature) tokens voor het ophalen van de miniatuurafbeeldingen. SAS-tokens kunnen u beperkte toegang bieden tot een container of blob op basis van IP, protocol, tijdsinterval of rechten die zijn toegestaan.

In dit voorbeeld wordt de bron-opslagplaats code gebruikt de `sasTokens` vertakking waarvoor een voorbeeld van de bijgewerkte code. Verwijder de bestaande GitHub-implementatie met de [az webapp implementatie bron verwijderen](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_delete). Configureer vervolgens de GitHub-implementatie naar de web-app met de [az webapp implementatieconfiguratie bron](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config) opdracht.  

In de volgende opdracht `<web-app>` is de naam van uw web-app.  

```azurecli-interactive 
az webapp deployment source delete --name <web-app> --resource-group myResourceGroup

az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch sasTokens --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
``` 

De `sasTokens` vertakking van de opslagplaats updates de `StorageHelper.cs` bestand. Vervangt de `GetThumbNailUrls` taak met het onderstaande voorbeeld. De bijgewerkte taak haalt de miniatuur URL's door in te stellen een [SharedAccessBlobPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy?view=azure-dotnet) de begintijd, verlooptijd en machtigingen voor het SAS-token op te geven. Zodra de web-app geïmplementeerd haalt de miniaturen nu met een URL met behulp van een SAS-token. De bijgewerkte taak wordt weergegeven in het volgende voorbeeld:
    
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

    // Set the permission of the container to public
    await container.SetPermissionsAsync(new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

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

De volgende klassen, eigenschappen en methoden worden gebruikt in de vorige bewerking:

|Klasse  |Eigenschappen| Methoden  |
|---------|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.windowsazure.storage.auth.storagecredentials?view=azure-dotnet)    |         |
|[CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)     | |[CreateCloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.createcloudblobclient?view=azure-dotnet#Microsoft_WindowsAzure_Storage_CloudStorageAccount_CreateCloudBlobClient)        |
|[CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)     | |[GetContainerReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getcontainerreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetContainerReference_System_String_)         |
|[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)     | |[SetPermissionsAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.setpermissionsasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_SetPermissionsAsync_Microsoft_WindowsAzure_Storage_Blob_BlobContainerPermissions_) <br> [ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_ListBlobsSegmentedAsync_System_String_System_Boolean_Microsoft_WindowsAzure_Storage_Blob_BlobListingDetails_System_Nullable_System_Int32__Microsoft_WindowsAzure_Storage_Blob_BlobContinuationToken_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_)       |
|[BlobContinuationToken](/dotnet/api/microsoft.windowsazure.storage.blob.blobcontinuationtoken?view=azure-dotnet)     |         |
|[BlobResultSegment](/dotnet/api/microsoft.windowsazure.storage.blob.blobresultsegment?view=azure-dotnet)    | [Resultaten](/dotnet/api/microsoft.windowsazure.storage.blob.blobresultsegment.results?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobResultSegment_Results)         |
|[CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet)    |         | [GetSharedAccessSignature](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_GetSharedAccessSignature_Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_)
|[SharedAccessBlobPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy?view=azure-dotnet)     | [SharedAccessStartTime](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.sharedaccessstarttime?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_SharedAccessStartTime)<br>[SharedAccessExpiryTime](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.sharedaccessexpirytime?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_SharedAccessExpiryTime)<br>[Machtigingen](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.permissions?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_Permissions) |        |

## <a name="server-side-encryption"></a>Server-side '-versleuteling

[Azure Storage Service versleuteling (SSE)](../common/storage-service-encryption.md) helpt u bij het beveiligen en uw gegevens beschermen. SSE versleutelt de gegevens in rust, versleuteling en ontsleuteling Sleutelbeheer verwerken. Alle gegevens worden versleuteld met behulp van 256-bits [AES-versleuteling](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), een van de sterkste blok coderingen beschikbaar.

In het volgende voorbeeld schakelt u versleuteling voor blobs. Bestaande blobs die zijn gemaakt vóór het inschakelen van versleuteling zijn niet versleuteld. De `x-ms-server-encrypted` header voor een aanvraag voor een blob toont de versleutelingsstatus van de blob.

```azurecli-interactive
az storage account update --encryption-services blob --name <storage-account-name> --resource-group myResourceGroup
```

Een nieuwe installatiekopie uploaden naar de webtoepassing nu dat versleuteling is ingeschakeld.

Met behulp van `curl` met de schakeloptie `-I` vervangt door uw eigen waarden voor het ophalen van alleen de koppen, `<storage-account-name>`, `<container>`, en `<blob-name>`.  

```azurecli-interactive
sasToken=$(az storage blob generate-sas \
    --account-name <storage-account-name> \
    --account-key <storage-account-key> \
    --container-name <container> \
    --name <blob-name> \
    --permissions r \
    --expiry `date --date="next day" +%Y-%m-%d` \
    --output tsv)

curl https://<storage-account-name>.blob.core.windows.net/<container>/<blob-name>?$sasToken -I
```

Let in het antwoord op de `x-ms-server-encrypted` header toont `true`. Deze header geeft aan dat de gegevens nu met SSE is versleuteld.

```
HTTP/1.1 200 OK
Content-Length: 209489
Content-Type: image/png
Last-Modified: Mon, 11 Sep 2017 19:27:42 GMT
Accept-Ranges: bytes
ETag: "0x8D4F94B2BE76D45"
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 57047db3-001e-0050-3e34-2ba769000000
x-ms-version: 2017-04-17
x-ms-lease-status: unlocked
x-ms-lease-state: available
x-ms-blob-type: BlockBlob
x-ms-server-encrypted: true
Date: Mon, 11 Sep 2017 19:27:46 GMT
```

## <a name="enable-https-only"></a>Alleen HTTPS inschakelen

Om ervoor te zorgen dat de aanvragen voor gegevens die naar en van een opslagaccount beveiligd zijn, kunt u alleen aanvragen om HTTPS te beperken. Het account vereist opslagprotocol bijwerken met behulp van de [az storage account bijwerken](/cli/azure/storage/account#az_storage_account_update) opdracht.

```azurecli-interactive
az storage account update --resource-group myresourcegroup --name <storage-account-name> --https-only true
```

Test de verbinding met behulp van `curl` met behulp van de `HTTP` protocol.

```azurecli-interactive
curl http://<storage-account-name>.blob.core.windows.net/<container>/<blob-name> -I
```

Nu dat veilige overdracht vereist is, wordt het volgende bericht:

```
HTTP/1.1 400 The account being accessed does not support http.
```

## <a name="next-steps"></a>Volgende stappen

Deel 3 van de reeks, hebt u geleerd hoe u veilige toegang tot het opslagaccount, zoals het:

> [!div class="checklist"]
> * SAS-tokens gebruiken voor toegang tot van miniatuurafbeeldingen
> * Server-side '-versleuteling inschakelen
> * Alleen HTTPS-transport inschakelen

Ga naar de vierde deel van de reeks voor informatie over het controleren en problemen oplossen van een cloud-storage-toepassing.

> [!div class="nextstepaction"]
> [Controleren en problemen oplossen toepassing toepassing cloudopslag](storage-monitor-troubleshoot-storage-application.md)

[previous-tutorial]: ../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json