---
title: Vooraf gedefinieerde ophalen van gegevens, natuurlijke taal en afbeeldingsverwerking vaardigheden (Azure Search) | Microsoft Docs
description: Ophalen van gegevens, natuurlijke taal, afbeeldingsverwerking cognitieve vaardigheden toevoegen semantiek en de structuur aan onbewerkte inhoud in een pijplijn met een Azure.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: c342eca8f27db713e139c187147abddd80eb854e
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45734531"
---
# <a name="predefined-skills-for-content-enrichment-azure-search"></a>Vooraf gedefinieerde vaardigheden voor inhoud verrijking (Azure Search)

In dit artikel leert u over de cognitieve vaardigheden met Azure Search is opgegeven. Een *cognitieve vaardigheden* is een bewerking die inhoud op een bepaalde manier worden getransformeerd. Het is vaak het geval is, een onderdeel dat gegevens worden uitgepakt of bepaalt welke structuur en daarom verbetert ons een beeld vormen van de ingevoerde gegevens. De uitvoer is bijna altijd op basis van tekst. Een *vaardigheden* is een verzameling van vaardigheden die de pijplijn verrijking definiëren. 

> [!NOTE]
> Cognitief zoeken is een openbare preview. Uitvoering van vaardigheden en uitpakken van de installatiekopie en normalisering worden momenteel gratis aangeboden. Op een later tijdstip, worden de prijzen van deze mogelijkheden aangekondigd. 

## <a name="predefined-skills"></a>Vooraf gedefinieerde vaardigheden

Verschillende vaardigheden zijn de flexibele in wat ze gebruiken of produceren. In het algemeen zijn de meeste vaardigheden gebaseerd op vooraf getrainde modellen, wat betekent dat u het model met behulp van uw eigen trainingsgegevens kan geen trainen. Zie voor instructies over het maken van een aangepaste vaardigheden [over het definiëren van een aangepaste interface](cognitive-search-custom-skill-interface.md) en [voorbeeld: het maken van een aangepaste vaardigheden](cognitive-search-create-custom-skill-example.md). De volgende tabel worden opgesomd en beschrijft de vaardigheden die is geleverd door Microsoft. 

| Vaardigheden | Beschrijving |
|-------|-------------|
| [Microsoft.Skills.Text.KeyPhraseSkill](cognitive-search-skill-keyphrases.md) | Deze vaardigheid maakt gebruik van een pretrained model voor het detecteren van belangrijke zinnen op basis van de termijn plaatsing, linguïstische regels, nabijheid tot andere voorwaarden en hoe ongebruikelijke de term is binnen de brongegevens. |
| [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)  | Deze kwalificatie maakt gebruik van een pretrained model voor het detecteren van welke taal wordt gebruikt (één taal-ID per document). Wanneer meerdere talen worden gebruikt binnen dezelfde tekst segmenten, is de uitvoer de landinstelling-id van de taal die hoofdzakelijk wordt gebruikt.|
| [Microsoft.Skills.Text.MergerSkill](cognitive-search-skill-textmerger.md) | Consolideert tekst uit een verzameling van velden in één veld.  |
| [Microsoft.Skills.Text.NamedEntityRecognitionSkill](cognitive-search-skill-named-entity-recognition.md) | Deze vaardigheid maakt gebruik van een pretrained model tot stand brengen van een vaste set categorieën entiteiten: mensen, locatie, organisatie. |
| [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)  | Deze vaardigheid maakt gebruik van een pretrained model om positief of negatief gevoel te beoordelen op basis van de door een andere record. De score is tussen 0 en 1. Neutrale scores optreden voor zowel het geval is null wanneer sentiment niet kunnen worden gedetecteerd, en voor de tekst die wordt beschouwd als neutrale.  |
| [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md) | Tekst splitst in pagina's, zodat u kunt verrijken of inhoud incrementeel te verbeteren. |
| [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md) | Deze vaardigheid maakt gebruik van een installatiekopie-detectie-algoritme om te bepalen van de inhoud van een afbeelding en genereren van een beschrijving. |
| [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md) | Optische tekenherkenning. |
| [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md) | Maps-uitvoer naar een complex type (een meerdelige gegevenstype, die kan worden gebruikt voor een volledige naam, een adres meerdere regels of een combinatie van achternaam en een persoonlijke id.) |

## <a name="see-also"></a>Zie ook

+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Aangepaste vaardigheden interfacedefinitie](cognitive-search-custom-skill-interface.md)
+ [Zelfstudie: Verrijkt indexeren met cognitief zoeken](cognitive-search-tutorial-blob.md)