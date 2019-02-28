---
title: Taalondersteuning - QnA Maker
titleSuffix: Azure Cognitive Services
description: Een lijst van cultuur, natuurlijke talen die worden ondersteund door de QnA Maker voor uw knowledge base. Geen verschillende talen in de dezelfde knowledge base.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/04/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 78ad656648215b0e328c62ecdd9eb0a1eb855a88
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961683"
---
# <a name="language-and-region-support-for-qna-maker"></a>Ondersteuning voor QnA Maker taal en regio

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

## <a name="query-matching-and-relevance"></a>Query's die overeenkomen met en relevantie
QnA Maker is afhankelijk van [taalanalyse](https://docs.microsoft.com/rest/api/searchservice/language-support) in Azure search om resultaten te bieden. Speciale opnieuw ranking functies zijn beschikbaar voor nl-* talen waarmee betere relevantie.

De Azure Search-mogelijkheden zijn op par voor ondersteunde talen, heeft QnA Maker een extra kerntechnologie die bevindt zich boven de Azure search-resultaten. In dit model kerntechnologie we enkele semantische speciale gebruiken en op basis van functies in word in nl-*, die nog niet beschikbaar voor andere talen. We doen niet deze beschikbaar maken, omdat ze deel van de interne werking van de kerntechnologie uitmaken. 

QnA Maker de taal van de knowledge base met een automatisch gedetecteerd tijdens het maken en de analyzer dienovereenkomstig ingesteld. U kunt knowledge bases maken in de volgende talen. Lezen [dit](../How-To/language-knowledge-base.md) voor meer informatie over hoe QnA Maker omgaat met talen.


> [!Tip]
> Taalanalyse, nadat deze is ingesteld, kan niet worden gewijzigd. Ook de taal-analysefunctie is van toepassing op alle knowledge bases in een [QnA Maker-service](../How-To/set-up-qnamaker-service-azure.md). Als u van plan bent om knowledge bases in andere taal, moet u ze in afzonderlijke QnA Maker-services maken.

|Ondersteunde talen|
|-----|
|Arabisch|
|Armeens|, 
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
