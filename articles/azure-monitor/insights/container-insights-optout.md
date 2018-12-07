---
title: Hoe Stop bewaking Your Azure Kubernetes Service-cluster | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt niet meer controleren van uw Azure-AKS-cluster met Azure Monitor voor containers.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/04/2018
ms.author: magoedte
ms.openlocfilehash: 1df4e345645fb216f1656b2bbe87396cf0719d1c
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52994985"
---
# <a name="how-to-stop-monitoring-your-azure-kubernetes-service-aks-with-azure-monitor-for-containers-preview"></a>Stoppen met het controleren van uw Azure Kubernetes Service (AKS) met Azure Monitor voor containers (Preview)

Als u inschakelt nadat de bewaking van uw AKS-cluster, u besluit u niet meer wilt bewaken, kunt u *opt-out*.  Dit artikel wordt beschreven hoe u dit doen met de Azure CLI of met de opgegeven Azure Resource Manager-sjablonen.  


## <a name="azure-cli"></a>Azure-CLI
Gebruik de [az aks disable--invoegtoepassingen](https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-disable-addons) opdracht voor het uitschakelen van Azure Monitor voor containers. De opdracht wordt de agent verwijderd van de clusterknooppunten, niet de oplossing of de gegevens die al zijn verzameld en opgeslagen in uw Log Analytics-resource wordt verwijderd.  

```azurecli
az aks disable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG
```

Opnieuw inschakelen van bewaking voor uw cluster [controle inschakelen met Azure CLI](container-insights-onboard.md#enable-monitoring-using-azure-cli).

## <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon
Gegeven worden twee Azure Resource Manager-sjabloon voor de ondersteuning van de resources van de oplossing consistent en herhaaldelijk in de resourcegroep te verwijderen. Een is een JSON-sjabloon op te geven van de configuratie van *opt-out* en de andere bevat parameterwaarden die u configureert om op te geven van de AKS-cluster-ID en resource resourcegroep die het cluster is geïmplementeerd in. 

Als u niet bekend met het concept bent van het implementeren van resources met behulp van een sjabloon, Zie:
* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Resources implementeren met Resource Manager-sjablonen en Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md)

>[!NOTE]
>De sjabloon opnieuw moet worden geïmplementeerd in dezelfde resourcegroep bevinden als het cluster.
>

Als u ervoor de Azure CLI gebruiken kiest, moet u eerst installeren en de CLI lokaal gebruikt. U moet worden uitgevoerd van Azure CLI versie 2.0.27 of hoger. Voor het identificeren van uw versie uitvoeren `az --version`. Als u wilt installeren of upgraden van de Azure CLI, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli). 

### <a name="create-template"></a>Sjabloon maken

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

    ![De eigenschappenpagina van container](media/container-insights-optout/container-properties-page.png)

    Als u zich op de **eigenschappen** pagina, kopieert u ook de **Resource-ID van werkruimte**. Deze waarde is vereist als u besluit dat u wilt verwijderen van de Log Analytics-werkruimte later opnieuw. Verwijderen van de Log Analytics-werkruimte wordt niet uitgevoerd als onderdeel van dit proces. 

5. Sla dit bestand als **OptOutParam.json** naar een lokale map.
6. U kunt deze sjabloon nu implementeren. 

### <a name="remove-the-solution-using-azure-cli"></a>Verwijderen van de oplossing met behulp van Azure CLI
De volgende opdracht uit met Azure CLI op Linux te verwijderen van de oplossing en het opschonen van de configuratie van uw AKS-cluster worden uitgevoerd.

```azurecli
az login   
az account set --subscription "Subscription Name" 
az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
```

Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid, wordt een bericht dat lijkt op de volgende mogelijkheden van het resultaat geretourneerd:

```azurecli
ProvisioningState       : Succeeded
```

### <a name="remove-the-solution-using-powershell"></a>Verwijderen van de oplossing met behulp van PowerShell

Voer de volgende PowerShell-opdrachten in de map met de sjabloon voor het verwijderen van de oplossing en het opschonen van de configuratie van uw AKS-cluster.    

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
New-AzureRmResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
```

Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid, wordt een bericht dat lijkt op de volgende mogelijkheden van het resultaat geretourneerd:

```powershell
ProvisioningState       : Succeeded
```

Als de werkruimte alleen ter ondersteuning van de bewaking van het cluster is gemaakt en deze niet meer nodig hebt, hebt u deze handmatig te verwijderen. Als u niet bekend bent met het verwijderen van een werkruimte, Zie [verwijderen van een Azure Log Analytics-werkruimte met de Azure-portal](../../azure-monitor/platform/delete-workspace.md). Vergeet niet over de **Resource-ID van werkruimte** we eerder in stap 4 hebt gekopieerd, gaat u die nodig hebt. 

