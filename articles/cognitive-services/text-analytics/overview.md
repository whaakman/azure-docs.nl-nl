---
title: 'Wat is de Tekstanalyse-API? -Mogelijkheden:'
titleSuffix: Azure Cognitive Services
description: Tekstanalyse-API in Azure Cognitive Services voor sentimentanalyse, sleuteltermextractie, taaldetectie en entiteiten koppelen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: overview
ms.date: 03/01/2019
ms.author: aahi
ms.openlocfilehash: f79700d3b57623693bf1d0d62dd9ab171df464fb
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58882508"
---
# <a name="what-is-text-analytics-api"></a>Wat is de Tekstanalyse-API?

De Text Analytics-API is een op de cloud gebaseerde service die geavanceerde verwerking van natuurlijke taal biedt voor onbewerkte tekst. De service bevat vier hoofdfuncties: sentimentanalyse, sleuteltermextractie, taaldetectie en entiteitskoppeling.

De API maakt deel uit van [Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/), een verzameling van machine learning- en AI-algoritmen in de cloud, die kunnen worden gebruikt in uw ontwikkelprojecten.

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Understanding-Text-using-Cognitive-Services/player]

Tekstanalyse kan verschillende dingen betekenen, maar in Cognitive Services, de Tekstanalyse-API biedt vier soorten analyse, zoals hieronder wordt beschreven.

## <a name="sentiment-analysis"></a>Sentimentanalyse
Gebruik [sentimentanalyse](how-tos/text-analytics-how-to-sentiment-analysis.md) om erachter te komen hoe klanten van uw merk of onderwerp denken door onbewerkte tekst of er aanwijzingen over positief of negatief gevoel analyseren. Deze API retourneert een gevoelsscore tussen 0 en 1 voor elk document, waarbij 1 het meest positief is.<br /> De analysemodellen zijn vooraf getraind met behulp van een uitgebreide hoofdtekst en natuurlijke-taaltechnologieën van Microsoft. Voor [geselecteerde talen](text-analytics-supported-languages.md) kan de API elke onbewerkte tekst die u opgeeft analyseren en beoordelen en de resultaten direct doorgeven aan de aanroepende toepassing. U kunt de [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) API of de [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) SDK.

## <a name="key-phrase-extraction"></a>Sleuteltermextractie
Automatisch [Extraheer sleuteluitdrukkingen](how-tos/text-analytics-how-to-keyword-extraction.md) snel de hoofdpunten te identificeren. Bijvoorbeeld, voor de invoertekst 'het eten was heerlijk en de bediening fantastisch' retourneert de API de belangrijkste gespreksonderwerpen: 'eten' en 'bediening fantastisch'. U kunt de [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) hier API of de [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) SDK.

## <a name="language-detection"></a>Taaldetectie
U kunt [detecteren welke taal de ingevoerde tekst is geschreven in](how-tos/text-analytics-how-to-language-detection.md) en rapporteren van een enkele taalcode voor elk document dat is ingediend bij de aanvraag voor maximaal 120 talen. De taalcode is gekoppeld aan een score die de sterkte van de score aangeeft. U kunt de [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) API of de [.NET](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) SDK.

## <a name="entity-recognition-preview"></a>Herkenning van entiteit (Preview)
[Identificeren en categoriseren entiteiten](how-tos/text-analytics-how-to-entity-linking.md) in uw tekst als mensen, plaatsen, organisaties, datum/tijd, hoeveelheden, percentages, valuta's en meer. Bekende entiteiten worden ook herkend en gekoppeld aan meer informatie op het web. U kunt de [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) API.

## <a name="use-containers"></a>Containers gebruiken

[Gebruik de Text Analytics-containers](how-tos/text-analytics-how-to-install-containers.md) om sleuteltermen te extraheren, taal te detecteren, en sentiment lokaal te analyseren, door gestandaardiseerde Docker-containers dichter bij uw gegevens te installeren.

## <a name="typical-workflow"></a>Standaardwerkstroom

De werkstroom is eenvoudig: u dient gegevens in die u wilt analyseren en verwerkt de uitvoer in uw code. Analyseprogramma's worden in de huidige staat gebruikt, zonder extra configuratie of aanpassing.

1. [Registreer](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) u voor een [toegangssleutel](how-tos/text-analytics-how-to-access-key.md). De sleutel moet bij elke aanvraag worden doorgegeven.

2. [Formuleer een aanvraag](how-tos/text-analytics-how-to-call-api.md#json-schema) in JSON, die uw gegevens bevat als onbewerkte tekst.

3. Publiceer de aanvraag aan het eindpunt dat is gemaakt tijdens de registratie, en voeg de gewenste resource toe: sentimentanalyse, sleuteltermextractie, taaldetectie of entiteitsidentificatie.

4. U kunt het antwoord streamen of lokaal opslaan. Afhankelijk van de aanvraag bestaan de resultaten uit een gevoelsscore, een verzameling geëxtraheerde sleuteltermen of een taalcode.

Uitvoer wordt geretourneerd als één JSON-document met de resultaten van elk tekstdocument dat u hebt geplaatst, op basis van de id. U kunt de resultaten vervolgens analyseren, visualiseren of categoriseren in inzichten waarvoor een actie kan worden uitgevoerd.

Gegevens worden niet opgeslagen in uw account. Bewerkingen die door de Text Analytics-API worden uitgevoerd zijn staatloos. Dat betekent dat de tekst die u opgeeft, wordt verwerkt en de resultaten direct worden geretourneerd.

<a name="supported-languages"></a>

## <a name="supported-languages"></a>Ondersteunde talen

Deze sectie is voor betere zichtbaarheid verplaatst naar een afzonderlijk artikel. Raadpleeg [Ondersteunde talen in de Text Analytics-API](text-analytics-supported-languages.md) voor deze inhoud.

<a name="data-limits"></a>

## <a name="data-limits"></a>Gegevenslimieten

Alle eindpunten van de Text Analytics-API accepteren onbewerkte tekstgegevens. De huidige limiet is 5120 tekens per document. Als u grotere documenten moet analyseren, kunt u deze splitsen in een aantal kleinere documenten. Als u toch een hogere limiet nodig hebt, [neem dan contact met ons op](https://azure.microsoft.com/overview/sales-number/) zodat we uw vereisten kunnen bespreken.

| Limiet | Value |
|------------------------|---------------|
| Maximale grootte van één document | 5120 tekens, zoals wordt gemeten door [`StringInfo.LengthInTextElements`](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements). |
| Maximale grootte van de hele aanvraag | 1 MB |
| Maximum aantal documenten in een aanvraag | 1000 documenten |

De limiet is 100 aanroepen per seconde en 1000 aanroepen per minuut. U kunt een groot aantal documenten in één aanroep (maximaal 1000 documenten) indienen.

## <a name="unicode-encoding"></a>Unicode-codering

De Text Analytics-API maakt gebruik van Unicode-codering voor tekstweergave en het berekenen van het aantal tekens. Aanvragen kunnen worden ingediend in UTF-8 en UTF-16, zonder meetbare verschillen in het aantal tekens. Unicode-codepunten worden gebruikt als de heuristiek voor tekenlengte en worden wat de gegevenslimieten voor tekstanalyse betreft als gelijkwaardig beschouwd. Als u [`StringInfo.LengthInTextElements`](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements) gebruikt om het aantal tekens te verkrijgen, gebruikt u dezelfde methode als voor het meten van de gegevensgrootte.

## <a name="next-steps"></a>Volgende stappen

+ [Registreer](how-tos/text-analytics-how-to-signup.md) u voor een toegangssleutel en bekijk de stappen [de API aan te roepen](how-tos/text-analytics-how-to-call-api.md).

+ [Snelstart](quickstarts/csharp.md) is een overzicht van de REST API-aanroepen die zijn geschreven in C#. Informatie over het indienen van tekst, het kiezen van een analyse en het bekijken van de resultaten met minimale code. Als u liever, kunt u beginnen met de [Python-quickstart](quickstarts/python.md) in plaats daarvan.

+ Dieper iets met dit [sentiment-analyse zelfstudie](https://docs.microsoft.com/azure/azure-databricks/databricks-sentiment-analysis-cognitive-services) met behulp van Azure Databricks.

+ Bekijk onze lijst met blogberichten en meer video's over het gebruik van Tekstanalyse-API met andere hulpprogramma's en technologieën in onze [pagina externe en Community-inhoud](text-analytics-resource-external-community.md).
