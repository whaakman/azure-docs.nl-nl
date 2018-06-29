---
title: Instellingen voor Azure Event raster abonnementen beheren
description: Beschrijft hoe de Bezorgingsopties gebeurtenis voor gebeurtenis raster aanpassen.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: tomfitz
ms.openlocfilehash: 65e79f492e96c418502e096b60992ba992868dd7
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035944"
---
# <a name="manage-event-grid-delivery-settings"></a>Gebeurtenis raster bezorgingsinstellingen beheren

U kunt de instellingen voor de levering van de gebeurtenis bij het maken van een gebeurtenisabonnement. U kunt instellen hoe lang gebeurtenis raster probeert het bericht te bezorgen. U kunt instellen dat een opslagaccount te gebruiken voor het opslaan van gebeurtenissen die naar een eindpunt kunnen niet worden bezorgd.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-retry-policy"></a>Set-beleid voor opnieuw proberen

Bij het maken van een gebeurtenis raster-abonnement, kunt u waarden voor hoe lang gebeurtenis raster proberen moet te leveren van de gebeurtenis kunt instellen. Standaard gebeurtenis raster probeert 24 uur (1440 minuten) en maximaal 30 keer geprobeerd. U kunt een van beide waarden instellen voor uw abonnement op gebeurtenissen raster.

U stelt de gebeurtenis time to live naar een andere waarde dan 1440 minuten met:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
  --event-ttl 720
```

Als u het maximale aantal nieuwe pogingen ingesteld op een andere waarde dan 30, wilt gebruiken:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
  --max-delivery-attempts 18
```

Als u zowel `event-ttl` en `max-deliver-attempts`, gebeurtenis raster gebruikt de eerste om te laten verlopen voor het opnieuw probeert.

## <a name="set-dead-letter-location"></a>Onbestelbare locatie instellen

Als gebeurtenis raster niet van een gebeurtenis afleveren, kan de niet-uitgevoerde gebeurtenis verzenden naar een opslagaccount. Dit proces staat bekend als de verwerking van onbestelbare berichten. Standaard inschakelen gebeurtenis raster verwerking van onbestelbare berichten niet. Als u wilt inschakelen, moet u een opslagaccount voor het opslaan van niet-uitgevoerde gebeurtenissen bij het maken van het gebeurtenisabonnement. Ophalen van gebeurtenissen van dit opslagaccount leveringen omzetten.

Gebeurtenis raster verzendt een gebeurtenis naar de locatie van onbestelbare als alle van de nieuwe pogingen probeert, of als het ontvangt een foutbericht dat aangeeft dat levering nooit slaagt. Bijvoorbeeld, als raster gebeurtenis een onjuiste indelingsfout ontvangt bij het leveren van een gebeurtenis, onmiddellijk het die gebeurtenis naar de locatie van onbestelbare berichten.

Voordat u de locatie van de wachtrij voor onbestelbare instelt, moet u een opslagaccount met een container hebben. U kunt het eindpunt voor deze container opgeven bij het maken van het gebeurtenisabonnement. Het eindpunt is in de indeling van: `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`

Het volgende script opgehaald van de bron-ID van een bestaand opslagaccount en maakt een gebeurtenisabonnement die gebruikmaakt van een container in dit opslagaccount voor het eindpunt voor onbestelbare berichten.

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

storagename=demostorage
containername=testcontainer

storageid=$(az storage account show --name $storagename --resource-group gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Gebeurtenis raster gebruiken om te reageren op niet-uitgevoerde gebeurtenissen [een gebeurtenisabonnement](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) voor de onbestelbare blobopslag. Wanneer uw blobopslag voor onbestelbare berichten ontvangt een niet-uitgevoerde gebeurtenis raster gebeurtenis ontvangt een melding van de handler. De handler reageert met de acties die u maken wilt voor het afhandelen van niet-uitgevoerde gebeurtenissen. 

## <a name="next-steps"></a>Volgende stappen

* Voor informatie over de levering van de gebeurtenis en nieuwe pogingen, [gebeurtenis raster berichtbezorging en probeer het opnieuw](delivery-and-retry.md).
* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Zie om snel aan de slag met Event raster [maken en route aangepaste gebeurtenissen met Azure Event raster](custom-event-quickstart.md).
