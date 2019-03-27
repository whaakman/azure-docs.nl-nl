---
title: Gast-OS metrische gegevens verzenden naar de Azure Monitor metrische store met behulp van een Azure Resource Manager-sjabloon voor een virtuele-machineschaalset voor Windows
description: Gast-OS metrische gegevens verzenden naar de Azure Monitor metrische store met behulp van Resource Manager-sjabloon voor een virtuele-machineschaalset voor Windows
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 573c205cd2e208a1cb2b526d96fb08ca21331c80
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58481320"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-by-using-an-azure-resource-manager-template-for-a-windows-virtual-machine-scale-set"></a>Gast-OS metrische gegevens verzenden naar de Azure Monitor metrische store met behulp van een Azure Resource Manager-sjabloon voor een virtuele-machineschaalset voor Windows

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Met behulp van de Azure Monitor [Windows Azure Diagnostics (WAD)-extensie](diagnostics-extension-overview.md), kunt u metrische gegevens en logboeken van het gastbesturingssysteem (als gast-OS) die wordt uitgevoerd als onderdeel van een virtuele machine, een cloudservice of een Azure Service Fabric-cluster verzamelen. De extensie kunt telemetrie verzenden naar verschillende locaties die worden vermeld in de eerder gekoppelde artikel.  

Dit artikel beschrijft het proces voor het verzenden Gast OS metrische gegevens voor prestaties voor een Windows virtuele-machineschaalset met het gegevensarchief van Azure Monitor. Beginnen met Windows Azure Diagnostics versie 1.11, kunt u metrische gegevens rechtstreeks naar de Azure Monitor metrische gegevens opslaan, waar al standaardplatform metrische gegevens worden verzameld. Door op te slaan ze op deze locatie, kunt u toegang tot de dezelfde acties die beschikbaar voor platform metrische gegevens zijn. Acties omvatten bijna realtime waarschuwingen, grafieken, routering, openen via de REST-API, en meer. In het verleden, wordt de Windows Azure Diagnostics-extensie geschreven naar Azure Storage, maar niet de Azure Monitor-gegevensarchief.  

Als u geen ervaring met Resource Manager-sjablonen, meer informatie over [sjabloonimplementaties](../../azure-resource-manager/resource-group-overview.md) en de structuur en de syntaxis.  

## <a name="prerequisites"></a>Vereisten

- Uw abonnement moet worden geregistreerd bij [Microsoft.Insights](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services). 

- U moet beschikken over [Azure PowerShell](/powershell/azure) geïnstalleerd, of kunt u [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview). 


## <a name="set-up-azure-monitor-as-a-data-sink"></a>Azure Monitor instellen als een gegevens-sink 
De Azure Diagnostics-extensie gebruikt een functie met de naam **gegevenssinks** route metrische gegevens en logboeken naar verschillende locaties. De volgende stappen laten zien hoe een Resource Manager-sjabloon en PowerShell gebruiken op een virtuele machine implementeren met behulp van de Azure Monitor-sink voor nieuwe gegevens. 

## <a name="author-a-resource-manager-template"></a>De auteur van een Resource Manager-sjabloon 
In dit voorbeeld kunt u een openbaar beschikbare [voorbeeldsjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-autoscale):  

- **Azuredeploy.JSON** is een vooraf geconfigureerde Resource Manager-sjabloon voor de implementatie van een virtuele-machineschaalset.

- **Azuredeploy.parameters.JSON** is een parameterbestand waarin informatie zoals de gebruikersnaam en wachtwoord die u wilt instellen voor uw virtuele machine. De Resource Manager-sjabloon gebruikt tijdens de implementatie van de parameters in dit bestand. 

Download en slaat u beide bestanden lokaal. 

###  <a name="modify-azuredeployparametersjson"></a>Azuredeploy.parameters.json wijzigen
Open de **azuredeploy.parameters.json** bestand:  
 
- Geef een **vmSKU** u wilt implementeren. Het wordt aangeraden de Standard_D2_v3. 
- Geef een **windowsOSVersion** u wilt gebruiken voor uw virtuele-machineschaalset. Het wordt aangeraden de 2016-Datacenter. 
- De naam van de virtuele-resource om te worden geïmplementeerd machineschaalset met behulp van een **vmssName** eigenschap. Een voorbeeld is **VMSS-WAD-TEST**.    
- Geef het aantal virtuele machines die u uitvoeren op de virtuele-machineschaalset wilt met behulp van de **instanceCount** eigenschap.
- Voer waarden in voor **adminUsername** en **adminPassword** instellen voor de virtuele-machineschaalset. Deze parameters worden gebruikt voor externe toegang tot de virtuele machines in de schaalset. Om te voorkomen dat uw virtuele machine gehackt **niet** die in deze sjabloon gebruiken. Bots scannen op het internet voor gebruikersnamen en wachtwoorden in openbare GitHub-opslagplaatsen. Ze waarschijnlijk worden VM's testen met deze standaardinstellingen. 


###  <a name="modify-azuredeployjson"></a>Azuredeploy.json wijzigen
Open de **azuredeploy.json** bestand. 

Toevoegen van een variabele voor de gegevens over het opslagaccount in de Resource Manager-sjabloon. Alle logboeken of prestatiemeteritems die zijn opgegeven in het configuratiebestand van de diagnostische gegevens worden geschreven naar zowel de Azure Monitor metrische store als het opslagaccount dat u hier opgeeft: 

```json
"variables": { 
//add this line       
"storageAccountName": "[concat('storage', uniqueString(resourceGroup().id))]", 
```
 
De virtuele-machineschaalset definitie in de sectie met resources zoeken en toevoegen de **identiteit** sectie aan de configuratie. Deze toevoeging zorgt ervoor dat Azure wijst deze toe een systeem-id. Deze stap zorgt er ook voor dat de virtuele machines in de schaalset metrische gegevens voor gasten over zichzelf naar Azure Monitor kunnen verzenden:  

```json
    { 
      "type": "Microsoft.Compute/virtualMachineScaleSets", 
      "name": "[variables('namingInfix')]", 
      "location": "[resourceGroup().location]", 
      "apiVersion": "2017-03-30", 
      //add these lines below
      "identity": { 
           "type": "systemAssigned" 
       }, 
       //end of lines to add
```

In de virtuele machine schaalset bron, vinden de **virtualMachineProfile** sectie. Toevoegen van een nieuw profiel met de naam **extensionsProfile** voor het beheren van extensies.  


In de **extensionProfile**, een nieuwe extensie toevoegen aan de sjabloon, zoals wordt weergegeven in de **VMSS-WAD-extensie** sectie.  In deze sectie wordt de beheerde identiteit voor Azure-resources-extensie die ervoor zorgt de metrische gegevens worden verzonden dat, worden geaccepteerd door Azure Monitor. De **naam** veld kan een willekeurige naam bevatten. 

De volgende code uit de MSI-extensie ook wordt toegevoegd de extensie voor diagnostische gegevens en configuratie als een extensie-resource met de virtuele machine scale set bron. U kunt toevoegen of verwijderen van prestatiemeteritems, indien nodig: 

```json
          "extensionProfile": { 
            "extensions": [ 
            // BEGINNING of added code  
            // Managed identities for Azure resources   
                { 
                 "name": "VMSS-WAD-extension", 
                 "properties": { 
                       "publisher": "Microsoft.ManagedIdentity", 
                       "type": "ManagedIdentityExtensionForWindows", 
                       "typeHandlerVersion": "1.0", 
                       "autoUpgradeMinorVersion": true, 
                       "settings": { 
                             "port": 50342 
                           }, 
                       "protectedSettings": {} 
                     } 
                                
            }, 
            // add diagnostic extension. (Remove this comment after pasting.)
            { 
              "name": "[concat('VMDiagnosticsVmExt','_vmNodeType0Name')]", 
              "properties": { 
                   "type": "IaaSDiagnostics", 
                   "autoUpgradeMinorVersion": true, 
                   "protectedSettings": { 
                        "storageAccountName": "[variables('storageAccountName')]", 
                        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')),'2015-05-01-preview').key1]", 
                        "storageAccountEndPoint": "https://core.windows.net/" 
                   }, 
                   "publisher": "Microsoft.Azure.Diagnostics", 
                   "settings": { 
                        "WadCfg": { 
                              "DiagnosticMonitorConfiguration": { 
                                   "overallQuotaInMB": "50000", 
                                   "PerformanceCounters": { 
                                       "scheduledTransferPeriod": "PT1M", 
                                       "sinks": "AzMonSink", 
                                       "PerformanceCounterConfiguration": [ 
                                          { 
              "counterSpecifier": "\\Memory\\% Committed Bytes In Use", 
                                              "sampleRate": "PT15S" 
           }, 
           { 
              "counterSpecifier": "\\Memory\\Available Bytes", 
              "sampleRate": "PT15S" 
           }, 
           { 
              "counterSpecifier": "\\Memory\\Committed Bytes", 
              "sampleRate": "PT15S" 
           } 
                                       ] 
                                 }, 
                                 "EtwProviders": { 
                                       "EtwEventSourceProviderConfiguration": [ 
                                           { 
                                              "provider": "Microsoft-ServiceFabric-Actors", 
                                              "scheduledTransferKeywordFilter": "1", 
                                              "scheduledTransferPeriod": "PT5M", 
                                              "DefaultEvents": { 
                                              "eventDestination": "ServiceFabricReliableActorEventTable" 
                                           } 
                                           }, 
                                           { 
                                              "provider": "Microsoft-ServiceFabric-Services", 
                                              "scheduledTransferPeriod": "PT5M", 
                                              "DefaultEvents": { 
                                                   "eventDestination": "ServiceFabricReliableServiceEventTable" 
                                              } 
                                           } 
                                     ], 
                                     "EtwManifestProviderConfiguration": [ 
                                           { 
                                              "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8", 
                                              "scheduledTransferLogLevelFilter": "Information", 
                                              "scheduledTransferKeywordFilter": "4611686018427387904", 
                                              "scheduledTransferPeriod": "PT5M", 
                                              "DefaultEvents": { 
                                                   "eventDestination": "ServiceFabricSystemEventTable" 
                                              } 
                                          } 
                                     ] 
                               } 
                               }, 
                               "SinksConfig": { 
                                     "Sink": [ 
                                          { 
                                              "name": "AzMonSink", 
                                              "AzureMonitor": {} 
                                          } 
                                      ] 
                               } 
                         }, 
                         "StorageAccount": "[variables('storageAccountName')]" 
                         }, 
                        "typeHandlerVersion": "1.11" 
                  } 
           } 
            ] 
          }
          }
      }
    },
    //end of added code plus a few brackets. Be sure that the number and type of brackets match properly when done. 
    {
      "type": "Microsoft.Insights/autoscaleSettings",
...
```


Voeg een **dependsOn** voor het opslagaccount om te controleren of deze gemaakt in de juiste volgorde: 

```json
"dependsOn": [ 
"[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]", 
"[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]" 
//add this line below
"[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]" 
```

Een storage-account maken als een al in de sjabloon is niet gemaakt: 

```json
"resources": [
// add this code    
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[resourceGroup().location]",
    "properties": {
    "accountType": "Standard_LRS"
    }
},
// end added code
{ 
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
```

Opslaan en sluiten van beide bestanden. 

## <a name="deploy-the-resource-manager-template"></a>De Resource Manager-sjabloon implementeren 

> [!NOTE]  
> U moet uitvoeren de Azure Diagnostics-extensieversie 1.5 of hoger **en** hebben de **autoUpgradeMinorVersion:** eigenschap ingesteld op **waar** in Resource Manager de sjabloon. Azure laadt vervolgens het juiste toestelnummer wanneer deze de virtuele machine wordt gestart. Als u deze instellingen in de sjabloon hebt, wijzigen en de sjabloon opnieuw implementeert. 


Voor het implementeren van de Resource Manager-sjabloon, kunt u Azure PowerShell gebruiken:  

1. Start PowerShell. 
1. Aanmelden bij Azure met `Login-AzAccount`.
1. Uw lijst met abonnementen ophalen met behulp van `Get-AzSubscription`.
1. Het abonnement dat u gaat maken of bijwerken van de virtuele machine instellen: 

   ```powershell
   Select-AzSubscription -SubscriptionName "<Name of the subscription>" 
   ```
1. Maak een nieuwe resourcegroep voor de virtuele machine wordt geïmplementeerd. Voer de volgende opdracht uit: 

   ```powershell
    New-AzResourceGroup -Name "VMSSWADtestGrp" -Location "<Azure Region>" 
   ```

   > [!NOTE]  
   > Geef een Azure-regio die ingeschakeld voor aangepaste metrische gegevens. Houd er rekening mee te gebruiken een [Azure-regio die ingeschakeld voor aangepaste metrische gegevens](https://github.com/MicrosoftDocs/azure-docs-pr/pull/metrics-custom-overview.md#supported-regions).
 
1. Voer de volgende opdrachten voor het implementeren van de virtuele machine:  

   > [!NOTE]  
   > Als u bijwerken van een bestaande schaalset wilt, voegt u toe **-modus incrementele** aan het einde van de opdracht. 
 
   ```powershell
   New-AzResourceGroupDeployment -Name "VMSSWADTest" -ResourceGroupName "VMSSWADtestGrp" -TemplateFile "<File path of your azuredeploy.JSON file>" -TemplateParameterFile "<File path of your azuredeploy.parameters.JSON file>"  
   ```

1. Nadat de implementatie is voltooid, moet u de virtuele-machineschaalset in Azure portal. Deze moet metrische gegevens naar Azure Monitor verzenden. 

   > [!NOTE]  
   > U kunt tegenkomen fouten om de geselecteerde **vmSkuSize**. In dat geval gaat u terug naar uw **azuredeploy.json** bestands- en bijwerken van de standaardwaarde van de **vmSkuSize** parameter. Het is raadzaam dat u probeert **Standard_DS1_v2**. 


## <a name="chart-your-metrics"></a>Grafiek van uw metrische gegevens 

1. Meld u aan bij Azure Portal. 

1. Selecteer in het menu links **Monitor**. 

1. Op de **Monitor** weergeeft, schakelt **metrische gegevens**. 

   ![Monitor - pagina van de metrische gegevens](media/collect-custom-metrics-guestos-resource-manager-vmss/metrics.png) 

1. Wijzig de aggregatie periode **afgelopen 30 minuten**.  

1. Selecteer de virtuele-machineschaalset die u hebt gemaakt in de vervolgkeuzelijst resource.  

1. Selecteer in de vervolgkeuzelijst naamruimten **azure.vm.windows.guest**. 

1. Selecteer in de vervolgkeuzelijst metrische gegevens **geheugen\%Committed Bytes in gebruik**.  

U kunt ook kies vervolgens de dimensies gebruiken op deze metrische gegevens voor de grafiek het voor een bepaalde virtuele machine of om te tekenen van elke virtuele machine in de schaalset. 



## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [aangepaste metrische gegevens](metrics-custom-overview.md).


