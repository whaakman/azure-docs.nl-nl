---
title: Verzenden van de Gast OS metrische gegevens in de Azure Monitor-metriek opslaan met een Resource Manager-sjabloon voor een Windows-Machine
description: Verzenden van de Gast OS metrische gegevens in de Azure Monitor-metriek opslaan met een Resource Manager-sjabloon voor een Windows-Machine
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 4ed911766a14dd35ea662326a5d50df11cf81698
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46984061"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-using-a-resource-manager-template-for-a-windows-virtual-machine"></a>Verzenden van de Gast OS metrische gegevens in de Azure Monitor-metriek opslaan met een Resource Manager-sjabloon voor een Windows-Machine

De Azure Monitor [Windows Azure Diagnostics-extensie](azure-diagnostics.md) (WAD), kunt u voor het verzamelen van metrische gegevens en logboeken van de Gast-besturingssysteem (als gast-OS) die wordt uitgevoerd als onderdeel van een virtuele Machine, Cloudservice of Service Fabric-cluster.  De extensie kunt telemetrie verzenden naar verschillende locaties die worden vermeld in de eerder gekoppelde artikel.  

Dit artikel beschrijft het proces voor het verzenden Gast OS-maatstaven voor prestaties voor een virtuele Machine van Windows met het gegevensarchief van Azure Monitor. U kunt beginnen met WAD versie 1.11, metrische gegevens schrijven rechtstreeks naar de Azure Monitor metrics store waar al standaardplatform metrische gegevens worden verzameld. Op deze locatie opslaat, kunt u toegang tot de dezelfde acties die beschikbaar zijn voor platform metrische gegevens.  Acties omvatten bijna realtime waarschuwingen, grafieken, routering, toegang tot van REST-API en nog veel meer.  In het verleden heeft de extensie WAD geschreven naar Azure Storage, maar niet de gegevensopslag voor Azure Monitor.   

Als u geen ervaring met Resource Manager-sjablonen, meer informatie over [sjabloonimplementaties](../azure-resource-manager/resource-group-overview.md), en de structuur en de syntaxis.  

## <a name="pre-requisites"></a>Vereisten

- Uw abonnement moet worden geregistreerd bij [Microsoft.Insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) 

- U moet beschikken over een [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) geïnstalleerd, of kunt u [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) 

 
## <a name="set-up-azure-monitor-as-a-data-sink"></a>Azure Monitor instellen als een gegevens-sink 
De Azure Diagnostics-extensie gebruikt een functie met de naam 'gegevens sinks"route metrische gegevens en logboeken naar verschillende locaties.  De volgende stappen laten zien hoe een Resource Manager-sjabloon en PowerShell gebruiken om te implementeren van een virtuele machine met behulp van de nieuwe "Azure Monitor" gegevens-sink. 

## <a name="author-resource-manager-template"></a>De auteur van Resource Manager-sjabloon 
In dit voorbeeld kunt u een openbaar beschikbare voorbeeldsjabloon. De eerste sjablonen lopen https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows 

- **Azuredeploy.JSON** is een vooraf geconfigureerde Resource Manager-sjabloon voor de implementatie van een virtuele Machine. 

- **Azuredeploy.parameters.JSON** is een parameterbestand waarin informatie, zoals gebruikersnaam en wachtwoord voor wat u wilt instellen voor uw virtuele machine. De Resource Manager-sjabloon gebruikt tijdens de implementatie van de parameters in dit bestand. 

Download en slaat u beide bestanden lokaal. 

###  <a name="modify-azuredeployparametersjson"></a>Azuredeploy.parameters.json wijzigen
Open de *azuredeploy.parameters.json* bestand 

1. Voer waarden in voor *adminUsername* en *adminPassword* voor de virtuele machine. Deze parameters worden gebruikt voor externe toegang tot de virtuele machine. Gebruik niet de namen in deze sjabloon om te voorkomen dat uw virtuele machine gehackt. Bots scannen op het internet voor gebruikersnamen en wachtwoorden in openbare Github-opslagplaatsen. Ze waarschijnlijk worden VM's testen met deze standaardinstellingen.  

1. Maak een unieke DNS-naam voor de virtuele machine.  

### <a name="modify-azuredeployjson"></a>Azuredeploy.json wijzigen

Open de *azuredeploy.json* bestand 

Toevoegen van een opslagaccount-ID voor de **variabelen** gedeelte van de sjabloon na de vermelding voor **storageAccountName**.  

```json
// Find these lines 
"variables": { 
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]", 

// Add this line directly below.  
    "accountid": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]", 
```

Deze extensie Managed Service Identity (MSI) toevoegen aan de sjabloon aan de bovenkant van de sectie 'resources'.  De extensie zorgt ervoor dat Azure Monitor accepteert de metrische gegevens worden verzonden.  

```json
//Find this code 
"resources": [
// Add this code directly below
    { 
        "type": "Microsoft.Compute/virtualMachines/extensions", 
        "name": "WADExtensionSetup", 
        "apiVersion": "2015-05-01-preview", 
        "location": "[resourceGroup().location]", 
        "dependsOn": [ 
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]" ], 
        "properties": { 
            "publisher": "Microsoft.ManagedIdentity", 
            "type": "ManagedIdentityExtensionForWindows", 
            "typeHandlerVersion": "1.0", 
            "autoUpgradeMinorVersion": true, 
            "settings": { 
                "port": 50342 
            } 
        } 
    }, 
```

De configuratie 'id' toevoegen aan de VM-resource om te controleren of Azure wijst de MSI-uitbreiding een systeem-id. Deze stap zorgt ervoor dat de virtuele machine kan metrische gegevens voor gasten over zichzelf naar Azure Monitor verzenden 

```json
// Find this section
                "subnet": {
            "id": "[variables('subnetRef')]"
            }
        }
        }
    ]
    }
},
{ 
    "apiVersion": "2017-03-30", 
    "type": "Microsoft.Compute/virtualMachines", 
    "name": "[variables('vmName')]", 
    "location": "[resourceGroup().location]", 
    // add these 3 lines below
    "identity": {  
    "type": "SystemAssigned" 
    }, 
    //end of added lines
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
    "hardwareProfile": {   
    ...
```

Voeg de volgende configuratie voor het inschakelen van de extensie voor diagnostische gegevens op een Windows-Machine.  We kunnen voor een eenvoudige Resource Manager gebaseerde virtuele Machine, de configuratie van de extensie toevoegen aan de matrix resources voor de virtuele Machine. De regel "sinks": 'AzMonSink' en de bijbehorende "SinksConfig" verderop in de sectie de extensie inschakelen om te verzenden van metrische gegevens rechtstreeks naar Azure Monitor. U kunt prestatiemeteritems behoefte toevoegen/verwijderen.  


```json
        "networkProfile": {
            "networkInterfaces": [
            {
                "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
            ]
        },
"diagnosticsProfile": { 
    "bootDiagnostics": { 
    "enabled": true, 
    "storageUri": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob]" 
    } 
} 
}, 
//Start of section to add 
"resources": [        
{ 
            "type": "extensions", 
            "name": "Microsoft.Insights.VMDiagnosticsSettings", 
            "apiVersion": "2015-05-01-preview", 
            "location": "[resourceGroup().location]", 
            "dependsOn": [ 
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]" 
            ], 
            "properties": { 
            "publisher": "Microsoft.Azure.Diagnostics", 
            "type": "IaaSDiagnostics", 
            "typeHandlerVersion": "1.4", 
            "autoUpgradeMinorVersion": true, 
            "settings": { 
                "WadCfg": { 
                "DiagnosticMonitorConfiguration": { 
    "overallQuotaInMB": 4096, 
    "DiagnosticInfrastructureLogs": { 
                    "scheduledTransferLogLevelFilter": "Error" 
        }, 
                    "Directories": { 
                    "scheduledTransferPeriod": "PT1M", 
    "IISLogs": { 
                        "containerName": "wad-iis-logfiles" 
                    }, 
                    "FailedRequestLogs": { 
                        "containerName": "wad-failedrequestlogs" 
                    } 
                    }, 
                    "PerformanceCounters": { 
                    "scheduledTransferPeriod": "PT1M", 
                    "sinks": "AzMonSink", 
                    "PerformanceCounterConfiguration": [ 
                        { 
                        "counterSpecifier": "\\Memory\\Available Bytes", 
                        "sampleRate": "PT15S" 
                        }, 
                        { 
                        "counterSpecifier": "\\Memory\\% Committed Bytes In Use", 
                        "sampleRate": "PT15S" 
                        }, 
                        { 
                        "counterSpecifier": "\\Memory\\Committed Bytes", 
                        "sampleRate": "PT15S" 
                        } 
                    ] 
                    }, 
                    "WindowsEventLog": { 
                    "scheduledTransferPeriod": "PT1M", 
                    "DataSource": [ 
                        { 
                        "name": "Application!*" 
                        } 
                    ] 
                    }, 
                    "Logs": { 
                    "scheduledTransferPeriod": "PT1M", 
                    "scheduledTransferLogLevelFilter": "Error" 
                    } 
                }, 
                "SinksConfig": { 
                    "Sink": [ 
                    { 
                        "name" : "AzMonSink", 
                        "AzureMonitor" : {} 
                    } 
                    ] 
                } 
                }, 
                "StorageAccount": "[variables('storageAccountName')]" 
            }, 
            "protectedSettings": { 
                "storageAccountName": "[variables('storageAccountName')]", 
                "storageAccountKey": "[listKeys(variables('accountid'),'2015-06-15').key1]", 
                "storageAccountEndPoint": "https://core.windows.net/" 
            } 
            } 
        } 
        ] 
//End of section to add 
```


Opslaan en sluiten van beide bestanden 
 

## <a name="deploy-the-resource-manager-template"></a>De Resource Manager-sjabloon implementeren 

> [!NOTE]
> U moet beschikken over de Azure Diagnostics-extensieversie 1.5 of hoger en de 'autoUpgradeMinorVersion' zijn: eigenschap is ingesteld op 'true' in de Resource Manager-sjabloon.  Azure laadt vervolgens het juiste toestelnummer wanneer deze de virtuele machine wordt gestart. Als u deze instellingen niet in de sjabloon hebt, wijzigen en de sjabloon opnieuw implementeert. 


Als de Resource Manager-sjabloon wilt implementeren we gebruik van Azure PowerShell.  

1. PowerShell starten 
1. Meld u aan bij Azure met `Login-AzureRmAccount`
1. De lijst met abonnementen ophalen `Get-AzureRmSubscription`
1. Stel het abonnement dat u zal worden maken of bijwerken van de virtuele machine in 

   ```PowerShell
   Select-AzureRmSubscription -SubscriptionName "<Name of the subscription>" 
   ```
1. Maak een nieuwe resourcegroep voor de virtuele machine wordt geïmplementeerd, voer de onderstaande opdracht 

   ```PowerShell
    New-AzureRmResourceGroup -Name "<Name of Resource Group>" -Location "<Azure Region>" 
   ```
   > [!NOTE] 
   > Houd er rekening mee te [gebruik van een Azure-regio die is ingeschakeld voor aangepaste metrische gegevens](metrics-custom-overview.md). 
 
1. Voer de volgende opdrachten voor het implementeren van de virtuele machine met de  
   > [!NOTE] 
   > Als u bijwerken van een bestaande virtuele machine wilt, Voeg *-modus incrementele* aan het einde van de volgende opdracht uit. 
 
   ```PowerShell
   New-AzureRmResourceGroupDeployment -Name "<NameThisDeployment>" -ResourceGroupName "<Name of the Resource Group>" -TemplateFile "<File path of your Resource Manager template>" -TemplateParameterFile "<File path of your parameters file>" 
   ```
  
1. Zodra de implementatie is gelukt moet u kunnen vinden van de virtuele machine in Azure Portal en deze metrische gegevens naar Azure Monitor te genereren. 

   > [!NOTE] 
   > U kunt uitvoeren in fouten om de geselecteerde vmSkuSize. Als dit het geval is, gaat u terug naar het bestand azuredeploy.json en de standaardwaarde van de parameter vmSkuSize bijwerken. In dit geval wordt aangeraden probeert 'Standard_DS1_v2'). 

## <a name="chart-your-metrics"></a>Grafiek van uw metrische gegevens 

1. Meld u aan de Azure-Portal 

1. Klik in het menu links op **Monitor** 

1. Klik op de pagina van de Monitor op **metrische gegevens**. 

   ![Pagina van de metrische gegevens](./media/metrics-store-custom-rest-api/metrics.png) 

1. Wijzig de aggregatie periode **afgelopen 30 minuten**.  

1. Selecteer de virtuele machine die zojuist hebt gemaakt in de vervolgkeuzelijst resource. Als u de naam in de sjabloon niet hebt gewijzigd, moet dit zijn *SimpleWinVM2*.  

1. Selecteer in de vervolgkeuzelijst-naamruimten **azure.vm.windows.guest** 

1. In de metrische gegevens vervolgkeuzelijst, selecteer **geheugen\%Committed Bytes in gebruik**.  
 

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [aangepaste metrische gegevens](metrics-custom-overview.md).