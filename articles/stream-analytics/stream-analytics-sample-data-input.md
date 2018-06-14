---
title: Een met voorbeeldgegevens in Azure Stream Analytics query testen
description: In dit artikel wordt beschreven hoe een met invoer voorbeeldgegevens in Azure Stream Analytics query testen.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: 305b767ee86de6c8b04fed17514a9092afc2d735
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/12/2018
ms.locfileid: "30961746"
---
# <a name="test-a-query-and-sample-input-in-azure-stream-analytics"></a>Testen van een query en ingevoerd in Azure Stream Analytics-voorbeeld 

U kunt steekproef nemen invoer gebeurtenissen die afkomstig zijn van een bestand en het testen van query's in de portal zonder starten of stoppen van een taak met behulp van Azure Stream Analytics.

## <a name="testing-your-query"></a>Uw query testen

Open in het detailvenster voor de Stream Analytics de **Query-editor** blade door te klikken op de naam van de query onder **Query**. (In ons voorbeeldscenario omdat nog geen query is gemaakt, klikt u op de **combinatie** tijdelijke aanduiding.)

![De Stream Analytics query-editor](media/stream-analytics-sample-data-input/stream-analytics-query-editor.png)

De blade uitgebreide editor voor het maken van een query wordt weergegeven, net als in de vorige release. Nu is de blade met een nieuwe linkerdeelvenster waarin de invoer en uitvoer die worden gebruikt door de query en gedefinieerd voor deze taak bijgewerkt.

![De Stream Analytics query-editor invoer en uitvoer van lijsten](media/stream-analytics-sample-data-input/stream-analytics-query-editor-highlight.png)

Ook wordt weergegeven, zijn een extra invoer en uitvoer, die niet zijn gedefinieerd. Ze afkomstig zijn van de nieuwe querysjabloon die u met start. Ze te wijzigen of zelfs verdwijnen helemaal, zoals bewerken van de query. U kunt deze gewoon negeren nu.

Als u wilt testen met invoer voorbeeldgegevens, met de rechtermuisknop op een van uw invoer en selecteer vervolgens **voorbeeldgegevens uit bestand uploaden**.

![De voorbeeldgegevens van Stream Analytics query-editor uploaden van bestandsopdracht](media/stream-analytics-sample-data-input/stream-analytics-query-editor-upload.png)

Nadat het uploaden voltooid is, klikt u op **testen** voor het testen van deze query op de voorbeeldgegevens die u zojuist hebt opgegeven.

![De knop testen van Stream Analytics query-editor](media/stream-analytics-sample-data-input/stream-analytics-query-editor-test.png)

Als u de test uitvoer voor later gebruik opslaan wilt, wordt de uitvoer van de query weergegeven in de browser met een koppeling naar de downloadresultaten. U kunt nu eenvoudig en iteratief uw query te wijzigen en testen herhaaldelijk om te zien hoe de uitvoer wordt gewijzigd.

![Voorbeelduitvoer van stream Analytics query-editor](media/stream-analytics-sample-data-input/stream-analytics-query-editor-samples-output.png)

In de vorige afbeelding is een tweede uitvoer toegevoegd, aangeroepen **HighAvgTempOutput**.

Wanneer u meerdere uitgangen in een query gebruikt, kunt u de resultaten voor elke uitvoer afzonderlijk zien en schakelen tussen deze twee.

Nadat u tevreden met de resultaten bent, kunt u uw query opslaan, uw taak starten, terug zitten en bekijk de magie van Stream Analytics.

## <a name="get-help"></a>Help opvragen

Voor verdere hulp kunt u proberen de [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
