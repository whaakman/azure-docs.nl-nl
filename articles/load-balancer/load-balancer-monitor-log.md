---
title: Bewaken van bewerkingen, gebeurtenissen en prestatiemeteritems voor openbare Basic Load Balancer | Microsoft Docs
description: Meer informatie over het inschakelen van waarschuwingsgebeurtenissen en registratie van de health-status voor openbare Basic Load Balancer-test
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/10/2018
ms.author: kumud
ms.openlocfilehash: eaa44b4fff47f8e3385e86f5c86a090ff84a6bf8
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51218879"
---
# <a name="log-analytics-for-public-basic-load-balancer"></a>Log analytics voor openbare Basic Load Balancer

>[!IMPORTANT] 
>Azure Load Balancer ondersteunt twee verschillende typen: Basic en Standard. In dit artikel wordt Basic Load Balancer beschreven. Zie voor meer informatie over Standard Load Balancer [overzicht van Standard Load Balancer](load-balancer-standard-overview.md) die telemetrie via multi-dimensionale metrische gegevens in Azure Monitor beschikbaar maakt.

U kunt verschillende soorten logboeken in Azure gebruiken om te beheren en problemen oplossen Basic Load Balancers. Sommige van deze logboeken kunnen worden geopend via de portal. Alle logboeken kunnen worden opgehaald uit Azure blob-opslag, en worden bekeken in verschillende hulpprogramma's, zoals Excel en Power BI. U kunt meer informatie over de verschillende typen logboeken in de onderstaande lijst.

* **Auditlogboeken:** kunt u [auditlogboeken van Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (voorheen bekend als de operationele Logboeken) om alle bewerkingen die worden verzonden naar uw Azure-abonnementen en hun status weer te geven. Auditlogboeken zijn standaard ingeschakeld en kunnen worden weergegeven in de Azure-portal.
* **Waarschuwing gebeurtenislogboeken:** kunt u dit logboek om weer te geven van waarschuwingen die worden gegenereerd door de load balancer. De status voor de load balancer worden verzameld om de vijf minuten. Dit logboek wordt alleen geschreven als een load balancer-waarschuwingsgebeurtenis wordt gegenereerd.
* **Statustestlogboeken:** kunt u dit logboek om problemen gedetecteerd door de statustest, zoals het aantal exemplaren in uw back-end-pool die geen aanvragen van de load balancer vanwege health test problemen ontvangen zijn weer te geven. Dit logboek wordt geschreven naar wanneer er een wijziging in de status van de test.

> [!IMPORTANT]
> Meld u analytics momenteel geldt alleen voor openbare Basic netwerktaakverdelers. Logboeken zijn alleen beschikbaar voor resources die zijn geïmplementeerd in het Resource Manager-implementatiemodel. U kunt Logboeken niet gebruiken voor resources in het klassieke implementatiemodel. Zie voor meer informatie over de implementatiemodellen [Understanding Resource Manager-implementatie en klassieke implementatie](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Logboekregistratie inschakelen

Het controlelogboek is automatisch ingeschakeld voor elke Resource Manager-resource. U moet inschakelen gebeurtenis en health test logboekregistratie voor het starten van de gegevens beschikbaar zijn via deze logboeken verzamelen. Gebruik de volgende stappen om in te schakelen van logboekregistratie.

Aanmelden bij de [Azure-portal](http://portal.azure.com). Als u een load balancer, nog niet hebt [maken van een load balancer](load-balancer-get-started-internet-arm-ps.md) voordat u doorgaat.

1. Klik in de portal op **Bladeren**.
2. Selecteer **Load Balancers**.

    ![Portal - load balancer](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Selecteer een bestaande load balancer >> **alle instellingen**.
4. Aan de rechterkant van het dialoogvenster onder de naam van de load balancer, schuif naar **bewaking**, klikt u op **Diagnostics**.

    ![Portal - instellingen van een load balancer](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. In de **Diagnostics** deelvenster onder **Status**, selecteer **op**.
6. Klik op **Opslagaccount**.
7. Onder **LOGBOEKEN**, selecteert u een bestaand opslagaccount of maak een nieuwe. Gebruik de schuifregelaar om te bepalen hoeveel dagen aan gebeurtenisgegevens worden opgeslagen in de gebeurtenislogboeken. 
8. Klik op **Opslaan**.

Diagnostische gegevens wordt opgeslagen in Table Storage in het opgegeven opslagaccount. Als u Logboeken niet worden opgeslagen, is omdat er geen relevante logboeken zijn wordt geproduceerd.

![Portal - logboeken met diagnostische gegevens](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

> [!NOTE]
> Auditlogboeken vereisen geen een afzonderlijk opslagaccount. Het gebruik van opslag voor gebeurtenis- en health test logboekregistratie service kosten in rekening gebracht.

## <a name="audit-log"></a>Auditlogboek

Het controlelogboek wordt standaard gegenereerd. De logboeken worden gedurende 90 dagen bewaard in de azurestore van gebeurtenislogboeken. Meer informatie over deze logboeken vindt de [weergeven van gebeurtenissen en auditlogboeken](../monitoring-and-diagnostics/insights-debugging-with-events.md) artikel.

## <a name="alert-event-log"></a>Waarschuwing gebeurtenislogboek

Dit logboek wordt alleen gegenereerd als u deze hebt ingeschakeld op een per load balancer basis. De gebeurtenissen worden vastgelegd in JSON-indeling en opgeslagen in het opslagaccount dat u opgeeft wanneer u de logboekregistratie hebt ingeschakeld. Hier volgt een voorbeeld van een gebeurtenis.

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

De JSON-uitvoer toont de *eventname* eigenschap waarin de reden voor de load balancer wordt een waarschuwing gemaakt. In dit geval is de waarschuwing gegenereerd vanwege een TCP-poortuitputting veroorzaakt door bron IP NAT (SNAT) limieten.

## <a name="health-probe-log"></a>Health test log

Dit logboek wordt alleen gegenereerd als u deze hebt ingeschakeld op een per load balancer gefactureerd zoals hierboven. De gegevens worden opgeslagen in het opslagaccount dat u hebt opgegeven toen u de logboekregistratie hebt ingeschakeld. Een container met de naam 'insights-logs-loadbalancerprobehealthstatus' wordt gemaakt en de volgende gegevens worden geregistreerd:

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

De JSON-uitvoer laat zien in het eigenschappenveld van de algemene informatie voor de status van de test. De *dipDownCount* eigenschap geeft het totale aantal exemplaren op de back-end die zijn geen netwerkverkeer ontvangen vanwege mislukte testrespons.

## <a name="view-and-analyze-the-audit-log"></a>Bekijk en Analyseer het auditlogboek

U kunt bekijken en analyseren van logboekgegevens van controle met behulp van een van de volgende methoden:

* **Azure-hulpprogramma's:** gegevens ophalen uit de auditlogboeken via Azure PowerShell, de Azure-opdrachtregelinterface (CLI), de Azure REST API of de Azure previewportal. Stapsgewijze instructies voor elke methode worden beschreven in de [bewerkingen controleren met Resource Manager](../azure-resource-manager/resource-group-audit.md) artikel.
* **Power BI:** als u nog geen een [Power BI](https://powerbi.microsoft.com/pricing) -account, kunt u proberen deze gratis. Met behulp van de [auditlogboeken van Azure-inhoudspakket voor Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), kunt u uw gegevens met vooraf geconfigureerde dashboards analyseren, of u weergaven aan de behoeften van uw behoeften kunt aanpassen.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>Bekijk en analyseer de statustest en het gebeurtenislogboek

U moet verbinding maken met uw opslagaccount en ophalen van vermeldingen in de JSON voor de gebeurtenis en health test-Logboeken. Nadat u de JSON-bestanden hebt gedownload, kunt u ze kunt converteren naar CSV en weergeven in Excel, Power BI of een ander hulpprogramma voor gegevensvisualisatie.

> [!TIP]
> Als u bekend bent met Visual Studio en de basisconcepten van het wijzigen van waarden voor constanten en variabelen in C#, kunt u de [logboekconversieprogramma’s](https://github.com/Azure-Samples/networking-dotnet-log-converter) gebruiken die beschikbaar zijn in GitHub.

## <a name="additional-resources"></a>Aanvullende resources

* [Uw Azure-auditlogboeken met Power BI visualiseren](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogbericht.
* [Bekijk en analyseer Azure-controlelogboeken in Power BI en meer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbericht.

## <a name="next-steps"></a>Volgende stappen

[Load Balancer-testen](load-balancer-custom-probe-overview.md)
