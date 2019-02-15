---
title: Gegevensstroom van Azure Data Factory toewijzing draaitabel opheffen voor transformatie
description: Gegevensstroom van Azure Data Factory toewijzing draaitabel opheffen voor transformatie
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 14326714fc8258e184024edb83666d3ed0c3eee7
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271722"
---
# <a name="azure-data-factory-mapping-data-flow-unpivot-transformation"></a>Gegevensstroom van Azure Data Factory toewijzing draaitabel opheffen voor transformatie

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gebruik Unpivot in ADF toewijzing gegevensstroom als een manier om te zetten van een niet-genormaliseerde gegevensset in een meer genormaliseerde versie door het uitbreiden van waarden uit meerdere kolommen in een enkel record naar meerdere records met dezelfde waarden in één kolom.

![Draaitabel opheffen voor transformatie](media/data-flow/unpivot1.png "draaitabel opheffen voor opties van 1")

## <a name="ungroup-by"></a>Groep opheffen door

![Draaitabel opheffen voor transformatie](media/data-flow/unpivot5.png "draaitabel opheffen voor opties 2")

Stel eerst de kolommen die u groeperen wilt op voor de aggregatie pivot. Stel een of meer kolommen voor de groepering met het plusteken naast de lijst met kolommen.

## <a name="unpivot-key"></a>Draaitabel opheffen voor sleutel

![Draaitabel opheffen voor transformatie](media/data-flow/unpivot6.png "draaitabel opheffen voor opties 3")

De Pivot-sleutel is de kolom die ADF uit de rij wordt Schakel over naar de kolom. Elke unieke waarde in de gegevensset voor dit veld wordt standaard Schakel over naar een kolom. U kunt echter (optioneel) de waarden uit de gegevensset die u wilt Schakel over naar kolomwaarden invoeren.

## <a name="unpivoted-columns"></a>Draaitabel voor kolommen

![Draaitabel opheffen voor transformatie](media/data-flow//unpivot7.png "draaitabel opheffen voor opties 4")

Kies ten slotte de aggregatie die u wilt gebruiken voor de gedraaide waarden en weten hoe u de kolommen in de nieuwe projectie van de uitvoer van de transformatie moet worden weergegeven.

(Optioneel) U kunt een specifiek naamgevingspatroon: met een voorvoegsel, midden en achtervoegsel om te worden toegevoegd aan elke nieuwe kolomnaam van de rijwaarden instellen.

Bijvoorbeeld, krijgt draaiende 'verkoop' door 'Regio' gewoon u nieuwe kolomwaarden van elke verkoopwaarde. Bijvoorbeeld: "25", "50", "1000", ... Echter, als u een waarde van het aanpassingsvoorvoegsel van 'Verkoop' hebt ingesteld, klikt u vervolgens 'Verkoop' wordt voorafgegaan op de waarden.

<img src="media/data-flow/unpivot3.png" width="400">

Instellen van de rangschikking van de kolom "Normale" samen worden gegroepeerd alle gedraaide kolommen met hun geaggregeerde waarden. Instellen van de kolommen rangschikking aan 'Laterale' wordt schakelen tussen de kolom en waarde.

![Draaitabel opheffen voor transformatie](media/data-flow//unpivot7.png "draaitabel opheffen voor opties 5")

De uiteindelijke draaitabel gegevens resultaat set ziet u de kolomtotalen nu draaitabel zijn in afzonderlijke rijwaarden.
