---
title: Documentatie voor cognitief zoeken (Azure Search) | Microsoft Docs
description: Een lijst met aantekeningen van artikelen, zelfstudies, voorbeelden en blog post met betrekking tot cognitive search workloads in Azure Search.
services: search
manager: cgronlun
author: HeidiSteen
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: heidist
ms.openlocfilehash: bad73d2bf8bdf4adee4ce6e40550e506a35a2083
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49165663"
---
# <a name="documentation-resources-for-cognitive-search-workloads"></a>Documentatie voor cognitief zoeken-workloads

Cognitief zoeken, nu is in openbare preview, een nieuwe verrijking-laag in Azure Search indexeren die latente informatie gevonden in niet-tekstuele bronnen en niet-gedifferentieerde tekst omzetten ervan in volledige tekst doorzoekbare inhoud in Azure Search.

De volgende artikelen zijn de volledige documentatie voor cognitief zoeken.

## <a name="getting-started"></a>Aan de slag
+ [Wat is cognitive search?](cognitive-search-concept-intro.md)
+ [Snelstartgids: Probeer cognitief zoeken in de portal](cognitive-search-quickstart-blob.md)
+ [Zelfstudie: Informatie over de cognitief zoeken-API 's](cognitive-search-tutorial-blob.md)
+ [Voorbeeld: een aangepaste vaardigheid maken](cognitive-search-create-custom-skill-example.md)

## <a name="how-to-guidance"></a>Uitleg
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Hoe om te verwijzen naar aantekeningen in een set vaardigheden](cognitive-search-concept-annotations-syntax.md)
+ [Velden toewijzen aan een index](cognitive-search-output-field-mapping.md)
+ [Hoe worden verwerkt en extraheer informatie uit afbeeldingen](cognitive-search-concept-image-scenarios.md)
+ [Het opnieuw opbouwen van een Azure Search-index](search-howto-reindex.md)
+ [Over het definiëren van een interface voor aangepaste vaardigheden](cognitive-search-custom-skill-interface.md)
+ [Tips voor probleemoplossing](cognitive-search-concept-troubleshooting.md)

## <a name="reference"></a>Referentie

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
  + [Microsoft.Skills.Text.KeyPhraseSkill](cognitive-search-skill-keyphrases.md)
  + [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)
  + [Microsoft.Skills.Text.NamedEntityRecognitionSkill](cognitive-search-skill-named-entity-recognition.md)
  + [Microsoft.Skills.Text.MergeSkill](cognitive-search-skill-textmerger.md)
  + [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md)
  + [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)
  + [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md)
  + [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md)
  + [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md)

+ [Preview-REST-API](search-api-2017-11-11-preview.md)
  + [Vaardighedenset maken (api-version = 2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
  + [Indexeerfunctie maken (api-version = 2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

## <a name="see-also"></a>Zie ook

+ [Azure Search REST-API](https://docs.microsoft.com/rest/api/searchservice/)
+ [Indexeerfuncties in Azure Search](search-indexer-overview.md)
+ [Wat is Azure Search?](search-what-is-azure-search.md)
