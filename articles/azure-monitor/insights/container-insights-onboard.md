---
title: Hoe u ingebouwde Azure-Monitor voor containers | Microsoft Docs
description: Dit artikel wordt beschreven hoe u met het voorbereiden en configureren van Azure Monitor voor containers, zodat u begrijpt hoe de container wordt uitgevoerd en welke problemen met betrekking tot prestaties zijn geïdentificeerd.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2019
ms.author: magoedte
ms.openlocfilehash: 403cbeb0a68e39eab714ceb428fcfaefe8de0ff7
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58576238"
---
# <a name="how-to-onboard-azure-monitor-for-containers"></a>Hoe u ingebouwde Azure-Monitor voor containers  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

In dit artikel wordt beschreven hoe u voor het instellen van Azure Monitor voor containers voor het bewaken van de prestaties van workloads die worden geïmplementeerd in Kubernetes-omgevingen en die worden gehost op [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/).

Azure Monitor voor containers kan worden ingeschakeld voor nieuwe of bestaande implementaties van een of meer van AKS met behulp van de volgende ondersteunde methoden:

* In de Azure portal, Azure PowerShell, of met Azure CLI
* Met behulp van [Terraform en AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md)


## <a name="prerequisites"></a>Vereisten 
Voordat u begint, zorg ervoor dat u het volgende hebt:

- **Een Log Analytics-werkruimte.** Als u bewaking van uw nieuwe AKS-cluster of laat het onboarding-ervaring een standaardwerkruimte maken in de standaard-resourcegroep van het AKS-cluster-abonnement, kunt u deze maken. Als u wilt deze zelf maken, kunt u het maken via [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md), tot en met [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json), of in de [Azure-portal](../../azure-monitor/learn/quick-create-workspace.md).
- U bent lid van de **rol van inzender van Log Analytics** containerbewaking inschakelen. Zie voor meer informatie over het beheren van toegang tot een Log Analytics-werkruimte [werkruimten beheren](../../azure-monitor/platform/manage-access.md).
- U bent lid van de **[eigenaar](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-role.mds#owner)** -rol op de AKS-cluster-bron. 

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

## <a name="components"></a>Onderdelen 

De mogelijkheid om prestaties te bewaken, is afhankelijk van een beperkte Log Analytics-agent voor Linux speciaal ontwikkeld voor Azure Monitor voor containers. Deze gespecialiseerde agent verzamelt prestaties en gebeurtenisgegevens van alle knooppunten in het cluster en de agent wordt automatisch geregistreerd en geïmplementeerd met de opgegeven Log Analytics-werkruimte tijdens de implementatie. De versie van de agent wordt door microsoft / oms:ciprod04202018 of hoger, en wordt vertegenwoordigd door een datum in de volgende indeling: *mmddyyyy*. 

Wanneer een nieuwe versie van de agent wordt uitgebracht, wordt deze automatisch bijgewerkt op uw beheerde Kubernetes-clusters die worden gehost in Azure Kubernetes Service (AKS). Als u wilt volgen de versies die zijn uitgebracht, Zie [agent aankondigingen](https://github.com/microsoft/docker-provider/tree/ci_feature_prod). 

>[!NOTE] 
>Als u al een AKS-cluster hebt geïmplementeerd, kunt u inschakelen bewaking met behulp van Azure CLI of een opgegeven Azure Resource Manager-sjabloon, zoals verderop in dit artikel wordt gedemonstreerd. U kunt geen gebruiken `kubectl` als u wilt bijwerken, verwijderen, opnieuw implementeren of implementeren van de agent. De sjabloon opnieuw moet worden geïmplementeerd in dezelfde resourcegroep bevinden als het cluster.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij [Azure Portal](https://portal.azure.com). 

## <a name="enable-monitoring-for-a-new-cluster"></a>Schakel bewaking voor een nieuw cluster
Tijdens de implementatie, kunt u de bewaking van een nieuw AKS-cluster in Azure portal, Azure CLI of met Terraform inschakelen.  Als u wilt inschakelen vanuit de portal, volgt u de stappen in dit artikel [een Azure Kubernetes Service (AKS)-cluster implementeren](../../aks/kubernetes-walkthrough-portal.md) en volg de stappen in de sectie **status en de logboeken controleren**.  

>[!NOTE]
>Wanneer u de stappen in dit artikel bewaking voor de AKS-cluster op basis van de portal wilt inschakelen, wordt u gevraagd een nieuwe maken of selecteren van een bestaande Log Analytics-werkruimte. 

### <a name="enable-using-azure-cli"></a>Inschakelen met behulp van Azure CLI
Volg de stappen in dit artikel in de sectie voor bewaking van een nieuw AKS-cluster gemaakt met Azure CLI, [maken-AKS-cluster](../../aks/kubernetes-walkthrough.md#create-aks-cluster).  

>[!NOTE]
>Als u ervoor de Azure CLI gebruiken kiest, moet u eerst installeren en de CLI lokaal gebruikt. U moet worden uitgevoerd van Azure CLI versie 2.0.59 of hoger. Voor het identificeren van uw versie uitvoeren `az --version`. Als u wilt installeren of upgraden van de Azure CLI, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli). 
>

### <a name="enable-using-terraform"></a>Inschakelen met behulp van Terraform
Als u [implementeren van een nieuw AKS-cluster met behulp van Terraform](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md), geeft u de argumenten vereist in het profiel [een Log Analytics-werkruimte maken](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_workspace.html) als u niet hebt gekozen om op te geven van een bestaande resourcegroep. 

>[!NOTE]
>Als u gebruiken, Terraform wilt, moet u uitvoeren de Terraform Azure DB-Provider versie 1.17.0 of hoger.

Azure Monitor voor containers toevoegen aan de werkruimte, Zie [azurerm_log_analytics_solution](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_solution.html) en het profiel te voltooien door de [ **addon_profile** ](https://www.terraform.io/docs/providers/azurerm/r/kubernetes_cluster.html#addon_profile) en opgeven **oms_agent**. 

Nadat u bewaking hebt ingeschakeld, en alle configuratietaken zijn voltooid, kunt u de prestaties van uw cluster op twee manieren controleren:

* Rechtstreeks in het AKS-cluster door te selecteren **Health** in het linkerdeelvenster.
* Door het selecteren van de **Monitor Container insights** tegel op de pagina van de AKS-cluster voor het geselecteerde cluster. In Azure Monitor in het linkerdeelvenster selecteert **Health**. 

  ![Opties voor het selecteren van Azure Monitor voor containers in AKS](./media/container-insights-onboard/kubernetes-select-monitoring-01.png)

Wanneer u bewaking inschakelt, is het duurt ongeveer 15 minuten voordat u de gezondheid van metrische gegevens voor het cluster kunt weergeven. 

## <a name="enable-monitoring-for-existing-managed-clusters"></a>Schakel de bewaking van bestaande beheerde clusters
U kunt inschakelen bewaking van een AKS-cluster dat al geïmplementeerd met behulp van een van de ondersteunde methoden:

* Azure-CLI
* Terraform
* [Van Azure Monitor](#enable-from-azure-monitor-in-the-portal) of [rechtstreeks vanuit het AKS-cluster](#enable-directly-from-aks-cluster-in-the-portal) in Azure portal 
* Met de [Azure Resource Manager-sjabloon opgegeven](#enable-using-an-azure-resource-manager-template) met behulp van de Azure PowerShell-cmdlet `New-AzResourceGroupDeployment` of met Azure CLI. 

### <a name="enable-using-azure-cli"></a>Inschakelen met behulp van Azure CLI
De volgende stap kunt bewaking van uw AKS-cluster met behulp van Azure CLI. In dit voorbeeld zijn u niet verplicht per maken of geef een bestaande werkruimte. Met deze opdracht vereenvoudigt het proces voor u door het maken van een standaard-werkruimte in de standaard-resourcegroep van het AKS-cluster-abonnement als deze niet al in de regio bestaat.  De standaard-werkruimte gemaakt lijkt op de indeling van *standaardwerkruimte -\<GUID >-\<regio >*.  

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

### <a name="enable-using-terraform"></a>Inschakelen met behulp van Terraform
1. Voeg de **oms_agent** invoegtoepassing profiel aan de bestaande [azurerm_kubernetes_cluster resource](https://www.terraform.io/docs/providers/azurerm/d/kubernetes_cluster.html#addon_profile)

   ```
   addon_profile {
    oms_agent {
      enabled                    = true
      log_analytics_workspace_id = "${azurerm_log_analytics_workspace.test.id}"
     }
   }
   ```

2. Voeg de [azurerm_log_analytics_solution](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_solution.html) de stappen in de documentatie van Terraform te volgen.

### <a name="enable-from-azure-monitor-in-the-portal"></a>Inschakelen van Azure Monitor in de portal 
Als u wilt inschakelen voor bewaking van uw AKS-cluster in Azure portal van Azure Monitor, het volgende doen:

1. Selecteer in de Azure portal, **Monitor**. 
2. Selecteer **Containers** in de lijst.
3. Op de **Monitor - containers** weergeeft, schakelt **clusters niet bewaakt**.
4. In de lijst met clusters niet-bewaakt, de container niet vinden in de lijst en klik op **inschakelen**.   
5. Op de **Onboarding naar Azure Monitor voor containers** pagina, hebt u een bestaande Log Analytics-werkruimte in hetzelfde abonnement bevinden als het cluster, selecteert u deze in de vervolgkeuzelijst.  
    De lijst worden er de standaardwerkruimte en de locatie die het AKS-container is geïmplementeerd op in het abonnement. 

    ![AKS-Container inzichten, bewaking inschakelen](./media/container-insights-onboard/kubernetes-onboard-brownfield-01.png)

    >[!NOTE]
    >Als u maken van een nieuwe werkruimte voor logboekanalyse wilt voor het opslaan van de gegevens uit het cluster, volg de instructies in [een Log Analytics-werkruimte maken](../../azure-monitor/learn/quick-create-workspace.md). Zorg ervoor dat de werkruimte maakt in hetzelfde abonnement dat voor de AKS-container is geïmplementeerd. 
 
Wanneer u bewaking inschakelt, is het duurt ongeveer 15 minuten voordat u de gezondheid van metrische gegevens voor het cluster kunt weergeven. 

### <a name="enable-directly-from-aks-cluster-in-the-portal"></a>Schakel rechtstreeks vanuit de AKS-cluster in de portal
Voor bewaking van een van uw AKS-clusters in Azure portal, het volgende doen:

1. Selecteer in de Azure-portal de optie **Alle services**. 
2. Begin met typen in de lijst met resources **Containers**.  
    De lijst gefilterd op basis van uw invoer. 
3. Selecteer **Kubernetes-services**.  

    ![De koppeling van Kubernetes-services](./media/container-insights-onboard/portal-search-containers-01.png)

4. Selecteer een container in de lijst met containers.
5. Selecteer op de overzichtspagina van container **Containers bewaken**.  
6. Op de **Onboarding naar Azure Monitor voor containers** pagina, hebt u een bestaande Log Analytics-werkruimte in hetzelfde abonnement bevinden als het cluster, selecteert u deze in de vervolgkeuzelijst.  
    De lijst worden er de standaardwerkruimte en de locatie die het AKS-container is geïmplementeerd op in het abonnement. 

    ![Statuscontrole van AKS container inschakelen](./media/container-insights-onboard/kubernetes-onboard-brownfield-02.png)

    >[!NOTE]
    >Als u maken van een nieuwe werkruimte voor logboekanalyse wilt voor het opslaan van de gegevens uit het cluster, volg de instructies in [een Log Analytics-werkruimte maken](../../azure-monitor/learn/quick-create-workspace.md). Zorg ervoor dat de werkruimte maakt in hetzelfde abonnement dat voor de AKS-container is geïmplementeerd. 
 
Wanneer u bewaking inschakelt, is het duurt ongeveer 15 minuten voordat u de operationele gegevens voor het cluster kunt weergeven. 

### <a name="enable-using-an-azure-resource-manager-template"></a>Inschakelen met behulp van een Azure Resource Manager-sjabloon
Deze methode omvat twee JSON-sjablonen. Een sjabloon Hiermee geeft u de configuratie voor bewaking en de andere bevat parameterwaarden die u configureert voor het volgende opgeven:

* De AKS container resource-ID. 
* De resourcegroep die in het cluster is geïmplementeerd.

>[!NOTE]
>De sjabloon opnieuw moet worden geïmplementeerd in dezelfde resourcegroep bevinden als het cluster.
>

De Log Analytics-werkruimte moet worden gemaakt voordat u controle inschakelen met Azure PowerShell of CLI. Voor het maken van de werkruimte, u kunt dit instellen via [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md), tot en met [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json), of in de [Azure-portal](../../azure-monitor/learn/quick-create-workspace.md).

Als u niet bekend met het concept bent van het implementeren van resources met behulp van een sjabloon, Zie:
* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Resources implementeren met Resource Manager-sjablonen en Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Als u ervoor de Azure CLI gebruiken kiest, moet u eerst installeren en de CLI lokaal gebruikt. U moet worden uitgevoerd van Azure CLI versie 2.0.59 of hoger. Voor het identificeren van uw versie uitvoeren `az --version`. Als u wilt installeren of upgraden van de Azure CLI, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli). 

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
           "value": "/subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
       },
       "aksResourceLocation": {
         "value": "<aksClusterLocation>"
       },
       "workspaceResourceId": {
         "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
       }  
     }
    }
    ```

4. Bewerk de waarden voor **aksResourceId** en **aksResourceLocation** met behulp van de waarden op de **overzicht van AKS** -pagina voor het AKS-cluster. De waarde voor **workspaceResourceId** is de volledige resource-ID van uw Log Analytics-werkruimte, waaronder de naam van de werkruimte. 
5. Sla dit bestand als **existingClusterParam.json** naar een lokale map.
6. U kunt deze sjabloon nu implementeren. 

   * Als u wilt implementeren met Azure PowerShell, gebruikt u de volgende opdrachten in de map waarin de sjabloon:

       ```powershell
       New-AzResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <ResourceGroupName> -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
       ```
       Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid, wordt er een bericht weergegeven dat vergelijkbaar is met het volgende en het resultaat bevat:

       ```powershell
       provisioningState       : Succeeded
       ```

   * Als u wilt implementeren met Azure CLI, voert u de volgende opdrachten:
    
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
az aks show -g <resourceGroupofAKSCluster> -n <nameofAksCluster>
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
