---
title: Bewerkingen, gebeurtenissen en prestatie meter items bewaken voor open bare basis Load Balancer
titlesuffix: Azure Load Balancer
description: Meer informatie over het inschakelen van waarschuwings gebeurtenissen en het controleren van de status logboek registratie voor open bare basis Load Balancer
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/10/2018
ms.author: allensu
ms.openlocfilehash: 1995ad5e8179fdee11e960c2ad0e7c03602ebd31
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68274805"
---
# <a name="azure-monitor-logs-for-public-basic-load-balancer"></a>Azure Monitor logboeken voor open bare basis Load Balancer

>[!IMPORTANT] 
>Azure Load Balancer ondersteunt twee verschillende typen: Basic en Standard. In dit artikel wordt Basic Load Balancer beschreven. Voor meer informatie over Standard Load Balancer raadpleegt u [Standard Load Balancer Overview](load-balancer-standard-overview.md) , waarmee u telemetrie kunt weer geven via multidimensionale metrische gegevens in azure monitor.

U kunt verschillende typen logboeken in azure gebruiken om Basic load balancers te beheren en problemen op te lossen. Sommige van deze logboeken kunnen worden geopend via de portal. Alle logboeken kunnen worden geëxtraheerd uit Azure Blob-opslag en worden weer gegeven in verschillende hulpprogram ma's, zoals Excel en PowerBI. In de onderstaande lijst vindt u meer informatie over de verschillende typen logboeken.

* **Audit logboeken:** U kunt [Azure-controle logboeken](../monitoring-and-diagnostics/insights-debugging-with-events.md) (voorheen ook wel operationele logboeken genoemd) gebruiken om alle bewerkingen weer te geven die worden verzonden naar uw Azure-abonnement (en) en hun status. Audit logboeken zijn standaard ingeschakeld en kunnen worden weer gegeven in de Azure Portal.
* **Gebeurtenis logboeken voor waarschuwingen:** U kunt dit logboek gebruiken om waarschuwingen weer te geven die zijn gegenereerd door de load balancer. De status voor de load balancer wordt elke vijf minuten verzameld. Dit logboek wordt alleen geschreven als er een load balancer waarschuwings gebeurtenis optreedt.
* **Health probe-logboeken:** U kunt dit logboek gebruiken om problemen weer te geven die zijn gedetecteerd door uw Health Probe, zoals het aantal exemplaren in uw back-end-pool die geen aanvragen ontvangen van de load balancer als gevolg van fouten in de status test. In dit logboek wordt geschreven wanneer er een wijziging is in de status van de status test.

> [!IMPORTANT]
> Azure Monitor-logboeken werken momenteel alleen voor open bare Basic load balancers. Logboeken zijn alleen beschikbaar voor resources die zijn geïmplementeerd in het Resource Manager-implementatie model. U kunt geen Logboeken gebruiken voor bronnen in het klassieke implementatie model. Zie [Resource Manager-implementatie en klassieke implementatie](../azure-resource-manager/resource-manager-deployment-model.md)voor meer informatie over de implementatie modellen.

## <a name="enable-logging"></a>Logboekregistratie inschakelen

Controle logboek registratie wordt automatisch ingeschakeld voor elke resource manager-resource. U moet logboek registratie voor gebeurtenis-en status controle inschakelen om te beginnen met het verzamelen van de gegevens die beschikbaar zijn via deze logboeken. Gebruik de volgende stappen om logboek registratie in te scha kelen.

Meld u aan bij de [Azure Portal](https://portal.azure.com). Als u nog geen load balancer hebt, [maakt u een Load Balancer](load-balancer-get-started-internet-arm-ps.md) voordat u doorgaat.

1. Klik in de portal op **Bladeren**.
2. Selecteer **load balancers**.

    ![Portal-taak verdeler](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Selecteer een bestaande load balancer > > **alle instellingen**.
4. Ga aan de rechter kant van het dialoog venster onder de naam van de load balancer naar **bewaking**en klik op **Diagnostische gegevens**.

    ![Portal-Load Balancer-instellingen](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. Selecteer in het deel venster **diagnostiek** onder **status**de optie **aan**.
6. Klik op **opslag account**.
7. Onder **Logboeken**selecteert u een bestaand opslag account of maakt u een nieuwe. Gebruik de schuif regelaar om te bepalen hoeveel dagen van gebeurtenis gegevens worden opgeslagen in de gebeurtenis Logboeken. 
8. Klik op **Opslaan**.

Diagnostische gegevens worden opgeslagen in Table Storage in het opgegeven opslag account. Als er geen logboeken worden opgeslagen, is dat niet het geval, omdat er geen relevante logboeken worden gemaakt.

![Portal: Diagnostische logboeken](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

> [!NOTE]
> Voor audit Logboeken is geen afzonderlijk opslag account vereist. Het gebruik van opslag voor gebeurtenis-en status test logboek registratie brengt service kosten met zich mee.

## <a name="audit-log"></a>Audit logboek

Het controle logboek wordt standaard gegenereerd. De logboeken blijven 90 dagen bewaard in de gebeurtenis logboeken van Azure. Lees het artikel [gebeurtenissen en controle logboeken weer geven](../monitoring-and-diagnostics/insights-debugging-with-events.md) voor meer informatie over deze logboeken.

## <a name="alert-event-log"></a>Gebeurtenis logboek voor waarschuwingen

Dit logboek wordt alleen gegenereerd als u het hebt ingeschakeld op basis van een per load balancer. De gebeurtenissen worden vastgelegd in de JSON-indeling en opgeslagen in het opslag account dat u hebt opgegeven tijdens het inschakelen van de logboek registratie. Hier volgt een voor beeld van een gebeurtenis.

```json
{
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }
}
```

De JSON-uitvoer toont de eigenschap *eventname* , waarmee wordt beschreven waarom de Load Balancer een waarschuwing heeft gemaakt. In dit geval is de gegenereerde waarschuwing als gevolg van de TCP-poort ontstaat veroorzaakt door IP NAT-limieten (SNAT) van de bron.

## <a name="health-probe-log"></a>Status test logboek

Dit logboek wordt alleen gegenereerd als u het hebt ingeschakeld op een per load balancer, zoals hierboven is beschreven. De gegevens worden opgeslagen in het opslag account dat u hebt opgegeven tijdens het inschakelen van de logboek registratie. Er wordt een container met de naam ' Insights-logs-loadbalancerprobehealthstatus ' gemaakt en de volgende gegevens worden geregistreerd:

```json
{
    "records":[
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 1,
            "healthPercentage": 50.000000
        }
    },
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 0,
            "healthPercentage": 100.000000
        }
    }]
}
```

De JSON-uitvoer wordt weer gegeven in het veld eigenschappen de basis informatie voor de test status. De eigenschap *dipDownCount* toont het totale aantal exemplaren op de back-end die geen netwerk verkeer ontvangen wegens mislukte test reacties.

## <a name="view-and-analyze-the-audit-log"></a>Het audit logboek weer geven en analyseren

U kunt met behulp van de volgende methoden controle logboek gegevens weer geven en analyseren:

* **Azure-hulpprogram ma's:** Gegevens uit de audit logboeken ophalen via Azure PowerShell, de Azure-opdracht regel interface (CLI), de Azure-REST API of de Azure preview-Portal. Stapsgewijze instructies voor elke methode worden beschreven in het artikel [controle bewerkingen met Resource Manager](../azure-resource-manager/resource-group-audit.md) .
* **Power BI:** Als u nog geen [Power bi](https://powerbi.microsoft.com/pricing) account hebt, kunt u het gratis uitproberen. Met het [Azure audit logs-inhouds pakket voor Power bi](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs)kunt u uw gegevens analyseren met vooraf geconfigureerde Dash boards, of u kunt weer gaven aanpassen aan uw vereisten.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>De status test en het gebeurtenis logboek weer geven en analyseren

U moet verbinding maken met uw opslag account en de JSON-logboek vermeldingen ophalen voor gebeurtenis-en status test Logboeken. Wanneer u de JSON-bestanden hebt gedownload, kunt u deze converteren naar CSV en weer geven in Excel, PowerBI of een ander hulp programma voor gegevens visualisatie.

> [!TIP]
> Als u bekend bent met Visual Studio en de basisconcepten van het wijzigen van waarden voor constanten en variabelen in C#, kunt u de [logboekconversieprogramma’s](https://github.com/Azure-Samples/networking-dotnet-log-converter) gebruiken die beschikbaar zijn in GitHub.

## <a name="additional-resources"></a>Aanvullende resources

* [Visualiseer uw Azure-audit logboeken met Power bi](https://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blog bericht.
* [Bekijk en analyseer Azure-audit Logboeken in Power bi en meer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blog berichten.

## <a name="next-steps"></a>Volgende stappen

[Load Balancer-testen](load-balancer-custom-probe-overview.md)
