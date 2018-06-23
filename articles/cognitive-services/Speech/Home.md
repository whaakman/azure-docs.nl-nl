---
title: De Service Microsoft Speech | Microsoft Docs
description: Gebruik Microsoft Speech-API spraak gestuurde activiteiten toevoegen aan uw apps, met inbegrip van realtime interactie met gebruikers.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: c041132e992f07e94e4b6669ec7ce174f7c2d0dd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344682"
---
# <a name="microsoft-speech-api-overview"></a>Microsoft Speech-API-overzicht

De cloud-gebaseerde Microsoft Speech-API biedt ontwikkelaars een eenvoudige manier om andere van krachtige functies voor spraak-functionaliteit in hun toepassingen, zoals voice opdrachtbesturingselement, met behulp van natuurlijke spraak gesprek, en spraak schrijffouten en dicteren gebruikersdialoogvenster maken. De Microsoft Speech-API ondersteunt zowel *spraak naar tekst* en *Text to Speech* conversie.

- **Spraak naar tekst** API menselijke stem converteert naar tekst die kan worden gebruikt als invoer of opdrachten om te bepalen van uw toepassing.
- **Text to Speech** API tekst wordt geconverteerd naar audio stromen die kunnen worden afgespeeld naar de gebruiker van uw toepassing.

## <a name="speech-to-text-speech-recognition"></a>Spraak naar tekst (spraakherkenning)

Microsoft-spraakherkenning API *transcribes* audio stromen naar tekst die uw toepassing kan worden weergegeven voor de gebruiker of bij het fungeren als invoer opdracht. Het biedt twee manieren voor ontwikkelaars spraak toevoegen aan hun apps: REST-API's **of** Websocket gebaseerde clientbibliotheken.

- [REST API's](GetStarted/GetStartedREST.md): ontwikkelaars kunnen gebruikmaken van HTTP-aanroepen vanuit hun apps naar de service voor spraakherkenning.
- [Clientbibliotheken](GetStarted/GetStartedClientLibraries.md): voor geavanceerde functies ontwikkelaars kunnen Microsoft Speech clientbibliotheken downloaden, en in hun apps koppelen.  De clientbibliotheken zijn beschikbaar op verschillende platforms (Android, Windows, iOS) met behulp van verschillende talen (C#, Java, JavaScript, ObjectiveC). In tegenstelling tot de REST-API's gebruikmaken van de clientbibliotheken procotol op basis van Websocket.

| Gebruiksvoorbeelden | [REST API's](GetStarted/GetStartedREST.md) | [Clientbibliotheken](GetStarted/GetStartedClientLibraries.md) |
|-----|-----|-----|
| Converteert een kort gesproken audio opdrachten bijvoorbeeld (audio lengte < 15 s) zonder tussentijdse resultaten | Ja | Ja |
| Converteren van een lang (> 15 s)-audio | Nee | Ja |
| Stroom audio met tussentijdse resultaten gewenst | Nee | Ja |
| De tekst geconverteerd van audio met LUIS begrijpen | Nee | Ja |

Welke aanpak ontwikkelaars kiezen (REST API of clientbibliotheken), Microsoft spraak-service ondersteunt het volgende:

- Geavanceerde spraak technologieën van Microsoft die door Cortana, Office dicteren Office conversieprogramma en andere Microsoft-producten worden gebruikt.
- Realtime continue herkenning. Spraakherkenning API kan gebruikers audio transcriberen naar tekst in realtime, met ondersteuning voor het ontvangen van de tussenliggende resultaten van de woorden die tot nu toe is herkend. De service spraak ondersteunt ook detectie van het einde van spraak. Bovendien kunnen gebruikers meer opmaak mogelijkheden, zoals hoofdlettergebruik en leestekens, maskering taalgebruik en tekst normalisatie kiezen.
- Ondersteunt geoptimaliseerd spraak herkenningsresultaten voor *interactieve*, *conversatie*, en *dicteren* scenario's. Voor gebruiker-scenario's waarvoor de taal van de aangepaste modellen en akoestisch modellen [aangepaste spraak Service](../custom-speech-service/cognitive-services-custom-speech-home.md) kunt u spraak modellen die op uw toepassing en uw gebruikers toegespitst maken.
- Ondersteuning voor veel talen in meerdere dialecten. Zie voor een volledige lijst van ondersteunde talen in beide modi erkenning [talen](api-reference-rest/supportedlanguages.md).
- Integratie met language understanding. Naast de invoer audio converteren naar tekst, de *spraak naar tekst* biedt toepassingen een aanvullende mogelijkheid om te begrijpen wat de tekst. Dit maakt gebruik van de [Language Understanding Intelligent Service(LUIS)](../LUIS/Home.md) intents en entiteiten ophalen uit de herkende tekst.

### <a name="next-steps"></a>Volgende stappen

- Aan de slag te gebruiken Microsoft speech recognition service met [REST-API's](GetStarted/GetStartedREST.md) of [clientbibliotheken](GetStarted/GetStartedClientLibraries.md).
- Bekijk [voorbeeldtoepassingen](samples.md) in de gewenste programmeertaal.
- Ga naar de sectie Verwijzingen vinden [Microsoft spraak Protocol](API-Reference-REST/websocketprotocol.md) details en API-verwijzingen.

## <a name="text-to-speech-speech-synthesis"></a>Text to speech (spraak synthese)

*Text to Speech* REST API's gebruikt gestructureerde tekst converteren naar een audiostroom. De API's bieden snel tekst-naar-spraak conversie in verschillende stemmen en talen. Bovendien hebben gebruikers de mogelijkheid om te wijzigen audiogegevens zoals uitspraak, volume presentatie enzovoort. met behulp van SSML-tags.

### <a name="next-steps"></a>Volgende stappen

- Gebruik Microsoft tekst-naar-spraak service aan de slag: [tekst Speech-API-referentiemateriaal](api-reference-rest/bingvoiceoutput.md). Zie voor een volledige lijst met talen en stemmen ondersteund door de tekst-naar-spraak, [ondersteund landinstellingen en stem lettertypen](api-reference-rest/bingvoiceoutput.md#SupLocales).
