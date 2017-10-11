---
title: Vervaldatum van Azure Storage-blobs in Azure CDN beheren | Microsoft Docs
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
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d4741921806e443d92c385a04b781cec296c2ae8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="manage-expiration-of-azure-storage-blobs-in-azure-cdn"></a>Vervaldatum van Azure Storage-blobs in Azure CDN beheren
> [!div class="op_single_selector"]
> * [Azure-Web-Apps/Cloud Services, ASP.NET of IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure blob Storage-service](cdn-manage-expiration-of-blob-content.md)
> 
> 

De [blob-service](../storage/common/storage-introduction.md#blob-storage) in [Azure Storage](../storage/common/storage-introduction.md) is een van de verschillende Azure gebaseerde oorsprongen geïntegreerd met Azure CDN.  Kan een openbaar toegankelijke blob-inhoud in cache worden opgeslagen in Azure CDN totdat de time-to-live (TTL) is verstreken.  De TTL-waarde wordt bepaald door de [ *Cache-Control* header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in de HTTP-antwoord uit Azure Storage.

> [!TIP]
> U kunt geen TTL instellen voor een blob.  In dit geval past Azure CDN automatisch een standaard-TTL van zeven dagen.
> 
> Zie voor meer informatie over de werking van Azure CDN om sneller toegang tot blobs en andere bestanden de [overzicht van Azure CDN](cdn-overview.md).
> 
> Zie voor meer informatie over de Azure Storage-blob-service, [concepten van Blob-Service](https://msdn.microsoft.com/library/dd179376.aspx). 
> 
> 

Deze zelfstudie laat zien dat verschillende manieren waarop u de TTL-waarde op een blob in Azure Storage instellen kunt.  

## <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) is een van de snelste, meest krachtige manieren voor het beheren van uw Azure-services.  Gebruik de `Get-AzureStorageBlob` cmdlet verwijst naar de blob, stel de `.ICloudBlob.Properties.CacheControl` eigenschap. 

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
U stelt een blob-TTL met .NET met de [Azure Storage-clientbibliotheek voor .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) om in te stellen de [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) eigenschap.

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
> Er zijn veel meer .NET-codevoorbeelden beschikbaar in de [voorbeelden van Azure Blob Storage voor .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 
> 

## <a name="other-methods"></a>Andere methoden
* [Azure-opdrachtregelinterface](../cli-install-nodejs.md)
  
    Tijdens het uploaden van de blob, stel de *cacheControl* met behulp van de eigenschap de `-p` overschakelen.  In het volgende voorbeeld wordt de TTL aan één uur (3600 seconden).
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [REST-API voor Azure Storage-services](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    Expliciet ingesteld de *x-ms-blob-cache-control* -eigenschap op een [Blob plaatsen](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [blokkeringslijst plaatsen](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), of [Blob-eigenschappen instellen](https://msdn.microsoft.com/library/azure/ee691966.aspx) aanvraag.
* Beheerhulpprogramma's voor opslag van derden
  
    Sommige hulpprogramma's van derden Azure Storage management kunnen u instellen de *CacheControl* -eigenschap op blobs. 

## <a name="testing-the-cache-control-header"></a>Testen van de *Cache-Control* koptekst
U kunt eenvoudig de TTL van uw blobs controleren.  Met behulp van uw browser [hulpprogramma's voor ontwikkelaars](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test die uw blob is inclusief de *Cache-Control* antwoordheader.  U kunt ook een hulpprogramma zoals **wget**, [Postman](https://www.getpostman.com/), of [Fiddler](http://www.telerik.com/fiddler) om te onderzoeken de antwoordheaders.

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over de *Cache-Control* koptekst](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Informatie over het beheren van de verlooptijd van Cloudservice-inhoud in Azure CDN](cdn-manage-expiration-of-cloud-service-content.md)

