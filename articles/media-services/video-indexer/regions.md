---
title: Regio's waarin Video Indexer beschikbaar is - Azure
titlesuffix: Azure Media Services
description: In dit artikel vertelt Azure-regio's waarin Video Indexer beschikbaar is.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: 404aaf91c0cb30df0a83353ef7397987ec3f8e80
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65799418"
---
# <a name="azure-regions-in-which-video-indexer-exists"></a>Azure-regio's waarin Video Indexer bestaat

Video Indexer-API's bevatten een **locatie** parameter die u moet instellen op de Azure-regio waarnaar u de oproep worden gerouteerd. Dit moet een [Azure-regio waarin Video Indexer beschikbaar is](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).

## <a name="locations"></a>Locaties

De **locatie** parameter de naam van de code Azure-regio als de waarde moet worden opgegeven. Als u van Video Indexer in preview-modus gebruikmaakt, plaatst u *"proefversie"* als de waarde. Anders, als u de naam van de code van de Azure-regio waar uw account in en dat de aanroep moet worden doorgestuurd naar, kunt u de volgende regel in uitvoeren [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest):

```bash
az account list-locations
```

Nadat u de regel hierboven, krijgt u een lijst met alle Azure-regio's worden uitgevoerd. Navigeer naar de Azure-regio met de *displayName* u zoekt en gebruiken de *naam* waarde voor de **locatie** parameter.

Bijvoorbeeld, voor de Azure-regio VS-West 2 (hieronder weergegeven), wordt u 'westus2' voor de **locatie** parameter.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/35c2594a-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="next-steps"></a>Volgende stappen

- [Taalmodel met behulp van API's aanpassen](customize-language-model-with-api.md)
- [Pas merken model met behulp van API 's](customize-brands-model-with-api.md)
- [Persoon model met behulp van API's aanpassen](customize-person-model-with-api.md)
