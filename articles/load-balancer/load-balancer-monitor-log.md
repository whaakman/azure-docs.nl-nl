---
title: Bewaken van bewerkingen, gebeurtenissen en prestatiemeteritems voor de Load Balancer | Microsoft Docs
description: Informatie over het inschakelen van waarschuwingsgebeurtenissen en registratie van de health-status voor Azure Load Balancer-test
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-resource-manager
ms.assetid: 56656d74-0241-4096-88c8-aa88515d676d
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 36b9379f88ce024c1dcbf9977a873d95076d10df
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="log-analytics-for-azure-load-balancer"></a>Logboekanalyse voor Azure Load Balancer

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

U kunt verschillende typen logboeken in Azure beheren en problemen oplossen netwerktaakverdelers. Sommige van deze logboeken zijn toegankelijk via de portal. Alle logboeken kunnen worden opgehaald uit Azure blob-opslag en worden bekeken in verschillende hulpprogramma's zoals Excel en Power BI. U kunt meer informatie over de verschillende typen logboeken in de onderstaande lijst.

* **Controlelogboeken:** kunt u [Azure controlelogboeken](../monitoring-and-diagnostics/insights-debugging-with-events.md) (voorheen bekend als operationele Logboeken) om alle bewerkingen die worden verzonden naar uw Azure-abonnementen en hun status weer te geven. Controlelogboeken zijn standaard ingeschakeld en kunnen worden weergegeven in de Azure-portal.
* **Waarschuwing van gebeurtenislogboeken:** kunt u dit logboek rasied waarschuwingen weergeven met de load balancer. De status voor de load balancer worden verzameld om de vijf minuten. Dit logboek wordt alleen geschreven als een load balancer waarschuwing gebeurtenis treedt op.
* **Health test Logboeken:** kunt u dit logboek bekijken door uw health test, zoals het aantal exemplaren in uw back-end-pool die aanvragen vanwege health test problemen niet van de load balancer ontvangen gedetecteerde problemen. Dit logboek wordt geschreven naar wanneer er een wijziging in de status van de test.

> [!IMPORTANT]
> Meld u analytics momenteel werkt alleen voor Internet gerichte load balancers. Logboeken zijn alleen beschikbaar voor resources die zijn geïmplementeerd in het Resource Manager-implementatiemodel. U kunt Logboeken niet gebruiken voor resources in het klassieke implementatiemodel. Zie voor meer informatie over de implementatiemodellen [Understanding Resource Manager-implementatie en klassieke implementatie](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Logboekregistratie inschakelen

Logboekregistratie is automatisch ingeschakeld voor elke Resource Manager-resource. U moet inschakelen gebeurtenis en health test logboekregistratie voor het starten van de gegevens beschikbaar zijn via deze logboeken worden verzameld. Gebruik de volgende stappen logboekregistratie in te schakelen.

Aanmelden bij de [Azure-portal](http://portal.azure.com). Als u een load balancer, nog niet hebt [maken van een load balancer](load-balancer-get-started-internet-arm-ps.md) voordat u doorgaat.

1. Klik in de portal op **Bladeren**.
2. Selecteer **Taakverdelers**.

    ![Portal - load balancer](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Selecteer een bestaande load balancer >> **alle instellingen**.
4. Aan de rechterkant van het dialoogvenster onder de naam van de load balancer, schuif naar **bewaking**, klikt u op **Diagnostics**.

    ![Portal - instellingen van een load balancer](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. In de **Diagnostics** deelvenster onder **Status**, selecteer **op**.
6. Klik op **Opslagaccount**.
7. Onder **LOGBOEKEN**, selecteer een bestaand opslagaccount of maak een nieuwe. Gebruik de schuifregelaar om te bepalen hoeveel dagen gebeurtenisgegevens worden opgeslagen in de gebeurtenislogboeken. 
8. Klik op **Opslaan**.

    ![Portal - logboeken met diagnostische gegevens](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

> [!NOTE]
> Controlelogboeken vereisen een afzonderlijke opslagaccount niet. Het gebruik van opslag voor de gebeurtenis en health test logboekregistratie wordt service worden kosten in rekening.

## <a name="audit-log"></a>Controlelogboek

Het controlelogboek wordt standaard gegenereerd. De logboeken worden bewaard gedurende 90 dagen in de Azure gebeurtenislogboeken store. Meer informatie over deze logboeken door te lezen de [gebeurtenissen bekijken en controlelogboeken](../monitoring-and-diagnostics/insights-debugging-with-events.md) artikel.

## <a name="alert-event-log"></a>Waarschuwing gebeurtenislogboek

Dit logboek wordt alleen gegenereerd als u het hebt ingeschakeld op een per load balancer basis. De gebeurtenissen worden vastgelegd in JSON-indeling en opgeslagen in het opslagaccount dat u hebt opgegeven als u de logboekregistratie ingeschakeld. Hier volgt een voorbeeld van een gebeurtenis.

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

De JSON-uitvoer bevat de *eventname* -eigenschap hebben die de reden voor de load balancer beschrijft een waarschuwing gemaakt. In dit geval is de waarschuwing gegenereerd als gevolg van uitputting van de TCP-poort is veroorzaakt door de bron-IP NAT limieten (snat omzetten).

## <a name="health-probe-log"></a>Health test-logboek

Dit logboek wordt alleen gegenereerd als u het hebt ingeschakeld op een per per load balancer als gedetailleerde hierboven. De gegevens worden opgeslagen in het opslagaccount dat u hebt opgegeven als u de logboekregistratie ingeschakeld. Een container met de naam 'insights-logboeken-loadbalancerprobehealthstatus' wordt gemaakt en de volgende gegevens worden geregistreerd:

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

De JSON-uitvoer ziet in het eigenschappenveld de basisgegevens voor de status van de test. De *dipDownCount* eigenschap geeft het totale aantal exemplaren op de back-end die netwerkverkeer vanwege mislukte test antwoorden niet ontvangen.

## <a name="view-and-analyze-the-audit-log"></a>Weergeven en analyseren van het controlelogboek

U kunt weergeven en analyseren van audit logboekgegevens met behulp van een van de volgende methoden:

* **Azure-hulpprogramma's:** gegevens ophalen uit de controlelogboeken via Azure PowerShell, de Azure-opdrachtregelinterface (CLI), de REST-API van Azure of de Azure preview portal. Stapsgewijze instructies voor elke methode zijn aangegeven in de [bewerkingen met Resource Manager controleren](../azure-resource-manager/resource-group-audit.md) artikel.
* **Power BI:** als u nog geen een [Power BI](https://powerbi.microsoft.com/pricing) account, kunt u proberen deze gratis. Met behulp van de [Azure controlelogboeken inhoud pack voor Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), kunt u uw gegevens met vooraf geconfigureerde dashboards analyseren, of u weergaven aanpassen aan uw vereisten kunt aanpassen.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>Weergeven en analyseren van de gebeurtenislogboeken en health test

U moet verbinding maken met uw opslagaccount en ophalen van de JSON-logboekvermeldingen voor gebeurtenis en health test-Logboeken. Zodra u de JSON-bestanden hebt gedownload, kunt u ze kunt converteren naar CSV en de weergave in Excel, Power BI of een ander gegevensvisualisatie-hulpprogramma.

> [!TIP]
> Als u bekend met Visual Studio en de basisconcepten bent van het wijzigen van waarden voor de constanten en variabelen in C#, kunt u de [Meld converter extra](https://github.com/Azure-Samples/networking-dotnet-log-converter) beschikbaar is via GitHub.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Uw Azure controlelogboeken met Power BI visualiseren](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogbericht.
* [Weergeven en analyseren van Azure controlelogboeken in Power BI en meer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbericht.

## <a name="next-steps"></a>Volgende stappen

[Load Balancer-testen](load-balancer-custom-probe-overview.md)
