---
title: Azure Data Factory-Parameters voor Flow toewijzen
description: Meer informatie over het voorzien van een gegevensstroom toewijzing van data factory-pijplijnen
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: 0a7140f70db78c8511f3c4da00b2f9c11c368163
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477703"
---
# <a name="mapping-data-flow-parameters"></a>Toewijzing van Parameters voor stroom

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Toewijzing van gegevens stromen in Azure Data Factory ondersteunt het gebruik van parameters. U kunt parameters definiëren in de Stroomdefinitie gegevens die u vervolgens in expressies gebruiken kunt. De parameterwaarden kunnen worden ingesteld door de aanroepende pijplijn via de activiteit gegevens stromen uitvoeren. Hebt u drie opties voor het instellen van de waarden in de gegevensstroom activiteit expressies:

* De pijplijn control flow expressietaal gebruiken om in te stellen van een dynamische waarde
* Gebruik de expressietaal voor stroom van gegevens om in te stellen van een dynamische waarde
* Een expressietaal gebruiken om in te stellen van een statische letterlijke waarde

Gebruik deze mogelijkheid om uw gegevensstromen voor algemeen gebruik, flexibel en herbruikbare. U kunt instellingen voor de stroom van gegevens en voor expressies met deze parameters voorzien.

> [!NOTE]
> Voor het gebruik van expressies voor pijplijn besturingselement flow, moet de parameter van de gegevensstroom van het typetekenreeks.

## <a name="create-parameters-in-mapping-data-flow"></a>Maak parameters in de gegevensstroom toewijzen

Parameters toevoegen aan uw gegevensstroom, klikt u op het lege gedeelte van het canvas van de gegevensstroom om te zien van de algemene eigenschappen. In het deelvenster instellingen ziet u een tabblad met de naam 'Parameters'. Klik op de knop 'Nieuw' voor het genereren van een nieuwe parameter. Voor elke parameter, moet u een naam toewijzen, selecteer een type en eventueel een standaardwaarde instelt.

![Maak parameters gegevensstroom](media/data-flow/create-params.png "parameters gegevensstroom maken")

Parameters kunnen worden gebruikt in een expressie van de gegevensstroom. Parameters beginnen met $ en zijn onveranderd. U vindt de lijst met beschikbare parameters in de opbouwfunctie voor expressies op het tabblad 'Parameters'.

![Gegevensstroom parameterexpressie](media/data-flow/parameter-expression.png "gegevensstroom parameterexpressie")

## <a name="use-parameters-in-your-data-flow"></a>Parameters voor gebruik in de gegevensstroom

* Binnen de transformatie-expressies kunt u parameterwaarden. De lijst met parameters onder het tabblad Parameters vindt u in de opbouwfunctie voor expressies. ![Gebruik parameters gegevensstroom](media/data-flow/params9.png "parameters gegevensstroom gebruiken")

* Parameters worden ook gebruikt voor het configureren van dynamische waarden voor de bron en Sink-transformatie-instellingen. Wanneer u op de velden kunnen worden geconfigureerd klikt, ziet u de koppeling 'Dynamische contect toevoegen' wordt weergegeven. Er klikken, gaat u naar een opbouwfunctie voor expressies waar u parameters gebruiken kunt voor het gebruik van dynamische waarden. ![Dynamische inhoud van de gegevensstroom](media/data-flow/params6.png "gegevensstroom dynamische inhoud")

## <a name="set-mapping-data-flow-parameters-from-pipeline"></a>Parameters gegevensstroom toewijzing van de pijplijn instellen

Nadat u de gegevensstroom met parameters hebt gemaakt, kunt u het uitvoeren van een pijplijn met de activiteit gegevensstroom uitvoeren. Nadat u de activiteit toegevoegd aan uw pijplijncanvas, u krijgt de beschikbare gegevens parameters van de stroom in het tabblad voor 'Parameters' van de activiteit.

![Als u een parameter gegevensstroom](media/data-flow/parameter-assign.png "als u een parameter gegevensstroom")

Als uw parametergegevenstype tekenreeks, wanneer u klikt op het tekstvak in op de parameterwaarden instellen, kunt u een pijplijn of een expressie van de gegevensstroom op te geven. Als u een pijplijn expressie kiest, u krijgt de expressiepaneel pijplijn. Zorg ervoor dat u de pijplijn-functies met behulp tekenreeksinterpolatie syntaxis ' @{<expression>}', bijvoorbeeld:

```'@{pipeline().RunId}'```

Als de parameter niet van het typetekenreeks is, wordt er altijd weergegeven met de opbouwfunctie voor gegevens stromen. U kunt hier elke expressie of letterlijke waarden die u wilt dat overeenkomt met het gegevenstype van de parameter invoeren. Hieronder volgen enkele voorbeelden van gegevensstroom expressie en een letterlijke tekenreeks van de opbouwfunctie voor expressies:

* ```toInteger(Role)```
* ```'this is my static literal string'```

Elke gegevensstroom toewijzing kan elke combinatie van de pijplijn en gegevens stroom expressie parameters hebben. 

![Steekproef voor flow parameters](media/data-flow/parameter-example.png "stroom parameters steekproef")



## <a name="next-steps"></a>Volgende stappen
* [Stroomactiviteit gegevens uitvoeren](control-flow-execute-data-flow-activity.md)
* [Besturingselement stroom expressies](control-flow-expression-language-functions.md)
