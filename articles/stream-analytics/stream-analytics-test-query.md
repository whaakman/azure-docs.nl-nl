---
title: Testen van een Azure Stream Analytics-taak met voorbeeldgegevens
description: In dit artikel wordt beschreven hoe u Azure portal gebruiken voor het testen van een Azure Stream Analytics-taak, een van Voorbeeldinvoer en voorbeeldgegevens uploaden.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 6/21/2019
ms.custom: seodec18
ms.openlocfilehash: 1a9caf83c6f4cd4ed15290afc872043c11234552
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508698"
---
# <a name="test-a-stream-analytics-query-with-sample-data"></a>Testen van een Stream Analytics-query met voorbeeldgegevens

U kunt met behulp van Azure Stream Analytics, sample van gegevens uit de invoer of uploaden van voorbeeldgegevens voor het testen van query's in Azure portal zonder te starten of stoppen van een taak.

## <a name="upload-or-sample-data-from-a-live-source-to-test-the-query"></a>Het uploaden of voorbeeldgegevens van een live bron om te testen van de query

1. Meld u aan bij Azure Portal. 

2. Zoek uw bestaande Stream Analytics-taak en selecteert u deze.

3. Op de Stream Analytics job pagina, onder de **Taaktopologie** kop, selecteer **Query** het Query-editor-venster te openen. 

4. Om uw query te testen kunt u gegevens uit een live invoer of uploaden van een bestand vervolgens een steekproef. De gegevens moeten worden geserialiseerd in JSON, CSV of AVRO. Van Voorbeeldinvoer moet worden gecodeerd in UTF-8- en niet gecomprimeerd. Alleen de scheidingsteken komma (,) wordt ondersteund voor het testen van CSV-invoer voor de portal.

    1. Met behulp van live invoer: met de rechtermuisknop op een van uw invoer. Selecteer vervolgens **voorbeeldgegevens van uitvoer**. In het volgende scherm stelt u de duur van het voorbeeld. Gebeurtenissen van een live bron steekproeven worden opgehaald maximaal 1000 gebeurtenissen of 1 MB (afhankelijk van wat het eerste komt), zodat de gegevens een steekproef genomen niet met het opgegeven fulltime-interval overeen mogelijk.

    1. Met behulp van bestand: met de rechtermuisknop op een van uw invoer. Selecteer vervolgens **voorbeeldgegevens uit het bestand uploaden**. 

    ![Stream analytics query-editor testovat dotaz](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

5. Nadat de steekproeven te nemen of het uploaden voltooid is, selecteert u **testen** voor het testen van deze query op de voorbeeldgegevens die u hebt opgegeven.

    ![Stream analytics-query editor test voorbeeldgegevens](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

6. Als u de testuitvoer voor later gebruik, wordt de uitvoer van de query in de browser met een koppeling naar de downloadresultaten weergegeven. 

7. Iteratief pas uw query en test deze opnieuw uit om te zien hoe de uitvoer verandert.

   ![Voorbeelduitvoer van Stream Analytics query-editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

   Wanneer u meerdere uitvoer in een query, worden de resultaten op een afzonderlijk tabblad weergegeven en u gemakkelijk kunt schakelen tussen beide.

8. Nadat u hebt gecontroleerd dat de resultaten weergegeven in de browser **opslaan** uw query. Vervolgens **Start** de taak en laat het verwerken van de binnenkomende gebeurtenissen.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
