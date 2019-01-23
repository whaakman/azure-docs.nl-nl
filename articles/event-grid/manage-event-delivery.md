---
title: Dead-letter en beleid voor opnieuw proberen voor Azure Event Grid-abonnementen
description: Beschrijft hoe u event Bezorgingsopties voor Event Grid aanpassen. Dead-letter uitvoeren voor doel ingesteld en geef op hoe lang levering opnieuw uit te voeren.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/06/2019
ms.author: spelluru
ms.openlocfilehash: 8660f09c41cf6226f2ffb173508d37c260522b80
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54474241"
---
# <a name="dead-letter-and-retry-policies"></a>Dead-letter en beleid voor opnieuw proberen

Bij het maken van een gebeurtenisabonnement, kunt u de instellingen voor de bezorging van gebeurtenissen kunt aanpassen. Dit artikel leest u hoe een dead-letter-locatie instellen en aanpassen van de instellingen voor opnieuw proberen. Zie voor meer informatie over deze functies [bezorging van berichten van Event Grid en probeer het opnieuw](delivery-and-retry.md).

## <a name="install-preview-feature"></a>Preview-functie installeren

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-dead-letter-location"></a>Dead-letter uitvoeren voor locatie instellen

Als u wilt een dead-letter-locatie instellen, moet u een opslagaccount voor de opslag van gebeurtenissen die naar een eindpunt kunnen niet worden bezorgd. De voorbeelden Haal de resource-ID van een bestaand opslagaccount. Ze maken een gebeurtenisabonnement die gebruikmaakt van een container in het storage-account voor het eindpunt dead-letter uitvoeren.

### <a name="azure-cli"></a>Azure-CLI

```azurecli-interactive
# If you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

containername=testcontainer

topicid=$(az eventgrid topic show --name demoTopic -g gridResourceGroup --query id --output tsv)
storageid=$(az storage account show --name demoStorage --resource-group gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Als u wilt uitschakelen onbestelbare, opnieuw de opdracht om het gebeurtenisabonnement te maken, maar geen waarde opgeeft voor `deadletter-endpoint`. U hoeft niet te verwijderen van het gebeurtenisabonnement.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

$containername = "testcontainer"

$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id
$storageid = (Get-AzureRmStorageAccount -ResourceGroupName gridResourceGroup -Name demostorage).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeadLetterEndpoint "$storageid/blobServices/default/containers/$containername"
```

Als u wilt uitschakelen onbestelbare, opnieuw de opdracht om het gebeurtenisabonnement te maken, maar geen waarde opgeeft voor `DeadLetterEndpoint`. U hoeft niet te verwijderen van het gebeurtenisabonnement.

## <a name="set-retry-policy"></a>Set-beleid voor opnieuw proberen

Bij het maken van een Event Grid-abonnement, kunt u waarden voor hoe lang Event Grid proberen moet te leveren van de gebeurtenis instellen. Standaard probeert Event Grid voor 24 uur (1440 minuten) of 30 keer beter. U kunt een van deze waarden instellen voor uw event grid-abonnement. De waarde voor time-to-live-gebeurtenis moet een geheel getal tussen 1 en 1440 liggen. De waarde voor maximale aantal pogingen moet een geheel getal tussen 1 en 30.

U kunt geen configureren de [opnieuw plannen](delivery-and-retry.md#retry-schedule-and-duration).

### <a name="azure-cli"></a>Azure-CLI

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

Om in te stellen het maximale aantal pogingen op een andere waarde dan 30, gebruikt u:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --max-delivery-attempts 18
```

Als u zowel `event-ttl` en `max-deliver-attempts`, Event Grid maakt gebruik van de eerste verlopen om te bepalen bij het stoppen van de bezorging van gebeurtenissen.

### <a name="powershell"></a>PowerShell

Om in te stellen de gebeurtenis time-to-live naar een andere waarde dan 1440 minuten, gebruikt u:

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -EventTtl 720
```

Om in te stellen het maximale aantal pogingen op een andere waarde dan 30, gebruikt u:

```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -MaxDeliveryAttempt 18
```

Als u zowel `EventTtl` en `MaxDeliveryAttempt`, Event Grid maakt gebruik van de eerste verlopen om te bepalen bij het stoppen van de bezorging van gebeurtenissen.

## <a name="next-steps"></a>Volgende stappen

* Zie voor een voorbeeldtoepassing die gebruikmaakt van een Azure-functie-app dead-letter om gebeurtenissen te verwerken, [Azure Event Grid Dead-Letter-voorbeelden voor .NET](https://azure.microsoft.com/resources/samples/event-grid-dotnet-handle-deadlettered-events/).
* Voor informatie over de bezorging van gebeurtenissen en nieuwe pogingen, [bezorging van berichten van Event Grid en probeer het opnieuw](delivery-and-retry.md).
* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Als u wilt snel aan de slag met Event Grid, Zie [aangepaste gebeurtenissen maken en routeren met Azure Event Grid](custom-event-quickstart.md).
