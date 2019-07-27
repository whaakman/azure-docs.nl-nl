---
title: Over de speech-apparaten SDK-Speech-Service
titleSuffix: Azure Cognitive Services
description: Aan de slag met de SDK van de apparaten spraak. De spraak-Service werkt met een groot aantal apparaten en audio-bronnen. Nu kunt u uw spraaktoepassingen-naar een hoger niveau met overeenkomende hardware en software uitvoeren. De spraak-apparaten-SDK is een vooraf afgestemd bibliotheek die gekoppeld met speciaal ontwikkelde, microfoon SDK in de matrix.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: f2819dd2194193ffa8171034fdbe01ac4cb26174
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68558914"
---
# <a name="about-the-speech-devices-sdk"></a>Over de speech-apparaten SDK

De [spraak Services](overview.md) werken met een groot aantal verschillende apparaten en audio bronnen. Nu kunt u uw spraaktoepassingen-naar een hoger niveau met overeenkomende hardware en software uitvoeren. De spraak-apparaten-SDK is een pretuned bibliotheek die gekoppeld met speciaal ontwikkelde, microfoon SDK in de matrix.

De SDK van de apparaten spraak kan u helpen:

* Snel test nieuwe stem-scenario's.
* De spraak Services op basis van de cloud kunt u gemakkelijker integreren in uw apparaat.
* Maak een uitstekende gebruikerservaring voor uw klanten.

De SDK van de apparaten spraak verbruikt de [spraak SDK](speech-sdk.md). De spraak-SDK wordt gebruikt voor het verzenden van de audio die door ons geavanceerde algoritme voor geluids verwerking van de microfoon matrix van het apparaat naar de [spraak Services](overview.md)wordt verwerkt. Meerkanaalse audio wordt gebruikt voor nauwkeurigere ver-veld [spraakherkenning](speech-to-text.md) via ruis onderdrukking, echo annulering beamforming en dereverberation.

U kunt ook de speech apparaten SDK gebruiken om omgevings apparaten te bouwen met uw eigen [aangepaste Ontwaak woord](speech-devices-sdk-create-kws.md) , zodat de hint die een gebruikers interactie initieert, uniek is voor uw merk.

De SDK voor spraak apparaten vereenvoudigt het maken van een groot aantal scenario's met spraak mogelijkheden, zoals [aangepaste spraak-eerste virtuele assistenten](https://aka.ms/bots/speech/va), drive-passthrough best Ellen, [conversatie transcriptie](conversation-transcription-service.md)en slimme sprekers. U kunt reageren op gebruikers met tekst, spreken terug naar deze in een standaard of [aangepaste gesproken](how-to-customize-voice-font.md), zoekresultaten, bieden [vertalen](speech-translation.md) in andere talen en meer. We kijken ernaar uit om te zien wat u bouwen!

## <a name="get-the-speech-devices-sdk"></a>De Speech Devices SDK ophalen

### <a name="android"></a>Android

Down load voor Android-apparaten de nieuwste versie van de [Android speech-apparaten SDK](https://aka.ms/sdsdk-download-android).

### <a name="windows"></a>Windows

Voor Windows wordt de voorbeeld toepassing meegeleverd als een cross-platform Java-toepassing. Down load de nieuwste versie van de software voor [jre speech-apparaten](https://aka.ms/sdsdk-download-JRE).
De toepassing is gebouwd met het Speech SDK-pakket en de eclips Java IDE (v4) op 64-bits Windows. De toepassing wordt uitgevoerd op een 64-bit Java 8 runtime-omgeving (JRE).

### <a name="linux"></a>Linux

Voor Linux wordt de voorbeeld toepassing meegeleverd als een cross-platform Java-toepassing. Down load de nieuwste versie van de software voor [jre speech-apparaten](https://aka.ms/sdsdk-download-JRE).
De toepassing is gebouwd met het Speech SDK-pakket en de eclips Java IDE (v4) op 64-bits Linux (Ubuntu 16,04, Ubuntu 18,04, Debian 9). De toepassing wordt uitgevoerd op een 64-bit Java 8 runtime-omgeving (JRE).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Uw spraak apparaat kiezen](get-speech-devices-sdk.md)
>
> [!div class="nextstepaction"]
> [Gratis een abonnements sleutel voor spraak Services aanschaffen](get-started.md)
