---
title: Testen van uw Azure Stream Analytics-taak met voorbeeldgegevens | Microsoft Docs
description: Klik hier voor meer informatie over het testen van uw query's in de Stream Analytics-taken.
keywords: testen van een taak, invoer steekproeven, voorbeeld datum uploaden
documentationcenter: ''
services: stream-analytics
author: SnehaGunda
manager: kfile
ms.assetid: ''
ms.service: stream-analytics
ms.workload: data-services
ms.date: 03/18/2018
ms.author: sngun
ms.openlocfilehash: c026a91fff5b8ef5774993b335f8d61877aa5d39
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="test-your-stream-analytics-query-with-sample-data"></a>De Stream Analytics query met voorbeeldgegevens testen

U kunt met behulp van Azure Stream Analytics voorbeeldquery gegevens en test's in de portal uploaden zonder starten of stoppen van een taak.

## <a name="upload-sample-data-and-test-the-query"></a>Uploaden van voorbeeldgegevens en de query testen

1. Navigeer naar een van uw bestaande Stream Analytics-taak > Klik op **Query** het venster Query-editor te openen. 

2. Als u wilt testen van uw query met invoer voorbeeldgegevens, met de rechtermuisknop op een van uw invoer en selecteer vervolgens **voorbeeldgegevens uit bestand uploaden**. Momenteel kunt u alleen de gegevens voor de JSON-indeling uploaden. Als uw gegevens zich in een andere indeling zoals CSV, moet u deze voordat u uploadt converteren naar JSON. U kunt conversieprogramma BronOpenen zoals [CSV voor het JSON-conversieprogramma](http://www.convertcsv.com/csv-to-json.htm) uw gegevens naar JSON converteren.

    ![query voor stream analytics query-editor testen](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

3. Nadat het uploaden voltooid is, klikt u op **testen** voor het testen van deze query op de voorbeeldgegevens die u hebt opgegeven.

    ![Stream analytics query-editor test voorbeeldgegevens](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

4. Als u de test uitvoer voor later gebruik opslaan wilt, wordt de uitvoer van de query weergegeven in de browser met een koppeling naar de downloadresultaten. U kunt nu eenvoudig en iteratief uw query te wijzigen en testen herhaaldelijk om te zien hoe de uitvoer wordt gewijzigd.

   ![Voorbeelduitvoer van stream Analytics query-editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

Wanneer u meerdere uitgangen in een query gebruikt, kunt u de resultaten voor elke uitvoer afzonderlijk zien en schakelen tussen deze twee. Nadat u de resultaten weergegeven in de browser, kunt u uw query opslaan, start de taak en kunnen controleren of deze gebeurtenissen zonder fouten verwerken.
Voor verdere hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
