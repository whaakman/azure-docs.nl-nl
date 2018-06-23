---
title: Azure Kubernetes Service (AKS) status (Preview) | Microsoft Docs
description: Dit artikel wordt beschreven hoe u de prestaties van uw container AKS sneller inzicht in het gebruik van uw gehoste omgeving voor Kubernetes gemakkelijk kunt bekijken.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/22/2018
ms.author: magoedte
ms.openlocfilehash: 23109a74fa707759cc3300896392dcc129f3e28c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36335751"
---
# <a name="monitor-azure-kubernetes-service-aks-container-health-preview"></a>Status van de container Azure Kubernetes Service (AKS) (Preview)

In dit artikel wordt beschreven hoe instellen en gebruiken van Azure controleprogramma container status voor het bewaken van de prestaties van uw werkbelastingen die zijn geïmplementeerd op Kubernetes omgevingen die worden gehost op Azure Kubernetes Service (AKS).  Het is erg belangrijk om uw Kubernetes-cluster en -containers te bewaken, vooral wanneer u een productiecluster op schaal uitvoert met meerdere toepassingen.

Status van de container biedt u de mogelijkheid door verzamelen geheugen en processors metrische gegevens van controllers, knooppunten en containers die beschikbaar zijn in Kubernetes via de API van de metrische gegevens voor prestatiebewaking.  Nadat de status van de container is ingeschakeld, deze metrische gegevens automatisch worden verzameld voor u een beperkte versie van de OMS-Agent voor Linux en opgeslagen in uw [logboekanalyse](../log-analytics/log-analytics-overview.md) werkruimte.  De vooraf gedefinieerde weergaven opgenomen weergeven tijdelijk container werkbelastingen en wat invloed op de prestatiestatus van het cluster Kubernetes zodat u inzicht in:  

* Welke containers worden uitgevoerd op het knooppunt en het gemiddelde gebruik voor de processor en geheugen resourceknelpunten identificeren
* Bepalen waar de container zich bevindt in een domeincontroller en/of de gehele product om te zien van de algehele prestaties van een domeincontroller of schil 
* Bekijk het Resourcegebruik van de werkbelasting op de host die geen verband houdt met de ondersteuning van de schil standaardprocedures
* Inzicht in het gedrag van het cluster onder belasting voor de gemiddelde en zwaarste om te identificeren die u nodig hebt en de maximale belasting is het bestand is tegen bepalen 

Als u geïnteresseerd bent in controleren en beheren van de Docker- en Windows container hosts configuratie weergeven, controle en bronnen beter worden benut, Zie de [oplossing Container bewaking](../log-analytics/log-analytics-containers.md).

## <a name="requirements"></a>Vereisten 
Lees voordat u begint, de volgende informatie zodat u in de ondersteunde vereisten inzicht.

- Een nieuwe of bestaande AKS-cluster
- Een beperkte OMS-agent voor Linux-versie microsoft / oms:ciprod04202018 en hoger. Deze agent wordt automatisch geïnstalleerd tijdens de voorbereiding van de status van de container.  
- Een Log Analytics-werkruimte.  Kan worden gemaakt wanneer u de bewaking van het nieuwe cluster AKS inschakelen of u via maken kunt [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), of vanuit de [Azure-portal](../log-analytics/log-analytics-quick-create-workspace.md).
- Lid van de rol Inzender van logboekanalyse om in te schakelen bewaking van de container.  Zie voor meer informatie over het beheren van toegang tot een werkruimte voor logboekanalyse [werkruimten beheren](../log-analytics/log-analytics-manage-access.md).

## <a name="components"></a>Onderdelen 

Deze mogelijkheid is afhankelijk van een beperkte OMS-Agent voor Linux prestatie- en gebeurtenisgegevens verzamelen van alle knooppunten in het cluster.  De agent is geregistreerd met de opgegeven werkruimte voor logboekanalyse nadat u de container bewaking inschakelen en automatisch geïmplementeerd. 

>[!NOTE] 
>Als u een cluster AKS al hebt geïmplementeerd, kunt u inschakelen bewaking met een opgegeven Azure Resource Manager-sjabloon, zoals verderop in dit artikel wordt gedemonstreerd. U kunt geen gebruiken `kubectl` als u wilt bijwerken, verwijderen, opnieuw implementeren of de agent te implementeren.  
>

## <a name="sign-in-to-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com). 

## <a name="enable-container-health-monitoring-for-a-new-cluster"></a>Inschakelen van de container Statuscontrole voor een nieuw cluster
U kunt alleen inschakelen bewaking van uw cluster AKS bij het implementeren van de Azure-portal.  Volg de stappen in de Quick Start-artikel [een Azure Kubernetes Service (AKS)-cluster implementeren](../aks/kubernetes-walkthrough-portal.md).  Wanneer u bent op de **bewaking** pagina **Ja** voor de optie **bewaking inschakelen** inschakelt, en selecteer een bestaande of maak een nieuwe werkruimte voor logboekanalyse.  

Nadat de controle is ingeschakeld alle configuratietaken zijn voltooid, kunt u de prestaties van uw cluster met twee manieren controleren:

1. Rechtstreeks vanaf het cluster AKS door te selecteren **Health** in het linkerdeelvenster.<br><br> 
2. Door te klikken op de **container status** tegel op de pagina van de cluster AKS voor het geselecteerde cluster.  Selecteer in de Azure-Monitor **Health** in het linkerdeelvenster.  

![Opties voor de status van de container in AKS selecteren](./media/monitoring-container-health/container-performance-and-health-select-01.png)

Nadat de controle is ingeschakeld, kunnen duurt ongeveer 15 minuten voordat u kunt voor een overzicht van operationele gegevens voor het cluster.  

## <a name="enable-container-health-monitoring-for-existing-managed-clusters"></a>Inschakelen van de container Statuscontrole voor bestaande beheerde clusters
Inschakelen van bewaking van uw AKS container al geïmplementeerd kan worden uitgevoerd vanuit de Azure-portal of met de opgegeven Azure Resource Manager-sjabloon met de PowerShell-cmdlet **New-AzureRmResourceGroupDeployment** of Azure CLI.  


### <a name="enable-from-azure-portal"></a>Inschakelen van Azure-portal
De volgende stappen uitvoeren om te Schakel de bewaking van de container AKS vanuit de Azure-portal.

1. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Containers**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Kubernetes services**.<br><br> ![Azure Portal](./media/monitoring-container-health/azure-portal-01.png)<br><br>  
2. Selecteer een container in de lijst van containers.
3. Selecteer op de overzichtspagina van container **container status** en de **voorbereiding voor de status van de Container en logboeken** pagina wordt weergegeven.
4. Op de **voorbereiding voor de status van de Container en logboeken** pagina, hebt u een bestaande logboekanalyse werkruimte in hetzelfde abonnement als het cluster, selecteert u deze in de vervolgkeuzelijst.  De lijst worden de standaardwerkruimte er en locatie van de container AKS wordt geïmplementeerd op in het abonnement. Of u kunt selecteren **nieuw** en geeft u een nieuwe werkruimte in hetzelfde abonnement.<br><br> ![Schakel de bewaking van AKS container health](./media/monitoring-container-health/container-health-enable-brownfield.png) 

    Als u selecteert **nieuw**, wordt de **Maak nieuwe werkruimte** deelvenster wordt weergegeven. De **regio** standaardwaarden voor de regio uw containerresource gemaakt in en accepteer de standaardinstelling of Selecteer een andere regio en geef een naam op voor de werkruimte.  Klik op **maken** te accepteren van uw selectie.<br><br> ![Werkruimte voor container monintoring definiëren](./media/monitoring-container-health/create-new-workspace-01.png)  

    >[!NOTE]
    >Op dit moment die u geen nieuwe werkruimte in de regio West-Centraal VS maken kunt, kunt u alleen een bestaande werkruimte selecteren in deze regio.  Hoewel in de lijst kunt u deze regio, de implementatie wordt gestart, maar mislukt kort daarna.  
    >
 
Nadat de controle is ingeschakeld, kunnen duurt ongeveer 15 minuten voordat u kunt voor een overzicht van operationele gegevens voor het cluster. 

### <a name="enable-using-azure-resource-manager-template"></a>Inschakelen met behulp van Azure Resource Manager-sjabloon
Deze methode bevat twee JSON-sjablonen, één sjabloon Hiermee geeft u de configuratie om bewaking inschakelen en het JSON-sjabloon bevat de parameterwaarden die u configureert de volgende opties:

* AKS container resource-ID 
* Resourcegroep het cluster is geïmplementeerd in 
* Log Analytics-werkruimte en de regio voor het maken van de werkruimte in 

De werkruimte voor logboekanalyse is handmatig worden gemaakt.  Voor het maken van de werkruimte kunt u instellen een via [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), uit de [Azure-portal](../log-analytics/log-analytics-quick-create-workspace.md).

Als u niet bekend met concepten bent voor het implementeren van resources met behulp van een sjabloon met PowerShell, raadpleegt u [implementeren van resources met Resource Manager-sjablonen en Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)of Azure CLI-Zie voor [resources met implementeren Resource Manager-sjablonen en Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).

Als u Azure CLI gebruiken, moet u eerst installeren en gebruiken van CLI lokaal.  Dit is vereist dat u de Azure CLI versie 2.0.27 worden uitgevoerd of hoger. Voer `az --version` kunnen herkennen. Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 

#### <a name="create-and-execute-template"></a>Maken en uitvoeren van de sjabloon

1. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "aksResourceId": {
        "type": "string",
        "metadata": {
           "description": "AKS Cluster Resource ID"
           }
    },
    "aksResourceLocation": {
    "type": "string",
     "metadata": {
        "description": "Location of the AKS resource e.g. \"East US\""
       }
    },
    "workspaceResourceId": {
      "type": "string",
      "metadata": {
         "description": "Azure Monitor Log Analytics Resource ID"
       }
    },
    "workspaceRegion": {
    "type": "string",
    "metadata": {
       "description": "Azure Monitor Log Analytics workspace region"
      }
     }
    },
    "resources": [
      {
    "name": "[split(parameters('aksResourceId'),'/')[8]]",
    "type": "Microsoft.ContainerService/managedClusters",
    "location": "[parameters('aksResourceLocation')]",
    "apiVersion": "2018-03-31",
    "properties": {
      "mode": "Incremental",
      "id": "[parameters('aksResourceId')]",
      "addonProfiles": {
        "omsagent": {
          "enabled": true,
          "config": {
            "logAnalyticsWorkspaceResourceID": "[parameters('workspaceResourceId')]"
          }
         }
       }
      }
     },
    {
        "type": "Microsoft.Resources/deployments",
        "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
        "apiVersion": "2017-05-10",
        "subscriptionId": "[split(parameters('workspaceResourceId'),'/')[2]]",
        "resourceGroup": "[split(parameters('workspaceResourceId'),'/')[4]]",
        "properties": {
            "mode": "Incremental",
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {},
                "variables": {},
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "location": "[parameters('workspaceRegion')]",
                        "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                        "properties": {
                            "workspaceResourceId": "[parameters('workspaceResourceId')]"
                        },
                        "plan": {
                            "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                            "product": "[Concat('OMSGallery/', 'ContainerInsights')]",
                            "promotionCode": "",
                            "publisher": "Microsoft"
                        }
                    }
                ]
            },
            "parameters": {}
        }
       }
     ]
    }
    ```

2. Sla dit bestand als **existingClusterOnboarding.json** naar een lokale map.
3. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
       "$schema": "https://schema.management.azure.com/  schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aksResourceId": {
           "value": "/subscriptions/<SubscroptiopnId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
       },
       "aksResourceLocation": {
         "value": "East US"
       },
       "workspaceResourceId": {
         "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
       },
       "workspaceRegion": {
         "value": "eastus"
       }
     }
    }
    ```

4. Bewerk de waarde voor **aksResourceId**, **aksResourceLocation** met de waarden die u kunt vinden op de **AKS overzicht** pagina voor het cluster AKS.  De waarde voor **workspaceResourceId** is de volledige resource-ID van de werkruimte voor logboekanalyse, waaronder de naam van de werkruimte.  Geef ook de locatie van de werkruimte voor het heeft **workspaceRegion**.    
5. Sla dit bestand als **existingClusterParam.json** naar een lokale map.
6. U kunt deze sjabloon nu implementeren. 

    * Gebruik de volgende PowerShell-opdrachten uit de map met de sjabloon:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        Wijzigen van de configuratie kan enkele minuten duren. Wanneer deze is voltooid, ziet u een bericht dat lijkt op de volgende die het resultaat bevat:

        ```powershell
        provisioningState       : Succeeded
        ```

    * Uit te voeren na de opdracht met Azure CLI op Linux:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        Wijzigen van de configuratie kan enkele minuten duren. Wanneer deze is voltooid, ziet u een bericht dat lijkt op de volgende die het resultaat bevat:

        ```azurecli
        provisioningState       : Succeeded
        ```
Nadat de controle is ingeschakeld, kunnen duurt ongeveer 15 minuten voordat u kunt voor een overzicht van operationele gegevens voor het cluster.  

## <a name="verify-agent-deployed-successfully"></a>Controleer of de agent geïmplementeerd
Om te controleren of de OMS-agent correct is geïmplementeerd, voer de volgende opdracht: `kubectl get ds omsagent --namespace=kube-system`.

De uitvoer moet eruitzien als de volgende aangeeft dat deze correct hebt geïmplementeerd:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-performance-utilization"></a>Gebruik van de prestaties weergeven
Wanneer u health container opent, geeft de pagina direct het gebruik van de prestaties van de clusterknooppunten.  Informatie bekijken over uw cluster AKS is onderverdeeld in drie perspectieven:

- Knooppunten 
- Controllers  
- Containers

De hiërarchie rij volgt het Kubernetes-objectmodel beginnen met een knooppunt in het cluster.  Vouw het knooppunt en ziet u een of meer gehele product op het knooppunt uitgevoerd als er meer dan een container voor een schil gegroepeerd, worden weergegeven als de laatste rij in de hiërarchie.<br><br> ![Voorbeeld van Kubernetes knooppunt hiërarchie in de Prestatieweergave](./media/monitoring-container-health/container-performance-and-health-view-03.png)

U kunt domeincontrollers of containers selecteren vanaf de bovenkant van de pagina en het gebruik van status- en resourcegegevens voor die objecten bekijken.  Gebruik de vervolgkeuzelijsten aan de bovenkant van het scherm om te filteren op de naamruimte-, service- en knooppunt. Als in plaats daarvan u bekijken geheugengebruik wilt, uit de **metriek** vervolgkeuzelijst Selecteer **geheugen RSS** of **geheugen werkset**.  **Geheugen RSS** wordt alleen ondersteund voor Kubernetes versie 1.8 en hoger. Raadpleeg anders waarden voor **Gem %** weergegeven als *NaN %*, dit is een waarde van het numerieke gegevens die een niet-gedefinieerde of unrepresentable waarde vertegenwoordigt. 

![Container prestaties knooppunten Prestatieweergave](./media/monitoring-container-health/container-performance-and-health-view-04.png)

Standaard prestatiegegevens is gebaseerd op de laatste zes uur maar u kunt het venster met de **tijdsbereik** vervolgkeuzelijst gevonden in de rechterbovenhoek van de pagina. Op dit moment is de pagina wordt niet automatisch vernieuwen, dus moet u dit handmatig vernieuwen. 

In het volgende voorbeeld ziet u voor het knooppunt *aks-agentpool-3402399-0*, de waarde voor **Containers** is 10, een updatepakket van het totale aantal containers die zijn geïmplementeerd.<br><br> ![Updatepakket van containers per knooppunt voorbeeld](./media/monitoring-container-health/container-performance-and-health-view-07.png)<br><br> Dit kunt u snel kunt nagaan of er geen een goede balans van containers tussen knooppunten in het cluster.  

De volgende tabel beschrijft de informatie die wordt weergegeven wanneer u knooppunten weergeven.

| Kolom | Beschrijving | 
|--------|-------------|
| Naam | De naam van de host |
| Status | Kubernetes weergave van de status van knooppunt |
| % GEM. | Gemiddelde knooppunt percentage op basis van de geselecteerde waarde voor de geselecteerde duur. |
| GEMIDDELD | Gemiddelde werkelijke waarde van knooppunten op basis van de geselecteerde waarde voor de geselecteerde duur.  De gemiddelde waarde wordt gemeten vanuit de CPU/geheugen limiet ingesteld voor een knooppunt. voor het gehele product en containers is de gemiddelde waarde gerapporteerd door de host. |
| Containers | Het aantal containers. |
| Bedrijfstijd | Geeft de tijd sinds een knooppunt is gestart of opnieuw is opgestart. |
| Schil | Alleen voor containers. Er wordt weergegeven dat deze zich pods. |
| Controllers | Alleen voor containers en het gehele product. Er wordt weergegeven welke domeincontroller het die zich bevindt. Niet alle gehele product bevindt zich in een domeincontroller, zodat het kan enkele N.V.T. tonen. | 
| Trend Gem % | Staafdiagram trend op basis van de container en knooppunt Gem metrische %. |


Kies in de selector **domeincontrollers**.<br><br> ![Selecteer controllers weergeven](./media/monitoring-container-health/container-performance-and-health-view-08.png)

Hier ziet u de prestatiestatus van uw domeincontrollers.<br><br> ![< naam > domeincontrollers Prestatieweergave](./media/monitoring-container-health/container-performance-and-health-view-05.png)

De rij-hiërarchie begint met een domeincontroller en de domeincontroller wordt uitgebreid en ziet u een of meer gehele product of een of meer containers.  Vouw een schil en de laatste rij weergeven in de container voor de schil gegroepeerd.  

De volgende tabel beschrijft de gegevens die zijn gepresenteerd wanneer u de Controllers weergeven.

| Kolom | Beschrijving | 
|--------|-------------|
| Naam | De naam van de domeincontroller|
| Status | Status van de containers wanneer deze is voltooid uitgevoerd met de status, zoals *beëindigd*, *mislukt* *gestopt*, of *onderbroken*. Als de container wordt uitgevoerd, maar de status is niet correct weergegeven of is niet opgehaald door de agent als heeft niet gereageerd meer dan 30 minuten, de status worden *onbekende*. |
| % GEM. | Gemiddelde van het gemiddelde percentage van elke entiteit voor de geselecteerde metriek getotaliseerd. |
| GEMIDDELD | Samenvouwen van de gemiddelde CPU millicore of geheugen prestaties van de container.  De gemiddelde waarde wordt van de CPU/geheugen limiet ingesteld voor een schil gemeten. |
| Containers | Totaal aantal containers voor de controller of schil. |
| Opnieuw opstarten | Van het aantal van opnieuw opstarten van berekenen containers. |
| Bedrijfstijd | Geeft de tijd sinds een container is gestart. |
| Schil | Alleen voor containers. Er wordt weergegeven dat deze zich pods. |
| Node | Alleen voor containers en het gehele product. Er wordt weergegeven welke domeincontroller het die zich bevindt. | 
| Trend Gem % | Staafdiagram trend presenteren gemiddelde metrische % van de container. |

Kies in de selector **Containers**.<br><br> ![Selecteer containers weergeven](./media/monitoring-container-health/container-performance-and-health-view-09.png)

Hier kunt ziet u de prestatiestatus van de containers.<br><br> ![< naam > domeincontrollers Prestatieweergave](./media/monitoring-container-health/container-performance-and-health-view-06.png)

De volgende tabel beschrijft de informatie die wordt weergegeven wanneer u Containers weergeven.

| Kolom | Beschrijving | 
|--------|-------------|
| Naam | De naam van de domeincontroller|
| Status | Status van de containers samengevouwen, indien van toepassing. |
| % GEM. | Gemiddelde van het gemiddelde percentage van elke entiteit voor de geselecteerde metriek getotaliseerd. |
| GEMIDDELD | Samenvouwen van de gemiddelde CPU millicore of geheugen prestaties van de container. De gemiddelde waarde wordt van de CPU/geheugen limiet ingesteld voor een schil gemeten. |
| Containers | Totaal aantal containers voor de controller.|
| Opnieuw opstarten | Geeft de tijd sinds een container is gestart. |
| Bedrijfstijd | Geeft de tijd sinds een container is gestart of opnieuw opgestart. |
| Schil | Schil informatie waar het zich bevindt. |
| Node |  Het knooppunt waarin de container zich bevindt.  | 
| Trend Gem % | Staafdiagram trend presenteren gemiddelde metrische % van de container. |

## <a name="container-data-collection-details"></a>De verzameling Gegevensdetails container
Status van de container verzamelt verschillende metrische gegevens en logboekbestanden prestatiegegevens van de container hosts en containers. Elke drie minuten, worden gegevens verzameld.

### <a name="container-records"></a>Containerrecords

De volgende tabel ziet u voorbeelden van records die door de status van de container en de gegevenstypen die worden weergegeven in zoekresultaten logboek worden verzameld.

| Gegevenstype | Het gegevenstype in logboek zoeken | Velden |
| --- | --- | --- |
| Prestaties voor hosts en -containers | `Perf` | Computer, objectnaam, CounterName &#40;percentage processortijd, de schijf leest MB, schijf schrijft MB geheugen gebruik MB netwerk ontvangen Bytes, netwerk verzenden Bytes Processor gebruik seconde, netwerk&#41;, tegenwaarde, TimeGenerated, itempad, SourceSystem |
| Inventarisatie van de container | `ContainerInventory` | TimeGenerated, Computer, containernaam ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, opdracht, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Container installatiekopie inventarisatie | `ContainerImageInventory` | TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, uitvoering is onderbroken, gestopt, is mislukt, SourceSystem, ImageID, TotalContainer |
| Container-logboek | `ContainerLog` | TimeGenerated, Computer, afbeeldings-ID, containernaam LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Container service-logboek | `ContainerServiceLog`  | TimeGenerated, Computer, TimeOfCommand, Image, opdracht, SourceSystem, ContainerID |
| Container knooppunt inventarisatie | `ContainerNodeInventory_CL`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Container-proces | `ContainerProcess_CL` | TimeGenerated, Computer, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Inventarisatie van het gehele product in een cluster Kubernetes | `KubePodInventory` | TimeGenerated, Computer, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, ServiceName, ControllerKind, controllernaam, ContainerStatus, ContainerID, ContainerName, naam, PodLabel, Namespace, PodStatus, ClusterName, PodIp, SourceSystem |
| Inventarisatie van knooppunten deel van een cluster Kubernetes | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, ClusterId, LastTransitionTimeReady, Labels, Status, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Kubernetes gebeurtenissen | `KubeEvents_CL` | TimeGenerated, Computer, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, bericht, SourceSystem | 
| Services in het cluster Kubernetes | `KubeServices_CL` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, SourceSystem | 
| Maatstaven voor prestaties voor knooppunten die deel uitmaken van het cluster Kubernetes | Perf &#124; waar ObjectName == "K8SNode" | Computer, objectnaam, CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, networkRxBytes, networkTxBytes, restartTimeEpoch, networkRxBytesPerSec, networkTxBytesPerSec, cpuAllocatableNanoCores, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes&#41;, tegenwaarde, TimeGenerated, itempad, SourceSystem | 
| Maatstaven voor prestaties voor containers die deel uitmaken van het cluster Kubernetes | Perf &#124; waar ObjectName == "K8SContainer" | CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, restartTimeEpoch, cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryLimitBytes&#41;, tegenwaarde, TimeGenerated, itempad, SourceSystem | 

## <a name="search-logs-to-analyze-data"></a>Zoeken in Logboeken om gegevens te analyseren
Log Analytics kunt u trends zoekt, knelpunten vaststellen, prognose, of het vergelijken gegevens waarmee u bepaalt of de huidige configuratie van het cluster optimaal.  Vooraf gedefinieerde logboek zoekopdrachten zijn beschikbaar om direct te gebruiken of aanpassen om de gewenste manier kunt u de gegevens. 

U kunt interactieve analyses van gegevens in de werkruimte uitvoeren door het selecteren van de **logboek weergeven** optie beschikbaar is op de helemaal rechts als u een container uitbreidt.  **Meld u zoekopdracht** pagina wordt weergegeven boven de pagina die u al had geopend in de portal.<br><br> ![Gegevens analyseren in Log Analytics](./media/monitoring-container-health/container-performance-and-health-view-logs-01.png)   

De uitvoer van de container-logboeken worden doorgestuurd naar logboekanalyse zijn STDOUT en STDERR. Omdat de status van de container wordt bewaakt door Azure beheerde Kubernetes (AKS), wordt Kube system niet vandaag verzameld vanwege de grote hoeveelheid gegevens die zijn gegenereerd.     

### <a name="example-log-search-queries"></a>Voorbeeld logboek zoekquery 's
Vaak is het nuttig voor het bouwen van query's beginnen met een voorbeeld ongeveer twee minuten en vervolgens aan uw behoeften te bewerken. U kunt experimenteren met de volgende voorbeeldquery's waarmee u kunt meer geavanceerde query's maken.

| Query | Beschrijving | 
|-------|-------------|
| ContainerInventory<br> &#124;Computer, naam, de installatiekopie, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime project<br> &#124;tabel weergeven | Lijst van alle Container lifecycle informatie| 
| KubeEvents_CL<br> &#124;waar not(isempty(Namespace_s))<br> &#124;TimeGenerated desc sorteren<br> &#124;tabel weergeven | Kubernetes gebeurtenissen|
| ContainerImageInventory<br> &#124;overzicht van AggregatedValue = count() door de installatiekopie, ImageTag, actief | Inventarisatie van de afbeelding | 
| **Selecteer in Advanced Analytics lijndiagrammen**:<br> Perf<br> &#124;waar ObjectName == "Container" en CounterName == "% processortijd"<br> &#124;overzicht van AvgCPUPercent avg(CounterValue) door bin (TimeGenerated, 30m), InstanceName = | Container CPU | 
| **Selecteer in Advanced Analytics lijndiagrammen**:<br> Perf &#124; waar ObjectName == "Container" en CounterName == 'MB geheugen gebruik'<br> &#124;overzicht van AvgUsedMemory avg(CounterValue) door bin (TimeGenerated, 30m), InstanceName = | Container-geheugen |

## <a name="how-to-stop-monitoring-with-container-health"></a>Hoe u met de status van de container bewaking te stoppen
Na het inschakelen van bewaking van de container AKS besluit u niet langer wilt controleren, kunt u *opt-out* met de opgegeven Azure Resource Manager-sjablonen met de PowerShell-cmdlet  **Nieuwe AzureRmResourceGroupDeployment** of Azure CLI.  Een JSON-sjabloon bevat de configuratie van *opt-out* en het JSON-sjabloon bevat de parameterwaarden die u configureert voor het opgeven van de AKS-ID en resource clusterbrongroep in het cluster is geïmplementeerd.  Als u niet bekend met concepten bent voor het implementeren van resources met behulp van een sjabloon met PowerShell, raadpleegt u [implementeren van resources met Resource Manager-sjablonen en Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md) of Azure CLI-Zie voor [resources met implementeren Resource Manager-sjablonen en Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).

Als u Azure CLI gebruiken, moet u eerst installeren en gebruiken van CLI lokaal.  Dit is vereist dat u de Azure CLI versie 2.0.27 worden uitgevoerd of hoger. Voer `az --version` kunnen herkennen. Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 

### <a name="create-and-execute-template"></a>Maken en uitvoeren van de sjabloon

1. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "aksResourceId": {
           "type": "string",
           "metadata": {
             "description": "AKS Cluster Resource ID"
           }
       },
      "aksResourceLocation": {
        "type": "string",
        "metadata": {
           "description": "Location of the AKS resource e.g. \"East US\""
         }
       }
    },
    "resources": [
      {
        "name": "[split(parameters('aksResourceId'),'/')[8]]",
        "type": "Microsoft.ContainerService/managedClusters",
        "location": "[parameters('aksResourceLocation')]",
        "apiVersion": "2018-03-31",
        "properties": {
          "mode": "Incremental",
          "id": "[parameters('aksResourceId')]",
          "addonProfiles": {
            "omsagent": {
              "enabled": false,
              "config": {
                "logAnalyticsWorkspaceResourceID": null
              }
            }
           }
         }
       }
      ]
    }
    ```

2. Sla dit bestand als **OptOutTemplate.json** naar een lokale map.
3. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "aksResourceId": {
         "value": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
      },
      "aksResourceLocation": {
        "value": "<aksClusterRegion>"
        }
      }
    }
    ```

4. Bewerk de waarde voor **aksResourceId** en **aksResourceLocation** met de waarden van het cluster AKS, kunt u vinden op de **eigenschappen** pagina voor het geselecteerde cluster.<br><br> ![De eigenschappenpagina van container](./media/monitoring-container-health/container-properties-page.png)<br>

    Als u zich op de **eigenschappen** pagina, kopieert u ook de **werkruimte Resource-ID**.  Deze waarde is vereist als u besluit dat u wilt verwijderen van de werkruimte voor logboekanalyse later, dat niet wordt uitgevoerd als onderdeel van dit proces.  

5. Sla dit bestand als **OptOutParam.json** naar een lokale map.
6. U kunt deze sjabloon nu implementeren. 

    * De volgende PowerShell-opdrachten gebruiken vanuit de map met de sjabloon:

        ```powershell
        Connect-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
        New-AzureRmResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
        ```

        Wijzigen van de configuratie kan enkele minuten duren. Wanneer deze is voltooid, wordt een bericht dat lijkt op de volgende mogelijkheden van het resultaat geretourneerd:

        ```powershell
        ProvisioningState       : Succeeded
        ```

    * Uit te voeren na de opdracht met Azure CLI op Linux:

        ```azurecli
        az login   
        az account set --subscription "Subscription Name" 
        az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
        ```

        Wijzigen van de configuratie kan enkele minuten duren. Wanneer deze is voltooid, wordt een bericht dat lijkt op de volgende mogelijkheden van het resultaat geretourneerd:

        ```azurecli
        ProvisioningState       : Succeeded
        ```

Als de werkruimte alleen ter ondersteuning van de bewaking van het cluster is gemaakt en niet langer nodig, hebt u het bestand handmatig verwijderen. Als u niet bekend met het verwijderen van een werkruimte bent, Zie [verwijderen met de Azure portal een Azure-logboekanalyse-werkruimte](../log-analytics/log-analytics-manage-del-workspace.md).  Vergeet niet over de **werkruimte Resource-ID** we eerder in stap 4 hebt gekopieerd, gaat u dat nodig.  

## <a name="troubleshooting"></a>Probleemoplossing
Deze sectie bevat informatie over het oplossen van problemen met de status van de container.

Als de status van de container met succes is ingeschakeld en geconfigureerd, maar u bent niet gezien statusinformatie of resulteert in een Log Analytics wanneer u een logboek zoekopdracht uitvoert, kunt u de volgende stappen uit om u te helpen bij het analyseren van het probleem kunt uitvoeren.   

1. Controleer de status van de agent met de volgende opdracht: `kubectl get ds omsagent --namespace=kube-system`

    De uitvoer moet eruitzien als de volgende weergegeven dat de agent wordt uitgevoerd op alle knooppunten in het cluster.  Bijvoorbeeld, dit cluster heeft twee knooppunten en houd rekening met de waarde van even groot aantal knooppunten.  

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```
2. Controleer de status van de schil om te controleren of dat deze wordt uitgevoerd of niet door de volgende opdracht uit te voeren: `kubectl get pods --namespace=kube-system`

    De uitvoer moet eruitzien als de status van de volgende *met* voor de omsagent:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

3. Raadpleeg de logboeken van de agent. Wanneer de beperkte-agent wordt geïmplementeerd, wordt een snelle controle uitgevoerd door de OMI opdrachten uit te voeren en toont de versie van de agent en de Docker-provider. Om te zien dat de agent vrijgegeven is is, voer de volgende opdracht: `kubectl logs omsagent-484hw --namespace=kube-system`

    De status moet als volgt uit:

    ```
    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.2
    omsagent 1.6.0.23
    docker-cimprov 1.0.0.31
    ```

## <a name="next-steps"></a>Volgende stappen

[Zoeken in een logboek](../log-analytics/log-analytics-log-search.md) om gedetailleerde container status en toepassing prestatiegegevens weer te geven.  