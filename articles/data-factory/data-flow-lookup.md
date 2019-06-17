---
title: Azure Data Factory gegevenstransformatie stroom Lookup toewijzen
description: Azure Data Factory gegevenstransformatie stroom Lookup toewijzen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 197f5ba9d6921f4a9921b7074b9e05162d3e37b8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64868128"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Azure Data Factory gegevenstransformatie stroom Lookup toewijzen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gebruik Lookup om toe te voegen referentiegegevens vanaf een andere bron voor uw gegevensstroom. De Lookup-transformatie is een gedefinieerde bron die verwijst naar de verwijzingstabel en komt overeen met op velden voor sleutels vereist.

![Lookup Transformation](media/data-flow/lookup1.png "Lookup")

Selecteer de velden voor sleutels die u wilt tussen de velden van de referentie-bron en de binnenkomende stream velden overeenkomen. U moet eerst hebt gemaakt een nieuwe bron op het canvas ontwerp gegevensstroom gebruiken als de rechterkant voor de zoekopdracht.

Wanneer overeenkomsten worden gevonden, wordt de resulterende rijen en kolommen uit de Verwijzingsbron worden toegevoegd aan de gegevensstroom. U kunt kiezen welke velden van belang dat u wilt opnemen in uw Sink aan het einde van de gegevensstroom.

## <a name="match--no-match"></a>Overeenkomen met / geen overeenkomst

Na uw transformatie opzoeken, kunt u volgende transformaties gebruiken om te controleren van de resultaten van elke rij van de overeenkomst met de functie expressie `isMatch()` meer keuzes in de logica op basis van het al dan niet de zoekopdracht heeft geresulteerd in een rij-overeenkomst of niet maken.

## <a name="optimizations"></a>Optimalisaties

In Data Factory uitvoeren in omgevingen met uitgebreide Spark gegevens stromen. Als uw gegevensset in de geheugenruimte voor worker-knooppunt passen kan, kunnen we uw Lookup-prestaties optimaliseren.

![Lid worden uitgezonden](media/data-flow/broadcast.png "Join Broadcast")

### <a name="broadcast-join"></a>Lid worden van broadcast

Selecteer links en/of rechterkant broadcast join om aan te vragen van ADF kunt u de volledige gegevensset pushen aan beide zijden van de opzoekrelatie in het geheugen.

### <a name="data-partitioning"></a>Gegevenspartitionering

U kunt ook opgeven partitioneren van uw gegevens door te selecteren 'Ingesteld partitioneren' op het tabblad optimaliseren van de Lookup-transformatie te maken van gegevenssets die beter in het geheugen per worker past.

## <a name="next-steps"></a>Volgende stappen

[Deelnemen aan](data-flow-join.md) en [Exists](data-flow-exists.md) transformaties soortgelijke taken uitvoeren in ADF toewijzing gegevens stromen. Kijk eens transformaties volgende.
