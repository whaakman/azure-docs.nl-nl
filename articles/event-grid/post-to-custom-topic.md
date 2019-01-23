---
title: Post-gebeurtenis om aangepaste Azure Event Grid-onderwerp
description: Beschrijft hoe u een gebeurtenis in een aangepast onderwerp plaatsen voor Azure Event Grid
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: b219e9475151ecd14d8b45db9501a06cde05875b
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54470593"
---
# <a name="post-to-custom-topic-for-azure-event-grid"></a>Plaatsen op aangepast onderwerp voor Azure Event Grid

In dit artikel wordt beschreven hoe u een gebeurtenis om een aangepast onderwerp te plaatsen. Hier ziet u de indeling van de boeken en gebeurtenis-gegevens. De [Service Level Agreement (SLA)](https://azure.microsoft.com/support/legal/sla/event-grid/v1_0/) alleen van toepassing op berichten die overeenkomen met de verwachte indeling.

## <a name="endpoint"></a>Eindpunt

Wanneer u de HTTP POST naar een aangepast onderwerp verzendt, gebruikt u de URI-notatie: `https://<topic-endpoint>?api-version=2018-01-01`.

Bijvoorbeeld, is het een geldige URI: `https://exampletopic.westus2-1.eventgrid.azure.net/api/events?api-version=2018-01-01`.

Voor het eindpunt voor een aangepast onderwerp met Azure CLI, gebruikt u:

```azurecli-interactive
az eventgrid topic show --name <topic-name> -g <topic-resource-group> --query "endpoint"
```

Voor het eindpunt voor een aangepast onderwerp met Azure PowerShell, gebruikt u:

```powershell
(Get-AzureRmEventGridTopic -ResourceGroupName <topic-resource-group> -Name <topic-name>).Endpoint
```

## <a name="header"></a>Header

Opnemen in de aanvraag een headerwaarde met de naam `aeg-sas-key` die bevat een sleutel voor verificatie.

Bijvoorbeeld, een geldige headerwaarde is `aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==`.

Voor de sleutel voor een aangepast onderwerp met Azure CLI, gebruikt u:

```azurecli
az eventgrid topic key list --name <topic-name> -g <topic-resource-group> --query "key1"
```

Voor de sleutel voor een aangepast onderwerp met PowerShell, gebruikt u:

```powershell
(Get-AzureRmEventGridTopicKey -ResourceGroupName <topic-resource-group> -Name <topic-name>).Key1
```

## <a name="event-data"></a>Gebeurtenisgegevens

Voor aangepaste onderwerpen bevat de gegevens op het hoogste niveau dezelfde velden als standaard resource gedefinieerde gebeurtenissen. Een van deze eigenschappen is een gegevens-eigenschap met eigenschappen die uniek is voor het aangepaste onderwerp. Als de uitgever van gebeurtenissen, kunt u de eigenschappen voor dat gegevensobject bepalen. Gebruik het volgende schema:

```json
[
  {
    "id": string,    
    "eventType": string,
    "subject": string,
    "eventTime": string-in-date-time-format,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string
  }
]
```

Zie voor een beschrijving van deze eigenschappen [Azure Event Grid-gebeurtenisschema](event-schema.md). Als u gebeurtenissen naar een event grid-onderwerp boeken, kan de matrix een totale grootte van maximaal 1 MB hebben. Elke gebeurtenis in de matrix is beperkt tot 64 KB.

Bijvoorbeeld, is een schema van de gegevens geldig gebeurtenis:

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2017-08-10T21:03:07+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "dataVersion": "1.0"
}]
```

## <a name="response"></a>Antwoord

Na het boeken met het eindpunt onderwerp, moet u een antwoord ontvangt. Het antwoord is een standaard HTTP-responscode. Sommige algemene antwoorden zijn:

|Resultaat  |Antwoord  |
|---------|---------|
|Geslaagd  | 200 OK  |
|Gebeurtenisgegevens heeft een onjuiste indeling | 400-Ongeldige aanvraag |
|Ongeldige toegangssleutel | 401-niet toegestaan |
|Onjuiste eindpunt | 404 – Niet gevonden |
|Matrix of een gebeurtenis overschrijdt de maximale grootte | 413 payload te groot |

Voor fouten heeft de hoofdtekst van het bericht de volgende indeling:

```json
{
    "error": {
        "code": "<HTTP status code>",
        "message": "<description>",
        "details": [{
            "code": "<HTTP status code>",
            "message": "<description>"
    }]
  }
}
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het controleren van de gebeurtenis leveringen [Monitor Event Grid berichtbezorging](monitor-event-delivery.md).
* Zie voor meer informatie over de verificatiesleutel [Event Grid-beveiliging en verificatie](security-authentication.md).
* Zie voor meer informatie over het maken van een Azure Event Grid-abonnement [Event Grid-abonnementsschema](subscription-creation-schema.md).
