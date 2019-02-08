---
title: Verzenden van Guest OS metrische gegevens in de Azure Monitor-metriek opslaan met behulp van Resource Manager-sjabloon voor een virtuele machine van Windows
description: Verzenden van de Gast OS metrische gegevens in de Azure Monitor-metriek opslaan met behulp van Resource Manager-sjabloon voor een virtuele machine van Windows
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 1eaf73e1d8b3c60ea32fa7d4aaf51a212d24a18c
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55894595"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-using-a-resource-manager-template-for-a-windows-virtual-machine"></a>Verzenden van Guest OS metrische gegevens in de Azure Monitor-metriek opslaan met behulp van Resource Manager-sjabloon voor een virtuele machine van Windows

Met behulp van de Azure Monitor [Diagnostics-extensie](diagnostics-extension-overview.md), kunt u metrische gegevens en logboeken verzamelen van het gastbesturingssysteem (Gastbesturingssysteem) die wordt uitgevoerd als onderdeel van een virtuele machine, een cloudservice of een Service Fabric-cluster. De extensie kunt telemetrie wordt verzonden naar [veel verschillende locaties.](https://docs.microsoft.com/azure/monitoring/monitoring-data-collection?toc=/azure/azure-monitor/toc.json)

In dit artikel beschrijft het proces voor het verzenden van Guest OS maatstaven voor prestaties voor een virtuele machine van Windows met het gegevensarchief van Azure Monitor. U kunt beginnen met diagnostische gegevens over versie 1.11, metrische gegevens schrijven rechtstreeks naar de Azure-Monitor metrische gegevens opslaan, waar al standaardplatform metrische gegevens worden verzameld.

Op deze locatie opslaat, kunt u toegang tot de dezelfde acties voor platform metrische gegevens. Acties zijn bijna realtime waarschuwingen, grafieken, Routering en toegang van een REST-API en nog veel meer. In het verleden heeft de extensie voor diagnostische gegevens geschreven naar Azure Storage, maar niet aan de gegevensopslag van Azure Monitor.

Als u geen ervaring met Resource Manager-sjablonen, meer informatie over [sjabloonimplementaties](../../azure-resource-manager/resource-group-overview.md) en de structuur en de syntaxis.

## <a name="prerequisites"></a>Vereisten

- Uw abonnement moet worden geregistreerd bij [Microsoft.Insights](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services).

- U moet beschikken over een [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) of [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) geïnstalleerd.


## <a name="set-up-azure-monitor-as-a-data-sink"></a>Azure Monitor instellen als een gegevens-sink
De Azure Diagnostics-extensie gebruikt een functie met de naam 'gegevens sinks"route metrische gegevens en logboeken naar verschillende locaties. De volgende stappen laten zien hoe een Resource Manager-sjabloon en PowerShell gebruiken op een virtuele machine implementeren met behulp van de nieuwe "Azure Monitor" gegevens-sink.

## <a name="author-resource-manager-template"></a>De auteur van Resource Manager-sjabloon
In dit voorbeeld kunt u een openbaar beschikbare voorbeeldsjabloon. De begin-sjablonen zijn op https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows.

- **Azuredeploy.JSON** is een vooraf geconfigureerde Resource Manager-sjabloon voor de implementatie van een virtuele machine.

- **Azuredeploy.parameters.JSON** is een parameterbestand waarin informatie zoals de gebruikersnaam en wachtwoord hebt ingesteld voor uw virtuele machine. De Resource Manager-sjabloon maakt gebruik van de parameters die in dit bestand zijn ingesteld tijdens de implementatie.

Download en slaat u beide bestanden lokaal.

### <a name="modify-azuredeployparametersjson"></a>Azuredeploy.parameters.json wijzigen
Open de *azuredeploy.parameters.json* bestand

1. Voer waarden in voor **adminUsername** en **adminPassword** voor de virtuele machine. Deze parameters worden gebruikt voor externe toegang tot de virtuele machine. Om te voorkomen dat uw virtuele machine zijn gehijackt, niet de waarden in deze sjabloon gebruiken. Bots scannen op het internet voor gebruikersnamen en wachtwoorden in openbare GitHub-opslagplaatsen. Ze waarschijnlijk worden VM's testen met deze standaardinstellingen.

1. Maak een unieke DNS-naam voor de virtuele machine.

### <a name="modify-azuredeployjson"></a>Azuredeploy.json wijzigen

Open de *azuredeploy.json* bestand

Toevoegen van een opslagaccount-ID voor de **variabelen** gedeelte van de sjabloon na de vermelding voor **storageAccountName.**

```json
// Find these lines.
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]",

// Add this line directly below.
    "accountid": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
```

Deze extensie Managed Service Identity (MSI) toevoegen aan de sjabloon aan de bovenkant van de **resources** sectie. De extensie zorgt ervoor dat Azure Monitor accepteert de metrische gegevens die zijn die wordt verzonden.

```json
//Find this code.
"resources": [
// Add this code directly below.
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

Voeg de **identiteit** configuratie van de VM-resource om ervoor te zorgen dat Azure wijst een systeem-id toe aan de MSI-extensie. Deze stap zorgt ervoor dat de virtuele machine metrische gegevens voor gasten over zichzelf naar Azure Monitor kan verzenden.

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

Voeg de volgende configuratie om in te schakelen van de extensie voor diagnostische gegevens op een Windows virtuele machine toe. We kunnen voor een eenvoudige Resource Manager gebaseerde virtuele machine, de configuratie van de extensie toevoegen aan de matrix resources voor de virtuele machine. De regel "sinks"&mdash; 'AzMonSink' en de bijbehorende "SinksConfig" verderop in de sectie&mdash;de extensie inschakelen voor het verzenden van metrische gegevens rechtstreeks naar Azure Monitor. U kunt toevoegen of verwijderen van prestatiemeteritems, indien nodig.


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
            "typeHandlerVersion": "1.12",
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


Opslaan en sluiten van beide bestanden.


## <a name="deploy-the-resource-manager-template"></a>De Resource Manager-sjabloon implementeren

> [!NOTE]
> U moet zijn waarop de Azure Diagnostics-extensieversie 1.5 of hoger worden uitgevoerd en de **autoUpgradeMinorVersion**: eigenschap is ingesteld op 'true' in de Resource Manager-sjabloon. Azure laadt vervolgens het juiste toestelnummer wanneer deze de virtuele machine wordt gestart. Als u deze instellingen in de sjabloon hebt, wijzigen en de sjabloon opnieuw implementeert.


Voor het implementeren van de Resource Manager-sjabloon, maken we gebruik van Azure PowerShell.

1. Start PowerShell.
1. Meld u aan bij Azure met `Login-AzureRmAccount`.
1. Uw lijst met abonnementen ophalen met behulp van `Get-AzureRmSubscription`.
1. Stel in het abonnement dat u gebruikt voor het maken/bijwerken van de virtuele machine in:

   ```PowerShell
   Select-AzureRmSubscription -SubscriptionName "<Name of the subscription>"
   ```
1. Voor het maken van een nieuwe resourcegroep voor de virtuele machine die wordt geïmplementeerd, moet u de volgende opdracht uitvoeren:

   ```PowerShell
    New-AzureRmResourceGroup -Name "<Name of Resource Group>" -Location "<Azure Region>"
   ```
   > [!NOTE]
   > Houd er rekening mee te [gebruik van een Azure-regio die is ingeschakeld voor aangepaste metrische gegevens](metrics-custom-overview.md).

1. Voer de volgende opdrachten voor het implementeren van de virtuele machine met behulp van de Resource Manager-sjabloon.
   > [!NOTE]
   > Als u bijwerken van een bestaande virtuele machine wilt, Voeg *-modus incrementele* aan het einde van de volgende opdracht uit.

   ```PowerShell
   New-AzureRmResourceGroupDeployment -Name "<NameThisDeployment>" -ResourceGroupName "<Name of the Resource Group>" -TemplateFile "<File path of your Resource Manager template>" -TemplateParameterFile "<File path of your parameters file>"
   ```

1. Nadat de implementatie is voltooid, moet de virtuele machine zich in de Azure-portal, metrische gegevens naar Azure Monitor genereren.

   > [!NOTE]
   > U kunt tegenkomen fouten om de geselecteerde vmSkuSize. Als dit het geval is, gaat u terug naar het bestand azuredeploy.json en de standaardwaarde van de parameter vmSkuSize bijwerken. In dit geval wordt aangeraden probeert 'Standard_DS1_v2').

## <a name="chart-your-metrics"></a>Grafiek van uw metrische gegevens

1. Meld u aan bij Azure Portal.

2. Selecteer in het menu links **Monitor**.

3. Selecteer op de pagina Monitor **metrische gegevens**.

   ![Pagina van de metrische gegevens](media/collect-custom-metrics-guestos-resource-manager-vm/metrics.png)

4. Wijzig de aggregatie periode **afgelopen 30 minuten**.

5. Selecteer de virtuele machine die u hebt gemaakt in de vervolgkeuzelijst resource. Als u de naam in de sjabloon niet hebt gewijzigd, moet dit zijn *SimpleWinVM2*.

6. Selecteer in de vervolgkeuzelijst naamruimten **azure.vm.windows.guest**

7. Selecteer in de vervolgkeuzelijst metrische gegevens **geheugen\%Committed Bytes in gebruik**.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [aangepaste metrische gegevens](metrics-custom-overview.md).

