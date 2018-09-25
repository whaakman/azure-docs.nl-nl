---
title: Verzenden van de Gast OS metrische gegevens in de Azure Monitor-metriek opslaan met behulp van Resource Manager-sjabloon voor een virtuele-machineschaalset voor Windows
description: Verzenden van de Gast OS metrische gegevens in de Azure Monitor-metriek opslaan met behulp van Resource Manager-sjabloon voor een virtuele-machineschaalset voor Windows
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: d896cb01c7dc2cd4ed028db418f838809c7ce25c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46986996"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-using-a-resource-manager-template-for-a-windows-virtual-machine-scale-set"></a>Verzenden van de Gast OS metrische gegevens in de Azure Monitor-metriek opslaan met behulp van Resource Manager-sjabloon voor een virtuele-machineschaalset voor Windows

De Azure Monitor [Windows Azure Diagnostics-extensie](azure-diagnostics.md) (WAD), kunt u voor het verzamelen van metrische gegevens en logboeken van de Gast-besturingssysteem (als gast-OS) die wordt uitgevoerd als onderdeel van een virtuele Machine, Cloudservice of Service Fabric-cluster.  De extensie kunt telemetrie verzenden naar verschillende locaties die worden vermeld in de eerder gekoppelde artikel.  

Dit artikel beschrijft het proces voor het verzenden Gast OS metrische gegevens voor prestaties voor een Windows virtuele-machineschaalset met het gegevensarchief van Azure Monitor. U kunt beginnen met WAD versie 1.11, metrische gegevens schrijven rechtstreeks naar de Azure Monitor metrics store waar al standaardplatform metrische gegevens worden verzameld. Op deze locatie opslaat, kunt u toegang tot de dezelfde acties die beschikbaar zijn voor platform metrische gegevens.  Acties omvatten bijna realtime waarschuwingen, grafieken, routering, toegang tot van REST-API en nog veel meer.  In het verleden heeft de extensie WAD geschreven naar Azure Storage, maar niet de gegevensopslag voor Azure Monitor.  

Als u niet bekend bent met Resource Manager-sjablonen, meer informatie over [sjabloonimplementaties](../azure-resource-manager/resource-group-overview.md), en de structuur en de syntaxis.  

## <a name="pre-requisites"></a>Vereisten

- Uw abonnement moet worden geregistreerd bij [Microsoft.Insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) 

- U moet beschikken over een [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) geïnstalleerd, of kunt u [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) 


## <a name="set-up-azure-monitor-as-a-data-sink"></a>Azure Monitor instellen als een gegevens-sink 
De Azure Diagnostics-extensie gebruikt een functie met de naam 'gegevens sinks"route metrische gegevens en logboeken naar verschillende locaties.  De volgende stappen laten zien hoe een Resource Manager-sjabloon en PowerShell gebruiken om te implementeren van een virtuele machine met behulp van de nieuwe "Azure Monitor" gegevens-sink. 

## <a name="author-resource-manager-template"></a>De auteur van Resource Manager-sjabloon 
In dit voorbeeld kunt u een openbaar beschikbare voorbeeldsjabloon. De eerste sjablonen lopen https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-autoscale  

- **Azuredeploy.JSON** is een vooraf geconfigureerde Resource Manager-sjabloon voor de implementatie van een virtuele-machineschaalset

- **Azuredeploy.parameters.JSON** is een parameterbestand waarin informatie, zoals gebruikersnaam en wachtwoord voor wat u wilt instellen voor uw virtuele machine. De Resource Manager-sjabloon gebruikt tijdens de implementatie van de parameters in dit bestand. 

Download en slaat u beide bestanden lokaal. 

###  <a name="modify-azuredeployparametersjson"></a>Azuredeploy.parameters.json wijzigen
Open de *azuredeploy.parameters.json* bestand 

- Geef een **vmSKU** u wilt implementeren (aangeraden Standard_D2_v3) 
- Geef een **windowsOSVersion** hebt voor uw virtuele-machineschaalset (we raden aan 2016-Datacenter) 
- De naam van de virtuele-machineschaalset resource om te worden geïmplementeerd met behulp van een **vmssName** eigenschap. Bijvoorbeeld, *VMSS-WAD-TEST*.    
- Geef het aantal virtuele machines die u wilt worden uitgevoerd op de VM-schaalset met behulp van de **instanceCount** eigenschap
- Voer waarden in voor **adminUsername** en **adminPassword** instellen voor de virtuele-machineschaalset. Deze parameters worden gebruikt voor externe toegang tot de virtuele machines in de schaalset. Deze parameters worden gebruikt voor externe toegang tot de virtuele machine. Gebruik niet de namen in deze sjabloon om te voorkomen dat uw virtuele machine gehackt. Bots scannen op het internet voor gebruikersnamen en wachtwoorden in openbare Github-opslagplaatsen. Ze waarschijnlijk worden VM's testen met deze standaardinstellingen. 


###  <a name="modify-azuredeployjson"></a>Azuredeploy.json wijzigen
Open de *azuredeploy.json* bestand 

Toevoegen van een variabele voor de gegevens over het opslagaccount in de Resource Manager-sjabloon. U moet nog steeds een Storage-Account als onderdeel van de installatie van de extensie voor diagnostische gegevens opgeven. Alle logboeken en/of de prestatiemeteritems die zijn opgegeven in het configuratiebestand van de diagnostische gegevens worden geschreven naar het opgegeven opslagaccount naast worden verzonden naar de Azure Monitor metrische store. 

```json
"variables": { 
//add this line       
"storageAccountName": "[concat('storage', uniqueString(resourceGroup().id))]", 
 ```
 
De definitie van de virtuele-Machineschaalset niet vinden in de sectie met resources en de sectie 'id' toevoegen aan de configuratie. Dit zorgt ervoor dat Azure een systeem-id wijst deze toe. Deze stap zorgt ervoor dat de virtuele machines in de schaalset metrische gegevens voor gasten over zichzelf naar Azure Monitor kunnen verzenden.  

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


In de **extensionProfile**, een nieuwe extensie toevoegen aan de sjabloon, zoals aangegeven in de **VMSS-WAD-extensie sectie**.  Deze sectie is het Managed Service Identity (MSI)-extensie die ervoor zorgt de metrische gegevens worden verzonden dat, worden geaccepteerd door Azure Monitor. De **naam** veld kan een willekeurige naam bevatten. 

De code hieronder onder de MSI-extensie ook wordt toegevoegd de extensie voor diagnostische gegevens en configuratie als een extensie-resource met de virtuele machine scale set-bron. U kunt prestatiemeteritems behoefte toevoegen/verwijderen. 

```json
          "extensionProfile": { 
            "extensions": [ 
            // BEGINNING of added code  
            // Managed service identity   
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


Voeg een dependsOn voor het opslagaccount om te controleren of dat deze gemaakt in de juiste volgorde. 
```json
"dependsOn": [ 
"[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]", 
"[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]" 
//add this line below
"[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]" 
 ```

Maak een storage-account als een is niet in de sjabloon is gemaakt.  
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
> U moet beschikken over de Azure Diagnostics-extensieversie 1.5 of hoger en hebben de 'autoUpgradeMinorVersion': de eigenschap ingesteld op *waar* in de Resource Manager-sjabloon.  Azure laadt vervolgens het juiste toestelnummer wanneer deze de virtuele machine wordt gestart. Als u deze instellingen niet in de sjabloon hebt, wijzigen en de sjabloon opnieuw implementeert. 


Voor het implementeren van de Resource Manager-sjabloon, we gebruik van Azure PowerShell.  

1. PowerShell starten 
1. Meld u aan voor het gebruik van Azure `Login-AzureRmAccount`
1. De lijst met abonnementen ophalen `Get-AzureRmSubscription`
1. Stel het abonnement dat u zal worden maken of bijwerken van de virtuele machine in 

   ```PowerShell
   Select-AzureRmSubscription -SubscriptionName "<Name of the subscription>" 
   ```
1. Maak een nieuwe resourcegroep voor de virtuele machine wordt geïmplementeerd, voer de onderstaande opdracht 

   ```PowerShell
    New-AzureRmResourceGroup -Name "VMSSWADtestGrp" -Location "<Azure Region>" 
   ```

   > [!NOTE]  
   > Geef een Azure-regio die is ingeschakeld voor aangepaste metrische gegevens. Zie 
 
1. Voer de volgende opdrachten voor het implementeren van de virtuele machine met de  
   > [!NOTE] 
   > Als u bijwerken van een bestaande schaalset wilt, Voeg *-modus incrementele* aan het einde van de volgende opdracht uit. 
 
   ```PowerShell
   New-AzureRmResourceGroupDeployment -Name "VMSSWADTest" -ResourceGroupName "VMSSWADtestGrp" -TemplateFile "<File path of your azuredeploy.JSON file>" -TemplateParameterFile "<File path of your azuredeploy.parameters.JSON file>"  
   ```

1. Zodra de implementatie is gelukt moet u kunnen vinden van de virtuele-machineschaalset in Azure portal en deze metrische gegevens naar Azure Monitor te genereren. 

   > [!NOTE] 
   > U kunt uitvoeren in fouten om de geselecteerde vmSkuSize. Als dit het geval is, gaat u terug naar het bestand azuredeploy.json en de standaardwaarde van de parameter vmSkuSize bijwerken. In dit geval wordt aangeraden probeert 'Standard_DS1_v2'). 


## <a name="chart-your-metrics"></a>Grafiek van uw metrische gegevens 

1. Aanmelden bij Azure Portal 

1. Klik in het menu links op **Monitor** 

1. Klik op de pagina Monitor **metrische gegevens**. 

   ![Pagina van de metrische gegevens](./media/metrics-store-custom-rest-api/metrics.png) 

1. Wijzig de aggregatie periode **afgelopen 30 minuten**.  

1. Selecteer de virtuele-machineschaalset die u zojuist hebt gemaakt in de vervolgkeuzelijst resource.  

1. Selecteer in de vervolgkeuzelijst naamruimten **azure.vm.windows.guest** 

1. In de metrische gegevens vervolgkeuzelijst, selecteer **geheugen\%Committed Bytes in gebruik**.  

U kunt ook kies vervolgens de dimensies gebruiken op deze metrische gegevens voor een grafiek van deze metrische gegevens voor een bepaalde virtuele machine in de schaalset, of moet worden getekend van elke virtuele machine in de schaalset. 



## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [aangepaste metrische gegevens](metrics-custom-overview.md).

