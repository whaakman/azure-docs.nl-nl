---
title: Ondersteunde talen - QnA Maker
titlesuffix: Azure Cognitive Services
description: De taal van een kennisdatabase is van invloed op de QnA Maker kunnen automatische extractie van vragen en antwoorden van bronnen, evenals de relevantie van de resultaten die qna Maker in reactie op query's van gebruikers biedt.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: ee04733064ec4e3d131b800fe1f18b27e5127fe8
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45572937"
---
# <a name="supported-languages"></a>Ondersteunde talen

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
