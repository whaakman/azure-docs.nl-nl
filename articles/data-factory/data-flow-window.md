---
title: Azure Data Factory gegevenstransformatie stroom venster toewijzen
description: Azure Data Factory gegevenstransformatie stroom venster toewijzen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 329125d39c0da403ff078182af2eee74bcd9c84d
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271733"
---
# <a name="azure-data-factory-mapping-data-flow-window-transformation"></a>Azure Data Factory gegevenstransformatie stroom venster toewijzen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Het venster-transformatie is waar definieert u aggregaties op basis van een venster van de kolommen in uw data-stromen. In de opbouwfunctie voor expressies kunt u verschillende soorten aggregaties die zijn gebaseerd op gegevens- of windows (component SQL OVER) zoals potentiële klanten, vertraging, NTILE, CUMEDIST, positie, enz.). Een nieuw veld wordt gegenereerd in de uitvoer die deze aggregaties bevat. U kunt ook optionele groeperen op velden bevatten.

![Opties voor](media/data-flow/windows1.png "windows 1")

## <a name="over"></a>Meer dan
Stel het partitioneren van kolomgegevens voor de transformatie van het venster. Het SQL-equivalent is de ```Partition By``` in de component Over in SQL. Als u wilt maken van een berekening of maakt u een expressie die moet worden gebruikt voor het partitioneren, kunt u dat doen door de muiswijzer op de naam van de kolom en selecteert u 'berekende kolom'.

![Opties voor](media/data-flow/windows4.png "windows 4")

## <a name="sort"></a>Sorteren
Het instellen van een ander deel van de component Over de ```Order By```. Deze wordt volgorde sorteren van gegevens ingesteld. U kunt ook een expressie voor een waarde berekenen maken in dit kolomveld voor sorteren.

![Opties voor](media/data-flow/windows5.png "windows 5")

## <a name="range-by"></a>Bereik door
Vervolgens stelt u de vensterframe als Unbounded of gebonden. Om een niet-gebonden vensterframe, stelt u de schuifregelaar naar Unbounded aan beide uiteinden. Als u een instelling tussen Unbounded en Current Row kiest, moet u instellen dat de verschuiving van de begindatum en waarden eindigen. Beide waarden worden positieve gehele getallen. U kunt de relatieve getallen of waarden uit uw gegevens gebruiken.

De schuifregelaar venster heeft twee waarden om in te stellen: de waarden voor de huidige rij en de waarden na de huidige rij. De verschuiving van de begin- en komt overeen met de twee selectoren op de schuifregelaar.

![Opties voor](media/data-flow/windows6.png "windows 6")

## <a name="window-columns"></a>Venster kolommen
Gebruik ten slotte de opbouwfunctie voor expressies voor het definiëren van de aggregaties die u wilt gebruiken met de gegevens windows zoals positie, COUNT, MIN, MAX, compacte positie, LEAD, vertraging, enzovoort.

![Opties voor](media/data-flow/windows7.png "windows 7")

De volledige lijst van aggregatie en analytische functies die beschikbaar zijn voor gebruik in de ADF gegevens Flow expressietaal via de opbouwfunctie voor expressies worden hier weergegeven: https://aka.ms/dataflowexpressions.

