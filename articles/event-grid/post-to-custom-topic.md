---
title: Na de gebeurtenis naar aangepaste Azure gebeurtenis raster onderwerp
description: Hierin wordt beschreven hoe u een gebeurtenis naar een aangepaste onderwerp posten voor Azure Event raster
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: tomfitz
ms.openlocfilehash: e4256de1d9112d785b6d1cd52067fc99144a0a04
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34303331"
---
# <a name="post-to-custom-topic-for-azure-event-grid"></a>Voor Azure Event raster naar aangepaste onderwerp boeken

Dit artikel wordt beschreven hoe u een gebeurtenis naar een aangepaste onderwerp boekt. De indeling van de post en gebeurtenis-gegevens worden weergegeven. De [Service Level Agreement (SLA)](https://azure.microsoft.com/support/legal/sla/event-grid/v1_0/) alleen van toepassing op berichten die overeenkomen met de verwachte indeling.

## <a name="endpoint"></a>Eindpunt

Bij het verzenden van de HTTP POST naar een aangepaste onderwerp, gebruikt u de URI-notatie: `https://<topic-endpoint>?api-version=2018-01-01`.

Een geldige URI is bijvoorbeeld: `https://exampletopic.westus2-1.eventgrid.azure.net/api/events?api-version=2018-01-01`.

Als u het eindpunt voor een aangepaste onderwerp met Azure CLI, gebruikt u:

```azurecli-interactive
az eventgrid topic show --name <topic-name> -g <topic-resource-group> --query "endpoint"
```

Als u het eindpunt voor een aangepaste onderwerp met Azure PowerShell, gebruikt u:

```powershell
(Get-AzureRmEventGridTopic -ResourceGroupName <topic-resource-group> -Name <topic-name>).Endpoint
```

## <a name="header"></a>Koptekst

Opnemen in de aanvraag een headerwaarde met de naam `aeg-sas-key` die bevat een sleutel voor verificatie.

Bijvoorbeeld, een geldige headerwaarde is `aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==`.

Als u de sleutel voor een aangepaste onderwerp met Azure CLI, gebruikt u:

```azurecli
az eventgrid topic key list --name <topic-name> -g <topic-resource-group> --query "key1"
```

Als u de sleutel voor een aangepaste onderwerp met PowerShell, gebruikt u:

```powershell
(Get-AzureRmEventGridTopicKey -ResourceGroupName <topic-resource-group> -Name <topic-name>).Key1
```

## <a name="event-data"></a>Gebeurtenisgegevens

Voor aangepaste onderwerpen bevat de gegevens op het hoogste niveau dezelfde velden als resource gedefinieerd gebeurtenissen (standaard). Een van deze eigenschappen is een gegevens-eigenschap met eigenschappen die uniek is voor het aangepaste onderwerp. Als gebeurtenisuitgever, moet u de eigenschappen voor dat gegevensobject bepalen. Gebruik het volgende schema:

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

Zie voor een beschrijving van deze eigenschappen [Azure gebeurtenis raster gebeurtenis schema](event-schema.md). Bij het publiceren van gebeurtenissen naar een onderwerp van het raster gebeurtenis kan een totale grootte van maximaal 1 MB hebben in de matrix. Elke gebeurtenis in de matrix is beperkt tot 64 KB.

Bijvoorbeeld, is een geldige gebeurtenisnaam gegevensschema:

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

Na het boeken naar het eindpunt onderwerp ontvangen reactie. Het antwoord is een standaard HTTP-responscode. Sommige algemene antwoorden zijn:

|Resultaat  |Antwoord  |
|---------|---------|
|Geslaagd  | 200 OK  |
|Gebeurtenisgegevens hebben onjuiste indeling | 400 onjuiste aanvraag |
|Ongeldige toegangstoets | 401-niet toegestaan |
|Onjuiste eindpunt | 404 – Niet gevonden |
|Matrix- of gebeurtenis overschrijdt de maximale grootte | 413 nettolading te groot |

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

* Zie voor meer informatie over het controleren van de gebeurtenis leveringen [Monitor gebeurtenis raster berichtbezorging](monitor-event-delivery.md).
* Zie voor meer informatie over de verificatiesleutel [gebeurtenis raster beveiligings- en verificatie](security-authentication.md).
* Zie voor meer informatie over het maken van een abonnement op Azure gebeurtenis raster [gebeurtenis raster abonnement schema](subscription-creation-schema.md).
