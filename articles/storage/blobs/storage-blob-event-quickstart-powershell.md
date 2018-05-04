---
title: Azure Blob storage-gebeurtenissen te routeren naar een aangepaste website-eindpunt - Powershell | Microsoft Docs
description: Gebruik Azure Event Grid om u te abonneren op gebeurtenissen van Blob Storage.
services: storage,event-grid
keywords: ''
author: david-stanford
ms.author: dastanfo
ms.date: 01/30/2018
ms.topic: article
ms.service: storage
ms.openlocfilehash: 6d7ccd94243d7064008197518f6194d5837b17be
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="route-blob-storage-events-to-a-custom-web-endpoint-with-powershell"></a>Blob storage gebeurtenissen routeren naar een aangepaste website-eindpunt met PowerShell

Azure Event Grid is een gebeurtenisservice voor de cloud. In dit artikel, Azure PowerShell gebruiken om u te abonneren op gebeurtenissen in de Blob-opslag, trigger een gebeurtenis en het resultaat te bekijken. 

Meestal stuurt u gebeurtenissen naar een eindpunt dat reageert op de gebeurtenis, zoals een webhook of Azure-functie. Om te vereenvoudigen het voorbeeld in dit artikel, worden gebeurtenissen verzonden naar een URL die alleen de berichten worden verzameld. U maakt deze URL met behulp van een hulpprogramma van derden, [Hookbin](https://hookbin.com/).

> [!NOTE]
> **Hookbin** is niet bedoeld voor gebruik met hoge doorvoer. Dit hulpprogramma wordt alleen gebruikt ter demonstratie. Als u meer dan een gebeurtenis tegelijk pusht, ziet u mogelijk niet alle gebeurtenissen in het hulpprogramma.

Als u de stappen in dit artikel hebt voltooid, ziet u dat de gegevens van gebeurtenissen naar een eindpunt zijn verzonden.

## <a name="setup"></a>Instellen

Voor dit artikel moet u de nieuwste versie van Azure PowerShell uitvoeren. Zie [Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps) als u de toepassing nog moet installeren of een upgrade moet uitvoeren.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Connect-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Connect-AzureRmAccount
```

> [!NOTE]
> De beschikbaarheid van gebeurtenissen voor opslag is gekoppeld aan de gebeurtenis raster [beschikbaarheid](../../event-grid/overview.md) en zijn binnenkort beschikbaar in andere regio's zoals het raster gebeurtenis.

In dit voorbeeld wordt **westus2** en slaat u de selectie in een variabele voor gebruik in.

```powershell
$location = "westus2"
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Event Grid-onderwerpen zijn Azure-resources en moeten in een Azure-resourcegroep worden geplaatst. De resourcegroep is een logische verzameling waarin Azure-resources worden geïmplementeerd en beheerd.

Maak een resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).

In het volgende voorbeeld wordt een resourcegroep met de naam **gridResourceGroup** gemaakt op de locatie **westus2**.  

```powershell
$resourceGroup = "gridResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location
```

## <a name="create-a-storage-account"></a>Create a storage account

Als u gebeurtenissen van Blob-opslag, moet u ofwel een [Blob-opslagaccount](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) of een [algemeen v2-opslagaccount](../common/storage-account-options.md#general-purpose-v2). **Algemene doel v2 (GPv2)** storage-accounts die ondersteuning bieden voor alle functies voor storage-services, met inbegrip van Blobs, bestanden, wachtrijen en tabellen zijn. Een **Blob-opslagaccount** is een gespecialiseerd opslagaccount voor het opslaan van ongestructureerde gegevens als blobs (objecten) in Azure Storage. BLOB storage-accounts zijn vergelijkbaar met opslagaccounts voor algemeen gebruik en delen van de meeste duurzaamheid, beschikbaarheid, schaalbaarheid en prestaties van functies dat u vandaag inclusief 100 procent API-consistentie voor blok-blobs gebruikt en toevoeg-blobs. Voor toepassingen die alleen blok- of toevoeg-blob-opslag nodig hebben, wordt het gebruik van Blob-opslagaccounts aangeraden.  

Een Blob storage-account maken met het gebruik van de replicatie LRS [nieuw AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount), haal vervolgens de context van het opslagaccount waarin de storage-account moet worden gebruikt. Wanneer optreedt op een storage-account, maar u verwijzen naar de context in plaats van herhaaldelijk geven de referenties. In dit voorbeeld maakt u een opslagaccount aangeroepen **gridstorage** met lokaal redundante opslag (LRS). 

> [!NOTE]
> Namen van opslagaccounts zijn in een globale naamruimte, dus moet u bepaalde willekeurige tekens toevoegen aan de naam in dit script.

```powershell
$storageName = "gridstorage"
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind BlobStorage `
  -AccessTier Hot

$ctx = $storageAccount.Context
```

## <a name="create-a-message-endpoint"></a>Het eindpunt van een bericht maken

Voordat u zich abonneert op het onderwerp, gaan we het eindpunt voor het gebeurtenisbericht maken. In plaats van code te schrijven om op de gebeurtenis te reageren, maken we een eindpunt waarop de berichten worden verzameld, zodat u ze kunt weergeven. Hookbin is een hulpprogramma van derden waarmee u een eindpunt kunt maken en aanvragen kunt weergeven die naar dit eindpunt worden verzonden. Ga naar [Hookbin](https://hookbin.com/) en klik op **Create New Endpoint**. Kopieer de URL van de opslaglocatie en vervang `<bin URL>` in het volgende script.

```powershell
$binEndPoint = "<bin URL>"
```

## <a name="subscribe-to-your-storage-account"></a>Abonneren op uw storage-account

U abonneert u op een onderwerp om Event Grid te laten weten welke gebeurtenissen u wilt traceren. Het opslagaccount hebt gemaakt de URL van Hookbin doorgegeven als het eindpunt voor gebeurtenismelding lid van het volgende voorbeeld. 

```powershell
$storageId = (Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup -AccountName $storageName).Id
New-AzureRmEventGridSubscription `
  -EventSubscriptionName gridBlobQuickStart `
  -Endpoint $binEndPoint `
  -ResourceId $storageId
```

## <a name="trigger-an-event-from-blob-storage"></a>Een gebeurtenis van Blob Storage activeren

Nu gaan we een gebeurtenis activeren om te zien hoe het bericht via Event Grid naar het eindpunt wordt gedistribueerd. Eerst gaan we een container en een object te maken. Vervolgens laten we het object te uploaden naar de container.

```powershell
$containerName = "gridcontainer"
New-AzureStorageContainer -Name $containerName -Context $ctx

echo $null >> gridTestFile.txt

Set-AzureStorageBlobContent -File gridTestFile.txt -Container $containerName -Context $ctx -Blob gridTestFile.txt
```

U hebt de gebeurtenis geactiveerd, en de gebeurtenis is via Event Grid verzonden naar het eindpunt dat u hebt geconfigureerd toen u zich abonneerde. Blader naar de eindpunt-URL die u eerder hebt gemaakt. U kunt ook op Vernieuwen klikken in het geopende browservenster. U ziet de gebeurtenis die u zojuist hebt verzonden. 

```json
[{
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Storage/storageAccounts/myblobstorageaccount",
  "subject": "/blobServices/default/containers/gridcontainer/blobs/gridTestFile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-08-16T20:33:51.0595757Z",
  "id": "4d96b1d4-0001-00b3-58ce-16568c064fab",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "Azure-Storage-PowerShell-d65ca2e2-a168-4155-b7a4-2c925c18902f",
    "requestId": "4d96b1d4-0001-00b3-58ce-16568c000000",
    "eTag": "0x8D4E4E61AE038AD",
    "contentType": "application/octet-stream",
    "contentLength": 0,
    "blobType": "BlockBlob",
    "url": "https://myblobstorageaccount.blob.core.windows.net/gridcontainer/gridTestFile.txt",
    "sequencer": "00000000000000EB0000000000046199",
    "storageDiagnostics": {
      "batchId": "dffea416-b46e-4613-ac19-0371c0c5e352"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]

```

## <a name="clean-up-resources"></a>Resources opschonen
Als u verder wilt werken met dit opslagaccount en dit gebeurtenisabonnement, moet u de resources die u hebt gemaakt in dit artikel niet opschonen. Als u niet verder wilt werken, gebruikt u de volgende opdracht om de resources te verwijderen die u in dit artikel hebt gemaakt.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Volgende stappen

U weet nu hoe u onderwerpen en gebeurtenisabonnementen maakt. Raadpleeg deze onderwerpen voor meer informatie over gebeurtenissen van Blob Storage en waar Event Grid u nog meer bij kan helpen:

- [Reageren op gebeurtenissen van Blob Storage](storage-blob-event-overview.md)
- [Over Event Grid](../../event-grid/overview.md)
