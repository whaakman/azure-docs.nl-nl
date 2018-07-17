---
title: Regio's van de spraakherkenning | Microsoft Docs
description: Verwijzing voor regio's van de spraak-service.
services: cognitive-services
author: mahilleb-msft
manager: wolmfa61
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 06/28/2018
ms.author: mahilleb
ms.openlocfilehash: 11360d163fdba057d373d091d46903cde7789a8b
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071416"
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

De volgende tabel bevat de beschikbare regio's voor **spraakherkenning** en **vertaling**:

Regio| Waarde voor parameter regio in de spraak-SDK
-|-
VS - west| `westus`
Oost-Azië| `eastasia`
Noord-Europa| `northeurope`

Beschikbare regio's voor **intentieherkenning** via de spraak-SDK worden vermeld in de [Language Understanding service regio pagina](/azure/cognitive-services/luis/luis-reference-regions).
Voor elke publishing regio die worden vermeld, wordt de bijbehorende parameter voor spraak SDK regio als het eerste deel van de domeinnaam van het eindpunt bepaald.
Gebruik bijvoorbeeld `westus` om op te geven van de publicatie regio VS-West.
