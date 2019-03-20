---
title: Gegevens filteren - aangepaste Translator
titleSuffix: Azure Cognitive Services
description: Wanneer u documenten kunnen worden gebruikt voor het trainen van een aangepast systeem verzendt, is de documenten worden onderworpen aan een reeks verwerken en filteren van de stappen om voor te bereiden voor training.
author: jann-skotdal
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: v-jansko
ms.topic: conceptual
ms.openlocfilehash: 195c39562e36000597d04bb760741ec7f40203c8
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57844670"
---
# <a name="data-filtering"></a>Gegevens filteren

Wanneer u documenten kunnen worden gebruikt voor het trainen van een aangepast systeem verzendt, is de documenten worden onderworpen aan een reeks verwerken en filteren van de stappen om voor te bereiden voor training. Deze stappen worden hier beschreven. Met behulp van de kennis van de filters kunt u inzicht in de zin aantal weergegeven in de aangepaste translator, evenals de stappen die u zelf het voorbereiden van de documenten voor training met aangepaste Translator kan uitvoeren.

## <a name="sentence-alignment"></a>Uitlijning van zinnen
Als het document niet in XLIFF, TMX of uitlijnen-indeling, wordt de zinnen van uw bron- en -documenten met elkaar, per zin door aangepaste Translator uitgelijnd. Translator document uitlijning niet uitvoeren: volgt de naamgeving van de documenten te vinden van de overeenkomende document van de andere taal. In het document, aangepaste Translator gezocht naar de bijbehorende zin in de andere taal. Hierbij document markeringen, zoals ingesloten HTML-codes om te helpen bij de uitlijning.  

Als u een groot verschil tussen het aantal zinnen in de bron ziet en doel aan documenten, het document kan niet zijn parallelle in de eerste plaats, of om andere redenen niet kan worden afgestemd. Het document paren met een groot verschil (> 10%) garandeert een tweede bekijken om te controleren of ze inderdaad parallelle van zinnen op elke zijde. Aangepaste Translator ziet u een waarschuwing naast het document als het aantal zin verdacht gedraagt verschilt.  


## <a name="deduplication"></a>Ontdubbeling
Aangepaste Translator Hiermee verwijdert u de zinnen die aanwezig zijn in de test- en documenten van trainingsgegevens afstemmen. Het verwijderen gebeurt dynamisch binnen de training, niet in de stap gegevensverwerking worden uitgevoerd. Aangepaste Translator rapporteert het aantal zin aan u in het projectoverzicht van het voordat u deze verwijderen.  

## <a name="length-filter"></a>Lengte van filter
* Zinnen met slechts één woord aan beide kanten verwijderen.
* Zinnen met meer dan 100 woorden aan beide kanten verwijderen.  Chinese, Japans, Koreaans, zijn uitgesloten.
* Verwijder zinnen met minder dan 3 tekens. Chinese, Japans, Koreaans, zijn uitgesloten.
* Zinnen met meer dan 2000 tekens voor Chinese, Japans, Koreaans verwijderen.
* Verwijder zinnen met minder dan 1% alfanumerieke tekens.
* Met meer dan 50 woorden dictionary-vermeldingen te verwijderen.

## <a name="white-space"></a>Witruimte
* Vervangen door een reeks spatietekens bevatten, met inbegrip van tabbladen en CR/LF reeksen met een spatie-teken.
* Voorloop-of volgspaties verwijderen uit de zin

## <a name="sentence-end-punctuation"></a>Zin end interpunctie
Meerdere zin end-leestekens vervangen door één exemplaar.  

## <a name="japanese-character-normalization"></a>Japans teken normalisering
Volledige breedte letters en cijfers converteren naar halve breedte tekens.

## <a name="unescaped-xml-tags"></a>Unescaped XML-tags
Transformaties unescaped tags met escape-teken tags filteren:
* `&lt;` wordt `&amp;lt;`
* `&gt;` wordt `&amp;gt;`
* `&amp;` wordt `&amp;amp;`

## <a name="invalid-characters"></a>Ongeldige tekens
Aangepaste Translator verwijdert zinnen die U + FFFD voor Unicode-tekens bevatten. Het teken dat U + FFFD geeft aan dat de conversie van een mislukte codering.

## <a name="next-steps"></a>Volgende stappen

- [Een model te trainen](how-to-train-model.md) in aangepaste Translator.
