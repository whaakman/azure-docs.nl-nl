---
title: Taalondersteuning - QnA Maker
titleSuffix: Azure Cognitive Services
description: De taal van een kennisdatabase is van invloed op de QnA Maker kunnen automatische extractie van vragen en antwoorden van bronnen, evenals de relevantie van de resultaten die qna Maker in reactie op query's van gebruikers biedt. Een lijst van cultuur, natuurlijke talen die worden ondersteund door de QnA Maker voor uw knowledge base. Geen verschillende talen in de dezelfde knowledge base.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/21/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 820a9ae0df91fba8cf00764428867bec6196841a
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58335858"
---
# <a name="language-support-for-qna-maker"></a>Taalondersteuning voor QnA Maker

De taal van een kennisdatabase is van invloed op de QnA Maker kunnen automatisch extract vragen en antwoorden uit [bronnen](../Concepts/data-sources-supported.md), evenals de relevantie van de resultaten QnA Maker in reactie op query's van gebruikers biedt.

## <a name="auto-extraction"></a>Automatische extractie
QnA Maker biedt ondersteuning voor vraag/antwoord uitpakken in een willekeurige pagina taal, maar de effectiviteit van de winning is veel hoger zijn voor de volgende talen, zoals QnA Maker trefwoorden gebruikt om te identificeren van vragen.

|Ondersteunde talen| Landinstelling|
|-----|----|
|Nederlands|nl-*|
|Frans|FR-*|
|Italiaans|IT-*|
|Duits|de-*|
|Spaans|ES-*|

## <a name="primary-language-detection"></a>Primaire taaldetectie

De primaire taal die wordt gebruikt voor de detectie is ingesteld voor de QnA Maker-resource en alle knowledge bases gemaakt voor die bron wanneer het eerste document of de URL wordt toegevoegd aan de eerste knowledge base. De taal kan niet worden gewijzigd. 

Als de gebruiker wil bieden ondersteuning voor meerdere talen, moeten ze hebben een nieuwe QnA Maker-resource voor elke taal. Meer informatie over het [maken op basis van taal QnA Maker-kennisdatabase](/how-to/language-knowledge-base.md).  

Controleer of de primaire taal met de volgende stappen uit:

1. Meld u aan bij [Azure Portal](http://portal.azure.com).  
1. Zoek en selecteer de Azure Search-resource gemaakt als onderdeel van uw resource QnA Maker. De naam van de Azure Search-resource begint met dezelfde naam als de QnA Maker-resource en heeft het type **Search-service**. 
1. Uit de **overzicht** pagina van de Search-resource, selecteer **indexen**. 
1. Selecteer de **testkb** index.
1. Selecteer de **velden** tabblad. 
1. Weergave de **Analyzer** kolom voor de **vragen** en **antwoord** velden. 


## <a name="query-matching-and-relevance"></a>Query's die overeenkomen met en relevantie
QnA Maker is afhankelijk van [taalanalyse](https://docs.microsoft.com/rest/api/searchservice/language-support) in Azure search om resultaten te bieden. Speciale opnieuw ranking functies zijn beschikbaar voor nl-* talen waarmee betere relevantie.

De Azure Search-mogelijkheden zijn op par voor ondersteunde talen, heeft QnA Maker een extra kerntechnologie die bevindt zich boven de Azure search-resultaten. In dit model kerntechnologie, gebruiken we enkele speciale functionaliteit voor semantische en word op basis van functies in nl-*, die nog niet beschikbaar voor andere talen. We doen niet deze functies beschikbaar maken, omdat ze deel van de interne werking van de QnA Maker kerntechnologie uitmaken. 

QnA Maker [automatisch detecteert de taal van de knowledge base](#primary-language-detection) tijdens het maken en de analyzer dienovereenkomstig ingesteld. U kunt knowledge bases maken in de volgende talen. 

|Ondersteunde talen|
|-----|
|Arabisch|
|Armeens|
Bengaals|
|Baskisch|
|Bulgaars|
|Catalaans|
|Chinese_Simplified|
|Chinese_Traditional|
|Kroatisch|
|Tsjechisch|
|Deens|
|Nederlands|
|Nederlands|
|Estisch|
|Fins|
|Frans|
|Galicisch|
|Duits|
|Grieks|
|Gujarati|
|Hebreeuws|
|Hindi|
|Hongaars|
|IJslands|
|Indonesisch|
|Iers|
|Italiaans|
|Japans|
|Kanarees|
|Koreaans|
|Lets|
|Litouws|
|Malayalam|
|Maleis|
|Noors|
|Pools|
|Portugees|
|Punjabi|
|Roemeens|
|Russisch|
|Serbian_Cyrillic|
|Serbian_Latin|
|Slowaaks|
|Sloveens|
|Spaans|
|Zweeds|
|Tamil|
|Telugu|
|Thais|
|Turks|
|Oekraïens|
|Urdu|
|Vietnamees|
