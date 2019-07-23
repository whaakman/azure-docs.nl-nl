---
title: Een Cognitive Services Text Analytics resource maken
titleSuffix: Azure Cognitive Services
description: Meer informatie over het maken van een Cognitive Services Text Analytics resource.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 6a9363472e1d5770232ca9a0b151fb309d9c4c75
ms.sourcegitcommit: b49431b29a53efaa5b82f9be0f8a714f668c38ab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68377415"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Een Cognitive Services Text Analytics resource maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer **een resource maken**en ga vervolgens naar **AI + machine learning** > **Text Analytics**.
   Of ga naar [Text Analytics maken](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics).
1. Voer alle vereiste instellingen in:

    |Instelling|Waarde|
    |--|--|
    |Name|Voer een naam in (2-64 tekens).|
    |Subscription|Selecteer het juiste abonnement.|
    |Location|Selecteer een locatie in de buurt.|
    |Prijscategorie| Voer **de**prijs categorie Standard in.|
    |Resource group|Selecteer een beschik bare resource groep.|

1. Selecteer **maken**en wacht tot de resource is gemaakt. Uw browser wordt automatisch omgeleid naar de zojuist gemaakte resource pagina.
1. De geconfigureerde `endpoint` en een API-sleutel verzamelen:

    |Het tabblad resource in de portal|Instelling|Value|
    |--|--|--|
    |**Overzicht**|Eindpunt|Kopieer het eind punt. Deze lijkt op `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`.|
    |**Subknooppuntsleutels**|API-sleutel|Kopieer een van de twee sleutels. Het is een alfanumerieke teken reeks van 32 tekens zonder spaties of streepjes: <`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>.|
