---
title: Azure Stream Analytics query testen | Microsoft Docs
description: Klik hier voor meer informatie over het testen van uw query's in de Stream Analytics-taken.
keywords: query testen, query oplossen
documentation center: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.openlocfilehash: 2898e3404dcfa3d75e3920f9c83e4efa7201998e
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="test-azure-stream-analytics-queries-in-the-azure-portal"></a>Azure Stream Analytics query's testen in de Azure-portal

U kunt met Azure Stream Analytics query's testen in de Azure portal zonder starten of stoppen van een taak.

## <a name="test-the-input"></a>De invoer testen

1. Als u wilt testen met invoer voorbeeldgegevens, met de rechtermuisknop op een van uw invoer en selecteer vervolgens **voorbeeldgegevens uit bestand uploaden**. Momenteel kunt u alleen de gegevens voor de JSON-indeling uploaden. Als uw gegevens zich in een andere indeling zoals CSV, moet u deze voordat u uploadt converteren naar JSON. U kunt conversieprogramma BronOpenen zoals [CSV voor het JSON-conversieprogramma](http://www.convertcsv.com/csv-to-json.htm) uw gegevens naar JSON converteren.

    ![query voor stream analytics query-editor testen](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. Nadat het uploaden voltooid is, klikt u op **testen** voor het testen van deze query op de voorbeeldgegevens die u hebt opgegeven.

    ![Stream analytics query-editor test voorbeeldgegevens](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

De uitvoer van de query wordt weergegeven in de browser, met resultaten downloadkoppeling moet u de testuitvoer voor later gebruik opslaan. U kunt nu eenvoudig en iteratief uw query te wijzigen en testen herhaaldelijk om te zien hoe de uitvoer wordt gewijzigd.

![Voorbeelduitvoer van stream Analytics query-editor](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

U kunt met meerdere Uitvoerbronnen dat wordt gebruikt in een query, zien de resultaten voor beide uitvoer afzonderlijk en schakelen tussen deze twee.

Nadat u tevreden bent met de resultaten weergegeven in de browser uw query opslaan, start de taak en kunt deze gebeurtenissen zonder fouten verwerken.

## <a name="get-help"></a>Help opvragen

Voor verdere hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
