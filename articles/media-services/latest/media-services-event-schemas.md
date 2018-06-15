---
title: Azure Event raster schema's voor Media Services-gebeurtenissen
description: Beschrijft de eigenschappen die beschikbaar zijn voor Media Services-gebeurtenissen met Azure Event raster
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: c443ea755c9e7e88b4685682d5e43f675d42efa4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788128"
---
# <a name="azure-event-grid-schemas-for-media-services-events"></a>Azure Event raster schema's voor Media Services-gebeurtenissen

In dit artikel biedt de eigenschappen en schema's voor Media Services-gebeurtenissen.

## <a name="media-services-job-state-change-event"></a>Taak voor Media Services-statuswijzigingsgebeurtenis

Deze sectie bevat dat de eigenschappen en het schema voor Media Services-taakstatus gebeurtenis wijzigen.  

### <a name="available-event-types"></a>Typen beschikbare gebeurtenissen

Een Media Services **taak** verzendt de volgende typen gebeurtenissen:

| Gebeurtenistype | Beschrijving |
| ---------- | ----------- |
| Microsoft.Media.JobStateChange| Deze gebeurtenis treedt op wanneer de status van de wijzigingen van de taak. |

### <a name="example-event"></a>Voorbeeld van de gebeurtenis

Het volgende voorbeeld ziet u het schema van een voltooide taakstatus: 

```json
[
  {
    "topic": "/subscriptions/{subscription id}/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount",
    "subject": "transforms/VideoAnalyzerTransform/jobs/{job id}",
    "eventType": "Microsoft.Media.JobStateChange",
    "eventTime": "2018-04-20T21:26:13.8978772",
    "id": "<id>",
    "data": {
      "previousState": "Processing",
      "state": "Finished"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

### <a name="event-properties"></a>Eigenschappen van gebeurtenis

Een gebeurtenis heeft de volgende gegevens op het hoogste niveau:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| Onderwerp | tekenreeks | Gebeurtenis raster bevat deze waarde. |
| Onderwerp | tekenreeks | Het bronpad onder de bron van Media Services-account. |
| EventType | tekenreeks | Een van de typen van de geregistreerde gebeurtenis van de bron van deze gebeurtenis. Bijvoorbeeld 'Microsoft.Media.JobStateChange'. |
| eventTime | tekenreeks | De tijd dat de gebeurtenis wordt gegenereerd, gebaseerd op de UTC-tijd van de provider. |
| id | tekenreeks | De unieke id voor de gebeurtenis. |
| gegevens | object | Media Services-gebeurtenisgegevens. |
| dataVersion | tekenreeks | De versie van het schema van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | tekenreeks | De versie van het schema van de metagegevens van de gebeurtenis. Gebeurtenis raster definieert het schema van de eigenschappen op het hoogste niveau. Gebeurtenis raster bevat deze waarde. |

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| previousState | tekenreeks | De status van de taak voor de gebeurtenis. |
| state | tekenreeks | De nieuwe status van de taak wordt een melding in deze gebeurtenis. Bijvoorbeeld ' in de wachtrij geplaatst: de taak wacht op resources ' of ' gepland: de taak is om te beginnen met '.|

Waar de taakstatus kan zijn een van de waarden: *in de wachtrij geplaatst*, *geplande*, *verwerken*, *voltooid*, *fout*, *Geannuleerd*, *annuleren*

## <a name="next-steps"></a>Volgende stappen

[Registreren voor de taak statuswijzigingsgebeurtenissen](job-state-events-cli-how-to.md)
