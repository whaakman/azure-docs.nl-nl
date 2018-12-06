---
title: Hoe u ingebouwde Azure-Monitor voor containers (Preview) | Microsoft Docs
description: Dit artikel wordt beschreven hoe u met het voorbereiden en configureren van Azure Monitor voor containers, zodat u begrijpt hoe de container wordt uitgevoerd en welke problemen met betrekking tot prestaties zijn geïdentificeerd.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/05/2018
ms.author: magoedte
ms.openlocfilehash: 4747c06ddb56a86c2efc7340043efdd019b86049
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52962906"
---
# <a name="how-to-onboard-azure-monitor-for-containers-preview"></a>Hoe u ingebouwde Azure-Monitor voor containers (Preview) 
In dit artikel wordt beschreven hoe u voor het instellen van Azure Monitor voor containers voor het bewaken van de prestaties van workloads die worden geïmplementeerd in Kubernetes-omgevingen en die worden gehost op [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/).

Azure Monitor voor containers kan worden ingeschakeld voor nieuwe implementaties van AKS met behulp van de volgende ondersteunde methodes:

* Een beheerde Kubernetes-cluster in Azure portal of met Azure CLI implementeren
* Het maken van een Kubernetes-cluster met [Terraform en AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md)

U kunt ook inschakelen voor bewaking voor een of meer bestaande AKS-clusters in Azure portal of met Azure CLI. 

## <a name="prerequisites"></a>Vereisten 
Voordat u begint, zorg ervoor dat u het volgende hebt:

- Een Log Analytics-werkruimte. Als u bewaking van uw nieuwe AKS-cluster of laat het onboarding-ervaring een standaardwerkruimte maken in de standaard-resourcegroep van het AKS-cluster-abonnement, kunt u deze maken. Als u wilt deze zelf maken, kunt u het maken via [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md), tot en met [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), of in de [Azure-portal](../../azure-monitor/learn/quick-create-workspace.md).
- U bent lid van de inzendersrol van Log Analytics voor containerbewaking inschakelen. Zie voor meer informatie over het beheren van toegang tot een Log Analytics-werkruimte [werkruimten beheren](../../log-analytics/log-analytics-manage-access.md).

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

## <a name="components"></a>Onderdelen 

De mogelijkheid om prestaties te bewaken, is afhankelijk van een beperkte Log Analytics-agent voor Linux, welke prestaties en gebeurtenisgegevens worden verzameld van alle knooppunten in het cluster. De agent is geregistreerd met de opgegeven Log Analytics-werkruimte nadat u Azure Monitor voor containers inschakelen en automatisch geïmplementeerd. De versie die is geïmplementeerd, is microsoft / oms:ciprod04202018 of hoger en wordt vertegenwoordigd door een datum in de volgende indeling: *mmddyyyy*. 

>[!NOTE] 
>Als u al een AKS-cluster hebt geïmplementeerd, kunt u inschakelen bewaking met behulp van Azure CLI of een opgegeven Azure Resource Manager-sjabloon, zoals verderop in dit artikel wordt gedemonstreerd. U kunt geen gebruiken `kubectl` als u wilt bijwerken, verwijderen, opnieuw implementeren of implementeren van de agent. De sjabloon moet worden geïmplementeerd in dezelfde resourcegroep bevinden als het cluster."

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij [Azure Portal](https://portal.azure.com). 

## <a name="enable-monitoring-for-a-new-cluster"></a>Schakel bewaking voor een nieuw cluster
Tijdens de implementatie, kunt u de bewaking van een nieuw AKS-cluster in Azure portal, Azure CLI of met Terraform inschakelen.  Volg de stappen in dit artikel [een Azure Kubernetes Service (AKS)-cluster implementeren](../../aks/kubernetes-walkthrough-portal.md) als u wilt inschakelen vanuit de portal. Op de **bewaking** pagina voor de **bewaking inschakelen** optie, selecteer **Ja**, en selecteer een bestaande Log Analytics-werkruimte of maak een nieuwe. 

Volg de stappen in dit artikel in de sectie voor bewaking van een nieuw AKS-cluster gemaakt met Azure CLI, [maken-AKS-cluster](../../aks/kubernetes-walkthrough.md#create-aks-cluster).  

>[!NOTE]
>Als u ervoor de Azure CLI gebruiken kiest, moet u eerst installeren en de CLI lokaal gebruikt. U moet worden uitgevoerd van Azure CLI versie 2.0.43 of hoger. Voor het identificeren van uw versie uitvoeren `az --version`. Als u wilt installeren of upgraden van de Azure CLI, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli). 
>

Als u [implementeren van een AKS-cluster met behulp van Terraform](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md), u kunt ook Azure Monitor voor containers inschakelen door het argument [ **addon_profile** ](https://www.terraform.io/docs/providers/azurerm/r/kubernetes_cluster.html#addon_profile) en optegeven**oms_agent**.  

Nadat u bewaking hebt ingeschakeld, en alle configuratietaken zijn voltooid, kunt u de prestaties van uw cluster op twee manieren controleren:

* Rechtstreeks in het AKS-cluster door te selecteren **Health** in het linkerdeelvenster.
* Door het selecteren van de **Monitor Container insights** tegel op de pagina van de AKS-cluster voor het geselecteerde cluster. In Azure Monitor in het linkerdeelvenster selecteert **Health**. 

  ![Opties voor het selecteren van Azure Monitor voor containers in AKS](./media/container-insights-onboard/kubernetes-select-monitoring-01.png)

Wanneer u bewaking inschakelt, is het duurt ongeveer 15 minuten voordat u de gezondheid van metrische gegevens voor het cluster kunt weergeven. 

## <a name="enable-monitoring-for-existing-managed-clusters"></a>Schakel de bewaking van bestaande beheerde clusters
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

Als u in plaats daarvan met een bestaande werkruimte integreren zou, gebruikt u de volgende opdracht uit om op te geven die werkruimte.

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG --workspace-resource-id <ExistingWorkspaceResourceID> 
```

De uitvoer ziet eruit als in het volgende:

```azurecli
provisioningState       : Succeeded
```

### <a name="enable-monitoring-from-azure-monitor"></a>Schakel de bewaking van Azure Monitor
Als u wilt inschakelen voor bewaking van uw AKS-cluster in Azure portal van Azure Monitor, het volgende doen:

1. Selecteer in de Azure portal, **Monitor**. 
2. Selecteer **Containers (preview)** in de lijst.
3. Op de **Monitor - containers (preview)** weergeeft, schakelt **clusters niet bewaakt**.
4. In de lijst met clusters niet-bewaakt, de container niet vinden in de lijst en klik op **inschakelen**.   
5. Op de **Onboarding naar containerstatus en logboeken** pagina, hebt u een bestaande Log Analytics-werkruimte in hetzelfde abonnement bevinden als het cluster, selecteert u deze in de vervolgkeuzelijst.  
    De lijst worden er de standaardwerkruimte en de locatie die het AKS-container is geïmplementeerd op in het abonnement. 

    ![AKS-Container inzichten, bewaking inschakelen](./media/container-insights-onboard/kubernetes-onboard-brownfield-01.png)

    >[!NOTE]
    >Als u maken van een nieuwe werkruimte voor logboekanalyse wilt voor het opslaan van de gegevens uit het cluster, volg de instructies in [een Log Analytics-werkruimte maken](../../azure-monitor/learn/quick-create-workspace.md). Zorg ervoor dat de werkruimte maakt in hetzelfde abonnement dat voor de AKS-container is geïmplementeerd. 
 
Wanneer u bewaking inschakelt, is het duurt ongeveer 15 minuten voordat u de gezondheid van metrische gegevens voor het cluster kunt weergeven. 

### <a name="enable-monitoring-from-aks-cluster-in-the-portal"></a>Schakel de bewaking van AKS-cluster in de portal
Als u wilt inschakelen voor bewaking van uw AKS-container in Azure portal, het volgende doen:

1. Selecteer in de Azure-portal de optie **Alle services**. 
2. Begin met typen in de lijst met resources **Containers**.  
    De lijst gefilterd op basis van uw invoer. 
3. Selecteer **Kubernetes-services**.  

    ![De koppeling van Kubernetes-services](./media/container-insights-onboard/portal-search-containers-01.png)

4. Selecteer een container in de lijst met containers.
5. Selecteer op de overzichtspagina van container **containerstatus bewaken**.  
6. Op de **Onboarding naar containerstatus en logboeken** pagina, hebt u een bestaande Log Analytics-werkruimte in hetzelfde abonnement bevinden als het cluster, selecteert u deze in de vervolgkeuzelijst.  
    De lijst worden er de standaardwerkruimte en de locatie die het AKS-container is geïmplementeerd op in het abonnement. 

    ![Statuscontrole van AKS container inschakelen](./media/container-insights-onboard/kubernetes-onboard-brownfield-02.png)

    >[!NOTE]
    >Als u maken van een nieuwe werkruimte voor logboekanalyse wilt voor het opslaan van de gegevens uit het cluster, volg de instructies in [een Log Analytics-werkruimte maken](../../azure-monitor/learn/quick-create-workspace.md). Zorg ervoor dat de werkruimte maakt in hetzelfde abonnement dat voor de AKS-container is geïmplementeerd. 
 
Wanneer u bewaking inschakelt, is het duurt ongeveer 15 minuten voordat u de operationele gegevens voor het cluster kunt weergeven. 

### <a name="enable-monitoring-by-using-an-azure-resource-manager-template"></a>Schakel de bewaking met behulp van een Azure Resource Manager-sjabloon
Deze methode omvat twee JSON-sjablonen. Een sjabloon Hiermee geeft u de configuratie voor bewaking en de andere bevat parameterwaarden die u configureert voor het volgende opgeven:

* De AKS container resource-ID. 
* De resourcegroep die in het cluster is geïmplementeerd.
* De Log Analytics-werkruimte en de regio voor het maken van de werkruimte in. 

>[!NOTE]
>De sjabloon opnieuw moet worden geïmplementeerd in dezelfde resourcegroep bevinden als het cluster.
>

De Log Analytics-werkruimte heeft handmatig worden gemaakt. Voor het maken van de werkruimte, u kunt dit instellen via [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md), tot en met [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), of in de [Azure-portal](../../azure-monitor/learn/quick-create-workspace.md).

Als u niet bekend met het concept bent van het implementeren van resources met behulp van een sjabloon, Zie:
* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Resources implementeren met Resource Manager-sjablonen en Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md)

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
        "name": "[Concat('ContainerInsights', '-',  uniqueString(parameters('workspaceResourceId')))]", 
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
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aksResourceId": {
           "value": "/subscriptions/<SubscriptiopnId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
       },
       "aksResourceLocation": {
         "value": "<aksClusterLocation>"
       },
       "workspaceResourceId": {
         "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
       },
       "workspaceRegion": {
         "value": "<workspaceLocation>"
       }
     }
    }
    ```

4. Bewerk de waarden voor **aksResourceId** en **aksResourceLocation** met behulp van de waarden op de **overzicht van AKS** -pagina voor het AKS-cluster. De waarde voor **workspaceResourceId** is de volledige resource-ID van uw Log Analytics-werkruimte, waaronder de naam van de werkruimte. Geef ook de locatie van de werkruimte voor **workspaceRegion**. 
5. Sla dit bestand als **existingClusterParam.json** naar een lokale map.
6. U kunt deze sjabloon nu implementeren. 

    * Gebruik de volgende PowerShell-opdrachten in de map met de sjabloon:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ClusterResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid, wordt er een bericht weergegeven dat vergelijkbaar is met het volgende en het resultaat bevat:

        ```powershell
        provisioningState       : Succeeded
        ```

    * De volgende opdracht uitvoeren met behulp van de Azure CLI:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid, wordt er een bericht weergegeven dat vergelijkbaar is met het volgende en het resultaat bevat:

        ```azurecli
        provisioningState       : Succeeded
        ```
Wanneer u bewaking inschakelt, is het duurt ongeveer 15 minuten voordat u de gezondheid van metrische gegevens voor het cluster kunt weergeven. 

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

Om te controleren dat de versie van de Log Analytics-agent vóór uitgebracht *06072018* correct is geïmplementeerd met de volgende opdracht uitvoeren:  

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
## <a name="next-steps"></a>Volgende stappen

* Als u problemen ondervindt tijdens een poging voor de Onboarding van de oplossing, raadpleegt u de [problemen oplossen met](container-insights-troubleshoot.md)

* Deze metrische gegevens voor servicestatus zijn met bewaking ingeschakeld om vast te leggen van de gezondheid van metrische gegevens voor de AKS-clusterknooppunten en de schillen, beschikbaar in de Azure-portal. Zie voor meer informatie over het gebruik van Azure Monitor voor containers, [weergave Azure Kubernetes Service health](container-insights-analyze.md).