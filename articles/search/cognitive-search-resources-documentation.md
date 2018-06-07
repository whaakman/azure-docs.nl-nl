---
title: Cognitieve zoeken in documentatie resources (Azure Search) | Microsoft Docs
description: Een aantekeningen lijst met artikelen, zelfstudies, voorbeelden en blog geboekt met betrekking tot cognitieve zoeken workloads in Azure Search.
services: search
manager: cgronlun
author: HeidiSteen
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: heidist
ms.openlocfilehash: cee479b81cb5d8b6cd3306e3735d72c9e64e6045
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640350"
---
# <a name="documentation-resources-for-cognitive-search-workloads"></a>Documentatiebronnen voor werkbelastingen cognitieve zoeken

Cognitieve zoeken is in de openbare preview nu een nieuwe verrijking laag in Azure Search indexeren die latente informatie in niet-tekstwaarden bronnen en deze omzetten in volledige tekst doorzoekbare inhoud in Azure Search niet gedifferentieerde tekst wordt gevonden.

De volgende artikelen zijn de volledige documentatie voor cognitieve zoeken.

## <a name="getting-started"></a>Aan de slag
+ [Wat is cognitieve zoeken?](cognitive-search-concept-intro.md)
+ [Snelstartgids: Probeer cognitieve zoeken in de portal](cognitive-search-quickstart-blob.md)
+ [Zelfstudie: Meer informatie over het cognitieve zoeken-API 's](cognitive-search-tutorial-blob.md)
+ [Voorbeeld: een aangepaste vaardigheid maken](cognitive-search-create-custom-skill-example.md)

## <a name="how-to-guidance"></a>Hoe kan ik richtlijnen
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)
+ [Aantekeningen in een vaardigheden verwijzen naar](cognitive-search-concept-annotations-syntax.md)
+ [Hoe velden toe te wijzen aan een index](cognitive-search-output-field-mapping.md)
+ [Hoe worden verwerkt en gegevens extraheren uit installatiekopieën](cognitive-search-concept-image-scenarios.md)
+ [Het opnieuw opbouwen van een Azure Search-index](search-howto-reindex.md)
+ [Het definiëren van een aangepaste vaardigheden-interface](cognitive-search-custom-skill-interface.md)
+ [Tips voor probleemoplossing](cognitive-search-concept-troubleshooting.md)

## <a name="reference"></a>Referentie

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
  + [Microsoft.Skills.Text.KeyPhraseSkill](cognitive-search-skill-keyphrases.md)
  + [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)
  + [Microsoft.Skills.Text.NamedEntityRecognitionSkill](cognitive-search-skill-named-entity-recognition.md)
  + [Microsoft.Skills.Text.MergerSkill](cognitive-search-skill-textmerger.md)
  + [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md)
  + [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)
  + [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md)
  + [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md)
  + [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md)

+ [Preview REST-API](search-api-2017-11-11-preview.md)
  + [Vaardigheden maken (api-version = 2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
  + [Maak indexeerfunctie (api-version = 2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

## <a name="see-also"></a>Zie ook

+ [Azure Search REST-API](https://docs.microsoft.com/rest/api/searchservice/)
+ [Indexeerfuncties in Azure Search](search-indexer-overview.md)
+ [Wat is Azure Search?](search-what-is-azure-search.md)