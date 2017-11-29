---
title: Een Service Fabric-cluster in- of schalen | Microsoft Docs
description: Een Service Fabric-cluster in- of vraag door het instellen van regels voor automatisch schalen voor elk knooppunt type/virtuele-machineschaalset overeen te schalen. Toevoegen of verwijderen van knooppunten aan een Service Fabric-cluster
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: aeb76f63-7303-4753-9c64-46146340b83d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2017
ms.author: chackdan
ms.openlocfilehash: 249fb4903c7b2de3ce290850a7759a4793f10aa7
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>Schalen van een Service Fabric-cluster in- of met regels voor automatisch schalen
Virtuele-machineschaalsets zijn een Azure compute resource die u gebruiken kunt om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunttype dat is gedefinieerd in een Service Fabric-cluster is ingesteld als een afzonderlijke virtuele-machineschaalset. Elk knooppunttype kan vervolgens worden uitgebreid of uit onafhankelijk, hebben verschillende sets van poorten openen en andere capaciteitsmetrieken kan hebben. Lees meer over in de [Service Fabric nodetypes](service-fabric-cluster-nodetypes.md) document. Omdat de typen van de Service Fabric-knooppunt in het cluster zijn gemaakt van de virtuele-machineschaalsets op de back-end, moet u regels voor elk knooppunt type/virtuele-machineschaalset automatisch schalen instellen.

> [!NOTE]
> Uw abonnement moet voldoende kernen om toe te voegen van de nieuwe virtuele machines die gezamenlijk dit cluster hebben. Er is geen modelvalidatie, er dus een tijd implementatie mislukt als een van de quotalimieten zijn bereikt.
> 
> 

## <a name="choose-the-node-typevirtual-machine-scale-set-to-scale"></a>Kies het type knooppunt/virtuele-machineschaalset ingesteld om te schalen
Op dit moment kunt zich u niet automatisch schalen regels opgeven voor virtuele-machineschaalsets met de portal, gebruikt u dus laat het ons Azure PowerShell (1.0 +) om een lijst van de knooppunttypen en vervolgens automatisch schalen regels toe te voegen aan deze.

Als u de lijst met virtuele-machineschaalset die gezamenlijk uw cluster, voert u de volgende cmdlets:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <Virtual Machine scale set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevirtual-machine-scale-set"></a>Regels voor het type knooppunt/virtuele-machineschaalset automatisch schalen instellen
Als uw cluster typen met meerdere knooppunten heeft, Herhaal dat dit voor elk knooppunt typen/virtuele-machineschaalset ingesteld dat u schalen wilt (inkomend of uitgaand). Rekening gehouden met het aantal knooppunten die u hebt nodig voordat u een automatische schaling instellen. Het minimum aantal knooppunten die u voor het primaire knooppunttype hebt nodig wordt aangedreven door het niveau van betrouwbaarheid dat u hebt gekozen. Lees meer over [betrouwbaarheid niveaus](service-fabric-cluster-capacity.md).

> [!NOTE]
> Het primaire knooppunt bij het verkleinen tot minder dan het minimale aantal Controleer het cluster onstabiel of typ brengt u het naar beneden. Dit kan leiden tot verlies van gegevens voor uw toepassingen en de systeemservices.
> 
> 

De functie voor automatisch schalen wordt momenteel niet aangedreven door de belasting die uw toepassingen kunnen worden rapporteren naar Service Fabric. Op dit moment de dat u automatisch geschaald puur wordt aangedreven door de prestatiemeteritems die worden gegenereerd door elk van de virtuele Machine ingesteld scale dus exemplaren.  

Volg deze instructies [voor het instellen van automatische-schaling voor elke virtuele-machineschaalset](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

> [!NOTE]
> In een schaal omlaag scenario, tenzij uw knooppunttype duurzaamheid niveau van de goud, Zilver heeft moet u aan te roepen de [cmdlet Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) met de naam van het juiste knooppunt.
> 
> 

## <a name="manually-add-vms-to-a-node-typevirtual-machine-scale-set"></a>Virtuele machines handmatig toevoegen aan een knooppunt type/virtuele-machineschaalset
Volg de sample/instructies in de [sjablonengalerie snel starten](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) het aantal virtuele machines in elke Nodetype te wijzigen. 

> [!NOTE]
> Toevoegen van virtuele machines kost tijd, zodat de toevoegingen aan onmiddellijk worden niet van plan bent. Plan daarom capaciteit goed in tijd, zodat het meer dan 10 minuten voordat het VM-capaciteit beschikbaar voor de replica's is toevoegen / service-exemplaren te worden geplaatst.
> 
> 

## <a name="manually-remove-vms-from-the-primary-node-typevirtual-machine-scale-set"></a>Virtuele machines handmatig van het primaire knooppunt type/virtuele-machineschaalset verwijderen
> [!NOTE]
> De service fabric system-services worden uitgevoerd in het primaire knooppunttype in uw cluster. Daarom moet nooit afgesloten of het aantal exemplaren in dat knooppunt types terugschroeven die kleiner zijn dan de betrouwbaarheidslaag garandeert. Raadpleeg [de details op betrouwbaarheid lagen hier](service-fabric-cluster-capacity.md). 
> 
> 

U moet de volgende stappen uit één VM-instantie tegelijk uitvoeren. Hierdoor kunnen de systeemservices (en uw stateful services) worden afsluiten op de VM-instantie die u wilt verwijderen en de nieuwe replica's die zijn gemaakt op andere knooppunten.

1. Voer [uitschakelen ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) met opzet 'RemoveNode' naar het knooppunt uitschakelen u wilt verwijderen (hoogste exemplaar in het desbetreffende knooppunttype).
2. Voer [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) om ervoor te zorgen dat het knooppunt inderdaad is overgegaan op uitgeschakeld. Als dat niet het geval is, wacht totdat het knooppunt is uitgeschakeld. U kunt geen wacht niet langer in deze stap.
3. Volg de sample/instructies in de [sjablonengalerie snel starten](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) wijzigen van het aantal virtuele machines met één die Nodetype. Het exemplaar dat is verwijderd is de hoogste VM-instantie. 
4. Herhaal stap 1 tot en met 3 indien nodig, maar nooit terugschroeven het aantal exemplaren in het primaire knooppunttypen die kleiner is dan wat de betrouwbaarheidslaag garandeert. Raadpleeg [de details op betrouwbaarheid lagen hier](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevirtual-machine-scale-set"></a>Virtuele machines handmatig uit de niet-primaire knooppunt type/virtuele-machineschaalset verwijderen
> [!NOTE]
> Voor een stateful service moet u een bepaald aantal knooppunten voor beschikbaarheid altijd maximaal worden onderhouden en het behouden van de status van uw service. Aan de zeer minimum moet u het aantal knooppunten gelijk zijn aan het aantal doel replica set van de partitie-/ service. 
> 
> 

In dat geval moet u het uitvoeren van de volgende stappen uit één VM-instantie in op een tijdstip. Hiermee kunt u de systeemservices (en uw stateful services) worden afsluiten op de VM-instantie die u wilt verwijderen en nieuwe replica's gemaakt waar u anders.

1. Voer [uitschakelen ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) met opzet 'RemoveNode' naar het knooppunt uitschakelen u wilt verwijderen (hoogste exemplaar in het desbetreffende knooppunttype).
2. Voer [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) om ervoor te zorgen dat het knooppunt inderdaad is overgegaan op uitgeschakeld. Als dat niet, wacht u tot de knooppunten is uitgeschakeld. U kunt geen wacht niet langer in deze stap.
3. Volg de sample/instructies in de [sjablonengalerie snel starten](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) wijzigen van het aantal virtuele machines met één die Nodetype. Hiermee wordt de hoogste VM-instantie nu verwijderd. 
4. Herhaal stap 1 tot en met 3 indien nodig, maar nooit terugschroeven het aantal exemplaren in het primaire knooppunttypen die kleiner is dan wat de betrouwbaarheidslaag garandeert. Raadpleeg [de details op betrouwbaarheid lagen hier](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Gedrag kunnen zich in Service Fabric Explorer
Wanneer u een cluster opschalen heeft de Service Fabric Explorer geeft het aantal knooppunten (virtuele-machineschaalset exemplaren) die deel van het cluster uitmaken.  Wanneer u schaalt een cluster omlaag u wordt echter wel het verwijderde knooppunt/VM-exemplaar in een slechte status weergegeven, tenzij u aanroepen [verwijderen ServiceFabricNodeState cmd](https://msdn.microsoft.com/library/mt125993.aspx) met de naam van het juiste knooppunt.   

Hier is de uitleg van dit probleem.

De knooppunten die worden vermeld in Service Fabric Explorer zijn afspiegeling van welke het Service Fabric-systeemservices (FM specifiek) op de hoogte van het aantal knooppunten van het cluster had/heeft. Wanneer u de virtuele-machineschaalset vastgestelde schaalt, de virtuele machine is verwijderd maar FM system-service nog steeds denkt dat het knooppunt (dat is toegewezen aan de virtuele machine die is verwijderd) wordt terugkeren. Zo blijft de Service Fabric Explorer om weer te geven dat knooppunt (Hoewel de status is mogelijk fout of een onbekend).

Om ervoor te zorgen dat een knooppunt worden verwijderd wanneer een virtuele machine wordt verwijderd, hebt u twee opties:

1) Kies een duurzaamheid van goud of zilver voor de knooppunttypen in het cluster, waarmee u de infrastructuur-integratie. Dit wordt vervolgens automatisch verwijderd de knooppunten van de status van onze services (FM) wanneer u omlaag schalen.
Raadpleeg [de details op duurzaamheid niveaus hier](service-fabric-cluster-capacity.md)

2) Zodra de VM-instantie is verkleind, moet u de [cmdlet Remove-ServiceFabricNodeState](https://msdn.microsoft.com/library/mt125993.aspx).

> [!NOTE]
> Service Fabric-clusters vereisen een bepaald aantal knooppunten tegelijkertijd de tijd om te onderhouden, beschikbaarheid en het behouden van status - aangeduid als 'onderhoud quorum'. Het wordt dus doorgaans onveilige alle machines in het cluster afsluiten, tenzij u eerst hebt uitgevoerd een [volledige back-up van de staat](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Volgende stappen
Lees het volgende wanneer u ook meer informatie over het plannen van capaciteit van de cluster, upgraden van een cluster en partitioneren services:

* [De capaciteit van de cluster plannen](service-fabric-cluster-capacity.md)
* [Cluster-upgrades](service-fabric-cluster-upgrade.md)
* [Stateful services van de partitie voor maximale schaal](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
