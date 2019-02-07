---
title: Ingebouwde gegevensextractie, natuurlijke taal, beeldverwerking - Azure Search
description: Ophalen van gegevens, natuurlijke taal, afbeeldingsverwerking cognitieve vaardigheden toevoegen semantiek en de structuur aan onbewerkte inhoud in een Azure Search-pijplijn.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: eddbb41f346661e86c64b1dd703321f434c5bbb6
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55770415"
---
# <a name="predefined-skills-for-content-enrichment-azure-search"></a>Vooraf gedefinieerde vaardigheden voor inhoud verrijking (Azure Search)

In dit artikel leert u over de cognitieve vaardigheden met Azure Search is opgegeven. Een *cognitieve vaardigheden* is een bewerking die inhoud op een bepaalde manier worden getransformeerd. Het is vaak het geval is, een onderdeel dat gegevens worden uitgepakt of bepaalt welke structuur en daarom verbetert ons een beeld vormen van de ingevoerde gegevens. De uitvoer is bijna altijd op basis van tekst. Een *vaardigheden* is een verzameling van vaardigheden die de pijplijn verrijking definiëren. 

> [!NOTE]
> Met ingang van 21 december 2018 kunt u een Cognitive Services-resource koppelen aan een vaardighedenset van Azure Search. Hierdoor kunnen we beginnen met het factureren van kosten voor het uitvoeren van vaardighedensets. Vanaf deze datum gaan we ook kosten in rekening brengen voor het extraheren van afbeeldingen als onderdeel van de fase waarin de documenten kunnen worden gekraakt. Het extraheren van tekst uit documenten blijft gratis.
>
> Het uitvoeren van ingebouwde vaardigheden wordt in rekening gebracht tegen de huidige [betalen per gebruik-prijs van Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/). Het extraheren van afbeeldingen wordt tegen de prijs voor een preview in rekening gebracht en wordt beschreven op de [pagina met prijzen voor Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400). [Meer](cognitive-search-attach-cognitive-services.md) informatie.

## <a name="predefined-skills"></a>Vooraf gedefinieerde vaardigheden

Verschillende vaardigheden zijn de flexibele in wat ze gebruiken of produceren. In het algemeen zijn de meeste vaardigheden gebaseerd op vooraf getrainde modellen, wat betekent dat u het model met behulp van uw eigen trainingsgegevens kan geen trainen. De volgende tabel worden opgesomd en beschrijft de vaardigheden die is geleverd door Microsoft. 

| Vaardigheden | Description |
|-------|-------------|
| [Microsoft.Skills.Text.KeyPhraseSkill](cognitive-search-skill-keyphrases.md) | Deze vaardigheid maakt gebruik van een pretrained model voor het detecteren van belangrijke zinnen op basis van de termijn plaatsing, linguïstische regels, nabijheid tot andere voorwaarden en hoe ongebruikelijke de term is binnen de brongegevens. |
| [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)  | Deze kwalificatie maakt gebruik van een pretrained model voor het detecteren van welke taal wordt gebruikt (één taal-ID per document). Wanneer meerdere talen worden gebruikt binnen dezelfde tekst segmenten, is de uitvoer de landinstelling-id van de taal die hoofdzakelijk wordt gebruikt.|
| [Microsoft.Skills.Text.MergeSkill](cognitive-search-skill-textmerger.md) | Consolideert tekst uit een verzameling van velden in één veld.  |
| [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md) | Deze vaardigheid maakt gebruik van een pretrained model tot stand brengen van een vaste set categorieën entiteiten: mensen, locatie, organisatie, e-mails, URL's, datetime-velden. |
| [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)  | Deze vaardigheid maakt gebruik van een pretrained model om positief of negatief gevoel te beoordelen op basis van de door een andere record. De score is tussen 0 en 1. Neutrale scores optreden voor zowel het geval is null wanneer sentiment niet kunnen worden gedetecteerd, en voor de tekst die wordt beschouwd als neutrale.  |
| [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md) | Tekst splitst in pagina's, zodat u kunt verrijken of inhoud incrementeel te verbeteren. |
| [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md) | Deze vaardigheid maakt gebruik van een installatiekopie-detectie-algoritme om te bepalen van de inhoud van een afbeelding en genereren van een beschrijving. |
| [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md) | Optische tekenherkenning. |
| [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md) | Maps-uitvoer naar een complex type (een meerdelige gegevenstype, die kan worden gebruikt voor een volledige naam, een adres meerdere regels of een combinatie van achternaam en een persoonlijke id.) |
| [Microsoft.Skills.Custom.WebApiSkill](cognitive-search-custom-skill-web-api.md) | Uitbreidbaarheid van cognitief zoeken-pijplijn kunt door een HTTP-aanroep in een aangepaste Web-API |


Voor hulp bij het maken van een [aangepaste vaardigheden](cognitive-search-custom-skill-web-api.md), Zie [over het definiëren van een aangepaste interface](cognitive-search-custom-skill-interface.md) en [voorbeeld: het maken van een aangepaste vaardigheden](cognitive-search-create-custom-skill-example.md).

## <a name="see-also"></a>Zie ook

+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Aangepaste vaardigheden interfacedefinitie](cognitive-search-custom-skill-interface.md)
+ [Zelfstudie: Verrijkt indexeren met cognitief zoeken](cognitive-search-tutorial-blob.md)
