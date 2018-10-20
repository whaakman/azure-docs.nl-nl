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
ms.openlocfilehash: a5fce6f9547a96da3ce482ce388e5ba2093f2af4
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49468123"
---
# <a name="regions-of-the-speech-service"></a>Regio's van de Spraakservice

De spraak-Service is beschikbaar in verschillende regio's.
Wanneer u een abonnement maakt, kunt u een beschikbare regio op basis van uw behoeften.

Wanneer u uw abonnement, moet u rekening voor de regio die u hebt geselecteerd.

## <a name="rest-api"></a>REST-API

De REST-API gebruiken om te selecteren van de juiste regiospecifiek-eindpunten.
Zie [REST-API's](rest-apis.md) voor meer informatie.

## <a name="speech-sdk"></a>Speech-SDK

In de [Speech Service SDK](speech-sdk.md), regio's zijn opgegeven als een tekenreeks (bijvoorbeeld, als een parameter voor `SpeechConfig.FromSubscription` in de spraak-SDK voor C#).

### <a name="regions-for-speech-recognition-and-translation"></a>Regio's voor spraakherkenning en vertaling

De volgende tabel bevat de beschikbare regio's voor **spraakherkenning** en **vertaling**.

  Regio | Parameter voor spraak-SDK | Portal
 ------|-------|--------
 US - west | `westus` | https://westus.cris.ai
 VS - west 2 | `westus2` | https://westus2.cris.ai 
 US - oost | `eastus` | https://eastus.cris.ai
 US - oost 2 | `eastus2` | https://eastus2.cris.ai
 Azië - oost | `eastasia` | https://eastasia.cris.ai
 Azië - zuidoost | `southeastasia` | https://southeastasia.cris.ai
 Europa - noord | `northeurope` | https://northeurope.cris.ai
 Europa -west | `westeurope` | https://westeurope.cris.ai


### <a name="regions-for-intent-recognition"></a>Regio's voor intentieherkenning

Beschikbare regio's voor **intentieherkenning** via de SDK spraak worden weergegeven op de [Language Understanding service regio pagina](/azure/cognitive-services/luis/luis-reference-regions).
Voor elke publishing regio die worden vermeld, wordt de bijbehorende parameter voor spraak SDK regio als het eerste deel van de domeinnaam van het eindpunt bepaald.
Gebruik bijvoorbeeld `westus` om op te geven van de publicatie regio VS-West.
