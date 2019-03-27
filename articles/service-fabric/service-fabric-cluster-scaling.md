---
title: Azure Service Fabric-cluster schalen | Microsoft Docs
description: Meer informatie over Azure Service Fabric-clusters in of uit en omhoog of omlaag schalen.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: aljo
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/13/2018
ms.author: aljo
ms.openlocfilehash: 5caae0a354ba0d68a9c089d2604acee96aa0128b
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499825"
---
# <a name="scaling-azure-service-fabric-clusters"></a>Schalen van Azure Service Fabric-clusters
Een Service Fabric-cluster is een netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Een machine of virtuele machine die deel uitmaakt van een cluster, heet een knooppunt. Clusters kunnen mogelijk duizenden knooppunten bevatten. Na het maken van een Service Fabric-cluster, kunt u het cluster horizontaal schalen (het aantal knooppunten wijzigen) of verticaal (de resources van de knooppunten wijzigen).  U kunt het cluster schalen op elk gewenst moment, zelfs wanneer workloads worden uitgevoerd op het cluster.  Als het cluster wordt geschaald, wordt uw toepassingen automatisch ook schalen.

Waarom het cluster schalen? Toepassingsbehoeften veranderen verloop van tijd.  Mogelijk moet u verhogen clusterbronnen om te voldoen aan een grotere werkbelasting of netwerk verkeer of verkleinen van clusterbronnen wanneer de vraag afneemt.

## <a name="scaling-in-and-out-or-horizontal-scaling"></a>In en uit te schalen of horizontaal schalen
Het aantal knooppunten in het cluster wordt gewijzigd.  Zodra de nieuwe knooppunten die zijn toegevoegd aan het cluster, de [Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md) verplaatst van services toe, waardoor er minder belasting van de bestaande knooppunten.  Als de resources van het cluster niet efficiënt worden gebruikt, kunt u ook het aantal knooppunten verminderen.  Als knooppunten het cluster laat, worden de services verplaatsen uit deze knooppunten en toeneemt op de resterende knooppunten.  Verlaag het aantal knooppunten in een cluster worden uitgevoerd in Azure kunt u geld besparen, omdat u voor het aantal VM's u gebruikt en niet de werkbelasting op deze virtuele machines betaalt.  

- Voordelen: Oneindige schaal, in theorie.  Als uw toepassing is ontworpen voor schaalbaarheid, kunt u onbeperkte groei inschakelen door meer knooppunten toe te voegen.  De hulpmiddelen in cloudomgevingen kunt eenvoudig toevoegen of verwijderen van knooppunten, zodat het eenvoudig om aan te passen capaciteit is en u betaalt alleen voor de resources die u gebruikt.  
- Nadelen: Toepassingen worden gebruikt, [ontworpen voor schaalbaarheid](service-fabric-concepts-scalability.md).  Databases van toepassingen en persistentie moet mogelijk extra architectuur werk zo goed te kunnen schalen.  [Betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md) in stateful Service Fabric-services, maken het echter veel gemakkelijker te schalen van uw toepassingsgegevens.

Virtuele-machineschaalsets vormen een Azure compute-resource die u gebruiken kunt om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunttype die is gedefinieerd in een Azure-cluster is [instellen als een afzonderlijke schaalset](service-fabric-cluster-nodetypes.md). Elk knooppunttype kan vervolgens worden geschaald of uit onafhankelijk van elkaar, hebben verschillende sets open poorten en verschillende capaciteitsstatistieken hebben. 

Wanneer u een Azure-cluster, houd rekening met de volgende richtlijnen:
- primaire knooppunttypen uitvoeren van productieworkloads moeten altijd vijf of meer knooppunten hebben.
- niet-primaire knooppunttypen uitvoeren van stateful productieworkloads moeten altijd vijf of meer knooppunten hebben.
- niet-primaire knooppunttypen uitvoeren van productieworkloads stateless moeten altijd twee of meer knooppunten hebben.
- Elk knooppunttype [duurzaamheidsniveau](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) Gold of Silver moet altijd zijn vijf of meer knooppunten.
- Willekeurige VM-exemplaren/knooppunten verwijderen uit een knooppunttype, altijd gebruik van de virtuele machine scale set omlaag schalen functie niet. Het verwijderen van willekeurige VM-exemplaren kan nadelige invloed heeft op de mogelijkheid systemen goed taakverdeling.
- Als regels voor automatisch schalen, stelt u de regels zodat één knooppunt tegelijk inschalen (VM-instanties verwijderen) wordt uitgevoerd. Meer dan één exemplaar tegelijk verkleint is niet veilig.

Omdat de Service Fabric-knooppunttypen in uw cluster zijn opgebouwd uit virtuele-machineschaalsets met de back-end, kunt u [regels voor automatisch schalen instellen of handmatig schalen](service-fabric-cluster-scale-up-down.md) elk type/virtuele-knooppunt-machineschaalset.

### <a name="programmatic-scaling"></a>Programmatisch schalen
In veel scenario's, [schalen van een cluster handmatig of met regels voor automatisch schalen](service-fabric-cluster-scale-up-down.md) zijn goede oplossingen. Voor meer geavanceerde scenario's, maar ze mogelijk niet geschikt. Nadelen aan deze methoden zijn onder andere:

- Handmatig schalen, moet u zich aanmeldt bij en aanvragen vergroten / verkleinen. Als vergroten / verkleinen vaak vereist of onvoorspelbare tijde is, is deze benadering mogelijk niet een goede oplossing.
- Wanneer automatisch schalen regels een exemplaar van een virtuele-machineschaalset verwijdert, verwijder ze automatisch niet kennis van dat knooppunt uit de gekoppelde Service Fabric-cluster, tenzij het knooppunttype een duurzaamheidsniveau van Silver- of Gold heeft. Omdat de regels voor automatisch schalen werkt in de schaalset niveau (in plaats van op het niveau van de Service Fabric), kunnen regels voor automatisch schalen Service Fabric-knooppunten verwijderen zonder dat ze probleemloos afgesloten. Deze verwijdering ruwe knooppunt laat 'ghost' status van Service Fabric-knooppunt achter na schaal-in-bewerkingen. Een persoon (of een service), moet voor het periodiek opschonen van de status van het verwijderde knooppunt in het Service Fabric-cluster.
- Verwijderde knooppunten ruimt een knooppunttype met een duurzaamheidsniveau van Gold of Silver automatisch zodat er geen extra opschonen nodig is.
- Er is wel [veel metrische gegevens](../azure-monitor/platform/autoscale-common-metrics.md) wordt ondersteund door regels voor automatisch schalen, het is nog steeds een beperkte set. Als uw scenario aanroept voor het schalen op basis van bepaalde meetwaarde niet wordt gedekt in deze is ingesteld, klikt u vervolgens regels voor automatisch schalen niet mogelijk een goede optie.

Hoe u het schalen van Service Fabric te benaderen, is afhankelijk van uw scenario. Als schalen niet gebruikelijk is, is de mogelijkheid toevoegen of verwijderen van knooppunten handmatig is waarschijnlijk voldoende. Voor complexere scenario's bieden regels voor automatisch schalen en SDK's die de mogelijkheid om programmatisch schalen een krachtige alternatieven.

Azure API's bestaan waarmee toepassingen programmatisch werken met virtuele machine-schaalsets en Service Fabric-clusters. Als bestaande opties voor automatisch schalen voor uw scenario niet werkt, maken deze API's het mogelijk om aangepaste vergroten/verkleinen logica te implementeren. 

Een benadering voor het implementeren van deze functionaliteit automatisch schalen 'die' is een nieuwe stateless service toevoegen aan de Service Fabric-toepassing voor het beheren van vergroten / verkleinen. Het maken van uw eigen service schalen biedt dat de hoogste mate van controle en aanpasbaarheid via uw toepassing het gedrag schalen. Dit kan zijn handig voor scenario's vereisen nauwkeurige controle over hoe of wanneer een toepassing kan worden geschaald in of uit. Dit besturingselement is voorzien van een negatieve gevolgen voor de code complexer. Met deze aanpak betekent dat u wilt eigen vergroten/verkleinen code, die essentieel is. Binnen een van de service `RunAsync` methode, een set van triggers kunt bepalen of schalen vereist is (met inbegrip van de parameters op, zoals de maximale clustergrootte controleren en schalen van cooldowns).   

De API die wordt gebruikt voor de virtuele machine scale set interacties (zowel om te controleren van het huidige aantal exemplaren van de virtuele machine en om deze te wijzigen) is de [fluent Azure Management-Compute-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/). De fluent compute-bibliotheek biedt een API eenvoudig te gebruiken voor interactie met virtual machine scale sets.  Gebruiken om te communiceren met de Service Fabric-cluster zelf, [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).

De schaal code hoeft te worden uitgevoerd als een service in het cluster kunnen worden geschaald, hoewel. Beide `IAzure` en `FabricClient` kunt verbinding maken met hun bijbehorende Azure-resources op afstand, zodat de service vergroten/verkleinen eenvoudig worden kan een consoletoepassing of een Windows-service wordt uitgevoerd van buiten de Service Fabric-toepassing.

Op basis van deze beperkingen, u kunt desgewenst [implementeren meer aangepaste modellen voor automatisch vergroten/verkleinen](service-fabric-cluster-programmatic-scaling.md).

## <a name="scaling-up-and-down-or-vertical-scaling"></a>Omhoog en omlaag schalen of verticaal schalen 
Hiermee wijzigt u de resources (CPU, geheugen of opslag) van knooppunten in het cluster.
- Voordelen: Architectuur van software en de toepassing blijft hetzelfde.
- Nadelen: Beperkte schaal, omdat er een limiet aan hoeveel u resources op afzonderlijke knooppunten kunt verhogen. Downtime, omdat u nemen fysieke of virtuele machines offline moet wilt toevoegen of verwijderen van resources.

Virtuele-machineschaalsets vormen een Azure compute-resource die u gebruiken kunt om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunttype die is gedefinieerd in een Azure-cluster is [instellen als een afzonderlijke schaalset](service-fabric-cluster-nodetypes.md). Vervolgens kan elk knooppunttype afzonderlijk worden beheerd.  Een knooppunttype schaal omhoog of omlaag omvat het wijzigen van de SKU van de virtuele machine-exemplaren in de schaalset. 

> [!WARNING]
> We raden u aan de VM-SKU van een scale set/knooppunttype niet te wijzigen, tenzij deze wordt uitgevoerd op [duurzaamheid van Silver of hoger](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). VM-SKU-grootte te wijzigen, is een gegevens-destructieve ter plekke infrastructuur-bewerking. Zonder enige mogelijkheid vertraging of bewaken van deze wijziging, is het mogelijk dat de bewerking kan leiden gegevensverlies voor stateful services tot of andere onvoorziene operationele problemen, zelfs voor staatloze werkbelastingen veroorzaken. 
>

Wanneer u een Azure-cluster, houd rekening met de volgende richtlijn:
- Als het verkleinen van een primaire knooppunttype, u moet nooit het omlaag schalen meer dan wat de [betrouwbaarheidslaag](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) kunt.

Het proces voor het schalen van een knooppunttype omhoog of omlaag verschilt afhankelijk van of het een niet-primaire of de primaire knooppunttype.

### <a name="scaling-non-primary-node-types"></a>Typen niet-primaire knooppunten schalen
Maak een nieuw knooppunttype met de bronnen die u nodig.  Werk de plaatsingsbeperkingen van het uitvoeren van services om op te nemen van het nieuwe knooppunttype.  Geleidelijk (één voor één), verminderen het aantal exemplaren van de oude exemplaren van het type voor knooppunt op nul, zodat de betrouwbaarheid van het cluster wordt niet beïnvloed.  Services wordt geleidelijk migreren naar het nieuwe knooppunttype als het oude knooppunttype is uit bedrijf genomen.

### <a name="scaling-the-primary-node-type"></a>Het primaire knooppunttype schalen
We raden u aan de VM-SKU van het primaire knooppunttype niet te wijzigen. Als u meer clustercapaciteit nodig hebt, kunt u het beste meer instanties toevoegen. 

Als dat niet mogelijk is, kunt u een nieuw cluster en [toepassingsstatus terugzetten](service-fabric-reliable-services-backup-restore.md) (indien van toepassing) van het oude cluster. U hoeft niet te herstellen van een systeemstatus van de service, worden ze opnieuw gemaakt wanneer u uw toepassingen naar het nieuwe cluster implementeert. Als u alleen stateless toepassingen uitgevoerd op het cluster, en u hoeft alleen uw toepassingen naar het nieuwe cluster te implementeren, er is niets om te herstellen. Als u besluit om te gaan van de niet-ondersteunde route en wilt wijzigen van de VM-SKU, instellen vervolgens maken wijzigingen in de virtuele-machineschaalset modeldefinitie in overeenstemming met de nieuwe SKU. Als uw cluster slechts één knooppunttype heeft, zorg ervoor dat uw stateful toepassingen op alle reageren [levenscyclusgebeurtenissen voor replica-Service](service-fabric-reliable-services-lifecycle.md) (zoals het maken van replica is vastgelopen) in een tijdige wijze en dat uw service-replica opnieuw maken duur is minder dan vijf minuten (voor Silver duurzaamheidsniveau). 

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [toepassing schaalbaarheid](service-fabric-concepts-scalability.md).
* [Schalen van een Azure-cluster in- of uitgeschaald](service-fabric-tutorial-scale-cluster.md).
* [Een Azure-cluster programmatisch schalen](service-fabric-cluster-programmatic-scaling.md) SDK met de fluent Azure compute.
* [Een zelfstandige-cluster in- of uitschalen](service-fabric-cluster-windows-server-add-remove-nodes.md).

