---
title: Een Azure Stream Analytics-taak met voorbeeldgegevens testen
description: Klik hier voor meer informatie over het testen van uw query's in de Stream Analytics-taken.
keywords: In dit artikel wordt beschreven hoe de Azure portal gebruiken voor het testen van een Azure Stream Analytics-taak, Voorbeeldinvoer en voorbeeldgegevens te uploaden.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
manager: kfile
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/27/2018
ms.openlocfilehash: 3dc9091934f3db8ededc13f74d2f302eccace4d6
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2018
---
# <a name="test-a-stream-analytics-query-with-sample-data"></a>Een Stream Analytics query met voorbeeldgegevens testen

U kunt met behulp van Azure Stream Analytics voorbeeldquery gegevens en test's in de Azure portal uploaden zonder starten of stoppen van een taak.

## <a name="upload-sample-data-and-test-the-query"></a>Uploaden van voorbeeldgegevens en de query testen

1. Meld u aan bij Azure Portal. 

2. Uw bestaande Stream Analytics-taak vinden en selecteren.

3. Taak op de Stream Analytics pagina onder de **taak topologie** kop, selecteer **Query** het venster Query-editor te openen. 

4. Als u wilt testen van uw query met invoer voorbeeldgegevens, met de rechtermuisknop op een van uw invoer.  Selecteer vervolgens **voorbeeldgegevens uit bestand uploaden**.

   De gegevens moeten alleen de gegevens voor de JSON-indeling. Als uw gegevens zich in een andere indeling zoals CSV, moet u deze voordat u uploadt converteren naar JSON. U kunt conversieprogramma BronOpenen zoals [CSV voor het JSON-conversieprogramma](http://www.convertcsv.com/csv-to-json.htm) uw gegevens naar JSON converteren.

    ![query voor stream analytics query-editor testen](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

5. Nadat het uploaden voltooid is, selecteert u **testen** voor het testen van deze query op de voorbeeldgegevens die u hebt opgegeven.

    ![Stream analytics query-editor test voorbeeldgegevens](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

6. Als u de testuitvoer van de voor later gebruik moet, wordt de uitvoer van de query wordt weergegeven in de browser met een koppeling naar de downloadresultaten. 

7. Iteratief pas uw query en test deze opnieuw uit om te zien hoe de uitvoer verandert.

   ![Voorbeelduitvoer van stream Analytics query-editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

   Wanneer u meerdere uitgangen in een query gebruikt, de resultaten worden weergegeven op afzonderlijke tabbladen en u kunt schakelen tussen deze twee.

8. Nadat u de resultaten weergegeven in de browser controleren **opslaan** uw query. Vervolgens **Start** de taak en laat het verwerken van binnenkomende gebeurtenissen.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
