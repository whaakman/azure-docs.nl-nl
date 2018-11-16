---
title: Spraak-Service-regio 's
titlesuffix: Azure Cognitive Services
description: Verwijzing voor regio's van de Speech-Service.
services: cognitive-services
author: mahilleb-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mahilleb
ms.openlocfilehash: 8d36036332e939075ffac8763bec9c23d8e4a3f7
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51712888"
---
# <a name="speech-service-supported-regions"></a>Speech Service ondersteunde regio 's

De spraak-service kunt uw toepassing audio naar tekst wilt converteren, voert u spraakomzetting en converteren tekst naar spraak. De service is beschikbaar in meerdere regio's met unieke eindpunten voor de spraak-SDK en REST-API's.

Zorg ervoor dat u het eindpunt dat overeenkomt met de regio voor uw abonnement.

## <a name="speech-sdk"></a>Speech-SDK

In de [Speech Service SDK](speech-sdk.md), regio's zijn opgegeven als een tekenreeks (bijvoorbeeld, als een parameter voor `SpeechConfig.FromSubscription` in de spraak-SDK voor C#).

### <a name="speech-recognition-and-translation"></a>Spraakherkenning en vertaling

De spraak-SDK is beschikbaar in deze regio's voor **spraakherkenning** en **vertaling**:

  Regio | Parameter voor spraak-SDK | Spraak aanpassing Portal
 ------|-------|--------
 US - west | `westus` | https://westus.cris.ai
 VS - west 2 | `westus2` | https://westus2.cris.ai
 US - oost | `eastus` | https://eastus.cris.ai
 US - oost 2 | `eastus2` | https://eastus2.cris.ai
 Azië - oost | `eastasia` | https://eastasia.cris.ai
 Azië - zuidoost | `southeastasia` | https://southeastasia.cris.ai
 Europa - noord | `northeurope` | https://northeurope.cris.ai
 Europa -west | `westeurope` | https://westeurope.cris.ai


### <a name="intent-recognition"></a>Intentieherkenning

**Intentieherkenning** voor de shares spraak SDK-regio's met LUIS ondersteunen. Zie voor een volledige lijst van beschikbare regio's, [publiceren regio's en -eindpunten - LUIS](https://docs.microsoft.comazure/cognitive-services/luis/luis-reference-regions)

Beschikbare regio's voor **intentieherkenning** via de SDK spraak worden weergegeven op de [Language Understanding service regio pagina](/azure/cognitive-services/luis/luis-reference-regions).

Voor elke publishing regio die wordt weergegeven, gebruikt u de opgegeven **API regionaam**. Gebruik bijvoorbeeld `westus` voor VS-West.

## <a name="rest-apis"></a>REST-API’s

De spraak-service wordt ook aangegeven REST-eindpunten voor spraak-naar-tekst en spraak-aanvragen.

### <a name="speech-to-text"></a>Spraak-naar-tekst

Zie voor spraak-naar-tekst naslagdocumentatie [REST-API's](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#speech-to-text).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="text-to-speech"></a>Tekst-naar-spraak

Zie voor de Text to Speech naslagdocumentatie [REST-API's](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#speech-to-text).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]
