---
title: Over de apparaten spraak SDK - spraakservices
titleSuffix: Azure Cognitive Services
description: Aan de slag met de SDK van de apparaten spraak. De Speech Services werken met een groot aantal apparaten en audio-bronnen. Nu kunt u uw spraaktoepassingen-naar een hoger niveau met overeenkomende hardware en software uitvoeren. De spraak-apparaten-SDK is een vooraf afgestemd bibliotheek die gekoppeld met speciaal ontwikkelde, microfoon SDK in de matrix.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 44e993375b17960ad5d9ced11d37ed3701a58f73
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53101735"
---
# <a name="about-the-speech-devices-sdk-preview"></a>Informatie over de apparaten van de spraakherkenning SDK (Preview)

De [Spraakservice](overview.md) werkt met een groot aantal apparaten en audio-bronnen. Nu kunt u uw spraaktoepassingen-naar een hoger niveau met overeenkomende hardware en software uitvoeren. De spraak-apparaten-SDK is een pretuned bibliotheek die gekoppeld met speciaal ontwikkelde, microfoon SDK in de matrix.

De SDK van de apparaten spraak kan u helpen:
* Snel test nieuwe stem-scenario's.
* Meer de Speech cloud-gebaseerde service voor het eenvoudig integreren in uw apparaat.
* Maak een uitstekende gebruikerservaring voor uw klanten.

De SDK van de apparaten spraak verbruikt de [spraak SDK](speech-sdk.md). De spraak-SDK wordt gebruikt voor het verzenden van de audio die wordt verwerkt door onze geavanceerde verwerking van audio-algoritme van de microfoon-matrix van het apparaat naar de [spraakservice](overview.md). Meerkanaalse audio wordt gebruikt voor nauwkeurigere ver-veld [spraakherkenning](speech-to-text.md) via ruis onderdrukking, echo annulering beamforming en dereverberation.

U kunt ook de spraak-apparaten-SDK gebruiken voor het bouwen van de apparaten waarop uw eigen [aangepast wake word](speech-devices-sdk-create-kws.md), zodat de hint die een tussenkomst van de gebruiker initieert uniek is voor uw merk is.

De SDK van de apparaten spraak vereenvoudigt het uitvoeren van een aantal verschillende spraakmogelijkheden's, zoals schijf-t/m bestellen systemen, in de winkel of Home assistenten en slimme sprekers. U kunt reageren op gebruikers met tekst, spreken terug naar deze in een standaard of [aangepaste gesproken](how-to-customize-voice-font.md), zoekresultaten, bieden [vertalen](speech-translation.md) in andere talen en meer. We kijken ernaar uit om te zien wat u bouwen!

## <a name="development-kit-providers"></a>Development kit-providers

Deze volledige, end-to-end system voorbeeldontwerpen zijn momenteel beschikbaar:

|||
|-|-|
|[![ROOBO-logo](media/speech-devices-sdk/roobo-logo.png)](http://ddk.roobo.com/)|ROOBO biedt volledige kunstmatige intelligentie (AI) system-oplossingen voor electric huishoudelijke, auto's, robots, toys en andere bedrijfstakken. Voorbeeldontwerpen van ROOBO minder aanzienlijk ontwikkeling time-to-market via integratie met de Microsoft Speech-service. [Ga naar ROOBO](http://ddk.roobo.com/).|

## <a name="next-steps"></a>Volgende stappen

Als u wilt beginnen, krijgen een [gratis Azure-account](https://azure.microsoft.com/free/ai/) en zich registreren voor de SDK van de apparaten spraak.

> [!div class="nextstepaction"]
> [Aanmelden voor de SDK van de apparaten spraak](get-speech-devices-sdk.md)
