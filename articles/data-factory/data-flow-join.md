---
title: Azure Data Factory stroom Join transformatie
description: Azure Data Factory stroom Join transformatie
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/07/2019
ms.openlocfilehash: 517afe21fbf9241e2b2423525e9caee12a5603f6
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271731"
---
# <a name="azure-data-factory-data-flow-join-transformation"></a>Azure Data Factory stroom Join transformatie

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Join gebruiken om gegevens uit twee tabellen in de gegevensstroom te combineren. Klik op de transformatie die wordt de linker-relatie en wordt een Join-transformatie toevoegen vanuit de werkset. In de Join-transformatie selecteert u een andere stroom van gegevens uit uw gegevensstroom naar de juiste relatie worden.

![Deelnemen aan de transformatie](media/data-flow/join.png "toevoegen")

## <a name="join-types"></a>Typen koppelen

Koppelingstype selecteren is vereist voor de Join-transformatie

### <a name="inner-join"></a>Inner Join

Inner join worden alleen rijen die overeenkomen met de voorwaarden van de kolom uit beide tabellen doorgegeven

### <a name="left-outer"></a>Linker outer join

Alle rijen uit de linker stroom die niet voldoen aan de join-voorwaarde worden doorgegeven via en uitvoerkolommen uit de andere tabel zijn ingesteld op NULL naast alle rijen geretourneerd door de inner join.

### <a name="right-outer"></a>Rechter outer join

Alle rijen uit de juiste stroom die niet voldoen aan de join-voorwaarde worden doorgegeven via en uitvoerkolommen die met de andere tabel overeenkomen zijn ingesteld op NULL, naast alle rijen geretourneerd door de inner join.

### <a name="full-outer"></a>Volledige Outer

Volledig Outer produceert alle kolommen en rijen van beide zijden met NULL-waarden voor kolommen die zijn niet aanwezig in de andere tabel

### <a name="cross-join"></a>Cross Join

Specifieke het vectorproduct van de twee stromen met een expressie

## <a name="specify-join-conditions"></a>Join-voorwaarden opgeven

De Left Join-voorwaarde is van de gegevensstroom verbonden aan de linkerkant van het domein. De Right Join-voorwaarde is de tweede gegevensstroom die is verbonden met uw deelname aan de onderzijde, een directe connector naar een andere stroom of een verwijzing naar een andere stroom is.

U bent verplicht om in te voeren ten minste 1 (1.. n) join-voorwaarden. Ze kunnen worden velden waarnaar hetzij rechtstreeks, hebt geselecteerd in de vervolgkeuzelijst of expressies.

## <a name="join-performance-optimizations"></a>Deelnemen aan Prestatieoptimalisaties

Join in ADF gegevensstroom is in tegenstelling tot samenvoegen Join in hulpprogramma's zoals SSIS, niet een verplichte samenvoegen join-bewerking. De join-sleutels hoeft dus niet moet eerst worden gesorteerd. De Join-bewerking wordt uitgevoerd in Spark met behulp van op basis van de optimale join-bewerking in Spark Databricks: Lid worden uitgezonden / toewijzen aan clientzijde:

![Deelnemen aan de transformatie optimaliseren](media/data-flow/joinoptimize.png "Join-optimalisatie")

Als uw gegevensset in het geheugen van Databricks worker-knooppunt passen, kunnen we uw Join-prestaties optimaliseren. U kunt ook opgeven van de gegevens in de Join-bewerking om te maken van gegevenssets die beter in het geheugen per worker past partitioneren.

## <a name="self-join"></a>Self-join

U kunt voorwaarden in ADF gegevensstroom self-join bereiken met behulp van de optie transformatie tot de alias van een bestaande stroom. Eerst maakt u een 'nieuwe vertakking' via een stroom vervolgens naar de alias gehele oorspronkelijke stream toevoegen, selecteert.

![Self-join](media/data-flow/selfjoin.png "Self-join")

In het bovenstaande diagram, wordt de optie transformatie is aan de bovenkant. Alle Bezig aliasing is de oorspronkelijke stroom naar 'OrigSourceBatting'. In de gemarkeerde Join-transformatie eronder kunt u zien dat we deze stroom Selecteer alias als de rechter join gebruiken, zodat we om te verwijzen naar dezelfde sleutel in de linker & de rechterkant van de Inner Join.
