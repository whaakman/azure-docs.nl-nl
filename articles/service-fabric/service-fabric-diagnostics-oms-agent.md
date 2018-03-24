---
title: Azure Service Fabric - bewaking met de OMS-Agent instellen | Microsoft Docs
description: Informatie over het instellen van de OMS-Agent voor het bewaken van containers en prestatiemeteritems voor uw Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2018
ms.author: dekapur
ms.openlocfilehash: 4c4095071235dac7e8be3c16b614bdfa5b706a1c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="add-the-oms-agent-to-a-cluster"></a>De OMS-Agent aan een cluster toevoegen

In dit artikel bevat informatie over de stappen voor de OMS-Agent niet toevoegen als een virtuele-machineschaalset extensie voor uw cluster instellen, en te verbinden met uw bestaande Azure Log Analytics-werkruimte. Hierdoor kunnen verzamelen diagnostische gegevens over de containers, toepassingen en bewaking van toepassingsprestaties. Door deze toe te voegen als een uitbreiding, Azure Resource Manager zorgt ervoor dat deze wordt geïnstalleerd op elk knooppunt, zelfs wanneer schalen van het cluster.

> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat u een Azure-logboekanalyse-werkruimte al ingesteld hebt. Als u dit niet doet, Ga naar [instellen van Azure Log Analytics](service-fabric-diagnostics-oms-setup.md)

## <a name="add-the-agent-extension-via-azure-cli"></a>De agentextensie via Azure CLI toevoegen

De beste manier om de OMS-Agent toevoegen aan het cluster is via de virtuele-machineschaalset API's die beschikbaar zijn met de Azure CLI ingesteld. Als u Azure CLI instellen nog niet hebt, Ga naar Azure-portal en opent u een [Cloud Shell](../cloud-shell/overview.md) -exemplaar of [2.0 voor Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Zodra uw Cloud-Shell wordt aangevraagd, zorg er dan voor dat u in hetzelfde abonnement als uw resource werkt. Schakel deze optie met `az account show` en zorg ervoor dat de waarde 'name' komt overeen met die van het abonnement voor uw cluster.

2. Navigeer naar de resourcegroep waar uw werkruimte voor logboekanalyse zich bevindt in de Portal. Klik in de Log Analytics-resource (het type van de resource zijn Log Analytics), op de juiste navigatie, schuif naar beneden en klik op **eigenschappen**.

    ![Log Analytics-eigenschappenpagina](media/service-fabric-diagnostics-oms-agent/oms-properties.png)

    Noteer de `workspaceId`. 

3. U moet ook uw `workspaceKey` om de agent te implementeren. Als u voor u sleutel, klikt u op **geavanceerde instellingen**onder de *instellingen* sectie van de linkernavigatiebalk. Klik op **Windows-Servers** als u een Windows-cluster zijn permanent en **Linux-Servers** als u een Linux-cluster maakt. U moet de *primaire sleutel* die wordt weergegeven voor de implementatie van de agents als de `workspaceKey`.

4. Voer de opdracht voor het installeren van de OMS-agent op het cluster met behulp van de `vmss extension set` API in uw Cloud-Shell:

    Voor een Windows-cluster:
    
    ```sh
    az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<OMSworkspaceId>'}" --protected-settings "{'workspaceKey':'<OMSworkspaceKey>'}"
    ```

    Voor een Linux-cluster:

    ```sh
    az vmss extension set --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<OMSworkspaceId>'}" --protected-settings "{'workspaceKey':'<OMSworkspaceKey>'}"
    ```

    Hier volgt een voorbeeld van de OMS-Agent wordt toegevoegd aan een Windows-cluster.

    ![OMS-agent cli-opdracht](media/service-fabric-diagnostics-oms-agent/cli-command.png)
 
5. Voer de opdracht deze configuratie toepassen op uw VM-exemplaren die al bestaan:  


    ```sh
    az vmss update-instances
    ```

Dit duurt minder dan 15 min. is de agent toevoegen aan uw knooppunten. U kunt controleren of de agents zijn toegevoegd met behulp van de `az vmss extension list` API:

    ```sh
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>De agent via het Resource Manager-sjabloon toevoegen

Voorbeeld Resource Manager-sjablonen dat de implementatie van een Azure-logboekanalyse-werkruimte en toevoegen van een agent aan elk van uw knooppunten is beschikbaar voor [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) of [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

U kunt downloaden en wijzigen van deze sjabloon voor het implementeren van een cluster dat het beste past bij uw behoeften.

## <a name="next-steps"></a>Volgende stappen

* Collect relevante [prestatiemeteritems](service-fabric-diagnostics-event-generation-perf.md). Voor het configureren van de OMS-agent voor het verzamelen van specifieke prestatiemeteritems bekijken [gegevensbronnen configureren](../log-analytics/log-analytics-data-sources.md#configuring-data-sources).
* Configureren van logboekanalyse voor het instellen van [geautomatiseerde waarschuwingen](../log-analytics/log-analytics-alerts.md) om te helpen detecteren en diagnostische gegevens
