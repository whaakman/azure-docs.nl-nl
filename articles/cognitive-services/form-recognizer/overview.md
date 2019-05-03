---
title: Wat is een formulier-herkenning?
titleSuffix: Azure Cognitive Services
description: Informatie over het gebruik van het formulier herkenning formulier-en tabelgegevens parseren.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: overview
ms.date: 04/08/2019
ms.author: pafarley
ms.openlocfilehash: df3db534550e709e40cc94d5f951056d93a1003e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027073"
---
# <a name="what-is-form-recognizer"></a>Wat is een formulier-herkenning?

Herkenning van Azure formulier is een cognitive service die gebruikmaakt van machine learning-technologie om te bepalen en de sleutel / waarde-paren en tabelgegevens extraheren uit de form-documenten. Vervolgens voert hier gestructureerde gegevens uit die de relaties in het oorspronkelijke bestand bevat. U kunt uw aangepaste formulier herkenning-model met behulp van een eenvoudige REST-API om te kunnen de complexiteit en eenvoudig integreren in uw werkstroom of een toepassing kunt aanroepen. U hoeft slechts vijf formulier documenten of een leeg formulier van hetzelfde type als uw invoer materiaal aan de slag. U kunt resultaten snel, nauwkeurig en die zijn afgestemd op uw specifieke inhoud zonder de noodzaak van zware handmatige tussenkomst of uitgebreide data science expertise.

## <a name="request-access"></a>Toegang aanvragen
Herkenning van formulier is beschikbaar als preview beperkte toegang. Voor toegang tot de Preview-versie, vul het en verzenden de [toegangsaanvraag voor herkenning van Cognitive Services-formulier](https://aka.ms/FormRecognizerRequestAccess) formulier. Het formulier vraagt om informatie over u, uw bedrijf en het scenario voor gebruikers waarvoor u formulier herkenning gebruikt. Als uw aanvraag is goedgekeurd door het team van Azure Cognitive Services, ontvangt u een e-mailbericht met instructies over hoe u toegang tot de service.

## <a name="what-it-does"></a>Wat het doet

Wanneer u uw gegevens verzendt, wordt het algoritme traint, clusters de formulieren door te typen, detecteert welke sleutels en tabellen aanwezig zijn, en leert om waarden voor sleutels en vermeldingen naar tabellen te koppelen. Leren zonder supervisie kan het model om te begrijpen van de indeling en de relaties tussen de velden en items zonder gegevens handmatig labelen of intensieve coderen en onderhoud. Vooraf getrainde machine learning-modellen gestandaardiseerde gegevens vereist en zijn minder nauwkeurig met invoer materiaal dat van traditionele indelingen afwijkt, zoals daarentegen branchespecifieke formulieren.

Zodra het model wordt getraind, kunt u testen, opnieuw trainen en uiteindelijk gebruiken op een betrouwbare manier om gegevens te extraheren uit meer formulieren op basis van uw behoeften.

## <a name="what-it-includes"></a>Samenstelling van Content Moderator

Herkenning van formulier is beschikbaar als een REST-API. U kunt maken, trainen en een model beoordelen door de API aan te roepen, en kunt u eventueel het model in een lokale Docker-container uitvoeren.

## <a name="input-requirements"></a>Vereisten voor invoer

Formulier herkenning werkt op invoer documenten aan de volgende vereisten voldoen:

* JPG, PNG- of een PDF-indeling (tekst of gescand). Tekst ingesloten PDF-bestanden zijn het geschiktst omdat er geen mogelijkheid om de fout in de extractie van tekens en de locatie.
* Bestandsgrootte moet minder dan 4 megabyte (MB)
* Voor installatiekopieën van moeten dimensies liggen tussen 50 x 50 en 4200 x 4200 pixels
* Als gescand van papierdocumenten, moet formulieren scans van hoge kwaliteit
* Het Latijnse alfabet (Engelse tekens) moet gebruiken
* Gegevens die worden afgedrukt (niet handgeschreven)
* Sleutels en waarden moet bevatten
* Sleutels kunnen worden weergegeven boven of links van de waarden, maar niet minder dan of aan de rechterkant.

Formulier herkenning biedt de volgende typen invoergegevens bovendien nog geen ondersteuning:

* Complexe tabellen (geneste tabellen, samengevoegde koptekst of cellen, enzovoort) 
* Selectievakjes of radio knoppen
* PDF-documenten die langer zijn dan 50 pagina 's

## <a name="where-do-i-start"></a>Waar moet ik beginnen?

**Stap 1:** Een formulier herkenning-resource maken in Azure portal.

**Stap 2:** Raadpleeg een snelstartgids voor praktische ervaring:
* [Snelstart: Een formulier herkenning-model te trainen en extraheren van gegevens met behulp van REST-API met cURL](quickstarts/curl-train-extract.md)
* [Snelstart: Een formulier herkenning-model te trainen en extraheren van gegevens met behulp van REST-API met Python](quickstarts/python-train-extract.md)

We raden aan de gratis service voor educatieve doeleinden, maar houd er rekening mee dat het aantal gratis pagina's beperkt tot 500 pagina's per maand is.

**Stap 3:** Controleer het gebruik van de REST-API de volgende API's om te trainen en extraheer gestructureerde gegevens uit formulieren.

| REST-API | Description |
|-----|-------------|
| Trainen | Een nieuw model voor het analyseren van uw formulieren met behulp van 5 formulieren van hetzelfde type of een leeg formulier trainen.  |
| Analyseren  |Analyseer één document doorgegeven als een stroom om op te halen van sleutel / waarde-paren en tabellen in het formulier met uw aangepaste model.  |

Verken de [REST-API-verwijzing document](https://aka.ms/form-recognizer/api). 

## <a name="data-privacy-and-security"></a>Gegevensprivacy en -beveiliging

De service wordt aangeboden als een [Preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) van een Azure-Service onder de [voorwaarden voor Online-Service](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31). U behoudt de eigendom van uw gegevens en we alleen gebruiken voor de Online Services, zoals wordt beschreven in de overeenkomst:

### <a name="processing-of-customer-data-ownership"></a>Verwerking van gegevens van klanten; eigendom

Gegevens van de klant worden gebruikt of anders verwerkt alleen gebruikt om toegang te bieden klanten de Online Services, met inbegrip van doeleinden die compatibel zijn met het leveren van die services. Microsoft zal niet gebruiken of anders klantgegevens verwerken of afleidt voor reclame- of vergelijkbare commerciële doeleinden. Tussen de partijen, wordt de klant behoudt alle rechten, titel en belangen in en op gegevens van de klant. Microsoft heeft geen rechten in klantgegevens, dan de klant naar Microsoft om de Online Services aanbieden aan klanten verleent rechten.

Als met de Cognitive Services, moeten ontwikkelaars die gebruikmaken van de service formulier herkenning rekening houden met het beleid van Microsoft op gegevens van de klant. Zie de [pagina Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) (Engelstalig) in het Microsoft Trust Center voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Ga als volgt een [snelstartgids](quickstarts/curl-train-extract.md) aan de slag met behulp van de [Form-herkenning API's](https://aka.ms/form-recognizer/api).
