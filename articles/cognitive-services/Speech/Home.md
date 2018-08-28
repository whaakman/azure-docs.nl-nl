---
title: Microsoft Bing Speech-Service | Microsoft Docs
description: Microsoft Speech-API gebruiken voor het spraakgestuurde acties toevoegen aan uw apps, met inbegrip van realtime-interactie met gebruikers.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: d58642b95a60d4f1c83dfd969d0c76511dca4653
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43097392"
---
# <a name="microsoft-bing-speech-api-overview"></a>Overzicht van Microsoft Bing Speech-API

De cloud gebaseerde Microsoft Bing Speech-API biedt ontwikkelaars een eenvoudige manier om te maken van krachtige functies voor spraak ingeschakelde in hun toepassingen, zoals voice opdrachtbesturingselement, met behulp van natuurlijke taal gesprek, en spraaktranscriptie en dicteren dialoogvenster voor de gebruiker. De Microsoft Speech-API ondersteunt zowel *spraak naar tekst* en *tekst naar spraak* conversie.

- **Spraak naar tekst** API converteert menselijke spraak naar tekst die kan worden gebruikt als invoer of opdrachten voor het beheren van uw toepassing.
- **Tekst naar spraak** API converteert tekst naar audio stromen die kunnen worden afgespeeld op de gebruiker van uw toepassing.

> [!NOTE] 
> In mei 2018, hebben we de nieuwe uitgebracht [spraakservice](../speech-service/overview.md) in openbare preview. We raden u aan [het gratis proberen](../speech-service/get-started.md).

## <a name="speech-to-text-speech-recognition"></a>Spraak naar tekst (spraakherkenning)

Microsoft-spraakherkenning API *transcribes* audiostreams naar tekst die uw toepassing kan worden weergegeven voor de gebruiker of bij het fungeren als invoer opdracht. Het biedt twee manieren voor ontwikkelaars voor het toevoegen van de spraakherkenning aan hun apps: REST-API's **of** op basis van Websocket-clientbibliotheken.

- [REST API's](GetStarted/GetStartedREST.md): ontwikkelaars kunnen gebruikmaken van HTTP-aanroepen naar de service voor spraakherkenning van hun apps.
- [Clientbibliotheken](GetStarted/GetStartedClientLibraries.md): voor geavanceerde functies, zijn de ontwikkelaars kunnen downloaden van Microsoft Speech-clientbibliotheken en de koppeling in hun apps.  De clientbibliotheken zijn beschikbaar op verschillende platformen (Windows, Android, iOS) met behulp van verschillende talen (C#, Java, JavaScript, ObjectiveC). In tegenstelling tot de REST-API's, de clientbibliotheken gebruikmaken van op basis van Websocket-protocol.

| Gebruiksvoorbeelden | [REST API's](GetStarted/GetStartedREST.md) | [Clientbibliotheken](GetStarted/GetStartedClientLibraries.md) |
|-----|-----|-----|
| (Audio lengte < 15 s) opdrachten converteren een korte gesproken audio, bijvoorbeeld zonder tussentijdse resultaten | Ja | Ja |
| Converteren van een lange audio (> 15 s) | Nee | Ja |
| Stream audio met tijdelijke resultaten gewenst | Nee | Ja |
| Inzicht in de tekst geconverteerd van audio gebruik maakt van LUIS | Nee | Ja |

Welke benadering ontwikkelaars kiezen (REST-API's of -clientbibliotheken), Microsoft spraak-service ondersteunt het volgende:

- Geavanceerde spraak technologieën van Microsoft die worden gebruikt door Cortana, dicteren Office Translator Office en andere Microsoft-producten.
- Realtime continue herkenning. De the spraakherkennings-API kan gebruikers audio naar tekst in realtime, met ondersteuning voor het ontvangen van de tussenliggende resultaten van de woorden die tot nu toe zijn herkend transcriberen. De spraakservice biedt ook ondersteuning voor end-of-speech detectie. Gebruikers kunnen daarnaast aanvullende opmaakopties mogelijkheden, zoals de hoofdletters en interpunctie, maskering grof taalgebruik en tekst normalisering kiezen.
- Ondersteunt geoptimaliseerd spraak herkenningsresultaten voor *interactieve*, *conversatie*, en *dicteren* scenario's. Voor gebruiker-scenario's waarvoor aangepaste taalmodellen en akoestische modellen [Custom Speech Service](../custom-speech-service/cognitive-services-custom-speech-home.md) kunt u spraakmodellen die zijn afgestemd op uw toepassing en uw gebruikers maken.
- Ondersteuning voor vele talen in meerdere dialecten. Zie voor de volledige lijst van ondersteunde talen in beide modi erkenning, [erkenning talen](api-reference-rest/supportedlanguages.md).
- Integratie met language understanding. Naast de audio-invoer converteren naar tekst, de *spraak naar tekst* biedt toepassingen een aanvullende mogelijkheden om te begrijpen wat de tekst betekent. Hierbij de [Language Understanding Intelligent Service(LUIS)](../LUIS/what-is-luis.md) intenties en entiteiten ophalen uit de herkende tekst.

### <a name="next-steps"></a>Volgende stappen

- Aan de slag om te gebruiken Microsoft-spraakherkenningsservice met [REST-API's](GetStarted/GetStartedREST.md) of [clientbibliotheken](GetStarted/GetStartedClientLibraries.md).
- Bekijk [voorbeeldtoepassingen](samples.md) in uw favoriete programmeertaal.
- Ga naar de sectie documentatie te vinden [Microsoft Speech Protocol](API-Reference-REST/websocketprotocol.md) details en API-verwijzingen.

## <a name="text-to-speech-speech-synthesis"></a>Tekst naar spraak (spraaksynthese)

*Tekst naar spraak* REST API's gebruiken voor het gestructureerde tekst converteren naar een audiostream. De API's bieden een snelle tekst naar spraak conversie in verschillende stemmen en talen. Daarnaast hebben gebruikers ook de mogelijkheid om te wijzigen van audio kenmerken, zoals de uitspraak, volume, presentatie, enzovoort. met behulp van SSML met tags.

### <a name="next-steps"></a>Volgende stappen

- Aan de slag om Microsoft text to speech-service te gebruiken: [tekst naar spraak-API-verwijzing](api-reference-rest/bingvoiceoutput.md). Zie voor de volledige lijst met talen en stemmen ondersteund door de tekst naar spraak, [ondersteunde landinstellingen en Spraakstijlen](api-reference-rest/bingvoiceoutput.md#SupLocales).
