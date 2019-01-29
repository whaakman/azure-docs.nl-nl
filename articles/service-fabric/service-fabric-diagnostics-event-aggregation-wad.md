---
title: Azure Service Fabric Event aggregatie met Windows Azure Diagnostics | Microsoft Docs
description: Meer informatie over aggregeren en het verzamelen van gebeurtenissen met WAD voor controle en diagnose van Azure Service Fabric-clusters.
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
ms.openlocfilehash: 89cd8e85c9902bb1caeedd80240811f59ebec409
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55187433"
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a>Aggregatie van de gebeurtenis en verzameling met Windows Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Wanneer u een Azure Service Fabric-cluster uitvoert, is het een goed idee om de logboeken te verzamelen van alle knooppunten in een centrale locatie. De logboeken die in een centrale locatie, helpt u bij het analyseren en oplossen van problemen in uw cluster, of problemen in de toepassingen en services die worden uitgevoerd in het cluster.

Eén manier om te uploaden en verzamelen van Logboeken is het gebruik van de extensie Windows Azure Diagnostics (WAD), die wordt logboeken geüpload naar Azure Storage en bevat ook een optie om Logboeken te verzenden naar Azure Application Insights of Eventhub. U kunt ook een extern proces gebruiken voor de gebeurtenissen kan lezen uit de opslag en plaats deze in een analyse platform product, zoals [Log Analytics](../log-analytics/log-analytics-service-fabric.md) of een andere oplossing voor het parseren van Logboeken.

## <a name="prerequisites"></a>Vereisten
In dit artikel worden de volgende hulpprogramma's gebruikt:

* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Resource Manager-sjabloon](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric-platform-events"></a>Service Fabric-platformgebeurtenissen
Service Fabric stelt u met een paar [out-of-the-box logboekkanalen](service-fabric-diagnostics-event-generation-infra.md), van die de volgende kanalen vooraf geconfigureerd met de extensie zijn voor het verzenden van bewaking en diagnostische gegevens aan een opslagtabel of elders:
  * [Operationele gebeurtenissen](service-fabric-diagnostics-event-generation-operational.md): op een hoger niveau bewerkingen die de Service Fabric-platform wordt uitgevoerd. Voorbeelden zijn onder meer het maken van toepassingen en services, knooppunt statuswijzigingen en upgrade-informatie. Deze worden verzonden als logboeken van Event Tracing voor Windows (ETW)
  * [Reliable Actors programming modelgebeurtenissen](service-fabric-reliable-actors-diagnostics.md)
  * [Reliable Services programming modelgebeurtenissen](service-fabric-reliable-services-diagnostics.md)

## <a name="deploy-the-diagnostics-extension-through-the-portal"></a>De extensie voor diagnostische gegevens via de portal implementeren
De eerste stap bij het verzamelen van Logboeken wordt de extensie voor diagnostische gegevens op de virtuele machine scale set knooppunten in het Service Fabric-cluster implementeren. De extensie voor diagnostische gegevens verzamelt logboeken op elke virtuele machine en geüpload naar het opslagaccount dat u opgeeft. De volgende stappen wordt beschreven hoe u dit doen voor nieuwe en bestaande clusters via de Azure-portal en Azure Resource Manager-sjablonen.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a>De extensie voor diagnostische gegevens als onderdeel van het maken van clusters via Azure portal implementeren
Bij het maken van uw cluster in de stap voor het cluster configureren, vouwt u de optionele instellingen en zorg ervoor dat er diagnostische gegevens is ingesteld op **op** (de standaardinstelling).

![Azure Diagnostics-instellingen in de portal voor het maken van clusters](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics-new.png)

We raden u aan dat u de sjabloon downloaden **voordat u klikt op maken** in de laatste stap. Raadpleeg voor meer informatie, [een Service Fabric-cluster instellen met behulp van een Azure Resource Manager-sjabloon](service-fabric-cluster-creation-via-arm.md). U moet de sjabloon te wijzigen op welke kanalen (Zie hierboven) voor het verzamelen van gegevens uit.

![Clustersjabloon](media/service-fabric-diagnostics-event-aggregation-wad/download-cluster-template.png)

Nu dat u bij het samenvoegen van gebeurtenissen in Azure Storage, [Log Analytics instellen](service-fabric-diagnostics-oms-setup.md) inzicht en query's voor uitvoert in de Log Analytics-portal

>[!NOTE]
>Er is momenteel geen manier om te filteren of opschonen van de gebeurtenissen die worden verzonden naar de tabellen. Als u een proces voor het verwijderen van gebeurtenissen uit de tabel niet implementeert, de tabel blijft groeien (de limiet van de standaardwaarde is 50 GB). Instructies over het wijzigen van deze zijn [verderop in dit artikel](service-fabric-diagnostics-event-aggregation-wad.md#update-storage-quota). Er is bovendien een voorbeeld van een gegevensservice opschonen die wordt uitgevoerd in de [Watchdog voorbeeld](https://github.com/Azure-Samples/service-fabric-watchdog-service), en wordt aanbevolen dat u een zelf ook, schrijven, tenzij er is een goede reden voor het opslaan van Logboeken na een periode 30 of 90 dagen.

## <a name="deploy-the-diagnostics-extension-through-azure-resource-manager"></a>De extensie voor diagnostische gegevens via Azure Resource Manager implementeren

### <a name="create-a-cluster-with-the-diagnostics-extension"></a>Een cluster maken met de extensie voor diagnostische gegevens
Voor het maken van een cluster met behulp van Resource Manager, moet u de configuratie van diagnostische JSON toevoegen aan de volledige Resource Manager-sjabloon. Wij bieden een voorbeeld van vijf VM-cluster Resource Manager-sjabloon met configuratie van diagnostische toegevoegd als onderdeel van onze voorbeelden Resource Manager-sjabloon. Op deze locatie in de galerie met voorbeelden van Azure kunt u deze bekijken: [Cluster met vijf knooppunten met een voorbeeld van diagnostische Resource Manager-sjabloon](https://azure.microsoft.com/resources/templates/service-fabric-secure-cluster-5-node-1-nodetype/).

Als u de instelling voor diagnostische gegevens in het Resource Manager-sjabloon, open het bestand azuredeploy.json en zoeken naar **IaaSDiagnostics**. Voor het maken van een cluster met behulp van deze sjabloon, selecteert u de **implementeren in Azure** knop beschikbaar op de vorige koppeling.

U kunt ook u kunt het Resource Manager-voorbeeld downloaden, wijzigingen aanbrengen en een cluster maken met de gewijzigde sjabloon met behulp van de `New-AzureRmResourceGroupDeployment` opdracht in een Azure PowerShell-venster. Zie de volgende code voor de parameters die u aan de opdracht doorgeeft. Zie het artikel voor gedetailleerde informatie over het implementeren van een resourcegroep met behulp van PowerShell [implementeren van een resourcegroep met de Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-template-deploy.md).

### <a name="add-the-diagnostics-extension-to-an-existing-cluster"></a>De extensie voor diagnostische gegevens toevoegen aan een bestaand cluster
Hebt u een bestaand cluster die geen diagnostische gegevens die zijn geïmplementeerd, kunt u deze kunt toevoegen of bijwerken via de clustersjabloon. Wijzig de Resource Manager-sjabloon die wordt gebruikt voor het maken van het bestaande cluster of de sjabloon downloaden via de portal zoals eerder beschreven. Wijzig het template.json-bestand door het uitvoeren van de volgende taken:

Geen nieuwe opslagresource toevoegen aan de sjabloon door toe te voegen aan de sectie met resources.

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

 Voeg vervolgens toe aan de parametersectie direct na de definities van storage-account, tussen `supportLogStorageAccountName`. Vervang de tijdelijke aanduiding voor tekst *storage-accountnaam komt hier* met de naam van het opslagaccount dat u wilt.

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
Werk vervolgens de `VirtualMachineProfile` sectie van het template.json-bestand door de volgende code binnen de matrix extensies toe te voegen. Zorg ervoor dat een door komma's toevoegen aan het begin of einde, afhankelijk van waar het ingevoegd.

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

Nadat u het template.json-bestand hebt gewijzigd zoals wordt beschreven, publiceert u de Resource Manager-sjabloon opnieuw. Als de sjabloon is geëxporteerd, het bestand deploy.ps1 uitvoert rapport opnieuw publiceert de sjabloon. Nadat u hebt geïmplementeerd, zorg ervoor dat **ProvisioningState** is **geslaagd**.

> [!TIP]
> Als u containers kunt implementeren met uw cluster, inschakelen WAD om op te halen docker statistieken door toe te voegen aan uw **WadCfg > DiagnosticMonitorConfiguration** sectie.
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

Sinds de tabellen die is gevuld met de extensie toeneemt totdat het quotum is bereikt, kunt u rekening houden met het quotum te verkleinen. De standaardwaarde is 50 GB en kan worden geconfigureerd in de sjabloon onder de `overallQuotaInMB` veld onder `DiagnosticMonitorConfiguration`

```json
"overallQuotaInMB": "50000",
```

## <a name="log-collection-configurations"></a>Meld u verzameling configuraties
Logboeken van extra kanalen zijn ook beschikbaar voor de verzameling, Hier volgen enkele van de meest voorkomende configuraties die kunt u in de sjabloon voor clusters die worden uitgevoerd in Azure.

* Operationele kanaal - Base: Standaard ingeschakeld, op hoog niveau bewerkingen die worden uitgevoerd door de Service Fabric en het cluster, met inbegrip van gebeurtenissen voor een knooppunt te komen, een nieuwe toepassing wordt geïmplementeerd of een upgrade terugdraaien, enzovoort. Raadpleeg voor een lijst van gebeurtenissen, [operationele kanaal gebeurtenissen](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-operational).
  
```json
      scheduledTransferKeywordFilter: "4611686018427387904"
  ```
* Operationele kanaal - beschreven: Dit omvat beslissingen, plus alles in de basis operationele kanaal voor taakverdeling en statusrapporten. Deze gebeurtenissen worden gegenereerd door het systeem of uw code met behulp van de status of API's van reporting zoals load [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) of [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). Om weer te geven deze gebeurtenissen in Visual Studio diagnostische logboeken toevoegen ' Microsoft-ServiceFabric:4:0x4000000000000008 ' aan de lijst van ETW-providers.

```json
      scheduledTransferKeywordFilter: "4611686018427387912"
  ```

* Wanneer u gegevens en Berichtenkanaal: Kritieke logboeken en gebeurtenissen die worden gegenereerd in de berichten (momenteel alleen de ReverseProxy) en het gegevenspad, ook naar kanaal met gedetailleerde operationele Logboeken. Deze gebeurtenissen zijn fouten en andere belangrijke problemen in de ReverseProxy verwerking van aanvragen, evenals aanvragen verwerkt. **Dit is onze aanbeveling voor uitgebreide logboekregistratie**. Als u wilt deze gebeurtenissen in Visual Studio diagnostische logboeken weergeven, toevoegen ' Microsoft-ServiceFabric:4:0x4000000000000010 ' aan de lijst van ETW-providers.

```json
      scheduledTransferKeywordFilter: "4611686018427387928"
  ```

* Gegevens & Berichtenkanaal - beschreven: Uitgebreide kanaal met alle niet-kritieke logboeken van gegevens en berichten in het cluster en de gedetailleerde operationele kanaal. Voor gedetailleerde probleemoplossing voor alle omgekeerde proxy gebeurtenissen, verwijzen naar de [omgekeerde proxy diagnostics handleiding](service-fabric-reverse-proxy-diagnostics.md).  Als u wilt deze gebeurtenissen in Visual Studio diagnostische logboeken weergeven, toevoegen ' Microsoft-ServiceFabric:4:0x4000000000000020 ' aan de lijst van ETW-providers.

```json
      scheduledTransferKeywordFilter: "4611686018427387944"
  ```

>[!NOTE]
>Dit kanaal is een zeer groot aantal gebeurtenissen, waardoor gebeurtenissen verzamelen uit deze gedetailleerde resultaten van kanaal in een groot aantal traceringen snel wordt gegenereerd en opslagcapaciteit kan gebruiken. Alleen dit inschakelen als dit absoluut noodzakelijk.


Om in te schakelen de **Base operationele kanaal** onze aanbeveling voor uitgebreide logboekregistratie met zo min mogelijk ruis, de `EtwManifestProviderConfiguration` in de `WadCfg` van uw sjabloon er als volgt:

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
```

## <a name="collect-from-new-eventsource-channels"></a>Verzamelen van nieuwe kanalen voor de gebeurtenisbron

Voor het bijwerken van diagnostische gegevens voor het verzamelen van Logboeken van nieuwe kanalen voor de gebeurtenisbron die staan voor een nieuwe toepassing bent over het implementeren, uit te voeren dezelfde cluster stappen zoals eerder beschreven voor het instellen van diagnostische gegevens voor een bestaand.

Update de `EtwEventSourceProviderConfiguration` sectie in het template.json-bestand om toe te voegen vermeldingen voor de nieuwe kanalen voor de gebeurtenisbron voordat u de configuratie van de toepassing met behulp van bijwerken de `New-AzureRmResourceGroupDeployment` PowerShell-opdracht. De naam van de bron van de gebeurtenis wordt gedefinieerd als onderdeel van uw code in het bestand ServiceEventSource.cs Visual Studio gegenereerd.

Bijvoorbeeld, als uw gebeurtenisbron de naam van mijn Eventsource, de volgende code om de gebeurtenissen uit mijn Eventsource in een tabel met de naam MyDestinationTableName toevoegen.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Voor het verzamelen van prestatiemeteritems of gebeurtenislogboeken, kunt u de Resource Manager-sjabloon aanpassen met behulp van de voorbeelden die zijn opgegeven [maken van een Windows-machine met controle en diagnostiek met behulp van een Azure Resource Manager-sjabloon](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Vervolgens publiceert u de Resource Manager-sjabloon opnieuw.

## <a name="collect-performance-counters"></a>Verzamelen van prestatiemeteritems

Voor het verzamelen van metrische gegevens voor prestaties van uw cluster, moet u de prestatiemeteritems toevoegen aan uw "WadCfg > DiagnosticMonitorConfiguration' in het Resource Manager-sjabloon voor uw cluster. Zie [prestatiebewaking met WAD](service-fabric-diagnostics-perf-wad.md) voor stapsgewijze instructies over het aanpassen van uw `WadCfg` voor het verzamelen van specifieke prestatiemeteritems. Naslaginformatie over [Service Fabric-prestatiemeteritems](service-fabric-diagnostics-event-generation-perf.md) voor items die voor een lijst van de prestaties wordt aangeraden verzamelen.
  
Als u een Application Insights-sink, zoals beschreven in de onderstaande sectie, en wilt dat deze metrische gegevens worden weergegeven in Application Insights, Controleer of voor het toevoegen van de naam van de sink in de sectie 'sinks' zoals u hierboven ziet. Hiermee wordt de prestatiemeteritems die afzonderlijk worden geconfigureerd automatisch verzonden naar uw Application Insights-resource.


## <a name="send-logs-to-application-insights"></a>Logboeken verzenden naar Application Insights

Controle en diagnostische gegevens verzenden naar Application Insights (AI) kan worden gedaan als onderdeel van de configuratie van de WAD. Als u u AI gebruikt voor de gebeurtenis analyses en visualisatie besluit, leest u [over het instellen van een AI-sink](service-fabric-diagnostics-event-analysis-appinsights.md#add-the-application-insights-sink-to-the-resource-manager-template) als onderdeel van uw 'WadCfg'.

## <a name="next-steps"></a>Volgende stappen

Wanneer u Azure diagnostics correct hebt geconfigureerd, ziet u gegevens in uw Storage-tabellen van Logboeken van de ETW- en gebeurtenisbron. Als u gebruikmaken van Log Analytics, Kibana of een andere analyse- en visualisatieservice gegevensplatform die niet rechtstreeks in het Resource Manager-sjabloon is geconfigureerd wilt, moet u voor het instellen van het platform van uw keuze in de gegevens uit deze opslagtabellen worden gelezen. Hierdoor voor Log Analytics is relatief eenvoudig en wordt uitgelegd in [analyse van gebeurtenis- en logboekbestanden](service-fabric-diagnostics-event-analysis-oms.md). Application Insights is een deel van een speciaal geval in dit opzicht, omdat deze kan worden geconfigureerd als onderdeel van de configuratie van de extensie voor diagnostische gegevens, zodat u verwijzen naar de [juiste artikel](service-fabric-diagnostics-event-analysis-appinsights.md) als u AI gebruikt.

>[!NOTE]
>Er is momenteel geen manier om te filteren of opschonen van de gebeurtenissen die worden verzonden naar de tabel. Als een proces voor het verwijderen van gebeurtenissen uit de tabel niet is geïmplementeerd, blijft de tabel om te groeien. Er is momenteel een voorbeeld van een gegevensservice opschonen die wordt uitgevoerd in de [Watchdog voorbeeld](https://github.com/Azure-Samples/service-fabric-watchdog-service), en wordt aanbevolen dat u een zelf ook, schrijven, tenzij er is een goede reden voor het opslaan van Logboeken na een periode 30 of 90 dagen.

* [Meer informatie over het verzamelen van prestatiemeteritems of de logboeken met behulp van de extensie voor diagnostische gegevens](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Gebeurtenis analyses en visualisatie met Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Gebeurtenis analyses en visualisatie met Log Analytics](service-fabric-diagnostics-event-analysis-oms.md)
