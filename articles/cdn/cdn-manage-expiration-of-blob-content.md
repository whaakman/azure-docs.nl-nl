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
ms.openlocfilehash: 6f82ae396a17f903a522c716f73a5f7d2de660e7
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-expiration-of-azure-blob-storage-in-azure-content-delivery-network"></a>Vervaldatum van Azure Blob storage in Azure Content Delivery Network beheren
> [!div class="op_single_selector"]
> * [Azure-webinhoud](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 
> 

De [Blob storage-service](../storage/common/storage-introduction.md#blob-storage) in Azure Storage is een van de verschillende Azure gebaseerde oorsprongen geïntegreerd met Azure Content Delivery Network (CDN). Kan een openbaar toegankelijke blob-inhoud in cache worden opgeslagen in Azure CDN totdat de time-to-live (TTL) is verstreken. De TTL-waarde wordt bepaald door de `Cache-Control` -header in het HTTP-antwoord op de bronserver. Dit artikel wordt beschreven op verschillende manieren die u kunt instellen de `Cache-Control` header op een blob in Azure Storage.

U kunt de cache-instellingen van de Azure-portal ook beheren door in te stellen [CDN regels opslaan in cache](cdn-caching-rules.md). Als u slechts één of meer caching regels en hun gedrag ingesteld op **overschrijven** of **cache overslaan**, de geleverde oorsprong cache-instellingen in dit artikel wordt beschreven, worden genegeerd. Zie voor meer informatie over algemene concepten voor caching [werking van cacheopslag](cdn-how-caching-works.md).

> [!TIP]
> U kunt geen TTL instellen voor een blob. In dit geval past Azure CDN automatisch een standaard-TTL van zeven dagen, tenzij u een cache van de regels in de Azure-portal hebt ingesteld. Deze standaard TTL geldt alleen voor levering optimalisaties van algemene webtoepassingen. De standaard TTL-waarde is één dag voor optimalisatie van grote bestanden, en voor mediastreaming optimalisaties de standaard TTL-waarde is één jaar.
> 
> Zie voor meer informatie over de werking van Azure CDN om sneller toegang tot blobs en andere bestanden [overzicht van het Azure Content Delivery Network](cdn-overview.md).
> 
> Zie voor meer informatie over Azure-blobopslag [Inleiding tot Blob storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).
 

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
Met de [Azure-opdrachtregelinterface](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest) (CLI), kunt u Azure blob-bronnen beheren vanaf de opdrachtregel. Om in te stellen van de header cache-control tijdens het uploaden van een blob met de Azure CLI het *cacheControl* eigenschap met behulp van de `-p` overschakelen. Het volgende voorbeeld laat zien hoe de TTL-waarde ingesteld op één uur (3600 seconden):
  
```azurecli
azure storage blob upload -c <connectionstring> -p cacheControl="max-age=3600" .\test.txt myContainer test.txt
```

### <a name="azure-storage-services-rest-api"></a>REST-API van Azure storage-services
U kunt de [REST-API van Azure storage-services](https://msdn.microsoft.com/library/azure/dd179355.aspx) expliciet instellen de *x-ms-blob-cache-control* eigenschap door de volgende bewerkingen voor een aanvraag:
  
   - [Blob plaatsen](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx)
   - [Lijst met geblokkeerde websites plaatsen](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)
   - [Blob-eigenschappen instellen](https://msdn.microsoft.com/library/azure/ee691966.aspx)

## <a name="testing-the-cache-control-header"></a>Testen van de Cache-Control-header
U kunt eenvoudig de TTL-instellingen van uw blobs controleren. Met uw browser [hulpprogramma's voor ontwikkelaars](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test die uw blob bevat de `Cache-Control` antwoordheader. U kunt ook een hulpprogramma zoals gebruiken [Wget](https://www.gnu.org/software/wget/), [Postman](https://www.getpostman.com/), of [Fiddler](http://www.telerik.com/fiddler) om te onderzoeken de antwoordheaders.

## <a name="next-steps"></a>Volgende stappen
* [Informatie over het beheren van de verlooptijd van Cloudservice-inhoud in Azure CDN](cdn-manage-expiration-of-cloud-service-content.md)
* [Meer informatie over concepten opslaan in cache](cdn-how-caching-works.md)

