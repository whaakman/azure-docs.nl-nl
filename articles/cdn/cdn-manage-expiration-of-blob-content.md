---
title: Vervaldatum van Azure Blob-opslag in Azure Content Delivery Network beheren | Microsoft Docs
description: Meer informatie over de opties voor het beheren van de time-to-live voor blobs in Azure CDN caching.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: ad4801e9-d09a-49bf-b35c-efdc4e6034e8
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 02/1/2018
ms.author: mazha
ms.openlocfilehash: 1b2009b54c7f436667c316b7ca002314bc966a1b
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57531926"
---
# <a name="manage-expiration-of-azure-blob-storage-in-azure-cdn"></a>Vervaldatum van Azure Blob-opslag in Azure CDN beheren
> [!div class="op_single_selector"]
> * [Azure-webinhoud](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 
> 

De [Blob storage-service](../storage/common/storage-introduction.md#blob-storage) in Azure Storage is een van de verschillende Azure gebaseerde oorsprongen geïntegreerd met Azure Content Delivery Network (CDN). Kan een openbaar toegankelijke blobinhoud in cache worden opgeslagen in Azure CDN totdat de time-to-live (TTL) is verstreken. De TTL-waarde wordt bepaald door de `Cache-Control` -header in het HTTP-antwoord van de oorspronkelijke server. Dit artikel wordt beschreven op verschillende manieren kunt u instellen de `Cache-Control` header voor een blob in Azure Storage.

U kunt de cache-instellingen van de Azure-portal ook beheren door de instelling CDN caching-regels. Als u een regel voor opslaan in cache maken en het cachinggedrag ingesteld op **overschrijven** of **cache overslaan**, de oorsprong-opgegeven cache-instellingen die worden beschreven in dit artikel worden genegeerd. Zie voor meer informatie over algemene concepten voor opslaan in cache [hoe caching werkt](cdn-how-caching-works.md).

> [!TIP]
> U kunt geen TTL ingesteld voor een blob. In dit geval Azure CDN automatisch van toepassing is een standaard-TTL zeven dagen, tenzij u een caching-regels in de Azure-portal hebt ingesteld. Deze standaardwaarde voor TTL geldt alleen voor leveringsoptimalisatie gewoon op Internet. De standaard-TTL is één dag voor optimalisatie van grote bestanden, en voor optimalisaties voor streaming van media, de standaard-TTL is één jaar.
> 
> Zie voor meer informatie over de werking van Azure CDN om toegang tot blobs en andere bestanden sneller [overzicht van het Azure Content Delivery Network](cdn-overview.md).
> 
> Zie voor meer informatie over Azure Blob-opslag, [Inleiding tot Blob-opslag](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).
 

## <a name="setting-cache-control-headers-by-using-cdn-caching-rules"></a>Cache-Control-headers instellen met behulp van CDN-regels voor caching
De aanbevolen methode voor het instellen van een blob `Cache-Control` header is met regels voor opslaan in cache in Azure portal. Zie voor meer informatie over CDN regels voor caching [Control Azure CDN caching-gedrag met regels voor caching](cdn-caching-rules.md).

> [!NOTE] 
> Regels voor opslaan in cache zijn alleen beschikbaar voor **Azure CDN Standard van Verizon** en **Azure CDN Standard van Akamai** profielen. Voor **Azure CDN Premium van Verizon** profielen, moet u de [regels-engine van Azure CDN](cdn-rules-engine.md) in de **beheren** portal voor vergelijkbare functionaliteit.

**Om te navigeren naar de pagina CDN caching-regels**:

1. In de Azure-portal, selecteert u een CDN-profiel, en selecteer vervolgens het eindpunt voor de blob.

2. Selecteer in het linkerdeelvenster onder Instellingen de optie **Regels voor opslaan in cache**.

   ![Knop voor opslaan in cache regels CDN](./media/cdn-manage-expiration-of-blob-content/cdn-caching-rules-btn.png)

   De pagina **Regels voor opslaan in cache** wordt weergegeven.

   ![CDN caching-pagina](./media/cdn-manage-expiration-of-blob-content/cdn-caching-page.png)


**Cache-Control-koppen die een Blob storage-service instellen met behulp van algemene regels voor opslaan in cache:**

1. Onder **algemene regels voor caching**, stel **queryreeks cachegedrag** naar **querytekenreeksen negeren** en stel **cachegedrag** naar  **Overschrijven**.
      
2. Voor **Vervaltijd van de Cache**, voer 3600 in de **seconden** box of 1 in de **uur** vak. 

   ![CDN algemene opslaan in cache regels voorbeeld](./media/cdn-manage-expiration-of-blob-content/cdn-global-caching-rules-example.png)

   Deze algemene regel voor opslaan in cache wordt de Cacheduur van een van één uur en is van invloed op alle aanvragen naar het eindpunt. Dit vervangt alle `Cache-Control` of `Expires` HTTP-headers die worden verzonden door de originele server door het eindpunt opgegeven.   

3. Selecteer **Opslaan**.
 
**Een blob-Cache-Control-headers van het bestand met behulp van aangepaste cacheregels instellen:**

1. Onder **aangepaste cacheregels**, maakt u twee criteria voor overeenkomst:

     A. Voor de eerste voorwaarde voor overeenkomst, stelt u **overeenkomen met de voorwaarde** naar **pad** en voer `/blobcontainer1/*` voor **overeenkomen met waarde**. Instellen **cachegedrag** naar **overschrijven** en voert u 4 in de **uur** vak.

    B. Voor de tweede voorwaarde voor overeenkomst, stelt u **overeenkomen met de voorwaarde** naar **pad** en voer `/blobcontainer1/blob1.txt` voor **overeenkomen met waarde**. Instellen **cachegedrag** naar **overschrijven** en voer 2 in de **uur** vak.

    ![Aangepaste caching regels voorbeeld van CDN](./media/cdn-manage-expiration-of-blob-content/cdn-custom-caching-rules-example.png)

    De eerste aangepaste cacheregel Hiermee stelt u een Cacheduur van vier uur voor alle blob-bestanden in de `/blobcontainer1` map op de oorspronkelijke server door het eindpunt opgegeven. De tweede regel overschrijft de eerste regel voor de `blob1.txt` blob-bestand en stelt de Cacheduur van twee uur.

2. Selecteer **Opslaan**.


## <a name="setting-cache-control-headers-by-using-azure-powershell"></a>Cache-Control-headers instellen met behulp van Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure PowerShell](/powershell/azure/overview) is een van de snelste en krachtigste manieren voor het beheren van uw Azure-services. Gebruik de `Get-AzStorageBlob` cmdlet om op te halen van een verwijzing naar de blob, stel de `.ICloudBlob.Properties.CacheControl` eigenschap. 

Bijvoorbeeld:

```powershell
# Create a storage context
$context = New-AzStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> U kunt ook PowerShell gebruiken om te gebruiken [beheren van uw CDN-profielen en eindpunten](cdn-manage-powershell.md).
> 
>

## <a name="setting-cache-control-headers-by-using-net"></a>Cache-Control-headers instellen met behulp van .NET
Om op te geven van een blob `Cache-Control` koptekst met behulp van .NET-code, gebruik de [Azure Storage-clientbibliotheek voor .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) om in te stellen de [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) eigenschap.

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
        CloudBlobContainer <container name> = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob <blob name> = container.GetBlobReference("<blob name>");

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

## <a name="setting-cache-control-headers-by-using-other-methods"></a>Cache-Control-headers instellen met behulp van andere methoden

### <a name="azure-storage-explorer"></a>Azure Opslagverkenner
Met [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/), u kunt bekijken en bewerken van uw blob storage-resources, met inbegrip van eigenschappen zoals de *CacheControl* eigenschap. 

Om bij te werken de *CacheControl* eigenschap van een blob met Azure Storage Explorer:
   1. Selecteer een blob en vervolgens **eigenschappen** in het contextmenu. 
   2. Schuif omlaag naar de *CacheControl* eigenschap.
   3. Voer een waarde in en selecteer vervolgens **opslaan**.


![Azure Storage Explorer-eigenschappen](./media/cdn-manage-expiration-of-blob-content/cdn-storage-explorer-properties.png)

### <a name="azure-command-line-interface"></a>Azure-opdrachtregelinterface
Met de [Azure Command-Line Interface](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) (CLI), kunt u Azure blob-resources beheren vanaf de opdrachtregel. Om in te stellen de cache-control-header tijdens het uploaden van een blob met de Azure CLI, de *cacheControl* eigenschap met behulp van de `-p` overschakelen. Het volgende voorbeeld laat zien hoe de TTL-waarde instellen op één uur (3600 seconden):
  
```azurecli
azure storage blob upload -c <connectionstring> -p cacheControl="max-age=3600" .\<blob name> <container name> <blob name>
```

### <a name="azure-storage-services-rest-api"></a>REST-API van Azure-opslagservices
U kunt de [REST-API van Azure-opslagservices](https://msdn.microsoft.com/library/azure/dd179355.aspx) expliciet in te stellen de *x-ms-blob-cache-control* eigenschap met behulp van de volgende bewerkingen voor een aanvraag:
  
   - [Blob plaatsen](https://msdn.microsoft.com/library/azure/dd179451.aspx)
   - [Lijst met geblokkeerde websites plaatsen](https://msdn.microsoft.com/library/azure/dd179467.aspx)
   - [Blobeigenschappen instellen](https://msdn.microsoft.com/library/azure/ee691966.aspx)

## <a name="testing-the-cache-control-header"></a>Testen van de Cache-Control-header
U kunt eenvoudig de TTL-instellingen van uw blobs controleren. Met van uw browser [hulpprogramma's voor ontwikkelaars](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test uw blob met de `Cache-Control` response-header. U kunt ook een hulpprogramma zoals gebruiken [Wget](https://www.gnu.org/software/wget/), [Postman](https://www.getpostman.com/), of [Fiddler](https://www.telerik.com/fiddler) om te controleren van de antwoordheaders.

## <a name="next-steps"></a>Volgende stappen
* [Informatie over het beheren van de vervaldatum van Cloud Service-inhoud in Azure CDN](cdn-manage-expiration-of-cloud-service-content.md)
* [Meer informatie over concepten opslaan in cache](cdn-how-caching-works.md)

