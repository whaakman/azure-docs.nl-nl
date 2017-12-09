---
title: Controle en diagnostische gegevens voor Windows-Containers in Azure Service Fabric | Microsoft Docs
description: Instellen van controle en diagnostische gegevens voor de Windows-Container gedirigeerd op Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2017
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 8fe3266cfcb7141684f9e1b5dfa74d6569c23b24
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="monitor-windows-containers-on-service-fabric-using-oms"></a>Windows-containers in Service Fabric met OMS bewaken

Dit maakt deel uit drie van een zelfstudie en wordt u begeleid bij het instellen van OMS voor het bewaken van uw Windows-containers gedirigeerd op Service Fabric.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * OMS configureren voor uw Service Fabric-cluster
> * Met een OMS-werkruimte kunt weergeven en logboeken van de containers en knooppunten query
> * Configureer de OMS-agent om op te halen container en metrische gegevens voor knooppunt

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint, moet u het volgende doen:
- Een cluster in Azure hebt of [in deze zelfstudie maken](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
- [Een beperkte toepassing implementeren](service-fabric-host-app-in-a-container.md)

## <a name="setting-up-oms-with-your-cluster-in-the-resource-manager-template"></a>Instellen van OMS met uw cluster in de Resource Manager-sjabloon

In het geval dat u gebruikt de [sjabloon opgegeven](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Tutorial) in het eerste deel van deze zelfstudie moet hierbij de volgende toevoegingen aan een algemene Service Fabric Azure Resource Manager-sjabloon. In geval het geval wilt dat u een cluster van uw eigen die hebt u instellen voor het bewaken van containers met OMS:
* De volgende wijzigingen aanbrengen in uw Resource Manager-sjabloon.
* Implementeren met behulp van PowerShell upgraden van uw cluster door [implementeren van de sjabloon](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm). Azure Resource Manager realiseert dat de resource bestaat, wordt dit rollen uit als een upgrade.

### <a name="adding-oms-to-your-cluster-template"></a>OMS toe te voegen aan de sjabloon voor het cluster

Breng de volgende wijzigingen in uw *template.json*:

1. De locatie van de OMS-werkruimte toevoegen en de naam van uw *parameters* sectie:
    
    ```json
    "omsWorkspacename": {
      "type": "string",
      "defaultValue": "[toLower(concat('sf',uniqueString(resourceGroup().id)))]",
      "metadata": {
        "description": "Name of your OMS Log Analytics Workspace"
      }
    },
    "omsRegion": {
      "type": "string",
      "defaultValue": "East US",
      "allowedValues": [
        "West Europe",
        "East US",
        "Southeast Asia"
      ],
      "metadata": {
        "description": "Specify the Azure Region for your OMS workspace"
      }
    }
    ```

    Wijzigen van de waarde gebruikt voor een toevoegen dezelfde parameters naar uw *template.parameters.json* en de gebruikte waarden wijzigen.

2. Toevoegen van de naam van de oplossing en de oplossing voor uw *variabelen*: 
    
    ```json
    "omsSolutionName": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "omsSolution": "ServiceFabric"
    ```

3. OMS Microsoft Monitoring Agent als de extensie van een virtuele machine toevoegen. Zoeken naar virtuele-machineschaalsets resource: *resources* > *'apiVersion': '[variables('vmssApiVersion')]'*. Onder de *eigenschappen* > *virtualMachineProfile* > *extensionProfile* > *extensies*, voeg de volgende extensie beschrijving onder de *ServiceFabricNode* extensie: 
    
    ```json
    {
        "name": "[concat(variables('vmNodeType0Name'),'OMS')]",
        "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "workspaceId": "[reference(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')), '2015-11-01-preview').customerId]"
            },
            "protectedSettings": {
                "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')),'2015-11-01-preview').primarySharedKey]"
            }
        }
    },
    ```

4. De OMS-werkruimte als een afzonderlijke resource toevoegen. In *resources*nadat de virtuele-machineschaalsets resource, voeg de volgende:
    
    ```json
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(variables('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', variables('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [ ],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', variables('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            },
            {
                "apiVersion": "2015-11-01-preview",
                "name": "System",
                "type": "datasources",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
                ],
                "kind": "WindowsEvent",
                "properties": {
                    "eventLogName": "System",
                    "eventTypes": [
                        {
                            "eventType": "Error"
                        },
                        {
                            "eventType": "Warning"
                        },
                        {
                            "eventType": "Information"
                        }
                    ]
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('omsSolutionName')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('OMSWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('omsSolutionName')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('omsSolution'))]",
            "promotionCode": ""
        }
    },
    ```

[Hier](https://github.com/ChackDan/Service-Fabric/blob/master/ARM%20Templates/Tutorial/azuredeploy.json) is een voorbeeldsjabloon (die wordt gebruikt in onderdeel een van deze zelfstudie) met alle van deze wijzigingen die u kunt verwijzen naar behoefte. Deze wijzigingen wordt een logboekanalyse OMS-werkruimte toevoegen aan de resourcegroep. De werkruimte wordt geconfigureerd voor het Service Fabric-platform gebeurtenissen uit de opslagtabellen geconfigureerd met de [Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) agent. De OMS-agent (Microsoft Monitoring Agent) is ook toegevoegd aan elk knooppunt in het cluster als de extensie van een virtuele machine - dit betekent dat als u uw cluster schalen, de agent wordt automatisch geconfigureerd op elke machine en aangesloten op dezelfde werkruimte.

Implementeer de sjabloon met de nieuwe wijzigingen in uw huidige cluster upgraden. U ziet de OMS-resources in uw resourcegroep wanneer dit is voltooid. Wanneer het cluster gereed is, moet u uw beperkte toepassing voor het implementeren. We instellen bewaking van de containers in de volgende stap.

## <a name="add-the-container-monitoring-solution-to-your-oms-workspace"></a>De oplossing voor het bewaken van Container toevoegen aan uw OMS-werkruimte

Als u de Container-oplossing instelt in uw werkruimte, zoek naar *Container bewaking oplossing* en maak een resource Containers (onder de bewaking en beheer categorie).

![Containers oplossing toevoegen](./media/service-fabric-tutorial-monitoring-wincontainers/containers-solution.png)

Als u wordt gevraagd de *OMS-werkruimte*, selecteer de werkruimte die is gemaakt in de resourcegroep en klik op **maken**. Hiermee voegt u een *Container bewaking oplossing* aan uw werkruimte automatisch, wordt de OMS-agent geïmplementeerd door de sjabloon die begint met het verzamelen van docker-logboeken en statistieken. 

Ga terug naar uw *resourcegroep*, waarbij u ziet nu de zojuist toegevoegde oplossing voor controle. Als u op in de App, de landingspagina moet worden weergegeven u het aantal container installatiekopieën die u hebt uitgevoerd. 

*Houd er rekening mee dat ik heb 5 exemplaren van mijn fabrikam container van het object [deel twee](service-fabric-host-app-in-a-container.md) van de zelfstudie*

![Startpagina van de container-oplossing](./media/service-fabric-tutorial-monitoring-wincontainers/solution-landing.png)

Te klikken in de **Container monitoroplossing** gaat u naar een meer gedetailleerde dashboard, zodat u kunt meerdere panelen schuift, evenals een query's uitvoeren in logboekanalyse. 

*Houd er rekening mee dat vanaf September 2017, de oplossing wordt verzonden via een aantal updates - eventuele fouten die mogelijk dat u over Kubernetes gebeurtenissen zoals we werken aan het integreren van meerdere orchestrators in dezelfde oplossing negeren.*

Omdat de agent docker-logboeken ophalen is, is de standaardwaarde tonen *stdout* en *stderr*. Als u naar rechts schuiven, ziet u container installatiekopie voorraad, status, metrische gegevens en voorbeeldquery's die meer nuttige gegevens kan worden uitgevoerd. 

![Dashboard van de container-oplossing](./media/service-fabric-tutorial-monitoring-wincontainers/container-metrics.png)

In een van deze vensters klikken, gaat u naar de Log Analytics-query die de weergegeven waarde genereert. Wijzig de query voor het  *\**  voor een overzicht van alle verschillende soorten logboeken die zijn worden opgenomen. Hier kunt u kunt een query of filter voor de prestaties van de container, Logboeken of kijken naar gebeurtenissen voor Service Fabric-platform. De agents zijn ook voortdurend tekensetcodering geen heartbeat van elk knooppunt dat u bekijken kunt om te controleren of gegevens nog steeds worden verzameld uit alle uw machines als uw clusterconfiguratie wordt gewijzigd.   

![Container query](./media/service-fabric-tutorial-monitoring-wincontainers/query-sample.png)

## <a name="configure-oms-agent-to-pick-up-performance-counters"></a>OMS-agent om op te halen prestatiemeteritems configureren

Een ander voordeel van het gebruik van de OMS-agent is de mogelijkheid om te wijzigen van de prestatiemeteritems die u wilt ophalen via de OMS-UI-mogelijkheden in plaats van hoeft te configureren van de Azure diagnostics-agent en een Resource Manager-sjabloon gebaseerd upgrade telkens. Om dit te doen, klikt u op **OMS-Portal** op de startpagina van uw oplossing bewaking van de Container (of Service Fabric).

![OMS-portal](./media/service-fabric-tutorial-monitoring-wincontainers/oms-portal.png)

Dit wordt gaat u naar uw werkruimte in de OMS-portal waar u kunt bekijken uw oplossingen, maak aangepaste dashboards, evenals de OMS-agent configureren. 
* Klik op de **tandwiel wheel** in de rechterbovenhoek van het scherm om te openen de *instellingen* menu.
* Klik op **verbonden bronnen** > **Windows-Servers** om te controleren of er *5 Windowscomputers verbonden*.
* Klik op **gegevens** > **Windows-prestatiemeteritems** om te zoeken en toevoegen van nieuwe prestatiemeteritems. Hier ziet u een lijst met aanbevelingen van OMS voor prestatiemeteritems dat u kan verzamelen en de optie om te zoeken naar andere tellers. Klik op **de geselecteerde prestatiemeteritems toevoegen** starten van de voorgestelde metrische gegevens te verzamelen.

    ![-Prestatiemeteritems](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters.png)

In de Azure portal **vernieuwen** uw oplossing Container bewaking in een paar minuten en u moet worden gestart om te zien *computerprestaties* gegevens die afkomstig zijn. Dit helpt u begrijpen hoe uw resources worden gebruikt. U kunt deze metrische gegevens ook gebruiken om juiste beslissingen te nemen over het schalen van uw cluster, of om te bevestigen dat als een cluster is taakverdeling uw zoals verwacht.

*Opmerking: Controleer of dat uw Tijdfilters correct zijn ingesteld voor u deze metrische gegevens te gebruiken.* 

![Prestatiemeteritems 2](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters2.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * OMS configureren voor uw Service Fabric-cluster
> * Met een OMS-werkruimte kunt weergeven en logboeken van de containers en knooppunten query
> * Configureer de OMS-agent om op te halen container en metrische gegevens voor knooppunt

Nu dat u hebt ingesteld voor uw toepassing met beperkte bewaking, probeert u het volgende:

* Stel de OMS voor een Linux-cluster moet dezelfde stappen als hierboven. Verwijzing [deze sjabloon](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux) wijzigingen aanbrengen in de Resource Manager-sjabloon.
* Configureren van OMS om in te stellen [geautomatiseerde waarschuwingen](../log-analytics/log-analytics-alerts.md) om te helpen detecteren en diagnostische gegevens.
* Lijst met de Service Fabric verkennen [aanbevolen prestatiemeteritems](service-fabric-diagnostics-event-generation-perf.md) configureren voor uw clusters.
* Familiarized ophalen met de [zoeken en uitvoeren van query's in logboek registreren](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van logboekanalyse.