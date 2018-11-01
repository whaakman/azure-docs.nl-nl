---
title: Gebeurtenis-domeinen in Azure Event Grid
description: Hierin wordt beschreven hoe gebeurtenis domeinen worden gebruikt voor het beheren van de onderwerpen in de Azure Event Grid.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: fe66ca8b8f5b4474290e302f73b35868dce68caa
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50633912"
---
# <a name="understand-event-domains-for-managing-event-grid-topics"></a>Meer informatie over gebeurtenis domeinen voor het beheren van Event Grid-onderwerpen

In dit artikel wordt beschreven hoe u Event domeinen gebruiken voor het beheren van de stroom van aangepaste gebeurtenissen naar uw verschillende bedrijven, organisaties, klanten of toepassingen. Gebeurtenis-domeinen te gebruiken:

* Multitenant eventing architecturen op schaal beheren.
* Beheer uw autorisatie en verificatie.
* Partitioneren uw onderwerpen zonder dat elk afzonderlijk worden beheerd.
* Vermijd afzonderlijk publiceren naar elk van de eindpunten van uw onderwerp.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]


## <a name="event-domains-overview"></a>Overzicht van de Event-domeinen

Een gebeurtenis-domein is een hulpprogramma voor grote aantallen Event Grid-onderwerpen met betrekking tot de dezelfde toepassing. U kunt deze beschouwen als een meta-onderwerp dat duizenden afzonderlijke onderwerpen kan bevatten.

Gebeurtenis-domeinen kunt u de architectuur van Azure-services zoals opslag- en IoT Hub gebruiken voor het publiceren van de gebeurtenissen weergegeven die beschikbaar zijn voor gebruik in uw eigen systeem. Hiermee kunt u gebeurtenissen tot duizenden onderwerpen publiceren. Domeinen ook krijgt u autorisatie en verificatie controle over elk onderwerp zodat u uw tenants kunt partitioneren.

### <a name="example-use-case"></a>Voorbeeld van de use-case

Gebeurtenis domeinen worden gebruikt met behulp van een voorbeeld van de meest eenvoudige beschreven. Stel dat u Contoso bouwmachines, waarbij de productie van door de constructie bepaalde, bestuderen apparatuur en andere zware machines worden uitgevoerd. Als onderdeel van het uitvoeren van het bedrijf, pusht u realtime gegevens naar klanten met betrekking tot apparatuur onderhoud, status van sitesystemen, contract updates enzovoort. Al deze gegevens naar verschillende eindpunten, met inbegrip van uw app, de klant eindpunten gaat, en andere klanten infrastructuur hebt ingesteld.

Gebeurtenis-domeinen kunt u model Contoso bouw machine als één eventing entiteit. Elke klant wordt weergegeven als een onderwerp in het domein / verificatie en autorisatie worden afgehandeld met behulp van Azure Active Directory. Elk van uw klanten kunt abonneren op hun onderwerp en voor hen ophalen van de gebeurtenissen die worden geleverd. Beheerde toegang via het domein van de gebeurtenis zorgt ervoor dat ze alleen toegang tot hun onderwerp.

Het biedt ook een één-eindpunt, waarmee u alle van de klant gebeurtenissen kunt publiceren. Event Grid zorgt ervoor dat elk onderwerp is alleen op de hoogte van de gebeurtenissen binnen het bereik van de tenant.

![Voorbeeld van de Contoso-constructie](./media/event-domains/contoso-construction-example.png)

## <a name="access-management"></a>Toegangsbeheer

 Met een domein krijgt u verfijnde autorisatie en verificatie controle over elk onderwerp via Azure rol op basis van toegang controleren (RBAC). U kunt deze rollen gebruiken om te beperken van elke tenant in uw toepassing alleen de onderwerpen die u wilt toegang te verlenen.

Rollen op basis van toegang controleren (RBAC) in domeinen van de gebeurtenis op dezelfde manier werkt [beheerd toegangsbeheer](https://docs.microsoft.com/azure/event-grid/security-authentication#management-access-control) werkt in de rest van Event Grid en Azure. RBAC gebruiken om te maken en af te dwingen aangepaste roldefinities in gebeurtenis-domeinen.

### <a name="built-in-roles"></a>Ingebouwde rollen

Event Grid heeft twee ingebouwde roldefinities RBAC om gemakkelijker te maken:

#### <a name="eventgrid-eventsubscription-contributor-preview"></a>EventGrid EventSubscription Inzender (Preview)

```json
[
  {
    "Description": "Lets you manage EventGrid event subscription operations.",
    "IsBuiltIn": true,
    "Id": "428e0ff05e574d9ca2212c70d0e0a443",
    "Name": "EventGrid EventSubscription Contributor (Preview)",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/*",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Resources/deployments/*",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": [],
        "Condition": null
      }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

#### <a name="eventgrid-eventsubscription-reader-preview"></a>EventGrid EventSubscription Reader (Preview)

```json
[
  {
    "Description": "Lets you read EventGrid event subscriptions.",
    "IsBuiltIn": true,
    "Id": "2414bbcf64974faf8c65045460748405",
    "Name": "EventGrid EventSubscription Reader (Preview)",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/read",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": []
       }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

## <a name="subscribing-to-topics"></a>Onderwerpen over abonneren op

Abonneren op gebeurtenissen op een onderwerp in het domein van een gebeurtenis is hetzelfde als [een gebeurtenisabonnement maken op een aangepast onderwerp](./custom-event-quickstart.md) of zich abonneert op een van de ingebouwde gebeurtenis uitgevers Azure biedt.

### <a name="domain-scope-subscriptions"></a>Domein bereik abonnementen

Gebeurtenis-domeinen kunnen ook voor domein bereik abonnementen. Een gebeurtenisabonnement op een domein gebeurtenis ontvangt alle gebeurtenissen die worden verzonden aan het domein, ongeacht het onderwerp dat de gebeurtenissen worden verzonden naar. Domein bereik abonnementen is handig voor het beheer- en controledoeleinden.

## <a name="publishing-to-an-event-domain"></a>Publiceren naar een gebeurtenis-domein

Wanneer u een gebeurtenis-domein maakt, krijgt u een publicatie-eindpunt die vergelijkbaar is met als u een onderwerp hebt gemaakt in Event Grid. 

Voor het publiceren van gebeurtenissen naar een onderwerp in het domein van een gebeurtenis, kunt u de gebeurtenissen pusht naar eindpunt van het domein de [dezelfde manier als u dat zou doen voor een aangepast onderwerp](./post-to-custom-topic.md). Het enige verschil is dat u het onderwerp dat u de gebeurtenis dat wilt moet worden geleverd aan moet opgeven.

Publiceren van de volgende reeks gebeurtenissen zouden worden verzonden, gebeurtenis met `"id": "1111"` onderwerp `foo` terwijl de gebeurtenis met `"id": "2222"` worden verzonden naar onderwerp `bar`:

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

Gebeurtenis domeinen verwerken publiceren naar onderwerpen die voor u. In plaats van het publiceren van gebeurtenissen naar elk onderwerp dat u afzonderlijk beheren, kunt u alle gebeurtenissen publiceren naar het eindpunt van het domein en Event Grid zorgt ervoor te zorgen dat elke gebeurtenis wordt verzonden naar het juiste onderwerp.

## <a name="limits-and-quotas"></a>Limieten en quota

### <a name="control-plane"></a>Besturingselement vlak

Tijdens de preview, wordt gebeurtenis-domeinen worden beperkt tot 1000 onderwerpen binnen een domein en 50 abonnementen per onderwerp binnen een domein. Bereik van de gebeurtenisabonnementen domein wordt ook worden beperkt tot 50.

### <a name="data-plane"></a>Gegevenslaag

Gebeurtenis doorvoer voor het domein van een gebeurtenis zijn beperkt tot dezelfde 5000 gebeurtenissen per seconde opnemen snelheid die aangepaste onderwerpen zijn beperkt tot de Preview-versie.

## <a name="pricing"></a>Prijzen

De Preview-versie gebeurtenis domeinen gebruikt hetzelfde [operations prijzen](https://azure.microsoft.com/pricing/details/event-grid/) die gebruikmaken van alle andere functies in Event Grid.

Operations werken op dezelfde manier in domeinen van de gebeurtenis als in aangepaste onderwerpen. Elke invoer van een gebeurtenis aan een domein van de gebeurtenis is een bewerking, en elke bezorgingspoging voor een gebeurtenis is een bewerking.

## <a name="next-steps"></a>Volgende stappen

* Voor meer informatie over het instellen van gebeurtenis domeinen, het maken van onderwerpen, abonnementen maken en publiceren van gebeurtenissen, Zie [gebeurtenis domeinen beheren](./how-to-event-domains.md).