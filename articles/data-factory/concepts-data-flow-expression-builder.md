---
title: Azure Data Factory Mapping Data Flow Expression Builder
description: De opbouwfunctie voor Azure Data Factory-toewijzingsgegevens stromen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 9267360394568f0f9259a3c818b21f4e585fd958
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57543726"
---
# <a name="mapping-data-flow-expression-builder"></a>Mapping Data Flow Expression Builder

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

In Azure Data Factory toewijzing gegevensstroom vindt u expressievakken waarin u expressies voor gegevenstransformatie kunt invoeren. Kolommen, velden, variabelen, parameters, functies van de gegevensstroom in deze vakken gebruiken. Gebruik de opbouwfunctie voor expressies, die wordt gestart door te klikken in het tekstvak voor expressie in de transformatie voor het bouwen van de expressie. U ziet ook wel 'Berekende kolom' opties bij het selecteren van kolommen voor transformatie. Wanneer u daarop klikt, ziet u ook de opbouwfunctie voor expressies gestart.

![Opbouwfunctie voor expressies](media/data-flow/expression.png "opbouwfunctie voor expressies")

De opbouwfunctie voor expressies hulpprogramma wordt standaard ingesteld op de optie voor tekst-editor. de functie automatisch aanvullen leest uit de gehele gegevensstroom van Azure Data Factory-objectmodel met syntaxis controleren en te markeren.

![Opbouwfunctie voor expressies automatisch aanvullen](media/data-flow/expb1.png "opbouwfunctie voor expressies automatisch aanvullen")

## <a name="currently-working-on-field"></a>Op dit moment werkt in veld

![Opbouwfunctie voor expressies](media/data-flow/exp3.png "momenteel bezig")

Links boven van de gebruikersinterface van de opbouwfunctie voor expressie, ziet u een veld met de naam 'Momenteel werken aan' met de naam van het veld dat u momenteel werkt. De expressie die u in de gebruikersinterface maakt wordt alleen op die momenteel werken veld worden toegepast. Als u een ander veld transformeren wilt, uw huidige werk op te slaan en deze vervolgkeuzelijst gebruiken voor het selecteren van een ander veld en een expressie voor de andere velden maken.

## <a name="data-preview-in-debug-mode"></a>Voorbeeld van gegevens in de foutopsporingsmodus

![Opbouwfunctie voor expressies](media/data-flow/exp4b.png "Gegevensvoorbeeld-expressie")

Wanneer u op uw expressies werkt, kunt u eventueel overschakelen op foutopsporingsmodus uit het ontwerpgebied gegevensstroom van Azure Data Factory Livevoorbeelden in uitvoering van de resultaten van uw gegevens uit de expressie die u bouwt inschakelen. Realtime live foutopsporing is ingeschakeld voor uw expressies.

![Foutopsporingsmodus](media/data-flow/debugbutton.png "knop foutopsporing")


![Opbouwfunctie voor expressies](media/data-flow/exp5.png "Gegevensvoorbeeld-expressie")

## <a name="comments"></a>Opmerkingen

Opmerkingen toevoegen aan uw expressies met behulp van één regel en de syntaxis van meerdere regels:

![Opmerkingen](media/data-flow/comments.png "opmerkingen")

## <a name="regular-expressions"></a>Reguliere expressies

De taal van de expressie gegevensstroom van Azure Data Factory [volledige naslaginformatie documentatie die hier beschikbaar](https://aka.ms/dataflowexpressions), kunnen functies die reguliere-expressiesyntaxis bevatten. Wanneer u de functies van de reguliere expressie, de opbouwfunctie voor expressies wordt geprobeerd om te interpreteren backslash (\) als een escapereeks-teken. Wanneer u de backslash-tekens in uw reguliere expressie, ofwel de gehele reguliere expressie insluiten in tikken ` ` of gebruik een dubbele backslash.

Voorbeeld met behulp van maatstreepjes

```
regex_replace('100 and 200', `(\d+)`, 'digits')
```

of met behulp van dubbele schuine streep

```
regex_replace('100 and 200', '(\\d+)', 'digits')
```

## <a name="addressing-array-indexes"></a>Matrixindexen-adressering

Gebruiken met de expressie-functies die matrices retourneren, vierkante haken [] om de specifieke indexen binnen dat matrixobject geretourneerde. De matrix is die zijn gebaseerd.

![Expressie Builder matrix](media/data-flow/expb2.png "Gegevensvoorbeeld-expressie")

## <a name="handling-names-with-special-characters"></a>Afhandeling van namen met speciale tekens

Wanneer u de namen van kolommen die speciale tekens of spaties bevatten, plaatst u rond de naam met behulp van accolades.
* ```{[dbo].this_is my complex name$$$}```

## <a name="next-steps"></a>Volgende stappen

[Begin met het maken van data transformation expressies](data-flow-expression-functions.md)
