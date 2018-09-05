---
title: Fouten opsporen in Azure Stream Analytics-query's met behulp van SELECT INTO
description: Dit artikel wordt beschreven hoe u dat er een steekproef halverwege gegevensquery in Azure Stream Analytics-taak met behulp van een instructie SELECT INTO in de query-syntaxis.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: b056d4c29464451d3dc0ef62437f934535820489
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43697989"
---
# <a name="debug-queries-by-using-select-into-statements"></a>Fouten opsporen in query's met behulp van een instructie SELECT INTO

In realtime gegevensverwerking, kan te weten hoe de gegevens eruit ziet in het midden van de query nuttig zijn. Omdat invoer- of stappen van een Azure Stream Analytics-taak kunnen meerdere keren worden gelezen, kunt u extra SELECT INTO-instructies schrijven. In dat geval voert tussenliggende gegevens op te slaan en kunt u de juistheid van de gegevens, net zoals inspecteren *Bekijk variabelen* doen wanneer u een programma foutopsporing.

## <a name="use-select-into-to-check-the-data-stream"></a>SELECT INTO gebruiken om te controleren of de gegevensstroom

De volgende voorbeeldquery in een Azure Stream Analytics-taak heeft een Stroominvoer, twee verwijzen naar gegevensinvoer en uitvoer naar Azure Table Storage. De query lid wordt van gegevens uit de event hub en twee verwijzing blobs om op te halen van de naam en categorie-informatie:

![Voorbeeld van de SELECT INTO-query](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

Houd er rekening mee dat de taak wordt uitgevoerd, maar er zijn geen gebeurtenissen zijn in de uitvoer wordt geproduceerd. Op de **bewaking** tegel, die hier worden weergegeven, kunt u zien dat de invoer is gegevens produceren, maar u niet welke stap van weet de **JOIN** veroorzaakt de gebeurtenissen worden verwijderd.

![De tegel bewaking](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
In dit geval kunt u een paar extra een instructie SELECT INTO ' aan ' de tussenliggende JOIN-resultaten en de gegevens die worden gelezen uit de invoer toevoegen.

In dit voorbeeld hebben we twee nieuwe 'tijdelijke uitvoer.' toegevoegd Ze kunnen een sink die u wilt worden. Hier gebruiken we Azure Storage als een voorbeeld:

![Toevoegen van extra een instructie SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

U kunt vervolgens Herschrijf de query als volgt:

![Herschreven SELECT INTO-query](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

Nu de taak opnieuw starten en laten uitvoeren voor een paar minuten. Vervolgens query temp1 en Tijdelijk2 met Visual Studio Cloud Explorer voor het produceren van de volgende tabellen:

**tabel temp1**
![SELECT INTO temp1-tabel](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)

**tabel Tijdelijk2**
![SELECT INTO Tijdelijk2-tabel](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)

Zoals u ziet, temp1 en Tijdelijk2 gegevens hebt en de naamkolom juist is ingevuld in Tijdelijk2. Echter, omdat er nog steeds geen gegevens in de uitvoer, er is iets mis:

![SELECT INTO output1 tabel zonder gegevens](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

Door middel van de gegevens steekproeven, kunt u zijn bijna zeker weet dat het probleem met de tweede JOIN is. U kunt de referentiegegevens downloaden vanuit de blob en een kijkje nemen:

![SELECT INTO ref-tabel](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

Zoals u zien kunt, de indeling van de GUID in deze verwijzingsgegevens verschilt van de indeling van de [kolom in Tijdelijk2 uit]. Daarom de gegevens niet hebt ontvangen in output1 zoals verwacht.

U kunt de gegevensindeling oplossen, uploaden om te verwijzen naar de blob en probeer het opnieuw:

![SELECT INTO tijdelijke tabel](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

Deze tijd wordt de gegevens in de uitvoer ingedeeld en ingevuld zoals verwacht.

![Selecteer in de laatste tabel](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)


## <a name="get-help"></a>Help opvragen

Voor verdere ondersteuning kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

