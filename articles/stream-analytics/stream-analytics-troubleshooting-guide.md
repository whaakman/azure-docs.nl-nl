---
title: Gids voor probleemoplossing voor Azure Stream Analytics | Microsoft Docs
description: Problemen met uw Stream Analytics-taak oplossen
keywords: problemen met de gids
documentationcenter: 
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
ms.openlocfilehash: dcff312e4a282b15e76ea32aadb1981a496a2446
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-guide-for-azure-stream-analytics"></a>Gids voor probleemoplossing voor Azure Stream Analytics

Azure Stream Analytics probleemoplossing kan worden weergegeven op het eerste gezicht worden van een complexe inspanning. Nadat u werkt met veel gebruikers, hebben wij deze handleiding om te stroomlijnen en verwijdert u meteen over uw invoer, uitvoer, query's en functies.

## <a name="troubleshoot-your-stream-analytics-job"></a>Problemen met uw Stream Analytics-taak oplossen

Gebruik de volgende richtlijnen voor de beste resultaten bij het oplossen van uw Stream Analytics-taak:

1.  Test de verbinding in orde:
    - Controleer de connectiviteit voor de invoer en uitvoer met behulp van de **testverbinding** knop voor elk invoer en uitvoer.

2.  Controleer uw invoer gegevens:
    - Gebruik om te controleren dat invoergegevens Event Hub is binnenkomt, [Service Bus Explorer](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a) verbinding maken met Azure Event Hub (als invoer van de Event Hub wordt gebruikt).  
    - Gebruik de [ **voorbeeldgegevens** ](stream-analytics-sample-data-input.md) knop voor elke invoer en download de voorbeeldgegevens van de invoer.
    - Inspecteer de voorbeeldgegevens voor inzicht in de vorm van de gegevens: het schema en de [gegevenstypen](https://msdn.microsoft.com/library/azure/dn835065.aspx).

3.  De query testen:
    - Op de **Query** tabblad, gebruikt u de **testen** klikken om de query testen en gebruik de gedownloade voorbeeldgegevens naar [de query testen](stream-analytics-test-query.md). Bekijk eventuele fouten en probeert te corrigeren.
    - Als u [ **Timestamp By**](https://msdn.microsoft.com/library/azure/mt573293.aspx), Controleer of de gebeurtenissen hebt tijdstempels groter is dan de [begintijd taak](stream-analytics-out-of-order-and-late-events.md).

4.  Fouten opsporen in uw query:
    - De query geleidelijk van eenvoudige select-instructie voor complexere statistische functies bouwen met behulp van stappen. Gebruik wilt maken van de query-logica stapsgewijze, [WITH](https://msdn.microsoft.com/library/azure/dn835049.aspx) componenten.
    - Gebruik [SELECT INTO](stream-analytics-select-into.md) fouten opsporen in querystappen.

5.  Elimineren verrassingen, zoals:
    - Een [ **waar** ](https://msdn.microsoft.com/library/azure/dn835048.aspx) component in de query alle gebeurtenissen, zo wordt voorkomen dat geen uitvoer gegenereerd uitgefilterd.
    - Wanneer u vensterfuncties gebruikt, wacht u totdat de hele vensterduur om te zien van de uitvoer van de query.
    - De tijdstempel voor gebeurtenissen lager is dan de begintijd van taak en daarom gebeurtenissen worden verwijderd.

6.  Gebruik ordening van gebeurtenis:
    - Als de vorige stappen nog prima, gaat u naar de **instellingen** blade en selecteer [ **gebeurtenis ordening**](stream-analytics-out-of-order-and-late-events.md). Controleren of dit beleid is geconfigureerd voor wat zinvol in uw scenario. Het beleid is *niet* toegepast wanneer u de **testen** knop voor het testen van de query. Dit resultaat is een verschil tussen het in de browser ten opzichte van de taak uitgevoerd in productie testen.

7.  Fouten opsporen in met behulp van de metrische gegevens:
    - Als u geen uitvoer verkrijgt na de verwachte duur (op basis van de query), probeert u het volgende:
        - Bekijk [ **bewaking metrische gegevens** ](stream-analytics-monitoring.md) op de **Monitor** tabblad. Omdat de waarden worden samengevoegd, worden de metrische gegevens die door een paar minuten vertraagd.
            - Als invoervelden > 0, de taak kunnen invoergegevens lezen is. Als invoer gebeurtenissen niet > 0, klikt u vervolgens:
                - Om te zien of de gegevensbron geldige gegevens heeft, controleren met behulp van [Service Bus Explorer](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a). Deze controle van toepassing als de taak Event Hub als invoer.
                - Controleer of de serialisatie-indeling van gegevens en gegevenscodering zijn zoals verwacht.
                - Als de taak van een Event Hub gebruikmaakt, controleert u of de hoofdtekst van het bericht is *Null*.
            - Als gegevens conversiefouten > 0 en moeten klimmen, kan het volgende waar zijn:
                - De taak kan mogelijk niet de gebeurtenissen niet deserialiseren.
                - Het schema van de gebeurtenis mogelijk niet overeen met het schema gedefinieerd of vanwege een verwachte van de gebeurtenissen in de query.
                - De gegevenstypen van een deel van de velden in de gebeurtenis mogelijk niet overeen met verwachtingen.
            - Als runtimefouten > 0, betekent dit dat de taak de gegevens kan ontvangen, maar fouten tijdens het verwerken van de query is gegenereerd.
                - Als u de fouten zoekt, gaat u naar de [controlelogboeken](../azure-resource-manager/resource-group-audit.md) en filtert u op *mislukt* status.
            - Als InputEvents > 0 en OutputEvents = 0, betekent dit een van de volgende is ingesteld op true:
                - Het verwerken van de query heeft geen uitvoergebeurtenissen opgeleverd.
                - Het is mogelijk dat gebeurtenissen of de velden ongeldig is, resulteert in nul uitvoer nadat de verwerking van query's.
                - De taak niet kon om gegevens te pushen naar de [uitvoerlocatie](stream-analytics-select-into.md) omwille van de netwerkverbinding of de verificatie.
        - In alle eerder genoemde foutgevallen uitgelegd operations logboekberichten aanvullende gegevens (inclusief wat er gebeurt), behalve in gevallen waarbij de logica van de query alle gebeurtenissen gefilterd. Als de verwerking van meerdere gebeurtenissen fouten genereert, registreert Stream Analytics de eerste drie foutberichten van hetzelfde type binnen 10 minuten in de Operations-Logboeken. Vervolgens onderdrukt extra identiek fouten met een bericht dat wordt gelezen "Fouten plaatsvinden te snel dat deze worden onderdrukt."

8. Fouten opsporen in met behulp van controle en diagnostische logboeken:
    - Gebruik [controlelogboeken](../azure-resource-manager/resource-group-audit.md), en filteren om te bepalen en fouten opsporen.
    - Gebruik [diagnostische logboeken taak](stream-analytics-job-diagnostic-logs.md) om te bepalen en fouten opsporen.

9. Bekijk de uitvoer:
    - Wanneer de status van de taak is *met*, afhankelijk van de duur die worden bepaald in de query, ziet u de uitvoer in de gegevensbron sink.
    - Als u uitvoer die naar een specifieke uitvoertype niet ziet, hen omleidt naar een uitvoertype die minder complex is, zoals een Azure-Blob. Met behulp van Storage Explorer, controleert u of de uitvoer ziet. Controleer bovendien of bandbreedteregeling limieten op de uitvoer worden voorkomen dat gegevens worden ontvangen.

10. Gegevensoverdracht analyse met taak diagram metrische gegevens gebruiken:
    - Gebruiken om gegevensoverdracht te analyseren en problemen identificeren, de [taak diagram met metrische gegevens](stream-analytics-job-diagram-with-metrics.md).

11. Open een ondersteuningsaanvraag:
    - Ten slotte, als alle anders mislukt, open een Microsoft-ondersteuningsaanvraag met behulp van de abonnements-id met de taak.

## <a name="get-help"></a>Help opvragen

Voor verdere hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
