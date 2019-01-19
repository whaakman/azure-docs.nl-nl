---
title: Regio - spraakservices
titlesuffix: Azure Cognitive Services
description: Verwijzing voor regio's van de Speech-Service.
services: cognitive-services
author: mahilleb-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: mahilleb
ms.custom: seodec18
ms.openlocfilehash: e33bf7cd98cdd5862af6f4d68d3d73de1a07d229
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2019
ms.locfileid: "54413155"
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
 US - west 2 | `westus2` | https://westus2.cris.ai
 US - oost | `eastus` | https://eastus.cris.ai
 US - oost 2 | `eastus2` | https://eastus2.cris.ai
 Azië - oost | `eastasia` | https://eastasia.cris.ai
 Azië - zuidoost | `southeastasia` | https://southeastasia.cris.ai
 Europa - noord | `northeurope` | https://northeurope.cris.ai
 Europa -west | `westeurope` | https://westeurope.cris.ai


### <a name="intent-recognition"></a>Intentieherkenning

Beschikbare regio's voor **intentieherkenning** via de spraak-SDK worden de volgende:

 Globale regio | Regio | Parameter voor spraak-SDK
 ------|-------|--------
 Azië | Azië - oost | `eastasia`
 Azië | Azië - zuidoost | `southeastasia`
 Australië | Australië - oost | `australiaeast`
 Europa | Europa - noord | `northeurope`
 Europa | Europa -west | `westeurope`
 Noord-Amerika | US - oost | `eastus`
 Noord-Amerika | US - oost 2 | `eastus2`
 Noord-Amerika | US - zuid-centraal | `southcentralus`
 Noord-Amerika | US - west-centraal | `westcentralus`
 Noord-Amerika | US - west | `westus`
 Noord-Amerika | US - west 2 | `westus2`
 Zuid-Amerika | Brazilië - zuid | `brazilsouth`

Dit is een subset van de publicatie regio's ondersteund door de [Language Understanding-service (LUIS)](/azure/cognitive-services/luis/luis-reference-regions).

## <a name="rest-apis"></a>REST-API’s

De spraak-service wordt ook aangegeven REST-eindpunten voor spraak-naar-tekst en spraak-aanvragen.

### <a name="speech-to-text"></a>Spraak-naar-tekst

Zie voor spraak-naar-tekst naslagdocumentatie [REST-API's](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="text-to-speech"></a>Tekst naar spraak

Zie voor de Text to Speech naslagdocumentatie [REST-API's](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]
