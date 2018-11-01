---
title: Over het beheren van grote sets van Azure Event Grid-onderwerpen en publiceren van gebeurtenissen op met behulp van de gebeurtenis domeinen
description: Laat zien hoe maken en beheren van de onderwerpen in de Azure Event Grid en publiceren van gebeurtenissen voor met behulp van de gebeurtenis domeinen.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: 48a5356b03e38e864ba76f048febdb0b040893f5
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50633909"
---
# <a name="manage-topics-and-publish-events-using-event-domains"></a>Onderwerpen beheren en publiceren van gebeurtenissen via Event-domeinen

Dit artikel wordt beschreven hoe u:

* Maken van een Event Grid-domein
* Abonneren op onderwerpen
* Een lijst met sleutels
* Gebeurtenissen publiceren aan een domein

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="create-an-event-domain"></a>Het domein van een gebeurtenis maken

Het maken van een domein gebeurtenis kan worden gedaan de `eventgrid` -extensie voor [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Als u een domein hebt gemaakt, kunt u deze kunt gebruiken voor het beheren van grote gegevenssets van onderwerpen.

```azurecli-interactive
# if you haven't already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid domain create \
  -g <my-resource-group> \
  --name <my-domain-name>
  -l <location>
```

Is gemaakt, wordt het volgende geretourneerd:

```json
{
  "endpoint": "https://<my-domain-name>.westus2-1.eventgrid.azure.net/api/events",
  "id": "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>",
  "inputSchema": "EventGridSchema",
  "inputSchemaMapping": null,
  "location": "westus2",
  "name": "<my-domain-name>",
  "provisioningState": "Succeeded",
  "resourceGroup": "<my-resource-group>",
  "tags": null,
  "type": "Microsoft.EventGrid/domains"
}
```

Houd er rekening mee de `endpoint` en `id` als ze moeten beheren van het domein en publiceren van gebeurtenissen.

## <a name="create-topics-and-subscriptions"></a>Maken van onderwerpen en abonnementen

De Event Grid-service maakt en beheert het desbetreffende onderwerp in een domein op basis van de aanroep voor het maken van een gebeurtenisabonnement voor een onderwerp domein automatisch. Er is geen afzonderlijke stap het maken van een onderwerp in een domein. Op dezelfde manier als een abonnement op de laatste gebeurtenis voor een onderwerp wordt verwijderd, is het onderwerp ook verwijderd.

Abonneren op een onderwerp in een domein is hetzelfde als het abonneren op een andere Azure-resource:

```azurecli-interactive
az eventgrid event-subscription create \
  --name <event-subscription> \
  --resource-id "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/<my-topic>" \
  --endpoint https://contoso.azurewebsites.net/api/f1?code=code
```

De opgegeven resource-ID is de dezelfde ID geretourneerd bij het maken van het domein eerder. Als u wilt instellen in het onderwerp dat u zich wilt abonneren, `/topics/<my-topic>` aan het einde van de resource-ID.

Voor het maken van een domein bereik gebeurtenisabonnement die alle gebeurtenissen in het domein ontvangt, geeft u het domein als de `resource-id` zonder op te geven alle onderwerpen die bijvoorbeeld `/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>`.

Als u een test-eindpunt voor de gebeurtenissen abonneren nodig hebt, kunt u altijd implementeren een [vooraf gemaakte web-app](https://github.com/Azure-Samples/azure-event-grid-viewer) waarmee de binnenkomende gebeurtenissen worden weergegeven. U kunt uw gebeurtenissen verzenden naar de website van uw test op `https://<your-site-name>.azurewebsites.net/api/updates`.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

Machtigingen die zijn ingesteld voor een onderwerp worden opgeslagen in Azure Active Directory en moeten expliciet worden verwijderd. Een gebeurtenisabonnement verwijderen wordt niet intrekken van een gebruikerstoegang voor het maken van gebeurtenisabonnementen als ze schrijftoegang voor een onderwerp hebben.

## <a name="manage-access-to-topics"></a>Toegang tot onderwerpen beheren

Beheer van toegang tot onderwerpen wordt uitgevoerd via [roltoewijzing](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-cli). Roltoewijzing gebruikt rol gebaseerde toegangscontrole voor het beperken van bewerkingen op Azure-resources tot gemachtigde gebruikers met een bepaald bereik.

Event Grid heeft twee ingebouwde rollen die u gebruiken kunt om toe te wijzen bepaalde gebruikers toegang over verschillende onderwerpen binnen een domein. Deze rollen zijn `EventGrid EventSubscription Contributor (Preview)`, waarmee voor maken en verwijderen van abonnementen, en `EventGrid EventSubscription Reader (Preview)`, zodat alleen kunt lijst gebeurtenisabonnementen.

De volgende opdracht zou beperken `alice@contoso.com` te maken en verwijderen van gebeurtenisabonnementen alleen op onderwerp `foo`:

```azurecli-interactive
az role assignment create --assignee alice@contoso.com --role "EventGrid EventSubscription Contributor (Preview)" --scope /subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/foo
```

Zie [Event Grid-beveiliging en verificatie](./security-authentication.md) voor meer informatie over:

* Beheer van toegangsbeheer
* Bewerkingstypen
* Het maken van aangepaste roldefinities

## <a name="publish-events-to-an-event-grid-domain"></a>Publiceren van gebeurtenissen naar een Event Grid-domein

Publiceren van gebeurtenissen naar een domein is hetzelfde als [publiceren naar een aangepast onderwerp](./post-to-custom-topic.md). Het enige verschil is dat u nodig hebt om op te geven van het onderwerp dat u wilt dat elke gebeurtenis naar. De volgende reeks gebeurtenissen zou leiden tot gebeurtenis met `"id": "1111"` onderwerp `foo` tijdens gebeurtenis met `"id": "2222"` worden verzonden naar onderwerp `bar`:

```json
[{
  "topic": "foo",
  "id": "1111",
  "eventType": "maintenanceRequested",
  "subject": "myapp/vehicles/diggers",
  "eventTime": "2018-10-30T21:03:07+00:00",
  "data": {
    "make": "Contoso",
    "model": "Small Digger"
  },
  "dataVersion": "1.0"
},
{
  "topic": "bar",
  "id": "2222",
  "eventType": "maintenanceCompleted",
  "subject": "myapp/vehicles/tractors",
  "eventTime": "2018-10-30T21:04:12+00:00",
  "data": {
    "make": "Contoso",
    "model": "Big Tractor"
  },
  "dataVersion": "1.0"
}]
```

Als u de sleutels voor een domein gebruikt:

```azurecli-interactive
az eventgrid domain key list \
  -g <my-resource-group> \
  -n <my-domain>
```

En vervolgens met uw favoriete methode van het maken van een HTTP POST naar uw gebeurtenissen publiceren naar uw Event Grid-domein.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de geavanceerde concepten in gebeurtenis-domeinen en waarom ze handig zijn, de [conceptueel overzicht van domeinen van de gebeurtenis](./event-domains.md).