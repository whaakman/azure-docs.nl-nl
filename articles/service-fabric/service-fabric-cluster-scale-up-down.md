---
title: Een Service Fabric-cluster in of uitschalen | Microsoft Docs
description: Een Service Fabric-cluster in- of uitgeschaald schalen zodat deze overeenkomen met de aanvraag door in te stellen de regels voor automatisch schalen voor elk type/virtuele-knooppunt-machineschaalset. Toevoegen of verwijderen van knooppunten in een Service Fabric-cluster
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: aeb76f63-7303-4753-9c64-46146340b83d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/12/2019
ms.author: aljo
ms.openlocfilehash: 400e4653800d445506d4854e70034a707dcc4629
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66161807"
---
# <a name="scale-a-cluster-in-or-out"></a>Een cluster in- of uitschalen

> [!WARNING]
> Lees deze sectie voordat u de schaal

Schaal rekenresources zdroji de werkbelasting van uw toepassing vereist opzettelijke planning, bijna altijd duurt langer dan een uur om uit te voeren voor een productie-omgeving en vereist dat u meer informatie over uw werkbelasting bedrijfscontext; zelfs als u deze activiteit voordat u nooit eerder hebt gedaan, het raadzaam u starten door te lezen en te begrijpen [Service Fabric-cluster overwegingen voor capaciteitsplanning](service-fabric-cluster-capacity.md), voordat u doorgaat met de rest van dit document. Deze aanbeveling is om te voorkomen dat onbedoelde LiveSite problemen en u kunt het beste testen is de bewerkingen die u besluit om uit te voeren op basis van een niet-productieomgeving. U kunt op elk gewenst moment [productieproblemen melden of betaalde ondersteuning aanvragen voor Azure](service-fabric-support.md#report-production-issues-or-request-paid-support-for-azure). Voor technici toegewezen voor het uitvoeren van deze bewerkingen die beschikken over de juiste context, moet vergroten / verkleinen in dit artikel wordt beschreven, maar u bepalen en weten welke bewerkingen zijn geschikt is voor uw situatie; zoals welke resources op schaal (CPU, opslag, geheugen), welke richting om te schalen (horizontaal of verticaal), en welke bewerkingen moeten worden uitgevoerd (Resource-sjabloon implementeren, Portal, PowerShell/CLI).


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules-or-manually"></a>Een Service Fabric-cluster in of uitschalen met behulp van regels voor automatisch schalen of handmatig
Virtuele-machineschaalsets vormen een Azure compute-resource die u gebruiken kunt om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunttype die is gedefinieerd in een Service Fabric-cluster is ingesteld als een afzonderlijke virtuele-machineschaalset. Elk knooppunttype kan vervolgens worden geschaald of uit onafhankelijk van elkaar, hebben verschillende sets open poorten en verschillende capaciteitsstatistieken hebben. Meer informatie over het in de [knooppunttypen van Service Fabric](service-fabric-cluster-nodetypes.md) document. Aangezien de typen van de Service Fabric-knooppunt in het cluster worden gemaakt van de virtuele-machineschaalsets met de back-end, moet u voor het instellen van regels voor automatisch schalen voor elk type/virtuele-knooppunt-machineschaalset.

> [!NOTE]
> Uw abonnement moet hebben onvoldoende cores om toe te voegen van de nieuwe virtuele machines die gezamenlijk dit cluster. Er momenteel geen validatie, zodat u een implementatiefout tijd als een van de quotalimieten zijn bereikt. Één knooppunttype kunt 100 knooppunten per VMSS ook gewoon niet overschrijden. U moet mogelijk om toe te voegen automatisch schalen, kunnen niet en VMSS van de betreffende schaal bereiken automagically van VMSS toevoegen. De VMSS in-place toe te voegen aan een live cluster is een lastige taak en vaak resulteert dit in nieuwe clusters inrichten met de juiste knooppunttypen ingericht tijdens de aanmaak; gebruikers [clustercapaciteit plannen](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) dienovereenkomstig. 
> 
> 

## <a name="choose-the-node-typevirtual-machine-scale-set-to-scale"></a>Kies het type knooppunt/virtuele-machineschaalset in Windows om te schalen
U bent momenteel niet kunnen de regels voor automatisch schalen voor virtuele-machineschaalsets met behulp van de portal wilt maken van een Service Fabric-Cluster, gebruikt u dus laat het ons Azure PowerShell (1.0 +) aan de lijst van de typen en vervolgens regels voor automatisch schalen toevoegen aan deze op te geven.

Als u de lijst met virtuele-machineschaalset waaruit het cluster, voert u de volgende cmdlets:

```powershell
Get-AzResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzVmss -ResourceGroupName <RGname> -VMScaleSetName <virtual machine scale set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevirtual-machine-scale-set"></a>Regels voor automatisch schalen voor de schaalset knooppunt type/virtuele machine instellen
Als uw cluster meerdere typen heeft, Herhaal dat dit voor elk knooppunt typen/virtuele-machineschaalset ingesteld dat u schalen wilt (inkomend of uitgaand). Bedenk eerst hoeveel knooppunten u moet hebben, voordat u automatisch schalen instelt. Het minimum aantal knooppunten dat u nodig hebt voor het primaire knooppunttype, wordt bepaald door het betrouwbaarheidsniveau dat u hebt gekozen. Meer informatie over [betrouwbaarheid niveaus](service-fabric-cluster-capacity.md).

> [!NOTE]
> Verkleint het primaire knooppunt, typt u het cluster stabiel die lager is dan het minimale aantal maken of het uitvallen. Dit kan leiden tot verlies van gegevens voor uw toepassingen en de systeemservices.
> 
> 

De functie voor automatisch schalen is momenteel niet aangestuurd door de belastingen die uw toepassingen aan Service Fabric kunnen rapporteren. Schaal op dit moment de krijgt u automatisch schalen is puur aangestuurd door de prestatiemeteritems die worden gegenereerd door elk van de virtuele machine zo ingesteld exemplaren.  

Volg deze instructies [voor het instellen van automatisch schalen voor elke virtuele-machineschaalset](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

> [!NOTE]
> In een schaalset omlaag scenario, tenzij het knooppunttype heeft een [duurzaamheidsniveau] [ durability] Gold of Silver moet u contact opnemen de [cmdlet Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) met de de naam van de juiste knooppunt. Voor de duurzaamheid brons, is het niet raadzaam om omlaag te schalen meer dan één knooppunt tegelijk.
> 
> 

## <a name="manually-add-vms-to-a-node-typevirtual-machine-scale-set"></a>VM's handmatig toevoegen aan een schaalset knooppunt type/virtuele machines

Wanneer u uitschaalt, voegt u meer instanties van de virtuele machine toe aan de schaalset. Deze instanties worden de knooppunten die door Service Fabric worden gebruikt. Service Fabric weet wanneer er meer exemplaren aan de schaalset zijn toegevoegd (door uitschaling) en reageert daar automatisch op. 

> [!NOTE]
> Toevoegen van virtuele machines kost tijd, zodat de toevoegingen aan onmiddellijk worden niet verwacht. Dus plan capaciteit om toe te staan voor meer dan tien minuten voordat de VM-capaciteit beschikbaar is voor de replica's / service-exemplaren worden geplaatst op productieniveau, toe te voegen.
> 

### <a name="add-vms-using-a-template"></a>VM's met behulp van een sjabloon toevoegen
Ga als volgt het voorbeeld/instructies in de [sjabloon snelstartgalerie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) te wijzigen van het aantal virtuele machines in elk knooppunttype. 

### <a name="add-vms-using-powershell-or-cli-commands"></a>VM's met behulp van PowerShell of CLI-opdrachten toevoegen
Met de volgende code verkrijgt u een schaalset op naam en verhoogt u de **capaciteit** van de schaalset met 1.

```powershell
$scaleset = Get-AzVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity += 1

Update-AzVmss -ResourceGroupName $scaleset.ResourceGroupName -VMScaleSetName $scaleset.Name -VirtualMachineScaleSet $scaleset
```

Met deze code stelt u de capaciteit in op 6.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 6
```

## <a name="manually-remove-vms-from-a-node-typevirtual-machine-scale-set"></a>Virtuele machines handmatig verwijderen uit een schaalset knooppunt type/virtuele machine
Wanneer u in een knooppunttype schalen, verwijdert u de VM-exemplaren uit de schaalset. Als het knooppunt niveau van de duurzaamheid Brons is, is Service Fabric is geen informatie over wat is er gebeurd en rapporteert dat een knooppunt is kwijtgeraakt. Service Fabric rapporteert daarom een onjuiste status voor het cluster. Om te voorkomen dat deze onjuiste status, moet u expliciet verwijderen van het knooppunt uit het cluster en de status verwijderen.

De service fabric-systeemservices worden uitgevoerd in het primaire knooppunttype in uw cluster. Als het primaire knooppunttype verkleint, nooit verkleinen het aantal exemplaren die lager is dan wat de [betrouwbaarheidslaag](service-fabric-cluster-capacity.md) garandeert. 
 
Voor een stateful service moet u een bepaald aantal knooppunten te worden altijd tot beschikbaarheid onderhouden en status van uw service behouden. Aan de zeer minimum moet u het aantal knooppunten gelijk zijn aan het aantal doel replica instellen van de partitie/service.

### <a name="remove-the-service-fabric-node"></a>Het Service Fabric-knooppunt verwijderen

De stappen voor het handmatig verwijderen van de status van knooppunt alleen van toepassing op knooppunttypen met een *Brons* duurzaamheidslaag.  Voor *Silver* en *Gold* duurzaamheidslaag, deze stappen automatisch worden uitgevoerd door het platform. Voor meer informatie over duurzaamheid raadpleegt u [Service Fabric cluster capacity planning][durability] (Capaciteitsplanning voor Service Fabric-clusters).

Als u de knooppunten van het cluster gelijkmatig verdeeld wilt houden in upgrade- en foutdomeinen en op die manier gelijkmatig gebruik wilt inschakelen, moet het meest recent gemaakte knooppunt eerst worden verwijderd. Met andere woorden - de knooppunten moeten op basis van 'last in, first out' worden verwijderd. Het meest recent gemaakte knooppunt heeft de hoogste `virtual machine scale set InstanceId`-eigenschapswaarde. Met de onderstaande codevoorbeelden wordt het meest recent gemaakte knooppunt geretourneerd.

```powershell
Get-ServiceFabricNode | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending | Select-Object -First 1
```

```azurecli
sfctl node list --query "sort_by(items[*], &name)[-1]"
```

Het Service Fabric-cluster moet weten dat dit knooppunt zal worden verwijderd. Er zijn drie stappen die u moet uitvoeren:

1. Schakel het knooppunt uit zodat het geen replica meer is voor gegevens.  
PowerShell: `Disable-ServiceFabricNode`  
sfctl: `sfctl node disable`

2. Stop het knooppunt zodat de Service Fabric-runtime netjes wordt afgesloten en uw app een beëindigingsaanvraag ontvangt.  
PowerShell: `Start-ServiceFabricNodeTransition -Stop`  
sfctl: `sfctl node transition --node-transition-type Stop`

2. Verwijder het knooppunt uit het cluster.  
PowerShell: `Remove-ServiceFabricNodeState`  
sfctl: `sfctl node remove-state`

Nadat deze drie stappen op het knooppunt zijn toegepast, kan het knooppunt worden verwijderd uit de schaalset. Als u naast de [bronzen][durability] duurzaamheidslaag nog een andere duurzaamheidslaag gebruikt, worden deze stappen voor u uitgevoerd wanneer de schaalsetinstantie wordt verwijderd.

In het volgende codeblok wordt het laatst gemaakte knooppunt opgehaald en wordt het knooppunt uitgeschakeld, gestopt en uit het cluster verwijderd.

```powershell
#### After you've connected.....
# Get the node that was created last
$node = Get-ServiceFabricNode | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending | Select-Object -First 1

# Node details for the disable/stop process
$nodename = $node.NodeName
$nodeid = $node.NodeInstanceId

$loopTimeout = 10

# Run disable logic
Disable-ServiceFabricNode -NodeName $nodename -Intent RemoveNode -TimeoutSec 300 -Force

$state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus

while (($state -ne [System.Fabric.Query.NodeStatus]::Disabled) -and ($loopTimeout -ne 0))
{
    Start-Sleep 5
    $loopTimeout -= 1
    $state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus
    Write-Host "Checking state... $state found"
}

# Exit if the node was unable to be disabled
if ($state -ne [System.Fabric.Query.NodeStatus]::Disabled)
{
    Write-Error "Disable failed with state $state"
}
else
{
    # Stop node
    $stopid = New-Guid
    Start-ServiceFabricNodeTransition -Stop -OperationId $stopid -NodeName $nodename -NodeInstanceId $nodeid -StopDurationInSeconds 300

    $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
    $loopTimeout = 10

    # Watch the transaction
    while (($state -eq [System.Fabric.TestCommandProgressState]::Running) -and ($loopTimeout -ne 0))
    {
        Start-Sleep 5
        $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
        Write-Host "Checking state... $state found"
    }

    if ($state -ne [System.Fabric.TestCommandProgressState]::Completed)
    {
        Write-Error "Stop transaction failed with $state"
    }
    else
    {
        # Remove the node from the cluster
        Remove-ServiceFabricNodeState -NodeName $nodename -TimeoutSec 300 -Force
    }
}
```

In onderstaande **sfctl**-code wordt de volgende opdracht gebruikt om de waarde van **knooppuntnaam** op te halen van het laatst gemaakte knooppunt: `sfctl node list --query "sort_by(items[*], &name)[-1].name"`

```azurecli
# Inform the node that it is going to be removed
sfctl node disable --node-name _nt1vm_5 --deactivation-intent 4 -t 300

# Stop the node using a random guid as our operation id
sfctl node transition --node-instance-id 131541348482680775 --node-name _nt1vm_5 --node-transition-type Stop --operation-id c17bb4c5-9f6c-4eef-950f-3d03e1fef6fc --stop-duration-in-seconds 14400 -t 300

# Remove the node from the cluster
sfctl node remove-state --node-name _nt1vm_5
```

> [!TIP]
> Gebruik de volgende **sfctl**-query's om de status van elke stap te controleren
>
> **Deactiveringsstatus controleren**
> `sfctl node list --query "sort_by(items[*], &name)[-1].nodeDeactivationInfo"`
>
> **Stopstatus controleren**
> `sfctl node list --query "sort_by(items[*], &name)[-1].isStopped"`
>

### <a name="scale-in-the-scale-set"></a>De schaalset inschalen

Nu het Service Fabric-knooppunt is verwijderd uit het cluster, kan de virtuele-machineschaalset worden ingeschaald. In onderstaand voorbeeld wordt de capaciteit van de schaalset verminderd met 1.

```powershell
$scaleset = Get-AzVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity -= 1

Update-AzVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Met deze code stelt u de capaciteit in op 5.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 5
```

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Gedrag dat u kunt zien in Service Fabric Explorer
Wanneer u een cluster omhoog schalen wordt het aantal knooppunten (virtuele-machineschaalset exemplaren) die deel van het cluster uitmaken weergegeven in de Service Fabric Explorer.  Echter, wanneer u de schaal van een cluster omlaag u ziet het verwijderde knooppunt/VM-exemplaar in een slechte status weergegeven, tenzij u aanroepen [Remove-ServiceFabricNodeState cmd](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) met de naam van het juiste knooppunt.   

Hier is de uitleg van dit gedrag.

De knooppunten die worden vermeld in Service Fabric Explorer zijn een weerspiegeling van wat de Service Fabric-systeemservices (FM specifiek) op de hoogte van het aantal knooppunten het cluster heeft/heeft. Wanneer u de virtuele-machineschaalset omlaag schaalt, de virtuele machine is verwijderd maar FM systeemservice nog steeds denkt dat het knooppunt (die is toegewezen aan de virtuele machine die is verwijderd) wordt terugkeren. Service Fabric Explorer blijft dus om weer te geven dat knooppunt (hoewel mogelijk is de status van de fout of onbekend).

Om ervoor te zorgen dat een knooppunt wordt verwijderd wanneer een virtuele machine wordt verwijderd, hebt u twee opties:

1. Kies duurzaamheidsniveau Gold of Silver voor de knooppunttypen in uw cluster, waardoor u de infrastructuurintegratie. Die vervolgens wordt automatisch verwijderd de knooppunten van de systeemstatus van de services (FM) wanneer u omlaag schalen.
Raadpleeg [de informatie over duurzaamheid niveaus hier](service-fabric-cluster-capacity.md)

2. Nadat het VM-exemplaar is verkleind, moet u aan te roepen de [cmdlet Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate).

> [!NOTE]
> Service Fabric-clusters vereist een bepaald aantal knooppunten zodat u op het moment om de beschikbaarheid en het behouden van status - aangeduid als "onderhouden quorum." Het is dus doorgaans onveilige alle machines in het cluster wordt afgesloten, tenzij u eerst hebt uitgevoerd een [volledige back-up van uw staat](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Volgende stappen
Lees de volgende ook meer informatie over planning clustercapaciteit, een cluster upgraden en services partitioneren:

* [De clustercapaciteit van uw plannen](service-fabric-cluster-capacity.md)
* [Upgraden van clusters](service-fabric-cluster-upgrade.md)
* [Stateful services van de partitie voor maximale schaal](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
