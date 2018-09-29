---
title: Taalondersteuning - QnA Maker
titleSuffix: Azure Cognitive Services
description: Een lijst van natuurlijke talen die worden ondersteund door de QnA Maker.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/25/2018
ms.author: tulasim
ms.openlocfilehash: 1a61d8f4008b0183ab5ddb51332d887217f52f48
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435495"
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
