---
title: Azure Video Indexer | Microsoft Docs
description: Dit onderwerp geeft een overzicht van de service Video Indexer.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: overview
ms.date: 07/25/2018
ms.author: nolachar
ms.openlocfilehash: f52c4af29d0c7de8b5edbe869640ffc5dddb5c5e
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397888"
---
# <a name="what-is-video-indexer-preview"></a>Wat is Video Indexer? (preview)

Video Indexer is een cloudtoepassing die is gebouwd met behulp van Azure Media Analytics, Cognitive Services (zoals de Face-API, Microsoft Translator, de Computer Vision-API en Custom Speech Service) en Azure Search. Gebruik de toepassing om met behulp van technologieën voor kunstmatige intelligentie de volgende inzichten op te halen uit video's:

- **Automatische taaldetectie**: de taal van een video kan automatisch worden vastgesteld door Video Indexer. Automatische taaldetectie ondersteunt momenteel Engels, Spaans, Frans, Duits, Italiaans, Chinees (vereenvoudigd), Japans en Russisch. De taal wordt standaard ingesteld op het Engels als de taal niet kan worden gedetecteerd.
- **Audiotranscriptie**: Video Indexer ondersteunt spraak-naar-tekst, waardoor klanten een transcriptie kunnen krijgen van de gesproken tekst. Ondersteunde talen zijn onder andere Engels, Spaans, Frans, Duits, Italiaans, Chinees (vereenvoudigd), Portugees (Brazilië), Japans en Russisch (met in de toekomst nog veel meer talen). 
- **Gezichtstracering en -detectie**: technologieën voor gezichtsdetectie maken het mogelijk om gezichten te detecteren in een video. De gedetecteerde gezichten worden vergeleken met een database met beroemdheden om vast te stellen of er beroemdheden aanwezig zijn in de video. Klanten kunnen ook gezichten labelen die niet worden herkend als een beroemdheid. Video Indexer bouwt op basis van die labels een gezichtsmodel op en kan die gezichten dan herkennen in video's die in de toekomst worden ingediend.
- **Indexering van sprekers**: Video Indexer biedt de mogelijkheid om aan te geven en te begrijpen welke spreker welke tekst heeft uitgesproken en wanneer.
- **Visuele tekstherkenning**: met deze technologie kan Video Indexer tekst ophalen die wordt weergegeven in de video's.  
- **Detectie van stemactiviteit**: deze functie stelt Video Indexer in staat om achtergrondgeluid te onderscheiden van stemactiviteit. 
- **Scènedetectie**: Video Indexer biedt de mogelijkheid om een visuele analyse uit te voeren van de video om te bepalen wanneer een scène verandert in een video.
- **Extractie van sleutelframes**: Video Indexer detecteert automatisch sleutelframes in een video. 
- **Sentimentanalyse**: Video Indexer voert sentimentanalyse uit op de tekst die is geëxtraheerd met behulp van spraak-naar-tekst en optische tekenherkenning en biedt die informatie aan in de vorm van positieve, negatief of neutrale sentimenten, samen met tijdcodes.
- **Vertaling**: Video Indexer biedt de mogelijkheid om audiotranscripties te vertalen van de ene taal naar de andere taal. De volgende talen worden ondersteund: Engels, Spaans, Frans, Duits, Italiaans, Chinees (vereenvoudigd), Portugees (Brazilië), Japans en Russisch. Zodra de vertaling is voltooid, kan de gebruiker in de videospeler ondertiteling in andere talen weergeven.
- **Detectie van ongepaste inhoud**: deze technologie maakt het mogelijk om de aanwezigheid van expliciet en/of ongepast materiaal in video's te detecteren en kan worden gebruikt voor het filteren van inhoud. 
- **Extractie van trefwoorden**: Video Indexer extraheert trefwoorden op basis van de transcriptie van de gesproken woorden en de tekst die is herkend door visuele tekstherkenning.
- **Labels**: Video Indexer biedt labels voor visuele objecten zoals een kat, hond, tafel of auto, evenals acties zoals staan, rennen of vliegen.
- **Merken**: Video Indexer extraheert merken op basis van de transcriptie van de gesproken woorden en de tekst die is herkend door visuele tekstherkenning.

Wanneer Video Indexer klaar is met de verwerking en analyse, kunt u de video-inzichten bekijken, cureren, doorzoeken en publiceren.

De Video Indexer-service is geschikt voor een groot aantal toepassingen, ongeacht of u inhoudsbeheerder of ontwikkelaar bent. Inhoudsbeheerders kunnen de webportal van Video Indexer gebruiken om de voordelen van de service te benutten zonder één regel code te schrijven. Zie [deze zelfstudie over Video Indexer om snel aan de slag te gaan met de portal](video-indexer-get-started.md). Ontwikkelaars kunnen API's gebruiken om inhoud op schaal te verwerken. Zie [REST-API van Video Indexer gebruiken](video-indexer-use-apis.md) voor meer informatie. De service stelt klanten ook in staat om met behulp van widgets videostreams en geëxtraheerde inzichten te publiceren in hun eigen toepassingen. Zie [Video Indexer-widgets insluiten in uw toepassingen](video-indexer-embed-widgets.md) voor meer informatie.

U kunt zich registreren voor de service met behulp van een bestaand account van AAD, LinkedIn, Facebook, Google of MSA. Zie [Aan de slag](video-indexer-get-started.md) voor meer informatie.

## <a name="scenarios"></a>Scenario's

Hieronder vindt u enkele scenario's waarin Video Indexer van pas kan komen:

- Zoeken: inzichten die zijn geëxtraheerd uit de video kunnen worden gebruikt voor het verbeteren van de zoekervaring in een videobibliotheek. Zo is het bijvoorbeeld mogelijk om door het indexeren van gesproken woorden en gezichten momenten in een video te vinden waarop een bepaalde persoon bepaalde woorden heeft uitgesproken of waarop twee personen samen zijn te zien. Zoekopdrachten op basis van dergelijke inzichten uit video's zijn handig voor persbureaus, onderwijsinstellingen, omroepen, eigenaren van inhoud voor de entertainmentindustrie, LOB-apps voor ondernemingen, kortom voor elke bedrijfstak waarin videobibliotheken worden onderhouden die door gebruikers worden doorzocht.

- Inkomsten genereren: Video Indexer kan helpen om de waarde van video's te verbeteren. Branches die bijvoorbeeld afhankelijk zijn van advertentie-inkomsten (via nieuwsmedia, social media, enzovoort), kunnen meer relevante advertenties aanbieden door de geëxtraheerde inzichten te gebruiken als aanvullende signalen voor de advertentieserver (het is relevanter om een advertentie voor sportschoenen weer te geven tijdens een voetbalwedstrijd dan tijdens een zwemwedstrijd).

- Betrokkenheid van gebruikers: video-inzichten kunnen worden gebruikt om de betrokkenheid van gebruikers te verbeteren door het positioneren van de relevante videomomenten aan gebruikers. Laten we als voorbeeld een educatieve video nemen waarin de eerste 30 minuten gaan over bollen en de 30 minuten daarna over piramiden. Een leerling die informatie zoekt over piramiden, zal het dan prettig vinden als de video start vanaf de markering voor 30 minuten en niet vanaf het begin.

Raadpleeg dit [blog](http://aka.ms/videoindexerblog) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

U kunt aan de slag met Video Indexer. Raadpleeg voor meer informatie de volgende artikelen:

- [Aan de slag met de Video Indexer-portal](video-indexer-get-started.md)
- [REST-API van Video Indexer gebruiken](video-indexer-use-apis.md)
- [Video Indexer-widgets insluiten in uw toepassingen](video-indexer-embed-widgets.md)
