---
title: Azure Stream Analytics-query's oplossen
description: Dit artikel wordt beschreven technieken voor het oplossen van uw query's in Azure Stream Analytics-taken.
services: stream-analytics
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/11/2018
ms.openlocfilehash: c437f350e394dc8c264903508a2a5a66fa8225a7
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49346749"
---
# <a name="troubleshoot-azure-stream-analytics-queries"></a>Azure Stream Analytics-query's oplossen

Dit artikel worden veelvoorkomende problemen met het ontwikkelen van Stream Analytics-query's en het oplossen van deze.

## <a name="query-is-not-producing-expected-output"></a>Query is geen verwachte uitvoer produceren. 
1.  Fouten onderzoeken door lokaal te testen:
    - Op de **Query** tabblad **Test**. Gebruik de gedownloade voorbeeldgegevens naar [testen van de query](stream-analytics-test-query.md). Analyseer eventuele fouten en proberen om ze te corrigeren.   
    - U kunt ook [testen van uw query rechtstreeks op live invoer](stream-analytics-live-data-local-testing.md) met Stream Analytics-hulpprogramma's voor Visual Studio.

2.  Als u [ **Timestamp By**](https://msdn.microsoft.com/library/azure/mt573293.aspx), Controleer of de gebeurtenissen tijdstempels die groter is dan de [begintijd taak](stream-analytics-out-of-order-and-late-events.md).

3.  Veelvoorkomende valkuilen, zoals voorkomen:
    - Een [ **waar** ](https://msdn.microsoft.com/library/azure/dn835048.aspx) component in de query is gefilterd van alle gebeurtenissen, zo wordt voorkomen dat geen uitvoer wordt gegenereerd.
    - Een [ **CAST** ](https://msdn.microsoft.com/azure/stream-analytics/reference/cast-azure-stream-analytics) functie mislukt, waardoor de taak is mislukt. Gebruiken om te voorkomen dat type cast fouten, [ **TRY_CAST** ](https://msdn.microsoft.com/azure/stream-analytics/reference/try-cast-azure-stream-analytics) in plaats daarvan.
    - Wanneer u vensterfuncties, wacht u totdat de duur van het gehele venster een uitvoer van de query.
    - De tijdstempel van gebeurtenissen vóór de begintijd van taak en daarom gebeurtenissen worden verwijderd.

4.  Zorg ervoor dat gebeurtenis bestellen beleidsregels zijn geconfigureerd zoals verwacht. Ga naar de **instellingen** en selecteer [ **Gebeurtenisvolgorde**](stream-analytics-out-of-order-and-late-events.md). Het beleid is *niet* toegepast wanneer u de **testen** knop voor het testen van de query. Dit is een verschil tussen het testen in browser ten opzichte van de taak uitgevoerd in de productieomgeving. 

5. Fouten opsporen met behulp van controle en diagnostische logboeken:
    - Gebruik [auditlogboeken](../azure-resource-manager/resource-group-audit.md), en filteren om te bepalen en fouten opsporen.
    - Gebruik [taak diagnostische logboeken](stream-analytics-job-diagnostic-logs.md) om te bepalen en fouten opsporen.

## <a name="job-is-consuming-too-many-streaming-units"></a>Taak is te veel Streaming-eenheden verbruikt
Zorg ervoor dat u profiteren van parallelle uitvoering in Azure Stream Analytics. U leert [schaal met query-parallellisatie](stream-analytics-parallelization.md) van Stream Analytics-taken door invoer partities configureren en afstemmen van de analytics-query definitie.

## <a name="debug-queries-progressively"></a>Fouten opsporen geleidelijk in query 's

In realtime gegevensverwerking, kan te weten hoe de gegevens eruit ziet in het midden van de query nuttig zijn. Omdat invoer- of stappen van een Azure Stream Analytics-taak kunnen meerdere keren worden gelezen, kunt u extra SELECT INTO-instructies schrijven. In dat geval voert tussenliggende gegevens op te slaan en kunt u de juistheid van de gegevens, net zoals inspecteren *Bekijk variabelen* doen wanneer u een programma foutopsporing.

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