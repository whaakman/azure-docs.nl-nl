---
title: Aggregatie van Azure Service Fabric-gebeurtenis met Windows Azure Diagnostics | Microsoft Docs
description: Meer informatie over het aggregeren en het verzamelen van gebeurtenissen met af voor controle en diagnostische gegevens van Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/03/2018
ms.author: srrengar
ms.openlocfilehash: 38a026e8995bb7384c866dcd2f12588ca816009f
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34205770"
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a>Gebeurtenis-aggregatie en verzameling op basis van Windows Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Wanneer u een Azure Service Fabric-cluster uitvoert, is het een goed idee de logboeken verzamelen van alle knooppunten in een centrale locatie. De logboeken die in een centrale locatie, helpt u bij het analyseren en oplossen van problemen in het cluster of problemen in de toepassingen en services die worden uitgevoerd in het cluster.

Een manier om te uploaden en verzamelen van Logboeken is het gebruik van de Windows Azure Diagnostics (af)-extensie, die Logboeken uploadt naar Azure Storage, en heeft ook de optie om Logboeken te verzenden naar Azure Application Insights of Event Hubs. U kunt ook een extern proces gebruiken om te lezen van de gebeurtenissen uit de opslag en plaats deze in een analyse platform-product, zoals [logboekanalyse](../log-analytics/log-analytics-service-fabric.md) of een andere oplossing voor het parseren van het logboek.

## <a name="prerequisites"></a>Vereisten
De volgende hulpprogramma's worden gebruikt in dit artikel:

* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Resource Manager-sjabloon](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric-platform-events"></a>Gebeurtenissen voor service Fabric-platform
Service Fabric stelt u met een paar [out-of-the-box logboekkanalen](service-fabric-diagnostics-event-generation-infra.md), die de volgende kanalen zijn vooraf geconfigureerd met de extensie voor het verzenden van de controle en diagnostische gegevens naar een tabel opslag of elders:
  * [Operationele gebeurtenissen](service-fabric-diagnostics-event-generation-operational.md): een hoger niveau bewerkingen waarmee de Service Fabric-platform. Voorbeelden zijn onder meer het maken van toepassingen en services, knooppunt statuswijzigingen en informatie over de upgrade. Deze worden verzonden als Logboeken Event Tracing voor Windows (ETW)
  * [Reliable Actors programming model gebeurtenissen](service-fabric-reliable-actors-diagnostics.md)
  * [Reliable Services programming model gebeurtenissen](service-fabric-reliable-services-diagnostics.md)

## <a name="deploy-the-diagnostics-extension-through-the-portal"></a>De extensie voor diagnostische gegevens via de portal implementeren
De eerste stap bij het verzamelen van Logboeken is voor het implementeren van de extensie voor diagnostische gegevens op de virtuele machine scale set knooppunten in het Service Fabric-cluster. De extensie voor diagnostische gegevens verzamelt logboeken op elke virtuele machine en geüpload naar het opslagaccount dat u opgeeft. De volgende stappen wordt uitgelegd hoe u kunt dit doen voor nieuwe en bestaande clusters via de Azure portal en Azure Resource Manager-sjablonen.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a>De extensie voor diagnostische gegevens als onderdeel van het maken van het cluster via Azure portal implementeren
Wanneer u uw cluster in de cluster-configuratiestap, vouw de optionele instellingen en zorg ervoor dat de diagnostische gegevens is ingesteld op **op** (de standaardinstelling).

![Azure Diagnostics-instellingen in de portal voor het maken van het cluster](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics-new.png)

We raden u de sjabloon downloaden **voordat u op maken klikt** in de laatste stap. Raadpleeg voor meer informatie, [een Service Fabric-cluster instellen met behulp van een Azure Resource Manager-sjabloon](service-fabric-cluster-creation-via-arm.md). U moet de sjabloon te wijzigen op welke kanalen (Zie hierboven) voor het verzamelen van gegevens uit.

![Cluster-sjabloon](media/service-fabric-diagnostics-event-aggregation-wad/download-cluster-template.png)

Nu dat u bij het samenvoegen van gebeurtenissen in Azure Storage [logboekanalyse instellen](service-fabric-diagnostics-oms-setup.md) inzicht en hierin zoeken in de portal Log Analytics

>[!NOTE]
>Er is momenteel geen manier om te filteren of Let op de gebeurtenissen die worden verzonden naar de tabellen. Als u een proces voor het verwijderen van gebeurtenissen uit de tabel niet implementeert, de tabel blijft groeien (het kapje standaardwaarde is 50 GB). Instructies voor het wijzigen van hiervan zijn [verdere hieronder in dit artikel](service-fabric-diagnostics-event-aggregation-wad.md#update-storage-quota). Bovendien is er een voorbeeld van een gegevensservice opschonen uitgevoerd in de [Watchdog voorbeeld](https://github.com/Azure-Samples/service-fabric-watchdog-service), en wordt aanbevolen dat u één voor uzelf, schrijft tenzij er een goede reden voor het opslaan van Logboeken na een periode 30 of 90 dagen.

## <a name="deploy-the-diagnostics-extension-through-azure-resource-manager"></a>De extensie voor diagnostische gegevens via Azure Resource Manager implementeren

### <a name="create-a-cluster-with-the-diagnostics-extension"></a>Een cluster maken met de extensie voor diagnostische gegevens
Voor het maken van een cluster met Resource Manager, moet u de configuratie van diagnostische JSON naar de volledige Resource Manager-sjabloon toevoegen voordat u het cluster maakt. We bieden een voorbeeldsjabloon vijf VM-cluster Resource Manager met de configuratie van diagnostische toegevoegd als onderdeel van onze voorbeelden Resource Manager-sjabloon. U kunt het zien op deze locatie in de galerie van Azure Samples: [cluster met vijf knooppunten met diagnostische gegevens van Resource Manager-sjabloon voorbeeld](https://azure.microsoft.com/en-in/resources/templates/service-fabric-secure-cluster-5-node-1-nodetype/).

Als u de instelling van de diagnostische gegevens in de Resource Manager-sjabloon, open het bestand azuredeploy.json en zoeken naar **IaaSDiagnostics**. Als u een cluster met behulp van deze sjabloon, selecteert de **implementeren in Azure** knop beschikbaar op de vorige koppeling.

U kunt ook u kunt het Resource Manager-voorbeeld downloaden, wijzigingen aanbrengen en een cluster maken met de gewijzigde sjabloon met behulp van de `New-AzureRmResourceGroupDeployment` opdracht in een Azure PowerShell-venster. Zie de volgende code voor de parameters die u aan de opdracht doorgeeft. Zie het artikel voor gedetailleerde informatie over het implementeren van een resourcegroep met behulp van PowerShell [een resourcegroep implementeren met de Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-template-deploy.md).

### <a name="add-the-diagnostics-extension-to-an-existing-cluster"></a>De extensie voor diagnostische gegevens toevoegen aan een bestaand cluster
Als er een bestaand cluster dat geen diagnostische gegevens die zijn geïmplementeerd, kunt u deze kunt toevoegen of bijwerken via de sjabloon van het cluster. Wijzig de Resource Manager-sjabloon die wordt gebruikt voor het maken van het bestaande cluster of de sjabloon downloaden van de portal, zoals eerder beschreven. Het bestand template.json wijzigen door de volgende taken:

Een nieuwe opslagresource toevoegen aan de sjabloon door toe te voegen aan de bronnensectie.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "sku": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Vervolgens voegt u het gedeelte parameters direct na de definities van de account opslag tussen `supportLogStorageAccountName`. Vervang de tijdelijke aanduiding voor tekst *opslagaccountnaam hier* met de naam van het opslagaccount dat u wilt.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "**STORAGE ACCOUNT NAME GOES HERE**",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Werk vervolgens de `VirtualMachineProfile` gedeelte van het bestand template.json door de volgende code binnen de matrix extensies toe te voegen. Zorg ervoor dat een komma toe te voegen aan het begin of einde, afhankelijk van waar het ingevoegd.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
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
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Nadat u het bestand template.json aanpassen zoals is beschreven, publiceert u de Resource Manager-sjabloon opnieuw. Als de sjabloon is geëxporteerd, het bestand deploy.ps1 uitvoert rapport opnieuw publiceert de sjabloon. Nadat u hebt geïmplementeerd, zorg ervoor dat **ProvisioningState** is **geslaagd**.

> [!TIP]
> Als u containers implementeren in uw cluster gaat, schakelt u af om op te halen docker-statistieken door toe te voegen aan uw **WadCfg > DiagnosticMonitorConfiguration** sectie.
>
>```json
>"DockerSources": {
>    "Stats": {
>        "enabled": true,
>        "sampleRate": "PT1M"
>    }
>},
>```

### <a name="update-storage-quota"></a>Opslaglimiet bijwerken

Sinds de tabellen die is gevuld met de extensie groeit totdat het quotum is bereikt, kunt u overwegen quotum wordt verkleind. De standaardwaarde is 50 GB en kan worden geconfigureerd in de sjabloon onder de `overallQuotainMB` veld onder `DiagnosticMonitorConfiguration`

```json
"overallQuotaInMB": "50000",
```

## <a name="log-collection-configurations"></a>Meld u verzameling configuraties
Logboeken van extra kanalen zijn ook beschikbaar voor de verzameling, Hier volgen enkele van de meest voorkomende configuraties die kunt u in de sjabloon voor clusters worden uitgevoerd in Azure.

* Operationele kanaal - Base: Ingeschakeld op hoog niveau bewerkingen wordt uitgevoerd door de Service Fabric en het cluster, met inbegrip van gebeurtenissen voor een knooppunt is afkomstig van een nieuwe toepassing wordt geïmplementeerd, of kunnen een upgrade terugdraaien, enzovoort. Raadpleeg voor een lijst met gebeurtenissen [operationele kanaal gebeurtenissen](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-operational).
  
```json
      scheduledTransferKeywordFilter: "4611686018427387904"
  ```
* Operationele kanaal - gedetailleerde: Dit omvat statusrapporten en taakverdeling beslissingen, plus alles in het basistype operationele kanaal. Deze gebeurtenissen worden gegenereerd door het systeem of uw code met behulp van de status of rapportage-API's, zoals load [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) of [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). Om weer te geven deze gebeurtenissen in Visual Studio diagnostische logboeken toevoegen ' Microsoft-ServiceFabric:4:0x4000000000000008 ' aan de lijst met ETW-providers.

```json
      scheduledTransferKeywordFilter: "4611686018427387912"
  ```

* Gegevens en berichten kanaal - Base: essentiële logboeken en gebeurtenissen die worden gegenereerd in de messaging-(momenteel alleen de ReverseProxy) en het gegevenspad, ook naar Logboeken gedetailleerde operationele kanaal. Deze gebeurtenissen zijn fouten en andere belangrijke problemen in de ReverseProxy verwerking van aanvragen als aanvragen verwerkt. **Dit is de aanbeveling voor uitgebreide logboekregistratie**. Toevoegen om weer te geven deze gebeurtenissen in Visual Studio diagnostische logboeken, ' Microsoft-ServiceFabric:4:0x4000000000000010 ' aan de lijst met ETW-providers.

```json
      scheduledTransferKeywordFilter: "4611686018427387928"
  ```

* Gegevens & Messaging-kanaal - gedetailleerde: uitgebreide kanaal dat deze bevat de niet-kritieke logboeken van gegevens en berichten in het cluster en het gedetailleerde operationele kanaal. Voor gedetailleerde probleemoplossing voor alle reverse proxy-gebeurtenissen, verwijzen naar de [omgekeerde proxy diagnostics handleiding](service-fabric-reverse-proxy-diagnostics.md).  Toevoegen om weer te geven deze gebeurtenissen in Visual Studio diagnostische logboeken, ' Microsoft-ServiceFabric:4:0x4000000000000020 ' aan de lijst met ETW-providers.

```json
      scheduledTransferKeywordFilter: "4611686018427387944"
  ```

>[!NOTE]
>Dit kanaal is een zeer groot aantal gebeurtenissen, waardoor gebeurtenissen verzamelen van deze gedetailleerde kanaal resultaten in een groot aantal traceringen snel wordt gegenereerd en opslagcapaciteit kan gebruiken. Schakel alleen dit in als dit echt nodig.


Om in te schakelen de **Base-gegevens en de Messaging-kanaal** de aanbeveling voor uitgebreide logboekregistratie, de `EtwManifestProviderConfiguration` in de `WadCfg` van uw sjabloon eruit als het volgende:

```json
  "WadCfg": {
        "DiagnosticMonitorConfiguration": {
          "overallQuotaInMB": "50000",
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
                "scheduledTransferKeywordFilter": "4611686018427387928",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                  "eventDestination": "ServiceFabricSystemEventTable"
                }
              }
            ]
          }
        }
      },
```

## <a name="collect-from-new-eventsource-channels"></a>Verzamelen van nieuwe EventSource kanalen

Voor het bijwerken van diagnostische gegevens voor het verzamelen van Logboeken van de nieuwe EventSource kanalen die een nieuwe toepassing bent over te implementeren en uit te voeren dezelfde vertegenwoordigen cluster zoals eerder beschreven stappen voor het instellen van diagnostische gegevens voor een bestaand.

Werk de `EtwEventSourceProviderConfiguration` sectie in het bestand template.json vermeldingen toevoegen voor de nieuwe EventSource kanalen voordat u de configuratie toepassen met behulp van bijwerken de `New-AzureRmResourceGroupDeployment` PowerShell-opdracht. De naam van de bron van de gebeurtenis wordt gedefinieerd als onderdeel van uw code in het bestand ServiceEventSource.cs Visual Studio gegenereerd.

Als uw gebeurtenisbron mijn Eventsource heet, bijvoorbeeld de volgende code om de gebeurtenissen uit mijn Eventsource plaatsen in een tabel met de naam MyDestinationTableName toevoegen.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Wijzigen voor het verzamelen van prestatiemeteritems of de gebeurtenislogboeken van de Resource Manager-sjabloon met behulp van de voorbeelden die is opgegeven in [virtuele Windows-machine maken met controle en diagnostische gegevens met behulp van een Azure Resource Manager-sjabloon](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Vervolgens publiceert u de Resource Manager-sjabloon opnieuw.

## <a name="collect-performance-counters"></a>Verzamelen van prestatiemeteritems

Toevoegen voor het verzamelen van prestatiegegevens van het cluster, de prestatiemeteritems voor uw 'WadCfg > DiagnosticMonitorConfiguration' in de Resource Manager-sjabloon voor uw cluster. Zie [prestatiebewaking met af](service-fabric-diagnostics-perf-wad.md) voor stapsgewijze instructies voor het wijzigen van uw `WadCfg` voor het verzamelen van specifieke prestatiemeteritems. Verwijzing [Service Fabric-prestatiemeteritems](service-fabric-diagnostics-event-generation-perf.md) voor een lijst van de prestaties van prestatiemeteritems die wordt aangeraden verzamelen.
  
Als u een Application Insights-sink gebruikt zoals beschreven in de onderstaande sectie en wilt dat deze metrische gegevens worden weergegeven in Application Insights, Controleer of de naam van de sink toevoegen in de sectie 'put' zoals hierboven. Hiermee wordt de prestatiemeteritems die afzonderlijk zijn geconfigureerd automatisch verzonden naar uw Application Insights-resource.


## <a name="send-logs-to-application-insights"></a>Logboeken verzenden naar Application Insights

Controle en diagnostische gegevens verzenden naar Application Insights (AI) kan worden uitgevoerd als onderdeel van de configuratie af. Als u besluit AI gebruiken voor analyse van gebeurtenis en visualisatie, lezen [het instellen van een sink AI](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-ai-sink-to-the-resource-manager-template) als onderdeel van uw 'WadCfg'.

## <a name="next-steps"></a>Volgende stappen

Wanneer u Azure diagnostics correct hebt geconfigureerd, ziet u gegevens in de tabellen bij opslag van de ETW- en EventSource Logboeken. Als u gebruiken Log Analytics, Kibana of een andere gegevens analytics en visualisatie platform dat niet rechtstreeks in de Resource Manager-sjabloon is geconfigureerd wilt, moet u voor het instellen van het platform van uw keuze in de gegevens uit deze opslagtabellen te lezen. Hierdoor voor logboekanalyse is relatief eenvoudig en wordt uitgelegd in [analyse van gebeurtenis- en logboekbestanden](service-fabric-diagnostics-event-analysis-oms.md). Application Insights is een bit van een speciaal geval in dit opzicht, omdat deze kan worden geconfigureerd als onderdeel van de configuratie voor de Diagnostics-uitbreiding, dus raadpleegt u de [juiste artikel](service-fabric-diagnostics-event-analysis-appinsights.md) als u wilt gebruiken AI.

>[!NOTE]
>Er is momenteel geen manier om te filteren of Let op de gebeurtenissen die worden verzonden naar de tabel. Als u een proces voor het verwijderen van gebeurtenissen uit de tabel niet implementeert, wordt de tabel blijven groeien. Er is een voorbeeld van een gegevensservice opschonen uitgevoerd in de [Watchdog voorbeeld](https://github.com/Azure-Samples/service-fabric-watchdog-service), en wordt aanbevolen dat u één voor uzelf, schrijft tenzij er een goede reden voor het opslaan van Logboeken na een periode 30 of 90 dagen.

* [Meer informatie over het verzamelen van prestatiemeteritems of logboeken met behulp van de extensie voor diagnostische gegevens](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Analyse van gebeurtenis en visualisatie met Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Analyse van gebeurtenis en visualisatie met logboekanalyse](service-fabric-diagnostics-event-analysis-oms.md)
