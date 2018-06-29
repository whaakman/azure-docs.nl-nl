---
title: Spraak service gebieden | Microsoft Docs
description: Verwijzing voor de regio's van de service spraak.
services: cognitive-services
author: mahilleb-msft
manager: wolmfa61
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 06/28/2018
ms.author: mahilleb
ms.openlocfilehash: 1eb3768f5a5c5a27a45dde3f62f862f36fa3e8ac
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098495"
---
# <a name="regions-of-the-speech-service"></a>Gebieden van de service spraak

De spraak-service is beschikbaar in verschillende regio's.
Wanneer u een abonnement maakt, kunt u een beschikbare regio kiezen afhankelijk van uw behoeften.

Wanneer u uw abonnement hebt u het account voor de regio die u verzameld.

## <a name="rest-api"></a>REST-API

Met de REST API, kies de juiste regiospecifieke-eindpunten.
Zie [REST-API's](rest-apis.md) voor meer informatie.



## <a name="speech-sdk"></a>Speech-SDK

In de [spraak SDK](speech-sdk.md), regio's zijn opgegeven als een tekenreeks (bijvoorbeeld als een parameter voor [SpeechFactory.FromSubscription](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechfactory.fromsubscription) in de SDK spraak voor C#).

De volgende tabel bevat de beschikbare regio's voor spraakherkenning en vertaling:

Regio| Waarde voor parameter in de SDK spraak regio
-|-
VS - west| `westus`
Oost-Azië| `eastasia`
Noord-Europa| `northeurope`

Beschikbare regio's voor opzet opname spraak SDK worden vermeld in de [Language Understanding-servicepagina regio](/azure/cognitive-services/luis/luis-reference-regions).
Voor elke publishing regio die wordt vermeld, wordt de bijbehorende parameter voor spraak-SDK regio als het eerste deel van de domeinnaam van het eindpunt bepaald.
Bijvoorbeeld: `westus` om op te geven van de publicatie regio VS-West.
