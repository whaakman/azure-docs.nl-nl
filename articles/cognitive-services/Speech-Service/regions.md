---
title: Spraak-service-regio 's
description: Verwijzing voor regio's van de spraak-service.
services: cognitive-services
author: mahilleb-msft
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 06/28/2018
ms.author: mahilleb
ms.openlocfilehash: d651a0a9e006e996db44b82eeaf0a1242a4114e0
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46296087"
---
# <a name="regions-of-the-speech-service"></a>Regio's van de spraak-service

De spraak-service is beschikbaar in verschillende regio's.
Wanneer u een abonnement maakt, kunt u een beschikbare regio kunt kiezen, afhankelijk van uw behoeften.

Wanneer u uw abonnement hebt u het account voor de regio die u hebt geselecteerd.

## <a name="rest-api"></a>REST-API

Met behulp van de REST-API, kies de juiste regiospecifiek-eindpunten.
Zie [REST-API's](rest-apis.md) voor meer informatie.

## <a name="speech-sdk"></a>Speech-SDK

In de [spraak SDK](speech-sdk.md), regio's zijn opgegeven als een tekenreeks (bijvoorbeeld, als een parameter voor [SpeechFactory.FromSubscription](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechfactory.fromsubscription) in de spraak-SDK voor C#).

### <a name="regions-for-speech-recognition-and-translation"></a>Regio's voor spraakherkenning en vertaling

De volgende tabel bevat de beschikbare regio's voor **spraakherkenning** en **vertaling**:

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

Beschikbare regio's voor **intentieherkenning** via de spraak-SDK worden vermeld in de [Language Understanding service regio pagina](/azure/cognitive-services/luis/luis-reference-regions).
Voor elke publishing regio die worden vermeld, wordt de bijbehorende parameter voor spraak SDK regio als het eerste deel van de domeinnaam van het eindpunt bepaald.
Gebruik bijvoorbeeld `westus` om op te geven van de publicatie regio VS-West.
