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
ms.date: 03/13/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: cb0e079eee78f865d472b55b162fb65b5eadaf44
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57900485"
---
# <a name="about-the-speech-devices-sdk-preview"></a>Informatie over de apparaten van de spraakherkenning SDK (Preview)

De [spraakservices](overview.md) werken met een groot aantal apparaten en audio-bronnen. Nu kunt u uw spraaktoepassingen-naar een hoger niveau met overeenkomende hardware en software uitvoeren. De spraak-apparaten-SDK is een pretuned bibliotheek die gekoppeld met speciaal ontwikkelde, microfoon SDK in de matrix.

De SDK van de apparaten spraak kan u helpen:
* Snel test nieuwe stem-scenario's.
* Integreer de cloudgebaseerde spraakservices eenvoudiger in uw apparaat.
* Maak een uitstekende gebruikerservaring voor uw klanten.

De SDK van de apparaten spraak verbruikt de [spraak SDK](speech-sdk.md). De spraak-SDK wordt gebruikt voor het verzenden van de audio die wordt verwerkt door onze geavanceerde verwerking van audio-algoritme van de microfoon-matrix van het apparaat naar de [spraakservices](overview.md). Meerkanaalse audio wordt gebruikt voor nauwkeurigere ver-veld [spraakherkenning](speech-to-text.md) via ruis onderdrukking, echo annulering beamforming en dereverberation.

U kunt ook de spraak-apparaten-SDK gebruiken voor het bouwen van de apparaten waarop uw eigen [aangepast wake word](speech-devices-sdk-create-kws.md), zodat de hint die een tussenkomst van de gebruiker initieert uniek is voor uw merk is.

De SDK van de apparaten spraak vereenvoudigt het uitvoeren van een aantal verschillende spraakmogelijkheden's, zoals schijf-t/m bestellen systemen, in de winkel of Home assistenten en slimme sprekers. U kunt reageren op gebruikers met tekst, spreken terug naar deze in een standaard of [aangepaste gesproken](how-to-customize-voice-font.md), zoekresultaten, bieden [vertalen](speech-translation.md) in andere talen en meer. We kijken ernaar uit om te zien wat u bouwen!

## <a name="development-kit-providers"></a>Development kit-providers

Deze volledige, end-to-end system voorbeeldontwerpen zijn momenteel beschikbaar:

|||
|-|-|
|[![ROOBO-logo](media/speech-devices-sdk/roobo-logo.png)](http://ddk.roobo.com/)|ROOBO biedt volledige kunstmatige intelligentie (AI) system-oplossingen voor electric huishoudelijke, auto's, robots, toys en andere bedrijfstakken. Voorbeeldontwerpen van ROOBO minder aanzienlijk ontwikkeling time-to-market Dankzij integratie met de Azure-Services voor spraak. [Ga naar ROOBO](http://ddk.roobo.com/).|

## <a name="next-steps"></a>Volgende stappen

Als u wilt beginnen, krijgen een [gratis Azure-account](https://azure.microsoft.com/free/ai/) en zich registreren voor de SDK van de apparaten spraak.

> [!div class="nextstepaction"]
> [Aanmelden voor de SDK van de apparaten spraak](get-speech-devices-sdk.md)
