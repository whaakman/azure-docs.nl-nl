---
title: Azure Service Fabric-noodherstel | Microsoft Docs
description: Azure Service Fabric biedt de mogelijkheden die nodig zijn om te gaan met alle soorten noodsituaties. Dit artikel wordt beschreven welke soorten noodsituaties die zich kunnen voordoen en hoe u omgaat met hen.
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
ms.openlocfilehash: 295772b70529f79c7a4c135d8ea7c12a1c661fe6
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="disaster-recovery-in-azure-service-fabric"></a>Herstel na noodgevallen in Azure Service Fabric
Een belangrijk onderdeel van het leveren van hoge beschikbaarheid is ervoor te zorgen dat alle soorten fouten meer services kunnen doorstaan. Dit is vooral belangrijk voor fouten die niet-geplande zijn en buiten uw beheer. Dit artikel worden enkele algemene fout modi die mogelijk noodsituaties als dat niet het gemodelleerd en correct worden beheerd. Bespreek ook oplossingen en acties moet uitvoeren als er een ramp toch opgetreden. Het doel is te beperken of voorkomen van uitvaltijd of gegevensverlies wanneer ze, fouten optreden, gepland of anders optreden.

## <a name="avoiding-disaster"></a>Noodherstel voorkomen
Het voornaamste doel van de service Fabric is om te helpen u zowel uw omgeving en uw services zodanig dat algemene fout typen niet noodsituaties zijn model. 

In het algemeen zijn er twee soorten na noodgevallen/mislukte scenario's:

1. Hardware of software-fouten
2. Operationele fouten

### <a name="hardware-and-software-faults"></a>Hardware en software-fouten
Hardware en software fouten zijn onvoorspelbaar. De eenvoudigste manier om te blijven functioneren fouten kan meer exemplaren van de service omspannen buiten de grenzen van de fout hardware of software wordt uitgevoerd. Bijvoorbeeld, als uw service wordt uitgevoerd op een bepaalde computer, is mislukt of er één machine een noodgeval voor de service. Er is een eenvoudige manier om te voorkomen dat deze na noodgevallen om ervoor te zorgen dat de service daadwerkelijk wordt uitgevoerd op meerdere machines. Testen is ook nodig om te controleren of dat het uitvallen van één machine de service niet wordt verstoord. Plannen van capaciteit zorgt ervoor dat het exemplaar van een vervanging kan worden gemaakt elders en dat vermindering van de capaciteit van de resterende services niet overbelasting. Hetzelfde patroon werkt ongeacht wat u probeert om te voorkomen dat het uitvallen van. Bijvoorbeeld. Als u zich zorgen over het mislukken van een SAN maakt, voert u via meerdere SAN's. Als u zich zorgen over het verlies van een rek met servers maakt, voert u via meerdere rekken. Als u bang over het verlies van datacenters, moet uw service worden uitgevoerd op meerdere Azure-regio's of datacenters. 

Wanneer in deze modus wordt uitgevoerd, u kunt nog steeds onderworpen aan bepaalde typen gelijktijdige fouten, maar één en zelfs meerdere fouten van een bepaald type (bijvoorbeeld: een enkele virtuele machine of netwerk koppeling mislukken) automatisch worden verwerkt (en dus niet langer een 'noodgeval'). Service Fabric bevat veel mechanismen voor het uitbreiden van het cluster en ingangen meebrengen mislukte knooppunten en services weer. Service Fabric kunnen ook veel exemplaren van uw services en waarop om te voorkomen dat deze niet-geplande storingen uit te schakelen in echte noodsituaties typen.

Mogelijk zijn er redenen waarom een implementatie die groot genoeg zijn om fouten beslaan uitgevoerd, niet haalbaar is. Bijvoorbeeld, duurt het hardwarebronnen meer dan u bereid bent te betalen voor ten opzichte van de kans op mislukte bent. Tijdens het afhandelen van gedistribueerde toepassingen, kan het zijn dat aanvullende communicatie hops of status replicatie over geografische afstanden oorzaken onaanvaardbaar latentie kost. Waar deze regel wordt getekend verschilt voor elke toepassing. Softwarefouten in het bijzonder kan de fout zich in de service die u probeert te schalen. In dit geval voorkomen niet meer exemplaren de noodherstel, omdat de voorwaarde is mislukt, wordt gecorreleerd in alle exemplaren.

### <a name="operational-faults"></a>Operationele fouten
Zelfs als de service overal ter wereld met veel redundantie omspannen is, kan deze nog steeds rampzalig zijn gebeurtenissen optreden. Bijvoorbeeld als iemand per ongeluk configureert de DNS-naam voor de service opnieuw of deze rechtstreekse wordt verwijderd. Stel bijvoorbeeld moest u een stateful Service Fabric-service en iemand die service per ongeluk hebt verwijderd. Tenzij er een aantal andere risicobeperking, of de service en alle van de status had is nu voltooid. Dit type operationele noodsituaties vereist ('Oeps') verschillende oplossingen en stappen voor herstel dan gewone niet-geplande storingen. 

De beste manieren om te voorkomen dat deze typen operationele storingen zijn aan
1. operationele toegang tot de omgeving beperken
2. strikt gevaarlijke bewerkingen controleren
3. automation leggen, te voorkomen dat handmatige of buiten band wijzigingen en specifieke wijzigingen tegen de werkelijke omgeving valideren voordat ze vast te stellen
4. Zorg ervoor dat destructieve operations 'soft'. Voorlopig bewerkingen onmiddellijk van kracht niet of kunnen in sommige tijdvenster ongedaan worden gemaakt

Service Fabric bevat enkele mechanismen om te voorkomen dat operationele fouten, zoals bieden [op basis van rollen](service-fabric-cluster-security-roles.md) toegangsbeheer voor bewerkingen voor een cluster. De meeste van deze operationele storingen vereisen echter organisatie inspanningen en andere systemen. Service Fabric bieden een mechanisme voor functionerende operationele fouten, met name back-up en herstel voor stateful services.

## <a name="managing-failures"></a>Het beheren van fouten
Het doel van de Service Fabric is bijna altijd automatisch beheer van fouten. Verwerken van bepaalde typen fouten een services echter aanvullende code. Andere typen van fouten moeten _niet_ worden automatisch opgelost redenen veiligheid en zakelijke continuïteit. 

### <a name="handling-single-failures"></a>Afhandeling van één fouten
Meerdere computers kunnen om allerlei redenen mislukken. Sommige van deze zijn oorzaken van hardware, zoals voedingen en netwerken hardwarefouten. Andere fouten zijn in software. Het gaat hierbij om fouten van het besturingssysteem en de service zelf. Service Fabric detecteert automatisch dergelijke fouten, met inbegrip van gevallen waarin de machine geïsoleerd van andere machines vanwege netwerkproblemen wordt.

Ongeacht het type van de service uitgevoerd één exemplaar resultaten uitvaltijd voor de service als dat één exemplaar van de code voor een of andere reden mislukt. 

Om te kunnen verwerken op één enkele storing, is het eenvoudigste wat dat u kunt doen om ervoor te zorgen dat uw services worden uitgevoerd op meer dan één knooppunt standaard. Voor stateless services, kan dit worden bereikt door een `InstanceCount` groter is dan 1. Voor stateful services, is de minimale aanbeveling altijd een `TargetReplicaSetSize` en `MinReplicaSetSize` van ten minste 3. Meer kopieën van uw servicecode wordt uitgevoerd, zorgt u ervoor dat uw service automatisch één enkele storing kan verwerken. 

### <a name="handling-coordinated-failures"></a>Verwerking gecoördineerd fouten
Gecoördineerde fouten kunnen gebeuren in een cluster vanwege geplande of niet-geplande infrastructuur fouten en wijzigingen of wijzigingen in de geplande software. Service Fabric modellen infrastructuur-zones die gecoördineerde fouten als domeinen met fouten optreden. Gebieden die gecoördineerde softwarewijzigingen merken zijn gemodelleerd als domeinen upgraden. Meer informatie over probleem- en upgrade domeinen zich in [dit document](service-fabric-cluster-resource-manager-cluster-description.md) die beschrijft clustertopologie en definitie.

Standaard beschouwt Service Fabric-fout- en upgrade-domeinen bij het plannen waar uw services moeten worden uitgevoerd. Standaard probeert de Service Fabric om ervoor te zorgen dat uw services worden uitgevoerd in verschillende domeinen voor probleem- en dus als gepland of ongepland wijzigingen per ongeluk uw services beschikbaar blijven. 

Stel dat falen van een stroombron zorgt ervoor dat een rack van machines tegelijkertijd mislukken. Hiermee schakelt u in een ander voorbeeld van een storing voor een bepaalde service met meerdere exemplaren van de service wordt uitgevoerd het verlies van veel computers in een domein-fout veroorzaakt. Dit is het beheren van domeinen met fouten essentieel is voor hoge beschikbaarheid van uw services. Wanneer het Service Fabric in Azure wordt uitgevoerd, worden foutdomeinen automatisch beheerd. In andere omgevingen ze mogelijk niet. Als u uw eigen clusters on-premises maakt, moet u toewijzen en uw domein veroorzaakt indeling onjuist plant.

Upgradedomeinen zijn nuttig voor het modelleren van gebieden waarop software is vereist, op hetzelfde moment worden bijgewerkt. Als gevolg hiervan domeinen upgraden ook vaak de grenzen te definiëren waar software wordt offline gezet tijdens geplande upgrades. Upgrades van zowel Service Fabric en uw services volgen hetzelfde model. Zie voor meer informatie over upgrades, upgradedomeinen en het Service Fabric-statusmodel die voorkomt dat onbedoelde wijzigingen die invloed hebben op het cluster en uw service rolling deze documenten:

 - [Upgrade van de toepassing](service-fabric-application-upgrade.md)
 - [Zelfstudie over Upgrade](service-fabric-application-upgrade-tutorial.md)
 - [Service Fabric-statusmodel](service-fabric-health-introduction.md)

U kunt de indeling van het cluster met behulp van de opgegeven in de clustertoewijzing visualiseren [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md):

<center>
![Knooppunten die zijn verdeeld over de domeinen met fouten in Service Fabric Explorer][sfx-cluster-map]
</center>

> [!NOTE]
> Modeling gebieden mislukt, rolling upgrades met veel exemplaren van uw servicecode en status, plaatsingsregels om te controleren of uw services uitvoeren in domeinen met fouten en upgrade, en de ingebouwde statuscontrole NET **sommige** van de functies die Service Fabric bevat om de normale operationele problemen en fouten voorkomen in te schakelen in noodsituaties. 
>

### <a name="handling-simultaneous-hardware-or-software-failures"></a>Verwerking van gelijktijdige hardware of software-fouten
Hierboven besproken we enkele fouten. Zoals u ziet, zijn gemakkelijk te verwerken voor zowel stateless als stateful services alleen doordat meer exemplaren van de code (en de status) uitvoert via domeinen met fouten en upgradedomeinen. Meerdere gelijktijdige willekeurige fouten kunnen ook gebeuren. Dit zijn vaker leiden tot een werkelijke na noodgevallen.


### <a name="random-failures-leading-to-service-failures"></a>Willekeurige fouten, wat leidt tot servicefouten
Stel dat de service heeft een `InstanceCount` van 5 en verschillende knooppunten op hetzelfde moment actieve die alle exemplaren mislukt. Service Fabric reageert automatisch vervangen door exemplaren te maken op andere knooppunten. Vervangingen maken totdat de service terug naar het aantal gewenste exemplaren is wordt voortgezet. Een ander voorbeeld: Stel dat er is een staatloze service met een `InstanceCount`van -1, wat betekent dat deze wordt uitgevoerd op alle geldige knooppunten in het cluster. Stel dat een aantal van deze exemplaren waren mislukken. In dit geval merkt Service Fabric dat de service niet in de gewenste status is en probeert te maken van de exemplaren op de knooppunten waar ze ontbreekt zijn. 

Voor stateful services de situatie is afhankelijk van of de service heeft gehandhaafd status of niet. Ook afhankelijk van hoe groot aantal replica's die de service heeft en hoeveel is mislukt. Bepalen of een noodgeval is opgetreden voor een stateful service bevat en het beheren van deze drie fasen:

1. Bepalen of er quorumverlies of niet is
 - Een quorumverlies is elk gewenst moment een meerderheid van de replica's van een stateful service niet beschikbaar zijn op hetzelfde moment, met inbegrip van de primaire.
2. Bepalen of de quorumverlies permanente of niet is
 - De meeste gevallen, fouten, zijn tijdelijke. Processen opnieuw zijn opgestart, opnieuw worden opgestart, virtuele machines zijn uitgebracht, retoucheren netwerkpartities. Fouten zijn soms echter permanent. 
    - Voor services zonder permanente status, een storing van een quorum of meer van de resultaten van de replica's _onmiddellijk_ permanente quorum verloren gaan. Wanneer het Service Fabric quorumverlies in een stateful service voor niet-permanente detecteert, loopt onmiddellijk hij naar stap 3 (mogelijke) dataloss declareert. U overgaat tot dataloss zin omdat Service Fabric weet dat er geen punt in het wachten op de replica's terugkeren, omdat zelfs als ze zijn hersteld, deze leeg zijn zou.
    - Een storing van een quorum of meer van de replica's voor permanente stateful services zorgt ervoor dat Service Fabric om te wachten op de replica's keert u terug en herstellen van quorum starten. Dit resulteert in een onderbreking van deze service voor een _schrijft_ naar de betreffende partitie (of 'replicaset') van de service. Leesbewerkingen kunnen echter nog wel worden mogelijk met verminderde consistentie wordt gegarandeerd. De hoeveelheid tijd dat het Service Fabric wacht quorum worden hersteld is oneindig, omdat u doorgaat een (mogelijke) dataloss gebeurtenis en andere risico's uitvoert. De standaardwaarde onderdrukken `QuorumLossWaitDuration` waarde is mogelijk, maar wordt niet aanbevolen. Op dit moment moeten alle pogingen in plaats daarvan worden aangebracht om te herstellen van de omlaag replica's. Hiervoor moet de knooppunten die niet beschikbaar back-up zijn, en ervoor te zorgen dat ze de stations waar ze de lokale permanente status opgeslagen opnieuw kunnen koppelen. Als de quorumverlies wordt veroorzaakt door een fout, probeert het Service Fabric automatisch opnieuw maken van de processen en opnieuw starten van de replica's in deze. Als dit mislukt, wordt in Service Fabric health fouten rapporteert. Als deze omgezet worden kunnen vervolgens terugkeren de replica's gewoonlijk. Soms moet echter niet kunnen de replica's worden teruggebracht. Bijvoorbeeld, de stations mogelijk alle is mislukt of de machines fysiek vernietigd enigszins. In dergelijke gevallen hebben we nu een permanente quorum verlies-gebeurtenis. Om te laten Service Fabric om te wachten op de omlaag replica's terugkeren stoppen, Clusterbeheerder welke partities die services worden beïnvloed en roept u moet bepalen de `Repair-ServiceFabricPartition -PartitionId` of ` System.Fabric.FabricClient.ClusterManagementClient.RecoverPartitionAsync(Guid partitionId)` API.  Deze API kunt u opgeven die de ID van de partitie worden verplaatst buiten QuorumLoss en potentiële dataloss.

> [!NOTE]
> Het is _nooit_ veilig voor het gebruik van deze API anders dan in een bepaalde manier tegen specifieke partities. 
>

3. Bepalen of er sprake is van feitelijke gegevens verloren gaan en het terugzetten van back-ups
  - Wanneer het Service Fabric roept de `OnDataLossAsync` methode altijd omdat is _vermoed_ dataloss. Service Fabric zorgt ervoor dat deze aanroep is geleverd aan de _aanbevolen_ resterende replica. Dit is de meeste voortgang heeft gemaakt, worden de replica. De reden we altijd spreken _vermoed_ dataloss is dat het is mogelijk dat de resterende replica alle dezelfde staat daadwerkelijk heeft als de primaire was voordat deze werd afgesloten. Echter, zonder die status om te vergelijken, er is geen goede manier voor Service Fabric of operators zeker weten. Op dit moment kent Service Fabric ook dat de andere replica's zijn niet terugkomen. Dat was de beslissing bij het wachten op de quorumverlies om op te lossen zelf is gestopt. De beste loop van de actie voor de service is meestal om te blokkeren en wacht tot specifieke interventie van een beheerder. Dus de betekenis van een typische implementatie van de `OnDataLossAsync` methode doen?
  - Meld u eerst die `OnDataLossAsync` is geactiveerd en eventuele benodigde beheerdersrechten waarschuwingen starten.
   - Meestal op dit punt wacht op invoer voor verdere beslissingen en handmatige acties worden ondernomen. Dit is omdat zelfs als back-ups beschikbaar zijn ze mogelijk moeten worden voorbereid. Bijvoorbeeld, als twee verschillende services informatie te coördineren, deze back-ups mogelijk moet worden gewijzigd om ervoor te zorgen dat nadat het herstel die gebeurt de informatie die twee services voorzichtig over consistent is. 
  - Vaak is ook andere telemetrie of uitlaatgas van de service. Deze metagegevens kan zijn opgenomen in de andere services of in Logboeken. Deze informatie kan worden gebruikt die nodig zijn om te bepalen of er zijn aanroepen ontvangen en verwerkt op de primaire die zijn niet aanwezig in de back-up of gerepliceerd naar deze bepaalde replica. Deze moeten worden cookies of toegevoegd aan de back-up voordat de herstelbewerking haalbaar is.  
   - Vergelijking van de status van de resterende replica in die deel uitmaakt van een back-ups die beschikbaar zijn. Als de betrouwbare Service Fabric-verzamelingen worden er hulpprogramma's voor databaseontwikkeling en beschikbaar verwerkt om te doen, dat wordt beschreven in [in dit artikel](service-fabric-reliable-services-backup-restore.md). Het doel is om te zien als de status in de replica voldoende is of ook wat de back-up ontbreekt mogelijk.
  - Als de vergelijking is voltooid, en als het nodig de herstelbewerking is voltooid, de code als resultaat moet true als er wijzigingen zijn aangebracht. Retourneert onwaar als de replica wordt vastgesteld dat het de beste kopie van de status is en er zijn geen wijzigingen hebt aangebracht. Waar geeft aan dat elke _andere_ resterende replica's nu mogelijk niet door deze gegevensset. Ze zullen worden verwijderd en opnieuw opgebouwd uit deze replica. Drempelwaarde.ONWAAR geeft aan dat er geen wijzigingen zijn aangebracht, zodat de andere replica's kunnen houden wat ze hebben. 

Het is zeer belangrijk dat service auteurs potentiële dataloss en scenario's fouten oefenen voordat services ooit worden geïmplementeerd in de productieomgeving. Ter bescherming tegen de mogelijkheid van dataloss, is het belangrijk om periodiek [back-up van de status van de](service-fabric-reliable-services-backup-restore.md) van elk van uw stateful services naar een geografisch redundante opslag. U moet er ook voor zorgen dat u hebt de mogelijkheid om het te herstellen. Sinds de back-ups van veel verschillende services worden uitgevoerd op verschillende tijdstippen, moet u ervoor zorgen dat de uw services na een herstelbewerking een consistente weergave van elkaar hebben. Neem bijvoorbeeld een situatie waarin een service een aantal genereert en opgeslagen en vervolgens verzendt het naar een andere service waarmee gegevens worden ook opgeslagen. Na een herstelbewerking mogelijk ontdekt u dat de tweede-service het aantal heeft opnemen, maar het eerste bestaat niet, omdat het back-up niet die voor deze bewerking.

Als u ontdekken dat de resterende replica's onvoldoende om door te gaan zijn uit in een scenario dataloss en u kan geen servicestatus van telemetrie of uitlaatgas Reconstrueer, bepaalt de frequentie van uw back-ups de best mogelijke beoogd herstelpunt (RPO). Service Fabric bevat veel hulpprogramma's voor het testen van verschillende scenario's voor mislukt, met inbegrip van permanente quorum en dataloss herstel vanaf een back-up vereisen. Deze scenario's zijn opgenomen als onderdeel van de Service Fabric testbaarheid-hulpprogramma's worden beheerd door de fout Analysis Services. Meer informatie over deze hulpprogramma's en patronen [hier](service-fabric-testability-overview.md). 

> [!NOTE]
> Systeemservices kunnen ook quorum verloren zijn gegaan, met de impact die specifiek zijn voor de betrokken service worden afnemen. Bijvoorbeeld: quorumverlies in de naamgevingsservice heeft gevolgen voor naamomzetting dat quorumverlies in de failover manager service maken van nieuwe services en failovers geblokkeerd. Terwijl de Service Fabric-systeemservices hetzelfde patroon als uw services voor het statusbeheer van volgen, wordt niet aanbevolen dat u proberen moet te verplaatsen uit Quorumverlies en in het potentieel dataloss. De aanbeveling is in plaats daarvan het [ondersteuning zoeken](service-fabric-support.md) om te bepalen van een oplossing die bestemd is voor uw specifieke situatie.  Meestal is het raadzaam om te wachten tot de omlaag replica's weer.
>

## <a name="availability-of-the-service-fabric-cluster"></a>Beschikbaarheid van het Service Fabric-cluster
Het Service Fabric-cluster zelf is over het algemeen een sterk gedistribueerde omgeving met geen enkele storingspunten. Een storing van een willekeurig knooppunt leidt niet tot de beschikbaarheid of betrouwbaarheidsproblemen voor het cluster, voornamelijk omdat de Service Fabric-systeemservices Volg dezelfde richtlijnen die eerder is verkregen: ze worden altijd uitgevoerd met drie of meer replica's standaard en deze systeem Services die staatloze zijn worden uitgevoerd op alle knooppunten. De onderliggende Service Fabric netwerk- en detectie lagen worden volledig gedistribueerd. De meeste systeemservices van metagegevens in het cluster kunnen worden gemaakt of weten hoe opnieuw synchroniseren van hun status vanuit andere locaties. De beschikbaarheid van het cluster kan niet meer betrouwbaar als systeemservices quorum verlies situaties zoals deze beschreven bovenstaande krijgen. In dergelijke gevallen niet mogelijk bepaalde bewerkingen op het cluster zoals begint met een upgrade of implementeren van nieuwe services kan uitvoeren, maar het cluster zelf nog steeds actief is. Services op het al wordt uitgevoerd blijft actief in deze voorwaarden, tenzij ze nodig schrijfbewerkingen naar de systeemservices hebben blijft werken. Bijvoorbeeld, als de Failover Manager is sprake van quorumverlies alle services worden uitgevoerd, maar kunnen de services die niet kan niet worden automatisch opnieuw wordt gestart, omdat dit de betrokkenheid van de Failover Manager vereist. 

### <a name="failures-of-a-datacenter-or-azure-region"></a>Fouten van een datacenter of een Azure-regio
In zeldzame gevallen zijn een fysieke Datacenter tijdelijk niet beschikbaar vanwege verlies van power of netwerkverbinding. In deze gevallen is is de Service Fabric-clusters en -services in het desbetreffende datacenter of een Azure-regio niet beschikbaar. Echter, _uw gegevens behouden blijven_. Voor clusters worden uitgevoerd in Azure, kunt u updates op storingen bekijken op de [pagina Azure status][azure-status-dashboard]. In de gebeurtenis zeer onwaarschijnlijk dat een fysieke Datacenter wordt volledig of gedeeltelijk vernietigd, alle Service Fabric-clusters die er worden gehost of de services in deze verloren kunnen gaan. Dit omvat geen status die niet een back-up buiten het datacenter of de regio.

Er is twee verschillende strategieën voor functionerende permanente of volgehouden storing in een enkel datacenter of regio. 

1. Afzonderlijke Service Fabric-clusters worden uitgevoerd in meerdere dergelijke regio's en gebruikmaken van een mechanisme voor failover en failback tussen deze omgevingen. De sortering van meerdere actief-actief of actief / passief clustermodel vereist aanvullende code voor beheer van en bewerkingen. Dit is ook vereist coördinatie van de back-ups van de services in een datacenter of regio zodat ze beschikbaar zijn in andere datacenters of de regio's wanneer een mislukt. 
2. Voer één Service Fabric-cluster die meerdere datacenters of regio's omvat. De minimale ondersteunde configuratie voor deze is drie datacenters of regio's. Het aanbevolen aantal gebieden of datacenters is vijf. Hiervoor moet een complexere clustertopologie. Het voordeel van dit model is echter dat falen van een datacenter of regio na een noodgeval is geconverteerd naar een normale fout. Deze fouten kunnen worden verwerkt door de mechanismen die werken voor clusters binnen één regio. Zorg ervoor werkbelastingen worden gedistribueerd, zodat ze normaal fouten tolereren domeinen met fouten en upgradedomeinen regels voor de plaatsing van de Service Fabric. Voor meer informatie over beleidsregels die kunnen helpen bij het gebruiken van services in dit type cluster Lees informatie over [plaatsingsbeleid](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)

### <a name="random-failures-leading-to-cluster-failures"></a>Willekeurige fouten voorloopspaties van clusters
Service Fabric heeft het concept van Seed-knooppunten. Dit zijn de knooppunten die de beschikbaarheid van de onderliggende cluster onderhouden. Deze knooppunten helpen om te controleren of dat het cluster blijft door tot stand brengen van leases met andere knooppunten en fungeren als tiebreakers tijdens bepaalde soorten netwerkfouten. Als willekeurige fouten een meerderheid van de seed-knooppunten in het cluster verwijderen en ze niet worden toegevoegd, het cluster automatisch afgesloten. In Azure, Seed-knooppunten automatisch worden beheerd: ze worden verdeeld over de beschikbare domeinen met fouten en upgradedomeinen en als een enkel seed-knooppunt wordt verwijderd uit het cluster een andere naam in plaats daarvan wordt gemaakt. 

'Primaire knooppunttype' is in zelfstandige Service Fabric-clusters en Azure die de zaden wordt uitgevoerd. Bij het definiëren van een primaire knooppunttype Service Fabric, automatisch profiteren van het aantal knooppunten dat is geleverd door het maken van maximaal 9 seed-knooppunten en 9 replica's van elk van de systeemservices. Als een set van willekeurige fouten tegelijkertijd uit een meerderheid van die replica van de service system duurt, voer de systeemservices quorum verloren zijn gegaan, zoals hierboven is beschreven. Als een meerderheid van de seed-knooppunten verloren gaan, wordt het cluster zich snel na afgesloten.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het simuleren van diverse fouten met de [testbaarheid framework](service-fabric-testability-overview.md)
- Lezen van andere bronnen voor herstel na noodgevallen en hoge beschikbaarheid. Microsoft heeft een grote hoeveelheid informatie over deze onderwerpen worden gepubliceerd. Hoewel sommige van deze documenten naar specifieke technieken voor gebruik in andere producten verwijzen, bevatten ze veel algemene aanbevolen procedures die u in de Service Fabric-context toepassen kunt:
  - [Beschikbaarheidscontrolelijst](../best-practices-availability-checklist.md)
  - [Uitvoeren van een herstel na noodgevallen detailanalyse](../sql-database/sql-database-disaster-recovery-drills.md)
  - [Herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen][dr-ha-guide]
- Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)

<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
