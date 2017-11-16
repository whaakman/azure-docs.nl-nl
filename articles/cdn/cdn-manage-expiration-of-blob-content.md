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
ms.openlocfilehash: c2b49058ec7dd52b5063e815447697fa17ddb53a
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="manage-expiration-of-azure-blob-storage-in-azure-content-delivery-network"></a>Vervaldatum van Azure Blob storage in Azure Content Delivery Network beheren
> [!div class="op_single_selector"]
> * [Azure-web-inhoud](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 
> 

De [Blob storage-service](../storage/common/storage-introduction.md#blob-storage) in Azure Storage is een van de verschillende Azure gebaseerde oorsprongen geïntegreerd met Azure Content Delivery Network (CDN). Kan een openbaar toegankelijke blob-inhoud in cache worden opgeslagen in Azure CDN totdat de time-to-live (TTL) is verstreken. De TTL-waarde wordt bepaald door de `Cache-Control` -header in het HTTP-antwoord op de bronserver. Dit artikel wordt beschreven op verschillende manieren die u kunt instellen de `Cache-Control` header op een blob in Azure Storage.

> [!TIP]
> U kunt geen TTL instellen voor een blob. In dit geval past Azure CDN automatisch een standaard-TTL van zeven dagen.
> 
> Zie voor meer informatie over de werking van Azure CDN om sneller toegang tot blobs en andere bestanden [overzicht van het Azure Content Delivery Network](cdn-overview.md).
> 
> Zie voor meer informatie over Azure-blobopslag [Inleiding tot Blob storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction).
 

## <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) is een van de snelste en effectieve manieren om uw Azure-services te beheren. Gebruik de `Get-AzureStorageBlob` cmdlet verwijst naar de blob, stel de `.ICloudBlob.Properties.CacheControl` eigenschap. 

Bijvoorbeeld:

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> U kunt ook PowerShell om te gebruiken [beheren van uw CDN-profielen en eindpunten](cdn-manage-powershell.md).
> 
>

## <a name="azure-storage-client-library-for-net"></a>Azure Storage-clientbibliotheek voor .NET
Instellen van een blob `Cache-Control` header met .NET, gebruik de [Azure Storage-clientbibliotheek voor .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) om in te stellen de [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) eigenschap.

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
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> Er zijn meer .NET-codevoorbeelden beschikbaar in [voorbeelden van Azure Blob Storage voor .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 
> 

## <a name="other-methods"></a>Andere methoden
* [Azure-opdrachtregelinterface](../cli-install-nodejs.md)
  
    Wanneer u een blob uploadt, kunt u instellen de *cacheControl* eigenschap met behulp van de `-p` switch in de Azure-opdrachtregelinterface. Het volgende voorbeeld wordt de TTL aan één uur (3600 seconden):
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [REST-API voor Azure Storage-services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    Expliciet ingesteld de *x-ms-blob-cache-control* -eigenschap op een [Blob plaatsen](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [blokkeringslijst plaatsen](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), of [Blob-eigenschappen instellen](https://msdn.microsoft.com/library/azure/ee691966.aspx) aanvraag.

* Beheerhulpprogramma's voor opslag van derden
  
    Sommige beheerhulpprogramma's van derden Azure-opslag kunnen u instellen de **CacheControl** -eigenschap op blobs. 

## <a name="testing-the-cache-control-header"></a>Testen van de Cache-Control-header
U kunt eenvoudig de TTL-instellingen van uw blobs controleren. Met uw browser [hulpprogramma's voor ontwikkelaars](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test die uw blob bevat de `Cache-Control` antwoordheader. U kunt ook een hulpprogramma zoals gebruiken **wget**, [Postman](https://www.getpostman.com/), of [Fiddler](http://www.telerik.com/fiddler) om te onderzoeken de antwoordheaders.

## <a name="next-steps"></a>Volgende stappen
* [Informatie over het beheren van de verlooptijd van Cloudservice-inhoud in Azure CDN](cdn-manage-expiration-of-cloud-service-content.md)

