---
title: Testen van een Azure Stream Analytics-taak met voorbeeldgegevens
description: Klik hier voor meer informatie over het testen van uw query's in de Stream Analytics-taken.
keywords: In dit artikel wordt beschreven hoe u Azure portal gebruiken voor het testen van een Azure Stream Analytics-taak, een van Voorbeeldinvoer en voorbeeldgegevens uploaden.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
manager: kfile
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/27/2018
ms.openlocfilehash: d699d69362b2d28c205aab14a4bfb26570a68a4e
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2018
ms.locfileid: "42060141"
---
# <a name="test-a-stream-analytics-query-with-sample-data"></a>Testen van een Stream Analytics-query met voorbeeldgegevens

U kunt met behulp van Azure Stream Analytics, voorbeeldquery gegevens- en Testscenario's in Azure portal uploaden zonder te starten of stoppen van een taak.

## <a name="upload-sample-data-and-test-the-query"></a>Voorbeeldgegevens uploaden en de query testen

1. Meld u aan bij Azure Portal. 

2. Zoek uw bestaande Stream Analytics-taak en selecteert u deze.

3. Op de Stream Analytics job pagina, onder de **Taaktopologie** kop, selecteer **Query** het Query-editor-venster te openen. 

4. Als u wilt testen van uw query's uitvoeren met voorbeelden van invoergegevens, met de rechtermuisknop op een van uw invoer.  Selecteer vervolgens **voorbeeldgegevens uit het bestand uploaden**. De gegevens moeten worden geserialiseerd in JSON, CSV of AVRO.

    ![Stream analytics query-editor testovat dotaz](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

5. Nadat het uploaden voltooid is, selecteert u **testen** voor het testen van deze query op de voorbeeldgegevens die u hebt opgegeven.

    ![Stream analytics-query editor test voorbeeldgegevens](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

6. Als u de testuitvoer voor later gebruik, wordt de uitvoer van de query in de browser met een koppeling naar de downloadresultaten weergegeven. 

7. Iteratief pas uw query en test deze opnieuw uit om te zien hoe de uitvoer verandert.

   ![Voorbeelduitvoer van Stream Analytics query-editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

   Wanneer u meerdere uitvoer in een query, worden de resultaten op een afzonderlijk tabblad weergegeven en u gemakkelijk kunt schakelen tussen beide.

8. Nadat u hebt gecontroleerd dat de resultaten weergegeven in de browser **opslaan** uw query. Vervolgens **Start** de taak en laat het verwerken van de binnenkomende gebeurtenissen.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
