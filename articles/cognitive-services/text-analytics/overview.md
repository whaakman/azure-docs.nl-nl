---
title: Overzicht van Text Analytics - Azure Cognitive Services | Microsoft Docs
description: Text Analytics in Azure Cognitive Services voor sentimentanalyse, sleuteltermextractie, taaldetectie en entiteiten koppelen.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 8/30/2018
ms.author: ashmaka
ms.openlocfilehash: 71af2bcbf58279681bbea66d4f76e951a2efce59
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43341483"
---
# <a name="what-is-text-analytics"></a>Wat is de Text Analytics?

De Text Analytics-service biedt geavanceerde voor onbewerkte ongestructureerde tekst voor de verwerking van natuurlijke taal. Het bevat vier hoofdfuncties: sentimentanalyse, sleuteltermextractie, taaldetectie en entiteiten koppelen.

## <a name="analyze-sentiment"></a>Stemming analyseren

[Ontdek](how-tos/text-analytics-how-to-sentiment-analysis.md) hoe klanten denken van uw merk of onderwerp door onbewerkte tekst of er aanwijzingen over positief of negatief gevoel analyseren. Deze API retourneert een gevoelsscore tussen 0 en 1 voor elk document, waarbij 1 het meest positief is.<br />
De analyse-modellen zijn dat met behulp van een uitgebreide hoofdtekst van de tekst en natuurlijke taal technologieën van Microsoft. Voor [talen geselecteerd](text-analytics-supported-languages.md), de API kunt analyseren en beoordelen van de onbewerkte tekst die u opgeeft.

## <a name="extract-key-phrases"></a>Belangrijke woordgroepen herkennen

Automatisch [Extraheer sleuteluitdrukkingen](how-tos/text-analytics-how-to-keyword-extraction.md) snel de hoofdpunten te identificeren. Bijvoorbeeld, krijgt de invoertekst "de voedselvoorzieningsketen delicious is en er waren prachtige personeel", stuurt de Text Analytics-service de belangrijkste gespreksonderwerpen: 'voeding' en "prachtige personeel".

## <a name="detect-language"></a>Taal detecteren

Voor maximaal 120 talen [detecteren](how-tos/text-analytics-how-to-language-detection.md) welke taal de ingevoerde tekst is geschreven en het rapport een code één taal voor elk document dat in de aanvraag is ingediend. De taalcode die is gekoppeld aan een score die wijzen op de sterkte van de score.

## <a name="identify-linked-entities-preview"></a>Identificeren van gekoppelde entiteiten (Preview)

[Identificeer](how-tos/text-analytics-how-to-entity-linking.md) bekende entiteiten in uw tekst en een koppeling naar meer informatie op het web. Entiteiten koppelen herkent en disambiguates wanneer een term die wordt gebruikt als een afzonderlijk verschillende entiteiten, bewerkingen en andere vormen van word.

## <a name="typical-workflow"></a>Standaardwerkstroom

De werkstroom is eenvoudig: voor het verzenden van de gegevens voor analyse en verwerk uitvoer in uw code. Analyse worden gebruikt als-is, zonder extra configuratie of aanpassen.

1. [Meld u](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) voor een [toegangssleutel](how-tos/text-analytics-how-to-access-key.md). De sleutel moet worden doorgegeven bij elke aanvraag.

2. [Maak een aanvraag](how-tos/text-analytics-how-to-call-api.md#json-schema) in JSON die uw gegevens als onbewerkte ongestructureerde tekst bevat.

3. De aanvraag om het eindpunt tot stand gebracht tijdens de registratie te plaatsen, toe te voegen de API die u wilt aanroepen: sentimentanalyse, sleuteltermextractie, taaldetectie of entiteits-id.

4. Stream of het antwoord lokaal opslaan. Resultaten zijn afhankelijk van de aanvraag een gevoelsscore, een verzameling met geëxtraheerde sleuteltermen of een taalcode.

Uitvoer wordt geretourneerd als een enkel JSON-document met de resultaten van elke tekstdocument die u geplaatst, op basis van de ID. U kunt vervolgens analyseren, visualiseren of de resultaten categoriseren in bruikbare inzichten.

Bewerkingen die door de Text Analytics-service zijn staatloos. Gegevens worden niet opgeslagen in uw account.

<a name="data-limits"></a>

## <a name="specifications"></a>Specificaties

### <a name="supported-languages"></a>Ondersteunde talen

Zie [ondersteunde talen in Text Analytics](text-analytics-supported-languages.md).

### <a name="data-limits"></a>Limieten voor gegevens

Alle van de Text Analytics-service-eindpunten accepteert onbewerkte tekstgegevens. De huidige limiet is 5.000 tekens in voor elk document. Als u nodig hebt voor het analyseren van grotere documenten, kunt u deze in kleinere chunks splitst u. Als u nog steeds een hogere limiet nodig hebt [contact met ons opnemen](https://azure.microsoft.com/overview/sales-number/) zodat we uw vereisten.

| Limiet | Waarde |
|------------------------|---------------|
| Maximale grootte van één document | maximaal 5000 tekens wordt gemeten door `String.Length`. |
| Maximale grootte van de gehele aanvraag | 1 MB |
| Maximum aantal documenten in een aanvraag | 1000 documenten |

De limiet is 100 aanroepen per minuut. Houd er rekening mee dat kunt u een groot aantal documenten in één aanroep (maximaal 1000 documenten) indienen.

### <a name="unicode-encoding"></a>Unicode-codering

De Text Analytics-service maakt gebruik van Unicode-codering voor tekstweergave en teken aantal berekeningen. U kunt aanvragen in UTF-8- of UTF-16 met geen meetbare verschillen in het aantal tekens kunt indienen. Als u `String.Length` als u het aantal tekens, gebruikt u dezelfde methode we gebruiken voor het meten van de grootte.

## <a name="next-steps"></a>Volgende stappen

Probeer eerst het [interactieve demo](https://azure.microsoft.com/services/cognitive-services/text-analytics/). U kunt een tekstinvoer (maximaal 5000 tekens) om te bepalen in welke taal (maximaal 120), een gevoelsscore berekenen, Extraheer sleuteluitdrukkingen plakken of gekoppelde entiteiten identificeren. Geen aanmelding is vereist.

Wanneer u klaar bent voor het rechtstreeks aanroepen van de Text Analytics-service:

+ [Meld u](how-tos/text-analytics-how-to-signup.md) voor een sleutel en bekijk de stappen voor [aanroepen van de API](how-tos/text-analytics-how-to-call-api.md).

+ [Snelstartgids](quickstarts/csharp.md) is een overzicht van de REST-API-aanroepen geschreven in C#. Informatie over het verzenden van tekst, kiest u een analyse en resultaten met minimale code te bekijken.

+ [API-referentiedocumentatie](//go.microsoft.com/fwlink/?LinkID=759346) biedt de technische documentatie voor de REST API's. De documentatie biedt ondersteuning voor ingesloten aanroepen, zodat u de API vanuit elke documentatiepagina aanroepen kunt.

+ [Externe & Community-inhoud](text-analytics-resource-external-community.md) bevat een lijst met blogberichten en video's die laten zien hoe het gebruik van Text Analytics met andere hulpprogramma's en technologieën.

## <a name="see-also"></a>Zie ook

 [Pagina met cognitive Services-documentatie](https://docs.microsoft.com/azure/cognitive-services/)
