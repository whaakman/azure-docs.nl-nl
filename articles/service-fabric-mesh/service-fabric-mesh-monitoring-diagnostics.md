---
title: Controle en diagnostische gegevens in Azure Service Fabric NET-toepassingen | Microsoft Docs
description: Meer informatie over controle en diagnose van toepassing in Service Fabric net op Azure.
services: service-fabric-mesh
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2019
ms.author: srrengar
ms.custom: mvc, devcenter
ms.openlocfilehash: 36c9a5d75c4a72365638619ab85d451df647feb3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64939828"
---
# <a name="monitoring-and-diagnostics"></a>Controle en diagnose
Azure Service Fabric Mesh is een volledig beheerde service waarmee ontwikkelaars microservices-toepassingen kunnen implementeren zonder virtuele machines, opslag of netwerken hoeven te beheren. Bewaking en diagnose voor Service Fabric-NET is onderverdeeld in drie belangrijkste typen diagnostische gegevens:

- Toepassingslogboeken - deze worden gedefinieerd als de logboeken van uw toepassingen in containers, op basis van hoe u uw toepassing hebt geïnstrumenteerd (bijvoorbeeld docker-Logboeken)
- Platform-gebeurtenissen - gebeurtenissen uit het Mesh-platform die relevant zijn voor uw containerbewerking, inclusief momenteel container activering en deactivering beëindiging.
- Metrische gegevens over container - resource-gebruik en prestatiemetrieken voor uw containers (docker statistieken)

Dit artikel worden de opties voor controle en diagnostische gegevens voor de meest recente preview-versie beschikbaar.

## <a name="application-logs"></a>Toepassingslogboeken

U kunt uw docker-logboeken van uw geïmplementeerde containers, op basis van per container bekijken. Elke container is in het model van de toepassing Service Fabric net een codepakket in uw toepassing. Als u wilt zien van de bijbehorende logboeken met een codepakket, gebruik de volgende opdracht:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --app-name <nameOfApp> --service-name <nameOfService> --replica-name <nameOfReplica> --code-package-name <nameOfCodePackage>
```

> [!NOTE]
> U kunt de opdracht "az NET service-replica" gebruiken om op te halen van de replicanaam van de. Namen van de replica zijn gehele getallen van 0 verhogen.

Hier ziet u hoe dit eruitziet voor het bekijken van de logboeken van de container VotingWeb.Code van de stemtoepassing:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --application-name SbzVoting --service-name VotingWeb --replica-name 0 --code-package-name VotingWeb.Code
```

## <a name="container-metrics"></a>Metrische containergegevens 

De NET-omgeving beschrijft een aantal metrische gegevens die aangeeft hoe uw containers worden uitgevoerd. De volgende metrische gegevens zijn beschikbaar via de Azure portal en Azure CLI bewaken:

| Gegevens | Description | Eenheden|
|----|----|----|
| CpuUtilization | ActualCpu/AllocatedCpu als een percentage | % |
| MemoryUtilization | ActualMem/AllocatedMem als een percentage | % |
| AllocatedCpu | CPU toegewezen aan de hand van Azure Resource Manager-sjabloon | Millicores |
| AllocatedMemory | Geheugen dat is toegewezen aan de hand van Azure Resource Manager-sjabloon | MB |
| ActualCpu | CPU-gebruik | Millicores |
| ActualMemory | Geheugengebruik | MB |
| ContainerStatus | 0 - ongeldig: De de containerstatus is onbekend <br> 1 - in behandeling: De container is gepland om te starten <br> 2 - Starting: De container wordt momenteel gestart <br> 3 - aan de slag: De container is gestart <br> 4 - Stopping: De container wordt gestopt <br> 5 - gestopt: De container is gestopt | N/A |
| ApplicationStatus | 0 - Onbekend: De status kan niet worden opgehaald <br> 1 - ready: De toepassing wordt correct uitgevoerd <br> 2 - upgrade: Er is een upgrade wordt uitgevoerd <br> 3 - maken: De toepassing wordt gemaakt <br> 4 - verwijderen: De toepassing wordt verwijderd <br> 5 - is mislukt: De toepassing kan niet implementeren | N/A |
| ServiceStatus | 0 - ongeldig: De service heeft momenteel geen een health-status <br> 1 - Ok: De service is in orde  <br> 2 - Waarschuwing: Mogelijk zijn er iets mis onderzoek vereisen <br> 3 - fout: Er is iets mis die onderzoek moet <br> 4 - onbekend: De status kan niet worden opgehaald | N/A |
| ServiceReplicaStatus | 0 - ongeldig: De replica heeft momenteel geen een health-status <br> 1 - Ok: De service is in orde  <br> 2 - Waarschuwing: Mogelijk zijn er iets mis onderzoek vereisen <br> 3 - fout: Er is iets mis die onderzoek moet <br> 4 - onbekend: De status kan niet worden opgehaald | N/A | 
| RestartCount | Aantal opnieuw opstarten van container | N/A |

> [!NOTE]
> De ServiceStatus en ServiceReplicaStatus waarden zijn gelijk aan de [HealthState](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate?view=azure-dotnet) in Service Fabric. 

Elke waarde is beschikbaar in verschillende dimensies, zodat u statistische functies op verschillende niveaus kunt zien. De huidige lijst van dimensies zijn als volgt:

* ApplicationName
* ServiceName
* ServiceReplicaName
* CodePackageName

> [!NOTE]
> De dimensie CodePackageName is niet beschikbaar voor Linux-toepassingen. 

Elke dimensie komt overeen met verschillende onderdelen van de [Service Fabric-toepassingsmodel](service-fabric-mesh-service-fabric-resources.md#applications-and-services)

### <a name="azure-monitor-cli"></a>Azure Monitor CLI

Een volledige lijst met opdrachten is beschikbaar in de [CLI van Azure Monitor-documenten](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list) , maar hebben we een aantal handige voorbeelden hieronder opgenomen 

In elk voorbeeld wordt volgt de Resource-ID dit patroon

`"/subscriptions/<your sub ID>/resourcegroups/<your RG>/providers/Microsoft.ServiceFabricMesh/applications/<your App name>"`


* CPU-gebruik van de containers in een toepassing

```cli
    az monitor metrics list --resource <resourceId> --metric "CpuUtilization"
```
* Geheugengebruik voor elke Service-Replica
```cli
    az monitor metrics list --resource <resourceId> --metric "MemoryUtilization" --dimension "ServiceReplicaName"
``` 

* Opnieuw opstarten voor elke container in een venster 1 uur 
```cli
    az monitor metrics list --resource <resourceId> --metric "RestartCount" --start-time 2019-02-01T00:00:00Z --end-time 2019-02-01T01:00:00Z
``` 

* Gemiddelde CPU-gebruik van services met de naam 'VotingWeb' in een venster 1 uur
```cli
    az monitor metrics list --resource <resourceId> --metric "CpuUtilization" --start-time 2019-02-01T00:00:00Z --end-time 2019-02-01T01:00:00Z --aggregation "Average" --filter "ServiceName eq 'VotingWeb'"
``` 

### <a name="metrics-explorer"></a>Metrics explorer

Metrics explorer wordt een blade in de portal waarin u de metrische gegevens voor uw toepassing Mesh kunt visualiseren. Deze blade is toegankelijk in de pagina van de toepassing in de portal en de Azure monitor-blade, de laatste die u gebruiken kunt om metrische gegevens voor al uw Azure-resources die ondersteuning bieden voor Azure Monitor weer te geven. 

![Metrics Explorer](./media/service-fabric-mesh-monitoring-diagnostics/metricsexplorer.png)


<!--
### Container Insights

In addition to the metrics explorer, we also have a dashboard available out of the box that shows sample metrics over time under the Insights blade in the application's page in the portal. 

![Container Insights](./media/service-fabric-mesh-monitoring-diagnostics/containerinsights.png)
-->

## <a name="next-steps"></a>Volgende stappen
* Lees [Wat is Service Fabric Mesh?](service-fabric-mesh-overview.md) voor meer informatie over Service Fabric Mesh.
* Bekijk voor meer informatie over de opdrachten van Azure Monitor metrics, de [CLI van Azure Monitor-documenten](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list).
