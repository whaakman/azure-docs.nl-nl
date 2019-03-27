---
title: Veelgestelde vragen over Microsoft Azure Service Fabric | Microsoft Docs
description: Veelgestelde vragen over Service Fabric en antwoorden
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: ''
ms.assetid: 5a179703-ff0c-4b8e-98cd-377253295d12
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: chackdan
ms.openlocfilehash: aa287111a9880911c7807734b566bf60f42db85e
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58447789"
---
# <a name="commonly-asked-service-fabric-questions"></a>Veelgestelde vragen over Service Fabric

Er zijn veel Veelgestelde vragen over Service Fabric kunt doen en hoe deze moet worden gebruikt. In dit document bevat informatie over veel van deze Veelgestelde vragen en antwoorden.

## <a name="cluster-setup-and-management"></a>Installatie en beheer

### <a name="how-do-i-roll-back-my-service-fabric-cluster-certificate"></a>Hoe ik terugdraaien mijn certificaat Service Fabric-cluster?

Een upgrade voor uw toepassing vereist het terugdraaien van een health foutdetectie voorafgaand aan uw Service Fabric-clusterquorum doorvoeren van de wijziging; doorgevoerde wijzigingen kunnen alleen worden doorgevoerd. Escalation engineer via Customer Support Services mogelijk vereist voor het herstellen van uw cluster als een niet-bewaakte belangrijke wijziging van het certificaat is geïntroduceerd.  [De service Fabric-toepassingsupgrade](https://review.docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade?branch=master) is van toepassing [parameters toepassingsupgrade](https://review.docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-parameters?branch=master), en levert nul downtime upgrade belofte.  Na onze aanbevolen toepassing upgraden bewaakte modus, automatisch wordt uitgevoerd via updatedomeinen is gebaseerd op statuscontroles aan te, rolling back automatisch als het bijwerken van een standaardservice is mislukt.
 
Als uw cluster maakt nog steeds gebruik van de klassieke eigenschap vingerafdruk van het certificaat in de Resource Manager-sjabloon, wordt u aangeraden [wijzigen-cluster op basis van de vingerafdruk van certificaat in een algemene naam](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-change-cert-thumbprint-to-cn), gebruikmaken van moderne geheimen beheerfuncties.

### <a name="can-i-create-a-cluster-that-spans-multiple-azure-regions-or-my-own-datacenters"></a>Kan ik een cluster die meerdere Azure-regio's of mijn eigen datacenters omvat maken?

Ja. 

De kern van het Service Fabric-clusters technologie kan worden gebruikt op computers met een willekeurige plaats in de hele wereld, combineren, zolang ze verbinding met het netwerk aan elkaar hebben. Echter, het bouwen en uitvoeren van deze clusters kunnen ingewikkeld zijn.

Als u geïnteresseerd in dit scenario bent, raden we u aan in Neem contact op met ofwel ophalen via de [lijst van Service Fabric GitHub-problemen](https://github.com/azure/service-fabric-issues) of via uw ondersteuningsmedewerker om te verkrijgen van aanvullende richtlijnen. De Service Fabric-team werkt aan meer duidelijkheid, richtlijnen en aanbevelingen voor dit scenario. 

Een aantal punten die u daarbij in overweging moet nemen: 

1. De resource van de Service Fabric-cluster in Azure is vandaag de dag regionale als dat de virtuele-machineschaalsets dat het cluster is gebouwd op. Dit betekent dat er een regionale storing optreedt u de mogelijkheid verliest voor het beheren van het cluster via Azure Resource Manager of de Azure-portal. Dit kan gebeuren zelfs als het cluster actief blijft en u zou kunnen communiceren met het rechtstreeks. Bovendien biedt Azure vandaag de mogelijkheid om één virtueel netwerk dat kan worden gebruikt in regio's. Dit betekent dat een cluster met meerdere regio's in Azure een vereist [openbare IP-adressen voor elke virtuele machine in de VM-Schaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#public-ipv4-per-virtual-machine) of [Azure VPN-Gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md). Deze netwerken keuzes verschillende gevolgen hebben voor de kosten, prestaties, en aan bepaalde mate toepassingsontwerp, dus een zorgvuldige analyse en planning voor het handhaven van dergelijke omgeving is vereist.
2. Het onderhoud, beheer en bewaking van deze virtuele machines kan ingewikkeld worden, met name wanneer liep over _typen_ van omgevingen, zoals tussen verschillende cloudproviders of tussen on-premises bronnen en Azure. Wees voorzichtig om ervoor te zorgen dat upgrades, bewaking, beheer en diagnostische gegevens voor het uitvoeren van productieworkloads in een dergelijke omgeving voor het cluster en de toepassingen worden begrepen. Als u al ervaring voor het oplossen van deze problemen in Azure of in uw eigen datacenters, is het waarschijnlijk dat deze dezelfde oplossingen kunnen worden toegepast wanneer het bouwen van of uw Service Fabric-cluster uitgevoerd. 

### <a name="do-service-fabric-nodes-automatically-receive-os-updates"></a>Ontvang Service Fabric-knooppunten automatisch updates van het besturingssysteem?

U kunt [Virtual Machine Scale Stel automatische installatiekopie Update van het besturingssysteem](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) algemeen beschikbare functie vandaag nog.

Voor clusters die niet worden uitgevoerd in Azure, hebben we [opgegeven van een toepassing](service-fabric-patch-orchestration-application.md) voor het vullen van de besturingssystemen worden uitgevoerd onder uw Service Fabric-knooppunten.

### <a name="can-i-use-large-virtual-machine-scale-sets-in-my-sf-cluster"></a>Kan ik grote virtuele-machineschaalsets gebruiken in mijn SF cluster? 

**Korte antwoord** : Nee 

**Lang antwoord** : hoewel de grote virtuele-machineschaalsets u kunnen voor het schalen van een virtuele machine maximaal 1000 VM-exemplaren Virtual Machine scale sets, doet dit door het gebruik van Plaatsingsgroepen (PGs). Foutdomeinen (FD's) en upgrade-domeinen (ud's) zijn alleen consistent binnen een plaatsing groep Service fabric gebruikt Foutdomeinen en Upgradedomeinen om beslissingen te plaatsing van uw service-replica's / Service-exemplaren. Aangezien de Foutdomeinen en Upgradedomeinen alleen binnen een plaatsingsgroep vergelijkbare zijn, moet op SF deze niet gebruiken. Bijvoorbeeld, als VM1 in PG1 een topologie van FD heeft = 0 en VM9 in PG2 heeft een topologie van FD = 4, dit betekent niet dat VM1 en VM2 zich op twee verschillende Hardware rekken, daarom SF niet gebruiken de FD-waarden in dit geval om beslissingen voor plaatsing.

Er momenteel andere problemen met grote virtuele-machineschaalsets, zoals het ontbreken van een niveau-4 taakverdeling ondersteuning niet laden. Raadpleeg voor [meer informatie over grote schaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md)



### <a name="what-is-the-minimum-size-of-a-service-fabric-cluster-why-cant-it-be-smaller"></a>Wat is de minimale grootte van een Service Fabric-cluster? Waarom kan deze niet kleiner zijn?

De minimale ondersteunde grootte voor een Service Fabric-cluster uitvoeren van productieworkloads is vijf knooppunten. Voor scenario's voor ontwikkelen ondersteunen we één knooppunt (geoptimaliseerd voor snelle ontwikkeling biedt bij het Visual Studio) en clusters met vijf knooppunten.

Vereist een productiecluster ten minste 5 knooppunten vanwege de volgende drie redenen hebben:
1. Zelfs als er geen gebruiker-services worden uitgevoerd, wordt een set van stateful systeemservices, waaronder de naming-service en de failover manager-service uitgevoerd in een Service Fabric-cluster. Deze systeemservices zijn essentieel voor het cluster blijven werken.
2. We geven altijd één replica van een service per knooppunt, zodat de clustergrootte de bovengrens van het aantal replica's die een service (daadwerkelijk een partitie is) kan hebben.
3. Nadat de clusterupgrade van een wordt ten minste één knooppunt uitvallen, we willen dat een buffer van minstens één knooppunt, dus we willen een productiecluster ten minste twee knooppunten *bovendien* aan de minimumwaarde. De minimumwaarde is de grootte van het quorum van een systeemservice, zoals hieronder wordt beschreven.  

We willen dat het cluster moet beschikbaar zijn bij gelijktijdige storing van twee knooppunten. Voor een Service Fabric-cluster moet beschikbaar zijn, moeten de systeemservices beschikbaar zijn. Stateful system-services zoals naamgevingsservice en failover manager-service, die afhankelijk zijn van sterke consistentie bijhouden welke services zijn geïmplementeerd op het cluster en waar ze momenteel wordt gehost. De mogelijkheid om te verkrijgen die sterke consistentie op zijn beurt afhankelijk een *quorum* voor een bepaalde update naar de status van deze services, waarbij een quorum vertegenwoordigt een strikte meerderheid van de replica's (N/2 + 1) voor een bepaalde service. Dus als we willen tolerantie tegen gelijktijdige verlies van twee knooppunten (dus gelijktijdige verlies van twee replica's van een systeemservice), moet hebben we de clustergrootte - QuorumSize > = 2, waardoor de minimale grootte moet vijf. Om te zien die rekening houden met het cluster heeft N knooppunten en er zijn N replica's van een systeemservice--één op elk knooppunt. De grootte van het quorum voor een system-service is (N/2 + 1). De bovenstaande ongelijkheid ziet eruit als N - (N/2 + 1) > = 2. Er zijn twee mogelijke situaties rekening houden: wanneer N zelfs is en wanneer N oneven is. Als N zelfs het geval is, bijvoorbeeld N is = 2\*m waar m > = 1, de ongelijkheid eruit 2\*m - (2\*m/2 + 1) > = 2 of m > = 3. Het minimale aantal voor N 6 is en dat is bereikt wanneer m = 3. Aan de andere kant als N oneven is, zeg N = 2\*m + 1 waar m > = 1, de ongelijkheid eruit 2\*m + 1 - ((2\*m + 1) / 2 + 1) > = 2 of 2\*m + 1 - (m + 1) > = 2 of m > = 2. Het minimale aantal voor N is 5 en die is bereikt wanneer m = 2. Daarom tussen alle waarden van N die voldoen aan de ongelijkheid de clustergrootte - QuorumSize > = 2, de minimumwaarde is 5.

Opmerking: in het bovenstaande argument dat we een veronderstelde hebben dat elk knooppunt is een replica van een systeemservice, duidt dit de quorum-grootte wordt berekend op basis van het aantal knooppunten in het cluster. Echter, door het veranderen van *TargetReplicaSetSize* maken we kan de grootte van de quorum minder dan (N / 2 + 1) die mogelijk de indruk dat we kan een cluster kleiner is dan 5 knooppunten hebt en nog steeds 2 extra knooppunten hierboven de grootte van de quorum geven. Bijvoorbeeld, in een cluster met 4 knooppunten als we de TargetReplicaSetSize ingesteld op 3, de quorum-grootte op basis van TargetReplicaSetSize is (3/2 + 1) of 2, dus hebben we de clustergrootte - QuorumSize = 4-2 > = 2. Echter, we kunnen niet garanderen dat de service is bij of boven quorum als we een paar knooppunten tegelijk, gaan verloren kan het zijn dat de twee knooppunten kwijt die als host twee replica's, fungeert zijn zodat de service wordt met ingang van quorumverlies (met alleen een enkele replica naar links) een ND wordt niet beschikbaar.

Die gedachte, we gaan sommige clusterconfiguraties mogelijk:

**Een knooppunt**: deze optie biedt geen hoge beschikbaarheid na het verlies van één knooppunt voor een bepaalde reden het verlies van gegevens van het hele cluster betekent.

**Twee knooppunten**: een quorum van een service die is geïmplementeerd op twee knooppunten (N = 2) is 2 (2/2 + 1 = 2). Wanneer een enkele replica verbroken is, is het niet mogelijk om te maken van een quorum. Omdat het uitvoeren van een service-upgrade tijdelijk waarbij u een replica vereist, maar dit is geen een handig configuratie.

**Drie knooppunten**: met drie knooppunten (N = 3), de vereiste voor het maken van een quorum is nog steeds twee knooppunten (3/2 + 1 = 2). Dit betekent dat u kunt een afzonderlijke knooppunt kwijtraakt en nog steeds onderhouden quorum, maar gelijktijdige storing van twee knooppunten wordt de systeemservices in quorumverlies werken en zorgt ervoor dat het cluster niet meer beschikbaar zijn.

**Vier knooppunten**: met vier knooppunten (N = 4), is de vereiste voor het maken van een quorum drie knooppunten (4/2 + 1 = 3). Dit betekent dat u kunt een afzonderlijke knooppunt kwijtraakt en nog steeds onderhouden quorum, maar gelijktijdige storing van twee knooppunten wordt de systeemservices in quorumverlies werken en zorgt ervoor dat het cluster niet meer beschikbaar zijn.

**Vijf knooppunten**: met vijf knooppunten (N = 5), de vereiste voor het maken van een quorum is nog steeds drie knooppunten (5/2 + 1 = 3). Dit betekent dat u kunt twee knooppunten op hetzelfde moment verliezen en nog steeds quorum voor de systeemservices onderhouden.

Voor werkbelastingen voor productie, moet u zijn tegen gelijktijdig uitvallen van ten minste twee knooppunten (bijvoorbeeld een vanwege een clusterupgrade van, een vanwege andere redenen), dus vijf knooppunten vereist zijn.

### <a name="can-i-turn-off-my-cluster-at-nightweekends-to-save-costs"></a>Kan ik mijn cluster op 's nachts/weekends om kosten te besparen uitschakelen?

In het algemeen niet. Service Fabric worden status opgeslagen op lokale tijdelijke schijven, wat betekent dat als de virtuele machine wordt verplaatst naar een andere host, de gegevens geen ermee verplaatst. In de normale werking, dat is niet een probleem als het nieuwe knooppunt wordt bijgewerkt door andere knooppunten. Als u alle knooppunten stopt en start u deze later opnieuw, is er echter een aanzienlijke mogelijkheid dat de meeste van de knooppunten starten op de nieuwe hosts en controleer het systeem kan niet worden hersteld.

Als u wilt maken van clusters om uw toepassing testen voordat deze is geïmplementeerd, wordt aangeraden deze clusters dynamisch te maken als onderdeel van uw [pijplijn voor continue integratie/continue implementatie](service-fabric-tutorial-deploy-app-with-cicd-vsts.md).


### <a name="how-do-i-upgrade-my-operating-system-for-example-from-windows-server-2012-to-windows-server-2016"></a>Hoe voer ik een upgrade het besturingssysteem (bijvoorbeeld van Windows Server 2012 naar Windows Server 2016)?

Terwijl er wordt gewerkt aan een betere ervaring vandaag de dag bent u verantwoordelijk voor de upgrade. U moet de installatiekopie van het besturingssysteem op de virtuele machines van het cluster een upgrade virtuele machine op een tijdstip. 

### <a name="can-i-encrypt-attached-data-disks-in-a-cluster-node-type-virtual-machine-scale-set"></a>Kan ik gekoppelde gegevensschijven in een clusterknooppunttype (virtuele-machineschaalset) coderen?
Ja.  Zie voor meer informatie, [een cluster maken met gekoppelde gegevensschijven](../virtual-machine-scale-sets/virtual-machine-scale-sets-attached-disks.md#create-a-service-fabric-cluster-with-attached-data-disks), [schijven (PowerShell) versleutelen](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-ps.md), en [schijven (CLI) coderen](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-cli.md).

### <a name="can-i-use-low-priority-vms-in-a-cluster-node-type-virtual-machine-scale-set"></a>Kan ik virtuele machines met lage prioriteit gebruiken in een clusterknooppunttype (virtuele-machineschaalset)?
Nee. Virtuele machines met lage prioriteit worden niet ondersteund. 

### <a name="what-are-the-directories-and-processes-that-i-need-to-exclude-when-running-an-anti-virus-program-in-my-cluster"></a>Wat zijn de mappen en processen die ik wil uitgesloten wanneer er een antivirusprogramma wordt uitgevoerd in mijn cluster?

| **Antivirus uitgesloten mappen** |
| --- |
| Program Files\Microsoft Service Fabric |
| FabricDataRoot (van de configuratie van het cluster) |
| FabricLogRoot (van de configuratie van het cluster) |

| **Antivirus uitgesloten processen** |
| --- |
| Fabric.exe |
| FabricHost.exe |
| FabricInstallerService.exe |
| FabricSetup.exe |
| FabricDeployer.exe |
| ImageBuilder.exe |
| FabricGateway.exe |
| FabricDCA.exe |
| FabricFAS.exe |
| FabricUOS.exe |
| FabricRM.exe |
| FileStoreService.exe |
 
### <a name="how-can-my-application-authenticate-to-keyvault-to-get-secrets"></a>Hoe kan mijn toepassing verifiëren met Key Vault geheimen ophalen?
Hier volgen betekent voor uw toepassing om op te halen van referenties voor verificatie bij de KeyVault:

A. Tijdens uw toepassingen bouwen/verpakking taak, kunt u een certificaat in het gegevenspakket van uw app SF ophalen en dit gebruiken om te verifiëren bij de KeyVault.
B. Voor virtuele-ingeschakelde MSI-hosts machineschaalset, kunt u een eenvoudige PowerShell SetupEntryPoint voor uw app SF om op te halen ontwikkelen [een toegangstoken van het MSI-eindpunt](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token), en vervolgens [uw geheimen ophalen uit Key Vault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Get-AzureKeyVaultSecret)

## <a name="application-design"></a>Het ontwerp van toepassing

### <a name="whats-the-best-way-to-query-data-across-partitions-of-a-reliable-collection"></a>Wat is de beste manier om gegevens te doorzoeken over meerdere partities van een betrouwbare verzameling?

Betrouwbare verzamelingen zijn doorgaans [gepartitioneerde](service-fabric-concepts-partitioning.md) om in te schakelen van scale-out voor betere prestaties en doorvoer. Dit betekent dat dat de status voor een bepaalde service kan worden verdeeld over tientallen of honderden computers. Om uit te voeren bewerkingen ten opzichte van die set met volledige gegevens, hebt u enkele opties:

- Maken van een service die in alle partities van een andere service om op te halen in de vereiste gegevens zoekt.
- Maken van een service waarmee gegevens kan worden ontvangen van alle partities van een andere service.
- Gegevens van elke service regelmatig pushen naar een externe opslag. Deze methode is alleen van toepassing als het uitvoeren van query's geen deel uit van uw belangrijkste zakelijke logica maken.


### <a name="whats-the-best-way-to-query-data-across-my-actors"></a>Wat is de beste manier om gegevens te doorzoeken op mijn actoren?

Actoren zijn ontworpen om onafhankelijke eenheden van de status en rekenkracht, dus deze niet aanbevolen wordt om uit te voeren van uitgebreide query's van de actorstatus tijdens runtime. Hebt u behoefte aan een query op de volledige set van de actorstatus, moet u overwegen een:

- Vervangen door uw actorservices stateful reliable services, zodat het aantal aanvragen voor het verzamelen van alle gegevens van het aantal actoren op het aantal partities in uw service.
- Het ontwerpen van uw actoren regelmatig hun status naar een externe opslag voor eenvoudiger query's te pushen. Als is hierboven, deze benadering alleen mogelijk als het uitvoeren van query's niet vereist voor de runtimegedrag zijn.

### <a name="how-much-data-can-i-store-in-a-reliable-collection"></a>Hoeveel gegevens kan ik opslaan in een betrouwbare verzameling?

Betrouwbare services zijn meestal gepartitioneerd, zodat de hoeveelheid die u kunt opslaan wordt alleen beperkt door het aantal machines dat u in het cluster hebt en de hoeveelheid geheugen die beschikbaar is op deze computers.

Een voorbeeld: Stel dat u hebt een betrouwbare verzameling in een service met 100 partities en 3 replica's voor het opslaan van objecten die een gemiddelde van 1 kb groot. Stel nu dat u een machinecluster met 10 met 16gb geheugen per machine hebt. Voor het gemak te vermijden, wordt ervan uitgegaan dat het besturingssysteem en systeemservices, de Service Fabric-runtime en uw services gebruikmaken van 6gb aan dat het verlaten van 10gb per machine beschikbaar, of 100 gb voor het cluster.

Houd er rekening mee dat elk object moet worden opgeslagen drie keer (één primair knooppunt en twee replica's), zou u voldoende geheugen voor ongeveer 35 miljoen objecten in uw verzameling hebben als het wordt uitgevoerd op volledige capaciteit. We raden echter aan dat aan de gelijktijdige verlies van een domein mislukt en een upgradedomein, die ongeveer 1/3 van de capaciteit aangeeft, en wordt minder tot ongeveer 23 miljoen.

Houd er rekening mee dat voor deze berekening ook wordt ervan uitgegaan dat:

- Dat de verdeling van gegevens over de partities ongeveer uniform is of dat u meetwaarden voor het Cluster Resource Manager rapporteert bent. Standaard wordt Service Fabric geladen verdelen op basis van aantal replica's. In het voorgaande voorbeeld, zou dat 10 primaire replica's en 20 secundaire replica's op elk knooppunt in het cluster plaatsen. Dat werkt goed voor de belasting gelijkmatig wordt verdeeld over de partities. Als de belasting wordt nog niet, moet u load melden dat de Resource Manager kunt pack kleinere replica's samen en grotere replica's verbruikt meer geheugen op een afzonderlijke knooppunt.

- Dat de betreffende betrouwbare service de status voor slechts één opslaan in het cluster is. Aangezien u meerdere services in een cluster implementeren kunt, moet u zich ervan bewust van de resources worden dat beide projecten moeten uitvoeren en beheren van de status.

- Dat is niet het cluster zelf groeien of krimpen. Als u meer machines toevoegt, Service Fabric wordt uw partitiereplica's opnieuw indelen voor het gebruik van de extra capaciteit totdat het aantal machines het aantal partities in uw service, overschrijdt omdat een afzonderlijke replica niet meerdere machines omvatten. Daarentegen, als u de grootte van het cluster verminderen door het verwijderen van computers, uw replica's dichter worden verpakt en hebben minder totale capaciteit.

### <a name="how-much-data-can-i-store-in-an-actor"></a>Hoeveel gegevens kan ik opslaan in een actor?

Net als bij betrouwbare services, wordt de hoeveelheid gegevens die u kunt opslaan in een actor-service alleen beperkt door de totale schijfruimte en het geheugen beschikbaar op de knooppunten in uw cluster. Afzonderlijke actors zijn echter meest effectief wanneer deze worden gebruikt om in te kapselen een kleine hoeveelheid status en de bijbehorende bedrijfslogica. Als in het algemeen moet een afzonderlijke actor status die wordt gemeten in kilobytes hebben.

## <a name="other-questions"></a>Andere vragen

### <a name="how-does-service-fabric-relate-to-containers"></a>Hoe Service Fabric in verband met containers?

Containers bieden een eenvoudige manier om pakketservices en de bijbehorende afhankelijkheden, zodat ze consistent in alle omgevingen worden uitgevoerd en kunnen worden uitgevoerd in een geïsoleerde manier worden geactiveerd op een enkele computer. Service Fabric biedt een manier om te implementeren en beheren van services, waaronder [services die zijn ingepakt in een container](service-fabric-containers-overview.md).

### <a name="are-you-planning-to-open-source-service-fabric"></a>Bent u van plan zijn open source Service fabric?

We hebben open-source-onderdelen van Service Fabric ([reliable services-framework](https://github.com/Azure/service-fabric-services-and-actors-dotnet), [betrouwbare actoren framework](https://github.com/Azure/service-fabric-services-and-actors-dotnet), [ASP.NET Core-integratie-bibliotheken](https://github.com/Azure/service-fabric-aspnetcore), [ Service Fabric Explorer](https://github.com/Azure/service-fabric-explorer), en [Service Fabric-CLI](https://github.com/Azure/service-fabric-cli)) op GitHub en bijdragen aan projecten van de community accepteren. 

We [onlangs aangekondigd](https://blogs.msdn.microsoft.com/azureservicefabric/2018/03/14/service-fabric-is-going-open-source/) dat wij van plan te open-source de Service Fabric-runtime bent. Op dit moment hebben we de [Service Fabric-opslagplaats](https://github.com/Microsoft/service-fabric/) maximaal op GitHub met Linux bouwen en testen van hulpprogramma's, wat betekent dat u kunt kloon de opslagplaats, het bouwen van Service Fabric voor Linux, eenvoudige tests uitvoeren, problemen melden en pull-aanvragen indienen. We werken hard aan de Windows-opbouwomgeving de migratie gespreid over, samen met een volledige CI-omgeving.

Ga als volgt de [Service Fabric-blog](https://blogs.msdn.microsoft.com/azureservicefabric/) voor meer informatie, zoals ze zijn aangekondigd.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [core-Service Fabric-concepten](service-fabric-technical-overview.md) en [aanbevolen procedures](service-fabric-best-practices-overview.md)
