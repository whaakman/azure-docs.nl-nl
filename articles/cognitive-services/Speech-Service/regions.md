---
title: Spraak-service-regio 's
description: Verwijzing voor regio's van de spraak-service.
services: cognitive-services
author: mahilleb-msft
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 09/24/2018
ms.author: mahilleb
ms.openlocfilehash: 8485caeff3a7c96ed8f7403befac0026fae16e90
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987557"
---
# <a name="regions-of-the-speech-service"></a>Regio's van de spraak-service

De spraak-service is beschikbaar in verschillende regio's.
Wanneer u een abonnement maakt, kunt u een beschikbare regio op basis van uw behoeften.

Wanneer u uw abonnement, moet u rekening voor de regio die u hebt geselecteerd.

## <a name="rest-api"></a>REST-API

De REST-API gebruiken om te selecteren van de juiste regiospecifiek-eindpunten.
Zie [REST-API's](rest-apis.md) voor meer informatie.

## <a name="speech-sdk"></a>Speech-SDK

In de [spraak SDK](speech-sdk.md), regio's zijn opgegeven als een tekenreeks (bijvoorbeeld, als een parameter voor `SpeechConfig.FromSubscription` in de spraak-SDK voor C#).

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
