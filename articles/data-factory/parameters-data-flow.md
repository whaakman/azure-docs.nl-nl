---
title: Parameters voor Azure Data Factory-toewijzing stroom
description: Meer informatie over het voorzien van een gegevensstroom toewijzing van data factory-pijplijnen
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: af5f421cc3802f3a7ad44bb294f5066c32569f8b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082883"
---
# <a name="mapping-data-flow-parameters"></a>Parameters voor flow toewijzing

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Toewijzing van gegevens ondersteunt stromen in data factory het gebruik van parameters. U kunt parameters definiëren in de Stroomdefinitie gegevens die u vervolgens in expressies gebruiken kunt. De parameters kunnen vervolgens worden ingesteld door de aanroepende pijplijn via de activiteit gegevens stromen uitvoeren. Hebt u drie opties om te gebruiken om in te stellen van de waarden in de gegevensstroom activiteit expressies:

* De pijplijn control flow expressietaal gebruiken om in te stellen van een dynamische waarde
* Gebruik de expressietaal voor stroom van gegevens om in te stellen van een dynamische waarde
* Een expressietaal gebruiken om in te stellen van een statische letterlijke waarde

Gebruik deze mogelijkheid om uw gegevensstromen voor algemeen gebruik, flexibel en herbruikbare. U kunt instellingen voor de stroom van gegevens en voor expressies met deze parameters voorzien.

> [!NOTE]
> Voor het gebruik van expressies voor pijplijn besturingselement flow, moet de parameter van de gegevensstroom van het typetekenreeks.

* Een activiteit Execute gegevensstroom toevoegen aan het pijplijncanvas.
* Als uw gegevensstroom parameters heeft, ziet u de lijst met beschikbare parameters in de Parameters tab.* * Klik op het tekstvak in naast elke parameter voor de parameterwaarde invoeren.
* U kunt kiezen om uw parameterexpressie via de pijplijn control flow expressietaal of gegevensstroom expressies te maken.

![Gegevensstroom parameters 3](media/data-flow/params3.png "gegevensstroom parameters 3")

## <a name="create-parameters-in-data-flow"></a>Maak parameters in de gegevensstroom

![Gegevensstroom parameters 1](media/data-flow/params1.png "gegevensstroom parameters 1")

Parameters toevoegen aan uw gegevensstroom, klikt u op het lege gedeelte van het canvas van de gegevensstroom om te zien van de algemene eigenschappen. In het deelvenster instellingen ziet u een tabblad met de naam van de Parameters. Klik op de nieuwe knop voor het genereren van nieuwe parameters, die vervolgens kunnen worden ingesteld vanuit de pipeline, doorgeven van waarden in de gegevensstroom. Voer een parameternaam in en selecteer het gegevenstype voor elke parameter.

Binnen uw expressies stroom van gegevens, kunt u gebruikmaken van de parameters van de waarden instellen vanuit de pipeline. Parameters beginnen met $ en zijn onveranderd. U vindt ook de lijst van de beschikbare parameters in de opbouwfunctie voor expressies op het tabblad Parameters. U kunt deze waarden gebruiken binnen uw expressies, maar u geen nieuwe waarden aan de parameters toewijzen kunt.

![Gegevensstroom parameters 2](media/data-flow/params2.png "gegevensstroom parameters 2")

## <a name="set-data-flow-parameters-from-pipeline"></a>Set parameters voor flow van pijplijn

Nadat u de gegevensstroom met parameters hebt gemaakt, kunt u die gegevensstroom nu uitvoeren van een pijplijn met de activiteit gegevensstroom uitvoeren. Als u deze activiteit aan uw ontwerp pijplijncanvas toevoegt, u krijgt de beschikbare gegevens stroom parameters van de activiteit parameters tabblad instellen.

![Gegevensstroom parameters expressietaal](media/data-flow/params4.png "gegevensstroom parameters expressietaal")

Wanneer u in het tekstvak aan de parameterwaarden invullen klikt, u krijgt de opbouwfunctie voor gegevens stromen. U kunt hier elke expressie of letterlijke waarden die u die overeenkomen met het gegevenstype van de parameter wilt invoeren. Hieronder volgen enkele voorbeelden van gegevensstroom expressie en een letterlijke tekenreeks van de opbouwfunctie voor expressies:

* ```toInteger(Role)```
* ```'this is my static literal string'```

Als het gegevenstype van de parameter een tekenreeks is, kunt klikt u vervolgens u een pijplijn of een expressie van de gegevensstroom op te geven. Als u een pijplijn expressie kiest, wordt u in plaats daarvan met de pijplijn expressiepaneel weergegeven. Zorg ervoor dat u de pijplijn-functies met behulp tekenreeksinterpolatie syntaxis ' @{<expression>}', bijvoorbeeld:

```'@{pipeline().RunId}'```

![Steekproef voor flow parameters](media/data-flow/params5.png "stroom parameters steekproef")

## <a name="next-steps"></a>Volgende stappen

* [Stroomactiviteit gegevens uitvoeren](control-flow-execute-data-flow-activity.md)
* [Besturingselement stroom expressies](control-flow-expression-language-functions.md)
