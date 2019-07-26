---
title: Wat is Video Indexer?
titlesuffix: Azure Media Services
description: Dit onderwerp geeft een overzicht van de service Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 07/22/2019
ms.author: juliako
ms.openlocfilehash: b0d0df5d113b1d75602022085b8bb17133f07333
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68422980"
---
# <a name="what-is-video-indexer"></a>Wat is Video Indexer?

Azure Video Indexer is een cloudtoepassing die is gebouwd met behulp van Azure Media Analytics, Azure Search, Cognitive Services (zoals de Face-API, Microsoft Translator, de Computer Vision-API en Custom Speech Service). U kunt de inzichten uit uw Video's extra heren met Video Indexer video-en audio modellen die hieronder worden beschreven:
  
## <a name="video-insights"></a>Video inzichten

- **Gezichts detectie**: Detecteert en groepeert gezichten die in de video worden weer gegeven.
- **Beroemdheden-id**: Video Indexer identificeert automatisch meer dan 1.000.000 beroemdheden, zoals wereld leiders, actoren, Actresses, atleten, onderzoekers, zakelijke en technologische leiders over de hele wereld. De gegevens van deze beroemdheden zijn ook beschikbaar op verschillende beroemde websites, zoals IMDB en Wikipedia.
- **Op account gebaseerde gezichts identificatie**: Video Indexer treinen een model voor een specifiek account. Vervolgens worden de gezichten herkend in de video op basis van het getrainde model. Zie [een persoons model aanpassen van de video indexer website](customize-person-model-with-website.md) en [een persoonlijk model aanpassen met de video indexer-API](customize-person-model-with-api.md)voor meer informatie.
- **Miniatuur extractie voor gezichten** ("beste gezicht"): Identificeert automatisch het beste vastgelegde gezicht in elke groep gezichten (op basis van kwaliteit, grootte en front-positie) en extraheer dit als een afbeeldings Asset.
- **Visuele tekst herkenning** (OCR): Extraheert tekst die visueel wordt weer gegeven in de video.
- **Visuele toezicht op inhoud**: Detecteert de visuele elementen volwassene en/of ongepaste.
- **Identificatie van labels**: Hiermee worden visuele objecten en weer gegeven acties geïdentificeerd.
- **Scène segmentering**: Hiermee wordt bepaald wanneer een scène in video wordt gewijzigd op basis van visuele hints. In een scène wordt één gebeurtenis weer gegeven. deze is samengesteld op basis van een reeks opeenvolgende opnamen, die semantisch verwant zijn. 
- **Opname detectie**: bepaalt wanneer een foto in video wordt gewijzigd op basis van visuele hints. Een foto is een reeks frames uit dezelfde camera voor animatie. Zie [scènes, afbeeldingen en keyframes](scenes-shots-keyframes.md)voor meer informatie.
- **Black frame-detectie**: Identificeert zwarte frames die in de video worden weer gegeven.
- **Extractie van keyframe**: Detecteert stabiele keyframes in een video.
- **Rollend tegoed**: Identificeer het begin en het einde van de roulerende tegoeden aan het einde van TV-Program ma's en films.

## <a name="audio-insights"></a>Audio inzichten

- **Automatische taal detectie**: Identificeert automatisch de taal voor de dominante spraak. Ondersteunde talen zijn Engels, Spaans, Frans, Duits, Italiaans, Chinees (vereenvoudigd), Japans, Russisch en Portugees (Brazilië). Als de taal niet met vertrouwen kan worden geïdentificeerd Video Indexer, wordt ervan uitgegaan dat de gesp roken taal Engels is. Zie [Language Identification model (Engelstalig)](language-identification-model.md)voor meer informatie.
- **Audio-transcriptie**: Hiermee converteert u spraak naar tekst in 12 talen en kunt u uitbrei dingen toestaan. Ondersteunde talen zijn Engels, Spaans, Frans, Duits, Italiaans, Chinees (vereenvoudigd), Japans, Arabisch, Russisch, Braziliaans Portugees, Hindi en Koreaans.
- Ondertiteling: Hiermee maakt u ondertiteling in drie indelingen: VTT, TTML, SRT.
- **Verwerking van twee kanalen**: Automatische detectie, afzonderlijke transcripten en samen voegingen op één tijd lijn.
- **Ruis reductie**: Hiermee wist u audio-of ruis opnamen van telefonie (op basis van Skype-filters).
- **Transcript aanpassing** (CRI'S): Traint aangepaste spraak naar tekst modellen om branchespecifieke transcripten te maken. Zie [een taal model aanpassen van de video indexer website](customize-language-model-with-website.md) en [een taal model aanpassen met de video indexer-api's](customize-language-model-with-api.md)voor meer informatie.
- **Opsomming**van de spreker: Hiermee wordt de spreker en het tijdstip waarop wordt gezocht, genoteerd en begrepen.
- **Sprekers statistieken**: Biedt statistieken voor spraak verhoudingen van luid sprekers.
- **Toezicht op tekstuele inhoud**: Detecteert expliciete tekst in de transcripten audio.
- **Audio-effecten**: Identificeert audio-effecten, zoals hand claps, spraak en stilte.
- **Detectie van Emotion**: Identificeert emoties op basis van de spraak (wat er wordt genoemd) en de spreek kleur (hoe deze wordt genoemd).  De emotie kan vreugde, verdriet, boosheid of angst zijn.
- **Vertaling**: Maakt vertalingen van de Audio-Transcript op 54 verschillende talen.

## <a name="audio-and-video-insights-multi-channels"></a>Audio-en video inzichten (multi kanalen)

Wanneer het indexeren per kanaal gedeeltelijk resultaat voor deze modellen beschikbaar is

- **Extractie van tref woorden**: Extraheert tref woorden uit spraak en visuele tekst.
- **Brands extractie**: Hiermee worden Brands geëxtraheerd uit spraak-en visuele tekst.
- **Onderwerp**voor demijnren: Hiermee maakt u de belangrijkste onderwerpen van transcripten onduidelijker. De IPTC-taxonomie op 1e niveau is opgenomen.
- **Artefacten**: Extraheert een uitgebreide set met artefacten van "volgende niveau Details" voor elk model.
- **Sentiment analyse**: Geeft positieve, negatieve en neutrale gevoel aan vanuit spraak-en visuele tekst.
 
Wanneer Video Indexer klaar is met de verwerking en analyse, kunt u de video-inzichten bekijken, cureren, doorzoeken en publiceren.

De Video Indexer-service is geschikt voor een groot aantal toepassingen, ongeacht of u inhoudsbeheerder of ontwikkelaar bent. Inhoudsbeheerders kunnen de webportal van Video Indexer gebruiken om de voordelen van de service te benutten zonder één regel code te schrijven. Zie [deze zelfstudie over Video Indexer](video-indexer-get-started.md) om aan de slag te gaan met de website. Ontwikkelaars kunnen API's gebruiken om inhoud op schaal te verwerken. Zie [REST-API van Video Indexer gebruiken](video-indexer-use-apis.md) voor meer informatie. De service stelt klanten ook in staat om met behulp van widgets videostreams en geëxtraheerde inzichten te publiceren in hun eigen toepassingen. Zie [Video Indexer-widgets insluiten in uw toepassingen](video-indexer-embed-widgets.md) voor meer informatie.

U kunt zich registreren voor de service met behulp van een bestaand account van AAD, LinkedIn, Facebook, Google of MSA. Zie [Aan de slag](video-indexer-get-started.md) voor meer informatie.

## <a name="scenarios"></a>Scenario's

Hieronder vindt u enkele scenario's waarin Video Indexer van pas kan komen:

- Zoeken: inzichten die zijn geëxtraheerd uit de video kunnen worden gebruikt voor het verbeteren van de zoekervaring in een videobibliotheek. Zo is het bijvoorbeeld mogelijk om door het indexeren van gesproken woorden en gezichten momenten in een video te vinden waarop een bepaalde persoon bepaalde woorden heeft uitgesproken of waarop twee personen samen zijn te zien. Zoekopdrachten op basis van dergelijke inzichten uit video's zijn handig voor persbureaus, onderwijsinstellingen, omroepen, eigenaren van inhoud voor de entertainmentindustrie, LOB-apps voor ondernemingen, kortom voor elke bedrijfstak waarin videobibliotheken worden onderhouden die door gebruikers worden doorzocht.
- Maken van inhoud: Insights geëxtraheerd uit Video's en helpt effectief inhoud te maken, zoals aanhang wagens, sociale media-inhoud, Nieuws clips, enzovoort van bestaande inhoud in het archief van de organisatie 
- Inkomsten genereren: Video Indexer kan helpen om de waarde van video's te verbeteren. Branches die bijvoorbeeld afhankelijk zijn van advertentie-inkomsten (via nieuwsmedia, social media, enzovoort), kunnen meer relevante advertenties aanbieden door de geëxtraheerde inzichten te gebruiken als aanvullende signalen voor de advertentieserver (het is relevanter om een advertentie voor sportschoenen weer te geven tijdens een voetbalwedstrijd dan tijdens een zwemwedstrijd).
- Betrokkenheid van gebruikers: video-inzichten kunnen worden gebruikt om de betrokkenheid van gebruikers te verbeteren door het positioneren van de relevante videomomenten aan gebruikers. Laten we als voorbeeld een educatieve video nemen waarin de eerste 30 minuten gaan over bollen en de 30 minuten daarna over piramiden. Een leerling die informatie zoekt over piramiden, zal het dan prettig vinden als de video start vanaf de markering voor 30 minuten en niet vanaf het begin.

## <a name="next-steps"></a>Volgende stappen

U kunt aan de slag met Video Indexer. Raadpleeg voor meer informatie de volgende artikelen:

- [Aan de slag met de Video Indexer-website](video-indexer-get-started.md)
- [REST-API van Video Indexer gebruiken](video-indexer-use-apis.md)
- [Video Indexer-widgets insluiten in uw toepassingen](video-indexer-embed-widgets.md)
