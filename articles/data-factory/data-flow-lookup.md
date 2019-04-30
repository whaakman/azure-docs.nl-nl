---
title: Azure Data Factory gegevenstransformatie stroom Lookup toewijzen
description: Azure Data Factory gegevenstransformatie stroom Lookup toewijzen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: ef473ea5f88b9108894787785fe1e9083fab1b0a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61348014"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Azure Data Factory gegevenstransformatie stroom Lookup toewijzen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gebruik Lookup om toe te voegen referentiegegevens vanaf een andere bron voor uw gegevensstroom. De Lookup-transformatie is een gedefinieerde bron die verwijst naar de verwijzingstabel en komt overeen met op velden voor sleutels vereist.

![Lookup Transformation](media/data-flow/lookup1.png "Lookup")

Selecteer de velden voor sleutels die u wilt tussen de velden van de referentie-bron en de binnenkomende stream velden overeenkomen. U moet eerst hebt gemaakt een nieuwe bron op het canvas ontwerp gegevensstroom gebruiken als de rechterkant voor de zoekopdracht.

Wanneer overeenkomsten worden gevonden, wordt de resulterende rijen en kolommen uit de Verwijzingsbron worden toegevoegd aan de gegevensstroom. U kunt kiezen welke velden van belang dat u wilt opnemen in uw Sink aan het einde van de gegevensstroom.

## <a name="optimizations"></a>Optimalisaties

In Data Factory uitvoeren in omgevingen met uitgebreide Spark gegevens stromen. Als uw gegevensset in de geheugenruimte voor worker-knooppunt passen kan, kunnen we uw Lookup-prestaties optimaliseren.

![Lid worden uitgezonden](media/data-flow/broadcast.png "Join Broadcast")

### <a name="broadcast-join"></a>Lid worden van broadcast

Selecteer links en/of rechterkant broadcast join om aan te vragen van ADF kunt u de volledige gegevensset pushen aan beide zijden van de opzoekrelatie in het geheugen.

### <a name="data-partitioning"></a>Gegevenspartitionering

U kunt ook opgeven partitioneren van uw gegevens door te selecteren 'Ingesteld partitioneren' op het tabblad optimaliseren van de Lookup-transformatie te maken van gegevenssets die beter in het geheugen per worker past.

## <a name="next-steps"></a>Volgende stappen

[Deelnemen aan](data-flow-join.md) en [Exists](data-flow-exists.md) transformaties soortgelijke taken uitvoeren in ADF toewijzing gegevens stromen. Kijk eens transformaties volgende.
