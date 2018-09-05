---
title: Gids voor probleemoplossing voor Azure Stream Analytics
description: Dit artikel wordt beschreven technieken voor het oplossen van uw Azure Stream Analytics-taken, verbindingen, invoer, uitvoer, query's en gegevens.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: b1b5d0af3f2b149959bcb97ddaf29ba2fe1f4668
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43702218"
---
# <a name="troubleshooting-guide-for-azure-stream-analytics"></a>Gids voor probleemoplossing voor Azure Stream Analytics

Het oplossen van Azure Stream Analytics kan worden weergegeven op het eerste gezicht moet een complexe inspanning. Nadat u werkt met veel gebruikers, hebben we deze handleiding om u te helpen bij het stroomlijnen en te gissen over uw invoer, uitvoer, query's en functies gemaakt.

## <a name="troubleshoot-your-stream-analytics-job"></a>Problemen met uw Stream Analytics-taak oplossen

Gebruik de volgende richtlijnen voor de beste resultaten bij het oplossen van uw Stream Analytics-taak:

1.  Test de verbinding:
    - Controleer de verbinding met de invoer en uitvoer met behulp van de **testverbinding** knop voor elke invoer en uitvoer.

2.  Controleer de ingevoerde gegevens:
    - Gebruik om te controleren dat de invoergegevens worden doorgestuurd naar Event Hub, [Service Bus Explorer](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a) verbinding maken met Azure Event Hub (indien Event Hub-invoer wordt gebruikt).  
    - Gebruik de [ **voorbeeldgegevens** ](stream-analytics-sample-data-input.md) knop voor elke invoer en download de voorbeeldinvoergegevens.
    - Bekijk de voorbeeldgegevens voor inzicht in de vorm van de gegevens: het schema en de [gegevenstypen](https://msdn.microsoft.com/library/azure/dn835065.aspx).

3.  De query testen:
    - Op de **Query** tabblad, gebruikt u de **testen** knop om de query te testen en gebruik de gedownloade voorbeeldgegevens naar [testen van de query](stream-analytics-test-query.md). Analyseer eventuele fouten en proberen om ze te corrigeren.
    - Als u [ **Timestamp By**](https://msdn.microsoft.com/library/azure/mt573293.aspx), Controleer of de gebeurtenissen tijdstempels die groter is dan de [begintijd taak](stream-analytics-out-of-order-and-late-events.md).

4.  Fouten opsporen in uw query:
    - De query progressief uit eenvoudige select-instructie tot complexere statistische functies met behulp van stappen opnieuw. Als u wilt maken van de query-logische stap voor stap, gebruikt u [WITH](https://msdn.microsoft.com/library/azure/dn835049.aspx) componenten.
    - Gebruik [SELECT INTO](stream-analytics-select-into.md) fouten opsporen in querystappen.

5.  Veelvoorkomende valkuilen, zoals voorkomen:
    - Een [ **waar** ](https://msdn.microsoft.com/library/azure/dn835048.aspx) component in de query is gefilterd van alle gebeurtenissen, zo wordt voorkomen dat geen uitvoer wordt gegenereerd.
    - Een [ **CAST** ](https://msdn.microsoft.com/azure/stream-analytics/reference/cast-azure-stream-analytics) functie mislukt, waardoor de taak is mislukt. Gebruiken om te voorkomen dat type cast fouten, [ **TRY_CAST** ](https://msdn.microsoft.com/azure/stream-analytics/reference/try-cast-azure-stream-analytics) in plaats daarvan.
    - Wanneer u vensterfuncties, wacht u totdat de duur van het gehele venster een uitvoer van de query.
    - De tijdstempel van gebeurtenissen vóór de begintijd van taak en daarom gebeurtenissen worden verwijderd.

6.  Gebruik de Gebeurtenisvolgorde:
    - Als de vorige stappen goed zijn uitgevoerd, gaat u naar de **instellingen** blade en selecteer [ **Gebeurtenisvolgorde**](stream-analytics-out-of-order-and-late-events.md). Controleren of dit beleid is geconfigureerd voor wat zinvol in uw scenario. Het beleid is *niet* toegepast wanneer u de **testen** knop voor het testen van de query. Dit is een verschil tussen het testen in browser ten opzichte van de taak uitgevoerd in de productieomgeving.

7.  Fouten opsporen met behulp van metrische gegevens:
    - Als u na de verwachte duur (op basis van de query) geen uitvoer hebt verkregen, probeert u het volgende:
        - Bekijk [ **metrische bewakingsgegevens** ](stream-analytics-monitoring.md) op de **Monitor** tabblad. Omdat de waarden worden samengevoegd, worden de metrische gegevens die door een paar minuten vertraagd.
            - Als Invoergebeurtenissen > 0, de taak is niet lezen van invoergegevens. Als Invoergebeurtenissen niet > 0 is, klikt u vervolgens:
                - Als u wilt zien of de gegevensbron geldige gegevens bevat, controleren met behulp van [Service Bus Explorer](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a). Deze controle is van toepassing als de taak van Event Hub als invoer gebruikmaakt.
                - Controleer of de gegevens serialisatie-indeling en gegevenscodering zijn zoals verwacht.
                - Als de taak een Event Hub gebruikt wordt, controleert u of de hoofdtekst van het bericht is *Null*.
            - Als Gegevensconversiefouten > 0 en stijgt, kan het volgende waar zijn:
                - De taak kan mogelijk niet de gebeurtenissen niet deserialiseren.
                - Het gebeurtenissenschema mogelijk niet overeen met het opgegeven of verwachte schema van de gebeurtenissen in de query.
                - De gegevenstypen van sommige velden in de gebeurtenis mogelijk niet overeen met de verwachtingen.
            - Als runtimefouten > 0, betekent dit dat de taak de gegevens kan worden ontvangen maar fouten tijdens het verwerken van de query wordt gegenereerd.
                - Als u de fouten zoekt, gaat u naar de [auditlogboeken](../azure-resource-manager/resource-group-audit.md) en filtert u op *mislukt* status.
            - Als invoergebeurtenissen groter is dan > 0 en Uitvoergebeurtenissen = 0, betekent dit dat een van de volgende geldt:
                - Het verwerken van de query heeft geen uitvoergebeurtenissen opgeleverd.
                - Gebeurtenissen of de bijbehorende velden is mogelijk onjuist, wat resulteert in een lege uitvoer na verwerking van query's.
                - De taak is om gegevens te pushen naar de [uitvoerlocatie](stream-analytics-select-into.md) omwille van de verbindings- of -verificatie.
        - In alle hierboven genoemde foutgevallen bewerkingenlogbestandberichten extra details gegeven (waaronder wat er gebeurt), behalve in gevallen waarbij de querylogica alle gebeurtenissen gefilterd. Als de verwerking van meerdere gebeurtenissen fouten genereert, registreert Stream Analytics de eerste drie foutberichten van hetzelfde type binnen 10 minuten Operations-Logboeken. Vervolgens onderdrukt andere identieke fouten met een bericht dat wordt gelezen "Fouten plaatsvinden te snel, dat deze worden onderdrukt."

8. Fouten opsporen met behulp van controle en diagnostische logboeken:
    - Gebruik [auditlogboeken](../azure-resource-manager/resource-group-audit.md), en filteren om te bepalen en fouten opsporen.
    - Gebruik [taak diagnostische logboeken](stream-analytics-job-diagnostic-logs.md) om te bepalen en fouten opsporen.

9. Controleer de uitvoer:
    - Wanneer de taak de status is *met*, afhankelijk van de duur die is bepaald in de query, ziet u de uitvoer in de sink-gegevensbron.
    - Als u uitvoer die een specifiek uitvoertype niet ziet, hen omleidt naar een uitvoertype dat is minder complex, zoals een Azure-Blob. Met behulp van Storage Explorer, controleert u of de uitvoer kan worden gezien. Controleer bovendien of beperking beperkingen aan de uitvoer worden voorkomen dat gegevens worden ontvangen.

10. Analyse van gegevens voor de gegevensstroom met taak diagram metrische gegevens over gebruik:
    - Voor het analyseren van de gegevensstroom en om problemen te identificeren, gebruikt u de [taakdiagram met metrische gegevens over](stream-analytics-job-diagram-with-metrics.md).

11. Een ondersteuningsaanvraag openen:
    - Ten slotte, als niets werkt, opent u een Microsoft-ondersteuningsaanvraag met behulp van de abonnements-id die uw taak bevat.

## <a name="get-help"></a>Help opvragen

Voor verdere ondersteuning kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
