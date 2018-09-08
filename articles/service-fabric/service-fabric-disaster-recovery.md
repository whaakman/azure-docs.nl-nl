---
title: Azure Service Fabric-noodherstel | Microsoft Docs
description: Azure Service Fabric biedt de mogelijkheden die nodig zijn om op te lossen met alle soorten rampen. Dit artikel wordt beschreven welke typen rampen die zich kunnen voordoen en hoe u omgaat met hen.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 4b13d2d277721d37a6b96f6640377c875f0b5c0f
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161574"
---
# <a name="disaster-recovery-in-azure-service-fabric"></a>Herstel na noodgevallen in Azure Service Fabric
Een belangrijk onderdeel van het leveren van hoge beschikbaarheid is ervoor te zorgen dat services verschillende typen fouten meer storingen kunnen doorstaan. Dit is vooral belangrijk voor fouten die niet-geplande zijn en buiten het besturingselement. Dit artikel worden enkele algemene foutmodi die mogelijk rampen als dat niet gemodelleerd en goed beheerd. Zij bespreken ook oplossingen en acties moet uitvoeren als er een ramp toch opgetreden. Het doel is om te beperken of de kans op downtime of gegevensverlies voorkomen wanneer ze zich voordoen met fouten, gepland of anders optreden.

## <a name="avoiding-disaster"></a>Noodherstel voorkomen
Het voornaamste doel van de service Fabric is kunt u zowel uw omgeving en uw services zodanig dat algemene fout typy nejsou rampen model. 

In het algemeen zijn er twee soorten na noodgevallen/foutscenario's:

1. Hardware of software-fouten
2. Operationele fouten

### <a name="hardware-and-software-faults"></a>Hardware- en fouten
Hardware- en fouten worden onvoorspelbaar. De eenvoudigste manier om het overbruggen van fouten wordt meer exemplaren van de service liep over de grenzen van de fout hardware of software uitgevoerd. Bijvoorbeeld, als uw service alleen op een bepaalde machine wordt uitgevoerd, is klikt u vervolgens het uitvallen van die één machine na een noodgeval voor die service. Er is een eenvoudige manier om te voorkomen dat deze na noodgevallen om ervoor te zorgen dat de service daadwerkelijk wordt uitgevoerd op meerdere machines. Testen is ook nodig om te controleren of dat de service niet worden onderbroken door het uitvallen van één machine. Plannen van capaciteit zorgt ervoor dat het exemplaar van een vervanging kan worden gemaakt ergens anders en dat de resterende services niet overbelast vermindering van de capaciteit raakt. Hetzelfde patroon werkt onafhankelijk van wat u probeert om te voorkomen dat het uitvallen van. Bijvoorbeeld. Als u zich zorgen maakt over de fout van een SAN, wordt u in meerdere SAN's uitvoeren. Als u zich zorgen maakt over het verlies van een rek met servers, voert u over meerdere rekken. Bent u bang over het verlies van datacenters, wordt uw service moet uitvoeren op meerdere Azure-regio's of datacenters. 

Wanneer in dit type modus uitgevoerd, bent u nog steeds afhankelijk van bepaalde typen gelijktijdige fouten, maar één en zelfs meerdere fouten van een bepaald type (bijvoorbeeld: een enkele virtuele machine of netwerk koppeling mislukt) automatisch worden verwerkt (en dus niet langer een 'noodherstel'). Service Fabric biedt veel mechanismen voor het uitbreiden van het cluster en verwerkt waardoor is mislukt voor knooppunten en -services weer. Service Fabric kunt ook veel exemplaren van uw services uitgevoerd om te voorkomen dat deze typen niet-geplande storingen uit te schakelen in echte rampen.

Mogelijk zijn er redenen waarom een implementatie die groot genoeg is voor fouten beslaan uitgevoerd niet haalbaar is. Het duurt bijvoorbeeld meer hardwareresources dan u bereid bent te betalen voor ten opzichte van de kans op fouten. Tijdens het afhandelen van gedistribueerde toepassingen, kan het zijn aanvullende communicatie hops of status replicatie voor de geografische afstand oorzaken onaanvaardbare latentie kosten. Waar deze lijn wordt getekend verschilt voor elke toepassing. Voor softwarefouten met name kan de fout worden in de service die u probeert te schalen. In dit geval voorkomen niet meer exemplaren de ramp, omdat de foutomstandigheid worden gecorreleerd voor alle instanties.

### <a name="operational-faults"></a>Operationele fouten
Zelfs als uw service is liep over de hele wereld met veel redundantie, kan deze nog steeds rampzalig zijn gebeurtenissen optreden. Bijvoorbeeld als iemand anders per ongeluk configureert de dns-naam voor de service opnieuw of verwijdert aanschaffen. Een voorbeeld: Stel u moest u een stateful Service Fabric-service en iemand die service per ongeluk hebt verwijderd. Tenzij er een andere beperking, die service en alle van de status had is nu verdwenen. Deze typen operationele rampen vereisen ('Oeps') verschillende oplossingen en stappen voor herstel dan de normale niet-geplande storingen. 

De beste manieren om te voorkomen dat dergelijke operationele fouten zijn aan
1. operationele toegang tot de omgeving beperken
2. strikt gevaarlijke bewerkingen controleren
3. automation leggen, te voorkomen dat handmatige of buiten-band-wijzigingen en bepaalde wijzigingen voor de werkelijke omgeving valideren voordat ze doorvoeren
4. Zorg ervoor dat destructieve operations 'soft'. Voorlopig bewerkingen niet onmiddellijk in werking treedt of binnen een periode ongedaan kunnen worden gemaakt

Service Fabric biedt met sommige methoden om te voorkomen dat operationele fouten, zoals het leveren van [op basis van rollen](service-fabric-cluster-security-roles.md) toegangsbeheer voor bewerkingen voor een cluster. De meeste van deze operationele fouten moet echter organisatie inspanningen en andere systemen. Service Fabric bieden een mechanisme voor functionerende operationele fouten, met name back-up en herstel voor stateful services.

## <a name="managing-failures"></a>Beheren van fouten
Het doel van Service Fabric is bijna altijd automatisch beheer van fouten. Beheer van bepaalde typen fouten moeten services hebben echter aanvullende code. Andere typen fouten moeten _niet_ worden automatisch opgelost veiligheids- en business continuity oorzaken hebben. 

### <a name="handling-single-failures"></a>Enkele fouten afhandelen
Enkele machines kunnen voor veel verschillende redenen mislukken. Sommige van deze zijn oorzaken van de hardware, zoals voedingen en hardwarestoringen netwerken. Er zijn andere fouten in de software. Het gaat hierbij om fouten van het besturingssysteem en de service zelf. Service Fabric detecteert automatisch dit soort fouten, met inbegrip van gevallen waarin de machine geïsoleerd van andere virtuele machines door netwerkproblemen wordt.

Ongeacht het type van de service uitgevoerd één exemplaar resultaten in de downtime voor die service als dat één exemplaar van de code voor een bepaalde reden mislukt. 

Om het verwerken van een storing in één, is het eenvoudigste wat dat u kunt doen om ervoor te zorgen dat uw services worden uitgevoerd op meer dan één knooppunt standaard. Voor stateless services, dit kan worden bereikt door een `InstanceCount` groter is dan 1. Voor stateful services de minimale aanbeveling is altijd een `TargetReplicaSetSize` en `MinReplicaSetSize` van ten minste 3. Meer exemplaren van de servicecode van uw wordt uitgevoerd, zorgt u ervoor dat uw service automatisch een storing in één kan verwerken. 

### <a name="handling-coordinated-failures"></a>Afhandeling van gecoördineerde fouten
Gecoördineerde fouten kunnen gebeuren in een cluster vanwege geplande of niet-geplande infrastructuuruitval en wijzigingen of wijzigingen in de geplande software. Service Fabric-infrastructuur-zones die gecoördineerde fouten als domeinen met fouten optreden in modellen. Gebieden die gecoördineerde softwarewijzigingen ervaren zijn gemodelleerd als Upgrade-domeinen. Meer informatie over fout- en upgradedomeinen is in [dit document](service-fabric-cluster-resource-manager-cluster-description.md) met de beschrijving van clustertopologie en definitie.

Standaard wordt in Service Fabric fout- en upgradedomeinen acht bij het plannen waar uw services moeten worden uitgevoerd. Standaard probeert de Service Fabric om ervoor te zorgen dat uw services worden uitgevoerd in verschillende domeinen met fouten en upgrade-domeinen, zodat als gepland of ongepland wijzigingen optreden uw services beschikbaar blijven. 

Stel dat een rek van machines tegelijkertijd mislukken zorgt ervoor dat uitval van een power-bron. Hiermee schakelt u in een ander voorbeeld van één fout voor een bepaalde service met meerdere exemplaren van de service wordt uitgevoerd het verlies van veel computers in domein-fout veroorzaakt. Dit is de reden voor het beheren van domeinen met fouten essentieel is om ervoor te zorgen dat de hoge beschikbaarheid van uw services. Bij het uitvoeren van Service Fabric in Azure, worden foutdomeinen automatisch beheerd. In andere omgevingen, ze mogelijk niet. Als u uw eigen clusters on-premises bouwt, moet toewijzen en uw fouttolerantie domein lay-out correct te plannen.

Upgradedomeinen zijn nuttig voor het modelleren van gebieden waar software gaat op hetzelfde moment worden bijgewerkt. Als gevolg hiervan, Upgrade-domeinen ook vaak de grenzen te definiëren waar software tijdens geplande upgrades offline wordt gezet. Upgrades van uw services en Service Fabric volgen hetzelfde model. Zie voor meer informatie over rolling upgrades, upgradedomeinen en de Service Fabric-statusmodel die helpt voorkomen dat onbedoelde wijzigingen die invloed hebben op het cluster en wordt uw service, deze documenten:

 - [Upgrade van de toepassing](service-fabric-application-upgrade.md)
 - [Zelfstudie over Upgrade](service-fabric-application-upgrade-tutorial.md)
 - [Service Fabric-statusmodel](service-fabric-health-introduction.md)

U kunt de indeling van uw cluster met behulp van de cluster-kaart die is opgegeven in visualiseren [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md):

<center>
![Knooppunten die zijn verdeeld over foutdomeinen in Service Fabric Explorer][sfx-cluster-map]
</center>

> [!NOTE]
> Gebieden van de fout, rolling upgrades uitvoeren van veel exemplaren van uw servicecode en de status, modelleren regels om te controleren of uw services voor de plaatsing van fout- en upgradedomeinen uitvoeren en alleen ingebouwde statuscontrole zijn **sommige** van de functies van Service Fabric om te voorkomen dat normale operationele problemen en fouten in noodsituaties te schakelen. 
>

### <a name="handling-simultaneous-hardware-or-software-failures"></a>Verwerking van gelijktijdige fouten in de hardware of software
Hoger we hebben het gehad over enkele fouten. Zoals u ziet, zijn gemakkelijk te verwerken voor staatloze en stateful services door te houden van meer exemplaren van de code (en de status) uitvoert via domeinen en upgradedomeinen. Meerdere gelijktijdige willekeurige fouten kunnen ook gebeuren. Dit zijn waarschijnlijk zal leiden tot een daadwerkelijk noodgeval.


### <a name="random-failures-leading-to-service-failures"></a>Willekeurige fouten leiden tot servicefouten
Laten we zeggen dat de service al een `InstanceCount` van 5 en verschillende knooppunten op hetzelfde moment wordt uitgevoerd die alle instanties mislukt. Service Fabric reageert automatisch vervangen door exemplaren te maken op andere knooppunten. Deze blijft vervangingen maken totdat de service terug naar de gewenste exemplaren is. Als een ander voorbeeld: Stel dat er is een staatloze service met een `InstanceCount`van-1, wat betekent dat deze wordt uitgevoerd op alle geldige knooppunten in het cluster. Stel dat sommige van deze exemplaren zijn mislukt. In dit geval merkt Service Fabric de service is niet in de gewenste status en probeert te maken van de exemplaren op de knooppunten waar ze ontbreken. 

Voor stateful services de situatie is afhankelijk van of de service heeft persistente staat of niet. Deze ook afhankelijk van hoe groot aantal replica's die de service heeft en hoeveel is mislukt. Bepalen of een noodsituatie is opgetreden voor een stateful service en drie fasen van het beheer volgt:

1. Bepalen of er quorumverlies of niet is
 - Het quorumverlies van een is telkens wanneer een meerderheid van de replica's van een stateful service niet beschikbaar zijn op hetzelfde moment, met inbegrip van de primaire.
2. Bepalen of de quorumverlies permanent of niet is
 - De meeste van de tijd zijn fouten tijdelijk. Processen opnieuw worden opgestart, knooppunten opnieuw worden opgestart, virtuele machines worden uitgebracht, netwerkpartities herstel knooppuntservice. Fouten zijn echter soms permanent. 
    - Voor services zonder permanente status, een storing van een quorum of meer van de resultaten van de replica's _onmiddellijk_ permanente quorum verloren gaan. Wanneer Service Fabric quorumverlies in een stateful service voor niet-permanente detecteert, het onmiddellijk gaat deze naar stap 3 door op te geven () verlies van gegevens. U doorgaat met de gegevens zinvol verloren gaan is omdat de Service Fabric weet dat er geen punt in het wachten op de replica's terugkomen, omdat, zelfs als ze zijn hersteld ze zou niet leeg zijn.
    - Stateful, permanente services, een storing van een quorum of meer replica's zorgt ervoor dat Service Fabric om te wachten op de replica's keert u terug en herstellen van quorum starten. Dit resulteert in een serviceonderbreking voor elk _schrijft_ naar de betreffende partitie (of 'replicaset') van de service. Leesbewerkingen kunnen echter nog wel worden mogelijk is met verminderde consistentiegarantie. De hoeveelheid tijd die Service Fabric voor het quorum wacht te herstellen is oneindig, omdat u doorgaat een (mogelijke) gegevensverlies en andere risico's heeft. De standaardwaarde overschrijven `QuorumLossWaitDuration` waarde is mogelijk, maar wordt niet aanbevolen. Op dit moment moeten alle pogingen in plaats daarvan worden gemaakt met de omlaag replica's herstellen. Hiervoor moet de knooppunten die niet actief back-up zijn, en ervoor te zorgen dat ze de stations waar ze de status van de lokale permanente opgeslagen kunnen koppelen. Als de quorumverlies wordt veroorzaakt door mislukt, probeert het Service Fabric automatisch opnieuw maken van de processen en opnieuw starten van de replica's in deze. Als dit mislukt, Service Fabric health fouten worden gemeld. Als deze omgezet worden kunnen vervolgens terug de replica's gewoonlijk. Soms echter kunnen niet de replica's worden teruggehaald. Bijvoorbeeld, de stations kunnen alle zijn mislukt of de machines fysiek vernietigd loopt de toepassing. In dergelijke gevallen hebben we nu een gebeurtenis van permanente quorum verloren gaan. Als u wilt laten weten Service Fabric om te stoppen met het wachten op de omlaag replica's terugkomen, Clusterbeheerder welke partities die worden beïnvloed en aanroepen van services moet bepalen de `Repair-ServiceFabricPartition -PartitionId` of ` System.Fabric.FabricClient.ClusterManagementClient.RecoverPartitionAsync(Guid partitionId)` API.  Deze API kunt u de ID van de partitie te verplaatsen uit QuorumLoss en potentiële dataloss opgeven.

  > [!NOTE]
  > Het is _nooit_ veilig voor het gebruik van deze API anders dan in een bepaalde manier op basis van specifieke partities. 
  >

3. Om te bepalen of er sprake is van feitelijke gegevens verloren gaan en het herstellen van back-ups
  - Wanneer Service Fabric roept de `OnDataLossAsync` methode, is het altijd vanwege _verdacht_ verlies van gegevens. Service Fabric zorgt ervoor dat deze aanroep is geleverd aan de _aanbevolen_ resterende replica. Dit is de replica de voortgang van de meeste heeft gemaakt. De reden we altijd zeggen _verdacht_ verlies van gegevens is mogelijk dat de resterende replica alle dezelfde toestand bevat als de primaire deed toen deze werd afgesloten. Zonder deze staat om te vergelijken, is er echter geen goede manier voor Service Fabric of operators zeker weten. Op dit moment weet Service Fabric ook dat de andere replica's komen niet terug. Dat was een beslissing heeft genomen wanneer we wachten op het quorum verlies van gegevens op te lossen zelf gestopt. De beste loop van de actie voor de service is meestal het blokkeren en wacht tot specifieke interventie van een beheerder. Dus wat doet een typische implementatie van de `OnDataLossAsync` methode doen?
  - Meld u eerst die `OnDataLossAsync` is geactiveerd en uit alle benodigde administratieve waarschuwingen geactiveerd.
   - Meestal op dit punt, onderbreken en wachten op verdere beslissingen en handmatige acties worden ondernomen. Dit komt doordat, zelfs als er back-ups beschikbaar zijn mogelijk moeten worden voorbereid. Bijvoorbeeld, als twee verschillende services gegevens coördineert, deze back-ups moet mogelijk worden gewijzigd om ervoor te zorgen dat zodra de terugzetbewerking dat gebeurt de informatie die twee services care over consistent is. 
  - Vaak is er ook andere telemetrie of uitlaatgas van de service. Deze metagegevens kan worden opgenomen in de andere services of in Logboeken. Deze informatie kan worden gebruikt die nodig zijn om te bepalen of er zijn aanroepen ontvangen en verwerkt op de primaire of zijn niet aanwezig in de back-up worden gerepliceerd naar deze bepaalde replica. Deze moeten worden opnieuw afgespeeld of toegevoegd aan de back-up voordat de herstelbewerking haalbaar is.  
   - Vergelijking van de resterende replica de status die deel uitmaakt van een back-ups die beschikbaar zijn. Als met behulp van de Service Fabric betrouwbare verzamelingen zijn hulpprogramma's en beschikbaar voor in dat geval worden verwerkt, dat wordt beschreven in [in dit artikel](service-fabric-reliable-services-backup-restore.md). Het doel is om te zien als de status in de replica voldoende is of ook wat de back-up mogelijk ontbreekt.
  - Wanneer de vergelijking is klaar en indien nodig het herstel is voltooid, de code van de moet retourneren ' True ' wanneer alle statuswijzigingen zijn aangebracht. Als de replica wordt vastgesteld dat het de beste kopie van de status is en er zijn geen wijzigingen aangebracht, retourneert de waarde false. Waar geeft aan dat elke _andere_ resterende replica's nu mogelijk niet door deze gegevensset. Ze zullen worden verwijderd en opnieuw worden opgebouwd uit deze replica. False geeft aan dat er geen wijzigingen zijn aangebracht, zodat de andere replica's kunnen houden wat zij erover. 

Het is van cruciaal belang dat service auteurs mogelijk gegevens verloren gaan en foutscenario's Oefen voordat services ooit zijn geïmplementeerd in een productieomgeving. Ter bescherming tegen de mogelijkheid om verlies van gegevens, het is belangrijk om periodiek [maakt u een back-up van de status](service-fabric-reliable-services-backup-restore.md) van elk van uw stateful services naar een geografisch redundante opslag. U moet er ook voor zorgen dat u hebt de mogelijkheid om het te herstellen. Sinds de back-ups van veel verschillende services worden uitgevoerd op verschillende tijdstippen, moet u ervoor zorgen dat de uw services na een herstelbewerking een consistente weergave van elkaar hebben. Neem bijvoorbeeld een situatie waarbij één service genereert een getal en slaat deze vervolgens verzonden naar een andere service die ook worden opgeslagen. Nadat deze is teruggezet, kunt u ontdekken dat de tweede service het aantal heeft, maar de eerste kiest, wordt er niet zo is, omdat het back-up hebt opgenomen die voor deze bewerking.

Als u weten dat de resterende replica's niet voldoende om door te gaan zijn uit in een scenario voor het verlies van gegevens en u servicestatus van telemetrie- of uitlaatgas niet kan reconstrueren, bepaalt de frequentie van uw back-ups de best mogelijke beoogd herstelpunt (RPO). Service Fabric biedt veel hulpprogramma's voor het testen van verschillende foutscenario's, met inbegrip van permanente quorum en verlies van gegevens hersteld op basis van een back-up vereisen. Deze scenario's worden opgenomen als een onderdeel van de Service-Fabric testbaarheid's, beheerd door de Fault Analysis Service. Vindt u meer informatie over deze hulpprogramma's en patronen [hier](service-fabric-testability-overview.md). 

> [!NOTE]
> Systeemservices kunnen ook leiden tot slechtere quorumverlies, met het effect dat specifiek is voor de betreffende service. Bijvoorbeeld, quorumverlies in de naming-service heeft gevolgen voor naamomzetting, terwijl quorumverlies in de failover manager-service worden geblokkeerd voor het maken van nieuwe services en failovers. Terwijl de Service Fabric-systeemservices hetzelfde patroon als voor uw services voor het beheer van de staat volgen, wordt niet aanbevolen dat u dat deze buiten het Quorum verloren gaan en in het verlies van gegevens moet proberen. De aanbeveling is in plaats daarvan [zoeken naar ondersteuning](service-fabric-support.md) om te bepalen van een oplossing die is gericht op uw specifieke situatie.  Meestal is het raadzaam om te wachten tot de omlaag replica's weer.
>

## <a name="availability-of-the-service-fabric-cluster"></a>Beschikbaarheid van de Service Fabric-cluster
In het algemeen is het Service Fabric-cluster zelf een hoge mate gedistribueerde omgeving met geen single point of failure. Een storing van een willekeurig knooppunt niet, zullen de beschikbaarheid of betrouwbaarheidsproblemen met voor het cluster, vooral omdat de Service Fabric-systeemservices dezelfde richtlijnen die eerder is verkregen volgen: ze worden altijd uitgevoerd met drie of meer replica's standaard, en deze systeem Services die staatloos zijn worden uitgevoerd op alle knooppunten. De onderliggende Service Fabric-netwerken en mislukte detectie lagen worden volledig gedistribueerd. De meeste systeemservices kunnen opnieuw worden gemaakt van metagegevens in het cluster of weet hoe opnieuw synchroniseren van de status vanuit andere plekken. De beschikbaarheid van het cluster kan worden aangetast als systeemservices quorum verliezen situaties zoals die wordt beschreven bovenstaande krijgen. In dergelijke gevallen niet mogelijk bepaalde bewerkingen op het cluster, zoals een upgrade wordt gestart of implementeren van nieuwe services kan uitvoeren, maar het cluster zelf nog steeds actief is. Services voor het uitvoeren van al blijft actief zijn in deze voorwaarden, tenzij ze nodig schrijfbewerkingen naar de systeemservices hebben blijven werken. Bijvoorbeeld, als de Failover Manager sprake van quorumverlies is alle services worden uitgevoerd, maar alle services die niet voldoen aan zich niet automatisch opnieuw wordt gestart, omdat dit de betrokkenheid van de Failover Manager vereist. 

### <a name="failures-of-a-datacenter-or-azure-region"></a>Fouten van een datacenter of een Azure-regio
In zeldzame gevallen kan het worden van een fysieke Datacenter tijdelijk niet beschikbaar vanwege verlies van kracht of op de netwerkverbinding. In deze gevallen wordt is uw Service Fabric-clusters en services in die datacenter of een Azure-regio niet beschikbaar. Echter, _uw gegevens blijven behouden_. Voor clusters die worden uitgevoerd in Azure, kunt u updates op uitval weergeven op de [pagina Azure status][azure-status-dashboard]. In de zeer onwaarschijnlijke geval dat een fysieke datacenter is gedeeltelijk of volledig worden verwijderd, een Service Fabric-clusters die worden gehost er of de services in deze verloren kunnen gaan. Dit omvat een staat geen back-ups van buiten die datacenter of regio.

Er is twee verschillende strategieën voor functionerende van de fout permanent of langdurig uitvoeren van een enkel datacenter of regio. 

1. Afzonderlijke Service Fabric-clusters worden uitgevoerd in meerdere dergelijke regio's en gebruikmaken van een mechanisme voor failover en failback tussen deze omgevingen. Dit soort meerdere actief / actief of actief-passief clustermodel vereist aanvullende code voor beheer en bewerkingen. Dit is ook vereist coördinatie van de back-ups van de services in een datacenter of regio zodat deze beschikbaar zijn in andere datacenters of regio's wanneer een mislukt. 
2. Een Service Fabric-cluster worden uitgevoerd omvat die meerdere datacenters of regio's. De minimale ondersteunde configuratie voor deze is drie datacenters of regio's. Het aanbevolen aantal regio's of datacenters is vijf. Hiervoor wordt een meer complexe clustertopologie. Het voordeel van dit model is echter dat uitval van één datacenter of regio na een noodgeval is geconverteerd naar een normale fout. Deze fouten kunnen worden verwerkt door de mechanismen die werken voor clusters in een enkele regio. Controleer of werkbelastingen worden gedistribueerd, zodat ze normaal fouten tolereren foutdomeinen en upgradedomeinen regels voor de plaatsing van Service Fabric. Lees voor meer informatie over het beleid waarmee services in dit type cluster werken van op [beleid voor serviceplaatsing](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)

### <a name="random-failures-leading-to-cluster-failures"></a>Willekeurige fouten leiden tot fouten van cluster
Service Fabric is het concept van Seed-knooppunten. Dit zijn de knooppunten die de beschikbaarheid van het onderliggende cluster. Deze knooppunten ervoor zorgen dat het cluster blijft van tot stand brengen van leases met andere knooppunten en fungeren als tiebreakers tijdens bepaalde soorten netwerkfouten. Als willekeurig mislukken een meerderheid van de seed-knooppunten in het cluster verwijderen en ze niet terug worden gebracht, wordt het cluster automatisch uitgeschakeld. In Azure, Seed-knooppunten automatisch worden beheerd: ze zijn verdeeld over de beschikbare domeinen en upgradedomeinen, en als een enkel seed-knooppunt wordt verwijderd uit het cluster een andere naam in plaats daarvan wordt gemaakt. 

In zowel zelfstandige Service Fabric-clusters en Azure is 'Primaire knooppunttype' degene die de seeds wordt uitgevoerd. Bij het definiëren van een primaire knooppunttype, wordt Service Fabric automatisch profiteren van het aantal knooppunten dat is geleverd door het maken van maximaal 9 seed-knooppunten en 9 replica's van elk van de systeemservices. Als een set van willekeurige fouten van het grootste deel van deze system service-replica's tegelijk, voer de systeemservices quorumverlies, zoals we hierboven is beschreven. Als een meerderheid van de seed-knooppunten kwijtgeraakt zijn, het cluster snel na afgesloten.

## <a name="next-steps"></a>Volgende stappen
- Leer hoe u verschillende storingen simuleren met de [testbaarheid framework](service-fabric-testability-overview.md)
- Lezen van andere bronnen voor herstel na noodgevallen en hoge beschikbaarheid. Microsoft heeft een grote hoeveelheid informatie over deze onderwerpen worden gepubliceerd. Hoewel sommige van deze documenten naar specifieke technieken voor gebruik in andere producten verwijzen, bevatten ze veel algemene aanbevolen procedures die u in de context van Service Fabric ook toepassen kunt:
  - [Beschikbaarheidscontrolelijst](../best-practices-availability-checklist.md)
  - [Een Dr-herstelanalyse uitvoeren](../sql-database/sql-database-disaster-recovery-drills.md)
  - [Herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen][dr-ha-guide]
- Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)

<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
