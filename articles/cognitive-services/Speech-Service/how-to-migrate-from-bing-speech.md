---
title: Migreren van Bing Speech naar de Speech-Service
titleSuffix: Azure Cognitive Services
description: Informatie over de verschillen tussen Bing Speech en de Spraakservice developer vanuit het oogpunt van en migratie van uw toepassing met de spraak-Service.
services: cognitive-services
author: wsturman
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: gracez
ms.openlocfilehash: baf9b9cd9b3f57c1d708dd404d59c036df6c169f
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466644"
---
# <a name="migrate-from-bing-speech-to-the-speech-service"></a>Migreren van Bing Speech naar de Speech-Service

Gebruik dit artikel voor het migreren van uw toepassingen uit de Bing Speech-API met de spraak-Service.

In dit artikel bevat een overzicht van de verschillen tussen de Bing Speech-API's en de Speech-Service en stelt strategieën voor het migreren van uw toepassingen. Uw abonnementssleutel Bing Speech-API wordt niet geaccepteerd door de Spraakservice; u moet een nieuw Speech Service-abonnement.

Een enkele abonnementssleutel voor de Speech-service verleent toegang tot de volgende functies. Elk wordt afzonderlijk gemeten, zodat u alleen betaalt voor de functies die u gebruikt.

* [Spraak naar tekst](speech-to-text.md)
* [Aangepaste spraak naar tekst ](https://cris.ai)
* [Tekst naar spraak](text-to-speech.md)
* [Aangepaste tekst naar spraak](how-to-customize-voice-font.md)
* [Spraakomzetting](speech-translation.md) (bevat geen [tekstomzetting](../translator/translator-info-overview.md))

De [spraak SDK](speech-sdk.md) is een functionele vervanging voor de Bing Speech-clientbibliotheken, maar een andere API gebruikt.

## <a name="comparison-of-features"></a>Vergelijking van functies

De spraak-Service is grotendeels op platform, functie en programming taalpariteit met Bing Speech met de volgende verschillen.

Functie | Bing Speech | Speech Service | Details
-|-|-|-
SDK VOOR C++ | : heavy_minus_sign: | : heavy_check_mark: | Speech Service biedt ondersteuning voor Windows en Linux
Java-SDK | : heavy_check_mark: | : heavy_check_mark: | Speech Service biedt ondersteuning voor Android- en spraakherkenning apparaten
C# SDK | : heavy_check_mark: | : heavy_check_mark: | Speech Service biedt ondersteuning voor Windows 10 UWP en .NET Standard 2.0
Continue spraakherkenning | 10 minuten | Onbeperkt (met de SDK) | Bing Speech zowel spraak Service WebSockets protocollen ondersteunen maximaal tien minuten per aanroep. De spraak-SDK automatisch opnieuw verbinding maken met een time-out of echter verbreken.
Tijdelijke of gedeeltelijke resultaten | : heavy_check_mark: | : heavy_check_mark: | Met WebSockets protocol of SDK
Aangepaste spraakmodellen | : heavy_check_mark: | : heavy_check_mark: | Bing Speech een afzonderlijke aangepaste spraak-abonnement is vereist
Aangepaste spraakstijlen | : heavy_check_mark: | : heavy_check_mark: | Bing Speech een afzonderlijke aangepaste spraak-abonnement is vereist
24-kHz stemmen | : heavy_minus_sign: | : heavy_check_mark: 
Spraakintentieherkenning | Afzonderlijke LUIS-API-aanroep is vereist | Geïntegreerd (met de SDK) |  Een LUIS-sleutel kan worden gebruikt met de Speech-Service.
Eenvoudige intentieherkenning | : heavy_minus_sign: | : heavy_check_mark: 
Batch transcriptie van lange audio-bestanden | : heavy_minus_sign: | : heavy_check_mark:
Herkennings-modus | Handmatig via URI van het eindpunt | Automatisch | Opname-modus is niet beschikbaar in de Speech-Service
Eindpunt plaats | Wereldwijd | Regionale | Regionale eindpunten verbeteren latentie. Er is een globaal eindpunt voor in aanmerking voor de Speech-Service.
REST-API’s | : heavy_check_mark: | : heavy_check_mark: | REST-API voor spraak-Service is compatibel met de Bing Speech (verschillende eindpunt). REST-API's ondersteunen voor tekst naar spraak en beperkte spraak-naar-tekst-functionaliteit.
WebSockets-protocollen | : heavy_check_mark: | : heavy_check_mark: | Spraak-API voor Service WebSockets is compatibel met de Bing Speech (verschillende eindpunt). Migreren naar de SDK spraak indien mogelijk moet uw code vereenvoudigen.
Service-naar-service API-aanroepen | : heavy_check_mark: | : heavy_minus_sign: | Opgegeven in de Bing Speech via de C# Service-bibliotheek. 
Open-source SDK | : heavy_check_mark: | : heavy_minus_sign: |

De spraak-Service maakt gebruik van een prijsmodel op basis van tijd (in plaats van een model op basis van een transactie). Zie [Prijzen van de Speech-service](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) voor meer informatie.

## <a name="migration-strategies"></a>Migratiestrategieën

Als u of uw organisatie hebt toepassingen in de ontwikkeling of productie die gebruikmaken van een Bing Speech-API, moet u ze voor het gebruik van de Speech-Service zo snel mogelijk bijwerken. Zie de [Spraakservice documentatie](index.yml) voor beschikbare SDK's, codevoorbeelden en zelfstudies.

De Spraakservice [REST-API's](rest-apis.md) compatibel zijn met de Bing Speech-API's. Als u momenteel van de Bing Speech REST-API's gebruikmaakt, moet u alleen het REST-eindpunt te wijzigen en schakel over naar een abonnementssleutel Speech-Service.

De spraak-Service WebSockets-protocollen zijn ook compatibel met die worden gebruikt door Bing Speech. U wordt aangeraden dat ontwikkeling van nieuwe als doel de spraak-SDK, in plaats van met behulp van WebSockets en we u raden aan het migreren van bestaande code naar de SDK ook. Als met de REST-API's en vereist bestaande code die gebruikmaakt van Bing Speech via WebSockets echter alleen een wijziging in het eindpunt en -sleutel voor een bijgewerkt.

Als u een Bing Speech client-bibliotheek voor een bepaalde programmeertaal, migreren naar de [spraak SDK](speech-sdk.md) wijzigingen in uw toepassing is vereist omdat de API. De spraak-SDK kunt u uw code eenvoudiger terwijl het ook zodat u toegang hebt tot nieuwe functies.

De spraak-SDK ondersteunt momenteel, C# (Windows 10, UWP of .NET Standard), Java (apparaten met Android- en aangepaste), Objective C (iOS), C++ (Windows en Linux), en JavaScript. API's op alle platforms zijn vergelijkbaar, vereenvoudigt de ontwikkeling van meerdere platforms.

De Speech-Service niet wordt momenteel een globaal eindpunt voor bieden. U moet bepalen als uw toepassing werkt efficiënt gebruik van één regionale eindpunt voor al het verkeer. Als dat niet het geval is, geolocatie gebruiken om te bepalen van het eindpunt van de meest efficiënte. U moet een afzonderlijk Speech Service-abonnement in elke regio die u gebruikt.

Als uw toepassing maakt gebruik van lange levensduur hebben verbindingen en een beschikbare SDK kan niet worden gebruikt, kunt u een verbinding WebsSockets gebruiken en beheren van de 10 minuten time-outlimiet door opnieuw verbinding maken op de juiste tijden.

Aan de slag met de spraak-SDK:

1. Download de [spraak SDK](speech-sdk.md).
1. Werk met de Spraakservice [snelstartgidsen](quickstart-csharp-dotnet-windows.md), [zelfstudies](how-to-recognize-intents-from-speech-csharp.md), en bekijk de [codevoorbeelden](samples.md) aan ervaring met de nieuwe API's.
1. Uw toepassing bijwerken voor gebruik van de service voor spraak en API's.

## <a name="support"></a>Ondersteuning

Bing Speech-klanten kunnen contact opnemen met klantondersteuning door het openen van een [ondersteuningsticket](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). U kunt ook contact met ons als uw behoeften ondersteuning vereist een [technisch Ondersteuningsabonnement](https://azure.microsoft.com/support/plans/).

Ga naar de Speech-Service voor spraak-Service, SDK en API-ondersteuning, [ondersteuningspagina](support.md).

## <a name="next-steps"></a>Volgende stappen

* [Speech Service gratis uitproberen](get-started.md)
* [Snelstartgids: Herkennen gesproken tekst in een UWP-app met behulp van de spraak-SDK](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Zie ook
* [Opmerkingen bij de release van de spraak-Service](releasenotes.md)
* [Wat is de Speech-service](overview.md)
* [Spraakservice en de SDK-documentatie](speech-sdk.md#get-the-sdk)
