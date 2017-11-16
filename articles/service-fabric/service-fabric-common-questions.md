---
title: Veelgestelde vragen over Microsoft Azure Service Fabric | Microsoft Docs
description: Veelgestelde vragen over Service Fabric en hun antwoorden
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: 
ms.assetid: 5a179703-ff0c-4b8e-98cd-377253295d12
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: chackdan
ms.openlocfilehash: a9b7490fd51a2a39e6438856041fb25110ddde69
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="commonly-asked-service-fabric-questions"></a>Veelgestelde vragen over Service Fabric

Er zijn veel Veelgestelde vragen over wat Service Fabric kan doen en hoe moet worden gebruikt. Dit document bevat informatie over veel van deze Veelgestelde vragen en antwoorden.

## <a name="cluster-setup-and-management"></a>Installatie en beheer

### <a name="can-i-create-a-cluster-that-spans-multiple-azure-regions-or-my-own-datacenters"></a>Kan ik een cluster die meerdere Azure-regio's of mijn eigen datacenters omvat maken?

Ja. 

De kern van het Service Fabric-clustertechnologie kunt combineren van computers waarop een overal ter wereld, zolang ze netwerk verbonden met elkaar zijn worden gebruikt. Bouwen en uitvoeren van deze clusters kunnen echter wel ingewikkeld.

Als u geïnteresseerd in dit scenario bent, raden we u aan ofwel in contact ophalen via de [Service Fabric Github lijst](https://github.com/azure/service-fabric-issues) of via uw ondersteuningsmedewerker ter verkrijging van aanvullende richtlijnen. Het Service Fabric-team werkt om extra informatie, richtlijnen en aanbevelingen in dit scenario. 

Een aantal punten die u daarbij in overweging moet nemen: 

1. De Service Fabric-cluster-bron in Azure is vandaag de dag regionale aangezien dat de schaal van de virtuele machine wordt ingesteld dat het cluster is gebouwd op. Dit betekent dat bij een storing regionale kunnen verloren gaan de mogelijkheid voor het beheren van het cluster via de Azure Resource Manager of de Azure-Portal. Dit kan gebeuren zelfs als het cluster actief blijft en u zou kunnen rechtstreeks mee. Bovendien biedt Azure vandaag niet de mogelijkheid om één virtueel netwerk dat kan worden gebruikt tussen regio's. Dit betekent dat een cluster met meerdere regio in Azure een vereist [openbare IP-adressen voor elke virtuele machine in de VM-Schaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#public-ipv4-per-virtual-machine) of [Azure VPN-Gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md). Deze netwerken keuzes verschillende gevolgen hebben voor de kosten, prestaties, en bepaalde mate toepassing ontwerpen en dus een zorgvuldige analyse en planning voordat permanente van een dergelijke omgeving is vereist.
2. Onderhoud, beheer, bewaking van deze virtuele machines kan worden ingewikkeld, vooral wanneer spanned over verschillende _typen_ omgevingen, zoals tussen verschillende cloudproviders of tussen lokale bronnen en Azure. Moet nauwkeurig om ervoor te zorgen dat upgrades, bewaking, management en diagnostische gegevens voor het cluster en de toepassingen zijn begrepen voordat productieworkloads wordt uitgevoerd in een dergelijke omgeving. Als u al veel ervaring voor het oplossen van deze problemen in Azure of in uw eigen datacenters, is het waarschijnlijk dat deze dezelfde oplossingen kunnen worden toegepast als samenstellen uit of uw Service Fabric-cluster wordt uitgevoerd. 

### <a name="do-service-fabric-nodes-automatically-receive-os-updates"></a>Ontvang Service Fabric-knooppunten automatisch updates voor het besturingssysteem?

Geen vandaag, maar dit is een algemene aanvraag die Azure wil leveren.

We hebben in de tussentijd [opgegeven van een toepassing](service-fabric-patch-orchestration-application.md) dat de besturingssystemen onder uw Service Fabric-knooppunten patches en up-to-date te houden blijven.

De uitdaging met updates voor het besturingssysteem is dat ze moeten doorgaans de computer, wat tot verlies van beschikbaarheid van tijdelijke leidt opnieuw worden opgestart. Zelfstandig gebruikt, die is geen probleem, omdat de Service Fabric worden verkeer voor deze services automatisch omgeleid naar andere knooppunten. Als de OS-updates niet in het cluster worden gecoördineerd, is er echter de kans dat veel knooppunten tegelijk uitvallen. Dergelijke gelijktijdige herstarts kunnen leiden tot volledige beschikbaarheid verlies voor een service of op minimaal gedurende een specifieke partitie (voor een stateful service).

In de toekomst zullen we ter ondersteuning van een update-beleid voor het besturingssysteem dat is volledig geautomatiseerd en gecoördineerd in meerdere domeinen, update, waarbij u ervoor zorgt dat beschikbaarheid behouden ondanks opnieuw wordt opgestart en andere onverwachte fouten blijft.

### <a name="can-i-use-large-virtual-machine-scale-sets-in-my-sf-cluster"></a>Kan ik grote virtuele-Machineschaalsets in mijn SF-cluster gebruiken? 

**Korte antwoord** : Nee. 

**Lang antwoord** : hoewel de grote virtuele-Machineschaalsets kunt u een virtuele machine schalen schalen set maximaal 1000 VM-instanties, dit gebeurt door het gebruik van groepen plaatsing (PGs). Domeinen met fouten (FDs) en upgradedomeinen (UDs) zijn alleen binnen een plaatsing groep Service fabric gebruikt FDs en UDs beslissingen voor de plaatsing van de replica's / Service-exemplaren van uw service consistent. Aangezien de FDs en UDs vergelijkbaar zijn alleen binnen een groep plaatsing SF niet gebruiken. Bijvoorbeeld, als VM1 in PG1 een topologie van FD heeft = 0 en VM9 in PG2 heeft een topologie van FD = 4, het betekent niet dat VM1 en VM2 zich op twee verschillende Hardware rekken kunnen daarom SF niet de waarden FD in dit geval om plaatsing beslissingen te nemen.

Momenteel andere problemen met een grote virtuele machine-schaalsets, zoals het ontbreken van niveau 4 Load balancing ondersteuning. Raadpleeg voor [details op grote schaal sets](../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md)



### <a name="what-is-the-minimum-size-of-a-service-fabric-cluster-why-cant-it-be-smaller"></a>Wat is de minimale grootte van een Service Fabric-cluster? Waarom kan deze niet kleiner zijn?

De minimale ondersteunde grootte voor een productie-workloads met Service Fabric-cluster is vijf knooppunten. Wij ondersteunen drie clusters met knooppunten voor scenario's met ontwikkelen en testen.

Deze alle minimumwaarden bestaan omdat de Service Fabric-cluster een reeks stateful systeemservices voert, waaronder de naming service en de failover manager. Deze services die afhankelijk zijn van sterke consistentie bijhouden welke services zijn geïmplementeerd op het cluster en waar deze momenteel wordt gehost. De mogelijkheid om toegang te verkrijgen die sterke consistentie op zijn beurt afhankelijk een *quorum* voor een bepaalde update naar de status van deze services, waarbij een quorum vertegenwoordigt een strikte meerderheid van de replica's (N/2 + 1) voor een bepaalde service.

Met deze achtergrond behandeld sommige clusterconfiguraties mogelijk:

**Eén knooppunt**: deze optie biedt geen hoge beschikbaarheid sinds het verlies van één knooppunt voor een bepaalde reden het verlies van het hele cluster betekent.

**Twee knooppunten**: een quorum voor een service die is geïmplementeerd op twee knooppunten (N = 2) 2 (2/2 + 1 = 2). Als een enkele replica verbroken is, is het onmogelijk maken van een quorum. Omdat u een service-upgrade uitvoert tijdelijk duurt omlaag een replica vereist, maar dit is geen configuratie van een handig.

**Drie knooppunten**: met drie knooppunten (N = 3), de vereiste voor het maken van een quorum is nog steeds twee knooppunten (3/2 + 1 = 2). Dit betekent dat u kunt een afzonderlijke knooppunten verliezen en nog steeds quorum onderhouden.

De configuratie van het cluster drie knooppunt wordt ondersteund voor ontwikkelen en testen omdat kunt u veilig uitvoeren van upgrades en fouten voor afzonderlijke knooppunten, overleven, zolang ze niet tegelijkertijd uitgevoerd. Voor productieworkloads moet u voor een gelijktijdige storingen, robuuste zodat vijf knooppunten vereist zijn.

### <a name="can-i-turn-off-my-cluster-at-nightweekends-to-save-costs"></a>Kan ik mijn cluster op nacht/weekends om kosten te besparen uitschakelen?

In het algemeen niet. Service Fabric opgeslagen status op lokale tijdelijke schijven, wat betekent dat als de virtuele machine wordt verplaatst naar een andere host, de gegevens verplaatst geen aan. In de normale werking die is niet een probleem als het nieuwe knooppunt wordt bijgewerkt door andere knooppunten. Als u alle knooppunten stoppen en deze later opnieuw opstarten, is er echter aanzienlijke kans dat de meeste van de knooppunten starten op de nieuwe hosts en controleer het systeem niet kan herstellen.

Als u maken van clusters wilt voor uw toepassing testen voordat deze is geïmplementeerd, raden wij aan dat u deze clusters dynamisch als onderdeel van maken uw [continue integratie/continue implementatie pijplijn](service-fabric-tutorial-deploy-app-with-cicd-vsts.md).


### <a name="how-do-i-upgrade-my-operating-system-for-example-from-windows-server-2012-to-windows-server-2016"></a>Hoe voer ik een upgrade het besturingssysteem (bijvoorbeeld van Windows Server 2012 naar Windows Server 2016)?

Terwijl we op een betere ervaring vandaag werkt, bent u verantwoordelijk voor de upgrade. U moet de upgrade de installatiekopie van het besturingssysteem op de virtuele machines van het cluster een virtuele machine op een tijdstip. 

## <a name="container-support"></a>Container-ondersteuning

### <a name="why-are-my-containers-that-are-deployed-to-sf-unable-to-resolve-dns-addresses"></a>Waarom wordt mijn containers die zijn geïmplementeerd voor SF kan niet worden omgezet DNS-adressen?

Dit probleem is gerapporteerd op clusters die zich op 5.6.204.9494 versie 

**Risicobeperking** : Volg [dit document](service-fabric-dnsservice.md) inschakelen van de DNS-service fabric-service in uw cluster.

**Los** : Upgrade naar een ondersteunde cluster-versie die hoger is dan 5.6.204.9494, wanneer deze beschikbaar is. Als uw cluster is ingesteld op automatische upgrades, klikt u vervolgens upgraden het cluster automatisch naar de versie met dit probleem is opgelost.

  
## <a name="application-design"></a>Het ontwerp van toepassing

### <a name="whats-the-best-way-to-query-data-across-partitions-of-a-reliable-collection"></a>Wat is de beste manier om querygegevens meerdere partities van een betrouwbare verzameling?

Betrouwbare verzamelingen zijn meestal [gepartitioneerde](service-fabric-concepts-partitioning.md) scale-out voor betere prestaties en doorvoer inschakelen. Dat betekent dat de status voor een bepaalde service kan worden verdeeld over per 10 of 100s machines. Als u wilt bewerkingen uitvoert via dat volledige gegevensset, hebt u een aantal opties:

- Maken van een service waarmee een query op alle partities van een andere service ophalen van de vereiste gegevens.
- Maken van een service die u gegevens van alle partities van een andere service ontvangen kunt.
- Gegevens van elke service periodiek pushen naar een externe winkel. Deze aanpak is alleen van toepassing als de query's die u uitvoert geen deel uitmaken van uw bedrijfslogica core.


### <a name="whats-the-best-way-to-query-data-across-my-actors"></a>Wat is de beste manier om gegevens opvragen via Mijn actoren?

Actors zijn bedoeld als onafhankelijke eenheden van de status en de compute-, zodat het niet aangeraden wordt voor het uitvoeren van algemene query's van de actorstatus tijdens runtime. Als u een query moet op de volledige set actorstatus hebt, moet u overwegen een:

- Vervangen door uw actorservices betrouwbare stateful services zodanig dat het aantal netwerk aanvragen voor het verzamelen van alle gegevens van het aantal actoren aan het aantal partities in uw service.
- Het ontwerpen van uw actoren voor de push-regelmatig hun status naar een externe store eenvoudiger query's. Als is hierboven, deze benadering alleen uitvoerbaar als de query's die u uitvoert, niet vereist voor uw runtimegedrag zijn.

### <a name="how-much-data-can-i-store-in-a-reliable-collection"></a>Hoeveel gegevens kan ik opslaan in een betrouwbare verzameling?

Betrouwbare services worden doorgaans gepartitioneerd, de hoeveelheid die u kunt opslaan, wordt alleen beperkt door het aantal machines hebt u in het cluster en de hoeveelheid geheugen die beschikbaar is op deze computers.

Stel bijvoorbeeld dat u hebt een betrouwbare verzameling in een service met 100 partities en 3 replica's voor het opslaan van objecten die 1kb groot gemiddelde. Stel nu dat u een machinecluster 10 met 16gb geheugen per computer hebt. Verbruiken 6gb aan dat het verlaten van 10gb beschikbare per computer of 100gb voor het cluster voor eenvoud en voor het zeer conservatief zijn, wordt ervan uitgegaan dat het besturingssysteem en systeemservices, de Service Fabric-runtime en uw services.

Houd in gedachten dat elk object moet worden opgeslagen drie tijden (een primaire en twee replica's), hebt u voldoende geheugen voor ongeveer 35 miljoen objecten in uw verzameling indien de volledige capaciteit. We raden echter aan het gelijktijdige verlies van een domein is mislukt en een upgradedomein, die ongeveer 1/3 capaciteit vertegenwoordigt, en het aantal zou beperken tot ongeveer 23 miljoen wordt.

Houd er rekening mee dat deze berekening ook wordt ervan uitgegaan dat:

- Dat de distributie van gegevens over de partities min of meer uniform is of dat u bent reporting metrische gegevens laden in het Cluster Resource Manager. Standaard wordt de Service Fabric saldo op basis van het aantal replica's geladen. In het bovenstaande voorbeeld plaatst die 10 primaire replica's en 20 secundaire replica's op elk knooppunt in het cluster. Die geschikt is voor belasting die evenredig verdeeld over de partities. Als er geen load zelfs is, moet u load melden zodat de Resource Manager kunt pack kleinere replica's samen en grotere replica's verbruikt meer geheugen op een afzonderlijke knooppunten.

- De betrouwbare service in kwestie is de status van de slechts één opslaan in het cluster. Omdat u meerdere services naar een cluster implementeren kunt, moet u zijn toegewezen resources dat elk moet uitvoeren en beheren van de status.

- Dat is niet het cluster zelf groeiende of verkleinen. Als u meer machines toevoegt, wordt de Service Fabric opnieuw verdelen replica's voor het gebruik van de extra capaciteit totdat het aantal machines het aantal partities in uw service, overschrijdt omdat de replica van een afzonderlijke niet meerdere machines omvatten. Daarentegen als u de grootte van het cluster verminderen door het verwijderen van computers, replica's veiliger worden ingepakt en minder algehele capaciteit.

### <a name="how-much-data-can-i-store-in-an-actor"></a>Hoeveel gegevens kan ik opslaan in een actor

Net als bij betrouwbare services, wordt de hoeveelheid gegevens die u kunt opslaan in een actor-service alleen beperkt door de totale schijfruimte en het beschikbaar geheugen op de knooppunten in het cluster. Afzonderlijke actoren zijn echter meest effectief wanneer ze worden gebruikt voor een kleine hoeveelheid status en de bijbehorende bedrijfslogica inkapselen. Als in het algemeen moet een afzonderlijke actor staat dat wordt gemeten in kilobytes hebben.

## <a name="other-questions"></a>Andere vragen

### <a name="how-does-service-fabric-relate-to-containers"></a>Hoe Service Fabric relateren aan containers?

Containers bieden een eenvoudige manier om het pakketservices en de bijbehorende afhankelijkheden zodanig zijn dat ze consistent worden uitgevoerd in alle omgevingen en kunnen worden uitgevoerd in een geïsoleerde wijze op een enkele computer. Service Fabric biedt een manier om te implementeren en beheren van services, waaronder [services die in een container verpakt zijn](service-fabric-containers-overview.md).

### <a name="are-you-planning-to-open-source-service-fabric"></a>Bent u van plan bron Service Fabric openen?

We proberen te openen gegevensbron reliable services en betrouwbare actoren frameworks op GitHub en community bijdragen aan projecten worden geaccepteerd. Volg de [Service Fabric-blog](https://blogs.msdn.microsoft.com/azureservicefabric/) voor meer informatie, zoals deze wordt vermeld.

De zijn momenteel geen plannen openen bron voor de Service Fabric-runtime.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over Service Fabric-kernconcepten en best practices](https://mva.microsoft.com/en-us/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965)
