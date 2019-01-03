---
title: Testen van een Azure Stream Analytics-taak met voorbeeldgegevens
description: In dit artikel wordt beschreven hoe u Azure portal gebruiken voor het testen van een Azure Stream Analytics-taak, een van Voorbeeldinvoer en voorbeeldgegevens uploaden.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: fca76b632e9bcc27ed762886eaea696a5696ad3f
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53557629"
---
# <a name="test-a-stream-analytics-query-with-sample-data"></a>Testen van een Stream Analytics-query met voorbeeldgegevens

U kunt met behulp van Azure Stream Analytics, voorbeeldquery gegevens- en Testscenario's in Azure portal uploaden zonder te starten of stoppen van een taak.

## <a name="upload-sample-data-and-test-the-query"></a>Voorbeeldgegevens uploaden en de query testen

1. Meld u aan bij Azure Portal. 

2. Zoek uw bestaande Stream Analytics-taak en selecteert u deze.

3. Op de Stream Analytics job pagina, onder de **Taaktopologie** kop, selecteer **Query** het Query-editor-venster te openen. 

4. Als u wilt testen van uw query's uitvoeren met voorbeelden van invoergegevens, met de rechtermuisknop op een van uw invoer.  Selecteer vervolgens **voorbeeldgegevens uit het bestand uploaden**. De gegevens moeten worden geserialiseerd in JSON, CSV of AVRO. Van Voorbeeldinvoer moet worden gecodeerd in UTF-8- en niet gecomprimeerd. Alleen de scheidingsteken komma (,) wordt ondersteund voor het testen van CSV-invoer voor de portal.

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
