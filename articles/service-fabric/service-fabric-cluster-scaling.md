---
title: Azure Service Fabric-cluster schalen | Microsoft Docs
description: Meer informatie over Service Fabric-clusters in of uit en omhoog of omlaag schalen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: aljo
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/09/2018
ms.author: ryanwi
ms.openlocfilehash: f9e3f190decdc907cf57a0235b9d7142081bd2f1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34208026"
---
# <a name="scaling-service-fabric-clusters"></a>Schalen Service Fabric-clusters
Een Service Fabric-cluster is een set netwerk verbonden virtuele of fysieke machines waarin uw microservices worden geïmplementeerd en beheerd. Een machine of virtuele machine die deel uitmaakt van een cluster wordt een knooppunt genoemd. Clusters kunnen mogelijk duizenden knooppunten bevatten. Na het maken van een Service Fabric-cluster, kunt u het cluster horizontaal schalen (het aantal knooppunten wijzigen) of verticaal (de bronnen van de knooppunten wijzigen).  U kunt het cluster schalen op elk gewenst moment, zelfs wanneer werkbelastingen worden uitgevoerd op het cluster.  Tijdens het cluster schalen, wordt uw toepassingen automatisch ook schalen.

Waarom het cluster schalen? Aanvragen van toepassing op den duur veranderen.  U moet mogelijk verhogen clusterbronnen om te voldoen aan een grotere werkbelasting of network-verkeer of verkleinen van clusterbronnen wanneer vraag uitvalt.

### <a name="scaling-in-and-out-or-horizontal-scaling"></a>Schalen en afmelden, of horizontaal schalen
Het aantal knooppunten in het cluster wordt gewijzigd.  Zodra nieuwe knooppunten toegevoegd aan het cluster, de [Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md) services naar deze verplaatst die beperkt belasting van de bestaande knooppunten.  Als de bronnen van het cluster niet efficiënt worden gebruikt, kunt u het aantal knooppunten verminderen.  Als knooppunten laat het cluster, worden services die knooppunten verlaten en verkeer toeneemt op de resterende knooppunten.  Het aantal knooppunten in een cluster worden uitgevoerd in Azure te verminderen kunt u geld besparen, aangezien u voor het aantal virtuele machines gebruiken en niet de werkbelasting op deze virtuele machines betalen.  

- Voordelen: Oneindige schaal aan, in theorie.  Als uw toepassing is ontworpen voor schaalbaarheid, kunt u onbeperkte groei inschakelen door meer knooppunten toe te voegen.  De tooling in cloudomgevingen eenvoudig toevoegen of verwijderen van knooppunten, zodat het gemakkelijk om aan te passen van capaciteit en u betaalt alleen voor de resources die u gebruikt.  
- Nadelen: Toepassingen moet [ontworpen voor schaalbaarheid](service-fabric-concepts-scalability.md).  Toepassingsdatabases en persistentie moet mogelijk extra architectuur werk zo goed te kunnen schalen.  [Betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md) in Service Fabric stateful services echter veel gemakkelijker om te schalen van uw toepassingsgegevens.

### <a name="scaling-up-and-down-or-vertical-scaling"></a>Omhoog en omlaag schalen of verticaal schalen 
Hiermee wijzigt u de resources (CPU, geheugen of opslag) van de knooppunten in het cluster.
- Voordelen: Software en toepassingsarchitectuur blijft hetzelfde.
- Nadelen: Beperkte schaal, omdat er een limiet aan hoeveel kunt u resources op afzonderlijke knooppunten verhogen. Uitvaltijd, omdat u moet ondernemen fysieke of virtuele machines offline om te kunnen toevoegen of verwijderen van resources.

## <a name="scaling-an-azure-cluster-in-or-out"></a>Een Azure-cluster in- of schalen
Virtuele-machineschaalsets zijn een Azure compute resource die u gebruiken kunt om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunttype dat is gedefinieerd in een Azure-cluster is [instellen als een afzonderlijke scale set](service-fabric-cluster-nodetypes.md). Elk knooppunttype kan vervolgens worden uitgebreid of uit onafhankelijk, hebben verschillende sets van poorten openen en andere capaciteitsmetrieken kan hebben. 

Houd rekening met de volgende richtlijnen bij het schalen van een Azure-cluster:
- primaire knooppunttypen uitgevoerd productieworkloads moeten altijd vijf of meer knooppunten hebben.
- niet-primaire knooppunttypen stateful productie-workloads uitvoeren moeten altijd vijf of meer knooppunten hebben.
- niet-primaire knooppunttypen staatloze productie-workloads uitvoeren moeten altijd twee of meer knooppunten hebben.
- Elk knooppunttype [duurzaamheid niveau](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) van goud of zilver moet altijd vijf of meer knooppunten hebben.
- Willekeurige VM-exemplaren/knooppunten van het type van een knooppunt verwijderen, gebruik altijd de virtuele machine scale set schaal omlaag functie niet. De verwijdering van VM-instanties van willekeurige kan de mogelijkheid systemen correct verdelen nadelig beïnvloeden.
- Als regels voor automatisch schalen, stelt u de regels, zodat één knooppunt tegelijk schaling in (VM-instanties verwijderen) wordt uitgevoerd. Het verkleinen van meer dan één exemplaar tegelijk is niet veilig.

Omdat de typen van de Service Fabric-knooppunt in het cluster zijn opgebouwd uit de virtuele-machineschaalset wordt ingesteld op de back-end, kunt u [regels voor automatisch schalen instellen of handmatig schalen](service-fabric-cluster-scale-up-down.md) elk knooppunt type/virtuele-machineschaalset.

### <a name="programmatic-scaling"></a>Programmatische schalen
In veel scenario's, [schalen van een cluster handmatig of automatisch schalen regels](service-fabric-cluster-scale-up-down.md) zijn goede oplossingen. Voor meer geavanceerde scenario's, maar ze mogelijk niet de juiste keuze te maken. Bij deze methoden zijn:

- Handmatig schalen, moet u zich aanmelden en aanvragen vergroten/verkleinen bewerkingen. Als vergroten/verkleinen bewerkingen vaak vereist of onvoorspelbare tijde zijn, is deze benadering mogelijk geen een goede oplossing.
- Wanneer automatisch schalen regels een exemplaar van een virtuele-machineschaalset verwijdert, verwijder ze automatisch niet kennis van dat knooppunt uit de gekoppelde Service Fabric-cluster tenzij het knooppunttype een niveau duurzaamheid van zilver of goud heeft. Omdat automatisch schalen regels werken met de schaal instellen (in plaats van op het niveau van Service Fabric), kunnen regels voor automatisch schalen Service Fabric-knooppunten verwijderen zonder afgesloten ze. Het verwijderen van ruwe knooppunt laat 'ghost' status van Service Fabric-knooppunt achter na scale-in-bewerkingen. Een persoon (of een service) moet periodiek opschonen van de status van het verwijderde knooppunt in het Service Fabric-cluster.
- Verwijderde knooppunten een knooppunttype met een serviceniveau duurzaamheid goud of zilver automatisch opgeruimd zodat er geen aanvullende opschoning nodig is.
- Er is wel [veel metrische gegevens](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) wordt ondersteund door regels voor automatisch schalen, is nog steeds een beperkte set. Als uw scenario aanroept voor schalen op basis van bepaalde metriek niet behandeld in deze groep, klikt u vervolgens regels voor automatisch schalen niet mogelijk een goede optie.

Hoe moet u benadert Service Fabric schalen, is afhankelijk van uw scenario. Als schalen ongewoon is, is de mogelijkheid toevoegen aan of het handmatig verwijderen van knooppunten is waarschijnlijk voldoende. Voor complexere scenario's, regels voor automatisch schalen en de mogelijkheid te schalen programmatisch blootstellen SDK's bieden krachtige alternatieven.

Azure-API's bestaan waarmee toepassingen programmatisch werken met virtuele machine sets schalen en Service Fabric-clusters. Als de bestaande opties voor automatisch schalen voor uw scenario niet werken, maken deze API's het mogelijk om aangepaste vergroten/verkleinen logica implementeren. 

Een aanpak voor de implementatie van deze functionaliteit automatisch schalen 'home-made' is een nieuwe staatloze service toevoegen aan de Service Fabric-toepassing voor vergroten/verkleinen bewerkingen beheren. Maken van uw eigen service schalen, biedt dat de grootste mate van controle en aanpassingsmogelijkheden via uw toepassing de gedrag schalen. Dit kan handig zijn voor scenario's waarin u nauwkeurige controle over hoe of wanneer een toepassing wordt geschaald in- of nodig zijn. Dit besturingselement is voorzien van een afweging van complexiteit bij de code. Met deze benadering, betekent dat u moet naar eigen vergroten/verkleinen code, die niet eenvoudig is. In de service `RunAsync` methode, een verzameling van triggers kan bepalen of schalen vereist is (inclusief parameters zoals maximale clustergrootte controleren en cooldowns schalen).   

De API gebruikt voor de virtuele machine scale set interacties (zowel het huidige aantal exemplaren van virtuele machines controleren en wijzigen) is de [beheersen Azure Management Compute-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/). De beheersen compute-bibliotheek biedt een API eenvoudig te gebruiken voor interactie met de virtuele-machineschaalsets.  Gebruiken om te kunnen communiceren met het Service Fabric-cluster zelf, [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).

De schaal code hoeft te worden uitgevoerd als een service in het cluster worden geschaald, hoewel. Beide `IAzure` en `FabricClient` verbinding kunnen maken met de bijbehorende Azure-resources op afstand, zodat de service vergroten/verkleinen eenvoudig worden kan een consoletoepassing of de Windows-service uitvoert vanuit buiten de Service Fabric-toepassing.

Op basis van deze beperkingen, u kunt desgewenst [implementeren meer aangepast automatisch vergroten/verkleinen modellen](service-fabric-cluster-programmatic-scaling.md).


## <a name="scaling-a-standalone-cluster-in-or-out"></a>Een zelfstandige cluster in- of schalen
Zelfstandige clusters kunt Service Fabric-cluster on-premises implementeren of in de cloudprovider van uw keuze.  Knooppunttypen bestaan uit fysieke machines of virtuele machines, afhankelijk van uw implementatie. Het proces van het schalen van een zelfstandige cluster wordt vergeleken met de clusters worden uitgevoerd in Azure, iets bewerkelijker.  U moet het aantal knooppunten in het cluster handmatig wijzigen en voer vervolgens de upgrade van een cluster-configuratie.

Verwijderen van knooppunten mogelijk meerdere upgrades initiëren. Sommige knooppunten zijn gemarkeerd met `IsSeedNode=”true”` labelen en een query uitgevoerd op het cluster kunnen worden geïdentificeerd met manifest [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest). Verwijderen van deze knooppunten kan het langer duren dan andere omdat de seed-knooppunten moeten worden verplaatst in dergelijke scenario's. Het cluster moet minimaal drie primaire knooppunt type knooppunten onderhouden.

Houd rekening met de volgende richtlijnen wanneer u een zelfstandige cluster schalen:
- De vervanging van de primaire knooppunten moet één knooppunt uitgevoerd na de andere in plaats van te verwijderen en vervolgens toe te voegen in batches.
- Voordat u een knooppunttype verwijdert, moet u controleren of er zijn geen knooppunten verwijzen naar het knooppunttype. Verwijder deze knooppunten voordat u het bijbehorende knooppunttype verwijdert. Zodra alle bijbehorende knooppunten zijn verwijderd, kunt u de NodeType verwijderen uit de clusterconfiguratie en beginnen met een configuratie met behulp van upgrade [Start ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade).

Zie voor meer informatie [schalen van een zelfstandige cluster](service-fabric-cluster-windows-server-add-remove-nodes.md).

## <a name="scaling-an-azure-cluster-up-or-down"></a>Een Azure-cluster schalen omhoog of omlaag
Virtuele-machineschaalsets zijn een Azure compute resource die u gebruiken kunt om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunttype dat is gedefinieerd in een Azure-cluster is [instellen als een afzonderlijke scale set](service-fabric-cluster-nodetypes.md). Elk knooppunttype kan vervolgens afzonderlijk worden beheerd.  Schalen van een knooppunttype omhoog of omlaag omvat het wijzigen van de SKU van de virtuele machine-exemplaren in de schaalset. 

> [!WARNING]
> We raden aan de VM-SKU van een scale set/knooppunttype niet te wijzigen wanneer deze wordt uitgevoerd op [Silver duurzaamheid of groter](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). VM-SKU-grootte wijzigen is een gegevens-destructieve in-place infrastructuur-bewerking. Zonder enige mogelijkheid om te stellen of het bewaken van deze wijziging is het mogelijk dat de bewerking kan leiden gegevensverlies van voor stateful services tot of andere onvoorziene operationele problemen kan, zelfs voor staatloze werkbelastingen veroorzaken. 
>

Houd rekening met de volgende richtlijn bij het schalen van een Azure-cluster:
- Als het verkleinen van een primaire knooppunttype, u moet nooit het omlaag schalen meer dan wat de [betrouwbaarheidslaag](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) toestaat.

Het proces van het schalen van een knooppunttype omhoog of omlaag verschilt, afhankelijk van of het is een niet-primaire of primaire knooppunttype.

### <a name="scaling-non-primary-node-types"></a>Niet-primaire knooppunttypen schalen
Een nieuw knooppunttype maken met de resources die u nodig hebt.  Werk de plaatsingsbeperkingen van het uitvoeren van services voor het nieuwe knooppunttype bevatten.  Geleidelijk (één op een tijdstip), het aantal exemplaren van de oude exemplaren van het type voor knooppunt op nul verkleinen, zodat de betrouwbaarheid van het cluster wordt niet beïnvloed.  Services worden geleidelijk worden gemigreerd naar het nieuwe knooppunttype omdat het oude knooppunttype decommisioned.

### <a name="scaling-the-primary-node-type"></a>Het primaire knooppunttype schalen
We raden u aan de VM-SKU van het primaire knooppunttype niet te wijzigen. Als u meer capaciteit van het cluster nodig hebt, raden wij meer exemplaren toe te voegen. 

Als dat niet mogelijk is, kunt u een nieuw cluster en [toepassingsstatus terugzetten](service-fabric-reliable-services-backup-restore.md) (indien van toepassing) van het oude cluster. U hoeft niet om eventuele systeemstatus van de service te herstellen, ze worden opnieuw gemaakt wanneer u uw toepassingen implementeren op het nieuwe cluster. Als u alleen staatloze toepassingen uitgevoerd op uw cluster, en u hoeft alleen uw toepassingen implementeren op het nieuwe cluster, niets om te herstellen. Als u besluit om te gaan van de niet-ondersteunde route en wilt wijzigen van de VM-SKU, ingesteld vervolgens zorg wijzigingen aan de virtuele-machineschaalset modeldefinitie in overeenstemming met de nieuwe SKU. Als uw cluster slechts één knooppunttype heeft, zorgt u ervoor dat alle stateful toepassingen op alle reageren [replica lifecycle gebeurtenissen Service](service-fabric-reliable-services-lifecycle.md) (zoals het maken van replica is vastgelopen) in een tijdige wijze en dat de replica van de service opnieuw samenstellen duur is minder dan vijf minuten (voor zilver duurzaamheid niveau). 

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [toepassing schaalbaarheid](service-fabric-concepts-scalability.md).
* [Een Azure-cluster in- of schalen](service-fabric-tutorial-scale-cluster.md).
* [Een Azure-cluster via een programma schalen](service-fabric-cluster-programmatic-scaling.md) SDK met de beheersen Azure berekenen.
* [Schalen van een cluster standaone in- of](service-fabric-cluster-windows-server-add-remove-nodes.md).

