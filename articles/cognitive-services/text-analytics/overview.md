---
title: Overzicht van tekst Analytics API - cognitieve Azure-Services | Microsoft Docs
description: Tekst Analytics API in Azure-cognitieve Services gevoel analyse, uitpakken van sleutel woordgroep en taal wordt gedetecteerd.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 5/02/2018
ms.author: ashmaka
ms.openlocfilehash: db5ea943f270aa512afb508668aec90cc4c90df4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345677"
---
# <a name="what-is-text-analytics-api-version-20"></a>Wat tekst Analytics API-versie 2.0 is?

De tekst Analytics-API is een cloud-gebaseerde service die geavanceerde natuurlijke taal verwerking via onbewerkte tekst bevat vier belangrijke functies: gevoel analyse, uitpakken van sleutel woordgroep taal wordt gedetecteerd en entiteit koppelen.

De API wordt ondersteund door de bronnen in [Microsoft cognitieve Services](https://docs.microsoft.com/azure/cognitive-services/), een verzameling van machine learning en AI algoritmen in de cloud, gemakkelijk kunnen worden gebruikt in uw ontwikkelingsprojecten.

## <a name="capabilities-in-text-analytics"></a>Mogelijkheden in Tekstanalyse

Analyse van tekst kan verschillende dingen betekenen, maar in cognitieve Services, de tekst Analytics API biedt vier typen analysis zoals beschreven in de volgende tabel.

| Bewerkingen| Beschrijving | API's |
|-----------|-------------|------|
|[**Gevoel analyse**](how-tos/text-analytics-how-to-sentiment-analysis.md) | Ontdek wat klanten met onbewerkte tekst aanwijzingen over positief of negatief gevoel analyseren van uw merk of onderwerp vindt. Deze API retourneert een gevoel score tussen 0 en 1 voor elk document 1 staat voor de positief.<br /> De analyse-modellen zijn pretrained met behulp van een uitgebreide hoofdtekst van de tekst en natuurlijke taal technologieën van Microsoft. Voor [talen geselecteerd](text-analytics-supported-languages.md), de API kunt analyseren en beoordelen van onbewerkte tekst die u opgeeft, resultaten rechtstreeks wordt teruggezonden naar de aanroepende toepassing. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) <br /> [.NET](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package)  |
|[**Uitpakken van sleutel woordgroep**](how-tos/text-analytics-how-to-keyword-extraction.md) | Automatisch extraheren sleutel zinnen om snel te identificeren de belangrijkste punten. Bijvoorbeeld: voor de ingevoerde tekst 'de voeding delicious is en er zijn prachtige personeel', de API te bespreken de belangrijkste punten retourneert: 'voeding' en 'prachtige personeel'.  | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) <br /> [.NET](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) |
|[**Taal wordt gedetecteerd**](how-tos/text-analytics-how-to-language-detection.md) | Voor maximaal 120 talen welke taal de ingevoerde tekst is geschreven in detecteren en rapporteren van een enkele taalcode voor elk document die op de aanvraag heeft ingediend. De taalcode is gekoppeld aan een score die aangeeft van de sterkte van de score. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) <br />  [.NET](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) | 
|[**Entiteit (Preview) koppelen**](how-tos/text-analytics-how-to-entity-linking.md) | Identificeert bekende entiteiten in uw tekst en een koppeling naar meer informatie over het web. Entiteit koppelen herkent en als een term die wordt gebruikt als een van de entiteiten afzonderlijk te onderscheiden, termen en andere formulieren word disambiguates. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) | 

## <a name="typical-workflow"></a>Standaardwerkstroom

De werkstroom is eenvoudig: verzenden van gegevens voor analyse en ingang uitvoer in uw code. Analyzers worden verbruikt als-is zonder aanvullende configuratie of aanpassen.

1. [Aanmelden](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) voor een [toegangssleutel](how-tos/text-analytics-how-to-access-key.md). De sleutel moet worden doorgegeven bij elke aanvraag.

2. [Een aanvraag formuleren](how-tos/text-analytics-how-to-call-api.md#json-schema) met uw gegevens als onbewerkte ongestructureerde tekst, in JSON.

3. De aanvraag voor het eindpunt dat tot stand gebracht tijdens boeken aanmelden, de gewenste resource toevoegen: gevoel analyse, uitpakken van sleutel woordgroep, taal wordt gedetecteerd of entiteits-id.

4. Stream of het antwoord lokaal opgeslagen. Resultaten zijn afhankelijk van de aanvraag een score gevoel, een verzameling van uitgepakte sleutel zinnen of een taalcode.

Uitvoer geretourneerd als een enkele JSON-document met resultaten voor elk document met tekst die u geboekt, op basis van-ID. U kunt vervolgens analyseren, visualiseren of de resultaten categoriseren in inzichten actie worden uitgevoerd.

Gegevens worden niet opgeslagen in uw account. Bewerkingen die worden uitgevoerd door de tekst Analytics API zijn staatloze, wat betekent dat de tekst die u opgeeft, worden verwerkt en resultaten onmiddellijk worden geretourneerd.

<a name="supported-languages"></a>

## <a name="supported-languages"></a>Ondersteunde talen

Deze sectie is verplaatst naar een apart artikel voor betere zichtbaarheid. Raadpleeg [ondersteunde talen in Text Analytics API](text-analytics-supported-languages.md) voor deze inhoud.

<a name="data-limits"></a>

## <a name="data-limits"></a>Gegevenslimieten

Alle van de tekst Analytics API-eindpunten accepteert onbewerkte tekstgegevens. De huidige limiet is 5000 tekens voor elk document; Als u analyseren van grote documenten wilt, kunt u ze onderverdelen in kleinere reeksen. Als u nog steeds een hogere limiet vereist [contact met ons opnemen](https://azure.microsoft.com/overview/sales-number/) zodat we uw vereisten.

| Limiet | Waarde |
|------------------------|---------------|
| Maximale grootte van één document | 5000 tekens gemeten door `String.Length`. |
| Maximale grootte van de gehele request | 1 MB |
| Maximum aantal documenten in een aanvraag | 1000 documenten |

De frequentielimiet is 100 aanroepen per minuut. Houd er rekening mee dat u een groot aantal documenten in één aanroep (maximaal 1000 documenten) kunt verzenden.

## <a name="unicode-encoding"></a>Unicode-codering

De tekst Analytics-API maakt gebruik van Unicode-codering voor tekstweergave en teken aantal berekeningen. Aanvragen kunnen worden verzonden in UTF-8 en UTF-16 met geen meetbare verschillen in het aantal tekens. Een codepunt voor Unicode worden gebruikt als de heuristiek voor tekenlengte en worden als gelijkwaardig beschouwd voor de doeleinden van tekst analytics gegevenslimieten. Als u `String.Length` om het aantal tekens, gebruikt u dezelfde methode we gebruiken voor het meten van de omvang van gegevens.

## <a name="next-steps"></a>Volgende stappen

Probeer eerst de [interactieve demo](https://azure.microsoft.com/services/cognitive-services/text-analytics/). U kunt een tekstinvoer (maximaal 5000 tekens) om te bepalen in welke taal (maximaal 120), een gevoel score berekenen plakken of uitpakken van sleutel zinnen. Er is geen registratie nodig.

Wanneer u klaar bent voor de API rechtstreeks aanroepen:

+ [Aanmelden](how-tos/text-analytics-how-to-signup.md) voor een toegang sleutel en bekijk de stappen voor [de API aanroept](how-tos/text-analytics-how-to-call-api.md).

+ [Quick Start](quickstarts/csharp.md) is een overzicht van de REST-API-aanroepen geschreven in C#. Informatie over het verzenden van tekst, kies een analyse en resultaten met minimale code weergeven.

+ [API-naslagdocumentatie](//go.microsoft.com/fwlink/?LinkID=759346) voorziet in de technische documentatie voor de API's. De documentatie ondersteunt ingesloten aanroepen, zodat u de API vanuit elke pagina met documentatie aanroepen kunt.

+ [Externe & Community-inhoud](text-analytics-resource-external-community.md) geeft een lijst van blogberichten en video's aan te tonen Text Analytics gebruiken met andere hulpprogramma's en -technologieën.

## <a name="see-also"></a>Zie ook

 [Cognitieve Services-documentatie pagina](https://docs.microsoft.com/azure/cognitive-services/)
