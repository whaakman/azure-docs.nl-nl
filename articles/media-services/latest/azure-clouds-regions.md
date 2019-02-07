---
title: Clouds en regio's in welke Azure Media Services v3 beschikbaar is | Microsoft Docs
description: In dit artikel vertelt Azure-clouds en regio's in welke Azure Media Services v3 beschikbaar is.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/11/2019
ms.author: juliako
ms.openlocfilehash: d176152429ecac1ed4e570533f1bc0426cc7655f
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55767449"
---
# <a name="clouds-and-regions-in-which-azure-media-services-v3-exists"></a>Clouds en regio's in welke Azure Media Services v3 bestaat

Azure Media Services v3 is beschikbaar via Azure Resource Manager-manifest in globale Azure, Azure Government, Azure Duitsland, Azure China 21Vianet. Niet alle Media Services-functies zijn echter beschikbaar in alle Azure-clouds. Dit document bevat een overzicht van de beschikbaarheid van de belangrijkste onderdelen van de Media Services v3.

## <a name="feature-availability-in-azure-clouds"></a>Beschikbaarheid van functies in Azure-clouds

| Functie|Globale Azure-regio 's | Azure Government|Azure Duitsland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| [Azure EventGrid](reacting-to-media-services-events.md) | Beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |
| [VideoAnalyzerPreset](analyzing-video-audio-files-concept.md) |  Beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |
| [AudioAnalyzerPreset](analyzing-video-audio-files-concept.md) |  Beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |
| [StandardEncoderPreset](encoding-concept.md) | Beschikbaar | Beschikbaar | Beschikbaar | Beschikbaar |
| [LiveEvents](live-streaming-overview.md) | Beschikbaar | Beschikbaar | Beschikbaar | Beschikbaar |
| [StreamingEndpoints](streaming-endpoint-concept.md) | Beschikbaar | Beschikbaar | Beschikbaar | Beschikbaar |

## <a name="regionsgeographieslocations"></a>Landen/regio's / locaties

* [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/)
* [Producten per regio](https://azure.microsoft.com/global-infrastructure/services/)
* [Azure-geografieën](https://azure.microsoft.com/global-infrastructure/geographies/)
* [Azure-locaties](https://azure.microsoft.com/global-infrastructure/locations/)

## <a name="region-code-name"></a>Regionaam 

Wanneer moet u opgeven de **locatie** parameter, moet u opgeven de naam van de regio-code als de **locatie** waarde. Als u de naam van de code van de regio waar uw account in en dat de aanroep moet worden doorgestuurd naar, kunt u de volgende regel uitvoeren in [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)

```bash
az account list-locations
```

Nadat u de regel hierboven, krijgt u een lijst met alle Azure-regio's worden uitgevoerd. Navigeer naar de Azure-regio met de *displayName* u zoekt en gebruiken de *naam* waarde voor de **locatie** parameter.

Bijvoorbeeld, voor de Azure-regio VS-West 2 (hieronder weergegeven), wordt u 'westus2' voor de **locatie** parameter.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/00000000-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="next-steps"></a>Volgende stappen

[Overzicht van Media Services v3](media-services-overview.md)
