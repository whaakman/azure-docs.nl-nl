---
title: Controleren van Azure Kubernetes Service (AKS)-status (preview) | Microsoft Docs
description: Dit artikel wordt beschreven hoe u de prestaties van uw AKS-container om te zien van het gebruik van uw gehoste Kubernetes-omgeving snel kunt zien.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/30/2018
ms.author: magoedte
ms.openlocfilehash: f84452af9c2c731d69d5805961266c46351a7687
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366093"
---
# <a name="monitor-azure-kubernetes-service-aks-container-health-preview"></a>Containerstatus van Azure Kubernetes Service (AKS) (voorbeeld) bewaken

Dit artikel wordt beschreven hoe u kunt instellen en containerstatus van Azure Monitor gebruiken voor het bewaken van de prestaties van workloads die zijn geïmplementeerd in Kubernetes-omgevingen en worden gehost in Azure Kubernetes Service (AKS). Controle van uw Kubernetes-cluster en de containers is kritiek, met name wanneer u een productiecluster op schaal, met meerdere toepassingen uitvoert.

Containerstatus biedt u de mogelijkheid door verzamelen geheugen en processors metrische gegevens van domeincontrollers, knooppunten en containers die beschikbaar in Kubernetes via de API voor metrische gegevens zijn voor prestatiebewaking. Nadat u de containerstatus hebt ingeschakeld, deze metrische gegevens automatisch worden via een beperkte versie van de Operations Management Suite (OMS)-Agent voor Linux voor u verzameld en opgeslagen in uw [Log Analytics](../log-analytics/log-analytics-overview.md) werkruimte. De opgenomen vooraf gedefinieerde weergaven weergegeven die container-workloads en wat van invloed is op de prestatiestatus van het Kubernetes-cluster dat u kunt:  

* Identificeer de containers die worden uitgevoerd op het knooppunt en het gemiddelde gebruik van de processor en geheugen. Aan de hand van deze kennis kunt u knelpunten in de resource.
* Bepalen waar de container bevindt zich in een domeincontroller of een schil. Aan de hand van deze kennis kunt u de algehele prestaties van de van de domeincontroller of de schil weergeven. 
* Controleer het brongebruik van workloads die worden uitgevoerd op de host die niet gerelateerd zijn aan de standaard processen die ondersteuning bieden voor de schil.
* Begrijp het gedrag van het cluster bij gemiddelde en zwaarste belasting. Deze kennis kunt u identificeren behoeften aan capaciteit en bepaal de maximale belasting van het cluster kan tolereren. 

Als u geïnteresseerd bent in controle en beheer van uw Docker- en Windows containerhosts configuratie weergeven, controle en gebruik van resources, Zie de [Container Monitoring solution](../log-analytics/log-analytics-containers.md).

## <a name="prerequisites"></a>Vereisten 
Voordat u begint, zorg ervoor dat u het volgende hebt:

- Een nieuw of bestaand AKS-cluster.
- Een App in een container OMS-Agent voor Linux-versie microsoft / oms:ciprod04202018 of hoger. Het versienummer wordt vertegenwoordigd door een datum in de volgende indeling: *mmddyyyy*. De agent wordt automatisch geïnstalleerd tijdens de onboarding van de containerstatus. 
- Een Log Analytics-werkruimte. Als u bewaking van uw nieuwe AKS-cluster of laat het onboarding-ervaring een standaardwerkruimte maken in de standaard-resourcegroep van het AKS-cluster-abonnement, kunt u deze maken. Als u wilt deze zelf maken, kunt u het maken via [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), tot en met [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), of in de [Azure-portal](../log-analytics/log-analytics-quick-create-workspace.md).
- De Log Analytics inzendersrol, containerbewaking van de wilt inschakelen. Zie voor meer informatie over het beheren van toegang tot een Log Analytics-werkruimte [werkruimten beheren](../log-analytics/log-analytics-manage-access.md).

## <a name="components"></a>Onderdelen 

De mogelijkheid om prestaties te bewaken is gebaseerd op een beperkte OMS-Agent voor Linux, welke prestaties en gebeurtenisgegevens worden verzameld van alle knooppunten in het cluster. De agent is geregistreerd met de opgegeven Log Analytics-werkruimte nadat u containerbewaking inschakelen en automatisch geïmplementeerd. 

>[!NOTE] 
>Als u al een AKS-cluster hebt geïmplementeerd, kunt u inschakelen bewaking met behulp van Azure CLI of een opgegeven Azure Resource Manager-sjabloon, zoals verderop in dit artikel wordt gedemonstreerd. U kunt geen gebruiken `kubectl` als u wilt bijwerken, verwijderen, opnieuw implementeren of implementeren van de agent. 
>

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij [Azure Portal](https://portal.azure.com). 

## <a name="enable-container-health-monitoring-for-a-new-cluster"></a>Inschakelen van de container en statusbewaking biedt voor een nieuw cluster
Tijdens de implementatie kunt u bewaking van een nieuw AKS-cluster in Azure portal of met Azure CLI. Volg de stappen in dit artikel [een Azure Kubernetes Service (AKS)-cluster implementeren](../aks/kubernetes-walkthrough-portal.md) als u wilt inschakelen vanuit de portal. Op de **bewaking** pagina voor de **bewaking inschakelen** optie, selecteer **Ja**, en selecteer een bestaande Log Analytics-werkruimte of maak een nieuwe. 

Volg de stappen in dit artikel in de sectie voor bewaking van een nieuw AKS-cluster gemaakt met Azure CLI, [maken-AKS-cluster](../aks/kubernetes-walkthrough.md#create-aks-cluster).  

>[!NOTE]
>Als u ervoor de Azure CLI gebruiken kiest, moet u eerst installeren en de CLI lokaal gebruikt. U moet worden uitgevoerd van Azure CLI versie 2.0.27 of hoger. Voor het identificeren van uw versie uitvoeren `az --version`. Als u wilt installeren of upgraden van de Azure CLI, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli). 
>

Nadat u bewaking hebt ingeschakeld, en alle configuratietaken zijn voltooid, kunt u de prestaties van uw cluster op twee manieren controleren:

* Rechtstreeks in het AKS-cluster door te selecteren **Health** in het linkerdeelvenster.
* Door het selecteren van de **containerstatus bewaken** tegel op de pagina van de AKS-cluster voor het geselecteerde cluster. In Azure Monitor in het linkerdeelvenster selecteert **Health**. 

  ![Opties voor het selecteren van de containerstatus in AKS](./media/monitoring-container-health/container-performance-and-health-select-01.png)

Wanneer u bewaking inschakelt, is het duurt ongeveer 15 minuten voordat u de operationele gegevens voor het cluster kunt weergeven. 

## <a name="enable-container-health-monitoring-for-existing-managed-clusters"></a>Inschakelen van de container en statusbewaking biedt voor bestaande beheerde clusters
U kunt inschakelen bewaking van een AKS-cluster dat al geïmplementeerd met behulp van Azure CLI, vanuit de portal of met de opgegeven Azure Resource Manager-sjabloon met behulp van de PowerShell-cmdlet `New-AzureRmResourceGroupDeployment`. 

### <a name="enable-monitoring-using-azure-cli"></a>Controle inschakelen met Azure CLI
De volgende stap kunt bewaking van uw AKS-cluster met behulp van Azure CLI. In dit voorbeeld zijn u niet verplicht per maken of geef een bestaande werkruimte. Met deze opdracht vereenvoudigt het proces voor u door het maken van een standaard-werkruimte in de standaard-resourcegroep van het AKS-cluster-abonnement als deze niet al in de regio bestaat.  De standaard-werkruimte gemaakt lijkt op de indeling van *standaardwerkruimte -<GUID>-<Region>*.  

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG  
```

De uitvoer ziet eruit als in het volgende:

```azurecli
provisioningState       : Succeeded
```

### <a name="enable-monitoring-in-the-azure-portal"></a>Schakel bewaking in Azure portal
Als u wilt inschakelen voor bewaking van uw AKS-container in Azure portal, het volgende doen:

1. Selecteer in de Azure portal, **alle services**. 
2. Begin met typen in de lijst met resources **Containers**.  
    De lijst gefilterd op basis van uw invoer. 
3. Selecteer **Kubernetes-services**.  

    ![De koppeling van Kubernetes-services](./media/monitoring-container-health/azure-portal-01.png)

4. Selecteer een container in de lijst met containers.
5. Selecteer op de overzichtspagina van container **containerstatus bewaken**.  
6. Op de **Onboarding naar containerstatus en logboeken** pagina, hebt u een bestaande Log Analytics-werkruimte in hetzelfde abonnement bevinden als het cluster, selecteert u deze in de vervolgkeuzelijst.  
    De lijst worden er de standaardwerkruimte en de locatie die het AKS-container is geïmplementeerd op in het abonnement. 

    ![Statuscontrole van AKS container inschakelen](./media/monitoring-container-health/container-health-enable-brownfield-02.png)

>[!NOTE]
>Als u maken van een nieuwe werkruimte voor logboekanalyse wilt voor het opslaan van de gegevens uit het cluster, volg de instructies in [een Log Analytics-werkruimte maken](../log-analytics/log-analytics-quick-create-workspace.md). Zorg ervoor dat de werkruimte maakt in hetzelfde abonnement dat voor de AKS-container is geïmplementeerd. 
 
Wanneer u bewaking inschakelt, is het duurt ongeveer 15 minuten voordat u de operationele gegevens voor het cluster kunt weergeven. 

### <a name="enable-monitoring-by-using-an-azure-resource-manager-template"></a>Schakel de bewaking met behulp van een Azure Resource Manager-sjabloon
Deze methode omvat twee JSON-sjablonen. Een sjabloon Hiermee geeft u de configuratie voor bewaking en de andere bevat parameterwaarden die u configureert voor het volgende opgeven:

* De AKS container resource-ID. 
* De resourcegroep die in het cluster is geïmplementeerd.
* De Log Analytics-werkruimte en de regio voor het maken van de werkruimte in. 

De Log Analytics-werkruimte heeft handmatig worden gemaakt. Voor het maken van de werkruimte, u kunt dit instellen via [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), tot en met [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), of in de [Azure-portal](../log-analytics/log-analytics-quick-create-workspace.md).

Als u niet bekend met het concept bent van het implementeren van resources met behulp van een sjabloon, Zie:
* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Resources implementeren met Resource Manager-sjablonen en Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)

Als u ervoor de Azure CLI gebruiken kiest, moet u eerst installeren en de CLI lokaal gebruikt. U moet worden uitgevoerd van Azure CLI versie 2.0.27 of hoger. Voor het identificeren van uw versie uitvoeren `az --version`. Als u wilt installeren of upgraden van de Azure CLI, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli). 

#### <a name="create-and-execute-a-template"></a>Maken en uitvoeren van een sjabloon

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
3. Plak de volgende JSON-syntaxis in het bestand:

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

4. Bewerk de waarden voor **aksResourceId** en **aksResourceLocation** met behulp van de waarden op de **overzicht van AKS** -pagina voor het AKS-cluster. De waarde voor **workspaceResourceId** is de volledige resource-ID van uw Log Analytics-werkruimte, waaronder de naam van de werkruimte. Geef ook de locatie van de werkruimte voor **workspaceRegion**. 
5. Sla dit bestand als **existingClusterParam.json** naar een lokale map.
6. U kunt deze sjabloon nu implementeren. 

    * Gebruik de volgende PowerShell-opdrachten in de map met de sjabloon:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid, wordt er een bericht weergegeven dat vergelijkbaar is met het volgende en het resultaat bevat:

        ```powershell
        provisioningState       : Succeeded
        ```

    * De volgende opdracht uitvoeren met behulp van de Azure CLI op Linux:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid, wordt er een bericht weergegeven dat vergelijkbaar is met het volgende en het resultaat bevat:

        ```azurecli
        provisioningState       : Succeeded
        ```
Wanneer u bewaking inschakelt, is het duurt ongeveer 15 minuten voordat u de operationele gegevens voor het cluster kunt weergeven. 

## <a name="verify-agent-and-solution-deployment"></a>Controleer of de implementatie van agent en de oplossing
Met de versie van agent *06072018* of hoger, kunt u controleren dat zowel de agent en de oplossing zijn geïmplementeerd. U kunt alleen de implementatie van de agent controleren met eerdere versies van de agent.

### <a name="agent-version-06072018-or-later"></a>Agentversie 06072018 of hoger
Voer de volgende opdracht om te controleren dat de agent is geïmplementeerd. 

```
kubectl get ds omsagent --namespace=kube-system
```

De uitvoer moet eruitzien zoals in het volgende, waarmee wordt aangegeven dat deze correct is geïmplementeerd:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

Als u wilt controleren of de implementatie van de oplossing, moet u de volgende opdracht uitvoeren:

```
kubectl get deployment omsagent-rs -n=kube-system
```

De uitvoer moet eruitzien zoals in het volgende, waarmee wordt aangegeven dat deze correct is geïmplementeerd:

```
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>Agent-versie ouder is dan 06072018

Om te controleren dat de versie van de OMS-agent vóór uitgebracht *06072018* correct is geïmplementeerd met de volgende opdracht uitvoeren:  

```
kubectl get ds omsagent --namespace=kube-system
```

De uitvoer moet eruitzien zoals in het volgende, waarmee wordt aangegeven dat deze correct is geïmplementeerd:  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-configuration-with-cli"></a>Configuratie met CLI
Gebruik de `aks show` opdracht om informatie te verkrijgen die de oplossing of niet ingeschakeld is, wat is de Log Analytics-werkruimte resourceID en overzichtsgegevens over het cluster.  

```azurecli
az aks show -g <resoourceGroupofAKSCluster> -n <nameofAksCluster>
```

Na een paar minuten, de opdracht is voltooid en retourneert JSON opgemaakte informatie over de oplossing.  De resultaten van de opdracht het controle-Add-on-profiel moet worden weergegeven en lijkt op de volgende voorbeelduitvoer:

```
"addonProfiles": {
    "omsagent": {
      "config": {
        "logAnalyticsWorkspaceResourceID": "/subscriptions/<WorkspaceSubscription>/resourceGroups/<DefaultWorkspaceRG>/providers/Microsoft.OperationalInsights/workspaces/<defaultWorkspaceName>"
      },
      "enabled": true
    }
  }
```

## <a name="view-performance-utilization"></a>Gebruik van de prestaties weergeven
Wanneer u containerstatus opent, geeft de pagina onmiddellijk het gebruik van de prestaties van uw hele cluster. Informatie bekijken over uw AKS-cluster is onderverdeeld in vier perspectieven:

- Cluster
- Knooppunten 
- Controllers  
- Containers

Op de **Cluster** tabblad van de vier lijndiagrammen prestaties geven belangrijke prestatiegegevens van uw cluster. 

![Voorbeeld van de prestatiegrafieken weergegeven op het tabblad Cluster](./media/monitoring-container-health/container-health-cluster-perfview.png)

Het diagram van de prestaties worden vier metrische gegevens voor prestaties weergegeven:

- **Knooppunt CPU-gebruik&nbsp;%**: een samengevoegde perspectief van de CPU-gebruik voor het hele cluster. U kunt filteren dat de resultaten voor het tijdsbereik selecteren **Avg**, **Min**, **Max**, **50e**, **90e**, en **95th** in de kiezer percentielen boven de grafiek, ofwel afzonderlijk of gecombineerd. 
- **Geheugengebruik knooppunt&nbsp;%**: een samengevoegde perspectief van geheugengebruik voor het hele cluster. U kunt filteren dat de resultaten voor het tijdsbereik selecteren **Avg**, **Min**, **Max**, **50e**, **90e**, en **95th** in de kiezer percentielen boven de grafiek, ofwel afzonderlijk of gecombineerd. 
- **Aantal knooppunten**: een aantal knooppunten en de status van Kubernetes. Status van de clusterknooppunten weergegeven zijn *alle*, *gereed*, en *niet gereed* en kunnen worden gefilterd afzonderlijk of in de kiezer boven de grafiek worden gecombineerd. 
- **Activiteit pod aantal**: een pod aantal en de status van Kubernetes. Status van de vertegenwoordigd schillen zijn *alle*, *in behandeling*, *uitgevoerd*, en *onbekende* en kunnen worden gefilterd, afzonderlijk of gecombineerd de kiezer boven de grafiek. 

Als u naar overschakelt de **knooppunten** tabblad en de hiërarchie rij volgt het Kubernetes-objectmodel beginnen met een knooppunt in het cluster. Vouw het knooppunt en vindt u een of meer schillen die worden uitgevoerd op het knooppunt. Als meer dan één container is gegroepeerd op een schil, worden ze weergegeven als de laatste rij in de hiërarchie. U kunt ook bekijken hoeveel niet-schil gerelateerde workloads worden uitgevoerd op de host als de host processor of geheugen.

![Voorbeeld van de Kubernetes-knooppunt-hiërarchie in de weergave van agentprestaties](./media/monitoring-container-health/container-health-nodes-view.png)

U kunt selecteren controllers of containers aan de bovenkant van de pagina en het gebruik van de status en resource voor die objecten bekijken. Gebruik de vervolgkeuzelijsten boven om te filteren op de naamruimte, service en knooppunt. Als in plaats daarvan u bekijken geheugengebruik wilt, in de **Metric** vervolgkeuzelijst, selecteer **geheugen RSS** of **geheugenwerkset**. **Geheugen RSS** wordt alleen ondersteund voor Kubernetes versie 1.8 en hoger. Anders wordt het weergeven van waarden voor **Min&nbsp; %**  als *NaN&nbsp;%*, dit is een waarde voor het type van numerieke gegevens die een niet-gedefinieerde vertegenwoordigt of Sjabloontaal waarde. 

![Prestatieweergave container-knooppunten](./media/monitoring-container-health/container-health-node-metric-dropdown.png)

Standaard prestatiegegevens is gebaseerd op de afgelopen zes uur, maar u kunt het venster wijzigen met behulp van de **tijdsbereik** vervolgkeuzelijst in de rechterbovenhoek. Op dit moment is de pagina wordt niet automatisch vernieuwen, dus u moet dit handmatig vernieuwen. U kunt ook de resultaten in het tijdsbereik filteren door te selecteren **Avg**, **Min**, **Max**, **50e**, **90e**, en **95th** in de lijst met percentiel. 

![Percentiel selectie voor het filteren van gegevens](./media/monitoring-container-health/container-health-metric-percentile-filter.png)

Noteer in het volgende voorbeeld voor knooppunt *aks-nodepool-3977305*, de waarde voor **Containers** is 5, dit is een totaliseren van het totale aantal containers die zijn geïmplementeerd.

![Totaliseren van containers per knooppunt voorbeeld](./media/monitoring-container-health/container-health-nodes-containerstotal.png)

U kunt snel bepalen of u een goede balans tussen containers tussen knooppunten in uw cluster hebt. 

De informatie die wordt weergegeven wanneer u knooppunten weergeven wordt in de volgende tabel beschreven:

| Kolom | Beschrijving | 
|--------|-------------|
| Naam | De naam van de host. |
| Status | Kubernetes-weergave van de status van het knooppunt. |
| Gem.&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50e&nbsp;%, 90e&nbsp;% | Gemiddelde knooppunt percentage is gebaseerd op percentiel tijdens de geselecteerde periode. |
| Avg, Min, Max, 50e, 90e | Gemiddelde knooppunten werkelijke waarde op basis van percentiel gedurende deze tijdsduur geselecteerd. De gemiddelde waarde wordt gemeten vanuit de CPU/geheugen die is ingesteld voor een knooppunt. het is de avg-waarde die wordt gerapporteerd door de host voor schillen en containers. |
| Containers | Het aantal containers. |
| Actieve tijdsduur | Geeft de tijd sinds een knooppunt is gestart of opnieuw is opgestart. |
| Controllers | Alleen voor containers en schillen. Hier ziet u welke domeincontroller is die zich bevinden in. Niet alle schillen zijn in een domeincontroller, zodat sommige mogelijk weergegeven **N.V.T**. | 
| Gem. trend&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50e&nbsp;%, 90e&nbsp;% | Trend van staafdiagram het percentiel metrische percentage van de controller wordt gepresenteerd. |


Selecteer in de lijst met **Controllers**.

![Selecteer controllers weergeven](./media/monitoring-container-health/container-health-controllers-tab.png)

Hier vindt u de prestatiestatus van uw domeincontrollers.

![< naam > controllers Prestatieweergave](./media/monitoring-container-health/container-health-controllers-view.png)

De hiërarchie van de rij wordt gestart met een domeincontroller en breidt de controller. U weergeven een of meer containers. Vouw een schil en de laatste rij geeft de container de schil gegroepeerd. 

De informatie die wordt weergegeven wanneer u de controllers weergeven wordt in de volgende tabel beschreven:

| Kolom | Beschrijving | 
|--------|-------------|
| Naam | De naam van de controller.|
| Status | De status totaliseren van de containers is voltooid met met de status, zoals *OK*, *beëindigd*, *mislukt* *gestopt*, of *Onderbroken*. Als de container wordt uitgevoerd, maar de status is niet correct weergegeven of niet is doorgevoerd door de agent als meer dan dertig minuten niet heeft gereageerd, de status is *onbekende*. Aanvullende details van de statuspictogram zijn opgegeven in de onderstaande tabel.|
| Gem.&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50e&nbsp;%, 90e&nbsp;% | Totaliseren gemiddelde van het gemiddelde percentage van elke entiteit voor de geselecteerde metrische gegevens en percentiel. |
| Avg, Min, Max, 50e, 90e  | Totaliseren van de gemiddelde CPU millicore of geheugen prestaties van de container voor het geselecteerde percentiel. De gemiddelde waarde wordt van de CPU/geheugen die is ingesteld voor een schil gemeten. |
| Containers | Totaal aantal containers voor de controller of de schil. |
| Opnieuw opstarten | Totaliseren van de telling opnieuw starten van containers. |
| Actieve tijdsduur | Geeft de tijd sinds een container is gestart. |
| Knooppunt | Alleen voor containers en schillen. Hier ziet u welke domeincontroller dat deze zich bevindt. | 
| Gem. trend&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50e&nbsp;%, 90e&nbsp;%| Staafdiagram trend voor percentiel metrische gegevens van de controller. |

De pictogrammen in het statusveld geven aan de online status van de containers:
 
| Pictogram | Status | 
|--------|-------------|
| ![Pictogram voor actieve status gereed](./media/monitoring-container-health/container-health-ready-icon.png) | Wordt uitgevoerd (klaar)|
| ![In afwachting van ' of ' onderbroken statuspictogram](./media/monitoring-container-health/container-health-waiting-icon.png) | In afwachting van ' of ' onderbroken|
| ![De laatste keer doorgegeven met statuspictogram](./media/monitoring-container-health/container-health-grey-icon.png) | Laatst gerapporteerd uitgevoerd maar meer dan 30 minuten nog niet is gereageerd|
| ![Geslaagde statuspictogram](./media/monitoring-container-health/container-health-green-icon.png) | Is gestopt of niet stoppen|

De van het statuspictogram wordt weergegeven een aantal op basis van wat de schil biedt. De slechtste twee statussen worden weergegeven, en wanneer u de muisaanwijzer over de status, status totaliseren uit alle schillen wordt weergegeven in de container. Als er geen een status gereed heeft, wordt de statuswaarde weergegeven **(0)**. 

Selecteer in de lijst met **Containers**.

![Selecteer containers weergeven](./media/monitoring-container-health/container-health-containers-tab.png)

Hier vindt u de prestatiestatus van uw containers.

![< naam > controllers Prestatieweergave](./media/monitoring-container-health/container-health-containers-view.png)

De informatie die wordt weergegeven wanneer u containers weergeven wordt in de volgende tabel beschreven:

| Kolom | Beschrijving | 
|--------|-------------|
| Naam | De naam van de controller.|
| Status | De status van de containers, indien van toepassing. Meer informatie over het statuspictogram vindt u in de volgende tabel.|
| Gem.&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50e&nbsp;%, 90e&nbsp;% | De implementatie-up van het gemiddelde percentage van elke entiteit voor de geselecteerde metrische gegevens en percentiel. |
| Avg, Min, Max, 50e, 90e  | Het totaliseren van de gemiddelde CPU millicore of geheugen prestaties van de container voor het geselecteerde percentiel. De gemiddelde waarde wordt van de CPU/geheugen die is ingesteld voor een schil gemeten. |
| Pod | De container waarin de schil zich bevindt.| 
| Knooppunt |  Het knooppunt waarin de container zich bevindt. | 
| Opnieuw opstarten | Geeft de tijd sinds een container is gestart. |
| Actieve tijdsduur | Geeft de tijd sinds een container is gestart of opnieuw opgestart. |
| Gem. trend&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50e&nbsp;%, 90e&nbsp;% | Een trend van het type staafdiagram dat de gemiddelde metrische percentage van de container vertegenwoordigt. |

De pictogrammen in het statusveld geven aan de online status van schillen, zoals beschreven in de volgende tabel:
 
| Pictogram | Status | 
|--------|-------------|
| ![Pictogram voor actieve status gereed](./media/monitoring-container-health/container-health-ready-icon.png) | Wordt uitgevoerd (klaar)|
| ![In afwachting van ' of ' onderbroken statuspictogram](./media/monitoring-container-health/container-health-waiting-icon.png) | In afwachting van ' of ' onderbroken|
| ![De laatste keer doorgegeven met statuspictogram](./media/monitoring-container-health/container-health-grey-icon.png) | Laatst uitgevoerd gemeld, maar nog niet is gereageerd in meer dan 30 minuten|
| ![Beëindigde statuspictogram](./media/monitoring-container-health/container-health-terminated-icon.png) | Is gestopt of niet stoppen|
| ![Pictogram van de status mislukt](./media/monitoring-container-health/container-health-failed-icon.png) | Status mislukt |

## <a name="container-data-collection-details"></a>Details van de gegevensverzameling container
Containerstatus verzamelt verschillende metrische gegevens en logboekbestanden prestatiegegevens van de hosts van de container en containers. Gegevens worden verzameld om de drie minuten.

### <a name="container-records"></a>Container-records

Voorbeelden van records die worden verzameld door de containerstatus en de gegevenstypen die worden weergegeven in de resultaten van de logboekzoekopdracht worden weergegeven in de volgende tabel:

| Gegevenstype | Het gegevenstype in zoeken in Logboeken | Velden |
| --- | --- | --- |
| Prestaties voor hosts en -containers | `Perf` | Computer, ObjectName, CounterName &#40;% processortijd, schijf leest MB, schijf schrijft MB, gebruik van geheugen (MB), netwerk ontvangen Bytes, netwerk verzenden Bytes, Processor gebruik seconde, netwerk&#41;, CounterValue, TimeGenerated, itempad, SourceSystem |
| Container-inventaris | `ContainerInventory` | TimeGenerated, de Computer, de containernaam, ContainerHostname, afbeelding, ImageTag, ContainerState, ExitCode, EnvironmentVar, opdracht, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Inventarisatie van container-installatiekopie | `ContainerImageInventory` | TimeGenerated, Computer, afbeelding, ImageTag, ImageSize, VirtualSize, die wordt uitgevoerd, is onderbroken, gestopt, is mislukt, SourceSystem, ImageID, TotalContainer |
| Container-logboek | `ContainerLog` | TimeGenerated, Computer, afbeeldings-ID, containernaam van de, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Container service-logboek | `ContainerServiceLog`  | TimeGenerated, Computer, TimeOfCommand, afbeelding, opdracht, SourceSystem, ContainerID |
| Inventaris van containerknooppunten | `ContainerNodeInventory_CL`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Containerverwerking | `ContainerProcess_CL` | TimeGenerated, Computer, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Inventaris van schillen in een Kubernetes-cluster | `KubePodInventory` | TimeGenerated, Computer, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, servicenaam, ControllerKind, controllernaam, ContainerStatus, ContainerID, ContainerName, naam, PodLabel, Namespace, PodStatus, ClusterName, PodIp, SourceSystem |
| Inventarisatie van een deel van de knooppunten van een Kubernetes-cluster | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, ClusterId, LastTransitionTimeReady, Labels, Status, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Kubernetes-gebeurtenissen | `KubeEvents_CL` | TimeGenerated, Computer, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, bericht, SourceSystem | 
| Services in het Kubernetes-cluster | `KubeServices_CL` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, SourceSystem | 
| Metrische gegevens voor prestaties voor een deel van de knooppunten van het Kubernetes-cluster | Perf &#124; waarbij ObjectName == "K8SNode" | Computer, ObjectName, CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, networkRxBytes, networkTxBytes, restartTimeEpoch, networkRxBytesPerSec, networkTxBytesPerSec, cpuAllocatableNanoCores, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes&#41;, CounterValue, TimeGenerated, itempad, SourceSystem | 
| Metrische gegevens voor prestaties voor containers die deel uitmaken van het Kubernetes-cluster | Perf &#124; waarbij ObjectName == "K8SContainer" | CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, restartTimeEpoch, cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryLimitBytes&#41;, CounterValue, TimeGenerated, itempad, SourceSystem | 

## <a name="search-logs-to-analyze-data"></a>Zoeken in Logboeken om gegevens te analyseren
Log Analytics kunt u zoeken naar bestedingstrends en een diagnose stellen van knelpunten, prognose, of het correleren gegevens die u kan helpen te bepalen of de huidige configuratie van het cluster optimaal presteert. Vooraf gedefinieerde zoekopdrachten in Logboeken vindt u onmiddellijk kunt beginnen met of om aan te passen als u wilt de gegevens van de manier waarop die u wilt retourneren. 

U kunt interactieve analyses van gegevens in de werkruimte uitvoeren door het selecteren van de **logboek weergeven** beschikbaar helemaal rechts wanneer u een domeincontroller of de container. De **zoeken in logboeken** pagina wordt weergegeven boven de Azure portal-pagina die u al had geopend.

![Gegevens analyseren in Log Analytics](./media/monitoring-container-health/container-health-view-logs.png)   

De uitvoer van de container-logboeken die wordt doorgestuurd naar Log Analytics zijn STDOUT en STDERR. Omdat de containerstatus wordt bewaakt door Azure beheerde Kubernetes (AKS), Kube-systeem niet vandaag verzameld vanwege het grote aantal gegenereerde gegevens. 

### <a name="example-log-search-queries"></a>Voorbeeld van de logboekbestanden zoekquery 's
Vaak is het handig om te maken van query's die beginnen met een voorbeeld of twee en vervolgens kan aanpassen aan uw behoeften. Om te helpen meer geavanceerde query's, kunt u experimenteren met de volgende voorbeeldquery's:

| Query’s uitvoeren | Beschrijving | 
|-------|-------------|
| ContainerInventory<br> &#124;Computer, de naam, afbeelding, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime project<br> &#124;tabel weergeven | Lijst met alle van de container levenscyclus van gegevens| 
| KubeEvents_CL<br> &#124;waar not(isempty(Namespace_s))<br> &#124;sorteren op TimeGenerated desc<br> &#124;tabel weergeven | Kubernetes-gebeurtenissen|
| ContainerImageInventory<br> &#124;summarize AggregatedValue = count() by afbeelding, ImageTag, actief | Voorraad | 
| **Selecteer in de Advanced Analytics lijndiagrammen**:<br> Voor prestaties<br> &#124;waarbij ObjectName == 'Container' en CounterName == "% processortijd"<br> &#124;samenvatten AvgCPUPercent avg(CounterValue) door bin (TimeGenerated, 30 min.), InstanceName = | Container CPU | 
| **Selecteer in de Advanced Analytics lijndiagrammen**:<br> Perf &#124; waarbij ObjectName == 'Container' en CounterName == "MB geheugen gebruik"<br> &#124;samenvatten AvgUsedMemory avg(CounterValue) door bin (TimeGenerated, 30 min.), InstanceName = | Container-geheugen |

## <a name="how-to-stop-monitoring-with-container-health"></a>Bewaking met de containerstatus stoppen
Als u inschakelt nadat de bewaking van uw AKS-container, u besluit u niet meer wilt bewaken, kunt u *opt-out* met behulp van de opgegeven Azure Resource Manager-sjablonen met de PowerShell-cmdlet  **Nieuwe-AzureRmResourceGroupDeployment** of de Azure CLI. Een JSON-sjabloon Hiermee geeft u de configuratie van *opt-out*. De andere bevat parameterwaarden die u configureert om op te geven van de AKS-cluster-ID en resource resourcegroep die het cluster is geïmplementeerd in. 

Als u niet bekend met het concept bent van het implementeren van resources met behulp van een sjabloon, Zie:
* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Resources implementeren met Resource Manager-sjablonen en Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)

Als u ervoor de Azure CLI gebruiken kiest, moet u eerst installeren en de CLI lokaal gebruikt. U moet worden uitgevoerd van Azure CLI versie 2.0.27 of hoger. Voor het identificeren van uw versie uitvoeren `az --version`. Als u wilt installeren of upgraden van de Azure CLI, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli). 

### <a name="create-and-execute-template"></a>Maken en uitvoeren van sjabloon

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
              "config": null
            }
           }
         }
       }
      ]
    }
    ```

2. Sla dit bestand als **OptOutTemplate.json** naar een lokale map.
3. Plak de volgende JSON-syntaxis in het bestand:

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

4. Bewerk de waarden voor **aksResourceId** en **aksResourceLocation** met behulp van de waarden van het AKS-cluster dat u kunt vinden op de **eigenschappen** pagina voor het geselecteerde cluster .

    ![De eigenschappenpagina van container](./media/monitoring-container-health/container-properties-page.png)

    Als u zich op de **eigenschappen** pagina, kopieert u ook de **Resource-ID van werkruimte**. Deze waarde is vereist als u besluit dat u wilt verwijderen van de Log Analytics-werkruimte later opnieuw. Verwijderen van de Log Analytics-werkruimte wordt niet uitgevoerd als onderdeel van dit proces. 

5. Sla dit bestand als **OptOutParam.json** naar een lokale map.
6. U kunt deze sjabloon nu implementeren. 

    * Gebruik de volgende PowerShell-opdrachten in de map met de sjabloon:

        ```powershell
        Connect-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
        New-AzureRmResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
        ```

        Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid, wordt een bericht dat lijkt op de volgende mogelijkheden van het resultaat geretourneerd:

        ```powershell
        ProvisioningState       : Succeeded
        ```

    * De volgende opdracht uit met Azure CLI uitvoeren op Linux:

        ```azurecli
        az login   
        az account set --subscription "Subscription Name" 
        az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
        ```

        Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid, wordt een bericht dat lijkt op de volgende mogelijkheden van het resultaat geretourneerd:

        ```azurecli
        ProvisioningState       : Succeeded
        ```

Als de werkruimte alleen ter ondersteuning van de bewaking van het cluster is gemaakt en deze niet meer nodig hebt, hebt u deze handmatig te verwijderen. Als u niet bekend bent met het verwijderen van een werkruimte, Zie [verwijderen van een Azure Log Analytics-werkruimte met de Azure-portal](../log-analytics/log-analytics-manage-del-workspace.md). Vergeet niet over de **Resource-ID van werkruimte** we eerder in stap 4 hebt gekopieerd, gaat u die nodig hebt. 

## <a name="troubleshooting"></a>Problemen oplossen
Deze sectie bevat informatie over het oplossen van problemen met de containerstatus.

Als containerstatus is ingeschakeld en geconfigureerd, maar u niet de van statusgegevens of resultaten in Log Analytics wanneer u een zoekopdracht in Logboeken uitvoert, kunt u helpen het probleem vaststellen door het volgende te doen: 

1. Controleer de status van de agent door het uitvoeren van de volgende opdracht uit: 

    `kubectl get ds omsagent --namespace=kube-system`

    De uitvoer moet eruitzien zoals in het volgende, waarmee wordt aangegeven dat deze correct is geïmplementeerd:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Controleer de implementatiestatus van de oplossing met agentversie *06072018* of hoger met de volgende opdracht:

    `kubectl get deployment omsagent-rs -n=kube-system`

    De uitvoer moet eruitzien zoals in het volgende, waarmee wordt aangegeven dat deze correct is geïmplementeerd:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Controleer de status van de schil om te controleren of deze wordt uitgevoerd door die wordt uitgevoerd de volgende opdracht uit: `kubectl get pods --namespace=kube-system`

    De uitvoer is vergelijkbaar met het volgende met de status van *met* voor de omsagent:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

4. Raadpleeg de logboeken van de agent. Wanneer de agent in containers wordt geïmplementeerd, wordt een snelle controle uitgevoerd door het OMI-opdrachten uitvoeren en de versie van de agent en de provider weergeven. 

5. Om te controleren dat de agent opnieuw toegevoegd is, moet u de volgende opdracht uitvoeren: `kubectl logs omsagent-484hw --namespace=kube-system`

    De status moet lijken op het volgende:

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

Als u gedetailleerde container status en toepassing prestaties informatie, Zie [zoeken in logboeken](../log-analytics/log-analytics-log-search.md). 
