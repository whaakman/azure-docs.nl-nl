---
title: Vervaldatum van Azure Blob storage in Azure Content Delivery Network beheren | Microsoft Docs
description: Meer informatie over de opties voor het beheren van time to live voor blobs in Azure CDN opslaan in cache.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: ad4801e9-d09a-49bf-b35c-efdc4e6034e8
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/10/2017
ms.author: mazha
ms.openlocfilehash: 6fa563e102569064d045fbb60ebd5d1df52e3e73
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2017
---
# <a name="manage-expiration-of-azure-blob-storage-in-azure-content-delivery-network"></a>Vervaldatum van Azure Blob storage in Azure Content Delivery Network beheren
> [!div class="op_single_selector"]
> * [Azure-web-inhoud](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 
> 

De [Blob storage-service](../storage/common/storage-introduction.md#blob-storage) in Azure Storage is een van de verschillende Azure gebaseerde oorsprongen geïntegreerd met Azure Content Delivery Network (CDN). Kan een openbaar toegankelijke blob-inhoud in cache worden opgeslagen in Azure CDN totdat de time-to-live (TTL) is verstreken. De TTL-waarde wordt bepaald door de `Cache-Control` -header in het HTTP-antwoord op de bronserver. Dit artikel wordt beschreven op verschillende manieren die u kunt instellen de `Cache-Control` header op een blob in Azure Storage.

> [!TIP]
> U kunt geen TTL instellen voor een blob. In dit geval past Azure CDN automatisch een standaard-TTL van zeven dagen. Deze standaard TTL geldt alleen voor levering optimalisaties van algemene webtoepassingen. De standaard TTL-waarde is één dag voor optimalisatie van grote bestanden, en voor mediastreaming optimalisaties de standaard TTL-waarde is één jaar.
> 
> Zie voor meer informatie over de werking van Azure CDN om sneller toegang tot blobs en andere bestanden [overzicht van het Azure Content Delivery Network](cdn-overview.md).
> 
> Zie voor meer informatie over Azure-blobopslag [Inleiding tot Blob storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction).
 

## <a name="setting-cache-control-headers-by-using-azure-powershell"></a>Cache-Control headers instellen met behulp van Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) is een van de snelste en effectieve manieren om uw Azure-services te beheren. Gebruik de `Get-AzureStorageBlob` cmdlet verwijst naar de blob, stel de `.ICloudBlob.Properties.CacheControl` eigenschap. 

Bijvoorbeeld:

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> U kunt ook PowerShell om te gebruiken [beheren van uw CDN-profielen en eindpunten](cdn-manage-powershell.md).
> 
>

## <a name="setting-cache-control-headers-by-using-net"></a>Cache-Control headers instellen met behulp van .NET
Instellen van een blob `Cache-Control` header met behulp van .NET-code, gebruik de [Azure Storage-clientbibliotheek voor .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) om in te stellen de [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) eigenschap.

Bijvoorbeeld:

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);

        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> Er zijn meer .NET-codevoorbeelden beschikbaar in [voorbeelden van Azure Blob Storage voor .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 

## <a name="setting-cache-control-headers-by-using-other-methods"></a>Cache-Control headers instellen via andere methoden

### <a name="azure-storage-explorer"></a>Azure Opslagverkenner
Met [Azure Opslagverkenner](https://azure.microsoft.com/en-us/features/storage-explorer/), u kunt bekijken en bewerken van uw blob storage-resources, inclusief eigenschappen, zoals de *CacheControl* eigenschap. 

Bijwerken van de *CacheControl* eigenschap van een blob met Azure Storage Explorer:
   1. Selecteer een blob en vervolgens **eigenschappen** in het contextmenu. 
   2. Schuif omlaag naar de *CacheControl* eigenschap.
   3. Voer een waarde in en klik vervolgens op **opslaan**.


![Azure Storage Explorer-eigenschappen](./media/cdn-manage-expiration-of-blob-content/cdn-storage-explorer-properties.png)

### <a name="azure-command-line-interface"></a>Azure-opdrachtregelinterface
Met de [Azure-opdrachtregelinterface](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) (CLI), kunt u Azure blob-bronnen beheren vanaf de opdrachtregel. Om in te stellen van de header cache-control tijdens het uploaden van een blob met de Azure CLI het *cacheControl* eigenschap met behulp van de `-p` overschakelen. Het volgende voorbeeld laat zien hoe de TTL-waarde ingesteld op één uur (3600 seconden):
  
''' Azure storage-blob van azure CLI -c uploaden <connectionstring> - p cacheControl = "maximumleeftijd 3600" .\test.txt myContainer test.txt =
```

### Azure storage services REST API
You can use the [Azure storage services REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx) to explicitly set the *x-ms-blob-cache-control* property by using the following operations on a request:
  
   - [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx)
   - [Put Block List](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)
   - [Set Blob Properties](https://msdn.microsoft.com/library/azure/ee691966.aspx)

## Testing the Cache-Control header
You can easily verify the TTL settings of your blobs. With your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test that your blob includes the `Cache-Control` response header. You can also use a tool such as [Wget](https://www.gnu.org/software/wget/), [Postman](https://www.getpostman.com/), or [Fiddler](http://www.telerik.com/fiddler) to examine the response headers.

## Next Steps
* [Learn how to manage expiration of Cloud Service content in Azure CDN](cdn-manage-expiration-of-cloud-service-content.md)

