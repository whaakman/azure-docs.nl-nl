---
title: Ondersteunde talen - QnA Maker - Azure cognitieve Services | Microsoft Docs
description: Meer informatie over welke talen worden ondersteund voor QnA Maker.
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: b210f59129a962046787b27d003c2872a54f6c8e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345593"
---
# <a name="supported-languages"></a>Ondersteunde talen

De taal van een knowledge base heeft gevolgen voor de mogelijkheid van QnA Maker automatisch extract vragen en antwoorden uit [bronnen](../Concepts/data-sources-supported.md), evenals de relevantie van de resultaten QnA Maker in reactie op gebruiker query's biedt.

## <a name="auto-extraction"></a>Automatische uitpakken
QnA Maker ondersteunt uitpakken van de vraag/antwoord op een pagina taal, maar de effectiviteit van de extractie is veel hoger voor de volgende talen als QnA Maker trefwoorden gebruikt om u te identificeren vragen.

|Ondersteunde talen| Landinstelling|
|-----|----|
|Engels|nl-*|
|Frans|FR-*|
|Italiaans|IT-*|
|Duits|de-*|
|Spaans|ES-*|

## <a name="query-matching-and-relevance"></a>Query overeenkomende en relevantie
Afhankelijk van de QnA Maker [taalanalyse](https://docs.microsoft.com/en-us/rest/api/searchservice/language-support) in Azure search voor het ontwikkelen van resultaten. Speciale opnieuw ranking functies zijn beschikbaar voor nl-* talen die betere relevantie inschakelen.

QnA Maker tijdens het maken van automatisch worden gedetecteerd voor de taal van de knowledge base en stelt de analyzer dienovereenkomstig. U kunt kennis basissen maken in de volgende talen. Lees [dit](../How-To/language-knowledge-base.md) voor meer informatie over hoe QnA Maker omgaat met talen.


> [!Tip]
> Taalanalyse, nadat deze is ingesteld, kan niet worden gewijzigd. Ook de taalanalyse is van toepassing op alle kennis-databases in een [QnA Maker service](../How-To/set-up-qnamaker-service-azure.md). Als u van plan bent om kennis basissen in andere taal, moet u ze onder afzonderlijke QnA Maker-services maken.

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
|Engels|
|Ests|
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
|Kannada|
|Koreaans|
|Lets|
|Litouws|
|Malayalam|
|Maleisisch|
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
|Thai|
|Turks|
|Oekraïens|
|Oerdoe|
|Vietnamees|
