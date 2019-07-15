---
title: Een Cognitive Services Text Analytics-resource maken
titleSuffix: Azure Cognitive Services
description: Informatie over het maken van een Cognitive Services Text Analytics-resource.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 7e8b4480911f00afa8524ef4b81d697bb8ee5bcc
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67877461"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Een Cognitive Services Text Analytics-resource maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer **een resource maken**, en ga vervolgens naar **AI + Machine Learning** > **Tekstanalyse**.
   Of Ga naar [Tekstanalyse maken](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics).
1. Voer de vereiste instellingen:

    |Instelling|Waarde|
    |--|--|
    |Name|Voer een naam (2-64 tekens)|
    |Subscription|Selecteer het juiste abonnement|
    |Location|Selecteer een locatie in de buurt|
    |Prijscategorie| Voer **S**, de prijscategorie standard|
    |Resource group|Selecteer een beschikbare resourcegroep|

1. Selecteer **maken** en wacht tot de resource moet worden gemaakt. Uw browser wordt automatisch omgeleid naar de zojuist gemaakte resource-pagina.
1. Verzamel de geconfigureerde `endpoint` en een API-sleutel:

    |Resource-tabblad in de portal|Instelling|Waarde|
    |--|--|--|
    |**Overzicht**|Eindpunt|Kopieer het eindpunt. Deze wordt weergegeven die vergelijkbaar is met `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |**sleutels**|API-sleutel|Kopieer een van de twee sleutels. Dit is een alfanumerieke tekenreeks van 32 tekens zonder spaties of streepjes bevatten: <`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>.|
