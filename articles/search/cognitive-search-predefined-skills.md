---
title: Vooraf gedefinieerde ophalen van gegevens, natuurlijke taal, afbeelding verwerken vaardigheden (Azure Search) | Microsoft Docs
description: Gegevens extraheren natuurlijke taal cognitieve vaardigheden verwerking van afbeeldingen toevoegen semantiek en structuur onbewerkte inhoud in een pijplijn Zoek in Azure.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 870cf9629c7af8faee0ce5709199b64910b27ffb
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="predefined-skills-for-content-enrichment-azure-search"></a>Vooraf gedefinieerde vaardigheden voor inhoud verrijking (Azure Search)

In dit artikel leert u de cognitieve vaardigheden voorzien cognitieve zoeken. Een *cognitieve kwalificatie* is een bewerking die inhoud op een bepaalde manier worden omgezet. Het is vaak het geval is, een onderdeel dat haalt gegevens op of infereert structuur en daarom verbetert onze kennis van de invoergegevens. De uitvoer is bijna altijd op basis van tekst. Een *vaardigheden* is een verzameling van de vaardigheden die de pijplijn verrijking definiëren. 

## <a name="predefined-skills"></a>Vooraf gedefinieerde vaardigheden

Er zijn verschillende vaardigheden flexibele in wat ze gebruiken of produceren. In het algemeen zijn zijn de meeste vaardigheden gebaseerd op vooraf getraind modellen, wat betekent dat u het model met behulp van uw eigen trainingsgegevens kan niet trainen. Zie voor instructies over het maken van een aangepaste kwalificatie [het definiëren van een aangepaste interface](cognitive-search-custom-skill-interface.md) en [voorbeeld: maken van een aangepaste kwalificatie](cognitive-search-create-custom-skill-example.md). De volgende tabel worden opgesomd en beschrijft de vaardigheden die is geleverd door Microsoft. 

| Kwalificatie | Beschrijving |
|-------|-------------|
| [Microsoft.Skills.Text.KeyPhraseSkill](cognitive-search-skill-keyphrases.md) | Deze kwalificatie maakt gebruik van een pretrained voor het detecteren van belangrijke zinnen op basis van de plaatsing van de termijn, taalkundige regels nabijheid tot andere voorwaarden en hoe ongebruikelijke de term is binnen de brongegevens. |
| [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)  | Deze kwalificatie maakt gebruik van een pretrained model voor het detecteren van welke taal wordt gebruikt (één taal-ID per document). Wanneer meerdere talen worden gebruikt binnen dezelfde tekst segmenten, is de uitvoer de LCID van de taal die voornamelijk gebruikt.|
| [Microsoft.Skills.Text.MergerSkill](cognitive-search-skill-textmerger.md) | Consolideert tekst uit een verzameling van velden in één veld.  |
| [Microsoft.Skills.Text.NamedEntityRecognitionSkill](cognitive-search-skill-named-entity-recognition.md) | Deze kwalificatie maakt gebruik van een pretrained tot stand brengen van entiteiten voor een vaste set van categorieën: mensen, de locatie, de organisatie. |
| [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)  | Deze kwalificatie maakt gebruik van een pretrained voor de beoordeling van positieve of negatieve gevoel op basis van door een andere record. De score ligt tussen 0 en 1. Neutrale scores plaatsvinden voor zowel het geval is null wanneer gevoel kan niet worden gedetecteerd en voor de tekst die wordt beschouwd als neutrale.  |
| [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md) | Tekst splitst in pagina's, zodat u kunt aanvullen of inhoud incrementeel verbeteren. |
| [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md) | Deze kwalificatie maakt gebruik van een installatiekopie detectie-algoritme om te bepalen van de inhoud van een installatiekopie en genereren van een beschrijving. |
| [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md) | OCR. |
| [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md) | Maps-uitvoer naar een complex type (een meerdelig gegevenstype, die kan worden gebruikt voor een volledige naam, een meerregelige-adres of een combinatie van achternaam en een persoonlijke id.) |

## <a name="see-also"></a>Zie ook

+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)
+ [Aangepaste vaardigheden interfacedefinitie](cognitive-search-custom-skill-interface.md)
+ [Zelfstudie: Verrijkt met cognitieve search indexeren](cognitive-search-tutorial-blob.md)