---
title: De Speech Devices SDK ophalen
titleSuffix: Azure Cognitive Services
description: De spraak-Service werkt met een groot aantal apparaten en audio-bronnen. Nu kunt u uw spraaktoepassingen-naar een hoger niveau met overeenkomende hardware en software uitvoeren. In dit artikel leert u hoe u toegang krijgt tot de speech-apparaten SDK en hoe u kunt ontwikkelen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: b60c8735833c3012523c5cc0d7eea6a50e9d10b6
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559688"
---
# <a name="get-the-cognitive-services-speech-devices-sdk"></a>De Cognitive Services spraak apparaten SDK ophalen

De SDK voor spraak apparaten is een vooraf geclusterde bibliotheek die is ontworpen om te werken met doel ontwikkelde ontwikkel kits en verschillende matrix configuraties van microfoons.

## <a name="choose-a-development-kit"></a>Een Development Kit kiezen

|Apparaten|Specificatie|Description|Scenario's|
|--|--|--|--|
|[Roobo slimme audio dev kit](https://ddk.roobo.com)</br>[](speech-devices-sdk-roobo-v1.md) / Quick start voor het instellen van[Snelstartgids](speech-devices-sdk-android-quickstart.md)![roobo Smart audio dev kit](media/speech-devices-sdk/device-roobo-v1.jpg)|7-Mic-matrix, ARM SOC, WIFI, audio out, IO. </br>[Android](speech-devices-sdk-android-quickstart.md)|De eerste speech-apparaten SDK voor het aanpassen van micro soft Mic-matrix en front processing SDK, voor het ontwikkelen van transcriptie-en spraak scenario's van hoge kwaliteit|Conversation transcriptie, Smart spreker, Voice agent, wearable|
|[Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)</br>[](https://docs.microsoft.com/azure/Kinect-dk/set-up-azure-kinect-dk) / [](speech-devices-sdk-windows-quickstart.md)QuickStart![voor Azure Kinect DK instellen](media/speech-devices-sdk/device-azure-kinect-dk.jpg)|7-microfoon matrix RGB en diepte camera's. </br>[Windows](speech-devices-sdk-windows-quickstart.md)/[Linux](speech-devices-sdk-linux-quickstart.md)|Een Developer Kit met geavanceerde kunst matige intelligentie (AI) Sens oren voor het bouwen van geavanceerde computer vision-en spraak modellen. Het combineert een ' best-in-class ' ruimtelijke microfoon matrix en diepte camera met een video camera en afdruk stand-sensor, allemaal op één klein apparaat met meerdere modi, opties en Sdk's om een bereik van reken typen te bieden.|Conversation transcriptie, Robotics, slim bouwen|
|Roobo slimme audio dev kit 2![Roobo slimme audio dev kit 2](media/speech-devices-sdk/device-roobo-v2.jpg)|7-Mic-matrix, ARM SOC, WIFI, Bluetooth, IO. </br>Linux|De tweede generatie speech-apparaten SDK die alternatieve besturings systemen en meer functies biedt in een kosteneffectd referentie ontwerp.|Conversation transcriptie, Smart spreker, Voice agent, wearable|
|URbetter T11 Development Board![URbetter DDK](media/speech-devices-sdk/device-urbetter.jpg)|7-microfoon matrix, ARM SOC, WIFI, Ethernet, HDMI, USB-camera. </br>Linux|De SDK voor spraak apparaten op branche niveau die micro soft Mic-matrix aanpast en ondersteuning biedt voor uitgebreide I/O, zoals HDMI/Ethernet en meer USB-rand apparatuur|Transcriptie, onderwijs, zieken huis, robots, OTT box, Voice agent, drive t/m|

## <a name="download-the-speech-devices-sdk"></a>De spraak-apparaten SDK downloaden

Down load de [SDK voor spraak apparaten](https://aka.ms/sdsdk-download).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aan de slag met de SDK van de apparaten spraak](https://aka.ms/sdsdk-quickstart)
