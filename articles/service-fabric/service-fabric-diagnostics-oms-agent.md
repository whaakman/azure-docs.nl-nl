---
title: Azure Service Fabric - prestatiebewaking met logboekanalyse | Microsoft Docs
description: Informatie over het instellen van de Log Analytics-Agent voor het bewaken van containers en prestatiemeteritems voor uw Azure Service Fabric-clusters.
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
ms.openlocfilehash: a31fe62f2e81a0e39e4c314fc736e91e72bf7517
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301550"
---
# <a name="performance-monitoring-with-log-analytics"></a>Prestatiebewaking met logboekanalyse

In dit artikel bevat informatie over de stappen voor het Log Analytics-Agent niet toevoegen als een virtuele-machineschaalset extensie voor uw cluster instellen, en te verbinden met uw bestaande Azure Log Analytics-werkruimte. Hierdoor kunnen verzamelen diagnostische gegevens over de containers, toepassingen en bewaking van toepassingsprestaties. Door deze toe te voegen als een uitbreiding op de virtuele machine scale set resource, Azure Resource Manager zorgt ervoor dat deze wordt geïnstalleerd op elk knooppunt, zelfs wanneer schalen van het cluster.

> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat u een Azure-logboekanalyse-werkruimte al ingesteld hebt. Als u dit niet doet, Ga naar [instellen van Azure Log Analytics](service-fabric-diagnostics-oms-setup.md)

## <a name="add-the-agent-extension-via-azure-cli"></a>De agentextensie via Azure CLI toevoegen

De beste manier om Log Analytics Agent toevoegen aan het cluster is via de virtuele-machineschaalset API's die beschikbaar zijn met de Azure CLI ingesteld. Als u Azure CLI instellen nog niet hebt, Ga naar Azure-portal en opent u een [Cloud Shell](../cloud-shell/overview.md) -exemplaar of [2.0 voor Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Zodra uw Cloud-Shell wordt aangevraagd, zorg er dan voor dat u in hetzelfde abonnement als uw resource werkt. Schakel deze optie met `az account show` en zorg ervoor dat de waarde 'name' komt overeen met die van het abonnement voor uw cluster.

2. Navigeer naar de resourcegroep waar uw werkruimte voor logboekanalyse zich bevindt in de Portal. Klik in de Log Analytics-resource (het type van de resource zijn Log Analytics). Wanneer u zich op de overzichtspagina van resource, klik op **geavanceerde instellingen** onder de sectie instellingen in het menu links.

    ![Log Analytics-eigenschappenpagina](media/service-fabric-diagnostics-oms-agent/oms-advanced-settings.png)
 
3. Klik op **Windows-Servers** als u een Windows-cluster zijn permanent en **Linux-Servers** als u een Linux-cluster maakt. Deze pagina ziet u uw `workspace ID` en `workspace key` (weergegeven als primaire sleutel in de portal). U moet zowel voor de volgende stap.

4. Voer de opdracht voor het installeren van de Log Analytics-agent op het cluster met behulp van de `vmss extension set` API in uw Cloud-Shell:

    Voor een Windows-cluster:
    
    ```sh
    az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    Voor een Linux-cluster:

    ```sh
    az vmss extension set --name Log AnalyticsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    Hier volgt een voorbeeld van de Log Analytics-Agent wordt toegevoegd aan een Windows-cluster.

    ![Meld u Analytics agent cli-opdracht](media/service-fabric-diagnostics-oms-agent/cli-command.png)
 
5. Dit duurt minder dan 15 min. is de agent toevoegen aan uw knooppunten. U kunt controleren of de agents zijn toegevoegd met behulp van de `az vmss extension list` API:

    ```sh
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>De agent via het Resource Manager-sjabloon toevoegen

Voorbeeld Resource Manager-sjablonen dat de implementatie van een Azure-logboekanalyse-werkruimte en toevoegen van een agent aan elk van uw knooppunten is beschikbaar voor [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) of [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

U kunt downloaden en wijzigen van deze sjabloon voor het implementeren van een cluster dat het beste past bij uw behoeften.

## <a name="view-performance-counters"></a>Prestatiemeteritems weergeven

Nu dat u hebt de agent Log Analytics, head toegevoegd op wilt naar het Log Analytics-portal om te kiezen welke prestatiemeteritems die u verzamelen. 

1. Ga naar de resourcegroep waarin u de Service Fabric Analytics-oplossing hebt gemaakt in de Azure portal. Selecteer **ServiceFabric\<nameOfLog AnalyticsWorkspace\>**.

2. Klik op **Log Analytics**.

3. Klik op **geavanceerde instellingen**.

4. Klik op **gegevens**, klikt u vervolgens op **Windows of Linux-prestatiemeteritems**. Er is een lijst met standaard prestatiemeteritems die u kiezen kunt om in te schakelen en u het interval voor de verzameling te kan instellen. U kunt ook toevoegen [aanvullende prestatiemeteritems](service-fabric-diagnostics-event-generation-perf.md) te verzamelen. In dit wordt verwezen naar de juiste indeling [artikel](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85).aspx).

5. Klik op **opslaan**, klikt u vervolgens op **OK**.

6. Sluit de blade geavanceerde instellingen.

7. Klik onder de kop algemene **overzicht**.

8. Tegels in de vorm van een grafiek ziet u voor elk van de oplossingen die zijn ingeschakeld, met inbegrip van één voor Service Fabric. Klik op de **Service Fabric** grafiek om door te gaan met de Service Fabric Analytics-oplossing.

9. U ziet enkele tegels met grafieken van operationele kanaal en betrouwbare services gebeurtenissen. De grafische weergave van de gegevens voor de items die u hebt geselecteerd die wordt weergegeven onder het knooppunt metrische gegevens. 

10. Klik op een grafiek Container metrische gegevens voor aanvullende informatie. U kunt ook een query op gegevens van prestatiemeteritems op dezelfde manier naar Clustergebeurtenissen en filter op de knooppunten, perf tellernaam en waarden met behulp van de querytaal Kusto.

![Log Analytics perf teller query](media/service-fabric-diagnostics-event-analysis-oms/oms_node_metrics_table.PNG)

## <a name="next-steps"></a>Volgende stappen

* Collect relevante [prestatiemeteritems](service-fabric-diagnostics-event-generation-perf.md). Voor het configureren van de agent voor logboekanalyse voor het verzamelen van specifieke prestatiemeteritems bekijken [gegevensbronnen configureren](../log-analytics/log-analytics-data-sources.md#configuring-data-sources).
* Configureren van logboekanalyse voor het instellen van [geautomatiseerde waarschuwingen](../log-analytics/log-analytics-alerts.md) om te helpen detecteren en diagnostische gegevens
* Als alternatief kunt u prestatiemeteritems via verzamelen [extensie voor diagnostische gegevens van Azure en verzend dit naar Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template)
