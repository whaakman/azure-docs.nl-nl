---
title: Migreren van Bing Speech naar de Speech-Service
titleSuffix: Azure Cognitive Services
description: Informatie over de verschillen tussen de Bing Speech en de Spraakservice developer vanuit het oogpunt van en migratie van uw toepassing met de spraak-Service.
services: cognitive-services
author: wsturman
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: gracez
ms.openlocfilehash: fdd22e14e0b7636dbc337a20dd69bf93696bb924
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416277"
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

De spraak-Service is grotendeels vergelijkbaar met de Bing Speech, met de volgende verschillen.

Functie | Bing Speech | Speech Service | Details
-|-|-|-
SDK VOOR C++ | : heavy_minus_sign: | :heavy_check_mark: | Speech Service biedt ondersteuning voor Windows en Linux.
Java-SDK | :heavy_check_mark: | :heavy_check_mark: | Speech Service biedt ondersteuning voor Android- en spraakherkenning apparaten.
C# SDK | :heavy_check_mark: | :heavy_check_mark: | Speech Service biedt ondersteuning voor Windows 10, Universal Windows Platform (UWP) en .NET Standard 2.0.
Continue spraakherkenning | 10 minuten | Onbeperkt (met de SDK) | Bing Speech zowel spraak Service WebSockets protocollen ondersteunen maximaal tien minuten per aanroep. De spraak-SDK automatisch opnieuw verbinding maken met een time-out of echter verbreken.
Tijdelijke of gedeeltelijke resultaten | :heavy_check_mark: | :heavy_check_mark: | Met WebSockets protocol of SDK.
Aangepaste spraakmodellen | :heavy_check_mark: | :heavy_check_mark: | Bing Speech vereist een afzonderlijke aangepaste spraak-abonnement.
Aangepaste spraakstijlen | :heavy_check_mark: | :heavy_check_mark: | Bing Speech vereist een afzonderlijke aangepaste spraak-abonnement.
24-kHz stemmen | : heavy_minus_sign: | :heavy_check_mark: 
Spraakintentieherkenning | Afzonderlijke LUIS-API-aanroep is vereist | Geïntegreerd (met de SDK) |  U kunt een LUIS-sleutel met de Speech-Service.
Eenvoudige intentieherkenning | : heavy_minus_sign: | :heavy_check_mark: 
Batch transcriptie van lange audio-bestanden | : heavy_minus_sign: | :heavy_check_mark:
Herkennings-modus | Handmatig via URI van het eindpunt | Automatisch | Opname-modus is niet beschikbaar in de Speech-Service.
Eindpunt plaats | Wereldwijd | Regionale | Regionale eindpunten verbeteren latentie.
REST-API’s | :heavy_check_mark: | :heavy_check_mark: | REST-API voor spraak-Service is compatibel met de Bing Speech (verschillende eindpunt). REST-API's ondersteunen voor tekst naar spraak en beperkte spraak-naar-tekst-functionaliteit.
WebSockets-protocollen | :heavy_check_mark: | :heavy_check_mark: | Spraak-API voor Service WebSockets is compatibel met de Bing Speech (verschillende eindpunt). Migreren naar de SDK-spraak, indien mogelijk, ter vereenvoudiging van uw code.
Service-naar-service API-aanroepen | :heavy_check_mark: | : heavy_minus_sign: | Opgegeven in de Bing Speech via de C# Service-bibliotheek. 
Open-source SDK | :heavy_check_mark: | : heavy_minus_sign: |

De spraak-Service maakt gebruik van een prijsmodel op basis van tijd (in plaats van een model op basis van een transactie). Zie [Prijzen van de Speech-service](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) voor meer informatie.

## <a name="migration-strategies"></a>Migratiestrategieën

Als u of uw organisatie hebt toepassingen in de ontwikkeling of productie die gebruikmaken van een Bing Speech-API, moet u ze voor het gebruik van de Speech-Service zo snel mogelijk bijwerken. Zie de [Spraakservice documentatie](index.yml) voor beschikbare SDK's, codevoorbeelden en zelfstudies.

De Spraakservice [REST-API's](rest-apis.md) compatibel zijn met de Bing Speech-API's. Als u momenteel van de Bing Speech REST-API's gebruikmaakt, moet u alleen het REST-eindpunt te wijzigen, en schakel over naar een abonnementssleutel Speech-Service.

De spraak-Service WebSockets-protocollen zijn ook compatibel met die worden gebruikt door Bing Speech. Raden wij aan dat voor de ontwikkeling van nieuwe, u de SDK van spraak in plaats van WebSockets. Er is een goed idee om te migreren van bestaande code naar de SDK ook. Als met de REST-API's en vereist bestaande code die gebruikmaakt van Bing Speech via WebSockets echter alleen een wijziging in het eindpunt en -sleutel voor een bijgewerkt.

Als u een Bing Speech client-bibliotheek voor een bepaalde programmeertaal, migreren naar de [spraak SDK](speech-sdk.md) vereist wijzigingen in uw toepassing, omdat de API. De spraak-SDK kunt u uw code eenvoudiger, terwijl ook zodat u toegang hebt tot nieuwe functies.

De spraak-SDK ondersteunt momenteel, C# (Windows 10, UWP of .NET Standard), Java (apparaten met Android- en aangepaste), Objective C (iOS), C++ (Windows en Linux), en JavaScript. API's op alle platforms zijn vergelijkbaar, vereenvoudigt de ontwikkeling van meerdere platforms.

De Speech-Service niet wordt momenteel een globaal eindpunt voor bieden. Als uw toepassing efficiënt functioneert als één regionale eindpunt wordt gebruikt voor al het verkeer bepalen Als dat niet het geval is, geolocatie gebruiken om te bepalen van het eindpunt van de meest efficiënte. U moet een afzonderlijke Speech Service-abonnement in elke regio die u gebruikt.

Als uw toepassing maakt gebruik van lange levensduur hebben verbindingen en een beschikbare SDK kan niet worden gebruikt, kunt u een verbinding WebSockets. De 10 minuten time-outlimiet beheren door op de juiste tijden verbinding.

Aan de slag met de spraak-SDK:

1. Download de [spraak SDK](speech-sdk.md).
1. Werk met de Spraakservice [snelstartgidsen](quickstart-csharp-dotnet-windows.md) en [zelfstudies](how-to-recognize-intents-from-speech-csharp.md). Bekijk ook de [codevoorbeelden](samples.md) aan ervaring met de nieuwe API's.
1. Uw toepassing bijwerken voor gebruik van de Service voor spraak en API's.

## <a name="support"></a>Ondersteuning

Bing Speech-klanten kunnen contact opnemen met klantondersteuning door het openen van een [ondersteuningsticket](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). U kunt ook contact met ons als uw behoeften ondersteuning vereist een [plan voor technische ondersteuning](https://azure.microsoft.com/support/plans/).

Ga naar de Speech-Service voor spraak-Service, SDK en API-ondersteuning, [ondersteuningspagina](support.md).

## <a name="next-steps"></a>Volgende stappen

* [Speech Service gratis uitproberen](get-started.md)
* [Snelstartgids: Herkennen gesproken tekst in een UWP-app met behulp van de spraak-SDK](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Zie ook
* [Opmerkingen bij de release van de spraak-Service](releasenotes.md)
* [Wat is de Speech-Service](overview.md)
* [Spraakservice en de SDK-documentatie](speech-sdk.md#get-the-sdk)
