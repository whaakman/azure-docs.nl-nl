---
title: Spraak containers installeren
titleSuffix: Azure Cognitive Services
description: Details over de configuratieopties voor spraak overkoepelende helm-grafiek.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: ed64412ccf9d192506fafe546b1ccee7941aa43a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711811"
---
### <a name="speech-umbrella-chart"></a>Spraak (overkoepelende grafiek)

Waarden in de grafiek op het hoogste niveau 'overkoepelende"overschrijven de waarden in de bijbehorende onderliggende grafiek. Daarom alle on-premises aangepaste waarden moeten hier niet worden toegevoegd.

|Parameter|Description|Standaard|
| -- | -- | -- | -- |
| `speechToText.enabled` | Of de **spraak-naar-tekst** -service is ingeschakeld. | `true` |
| `speechToText.verification.enabled` | Of de `helm test` mogelijkheden voor **spraak-naar-tekst** -service is ingeschakeld. | `true` |
| `speechToText.verification.image.registry` | De opslagplaats voor docker-installatiekopieën die `helm test` wordt gebruikt voor het testen van **spraak-naar-tekst** service. Helm maakt u afzonderlijke pod binnen het cluster voor het testen en haalt de *test-gebruik* installatiekopie van het register. | `docker.io` |
| `speechToText.verification.image.repository` | De opslagplaats voor docker-installatiekopieën die `helm test` wordt gebruikt voor het testen van **spraak-naar-tekst** service. Helm test pod maakt gebruik van deze opslagplaats om op te halen *test-gebruik* installatiekopie. | `antsu/on-prem-client` |
| `speechToText.verification.image.tag` | De docker-afbeeldingscode gebruikt in combinatie met `helm test` voor **spraak-naar-tekst** service. Deze code om op te halen maakt gebruik van helm test pod *test-gebruik* installatiekopie. | `latest` |
| `speechToText.verification.image.pullByHash` | Of de *test-gebruik* docker-installatiekopie is opgehaald door hash. Als `true`, `speechToText.verification.image.hash` met geldige afbeelding hash-waarde moet worden toegevoegd. | `false` |
| `speechToText.verification.image.arguments` | De argumenten gebruikt om uit te voeren de *test-gebruik* docker-installatiekopie. Helm test pod wordt deze argumenten doorgegeven aan de container bij het uitvoeren van `helm test`. | `"./speech-to-text-client"`<br/> `"./audio/whatstheweatherlike.wav"` <br/> `"--expect=What's the weather like"`<br/>`"--host=$(SPEECH_TO_TEXT_HOST)"`<br/>`"--port=$(SPEECH_TO_TEXT_PORT)"` |
| `textToSpeech.enabled` | Of de **tekst naar spraak** -service is ingeschakeld. | `true` |
| `textToSpeech.verification.enabled` | Of de `helm test` mogelijkheden voor **spraak-naar-tekst** -service is ingeschakeld. | `true` |
| `textToSpeech.verification.image.registry` | De opslagplaats voor docker-installatiekopieën die `helm test` wordt gebruikt voor het testen van **spraak-naar-tekst** service. Helm maakt u afzonderlijke pod binnen het cluster voor het testen en haalt de *test-gebruik* installatiekopie van het register. | `docker.io` |
| `textToSpeech.verification.image.repository` | De opslagplaats voor docker-installatiekopieën die `helm test` wordt gebruikt voor het testen van **spraak-naar-tekst** service. Helm test pod maakt gebruik van deze opslagplaats om op te halen *test-gebruik* installatiekopie. | `antsu/on-prem-client` |
| `textToSpeech.verification.image.tag` | De docker-afbeeldingscode gebruikt in combinatie met `helm test` voor **spraak-naar-tekst** service. Deze code om op te halen maakt gebruik van helm test pod *test-gebruik* installatiekopie. | `latest` |
| `textToSpeech.verification.image.pullByHash` | Of de *test-gebruik* docker-installatiekopie is opgehaald door hash. Als `true`, `textToSpeech.verification.image.hash` met geldige afbeelding hash-waarde moet worden toegevoegd. | `false` |
| `textToSpeech.verification.image.arguments` | De argumenten om uit te voeren met de *test-gebruik* docker-installatiekopie. De test helm schil wordt deze argumenten doorgegeven aan de container bij het uitvoeren van `helm test`. | `"./text-to-speech-client"`<br/> `"--input='What's the weather like'"` <br/> `"--host=$(TEXT_TO_SPEECH_HOST)"`<br/>`"--port=$(TEXT_TO_SPEECH_PORT)"` |