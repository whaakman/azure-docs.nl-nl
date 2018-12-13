---
title: Over spraak naar tekst - spraakservices
titleSuffix: Azure Cognitive Services
description: De spraak-naar-tekst-API transcribes audiostreams naar tekst die uw app kunt weergeven of erop als invoer. De service is beschikbaar via de SDK en een RESTful-eindpunt.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 050d395614fa4a08e6d0d0967200029e8fc36010
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53094709"
---
# <a name="about-the-speech-to-text-api"></a>Informatie over de Speech to Text-API

De **spraak naar tekst** API *transcribes* audiostreams naar tekst die uw toepassing kan worden weergegeven voor de gebruiker of bij het fungeren als invoer opdracht. De API's kunnen worden gebruikt met een SDK-clientbibliotheek (voor ondersteunde platforms en talen) of een REST-API.

De **spraak naar tekst** API biedt de volgende functies:

- Geavanceerde spraakherkenningstechnologie van Microsoft, hetzelfde zijn als voor Cortana, Office en andere Microsoft-producten.

- Realtime continue herkenning. **Spraak naar tekst** kunnen gebruikers audio naar tekst in realtime te transcriberen. Het ondersteunt ook tussenliggende resultaten van de woorden die tot nu toe zijn herkend. De service automatisch worden herkend als het einde van spraak. Gebruikers kunnen ook aanvullende opmaakopties, met inbegrip van hoofdletters en interpunctie, grof taalgebruik maskeren en inverse tekst normalisering kiezen.

- De resultaten weergegeven in formulieren voor zowel Lexical en weergeven (lexicale resultaten, Zie DetailedSpeechRecognitionResult in voor de voorbeelden of API).

- Ondersteuning van verschillende talen en dialecten. Zie voor de volledige lijst van ondersteunde talen in beide modi erkenning, [ondersteunde talen](language-support.md#speech-to-text).

- Aangepaste taal en akoestische modellen, zodat u uw toepassing op het vocabulaire van uw gebruikers gespecialiseerde domein, spreekstijl omgeving en manier aanpassen kunt van spreken.

- Natuurlijke taal begrijpen. Dankzij de integratie met [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS), u kunt afleiden intenties en entiteiten van spraak. Gebruikers hoeft te weten vocabulaire van uw app, maar wordt beschreven wat ze willen in hun eigen woorden.

- Als u een gedetailleerde uitvoer van de spraak-configuratieobject (SpeechConfig.OutputFormat eigenschap) opgeeft, wordt de betrouwbaarheidsscore geretourneerd reactie van de service. Vervolgens kunt u een van beide methoden Best() op het resultaat gebruiken of deze rechtstreeks de score ophalen van JSON geretourneerd door de service (ongeveer als resultaat. Properties.GetProperty(PropertyId.SpeechServiceResponse_JsonResult)).

## <a name="api-capabilities"></a>API-functies

Sommige van de mogelijkheden van de **spraak naar tekst** API, met name wat betreft de aanpassing kunnen zijn beschikbaar via REST. De volgende tabel geeft een overzicht van de mogelijkheden van elke methode van de toegang tot de API. Zie voor een volledige lijst van de mogelijkheden en API-details, [Swagger verwijzing](https://westus.cris.ai/swagger/ui/index).

| Use-case | REST | SDK's |
|-----|-----|-----|----|
| Een korte utterance, zoals een opdracht (lengte < 15 s); transcriberen Er zijn geen tijdelijke resultaten | Ja | Ja |
| Transcriberen een langer utterance (> 15 s) | Nee | Ja |
| Streaming audio met optionele tussentijdse resultaten transcriberen | Nee | Ja |
| Sprekerherkenning intents via LUIS begrijpen | Nee\* | Ja |
| Nauwkeurigheidstests maken | Ja | Nee |
| Gegevenssets voor modelaanpassing uploaden | Ja | Nee |
| Maken en beheren van modellen voor spraakherkenning | Ja | Nee |
| Maken en beheren van modelimplementaties | Ja | Nee |
| Abonnementen beheren | Ja | Nee |
| Maken en beheren van modelimplementaties | Ja | Nee |
| Maken en beheren van modelimplementaties | Ja | Nee |

> [!NOTE]
> De REST-API implementeert die de API-aanvragen voor 25 per van vijf seconden beperkingslimieten. Berichtkoppen wordt op de hoogte van de limieten

\* *LUIS intenties en entiteiten kunnen worden afgeleid met behulp van een afzonderlijk LUIS-abonnement. Met dit abonnement, de SDK LUIS-aanroep voor u en bieden entiteit en voor welk doel resultaten, evenals transcriptie van spraak. Met de REST-API, roept u LUIS zelf voor het afleiden van intenties en entiteiten met uw LUIS-abonnement.*

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Snelstartgids: herkennen gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
* [Zie voor het herkennen van intenties van gesproken tekst in C#](how-to-recognize-intents-from-speech-csharp.md)
