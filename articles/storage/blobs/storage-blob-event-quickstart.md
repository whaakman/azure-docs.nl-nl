---
title: Azure Blob storage-gebeurtenissen te routeren naar een aangepaste website-eindpunt | Microsoft Docs
description: Gebruik Azure Event Grid om u te abonneren op gebeurtenissen van Blob Storage.
services: storage,event-grid
keywords: ''
author: cbrooksmsft
ms.author: cbrooks
ms.date: 01/30/2018
ms.topic: article
ms.service: storage
ms.openlocfilehash: f0764ebc423cfb5323f2b634ce5a5ecbe075135c
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="route-blob-storage-events-to-a-custom-web-endpoint-with-azure-cli"></a>Blob storage gebeurtenissen routeren naar een aangepaste website-eindpunt met Azure CLI

Azure Event Grid is een gebeurtenisservice voor de cloud. In dit artikel gebruikt u de Azure CLI om u te abonneren op gebeurtenissen van Blob Storage, waarna u vervolgens een gebeurtenis activeert om het resultaat weer te geven. 

Meestal stuurt u gebeurtenissen naar een eindpunt dat reageert op de gebeurtenis, zoals een webhook of Azure-functie. Ter vereenvoudiging van het voorbeeld in dit artikel sturen we de gebeurtenissen naar een URL die de berichten alleen maar verzamelt. U maakt deze URL met behulp van een hulpprogramma van derden, [Hookbin](https://hookbin.com/).

> [!NOTE]
> **Hookbin** is niet bedoeld voor gebruik met hoge doorvoer. Dit hulpprogramma wordt alleen gebruikt ter demonstratie. Als u meer dan een gebeurtenis tegelijk pusht, ziet u mogelijk niet alle gebeurtenissen in het hulpprogramma.

Als u de stappen in dit artikel hebt voltooid, ziet u dat de gegevens van gebeurtenissen naar een eindpunt zijn verzonden.

[!INCLUDE [quickstarts-free-trial-note.md](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel de nieuwste versie van Azure CLI (2.0.24 of hoger) uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).

Als u Cloud Shell niet gebruikt, moet u eerst zich aanmelden met `az login`.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Event Grid-onderwerpen zijn Azure-resources en moeten in een Azure-resourcegroep worden geplaatst. De resourcegroep is een logische verzameling waarin Azure-resources worden geïmplementeerd en beheerd.

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az_group_create). 

In het volgende voorbeeld wordt een resourcegroep met de naam `<resource_group_name>` gemaakt op de locatie *westcentralus*.  Vervang `<resource_group_name>` door een unieke naam voor uw resourcegroep.

```azurecli-interactive
az group create --name <resource_group_name> --location westcentralus
```

## <a name="create-a-storage-account"></a>Create a storage account

Als u gebeurtenissen van Blob-opslag, moet u ofwel een [Blob-opslagaccount](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) of een [algemeen v2-opslagaccount](../common/storage-account-options.md#general-purpose-v2). **Algemene doel v2 (GPv2)** storage-accounts die ondersteuning bieden voor alle functies voor storage-services, met inbegrip van Blobs, bestanden, wachtrijen en tabellen zijn. Een **Blob-opslagaccount** is een gespecialiseerd opslagaccount voor het opslaan van ongestructureerde gegevens als blobs (objecten) in Azure Storage. BLOB storage-accounts zijn vergelijkbaar met opslagaccounts voor algemeen gebruik en delen van de meeste duurzaamheid, beschikbaarheid, schaalbaarheid en prestaties van functies dat u vandaag inclusief 100 procent API-consistentie voor blok-blobs gebruikt en toevoeg-blobs. Voor toepassingen die alleen blok- of toevoeg-blob-opslag nodig hebben, wordt het gebruik van Blob-opslagaccounts aangeraden. 

> [!NOTE]
> De beschikbaarheid van gebeurtenissen voor opslag is gekoppeld aan de gebeurtenis raster [beschikbaarheid](../../event-grid/overview.md) en zijn binnenkort beschikbaar in andere regio's zoals het raster gebeurtenis.

Vervang `<storage_account_name>` door een unieke naam voor uw opslagaccount en `<resource_group_name>` door de resourcegroep die u eerder hebt gemaakt.

```azurecli-interactive
az storage account create \
  --name <storage_account_name> \
  --location westcentralus \
  --resource-group <resource_group_name> \
  --sku Standard_LRS \
  --kind BlobStorage \
  --access-tier Hot
```

## <a name="create-a-message-endpoint"></a>Het eindpunt van een bericht maken

Voordat u zich abonneert op het onderwerp, gaan we het eindpunt voor het gebeurtenisbericht maken. In plaats van code te schrijven om op de gebeurtenis te reageren, maken we een eindpunt waarop de berichten worden verzameld, zodat u ze kunt weergeven. Hookbin is een hulpprogramma van derden waarmee u een eindpunt kunt maken en aanvragen kunt weergeven die naar dit eindpunt worden verzonden. Ga naar [Hookbin](https://hookbin.com/) en klik op **Create New Endpoint**.  Kopieer de URL. U hebt deze nodig wanneer u zich abonneert op het onderwerp.

## <a name="subscribe-to-your-storage-account"></a>Abonneren op uw storage-account

U abonneert u op een onderwerp om Event Grid te laten weten welke gebeurtenissen u wilt traceren. Het opslagaccount hebt gemaakt de URL van Hookbin doorgegeven als het eindpunt voor gebeurtenismelding lid van het volgende voorbeeld. Vervang `<event_subscription_name>` door een unieke naam voor het gebeurtenisabonnement, en `<endpoint_URL>` door de waarde uit de voorgaande sectie. Door een eindpunt op te geven wanneer u zich abonneert, wordt via Event Grid de routering van gebeurtenissen naar dit eindpunt verwerkt. Gebruik voor `<resource_group_name>` en `<storage_account_name>` de waarden die u eerder hebt gemaakt.  

```azurecli-interactive
storageid=$(az storage account show --name <storage_account_name> --resource-group <resource_group_name> --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $storageid \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
```

## <a name="trigger-an-event-from-blob-storage"></a>Een gebeurtenis van Blob Storage activeren

Nu gaan we een gebeurtenis activeren om te zien hoe het bericht via Event Grid naar het eindpunt wordt gedistribueerd. Eerst laten we configureert u de naam en sleutel voor het opslagaccount we een container maken, klikt u vervolgens maken en uploaden van een bestand. Gebruik voor `<storage_account_name>` en `<resource_group_name>` weer de waarden die u eerder hebt gemaakt.

```azurecli-interactive
export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_ACCESS_KEY="$(az storage account keys list --account-name <storage_account_name> --resource-group <resource_group_name> --query "[0].value" --output tsv)"

az storage container create --name testcontainer

touch testfile.txt
az storage blob upload --file testfile.txt --container-name testcontainer --name testfile.txt
```

U hebt de gebeurtenis geactiveerd, en de gebeurtenis is via Event Grid verzonden naar het eindpunt dat u hebt geconfigureerd toen u zich abonneerde. Blader naar de eindpunt-URL die u eerder hebt gemaakt. U kunt ook op Vernieuwen klikken in het geopende browservenster. U ziet de gebeurtenis die u zojuist hebt verzonden. 

```json
[{
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Storage/storageAccounts/myblobstorageaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-08-16T20:33:51.0595757Z",
  "id": "4d96b1d4-0001-00b3-58ce-16568c064fab",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "d65ca2e2-a168-4155-b7a4-2c925c18902f",
    "requestId": "4d96b1d4-0001-00b3-58ce-16568c000000",
    "eTag": "0x8D4E4E61AE038AD",
    "contentType": "text/plain",
    "contentLength": 0,
    "blobType": "BlockBlob",
    "url": "https://myblobstorageaccount.blob.core.windows.net/testcontainer/testblob1.txt",
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

Vervang `<resource_group_name>` door de resourcegroep die u eerder hebt gemaakt.

```azurecli-interactive
az group delete --name <resource_group_name>
```

## <a name="next-steps"></a>Volgende stappen

U weet nu hoe u onderwerpen en gebeurtenisabonnementen maakt. Raadpleeg deze onderwerpen voor meer informatie over gebeurtenissen van Blob Storage en waar Event Grid u nog meer bij kan helpen:

- [Reageren op gebeurtenissen van Blob Storage](storage-blob-event-overview.md)
- [Over Event Grid](../../event-grid/overview.md)
