---
title: Azure Service Fabric-capaciteit plannen en best practices voor schalen | Microsoft Docs
description: Aanbevolen procedures voor het plannen en schalen van Service Fabric-clusters en toepassingen.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: c3b97ee7181d6475b7271d5ae4bcea0f6c9ff2b3
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56805219"
---
# <a name="capacity-planning-and-scaling"></a>Capaciteitsplanning en schalen

Voordat u een Azure Service Fabric-cluster maken of die als host fungeert voor uw cluster rekenresources vergroten/verkleinen, is het belangrijk voor het plannen van capaciteit. Zie voor meer informatie over het plannen van capaciteit [Planning van de capaciteit van de Service Fabric-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity). Naast u overweegt Nodetype en Cluster kenmerken, plan voor het schalen herverdelen duurt langer dan een uur om uit te voeren voor een productie-omgeving, ongeacht het aantal virtuele machines die u wilt toevoegen.

## <a name="auto-scaling"></a>Automatisch schalen
Schaalbewerkingen moet worden uitgevoerd via Azure-Resource-sjabloonimplementatie, omdat het is een best practice om te behandelen [resourceconfiguraties als code]( https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code), en het gebruik van virtuele-Machineschaalset automatisch schalen resulteert in uw samengestelde Resource Manager-sjabloon koppelingsvelden definiëren van uw virtuele-machineschaalset instellen aantal instanties van; verhoging van het risico van toekomstige implementaties onbedoelde vergroten / verkleinen veroorzaakt, en in het algemeen moet u automatisch schalen als:

* Uw Resource Manager-sjablonen implementeren met de juiste capaciteit gedeclareerd biedt geen ondersteuning voor uw situatie.
  * Naast het handmatig schalen, kunt u configureren een [continue integratie en Leveringspijplijn in Azure DevOps-Services met behulp van Azure Resource Group deployment projecten]( https://docs.microsoft.com/azure/vs-azure-tools-resource-groups-ci-in-vsts), die vaak wordt geactiveerd door een logische App die gebruikmaakt van maatstaven voor prestaties van virtuele machine opgevraagd uit [Azure Monitor REST API](https://docs.microsoft.com/azure/azure-monitor/platform/rest-api-walkthrough); effectief automatisch schalen op basis van welke gegevens u wilt, terwijl het optimaliseren van voor Azure Resource Manager is de toegevoegde waarde.
* U hoeft alleen te horizontaal schalen 1 VM scale set knooppunt tegelijk.
  * Om te worden geschaald door 3 of meer knooppunten tegelijk, moet u [schalen van een Service Fabric-cluster af door het toevoegen van een virtuele-Machineschaalset](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out), en deze is het verstandig om in te schalen en van virtuele-Machineschaalset stelt u horizontaal 1 knooppunt tegelijk.
* U hebt op de betrouwbaarheid van Silver of hoger voor uw Service Fabric-Cluster, en Silver duurzaamheid of hoger op elke schaal Set u regels voor automatisch schalen configureren.
  * Capaciteit voor regels voor automatisch schalen [minimale] moet gelijk zijn aan of groter zijn dan 5 VM-exemplaren en moet gelijk zijn aan of groter zijn dan de minimale Betrouwbaarheidslaag voor het primaire knooppunttype.

> [!NOTE]
> Azure Service Fabric stateful service fabric: / System/InfastructureService/< NODE_TYPE_NAME > wordt uitgevoerd op elke knooppunttype waarvoor Silver- of een hogere duurzaamheid, dit is de enige System Service die wordt ondersteund om uit te voeren in Azure op een van uw clusters knooppunttypen . 

## <a name="vertical-scaling-considerations"></a>Overwegingen voor verticaal vergroten/verkleinen

[Verticaal schalen](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out#upgrade-the-size-and-operating-system-of-the-primary-node-type-vms) een knooppunttype in Azure Service Fabric is een aantal stappen en overwegingen bij vereist. Bijvoorbeeld:
* Het cluster moet in orde zijn voor het schalen. Anders wordt u alleen cluster verder instabiel.
* **Duurzaamheid niveau of hoger van Silver** is vereist voor alle Service Fabric-Cluster NodeTypes die als host van stateful services.

> [!NOTE]
> Het primaire NodeType die als host fungeert voor Stateful Service Fabric-systeemservices moet Silver duurzaamheid niveau of hoger. Zodra het inschakelen van silver duurzaamheid, clusterbewerkingen, zoals upgrades, zijn toevoegen of verwijderen van knooppunten, enzovoort lager omdat het systeem is geoptimaliseerd voor de veiligheid van gegevens via de snelheid van bewerkingen.

Verticaal schalen met een virtuele-Machineschaalset is een destructieve bewerking. In plaats daarvan horizontaal schalen van uw cluster door toe te voegen een nieuwe Schaalset met de gewenste SKU en uw services migreren naar uw gewenste SKU om een veilige verticaal vergroten/verkleinen bewerking te voltooien. Wijzigen van een virtuele-Machineschaalset resource SKU is een destructieve bewerking omdat deze werkrolgrootte uw hosts die Hiermee verwijdert u alle lokaal permanente status.

Service Fabric [knooppunteigenschappen en plaatsingsbeperkingen](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-cluster-description#node-properties-and-placement-constraints) worden gebruikt door uw cluster om te bepalen waar u voor het hosten van de services van uw toepassingen. Wanneer identieke eigenschapswaarden voor verticaal schalen het primaire knooppunttype declareren `"nodeTypeRef"`, die is gevonden in Virtual Machine Scale ingesteld Service Fabric-uitbreiding. Het volgende fragment van het Resource Manager-sjabloon bevat de eigenschappen die u declareert, met dezelfde waarde voor uw nieuw ingerichte schaalsets die u naar te schalen zijn en wordt alleen ondersteund als een tijdelijke stateful voor uw cluster:

```json
"settings": {
   "nodeTypeRef": ["[parameters('primaryNodetypeName')]"]
}
```

> [!NOTE]
> Laat uw cluster uitgevoerd met meerdere schaalsets die gebruikmaken van dezelfde niet `nodeTypeRef` eigenschapswaarde langer zijn dan nodig is om een geslaagde verticaal vergroten/verkleinen bewerking te voltooien.
> Valideer altijd bewerkingen in een testomgeving voordat u wijzigingen in de productie-omgeving. Service Fabric-Cluster-systeemservices hebben standaard een beperking plaatsing om alleen het primaire nodetype.

Met de eigenschappen van het knooppunt en plaatsingsbeperkingen gedeclareerd, door de volgende stappen uit één VM-exemplaar op een tijdstip te doen. Hiermee worden de systeemservices (en de stateful services) zonder problemen worden afgesloten op de VM-exemplaar dat u verwijderen wilt nadat er nieuwe replica's ergens anders zijn gemaakt.
1. Voer vanuit PowerShell, `Disable-ServiceFabricNode` met opzet 'RemoveNode' naar het knooppunt uitschakelen u gaat verwijderen. Verwijder het knooppunttype met het hoogste getal. Als u een cluster met zes knooppunten hebt, bijvoorbeeld het exemplaar van de virtuele machine "MyNodeType_5" verwijderen.
2. Voer `Get-ServiceFabricNode` om ervoor te zorgen dat het knooppunt is overgegaan op uitgeschakeld. Als dat niet het geval is, wacht totdat het knooppunt is uitgeschakeld. Dit duurt een paar uur voor elk knooppunt. Niet worden voortgezet totdat het knooppunt is overgegaan op uitgeschakeld.
3. Verminder het aantal virtuele machines met één in het desbetreffende type knooppunt. Het hoogste VM-exemplaar wordt nu verwijderd.
4. Herhaal stappen 1 t/m 3 indien nodig, maar niet verkleinen het aantal exemplaren in het primaire knooppunttypen die kleiner is dan wat de betrouwbaarheidslaag garandeert. Zie [Planning van de capaciteit van de Service Fabric-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) voor een lijst met aanbevolen exemplaren.

## <a name="horizontal-scaling"></a>Horizontaal schalen

Horizontaal schalen in Service Fabric kan worden gedaan [handmatig](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down) of [programmatisch](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-programmatic-scaling).

> [!NOTE]
> Als u een knooppunttype met silver- of gold duurzaamheid zijn schalen, wordt schalen traag zijn.

### <a name="scaling-out"></a>Uitschalen

Een Service Fabric-cluster uitbreiden door het verhogen van het aantal exemplaren voor een bepaalde virtuele-Machineschaalset opgehaald. U kunt opwaarts schalen via een programma met behulp van de Azure- en de ID voor de gewenste schaal instellen om de capaciteit te vergroten.

```c#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

Als u wilt schalen handmatig, werk de capaciteit in de SKU-eigenschap van de gewenste [virtuele-Machineschaalset](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) resource.
```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

### <a name="scaling-in"></a>Inschalen

Inschalen vereist meer betrokken dan uitschalen. Bijvoorbeeld:
* Service Fabric-systeemservices worden uitgevoerd in het primaire knooppunttype in uw cluster. Nooit afgesloten of verkleinen het aantal exemplaren voor dat knooppunttype zodat er minder exemplaren dan wat de betrouwbaarheidslaag garandeert. 
* Voor een stateful service moet u een bepaald aantal knooppunten die altijd van beschikbaarheid en de status van uw service behouden. Ten minste moet u een aantal knooppunten gelijk zijn aan het aantal doel replica instellen van de partitie/service.

Als u wilt handmatig schalen de volgende stappen uit:

1. Voer vanuit PowerShell, `Disable-ServiceFabricNode` met opzet 'RemoveNode' naar het knooppunt uitschakelen u gaat verwijderen. Verwijder het knooppunttype met het hoogste getal. Als u een cluster met zes knooppunten hebt, bijvoorbeeld het exemplaar van de virtuele machine "MyNodeType_5" verwijderen.
2. Voer `Get-ServiceFabricNode` om ervoor te zorgen dat het knooppunt is overgegaan op uitgeschakeld. Als dat niet het geval is, wacht totdat het knooppunt is uitgeschakeld. Dit duurt een paar uur voor elk knooppunt. Niet worden voortgezet totdat het knooppunt is overgegaan op uitgeschakeld.
3. Verminder het aantal virtuele machines met één in het desbetreffende type knooppunt. Het hoogste VM-exemplaar wordt nu verwijderd.
4. Herhaal stappen 1 t/m 3 indien nodig, maar niet verkleinen het aantal exemplaren in het primaire knooppunttypen die kleiner is dan wat de betrouwbaarheidslaag garandeert. Zie [Planning van de capaciteit van de Service Fabric-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) voor een lijst met aanbevolen exemplaren.

Als u wilt schalen handmatig, werk de capaciteit in de SKU-eigenschap van de gewenste [virtuele-Machineschaalset](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) resource.

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

1. Herhaal stappen 1 t/m 3 totdat u de capaciteit die u wilt inrichten. Omlaag het aantal exemplaren in de primaire knooppunttypen die lager is dan wat de betrouwbaarheidslaag garandeert niet schalen. Raadpleeg voor meer informatie over de lagen van de betrouwbaarheid en het aantal instanties dat ze vereist de [Planning van de capaciteit van de Service Fabric-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity).

U moet het knooppunt voorbereiden voor schalen in via een programma is afgesloten. Dit omvat het zoeken naar het knooppunt worden verwijderd die is de hoogste exemplaar-knooppunt en het deactiveren. Bijvoorbeeld:

```c#
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n =>
        {
            var instanceIdIndex = n.NodeName.LastIndexOf("_");
            var instanceIdString = n.NodeName.Substring(instanceIdIndex + 1);
            return int.Parse(instanceIdString);
        })
        .FirstOrDefault();
```

Wanneer u het knooppunt verwijderen hebt geïdentificeerd, deactiveren en verwijderen met behulp van dezelfde `FabricClient` exemplaar (`client` in dit geval) en knooppunt-exemplaarnaam (`instanceIdString` in dit geval) die u in de bovenstaande code wordt gebruikt:

```c#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shutdown
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement VMSS capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply();
```

## <a name="reliability-levels"></a>Betrouwbaarheid niveaus

De [betrouwbaarheidsniveau](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) is een eigenschap van de resource van uw Service Fabric-Cluster, en anders voor afzonderlijke nodeTypes kan niet worden geconfigureerd. Deze besturingselementen van de replicatiefactor van het system-services voor het cluster en is een instelling op het niveau van het cluster-bron. Het betrouwbaarheidsniveau dat bepaalt het minimum aantal knooppunten waaruit het primaire knooppunttype moet bestaan. De betrouwbaarheidslaag kan duren voordat de volgende waarden:
* De systeemservices Platinum - uitgevoerd met een telling doel replica set van zeven en negen seed-knooppunten.
* De systeemservices Gold - uitgevoerd met een telling doel replica set van zeven en zeven seed-knooppunten.
* Silver - de systeem-services worden uitgevoerd met een telling doel replica set van vijf en vijf seed-knooppunten.
* Bronze - de systeem-services worden uitgevoerd met een telling doel replica set van drie en drie seed-knooppunten.

De minimale aanbevolen betrouwbaarheidsniveau is lichtgrijs.

Het betrouwbaarheidsniveau dat is ingesteld in het eigenschappengedeelte van de [Microsoft.ServiceFabric/clusters resource](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters), zoals deze:

```json
"properties":{
    "reliabilityLevel": "Silver"
}
```

## <a name="durability-levels"></a>Duurzaamheid niveaus

> [!WARNING]
> Knooppunttypen met de duurzaamheid Brons verkrijgen _geen bevoegdheden_. Dit betekent dat infrastructuur-taken die invloed hebben op uw staatloze werkbelastingen worden niet worden gestopt of uitgesteld, kan dit gevolgen hebben voor uw workloads. Gebruik de duurzaamheid Brons alleen voor knooppunttypen dat staatloze werkbelastingen worden uitgevoerd. Voor productieworkloads uitvoeren Silver of hoger op de status van de consistentie. Kies de juiste betrouwbaarheid op basis van de instructies in de [capaciteitsplanning documentatie](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity).

Het duurzaamheidsniveau van de moet worden ingesteld in de twee resources. Het extensieprofiel van de [resource virtuele-Machineschaalset](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile):

```json
"extensionProfile": {
    "extensions":          {
        "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
        "properties": {
            "settings": {
                "durabilityLevel": "Bronze"
            }
        }
    }
}
```

En klikt u onder `nodeTypes` in de [Microsoft.ServiceFabric/clusters resource](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters) 

```json
"nodeTypes": [
    {
        "name": "[variables('vmNodeType0Name')]",
        "durabilityLevel": "Bronze"
    }
]
```

## <a name="next-steps"></a>Volgende stappen

* Een cluster maken op virtuele machines of op computers met Windows Server: [Service Fabric-cluster maken voor Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Een cluster maken op VM's of computers waarop Linux wordt uitgevoerd: [Een Linux-cluster maken](service-fabric-cluster-creation-via-portal.md)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png
