---
title: Regio - spraakservices
titlesuffix: Azure Cognitive Services
description: Verwijzing voor regio's van de Speech-Service.
services: cognitive-services
author: mahilleb-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 90fa56bb0e2044d24533c35a051d3f4b644e05b5
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58349634"
---
# <a name="speech-service-supported-regions"></a>Speech Service ondersteunde regio 's

De spraak-service kunt uw toepassing audio naar tekst wilt converteren, voert u spraakomzetting en converteren tekst naar spraak. De service is beschikbaar in meerdere regio's met unieke eindpunten voor de spraak-SDK en REST-API's.

Zorg ervoor dat u het eindpunt dat overeenkomt met de regio voor uw abonnement.

## <a name="speech-sdk"></a>Speech-SDK

In de [spraak SDK](speech-sdk.md), regio's zijn opgegeven als een tekenreeks (bijvoorbeeld, als een parameter voor `SpeechConfig.FromSubscription` in de spraak-SDK voor C#).

### <a name="speech-recognition-and-translation"></a>Spraakherkenning en vertaling

De spraak-SDK is beschikbaar in deze regio's voor **spraakherkenning** en **vertaling**:

  Regio | Parameter voor spraak-SDK | Spraak aanpassing Portal
 ------|-------|--------
 US - west | `westus` | https://westus.cris.ai
 US - west 2 | `westus2` | https://westus2.cris.ai
 US - oost | `eastus` | https://eastus.cris.ai
 US - oost 2 | `eastus2` | https://eastus2.cris.ai
 US - centraal | 'centralus' | https://centralus.cris.ai
 US - noord-centraal | 'northcentralus' | https://northcentralus.cris.ai
 US - zuid-centraal | 'southcentralus' | https://southcentralus.cris.ai
 India - centraal | 'centralindia' | https://centralindia.cris.ai
 Azië - oost | `eastasia` | https://eastasia.cris.ai
 Azië - zuidoost | `southeastasia` | https://southeastasia.cris.ai
 Japan - oost | `japaneast` | https://japaneast.cris.ai
 Korea - centraal | `koreacentral` | https://koreacentral.cris.ai
 Australië - oost | `australiaeast` | https://australiaeast.cris.ai
 Canada - midden | `canadacentral` | https://canadacentral.cris.ai
 Europa - noord | `northeurope` | https://northeurope.cris.ai
 Europa -west | `westeurope` | https://westeurope.cris.ai
 Verenigd Koninkrijk Zuid | `uksouth` | https://uksouth.cris.ai
 Frankrijk - centraal | `francecentral` | https://francecentral.cris.ai

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
