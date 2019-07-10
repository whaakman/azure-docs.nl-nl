---
title: Over de apparaten spraak SDK - spraakservices
titleSuffix: Azure Cognitive Services
description: Aan de slag met de SDK van de apparaten spraak. De Speech Services werken met een groot aantal apparaten en audio-bronnen. Nu kunt u uw spraaktoepassingen-naar een hoger niveau met overeenkomende hardware en software uitvoeren. De spraak-apparaten-SDK is een vooraf afgestemd bibliotheek die gekoppeld met speciaal ontwikkelde, microfoon SDK in de matrix.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 341f35d04c7f3f591198acbbf05ac32afe84e95c
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718538"
---
# <a name="about-the-speech-devices-sdk"></a>Informatie over de apparaten van de spraak-SDK

De [spraakservices](overview.md) werken met een groot aantal apparaten en audio-bronnen. Nu kunt u uw spraaktoepassingen-naar een hoger niveau met overeenkomende hardware en software uitvoeren. De spraak-apparaten-SDK is een pretuned bibliotheek die gekoppeld met speciaal ontwikkelde, microfoon SDK in de matrix.

De SDK van de apparaten spraak kan u helpen:

* Snel test nieuwe stem-scenario's.
* Integreer de cloudgebaseerde spraakservices eenvoudiger in uw apparaat.
* Maak een uitstekende gebruikerservaring voor uw klanten.

De SDK van de apparaten spraak verbruikt de [spraak SDK](speech-sdk.md). De spraak-SDK wordt gebruikt voor het verzenden van de audio die wordt verwerkt door onze geavanceerde verwerking van audio-algoritme van de microfoon-matrix van het apparaat naar de [spraakservices](overview.md). Meerkanaalse audio wordt gebruikt voor nauwkeurigere ver-veld [spraakherkenning](speech-to-text.md) via ruis onderdrukking, echo annulering beamforming en dereverberation.

U kunt ook de spraak-apparaten-SDK gebruiken voor het bouwen van de apparaten waarop uw eigen [aangepast wake word](speech-devices-sdk-create-kws.md) , zodat de hint die een tussenkomst van de gebruiker initieert uniek is voor uw merk is.

De SDK van de apparaten spraak vereenvoudigt het uitvoeren van een verscheidenheid aan scenario's voor het stem-functionaliteit, zoals [aangepaste spraak op de eerste virtuele assistenten](https://aka.ms/bots/speech/va), schijf-t/m-systemen, bestellen [conversatie transcriptie](conversation-transcription-service.md), en slimme sprekers. U kunt reageren op gebruikers met tekst, spreken terug naar deze in een standaard of [aangepaste gesproken](how-to-customize-voice-font.md), zoekresultaten, bieden [vertalen](speech-translation.md) in andere talen en meer. We kijken ernaar uit om te zien wat u bouwen!

## <a name="get-the-speech-devices-sdk"></a>De Speech Devices SDK ophalen

### <a name="android"></a>Android

Voor Android-apparaten downloaden de nieuwste versie van de [Android spraak Devices SDK](https://aka.ms/sdsdk-download-android).

### <a name="windows"></a>Windows

Voor Windows wordt de voorbeeldtoepassing geleverd als een Java-toepassing voor meerdere platforms. Download de nieuwste versie van de [JRE spraak Devices SDK](https://aka.ms/sdsdk-download-JRE).
De toepassing is gemaakt met de spraak-SDK-pakket en de Eclipse Java-IDE (v4) op 64-bits Windows. De toepassing wordt uitgevoerd op een 64-bit Java 8 runtime-omgeving (JRE).

### <a name="linux"></a>Linux

Voor Linux wordt de voorbeeldtoepassing geleverd als een Java-toepassing voor meerdere platforms. Download de nieuwste versie van de [JRE spraak Devices SDK](https://aka.ms/sdsdk-download-JRE).
De toepassing is gemaakt met de spraak-SDK-pakket en de Eclipse Java-IDE (v4) op 64-bits Linux (Ubuntu 16.04, 18.04 Ubuntu, Debian 9). De toepassing wordt uitgevoerd op een 64-bit Java 8 runtime-omgeving (JRE).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Kies uw apparaat spraak](get-speech-devices-sdk.md)
>
> [!div class="nextstepaction"]
> [Ontvangt u een abonnementssleutel Speech Services gratis](get-started.md)
