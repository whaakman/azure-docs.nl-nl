---
title: Azure Data Factory gegevenstransformatie stroom Pivot toewijzen
description: Azure Data Factory gegevenstransformatie stroom Pivot toewijzen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 86404f4eb2eb2de4243c6bb725f4292e7b560d18
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271852"
---
# <a name="azure-data-factory-mapping-data-flow-pivot-transformation"></a>Azure Data Factory gegevenstransformatie stroom Pivot toewijzen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gebruik Pivot in ADF-gegevensstroom als een aggregatie waarbij een of meer groeperende kolommen zijn afzonderlijke rijwaarden omgezet in afzonderlijke kolommen heeft. U kunt in principe rijwaarden draaien in nieuwe kolommen (gegevens omzetten in metagegevens).

![Opties van draaipunt](media/data-flow/pivot1.png "1 van draaipunt")

## <a name="group-by"></a>Groeperen op

![Opties van draaipunt](media/data-flow/pivot2.png "2 van draaipunt")

Stel eerst de kolommen die u groeperen wilt op voor de aggregatie pivot. U kunt instellen dat meer dan 1 kolom hier met het plusteken naast de lijst met kolommen.

## <a name="pivot-key"></a>Sleutel draaien

![Opties van draaipunt](media/data-flow/pivot3.png "3 van draaipunt")

De Pivot-sleutel is de kolom die ADF uit de rij wordt Schakel over naar de kolom. Elke unieke waarde in de gegevensset voor dit veld wordt standaard Schakel over naar een kolom. U kunt echter (optioneel) de waarden uit de gegevensset die u wilt Schakel over naar kolomwaarden invoeren.

## <a name="pivoted-columns"></a>Gedraaide kolommen

![Opties van draaipunt](media/data-flow/pivot4.png "4 van draaipunt")

Ten slotte, kiest u de aggregatie die u wilt gebruiken voor de gedraaide waarden en weten hoe u de kolommen in de nieuwe projectie van de uitvoer van de transformatie moet worden weergegeven.

(Optioneel) U kunt een specifiek naamgevingspatroon: met een voorvoegsel, midden en achtervoegsel om te worden toegevoegd aan elke nieuwe kolomnaam van de rijwaarden instellen.

Bijvoorbeeld zou 'Verkoop' door 'Regio' draaien leiden tot nieuwe kolomwaarden van elke waarde van de verkopen, dat wil zeggen "25", "50", "1000", etc. Echter, als u een waarde van het aanpassingsvoorvoegsel van 'Verkoop' instellen 

![Opties van draaipunt](media/data-flow/pivot5.png "5 van draaipunt")

Instellen van de rangschikking van de kolom "Normale" samen worden gegroepeerd alle gedraaide kolommen met hun geaggregeerde waarden. Instellen van de kolommen rangschikking aan 'Laterale' wordt schakelen tussen de kolom en waarde.

### <a name="aggregation"></a>Aggregatie

Om in te stellen de aggregatie die u wilt gebruiken voor de pivot-waarden, klikt u op het veld onder aan het deelvenster kolommen gedraaid. Voert u in de gegevensstroom ADF opbouwfunctie voor expressies kunt u een aggregatie van expressie maken en geef een beschrijvende aliasnaam voor uw nieuwe geaggregeerde waarden.

De ADF gegevens Flow expressietaal gebruiken om te beschrijven van de transformaties gedraaide kolom in de opbouwfunctie voor expressies: https://aka.ms/dataflowexpressions.

### <a name="how-to-rejoin-original-fields"></a>Het oorspronkelijke velden weer
> [!NOTE]
> De Pivot-transformatie wordt alleen de kolommen die worden gebruikt in de aggregatie, de groepering en de pivot actie van het project. Als u opnemen van de andere kolommen uit de vorige stap in de stroom wilt, gebruikt u een nieuwe vertakking in de vorige stap en het patroon self-join om de stroom verbinding met de oorspronkelijke metagegevens te gebruiken
