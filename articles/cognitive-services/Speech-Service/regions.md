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
ms.openlocfilehash: 088e581da7511797a0f39959d867c6298262462a
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242327"
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


### <a name="regions-for-intent-recognition"></a>Regio's voor intentieherkenning

Beschikbare regio's voor **intentieherkenning** via de SDK spraak worden weergegeven op de [Language Understanding service regio pagina](/azure/cognitive-services/luis/luis-reference-regions).
Voor elke publishing regio die worden vermeld, wordt de bijbehorende parameter voor spraak SDK regio als het eerste deel van de domeinnaam van het eindpunt bepaald.
Gebruik bijvoorbeeld `westus` om op te geven van de publicatie regio VS-West.
