---
title: Wat is Video Indexer?
titlesuffix: Azure Cognitive Services
description: Dit onderwerp geeft een overzicht van de service Video Indexer.
services: cognitive services
author: juliako
manager: cgronlun
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: overview
ms.date: 09/15/2018
ms.author: nolachar
ms.openlocfilehash: fd92e91989bd1a37626227b327d644c9d704ab6c
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983011"
---
# <a name="what-is-video-indexer"></a>Wat is Video Indexer?

Azure Video Indexer is een cloudtoepassing die is gebouwd met behulp van Azure Media Analytics, Azure Search, Cognitive Services (zoals de Face-API, Microsoft Translator, de Computer Vision-API en Custom Speech Service). Hiermee kunt u inzichten ophalen uit uw video's met Video Indexer-modellen die hieronder worden beschreven:
 
- **Automatische taaldetectie**: identificeert automatisch de meest gesproken taal. De volgende talen worden ondersteund: Engels, Spaans, Frans, Duits, Italiaans, Chinees (vereenvoudigd), Japans, Russisch en Portugees (Brazilië). De taal wordt standaard ingesteld op het Engels als de taal niet kan worden gedetecteerd.
- **Audiotranscriptie**: converteert spraak naar tekst in 10 talen en kan worden uitgebreid. De volgende talen worden ondersteund: Engels, Spaans, Frans, Duits, Italiaans, Chinees (vereenvoudigd), Japans, Arabisch, Russisch en Portugees (Brazilië).
- **Ondertiteling**: hiermee maakt u ondertiteling in drie indelingen: VTT, TTML, SRT.
- **Verwerking van twee kanalen**: detecteert automatisch afzonderlijke transcripten en voegt ze samen in één tijdlijn.
- **Ruisvermindering**: schoont telefoonaudio of luide opnamen op (gebaseerd op Skype-filters).
- **Transcriptaanpassing (CRIS)**: traint uitgebreide aangepaste spraak-naar-tekst-modellen zodat ze branchespecifieke transcripten maken en voert deze uit.
- **Sprekersopsomming**: detecteert en begrijpt welke spreker welke woorden heeft gezegd en wanneer.
- **Statistieken van de spreker**: biedt statistieken voor de spraakverhouding van sprekers.
- **Visuele tekstherkenning (OCR)**: extraheert de tekst die visueel wordt weergegeven in de video.
- **Extractie van sleutelframes**: detecteert stabiele sleutelframes in een video.
- **Gevoelsanalyse**: identificeert positieve, negatieve en neutrale gevoelens uit visuele tekst en gesproken woorden.
- **Visueel inhoudstoezicht**: detecteert inhoud voor volwassenen en/of ongepaste visuele elementen.
- **Trefwoorden extraheren**: trefwoorden worden geëxtraheerd uit de visuele tekst en spraak.
- **Identificatie van labels**: identificeert visuele objecten en acties die worden weergegeven.
- **Merken extraheren**: merken worden geëxtraheerd uit de visuele tekst en spraak.
- **Gezichtsdetectie**: detecteert en groepeert gezichten die worden weergegeven in de video.
- **Miniatuurextractie voor gezichten ('beste gezicht')**: identificeert automatisch het beste vastgelegde gezicht in elke groep van gezichten (gebaseerd op de kwaliteit, de grootte en positie) en extraheert deze als een afbeeldingsasset.
- **Beroemdheden identificeren**: herkent beroemdheden in de video op basis van een database van 1 miljoen beroemdheden. De bron is afkomstig van IMDB, Wikipedia en belangrijke LinkedIn-influencers.
- **Aangepaste gezichtsidentificatie**: herkent gezichten in de video op basis van een aangepast model dat is getraind voor het specifieke account.
- **Tekstueel inhoudsbeheer**: detecteert expliciete tekst in het audiotranscript.
- **Scènedetectie**: herkent wanneer een scène wijzigt in de video.
- **Detectie van zwarte frames**: identificeert zwarte frames in de video.
- **Audio-effecten**: identificeert audio-effecten, zoals applaus, spraak en stilte.
- **Onderwerpdeductie**: maakt een deductie van de belangrijkste onderwerpen uit de transcripten. De [IPTC](https://iptc.org/standards/media-topics/)-taxonomie van het eerste niveau is opgenomen.
- **Detectie van emoties**: identificeert emoties op basis van hints in de spraak en audio. De emotie kan vreugde, verdriet, boosheid of angst zijn.
- **Artefacten**: extraheert een grote verscheidenheid aan 'extra gedetailleerde' artefacten voor elk van de modellen.
- **Vertaling**: maakt vertalingen van het audiotranscript in 54 verschillende talen.

Wanneer Video Indexer klaar is met de verwerking en analyse, kunt u de video-inzichten bekijken, cureren, doorzoeken en publiceren.

De Video Indexer-service is geschikt voor een groot aantal toepassingen, ongeacht of u inhoudsbeheerder of ontwikkelaar bent. Inhoudsbeheerders kunnen de webportal van Video Indexer gebruiken om de voordelen van de service te benutten zonder één regel code te schrijven. Zie [deze zelfstudie over Video Indexer](video-indexer-get-started.md) om aan de slag te gaan met de website. Ontwikkelaars kunnen API's gebruiken om inhoud op schaal te verwerken. Zie [REST-API van Video Indexer gebruiken](video-indexer-use-apis.md) voor meer informatie. De service stelt klanten ook in staat om met behulp van widgets videostreams en geëxtraheerde inzichten te publiceren in hun eigen toepassingen. Zie [Video Indexer-widgets insluiten in uw toepassingen](video-indexer-embed-widgets.md) voor meer informatie.

U kunt zich registreren voor de service met behulp van een bestaand account van AAD, LinkedIn, Facebook, Google of MSA. Zie [Aan de slag](video-indexer-get-started.md) voor meer informatie.

## <a name="scenarios"></a>Scenario's

Hieronder vindt u enkele scenario's waarin Video Indexer van pas kan komen:

- Zoeken: inzichten die zijn geëxtraheerd uit de video kunnen worden gebruikt voor het verbeteren van de zoekervaring in een videobibliotheek. Zo is het bijvoorbeeld mogelijk om door het indexeren van gesproken woorden en gezichten momenten in een video te vinden waarop een bepaalde persoon bepaalde woorden heeft uitgesproken of waarop twee personen samen zijn te zien. Zoekopdrachten op basis van dergelijke inzichten uit video's zijn handig voor persbureaus, onderwijsinstellingen, omroepen, eigenaren van inhoud voor de entertainmentindustrie, LOB-apps voor ondernemingen, kortom voor elke bedrijfstak waarin videobibliotheken worden onderhouden die door gebruikers worden doorzocht.

- Inkomsten genereren: Video Indexer kan helpen om de waarde van video's te verbeteren. Branches die bijvoorbeeld afhankelijk zijn van advertentie-inkomsten (via nieuwsmedia, social media, enzovoort), kunnen meer relevante advertenties aanbieden door de geëxtraheerde inzichten te gebruiken als aanvullende signalen voor de advertentieserver (het is relevanter om een advertentie voor sportschoenen weer te geven tijdens een voetbalwedstrijd dan tijdens een zwemwedstrijd).

- Betrokkenheid van gebruikers: video-inzichten kunnen worden gebruikt om de betrokkenheid van gebruikers te verbeteren door het positioneren van de relevante videomomenten aan gebruikers. Laten we als voorbeeld een educatieve video nemen waarin de eerste 30 minuten gaan over bollen en de 30 minuten daarna over piramiden. Een leerling die informatie zoekt over piramiden, zal het dan prettig vinden als de video start vanaf de markering voor 30 minuten en niet vanaf het begin.

Raadpleeg dit [blog](http://aka.ms/videoindexerblog) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

U kunt aan de slag met Video Indexer. Raadpleeg voor meer informatie de volgende artikelen:

- [Aan de slag met de Video Indexer-website](video-indexer-get-started.md)
- [REST-API van Video Indexer gebruiken](video-indexer-use-apis.md)
- [Video Indexer-widgets insluiten in uw toepassingen](video-indexer-embed-widgets.md)
