---
title: Bezorgingsinstellingen voor Azure Event Grid-abonnementen beheren
description: Beschrijft hoe u event Bezorgingsopties voor Event Grid aanpassen.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: tomfitz
ms.openlocfilehash: e91ee640d18e2cf804be33fd130bf48737c9efb1
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035666"
---
# <a name="manage-event-grid-delivery-settings"></a>Event Grid bezorgingsinstellingen beheren

Bij het maken van een gebeurtenisabonnement, kunt u de instellingen voor de bezorging van gebeurtenissen kunt aanpassen. U kunt instellen hoe lang Event Grid wordt geprobeerd het bericht te bezorgen. U kunt een storage-account te gebruiken voor het opslaan van gebeurtenissen die naar een eindpunt kunnen niet worden bezorgd instellen.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-retry-policy"></a>Set-beleid voor opnieuw proberen

Bij het maken van een Event Grid-abonnement, kunt u waarden voor hoe lang Event Grid proberen moet te leveren van de gebeurtenis instellen. Standaard Event Grid probeert 24 uur (1440 minuten) en probeert een maximum van 30 keer beter. U kunt een van deze waarden instellen voor uw event grid-abonnement.

Om in te stellen de gebeurtenis time-to-live naar een andere waarde dan 1440 minuten, gebruikt u:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --event-ttl 720
```

Als u wilt het maximale aantal nieuwe pogingen ingesteld op een andere waarde dan 30, gebruiken:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --max-delivery-attempts 18
```

Als u zowel `event-ttl` en `max-deliver-attempts`, Event Grid maakt gebruik van de eerste verlopen voor nieuwe pogingen.

## <a name="set-dead-letter-location"></a>Dead-letter uitvoeren voor locatie instellen

Wanneer een gebeurtenis kan niet van Event Grid leveren, kan de niet-bezorgde gebeurtenis verzenden naar een opslagaccount. Dit proces staat bekend als onbestelbare. Standaard inschakelen Event Grid onbestelbare niet. Als u wilt inschakelen, moet u een opslagaccount voor niet-bezorgde gebeurtenissen bij het maken van het gebeurtenisabonnement. U gebeurtenissen van dit opslagaccount wordt gebruikt om op te lossen leveringen op te halen.

Als alle van de nieuwe pogingen is geprobeerd, of als er een foutbericht dat aangeeft dat levering nooit slaagt, Event Grid een gebeurtenis verzendt naar de dead-letter-locatie. Bijvoorbeeld, als Event Grid een onjuiste indelingsfout ontvangt bij het leveren van een gebeurtenis, verzendt het onmiddellijk dat de gebeurtenis naar de dead-letter-locatie.

Voordat u de locatie voor onbestelbare berichten, moet u een opslagaccount met een container hebt. U kunt het eindpunt voor deze container opgeven bij het maken van het gebeurtenisabonnement. Het eindpunt is in de indeling van: `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`

Het volgende script opgehaald van de resource-ID van een bestaand opslagaccount en maakt u een gebeurtenisabonnement die gebruikmaakt van een container in het storage-account voor het eindpunt dead-letter uitvoeren.

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
  --endpoint <endpoint_URL> \
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Event Grid gebruiken om te reageren op gebeurtenissen die door niet-bezorgde [een gebeurtenisabonnement maken](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) voor de dead-letter uitvoeren voor blob-opslag. Telkens wanneer de dead-letter uitvoeren voor blob-opslag een niet-bezorgde gebeurtenis ontvangt, meldt Event Grid de handler. De handler reageert met de acties die u wilt deelnemen aan voor het afhandelen van niet-bezorgde gebeurtenissen. 

Als u wilt uitschakelen onbestelbare, opnieuw de opdracht om het gebeurtenisabonnement te maken, maar geen waarde opgeeft voor `deadletter-endpoint`. U hoeft niet te verwijderen van het gebeurtenisabonnement.

## <a name="next-steps"></a>Volgende stappen

* Voor informatie over de bezorging van gebeurtenissen en nieuwe pogingen, [bezorging van berichten van Event Grid en probeer het opnieuw](delivery-and-retry.md).
* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Als u wilt snel aan de slag met Event Grid, Zie [aangepaste gebeurtenissen maken en routeren met Azure Event Grid](custom-event-quickstart.md).
