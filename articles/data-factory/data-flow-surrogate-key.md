---
title: Azure Data Factory stroom vervangend sleutels transformatie toewijzen
description: Het gebruik van Azure Data Factory toewijzing Flow Surrogate sleutel gegevenstransformatie voor het genereren van opeenvolgende sleutelwaarden
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: eaa1c577f7e208400d3430222b006e0dbbd7956a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61350431"
---
# <a name="mapping-data-flow-surrogate-key-transformation"></a>Toewijzing van Flow vervangend sleutels gegevenstransformatie

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gebruik de transformatie van de sleutel Surrogate een willekeurige sleutel waarde voor ophogende niet-zakelijke toevoegen aan uw stroom gegevensrijenset. Dit is handig bij het ontwerpen van dimensietabellen in een stervormig schema analytische gegevensmodel waar elk lid in de dimensietabellen moet een unieke sleutel die een niet-zakelijke sleutel, deel uitmaakt van de Kimball-DW-methode.

![Vervangend sleutels transformatie](media/data-flow/surrogate.png "vervangend sleutels transformatie")

'Sleutelkolom' is de naam die u u aan uw nieuwe surrogate-sleutelkolom krijgt.

'Waarde start' is het beginpunt van de incrementele waarde.

## <a name="increment-keys-from-existing-sources"></a>Sleutels van de verhoging van bestaande bronnen

Als u uw takenreeks starten vanaf een waarde die deel uitmaakt van een bron wilt, kunt u een afgeleide kolom transformatie direct na de transformatie surrogaatsleutel gebruiken en de twee waarden samen te voegen:

![SK toevoegen Max](media/data-flow/sk006.png "Surrogate sleutel transformatie toevoegen Max")

De waarde van de sleutel met de vorige max wilt invullen, zijn er twee methoden die u kunt gebruiken:

### <a name="database-sources"></a>Databasebronnen

Gebruik de optie 'Query' MAX() software selecteren uit de gegevensbron met behulp van de transformatie van bron:

![Surrogate Key Query](media/data-flow/sk002.png "Surrogate Key Transformation Query")

### <a name="file-sources"></a>Bestandsbronnen

Als uw vorige maximale waarde in een bestand, kunt u deze kunt gebruiken uw bron-transformatie samen met een statistische transformatie en de functie van de expressie MAX() gebruiken om de vorige maximale waarde:

![Vervangend sleutelbestand](media/data-flow/sk008.png "vervangend sleutelbestand")

In beide gevallen moet u uw nieuwe binnenkomende gegevens samen met de gegevensbron met de vorige maximale waarde toevoegen:

![Vervangend sleutels Join](media/data-flow/sk004.png "vervangend sleutels Join")

## <a name="next-steps"></a>Volgende stappen

Deze voorbeelden gebruiken de [Join](data-flow-join.md) en [afgeleide kolom](data-flow-derived-column.md) transformaties.
