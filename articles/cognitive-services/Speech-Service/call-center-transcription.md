---
title: Call Center Transcriptie - spraakservices
titleSuffix: Azure Cognitive Services
description: Een veelvoorkomend scenario voor spraak-naar-tekst is te transcriberen grote volumes van de telephony-gegevens die afkomstig uit verschillende systemen, zoals Interactive Voice Response (IVR zijn). De audio mag stereo of mono en onbewerkte met weinig geen naverwerking uitgevoerd op het signaal. Met behulp van Speech Services en het model van de spraakherkenning Unified, krijgt een bedrijf hoogwaardige transcripties, met veel vastleggen systeemaudio-systemen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 055d141cab8ece3fcb462573f6ed4d8941c19751
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064129"
---
# <a name="speech-services-for-telephony-data"></a>Spraakservices voor TAPI-gegevens

Telephony-gegevens die worden gegenereerd door landlines, mobiele telefoons en radio's zijn doorgaans lage kwaliteit en smalband binnen het bereik van 8 KHz, die zorgt voor problemen bij het converteren van spraak-naar-tekst. De meest recente spraakmodellen voor herkenning van Azure Speech Services blinken uit in het te transcriberen deze telephony-gegevens, zelfs in gevallen wanneer de gegevens is het moeilijk voor een human om te begrijpen. Deze modellen zijn getraind met grote hoeveelheden gegevens van de telefoon en hebben beste in de nauwkeurigheid van de markt, zelfs in omgevingen met veel ruis.

Een veelvoorkomend scenario voor spraak-naar-tekst is te transcriberen grote volumes van de telephony-gegevens die afkomstig uit verschillende systemen, zoals Interactive Voice Response (IVR zijn). De audio deze systemen bieden kan worden stereo of mono en onbewerkte met weinig geen naverwerking uitgevoerd op het signaal. Met behulp van Speech Services en het gecombineerde spraak-model, krijgt een bedrijf hoogwaardige transcripties, ongeacht de systemen die wordt gebruikt voor het vastleggen van audio.

Telephony gegevens kunnen worden gebruikt om beter inzicht in de behoeften van uw klanten, nieuwe marketing kansen te identificeren of de prestaties van call center-agents evalueren. Nadat de gegevens is omgezet, kunt de uitvoer in een bedrijf gebruiken voor verbeterde telemetrie, sleuteltermen identificeren en analyseren van sentiment van klanten.

De technologieën die worden beschreven in deze pagina zijn door Microsoft intern voor verschillende services call verwerking in realtime ondersteunen en de batchmodus.

Laten we bekijken enkele van de technologie en verwante functies Azure Speech Services biedt.

> [!IMPORTANT]
> Speech Services Unified model wordt getraind met gevarieerde en biedt een oplossing voor één model voor een aantal scenario van dicteren naar Telephony analytics.

## <a name="azure-technology-for-call-centers"></a>Azure-technologie voor callcenters

Naast de functionele aspecten van de spraakservices is hun primaire doel – wanneer toegepast op het callcenter – om de klantervaring te verbeteren. Drie duidelijke domeinen bestaan in dit opzicht:

* Na aanroep analytics batch-dat wil zeggen, verwerking van de aanroep van opnamen 
* Analyses in realtime verwerking van de audio signaal dat moet worden verschillende inzichten uit te extraheren als de oproep is plaatsvinden (met gevoel wordt een prominente use case) en
* Virtuele assistenten (Bots), de dialoog verkeer tussen de klant en de bot in een poging om het probleem met geen agent deelname van de klant of wordt de toepassing van AI protocollen om te helpen de agent.

Een typische architectuur voor een diagram van de implementatie van een batch-scenario wordt beschreven in de onderstaande afbeelding ![Call center transcriptie architectuur](media/scenarios/call-center-transcription-architecture.png)

## <a name="speech-analytics-technology-components"></a>Spraak analyse technologie-onderdelen

Of het domein na oproep of realtime is, biedt Azure een set volwassen en opkomende reeks technologieën om de klantervaring te verbeteren. 

### <a name="speech-to-text-stt"></a>Spraak naar tekst (STT) 

[Spraak-naar-tekst](speech-to-text.md) is de populairste na de functie in een call center-oplossing. Omdat veel van de downstream analytics processen zijn afhankelijk van de getranscribeerde tekst, wordt de frequentie van word fouten (Windows Foutrapportage) van het grootste belang is. Een van de belangrijkste uitdagingen in call center transcriptie is de ruis die wordt gebruikt in de callcenter (bijvoorbeeld andere agents op de achtergrond spreken), de uitgebreide verschillende taalinstellingen en dialecten, evenals de lage kwaliteit van het werkelijke telefoon signaal. Windows Foutrapportage is nauw verband houden met hoe goed de akoestische en modellen worden getraind voor een bepaalde landinstelling, dus is het kunnen aanpassen van het model op uw landinstelling is belangrijk. Onze nieuwste gecombineerde versie 4.x-modellen zijn de oplossing voor zowel transcriptie nauwkeurigheid en latentie. Getraind met tientallen van duizenden uur om akoestische gegevens en miljarden lexicale informatie Unified modellen zijn de meest nauwkeurige modellen in de markt te transcriberen call center-gegevens.

### <a name="sentiment"></a>Sentiment
Rendementscurves of de klant een goede ervaring heeft is een van de belangrijkste gebieden van spraakanalyse wanneer toegepast op de call center-ruimte. Onze [Batch transcriptie API](batch-transcription.md) sentimentanalyse per utterance biedt. U kunt de set met waarden die zijn verkregen als onderdeel van een transcript aanroep om te bepalen het gevoel van de aanroep voor zowel de agents als de klant samenvoegen.

### <a name="silence-non-talk"></a>Stilte (niet-presentatie)
het is niet ongebruikelijk dat 35 procent van een aanroep van ondersteuning worden zogeheten niet gesprekstijd. Sommige scenario's die niet-contact vindt plaats zijn: agents opzoeken van geschiedenis van de voorafgaande aanvraag met een klant, agents met behulp van hulpprogramma's waarmee ze toegang tot bureaublad van de klant en uitvoeren van functies, klanten die zich op houdt wachten op een overdracht enzovoort. Het is uitermate belangrijk om te meten wanneer stilte in een aanroep optreedt als er belangrijke klant gevoeligheden die zich voordoen rond dit soort scenario's en waar ze zich voordoen in de aanroep van het aantal.

### <a name="translation"></a>Omzetting
Sommige bedrijven experimenteren met het leveren van vertaalde Transcripten van vreemde talen ondersteuningsaanvragen worden ingediend zodat levering managers de world wide-ervaring van hun klanten begrijpt. Onze [vertaling](translation.md) mogelijkheden voor ongeëvenaard zijn. We kunnen audio-audio- of audio naar tekst vertalen van een groot aantal landinstellingen.

### <a name="text-to-speech"></a>Tekst naar spraak
[Tekst naar spraak](text-to-speech.md) is een ander belangrijk aspect in de implementatie van bots die interactie hebben met de klanten. De typische route is dat de klant spreekt, hun stem is getranscribeerde tekst, de tekst wordt geanalyseerd voor intents, een antwoord is gemaakt op basis van de herkende intentie en vervolgens een asset ofwel wordt opgehaald voor de klant of een kunstmatige stem-antwoord is gegenereerd. Natuurlijk is dit allemaal optreden snel – latentie is dus een belangrijk onderdeel van het succes van deze systemen. 

Onze end-to-end-latentie is vrij laag overweegt de verschillende technologieën zoals betrokken [spraak-naar-tekst](speech-to-text.md), [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/), [Bot Framework](https://dev.botframework.com/), [ Tekst naar spraak](text-to-speech.md). 

Onze nieuwe stemmen zijn ook niet te onderscheiden van menselijke stemmen. Van stemmen kunt u uw bot de unieke karakter geven.

### <a name="search"></a>Search
Een andere nieten van analytics is het identificeren van interacties waarbij een bepaalde gebeurtenis of ervaring is opgetreden. Dit gebeurt meestal met een van twee benaderingen, een ad-hoc zoekopdracht waar de gebruiker een zin te typen en het systeem reageert of een meer gestructureerde query, waarin een analist kunt maken een set van logische instructies die worden geïdentificeerd door een scenario in een aanroep , en vervolgens elke aanroep op basis van deze set van query's kan worden geïndexeerd. Voorbeeld van een goede zoekopdracht wordt de instructie alomtegenwoordige naleving 'deze aanroep worden geregistreerd voor kwaliteit doeleinden... "– zoals veel bedrijven er zeker van te zijn willen dat hun agents dit fragment zijn bieden aan klanten voordat de aanroep daadwerkelijk wordt vastgelegd. De meeste analytics systemen hebben de mogelijkheid om trend van het gedrag door query /search algoritmen gevonden – als deze rapportage van trends uiteindelijk een van de belangrijkste functies van een analytics-systeem is. Via [Cognitive services-directory](https://azure.microsoft.com/services/cognitive-services/directory/search/) uw end-to-endoplossing kan aanzienlijk worden verbeterd met mogelijkheden voor indexeren en zoeken.

### <a name="key-phrase-extraction"></a>Sleuteltermextractie
Dit gebied is een van de meer uitdaging zijn om toepassingen voor gegevensanalyse en één die wordt profiteert van de toepassing van AI en ML. De primaire scenario hier is het afleiden van het doel van de klant. Waarom is de klant aanroepen? Wat is het probleem van de klant? Waarom de klant beschikt over een negatieve ervaring? Onze [Text analytics-service](https://azure.microsoft.com/services/cognitive-services/text-analytics/) biedt een set analytics buiten het vak voor het upgraden van snel uw end-to-end oplossing om uit te pakken die belangrijke zoekwoorden of zinnen.

We hebben nu eens naar de batch-verwerking en de realtime pijplijnen voor spraakherkenning in iets meer details.

## <a name="batch-transcription-of-call-center-data"></a>Batch transcriptie call center-gegevens

Voor het grootste deel van de audio te transcriberen we ontwikkeld die de [Batch transcriptie API](batch-transcription.md). De Batch-API voor transcriptie is ontwikkeld voor grote hoeveelheden audiogegevens asynchroon transcriberen. Met betrekking tot te transcriberen call center-gegevens, is onze oplossing op basis van deze onderdelen:

* **Nauwkeurigheid**: Met de vierde generatie Unified modellen bieden we ongekende transcriptie kwaliteit.
* **Latentie**: We begrijpen dat bij het uitvoeren van bulksgewijs transcripties, de transcripties nodig zijn snel. De taken transcriptie gestart de [Batch transcriptie API](batch-transcription.md) wordt in de wachtrij geplaatst onmiddellijk, en nadat de taak wordt gestart met het sneller dan de real-time transcriptie wordt uitgevoerd.
* **Beveiliging**: We begrijpen dat aanroepen mogelijk gevoelige gegevens bevatten. Voel u beveiliging is een van onze hoogste prioriteiten. Onze service heeft verkregen, ISO, SOC, HIPAA, PCI-certificering.

Callcenters grote hoeveelheden audiogegevens dagelijks genereren. Als uw bedrijf telephony-gegevens worden opgeslagen in een centrale locatie, zoals Azure Storage, kunt u de [Batch transcriptie API](batch-transcription.md) asynchroon aanvragen en ontvangen transcripties.

Een typische oplossing maakt gebruik van deze services:

* Azure Speech Services worden gebruikt voor spraak-naar-tekst te transcriberen. Een standard-abonnement (Enzovoort) voor de Speech Services is vereist voor het gebruik van de Batch transcriptie-API. Gratis abonnementen (F0) werkt niet.
* [Azure Storage](https://azure.microsoft.com/services/storage/) wordt gebruikt voor het opslaan van de telephony-gegevens en de transcripties geretourneerd door de Batch transcriptie-API. Dit opslagaccount moet meldingen, specifiek voor gebruiken wanneer er nieuwe bestanden worden toegevoegd. Deze meldingen worden gebruikt voor het activeren van het proces transcriptie.
* [Azure Functions](https://docs.microsoft.com/azure/azure-functions/) wordt gebruikt voor het maken van de handtekeningen voor gedeelde toegang (SAS) URI voor elke opname en trigger van de HTTP POST-aanvraag om te beginnen een transcriptie. Daarnaast wordt Azure Functions gebruikt om te maken van aanvragen voor het ophalen en verwijderen met behulp van de Batch-API voor transcriptie transcripties.
* [WebHooks](webhooks.md) worden gebruikt voor het ontvangen van meldingen wanneer transcripties zijn voltooid.

Intern gebruiken we de bovenstaande technologieën voor de ondersteuning van Microsoft customer aanroept in de batchmodus.
![Batch-architectuur](media/scenarios/call-center-batch-pipeline.png)

## <a name="real-time-transcription-for-call-center-data"></a>Realtime transcriptie voor call center-gegevens

Sommige bedrijven zijn transcriberen conversaties in realtime vereist. Realtime transcriptie kan worden gebruikt om te bepalen van de sleutel woorden en zoekt naar inhoud en bronnen die relevant zijn voor het gesprek, voor het bewaken van sentiment, voor het verbeteren van toegankelijkheid, of voor vertalingen voor klanten als agents die geen systeemeigen activeren sprekers.

Voor scenario's waarvoor realtime transcriptie, wordt u aangeraden de [spraak SDK](speech-sdk.md). Spraak-naar-tekst is momenteel beschikbaar in [meer dan 20 talen](language-support.md), en de SDK is beschikbaar in C++, C#, Java, Python, Node.js, Objective-C en JavaScript. Voorbeelden zijn beschikbaar in elke taal op [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk). Zie voor het laatste nieuws en updates [opmerkingen bij de Release](releasenotes.md).

Intern gebruiken we de bovenstaande technologieën voor analyse in realtime gesprekken met Microsoft-klanten wanneer deze zich voordoen.

![Batch-architectuur](media/scenarios/call-center-reatime-pipeline.png)

## <a name="a-word-on-ivrs"></a>Een woord op IVRs

Speech Services gemakkelijk kunnen worden geïntegreerd in een oplossing met behulp van de [spraak SDK](speech-sdk.md) of de [REST-API](rest-apis.md). Call center transcriptie mogelijk echter aanvullende technologieën. Een verbinding tussen een IVR-systeem en Azure is normaal gesproken vereist. Hoewel we deze onderdelen bieden, willen we beschrijven wat een verbinding met een IVR inhoudt.

Verschillende IVR of telephony serviceproducten (zoals Genesys of AudioCodes) bieden mogelijkheden voor gegevensintegratie die kunnen worden gebruikt om binnenkomend en uitgaand audio passthrough naar een Azure-Service inschakelen. Een aangepaste Azure-service mogelijk in feite, voorzien van een specifieke interface om te definiëren van sessies telefonische oproep (zoals aanroepen begin of einde aanroepen) en een WebSocket-API voor het ontvangen van binnenkomende stream audio die wordt gebruikt met de Speech Services beschikbaar. Uitgaande antwoorden, zoals conversatie transcriptie of verbindingen met de Bot Framework kunnen worden gemaakt met de service van Microsoft voor tekst naar spraak en geretourneerd naar de IVR om te worden afgespeeld.

Een ander scenario is Direct SIP-integratie. Een Azure-service maakt verbinding met een SIP-Server, dus ophalen van een binnenkomende gegevensstroom en een uitgaande stroom, die wordt gebruikt voor de fasen van de spraak-naar-tekst en spraak. Verbinding maken met een SIP-Server er commerciële software-aanbiedingen, zoals Ozeki SDK, zijn of [de Teams aanroepen en de vergaderingen API](https://docs.microsoft.com/graph/api/resources/calls-api-overview?view=graph-rest-beta) (momenteel in bètatoestand is), die zijn ontworpen ter ondersteuning van dit soort scenario voor audio-aanroepen.

## <a name="customize-existing-experiences"></a>Bestaande ervaring aanpassen

Azure Speech Services werkt goed met ingebouwde modellen, kunt u echter verder aanpassen en afstemmen van de ervaring voor het product of de omgeving. Aanpassing van opties voor het bereik van akoestisch model afstemmen op de unieke spraakstijlen voor uw merk. Nadat u een aangepast model hebt gemaakt, kunt u deze kunt gebruiken met een van de Azure-spraakservices zowel in realtime of in de batchmodus.

| Spraakservice | Model | Description |
|----------------|-------|-------------|
| Spraak-naar-tekst | [Akoestisch model](how-to-customize-acoustic-models.md) | Maak een aangepast akoestisch model voor toepassingen, hulpprogramma's, of apparaten die worden gebruikt in het bijzonder omgevingen, zoals in een auto of op een fabriek, elk met specifieke opname voorwaarden. Voorbeelden zijn onder meer accenten zijn niet toegestaan spraak, specifieke achtergrondgeluiden of met behulp van een specifieke microfoon voor de opname. |
| | [Taalmodel](how-to-customize-language-model.md) | Maak een aangepast taalmodel ter verbetering van transcriptie van branchespecifieke vocabulaire en grammatica, zoals medische terminologie of IT-jargon. |
| | [Uitspraakmodel](how-to-customize-pronunciation.md) | Met een aangepaste uitspraak-model, kunt u de fonetische vorm en de weergave van een woord of een term kunt definiëren. Dit is handig voor het verwerken van aangepaste voorwaarden, zoals productnamen of afkortingen. Alles wat u nodig hebt om te beginnen is een uitspraak van bestand--een eenvoudige txt-bestand. |
| Tekst naar spraak | [Spraakstijl](how-to-customize-voice-font.md) | Aangepaste spraakstijlen kunnen u een herkenbare, één van een unieke stem voor uw merk maken. Het duurt slechts een kleine hoeveelheid gegevens aan de slag. Hoe meer gegevens dat u opgeeft, wordt de meer natuurlijke en menselijke uw spraakstijl klinkt. |

## <a name="sample-code"></a>Voorbeeldcode

Voorbeeldcode is beschikbaar op GitHub voor elk van de Azure-Services voor spraak. Deze voorbeelden voor algemene scenario's, zoals lezen van audio van een bestand of de stroom, herkenning van doorlopende en één en werken met aangepaste modellen. Gebruik deze koppelingen om weer te geven SDK en REST-voorbeelden:

* [Spraak-naar-tekst en spraak NAT-voorbeelden (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Voorbeelden van batch transcriptie (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
* [Text to Speech-voorbeelden (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="reference-docs"></a>Referentiedocumenten

* [Speech-SDK](speech-sdk-reference.md)
* [Spraak apparaten SDK](speech-devices-sdk.md)
* [REST API: Spraak-naar-tekst](rest-speech-to-text.md)
* [REST API: Text-to-speech](rest-text-to-speech.md)
* [REST API: Batch transcriptie en aanpassen](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ontvangt u een abonnementssleutel Speech Services gratis](get-started.md)
