---
title: Fouten opsporen in Azure Stream Analytics query's met behulp van SELECT INTO
description: Dit artikel wordt beschreven hoe u voor de steekproef halverwege gegevensquery in Azure Stream Analytics-taak met behulp van een instructie SELECT INTO in de querysyntaxis.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: ccaa6203e4bfe52758e26416646f9152ac5378ea
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30907952"
---
# <a name="debug-queries-by-using-select-into-statements"></a>Fouten opsporen in query's met behulp van een instructie SELECT INTO

In realtime-gegevensverwerking kan weten hoe de gegevens in het midden van de query eruit nuttig zijn. Omdat de invoer- of stappen van een Azure Stream Analytics-taak kunnen meerdere keren worden gelezen, kunt u extra SELECT INTO-instructies schrijven. In dat geval voert tussenliggende gegevens naar de opslag en kunt u controleren of de gegevens juist net zoals *bekijken variabelen* doen wanneer u een programma foutopsporing.

## <a name="use-select-into-to-check-the-data-stream"></a>SELECT INTO gebruiken om te controleren van de gegevensstroom

De volgende voorbeeldquery in een Azure Stream Analytics-taak heeft invoer van één stream, twee verwijzing gegevens invoer en uitvoer naar Azure Table Storage. De query koppelt gegevens van de event hub en twee verwijzing blobs ophalen van de naam en categorie-informatie:

![Voorbeeld van de SELECT INTO-query](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

Houd er rekening mee dat de taak wordt uitgevoerd, maar er zijn geen gebeurtenissen zijn in de uitvoer wordt geproduceerd. Op de **bewaking** tegel, die hier worden weergegeven, kunt u zien dat de invoer is het opstellen van gegevens, maar u niet welke stap van weet de **JOIN** veroorzaakt de gebeurtenissen worden verwijderd.

![De tegel controle](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
In dit geval kunt u een paar extra een instructie SELECT INTO ' Meld ' de tussenliggende JOIN-resultaten en de gegevens die worden gelezen uit de invoer toevoegen.

In dit voorbeeld toegevoegd twee nieuwe 'tijdelijke uitvoer." Ze kunnen worden alle gewenste sink. We gebruiken hier Azure Storage als een voorbeeld:

![Toevoegen van extra SELECT INTO-instructies](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

U kunt vervolgens Herschrijf de query als volgt:

![Herschreven SELECT INTO-query](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

Nu de taak opnieuw starten en laten uitvoeren voor een paar minuten. Vervolgens query temp1 en Tijdelijk2 met Visual Studio Cloud Explorer voor het produceren van de volgende tabellen:

**tabel temp1**
![SELECT INTO temp1 tabel](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)

**tabel Tijdelijk2**
![SELECT INTO Tijdelijk2 tabel](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)

Zoals u ziet, temp1 en Tijdelijk2 gegevens hebt en de kolom name correct in Tijdelijk2 is ingevuld. Echter, omdat er nog geen gegevens in de uitvoer, er is iets mis:

![SELECT INTO output1 tabel zonder gegevens](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

Door middel van de gegevens steekproeven, kunt u zijn bijna zeker weet dat het probleem met de tweede JOIN is. U kunt de referentiegegevens downloaden van de blob en een kijkje nemen:

![SELECT INTO ref-tabel](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

Zoals u ziet de indeling van GUID van de in deze referentiegegevens verschilt van de indeling van de [kolom in Tijdelijk2 uit]. Daarom is de gegevens niet aangekomen in output1 zoals verwacht.

U kunt los van de gegevensindeling, om te verwijzen naar blob en probeer het opnieuw te uploaden:

![SELECT INTO tijdelijke tabel](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

Deze tijd wordt de gegevens in de uitvoer ingedeeld en ingevuld zoals verwacht.

![Selecteer in de laatste tabel](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)


## <a name="get-help"></a>Help opvragen

Voor verdere hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

