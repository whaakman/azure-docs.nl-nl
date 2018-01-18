---
title: Azure Service Fabric - bewaking met de OMS-Agent instellen | Microsoft Docs
description: Informatie over het instellen van de OMS-Agent voor het bewaken van containers en prestatiemeteritems voor uw Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/31/2017
ms.author: dekapur
ms.openlocfilehash: 095db20e7d22bd517337f24fc9a81b84988d1465
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="add-the-oms-agent-to-a-cluster"></a>De OMS-Agent aan een cluster toevoegen

In dit artikel bevat informatie over de stappen voor de OMS-Agent niet toevoegen als een virtuele-machineschaalset extensie voor uw cluster instellen, en te verbinden met uw bestaande logboekanalyse OMS-werkruimte. Hierdoor kunnen verzamelen diagnostische gegevens over de containers, toepassingen en bewaking van toepassingsprestaties. Door deze toe te voegen als een uitbreiding, Azure Resource Manager zorgt ervoor dat deze wordt geïnstalleerd op elk knooppunt, zelfs wanneer schalen van het cluster.

> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat u een logboekanalyse OMS-werkruimte al ingesteld hebt. Als u dit niet doet, Ga naar [OMS Log Analytics instellen](service-fabric-diagnostics-oms-setup.md)

## <a name="add-the-agent-extension-via-azure-cli"></a>De agentextensie via Azure CLI toevoegen

De beste manier om de OMS-Agent toevoegen aan het cluster is via de virtuele-machineschaalset API's die beschikbaar zijn met de Azure CLI ingesteld. Als u Azure CLI instellen nog niet hebt, Ga naar Azure-portal en opent u een [Cloud Shell](../cloud-shell/overview.md) -exemplaar of [2.0 voor Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Zodra uw Cloud-Shell wordt aangevraagd, zorg er dan voor dat u in hetzelfde abonnement als uw resource werkt. Schakel deze optie met `az account show` en zorg ervoor dat de waarde 'name' komt overeen met die van het abonnement voor uw cluster.

2. Navigeer naar de resourcegroep waar uw OMS-werkruimte zich bevindt in de Portal. Klik in de Log Analytics-resource (het type van de resource zijn Log Analytics), op de juiste navigatie, schuif naar beneden en klik op **eigenschappen**.

    ![OMS-eigenschappenpagina](media/service-fabric-diagnostics-oms-agent/oms-properties.png)

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
 
    Dit duurt minder dan 15 min. is de agent toevoegen aan uw knooppunten. U kunt controleren of de agents zijn toegevoegd met behulp van de `az vmss extension list` API:

    ```sh
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>De agent via het Resource Manager-sjabloon toevoegen

Voorbeeld Resource Manager-sjablonen dat de implementatie van een werkruimte voor logboekanalyse OMS en toevoegen van een agent aan elk van uw knooppunten is beschikbaar voor [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) of [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

U kunt downloaden en wijzigen van deze sjabloon voor het implementeren van een cluster dat het beste past bij uw behoeften.

## <a name="next-steps"></a>Volgende stappen

* Collect relevante [prestatiemeteritems](service-fabric-diagnostics-event-generation-perf.md). De OMS-agent om op te halen specifieke prestatiemeteritems, head naar de OMS-Portal (gekoppeld aan de bovenkant van de resource OMS Log Analytics) configureren. Klik vervolgens op **Start > Instellingen > gegevens > Windows-prestatiemeteritems** of **Linux prestatiemeteritems** en kies de items die u wilt verzamelen.
* Configureren van OMS om in te stellen [geautomatiseerde waarschuwingen](../log-analytics/log-analytics-alerts.md) om te helpen detecteren en diagnostische gegevens
