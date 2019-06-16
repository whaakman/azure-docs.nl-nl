---
title: Capaciteitsplanning en schaalbaarheid voor Azure Service Fabric | Microsoft Docs
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
ms.date: 04/25/2019
ms.author: pepogors
ms.openlocfilehash: 58c50eac60f1a8a47aac9a88125bc3e0132ec3db
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67059155"
---
# <a name="capacity-planning-and-scaling-for-azure-service-fabric"></a>Planning van capaciteit en schaalbaarheid van Azure Service Fabric

Voordat u een Azure Service Fabric-cluster maken of resources die als host fungeren van uw cluster de schaal van rekenkracht, is het belangrijk om te plannen voor capaciteit. Zie voor meer informatie over het plannen van capaciteit [Planning van de capaciteit van de Service Fabric-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity). Zie voor meer best practices richtlijnen voor schaalbaarheid, [overwegingen met betrekking tot Service Fabric-schaalbaarheid](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#scalability-considerations).

Naast het knooppunt type en de cluster-kenmerken overweegt, moet u verwachten dat vergroten / verkleinen duurt langer dan een uur om voor een productie-omgeving te voltooien. Deze overweging geldt ongeacht het aantal virtuele machines die u wilt toevoegen.

## <a name="autoscaling"></a>Automatisch schalen
U kunt vergroten / verkleinen via Azure Resource Manager-sjablonen, moet uitvoeren, omdat dit de aanbevolen procedure voor het behandelen [resourceconfiguraties als code]( https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code). 

Met automatisch schalen met schaalsets voor virtuele machines, wordt uw samengestelde Resource Manager-sjabloon koppelingsvelden definieert het aantal instanties voor virtuele-machineschaalsets maken. Onjuiste definitie verhoogt het risico dat toekomstige implementaties ervoor zorgen onbedoelde vergroten / verkleinen dat. In het algemeen moet u automatisch schalen als:

* Uw Resource Manager-sjablonen implementeren met de juiste capaciteit gedeclareerd biedt geen ondersteuning voor uw situatie.
     
   Naast het handmatig schalen, kunt u configureren een [continue integratie en levering pijplijn in Azure DevOps-Services met behulp van Azure resourcegroepimplementatieprojecten](https://docs.microsoft.com/azure/vs-azure-tools-resource-groups-ci-in-vsts). Deze pijplijn wordt vaak geactiveerd door een logische app die gebruikmaakt van maatstaven voor prestaties van virtuele machine opgevraagd uit de [Azure Monitor REST API](https://docs.microsoft.com/azure/azure-monitor/platform/rest-api-walkthrough). De pijplijn effectief automatisch wordt geschaald op basis van welke metrische gegevens u wilt, terwijl optimaliseren voor Resource Manager-sjablonen.
* U moet slechts één virtual machine scale set knooppunt horizontaal schalen op een tijdstip.
   
   Als u wilt schalen door drie of meer knooppunten tegelijk, moet u [scale-out van een Service Fabric-cluster door het toevoegen van een virtuele-machineschaalset](virtual-machine-scale-set-scale-node-type-scale-out.md). Is het verstandig om in te schalen en scale-out virtuele-machineschaalset stelt horizontaal, één knooppunt tegelijk.
* U hebt Silver betrouwbaarheid of hoger voor uw Service Fabric-cluster, en Silver duurzaamheid of hoger op elke schaal waarin u regels voor automatisch schalen configureren.
  
   De opgegeven Minimumcapaciteit voor regels voor automatisch schalen moet gelijk zijn aan of groter zijn dan vijf exemplaren van de virtuele machine. Het moet ook zijn gelijk aan of groter is dan de minimale Betrouwbaarheidslaag voor het primaire knooppunttype.

> [!NOTE]
> De Service Fabric stateful service fabric: / System/InfastructureService/< NODE_TYPE_NAME > wordt uitgevoerd op elk knooppunttype waarvoor Silver- of hogere duurzaamheid. Het is de enige service die wordt ondersteund om uit te voeren in Azure op een van de typen clusters.

## <a name="vertical-scaling-considerations"></a>Overwegingen voor verticaal vergroten/verkleinen

[Verticaal schalen](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out) een knooppunttype in Azure Service Fabric is een aantal stappen en overwegingen bij vereist. Bijvoorbeeld:

* Het cluster moet in orde zijn voor het schalen. Anders gaat u verder in het cluster instabiel.
* Silver duurzaamheid niveau of hoger is vereist voor alle Service Fabric-clusterknooppunttypen dat als host van stateful services.

> [!NOTE]
> Het primaire knooppunttype die als host fungeert voor stateful Service Fabric-systeemservices moet Silver duurzaamheid niveau of hoger. Nadat u de Silver duurzaamheid, clusterbewerkingen, zoals upgrades, ingeschakeld zijn toevoegen of verwijderen van knooppunten, enzovoort lager omdat het systeem is geoptimaliseerd voor de veiligheid van gegevens via de snelheid van bewerkingen.

Verticaal schalen van een virtuele-machineschaalset is een destructieve bewerking. Horizontaal schalen in plaats daarvan uw cluster door toe te voegen een nieuwe schaalset met de gewenste SKU. Vervolgens moet u uw services migreren naar uw gewenste SKU om een veilige verticaal vergroten/verkleinen bewerking te voltooien. Wijzigen van een virtuele machine scale set resource SKU is een destructieve bewerking omdat deze uw hosts werkrolgrootte, welke verwijdert alle lokaal status opgeslagen.

Uw cluster maakt gebruik van Service Fabric [knooppunteigenschappen en plaatsingsbeperkingen](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-cluster-description#node-properties-and-placement-constraints) om te bepalen waar u voor het hosten van services van uw toepassing. Wanneer u het primaire knooppunttype verticaal schaalt, declareren identieke eigenschapswaarden voor `"nodeTypeRef"`. U kunt deze waarden vinden in de Service Fabric-extensie voor virtuele-machineschaalsets. 

Het volgende fragment van een Resource Manager-sjabloon bevat de eigenschappen die u moet declareren. Deze heeft dezelfde waarde voor de nieuw ingerichte scale sets die u hebt schalen naar, en alleen als een tijdelijke stateful service voor uw cluster wordt ondersteund.

```json
"settings": {
   "nodeTypeRef": ["[parameters('primaryNodetypeName')]"]
}
```

> [!NOTE]
> Laat uw cluster uitgevoerd met meerdere schaalsets die gebruikmaken van dezelfde niet `nodeTypeRef` eigenschapswaarde langer zijn dan nodig is om een geslaagde verticaal vergroten/verkleinen bewerking te voltooien.
>
> Valideer altijd bewerkingen in een testomgeving voordat u wijzigingen in de productie-omgeving. Standaard hebben systeemservices van Service Fabric-cluster een beperking plaatsing naar alleen het doeltype primaire knooppunt.

Met de eigenschappen van het knooppunt en plaatsingsbeperkingen gedeclareerd, door de volgende stappen uit één VM-exemplaar op een tijdstip te doen. Hiermee worden de systeemservices (en de stateful services) zonder problemen worden afgesloten op de VM-exemplaar dat u verwijderen wilt nadat er nieuwe replica's ergens anders zijn gemaakt.

1. Voer vanuit PowerShell, `Disable-ServiceFabricNode` met opzet `RemoveNode` om uit te schakelen van het knooppunt dat u wilt verwijderen. Verwijder het knooppunttype met het hoogste getal. Als u een cluster met zes knooppunten hebt, bijvoorbeeld het exemplaar van de virtuele machine "MyNodeType_5" verwijderen.
2. Voer `Get-ServiceFabricNode` om ervoor te zorgen dat het knooppunt is overgegaan op uitgeschakeld. Als dat niet het geval is, wacht totdat het knooppunt is uitgeschakeld. Dit kan een paar uur voor elk knooppunt duren. Niet worden voortgezet totdat het knooppunt is overgegaan op uitgeschakeld.
3. Verminder het aantal virtuele machines met één in het desbetreffende knooppunttype. Het hoogste VM-exemplaar wordt nu verwijderd.
4. Herhaal stappen 1 t/m 3 indien nodig, maar niet verkleinen het aantal exemplaren in het primaire knooppunttypen die kleiner is dan wat de betrouwbaarheidslaag garandeert. Zie [Planning van de capaciteit van de Service Fabric-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) voor een lijst met aanbevolen exemplaren.

### <a name="example-scenario"></a>Voorbeeldscenario
Een ondersteund scenario voor het uitvoeren van een bewerking voor verticaal vergroten/verkleinen is: u wilt migreren van uw Service Fabric-cluster en de toepassing van een niet-beheerde schijf naar managed disks zonder downtime van toepassingen. 

U kunt inrichten van een nieuwe virtuele-machineschaalset met beheerde schijven en voer een toepassingsupgrade met plaatsingsbeperkingen die zijn gericht op ingerichte capaciteit. Uw Service Fabric-cluster kunt vervolgens uw workload in ingerichte cluster-knooppunt de opslagcapaciteit die wordt geïmplementeerd door upgradedomein zonder downtime van toepassingen plannen. 

Back-end-pool-eindpunten voor de [Azure Load Balancer basis-SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) virtuele machines in één beschikbaarheidsset of een virtuele-machineschaalset kan zijn. Dit betekent dat u niet een basis-SKU load balancer gebruiken als u uw Service Fabric-toepassing voor systemen tussen schaalsets verplaatsen, zonder dat de tijdelijke ontoegankelijkheid van het beheereindpunt voor Service Fabric-cluster. Dit geldt zelfs als het cluster en de toepassing nog steeds worden uitgevoerd.

Gebruikers inrichten vaak een standaard-SKU load balancer bij het uitvoeren van een virtuele IP-adres (VIP) wisselen tussen Basic SKU load balancer en standaard-SKU load balancer-resources. Deze techniek beperkt eventuele toekomstige ontoegankelijkheid tot ongeveer 30 seconden voor wisselen van VIP is vereist.

## <a name="horizontal-scaling"></a>Horizontaal schalen

U kunt doen horizontaal schalen een [handmatig](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down) of [programmatisch](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-programmatic-scaling).

> [!NOTE]
> Als u een knooppunttype met Silver- of Gold duurzaamheid schaalt, wordt schalen traag zijn.

### <a name="scaling-out"></a>Uitschalen

Een Service Fabric-cluster uitbreiden door het verhogen van het aantal exemplaren voor een bepaalde virtuele-machineschaalset. U kunt opwaarts schalen via een programma met behulp van `AzureClient` en de ID voor de gewenste schaal instellen om de capaciteit te vergroten.

```c#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

Als u wilt schalen handmatig, werk de capaciteit in de SKU-eigenschap van de gewenste [virtuele-machineschaalset](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) resource.

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
* Voor een stateful service moet u een bepaald aantal knooppunten die altijd van beschikbaarheid en de status van uw service behouden. Ten minste moet u een aantal knooppunten gelijk zijn aan het aantal doel replica instellen van de partitie of de service.

Als u wilt handmatig schalen de volgende stappen uit:

1. Voer vanuit PowerShell, `Disable-ServiceFabricNode` met opzet `RemoveNode` om uit te schakelen van het knooppunt dat u wilt verwijderen. Verwijder het knooppunttype met het hoogste getal. Als u een cluster met zes knooppunten hebt, bijvoorbeeld het exemplaar van de virtuele machine "MyNodeType_5" verwijderen.
2. Voer `Get-ServiceFabricNode` om ervoor te zorgen dat het knooppunt is overgegaan op uitgeschakeld. Als dat niet het geval is, wacht totdat het knooppunt is uitgeschakeld. Dit kan een paar uur voor elk knooppunt duren. Niet worden voortgezet totdat het knooppunt is overgegaan op uitgeschakeld.
3. Verminder het aantal virtuele machines met één in het desbetreffende knooppunttype. Het hoogste VM-exemplaar wordt nu verwijderd.
4. Herhaal stappen 1 t/m 3 zo nodig tot u de capaciteit die u wilt inrichten. Omlaag het aantal exemplaren in de primaire knooppunttypen die lager is dan wat de betrouwbaarheidslaag garandeert niet schalen. Zie [Planning van de capaciteit van de Service Fabric-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) voor een lijst met aanbevolen exemplaren.

Als u wilt schalen handmatig, werk de capaciteit in de SKU-eigenschap van de gewenste [virtuele-machineschaalset](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) resource.

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

U moet het knooppunt voor afsluiten om te schalen programmatisch voorbereiden. Zoek het knooppunt worden verwijderd (de hoogste-instance-knooppunt). Bijvoorbeeld:

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

Deactiveren en verwijderen van het knooppunt met behulp van dezelfde `FabricClient` exemplaar (`client` in dit geval) en knooppuntexemplaar (`instanceIdString` in dit geval) die u hebt gebruikt in de vorige code:

```c#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shut down
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement virtual machine scale set capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply();
```

> [!NOTE]
> Wanneer u een cluster verkleinen, ziet u het verwijderde knooppunt/VM-exemplaar in een slechte staat in Service Fabric Explorer weergegeven. Zie voor een uitleg van dit gedrag [gedrag die u in Service Fabric Explorer zien kunt](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down#behaviors-you-may-observe-in-service-fabric-explorer). U kunt:
> * Roep de [de opdracht Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) met de naam van het juiste knooppunt.
> * Implementeer de [Service Fabric-toepassing voor automatisch schalen helper](https://github.com/Azure/service-fabric-autoscale-helper/) in uw cluster. Deze toepassing zorgt ervoor dat de gereduceerde-knooppunten zijn uitgeschakeld vanuit Service Fabric Explorer.

## <a name="reliability-levels"></a>Betrouwbaarheid niveaus

De [betrouwbaarheidsniveau](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) is een eigenschap van de resource van uw Service Fabric-cluster. Deze kan niet anders worden geconfigureerd voor afzonderlijke typen. Deze besturingselementen van de replicatiefactor van het system-services voor het cluster en is een instelling op het niveau van het cluster-bron. 

Het betrouwbaarheidsniveau dat bepaalt het minimum aantal knooppunten waaruit het primaire knooppunttype moet bestaan. De betrouwbaarheidslaag kan duren voordat de volgende waarden:

* Platina: Het systeemservices worden uitgevoerd met een telling doel replica set van zeven en negen seed-knooppunten.
* Goud: Het systeemservices worden uitgevoerd met een telling doel replica set van zeven en zeven seed-knooppunten.
* Silver: Het systeemservices worden uitgevoerd met een telling doel replica set van vijf en vijf seed-knooppunten.
* Brons: Het systeemservices worden uitgevoerd met een telling doel replica set van drie en drie seed-knooppunten.

De minimale aanbevolen betrouwbaarheidsniveau is lichtgrijs.

Het betrouwbaarheidsniveau dat is ingesteld in het eigenschappengedeelte van de [Microsoft.ServiceFabric/clusters resource](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters), zoals deze:

```json
"properties":{
    "reliabilityLevel": "Silver"
}
```

## <a name="durability-levels"></a>Duurzaamheid niveaus

> [!WARNING]
> Knooppunttypen met de duurzaamheid Brons verkrijgen _geen bevoegdheden_. Infrastructuur-taken die invloed hebben op uw staatloze werkbelastingen worden niet gestopt of vertraagd, die mogelijk van invloed zijn op uw workloads. 
>
> Gebruik de duurzaamheid Brons alleen voor knooppunttypen dat staatloze werkbelastingen worden uitgevoerd. Voor productieworkloads, zilver of hoger om te zorgen voor consistentie van de status worden uitgevoerd. Kies de juiste betrouwbaarheid op basis van de instructies in de [capaciteitsplanning documentatie](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity).

Het duurzaamheidsniveau van de moet worden ingesteld in de twee resources. Een is het extensieprofiel van de [virtuele-machineschaalset resource](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile):

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

De andere resource ligt onder de `nodeTypes` in de [Microsoft.ServiceFabric/clusters resource](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters): 

```json
"nodeTypes": [
    {
        "name": "[variables('vmNodeType0Name')]",
        "durabilityLevel": "Bronze"
    }
]
```

## <a name="next-steps"></a>Volgende stappen

* Een cluster maken op virtuele machines of op computers met Windows Server: [Service Fabric-cluster maken voor Windows Server](service-fabric-cluster-creation-for-windows-server.md).
* Een cluster maken op VM's of computers waarop Linux wordt uitgevoerd: [Een Linux-cluster maken](service-fabric-cluster-creation-via-portal.md).
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md).

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png
