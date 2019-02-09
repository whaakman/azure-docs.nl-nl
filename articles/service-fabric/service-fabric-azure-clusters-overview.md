---
title: Azure Service Fabric-clusters maken in Windows Server en Linux | Microsoft Docs
description: Service Fabric-clusters worden uitgevoerd op Windows Server en Linux, wat betekent dat u zult kunnen implementeren en host-Service Fabric-toepassingen overal kunt u Windows Server of Linux uitvoeren.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/01/2019
ms.author: dekapur
ms.openlocfilehash: a968b173357bf8bcb83990b891f38306895b4ca8
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967207"
---
# <a name="overview-of-service-fabric-clusters-on-azure"></a>Overzicht van Service Fabric-clusters op Azure
Een Service Fabric-cluster is een netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Een machine of virtuele machine die deel uitmaakt van een cluster wordt een clusterknooppunt genoemd. Clusters kunnen worden geschaald naar duizenden knooppunten. Als u nieuwe knooppunten aan het cluster toevoegt, rebalances Service Fabric de partitiereplica's service en de exemplaren in het grotere aantal knooppunten. Algemene verbetert de prestaties van toepassingen en conflicten voor toegang tot het geheugen vermindert. Als de knooppunten in het cluster niet efficiënt worden gebruikt, kunt u het aantal knooppunten in het cluster verminderen. Service Fabric rebalances opnieuw de partitiereplica's en exemplaren voor het kleinere aantal knooppunten om beter gebruik van de hardware op elk knooppunt.

Het knooppunttype bepaalt de grootte, het aantal en de eigenschappen voor een set knooppunten (virtuele machines) in het cluster. Elk knooppunttype kan dan onafhankelijk omhoog of omlaag worden geschaald, verschillende open poorten bevatten en diverse capaciteitsstatistieken hebben. Knooppunttypen worden gebruikt voor het definiëren van rollen voor een set clusterknooppunten, zoals 'front-end' of 'back-end'. Uw cluster kan meer dan één knooppunttype hebben, maar voor productieclusters moet het primaire knooppunttype ten minste vijf VM's bevatten (of ten minste drie VM's voor testclusters). [Service Fabric-systeemservices](service-fabric-technical-overview.md#system-services) worden op de knooppunten van het primaire knooppunttype geplaatst. 

## <a name="cluster-components-and-resources"></a>Clusteronderdelen en bronnen
Een Service Fabric-cluster op Azure is een Azure-resource die wordt gebruikt, en communiceert met andere Azure-resources:
* Virtuele machines en virtuele netwerkkaarten
* schaalsets voor virtuele machines
* virtuele netwerken
* Load balancers
* opslagaccounts
* Openbare IP-adressen

![Service Fabric-cluster][Image]

### <a name="virtual-machine"></a>Virtuele machine
Een [virtuele machine](/azure/virtual-machines/) dat deel uitmaakt van een cluster een knooppunt wordt genoemd, hoewel het technisch, een clusterknooppunt is een Service Fabric-runtime-proces. Aan elk knooppunt wordt een knooppuntnaam toegewezen (een tekenreeks). Knooppunten hebben kenmerken, zoals [plaatsingseigenschappen](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints). Elke computer of virtuele machine is een service automatisch wordt gestart, *FabricHost.exe*, die wordt uitgevoerd bij het opstarten en start vervolgens twee uitvoerbare bestanden, *Fabric.exe* en *FabricGateway.exe* , waardoor het knooppunt. Een productie-implementatie is een van de knooppunten per fysieke of virtuele machine. Voor het testen van scenario's, kunt u meerdere knooppunten op een enkele machine of virtuele machine host door het uitvoeren van meerdere exemplaren van *Fabric.exe* en *FabricGateway.exe*.

Elke virtuele machine is gekoppeld aan een virtuele netwerkinterfacekaart (NIC) en elke NIC een privé IP-adres is toegewezen.  Een virtuele machine is toegewezen aan een virtueel netwerk en lokale balancer via de NIC.

Alle virtuele machines in een cluster worden geplaatst in een virtueel netwerk.  Alle knooppunten in het hetzelfde knooppunt-type/schaalset worden geplaatst in hetzelfde subnet in het virtuele netwerk.  Deze knooppunten met particuliere IP-adressen alleen en zijn niet direct beschikbaar is buiten het virtuele netwerk.  Clients hebben toegang tot services op de knooppunten via de Azure load balancer.

### <a name="scale-setnode-type"></a>Schaaltype/knooppunt
Wanneer u een cluster maakt, definieert u een of meer typen.  De knooppunten of virtuele machines in een knooppunttype hebben dezelfde grootte en kenmerken, zoals het aantal CPU's, geheugen, het aantal schijven, en schijf-i/o.  Één knooppunttype kan bijvoorbeeld zijn voor kleine, front-end-VM's met poorten met het internet openen terwijl een ander knooppunttype kan worden voor grote, back-end VM's die gegevens verwerken. In Azure-clusters, elk knooppunttype is toegewezen aan een [virtuele-machineschaalset](/azure/virtual-machine-scale-sets/).

U kunt schaalsets gebruiken om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunttype die u in een Azure Service Fabric-cluster definieert, stelt u een afzonderlijke schaalset. De Service Fabric-runtime is gestart met een bootstrap op elke virtuele machine in de schaalset met behulp van Azure VM-extensies. U kunt onafhankelijk omhoog of omlaag schalen van elk knooppunttype, wijzigen van de SKU van het besturingssysteem die worden uitgevoerd op elk clusterknooppunt, verschillende open poorten bevatten en verschillende capaciteitsstatistieken gebruikt. Een schaalset is vijf [upgradedomeinen](service-fabric-cluster-resource-manager-cluster-description.md#upgrade-domains) en vijf [foutdomeinen](service-fabric-cluster-resource-manager-cluster-description.md#fault-domains) en kunt u maximaal 100 virtuele machines.  U maken clusters met meer dan 100 knooppunten met het maken van meerdere sets/knooppunttypen van schaal.

> [!IMPORTANT]
> Kiezen voor het aantal knooppunttypen voor uw cluster en de eigenschappen van elk knooppunttype (grootte, primair, verbonden met internet, aantal virtuele machines, enzovoort) is een belangrijke taak.  Lees voor meer informatie, [overwegingen voor capaciteitsplanning cluster](service-fabric-cluster-capacity.md).

Lees voor meer informatie, [knooppunttypen van Service Fabric en virtual machine-schaalsets](service-fabric-cluster-nodetypes.md).

### <a name="azure-load-balancer"></a>Azure Load Balancer
VM-exemplaren lid zijn van achter een [Azure load balancer](/azure/load-balancer/load-balancer-overview), die is gekoppeld aan een [openbaar IP-adres](/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses) en DNS-label.  Wanneer u een cluster met inrichten  *&lt;clustername&gt;*, de DNS-naam  *&lt;clustername&gt;.&lt; locatie&gt;. cloudapp.azure.com* is de DNS-label dat is gekoppeld aan de load balancer vóór de schaalset.

Virtuele machines in een cluster hebben alleen [privé IP-adressen](/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses).  Beheer van verkeer en serviceverkeer worden gerouteerd via de openbare load balancer.  Netwerkverkeer wordt doorgestuurd naar deze machines via NAT-regels (clients verbinding maken met specifieke knooppunten/exemplaren) of regels voor taakverdeling (verkeer wordt gerouteerd naar virtuele machines round robin).  Een load balancer heeft een bijbehorende openbare IP-adres met een DNS-naam in de indeling:  *&lt;clustername&gt;.&lt; locatie&gt;. cloudapp.azure.com*.  Een openbaar IP-adres is een andere Azure-resource in de resourcegroep.  Als u meerdere typen in een cluster definiëren, wordt een load balancer gemaakt voor elk knooppunt type/schaalset. Of u kunt instellen dat een enkele load balancer voor meerdere typen.  Het primaire knooppunttype is het DNS-label  *&lt;clustername&gt;.&lt; locatie&gt;. cloudapp.azure.com*, andere typen DNS-label hebben  *&lt;clustername&gt;-&lt;nodetype&gt;.&lt; locatie&gt;. cloudapp.azure.com*.

### <a name="storage-accounts"></a>Opslagaccounts
Elk clusterknooppunttype wordt ondersteund door een [Azure storage-account](/azure/storage/common/storage-introduction) en beheerde schijven.

## <a name="cluster-security"></a>Clusterbeveiliging
Een Service Fabric-cluster is een resource waarvan u eigenaar bent.  Het is uw verantwoordelijkheid voor het beveiligen van uw clusters om te voorkomen dat onbevoegde gebruikers verbinding maken met deze. Een beveiligd cluster is vooral belangrijk wanneer u bij het uitvoeren van productieworkloads op het cluster. 

### <a name="node-to-node-security"></a>Beveiliging van knooppunt-naar-knooppunt
Beveiliging van knooppunt-naar-knooppunt voor beveiliging van communicatie tussen de virtuele machines of computers in een cluster. In dit scenario voor beveiliging zorgt ervoor dat alleen computers die zijn gemachtigd om lid van het cluster te kunnen deelnemen die als host fungeert voor toepassingen en services in het cluster. Service Fabric maakt gebruik van X.509-certificaten voor het beveiligen van een cluster en beveiligingsfuncties van de toepassing opgeven.  Een clustercertificaat is vereist voor het beveiligen van verkeer het cluster en het cluster en de server-verificatie.  Zelf-ondertekend-certificaten kunnen worden gebruikt voor testclusters, maar een certificaat van een vertrouwde certificeringsinstantie moet worden gebruikt voor het beveiligen van productieclusters.

Lees voor meer informatie, [beveiliging van knooppunt-naar-knooppunt](service-fabric-cluster-security.md#node-to-node-security)

### <a name="client-to-node-security"></a>Beveiliging van de client-naar-knooppunt
Beveiliging van de client-naar-knooppunt clients te verifiëren en helpt veilige communicatie tussen een client en de afzonderlijke knooppunten in het cluster. Dit type beveiliging zorgt ervoor dat alleen gemachtigde gebruikers toegang krijgen tot het cluster en de toepassingen die zijn geïmplementeerd op het cluster. Clients worden uniek geïdentificeerd door een van de beveiligingsreferenties van de X.509-certificaat. Een willekeurig aantal optionele clientcertificaten kan worden gebruikt om beheerder of gebruiker clients met het cluster te verifiëren.

Naast de clientcertificaten, worden Azure Active Directory ook geconfigureerd om te verifiëren van clients met het cluster.

Lees voor meer informatie, [beveiliging van de Client-naar-knooppunt](service-fabric-cluster-security.md#client-to-node-security)

### <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer
Rollen gebaseerd toegangsbeheer (RBAC) kunt u fijnmazig toegangsbeheer op Azure-resources toewijzen.  U kunt verschillende toegangsregels toewijzen aan abonnementen, resourcegroepen en resources.  RBAC-regels worden overgenomen langs de resource-hiërarchie, tenzij op een lager niveau wordt overschreven.  U kunt gebruikers of gebruikersgroepen op uw AAD met RBAC regels zodat aangewezen gebruikers en groepen uw cluster kunnen wijzigen.  Lees voor meer informatie de [Azure RBAC-overzicht](/azure/role-based-access-control/overview).

Service Fabric biedt ook ondersteuning voor toegangsbeheer om te beperken van toegang tot bepaalde bewerkingen voor een cluster voor verschillende groepen gebruikers. Zo kunt u het cluster beter te beveiligen. Twee access control-typen worden ondersteund voor clients die verbinding met een cluster maken: Beheerdersrol en gebruikersrol.  

Lees voor meer informatie, [Service Fabric Role-Based toegangsbeheer (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac).

### <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen 
Netwerkbeveiligingsgroepen (nsg's) bepalen binnenkomend en uitgaand verkeer van een subnet, VM of specifieke NIC.  Standaard als meerdere virtuele machines in hetzelfde virtuele netwerk wordt gezet kunnen ze communiceren met elkaar via een willekeurige poort.  Als u wilt beperken de communicatie tussen de machines kunt u nsg's voor het segmenteren van het netwerk of virtuele machines van elkaar isoleren.  Als u meerdere typen in een cluster hebt, kunt u nsg's toepassen op subnetten om te voorkomen dat computers die behoren tot verschillende knooppunttypen met elkaar te communiceren.  

Lees voor meer informatie over [beveiligingsgroepen](/azure/virtual-network/security-overview)

## <a name="scaling"></a>Schalen

Toepassingsbehoeften veranderen verloop van tijd. Mogelijk moet u verhogen clusterbronnen om te voldoen aan een grotere werkbelasting of netwerk verkeer of verkleinen van clusterbronnen wanneer de vraag afneemt. Na het maken van een Service Fabric-cluster, kunt u het cluster horizontaal schalen (het aantal knooppunten wijzigen) of verticaal (de resources van de knooppunten wijzigen). U kunt het cluster schalen op elk gewenst moment, zelfs wanneer workloads worden uitgevoerd op het cluster. Als het cluster wordt geschaald, wordt uw toepassingen automatisch ook schalen.

Lees voor meer informatie, [Azure schalen clusters](service-fabric-cluster-scaling.md).

## <a name="upgrading"></a>Upgraden
Een Azure Service Fabric-cluster is een resource die u bezit, maar gedeeltelijk wordt beheerd door Microsoft. Microsoft is verantwoordelijk voor het onderliggende besturingssysteem toepassen van patches en upgrades van Service Fabric-runtime uitvoert op uw cluster. U kunt instellen van uw cluster runtime automatische upgrades, ontvangen wanneer een nieuwe versie door Microsoft worden uitgegeven of kies een ondersteunde runtime-versie die u wilt. Naast de runtime-upgrades, kunt u ook clusterconfiguratie, zoals certificaten of toepassingspoorten bijwerken.

Lees voor meer informatie, [clusters upgraden](service-fabric-cluster-upgrade.md).

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
Kunt u clusters maken in virtuele machines die deze besturingssystemen wordt uitgevoerd:

* Windows Server 2012 R2
* Windows Server 2016 
* Windows Server 1709
* WindowsServer 1803
* Linux Ubuntu 16.04
* Red Hat Enterprise Linux 7.4 (ondersteuning voor Preview-versie)

> [!NOTE]
> Als u besluit het implementeren van Service Fabric in Windows Server 1709, houd er rekening mee dat (1) het is niet een van long term servicing branch, zodat u wellicht versies in de toekomst te verplaatsen, en (2) als u containers implementeert, containers die zijn gebouwd op Windows Server 2016 niet geschikt voor Windows Server  1709, en vice versa (u moet ze voor het implementeren van deze opnieuw maken).
>


## <a name="next-steps"></a>Volgende stappen
Meer informatie over [beveiligen](service-fabric-cluster-security.md), [schalen](service-fabric-cluster-scaling.md), en [upgraden](service-fabric-cluster-upgrade.md) Azure-clusters.

Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md).

[Image]: media/service-fabric-azure-clusters-overview/Cluster.PNG