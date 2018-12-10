---
title: Een Service Fabric-cluster in of uitschalen | Microsoft Docs
description: Een Service Fabric-cluster in- of uitgeschaald schalen zodat deze overeenkomen met de aanvraag door in te stellen de regels voor automatisch schalen voor elk knooppunt type/virtuele-machineschaalset. Toevoegen of verwijderen van knooppunten in een Service Fabric-cluster
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: aeb76f63-7303-4753-9c64-46146340b83d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2017
ms.author: aljo
ms.openlocfilehash: 85a1e874ad80d0a3251c93c9c1199f56ab045527
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53140553"
---
# <a name="read-before-you-scale"></a>Lees voordat u de schaal
Schaal rekenresources zdroji de werkbelasting van uw toepassing vereist opzettelijke planning, bijna altijd duurt langer dan een uur om uit te voeren voor een productie-omgeving en vereist dat u meer informatie over uw werkbelasting bedrijfscontext; zelfs als u deze activiteit voordat u nooit eerder hebt gedaan, het raadzaam u starten door te lezen en te begrijpen [Service Fabric-cluster overwegingen voor capaciteitsplanning](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity), voordat u doorgaat met de rest van dit document. Deze aanbeveling is om te voorkomen dat onbedoelde LiveSite problemen en u kunt het beste testen is de bewerkingen die u besluit om uit te voeren op basis van een niet-productieomgeving. U kunt op elk gewenst moment [productieproblemen melden of betaalde ondersteuning aanvragen voor Azure](https://docs.microsoft.com/azure/service-fabric/service-fabric-support#report-production-issues-or-request-paid-support-for-azure). Voor technici toegewezen voor het uitvoeren van deze bewerkingen die beschikken over de juiste context, moet vergroten / verkleinen in dit artikel wordt beschreven, maar u bepalen en weten welke bewerkingen zijn geschikt is voor uw situatie; zoals welke resources op schaal (CPU, opslag, geheugen), welke richting om te schalen (horizontaal of verticaal), en welke bewerkingen moeten worden uitgevoerd (Resource-sjabloon implementeren, Portal, PowerShell/CLI).

# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules-or-manually"></a>Een Service Fabric-cluster in of uitschalen met behulp van regels voor automatisch schalen of handmatig
Virtuele-machineschaalsets vormen een Azure compute-resource die u gebruiken kunt om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunttype die is gedefinieerd in een Service Fabric-cluster is ingesteld als een afzonderlijke virtuele-machineschaalset. Elk knooppunttype kan vervolgens worden geschaald of uit onafhankelijk van elkaar, hebben verschillende sets open poorten en verschillende capaciteitsstatistieken hebben. Meer informatie over het in de [Service Fabric nodetypes](service-fabric-cluster-nodetypes.md) document. Aangezien de typen van de Service Fabric-knooppunt in het cluster worden gemaakt van de virtuele-machineschaalsets met de back-end, moet u voor het instellen van regels voor automatisch schalen voor elk knooppunt type/virtuele-machineschaalset.

> [!NOTE]
> Uw abonnement moet hebben onvoldoende cores om toe te voegen van de nieuwe virtuele machines die gezamenlijk dit cluster. Er momenteel geen validatie, zodat u een implementatiefout tijd als een van de quotalimieten zijn bereikt. Één knooppunttype kunt 100 knooppunten per VMSS ook gewoon niet overschrijden. U moet mogelijk om toe te voegen automatisch schalen, kunnen niet en VMSS van de betreffende schaal bereiken automagically van VMSS toevoegen. De VMSS in-place toe te voegen aan een live cluster is een lastige taak en vaak resulteert dit in nieuwe clusters inrichten met de juiste knooppunttypen ingericht tijdens de aanmaak; gebruikers [clustercapaciteit plannen](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) dienovereenkomstig. 
> 
> 

## <a name="choose-the-node-typevirtual-machine-scale-set-to-scale"></a>Kies het type knooppunt/virtuele-machineschaalset in Windows om te schalen
U bent momenteel niet kunnen de regels voor automatisch schalen voor virtuele-machineschaalsets met behulp van de portal wilt maken van een Service Fabric-Cluster, gebruikt u dus laat het ons Azure PowerShell (1.0 +) aan de lijst van de typen en vervolgens regels voor automatisch schalen toevoegen aan deze op te geven.

Als u de lijst met virtuele-machineschaalset waaruit het cluster, voert u de volgende cmdlets:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <Virtual Machine scale set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevirtual-machine-scale-set"></a>Regels voor het type knooppunt/virtuele-machineschaalset automatisch schalen instellen
Als uw cluster meerdere typen heeft, Herhaal dat dit voor elk knooppunt typen/virtuele-machineschaalset ingesteld dat u schalen wilt (inkomend of uitgaand). Bedenk eerst hoeveel knooppunten u moet hebben, voordat u automatisch schalen instelt. Het minimum aantal knooppunten dat u nodig hebt voor het primaire knooppunttype, wordt bepaald door het betrouwbaarheidsniveau dat u hebt gekozen. Meer informatie over [betrouwbaarheid niveaus](service-fabric-cluster-capacity.md).

> [!NOTE]
> Verkleint het primaire knooppunt, typt u het cluster stabiel die lager is dan het minimale aantal maken of het uitvallen. Dit kan leiden tot verlies van gegevens voor uw toepassingen en de systeemservices.
> 
> 

De functie voor automatisch schalen is momenteel niet aangestuurd door de belastingen die uw toepassingen aan Service Fabric kunnen rapporteren. Schaal op dit moment de krijgt u automatisch schalen is puur aangestuurd door de prestatiemeteritems die worden gegenereerd door elk van de virtuele machine zo ingesteld exemplaren.  

Volg deze instructies [voor het instellen van automatisch schalen voor elke virtuele-machineschaalset](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

> [!NOTE]
> In een schaalset omlaag scenario, tenzij het knooppunttype een duurzaamheidsniveau van Gold of Silver is moet u contact opnemen de [cmdlet Remove-ServiceFabricNodeState](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) met de naam van het juiste knooppunt. Voor de duurzaamheid brons, is het niet raadzaam om omlaag te schalen meer dan één knooppunt tegelijk.
> 
> 

## <a name="manually-add-vms-to-a-node-typevirtual-machine-scale-set"></a>VM's handmatig toevoegen aan een knooppunt type/virtuele-machineschaalset
Ga als volgt het voorbeeld/instructies in de [galerie snel aan de slag met sjablonen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) te wijzigen van het aantal virtuele machines in elke Nodetype. 

> [!NOTE]
> Toevoegen van virtuele machines kost tijd, zodat de toevoegingen aan onmiddellijk worden niet verwacht. Dus plan het toevoegen van capaciteit en in-time, waarmee meer dan 10 minuten voordat de VM-capaciteit beschikbaar voor de replica's is / service-exemplaren worden geplaatst.
> 
> 

## <a name="manually-remove-vms-from-the-primary-node-typevirtual-machine-scale-set"></a>Virtuele machines handmatig uit het primaire knooppunt type/virtuele-machineschaalset verwijderen
> [!NOTE]
> De service fabric-systeemservices worden uitgevoerd in het primaire knooppunttype in uw cluster. Zo moet nooit afgesloten of het aantal exemplaren in dat knooppunt types verkleinen die kleiner zijn dan wat de betrouwbaarheidslaag garandeert. Raadpleeg [de details op betrouwbaarheid lagen hier](service-fabric-cluster-capacity.md). 
> 
> 

U moet de volgende stappen uit één VM-exemplaar tegelijk uitvoeren. Hierdoor kunnen de systeemservices (en de stateful services) zonder problemen worden afgesloten op de VM-instantie die u wilt verwijderen en de nieuwe replica's die zijn gemaakt op andere knooppunten.

1. Voer [Disable-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) met opzet 'RemoveNode' naar het knooppunt uitschakelen u wilt verwijderen (de hoogste exemplaar in het desbetreffende knooppunttype).
2. Voer [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) om ervoor te zorgen dat het knooppunt inderdaad is overgegaan op uitgeschakeld. Als dat niet het geval is, wacht totdat het knooppunt is uitgeschakeld. U kunt geen wacht niet langer in deze stap.
3. Ga als volgt het voorbeeld/instructies in de [galerie snel aan de slag met sjablonen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) te wijzigen van het aantal virtuele machines met één in het desbetreffende type knooppunt. Het exemplaar verwijderd is het hoogste VM-exemplaar. 
4. Herhaal stappen 1 t/m 3 indien nodig, maar niet verkleinen het aantal exemplaren in het primaire knooppunttypen die kleiner is dan wat de betrouwbaarheidslaag garandeert. Raadpleeg [de details op betrouwbaarheid lagen hier](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevirtual-machine-scale-set"></a>Virtuele machines handmatig verwijderen van het type niet-primaire knooppunten/virtuele-machineschaalset
> [!NOTE]
> Voor een stateful service moet u een bepaald aantal knooppunten te worden altijd tot beschikbaarheid onderhouden en status van uw service behouden. Aan de zeer minimum moet u het aantal knooppunten gelijk zijn aan het aantal doel replica instellen van de partitie/service. 
> 
> 

U moet het uitvoeren van de volgende stappen uit één VM-exemplaar op een tijdstip. Hiermee kunt u de systeemservices (en de stateful services) zonder problemen worden afgesloten op de VM-exemplaar dat u wilt verwijderen en de nieuwe replica's gemaakt waar u anders.

1. Voer [Disable-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) met opzet 'RemoveNode' naar het knooppunt uitschakelen u wilt verwijderen (de hoogste exemplaar in het desbetreffende knooppunttype).
2. Voer [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) om ervoor te zorgen dat het knooppunt inderdaad is overgegaan op uitgeschakeld. Als dat niet het geval is, wacht totdat het knooppunt is uitgeschakeld. U kunt geen wacht niet langer in deze stap.
3. Ga als volgt het voorbeeld/instructies in de [galerie snel aan de slag met sjablonen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) te wijzigen van het aantal virtuele machines met één in het desbetreffende type knooppunt. Hiermee wordt het hoogste VM-exemplaar nu verwijderd. 
4. Herhaal stappen 1 t/m 3 indien nodig, maar niet verkleinen het aantal exemplaren in het primaire knooppunttypen die kleiner is dan wat de betrouwbaarheidslaag garandeert. Raadpleeg [de details op betrouwbaarheid lagen hier](service-fabric-cluster-capacity.md).

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
