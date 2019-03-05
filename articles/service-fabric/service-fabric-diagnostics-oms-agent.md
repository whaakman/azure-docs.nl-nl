---
title: Logboeken van Azure Service Fabric - prestaties controleren met Azure Monitor | Microsoft Docs
description: Meer informatie over het instellen van de Log Analytics-Agent voor het bewaken van containers en prestatiemeteritems voor uw Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/16/2018
ms.author: srrengar
ms.openlocfilehash: 6562156432a86c346a0fee382af50f210e3cf6dc
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57308513"
---
# <a name="performance-monitoring-with-azure-monitor-logs"></a>Prestaties controleren met Azure Monitor-Logboeken

In dit artikel bevat informatie over de stappen voor het toevoegen van de Log Analytics-agent als een virtuele-machineschaalset extensie met uw cluster instellen en deze verbinden met uw bestaande Azure Log Analytics-werkruimte. Hierdoor kunnen verzamelen van diagnostische gegevens over containers, toepassingen en bewaking van toepassingsprestaties. Door deze toe te voegen als een uitbreiding op de virtuele machine scale set-resource, Azure Resource Manager zorgt ervoor dat deze wordt geïnstalleerd op elk knooppunt, zelfs wanneer schalen van het cluster.

> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat u een Azure Log Analytics-werkruimte al ingesteld hebt. Als u dit niet doet, Ga naar [instellen van Azure Monitor-Logboeken](service-fabric-diagnostics-oms-setup.md)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="add-the-agent-extension-via-azure-cli"></a>Toevoegen van de agent-extensie via Azure CLI

De beste manier om de Log Analytics-agent toevoegen aan uw cluster is via de virtuele-machineschaalset API's die beschikbaar zijn met de Azure CLI ingesteld. Als u Azure CLI instellen van nog niet hebt, Ga naar Azure portal en maak een [Cloud Shell](../cloud-shell/overview.md) -exemplaar of [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Zodra uw Cloud Shell is aangevraagd, zorg er dan voor dat u werkt in hetzelfde abonnement als uw resource. Schakel deze optie met `az account show` en zorg ervoor dat de waarde "naam" komt overeen met die van het abonnement van uw cluster.

2. Navigeer naar de resourcegroep waar uw Log Analytics-werkruimte zich bevindt in de Portal. Klik in de log analytics-resource (het type van de resource zijn Log Analytics-werkruimte). Als u zich op de overzichtspagina van resource, klikt u op **geavanceerde instellingen** in het gedeelte instellingen in het menu links.

    ![Eigenschappenpagina van log analytics](media/service-fabric-diagnostics-oms-agent/oms-advanced-settings.png)

3. Klik op **Windows Servers** als u een Windows-cluster zijn permanent en **Linux-Servers** als u een Linux-cluster maakt. Deze pagina ziet u uw `workspace ID` en `workspace key` (weergegeven als primaire sleutel in de portal). U moet zowel voor de volgende stap.

4. Voer de opdracht voor het installeren van de Log Analytics-agent op uw cluster met behulp van de `vmss extension set` API in uw Cloud-Shell:

    Voor een Windows-cluster:

    ```sh
    az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    Voor een Linux-cluster:

    ```sh
    az vmss extension set --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    Hier volgt een voorbeeld van de Log Analytics-agent wordt toegevoegd aan een Windows-cluster.

    ![Meld u Analytics-agent cli-opdracht](media/service-fabric-diagnostics-oms-agent/cli-command.png)

5. Dit duurt minder dan 15 minuten is de agent toevoegen aan uw knooppunten. U kunt controleren dat de agents zijn toegevoegd met behulp van de `az vmss extension list` API:

    ```sh
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>De agent via het Resource Manager-sjabloon toevoegen

Voorbeeld van Resource Manager-sjablonen die bij het implementeren van een Azure Log Analytics-werkruimte en een agent toevoegen aan elk van de knooppunten is beschikbaar voor [Windows](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-OMS-UnSecure) of [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

U kunt downloaden en wijzigen van deze sjabloon voor het implementeren van een cluster die het beste bij uw behoeften.

## <a name="view-performance-counters"></a>Prestatiemeteritems weergeven

Nu dat u hebt de Log Analytics-agent, head toegevoegd op wilt naar de Log Analytics-portal om te kiezen welke prestatiemeteritems die u verzamelen.

1. In de Azure-portal, gaat u naar de resourcegroep waarin u de Service Fabric-analyse-oplossing hebt gemaakt. Selecteer **ServiceFabric\<nameOfLog AnalyticsWorkspace\>**.

2. Klik op **Log Analytics**.

3. Klik op **geavanceerde instellingen**.

4. Klik op **gegevens**, klikt u vervolgens op **Windows of Linux-prestatiemeteritems**. Er is een lijst van standaard-prestatiemeteritems waarop die u kiezen kunt om in te schakelen en u kunt het interval voor de verzameling te instellen. U kunt ook toevoegen [aanvullende prestatiemeteritems](service-fabric-diagnostics-event-generation-perf.md) te verzamelen. De juiste indeling wordt verwezen in dit [artikel](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85).aspx).

5. Klik op **opslaan**, klikt u vervolgens op **OK**.

6. Sluit de blade geavanceerde instellingen.

7. Klik onder de kop algemene **werkruimte overzicht**.

8. Tegels in de vorm van een grafiek ziet u voor elk van de oplossingen ingeschakeld, met inbegrip van een Service Fabric. Klik op de **Service Fabric** graph om door te gaan naar de Service Fabric-analyse-oplossing.

9. U ziet een aantal tegels met grafieken op operationele kanaal en betrouwbare services-gebeurtenissen. De grafische weergave van de gegevens die voor de items die u hebt geselecteerd, worden weergegeven onder knooppunt metrische gegevens.

10. Klik op een grafiek Container metrische gegevens om meer details te bekijken. U kunt ook een query op gegevens van prestatiemeteritems op soortgelijke wijze aan Clustergebeurtenissen en filtert u op de knooppunten, naam van het prestatiemeteritem voor prestaties en waarden met behulp van de Kusto-query-taal.

![Log Analytics voor prestaties teller-query](media/service-fabric-diagnostics-event-analysis-oms/oms_node_metrics_table.PNG)

## <a name="next-steps"></a>Volgende stappen

* Collect relevante [prestatiemeteritems](service-fabric-diagnostics-event-generation-perf.md). Voor het configureren van de Log Analytics-agent voor het verzamelen van specifieke prestatiemeteritems, Bekijk [gegevensbronnen configureren](../azure-monitor/platform/agent-data-sources.md#configuring-data-sources).
* Logboeken voor het instellen van Azure Monitor configureren [automatische waarschuwingen](../log-analytics/log-analytics-alerts.md) bij detectie en diagnostiek
* Als alternatief kunt u verzamelen van prestatiemeteritems via [Azure Diagnostics-extensie en ze verzenden naar Application Insights](service-fabric-diagnostics-event-aggregation-wad.md#add-the-application-insights-sink-to-the-resource-manager-template)
