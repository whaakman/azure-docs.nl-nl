---
title: Aggregatie van Azure Service Fabric-gebeurtenis met Windows Azure Diagnostics | Microsoft Docs
description: Meer informatie over het aggregeren en het verzamelen van gebeurtenissen met af voor controle en diagnostische gegevens van Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: dekapur
ms.openlocfilehash: 8e6c82aa60544d672bb249d589b63d55b48309fe
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a>Gebeurtenis-aggregatie en verzameling op basis van Windows Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Wanneer u een Azure Service Fabric-cluster uitvoert, is het een goed idee de logboeken verzamelen van alle knooppunten in een centrale locatie. De logboeken die in een centrale locatie, helpt u bij het analyseren en oplossen van problemen in het cluster of problemen in de toepassingen en services die worden uitgevoerd in het cluster.

Een manier om te uploaden en verzamelen van Logboeken is het gebruik van de Windows Azure Diagnostics (af)-extensie, die Logboeken uploadt naar Azure Storage, en heeft ook de optie om Logboeken te verzenden naar Azure Application Insights of Event Hubs. U kunt ook een extern proces gebruiken om te lezen van de gebeurtenissen uit de opslag en plaats deze in een analyse platform-product, zoals [OMS Log Analytics](../log-analytics/log-analytics-service-fabric.md) of een andere oplossing voor het parseren van het logboek.

## <a name="prerequisites"></a>Vereisten
Deze hulpprogramma's worden gebruikt voor het uitvoeren van de bewerkingen in dit document:

* [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (gerelateerd aan Azure Cloud Services, maar heeft goede informatie over en voorbeelden)
* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Resource Manager-client](https://github.com/projectkudu/ARMClient)
* [Azure Resource Manager-sjabloon](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-and-event-sources"></a>Logboekbestand en de gebeurtenis

### <a name="service-fabric-platform-events"></a>Gebeurtenissen voor service Fabric-platform
Zoals beschreven in [in dit artikel](service-fabric-diagnostics-event-generation-infra.md), Service Fabric stelt u met enkele out-of-the-box-logboekregistratie kanalen, waarvan de volgende kanalen eenvoudig zijn geconfigureerd met af te verzenden bewaking en diagnostische gegevens naar een tabel opslag of elders:
  * Operationele gebeurtenissen: een hoger niveau bewerkingen waarmee de Service Fabric-platform. Voorbeelden zijn onder meer het maken van toepassingen en services, knooppunt statuswijzigingen en informatie over de upgrade. Deze worden verzonden als Logboeken Event Tracing voor Windows (ETW)
  * [Reliable Actors programming model gebeurtenissen](service-fabric-reliable-actors-diagnostics.md)
  * [Reliable Services programming model gebeurtenissen](service-fabric-reliable-services-diagnostics.md)

### <a name="application-events"></a>Toepassingsgebeurtenissen
 Gebeurtenissen van uw toepassingen en services code verzonden en geschreven met behulp van de EventSource helperklasse opgegeven in de Visual Studio-sjablonen. Zie voor meer informatie over het schrijven van EventSource logboeken van uw toepassing [bewaken en onderzoeken van services in de instellingen voor een lokale computer ontwikkeling](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-the-diagnostics-extension"></a>De extensie voor diagnostische gegevens implementeren
De eerste stap bij het verzamelen van Logboeken is voor het implementeren van de extensie voor diagnostische gegevens over elk van de virtuele machines in het Service Fabric-cluster. De extensie voor diagnostische gegevens verzamelt logboeken op elke virtuele machine en geüpload naar het opslagaccount dat u opgeeft. De stappen verschillen enigszins op basis van of u de Azure portal of Azure Resource Manager gebruiken. De stappen ook variëren, afhankelijk van of de implementatie maakt deel uit van het maken van het cluster of voor een cluster dat al bestaat. Bekijk de stappen voor elk scenario.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a>De extensie voor diagnostische gegevens als onderdeel van het maken van het cluster via Azure portal implementeren
Als u wilt implementeren de extensie voor diagnostische gegevens op de virtuele machines in het cluster als onderdeel van het maken van het cluster, die u kunt het deelvenster van de instellingen voor diagnostische gegevens weergegeven in de volgende afbeelding: Controleer of de diagnostische gegevens is ingesteld op **op** (de standaardinstelling). Nadat u het cluster hebt gemaakt, kunt u deze instellingen niet wijzigen met behulp van de portal.

![Azure Diagnostics-instellingen in de portal voor het maken van het cluster](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics.png)

Wanneer u een cluster met behulp van de portal maakt, wordt ten zeerste aanbevolen dat u de sjabloon downloaden **voordat u op OK** om het cluster te maken. Raadpleeg voor meer informatie, [een Service Fabric-cluster instellen met behulp van een Azure Resource Manager-sjabloon](service-fabric-cluster-creation-via-arm.md). U moet de sjabloon wijzigingen later aanbrengen omdat u geen enkele wijzigingen aanbrengen met behulp van de portal.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>De extensie voor diagnostische gegevens als onderdeel van het maken van het cluster implementeren met behulp van Azure Resource Manager
Voor het maken van een cluster met Resource Manager, moet u de configuratie van diagnostische JSON toevoegen aan het volledige cluster Resource Manager-sjabloon voordat u het cluster maakt. We bieden een voorbeeldsjabloon vijf VM-cluster Resource Manager met de configuratie van diagnostische toegevoegd als onderdeel van onze voorbeelden Resource Manager-sjabloon. U kunt het zien op deze locatie in de galerie van Azure Samples: [cluster met vijf knooppunten met diagnostische gegevens van Resource Manager-sjabloon voorbeeld](https://azure.microsoft.com/en-in/resources/templates/service-fabric-secure-cluster-5-node-1-nodetype/).

Als u de instelling van de diagnostische gegevens in de Resource Manager-sjabloon, open het bestand azuredeploy.json en zoeken naar **IaaSDiagnostics**. Als u een cluster met behulp van deze sjabloon, selecteert de **implementeren in Azure** knop beschikbaar op de vorige koppeling.

U kunt ook u kunt het Resource Manager-voorbeeld downloaden, wijzigingen aanbrengen en een cluster maken met de gewijzigde sjabloon met behulp van de `New-AzureRmResourceGroupDeployment` opdracht in een Azure PowerShell-venster. Zie de volgende code voor de parameters die u aan de opdracht doorgeeft. Zie het artikel voor gedetailleerde informatie over het implementeren van een resourcegroep met behulp van PowerShell [een resourcegroep implementeren met de Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-template-deploy.md).

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>De extensie voor diagnostische gegevens aan een bestaand cluster implementeren
Als u een bestaand cluster dat geen diagnostische gegevens die zijn geïmplementeerd, of als u wilt wijzigen van een bestaande configuratie, kunt u deze kunt toevoegen of bijwerken. Wijzig de Resource Manager-sjabloon die wordt gebruikt voor het maken van het bestaande cluster of de sjabloon downloaden van de portal, zoals eerder beschreven. Het bestand template.json wijzigen door de volgende taken uitvoeren.

Een nieuwe opslagresource toevoegen aan de sjabloon door toe te voegen aan de bronnensectie.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Vervolgens voegt u het gedeelte parameters direct na de definities van de account opslag tussen `supportLogStorageAccountName` en `vmNodeType0Name`. Vervang de tijdelijke aanduiding voor tekst *opslagaccountnaam hier* met de naam van het opslagaccount.

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
      "defaultValue": "storage account name goes here",
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

## <a name="collect-health-and-load-events"></a>Health verzamelen en laden van gebeurtenissen

Beginnen met de 5,4 release van Service Fabric, zijn status en load metrische gebeurtenissen beschikbaar voor de verzameling. Deze gebeurtenissen gebeurtenissen die door het systeem of uw code worden gegenereerd met behulp van de status aangeven of rapportage-API's, zoals laden [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) of [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). Hierdoor kan voor het aggregeren en weergeven van systeemstatus na verloop van tijd en voor waarschuwingen op basis van status- of load-gebeurtenissen. Om weer te geven deze gebeurtenissen in Visual Studio diagnostische logboeken toevoegen ' Microsoft-ServiceFabric:4:0x4000000000000008 ' aan de lijst met ETW-providers.

Voor het verzamelen van gebeurtenissen in het cluster, wijzig de `scheduledTransferKeywordFilter` in de WadCfg van uw sjabloon Resource Manager `4611686018427387912`.

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387912",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

## <a name="collect-reverse-proxy-events"></a>Omgekeerde proxy-gebeurtenissen verzamelen

Beginnen met de 5.7 release van Service Fabric [omgekeerde proxy](service-fabric-reverseproxy.md) gebeurtenissen zijn beschikbaar voor de verzameling via de kanalen gegevens & Messaging. 

De omgekeerde proxy duwt alleen foutgebeurtenissen via de belangrijkste gegevens & Messaging kanaal - Aanvraagverwerking kritieke problemen en fouten opgetreden bij het weergeven. De gedetailleerde kanaal bevat uitgebreide gebeurtenissen over de aanvragen die zijn verwerkt door de omgekeerde proxy. 

Om weer te geven de foutgebeurtenissen in Visual Studio diagnostische logboeken toevoegen ' Microsoft-ServiceFabric:4:0x4000000000000010 ' aan de lijst met ETW-providers. Voor alle aanvraagtelemetrie update voor de Microsoft-ServiceFabric vermelding in de lijst voor ETW-provider voor ' Microsoft-ServiceFabric:4:0x4000000000000020 '.

Voor clusters worden uitgevoerd in Azure:

Wijzigen zodat de traceringen in de belangrijkste gegevens & Messaging-kanaal worden opgepikt, de `scheduledTransferKeywordFilter` waarde in de WadCfg van uw sjabloon Resource Manager `4611686018427387920`.

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387920",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

Gedetailleerde verwerking van alle gebeurtenissen verzamelt, schakelt u de gegevens & Messaging - kanaal door het wijzigen van de `scheduledTransferKeywordFilter` waarde in de WadCfg van uw sjabloon Resource Manager `4611686018427387936`.

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387936",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

Inschakelen van verzamelen van gebeurtenissen van deze gedetailleerde kanaal resultaten in een groot aantal traceringen snel wordt gegenereerd en opslagcapaciteit kan gebruiken. Alleen dit inschakelen als dit echt nodig.
Voor gedetailleerde probleemoplossing van de reverse proxy-gebeurtenissen, raadpleegt u de [omgekeerde proxy diagnostics handleiding](service-fabric-reverse-proxy-diagnostics.md).

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

Controle en diagnostische gegevens verzenden naar Application Insights (AI) kan worden uitgevoerd als onderdeel van de configuratie af. Als u besluit AI gebruiken voor analyse van gebeurtenis en visualisatie, lezen [analyse van gebeurtenis en visualisatie met Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) voor het instellen van een AI-Sink als onderdeel van uw 'WadCfg'.

## <a name="next-steps"></a>Volgende stappen

Wanneer u Azure diagnostics correct hebt geconfigureerd, ziet u gegevens in de tabellen bij opslag van de ETW- en EventSource Logboeken. Als u gebruiken OMS, Kibana of een andere gegevens analytics en visualisatie platform dat niet rechtstreeks in de Resource Manager-sjabloon is geconfigureerd wilt, moet u voor het instellen van het platform van uw keuze in de gegevens uit deze opslagtabellen te lezen. Hierdoor voor OMS is relatief eenvoudig en wordt uitgelegd in [analyse van gebeurtenis- en logboekbestanden via OMS](service-fabric-diagnostics-event-analysis-oms.md). Application Insights is een bit van een speciaal geval in dit opzicht, omdat deze kan worden geconfigureerd als onderdeel van de configuratie voor de Diagnostics-uitbreiding, dus raadpleegt u de [juiste artikel](service-fabric-diagnostics-event-analysis-appinsights.md) als u wilt gebruiken AI.

>[!NOTE]
>Er is momenteel geen manier om te filteren of Let op de gebeurtenissen die worden verzonden naar de tabel. Als u een proces voor het verwijderen van gebeurtenissen uit de tabel niet implementeert, wordt de tabel blijven groeien. Er is een voorbeeld van een gegevensservice opschonen uitgevoerd in de [Watchdog voorbeeld](https://github.com/Azure-Samples/service-fabric-watchdog-service), en wordt aanbevolen dat u één voor uzelf, schrijft tenzij er een goede reden voor het opslaan van Logboeken na een periode 30 of 90 dagen.

* [Meer informatie over het verzamelen van prestatiemeteritems of logboeken met behulp van de extensie voor diagnostische gegevens](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Analyse van gebeurtenis en visualisatie met Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Analyse van gebeurtenis en visualisatie met OMS](service-fabric-diagnostics-event-analysis-oms.md)
