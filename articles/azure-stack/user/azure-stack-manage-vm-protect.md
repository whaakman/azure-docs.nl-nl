---
title: Beveiligen van virtuele machines die worden geïmplementeerd op Azure-Stack | Microsoft Docs
description: Richtlijnen voor het beveiligen van virtuele machines die worden geïmplementeerd op Azure-Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 4e5833cf-4790-4146-82d6-737975fb06ba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/14/2018
ms.author: jeffgilb
ms.reviewer: hector.linares
ms.openlocfilehash: 734ee0e6ffb0dab660a2b63b431780208e0e0484
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="protect-virtual-machines-deployed-on-azure-stack"></a>Beveiligen van virtuele machines die worden geïmplementeerd op Azure-Stack

Gebruik dit artikel als richtlijn bij het ontwikkelen van een plan voor het beveiligen van virtuele machines (VM's) die uw gebruikers op Azure-Stack implementeert.

Om te beschermen tegen gegevensverlies en niet-geplande uitvaltijd, moet u een plan voor herstel van back-up of herstel na noodgevallen voor toepassingen en hun gegevens te implementeren. Dit plan kan zijn uniek voor elke toepassing, maar volgt een framework dat is ingesteld door uw organisatie uitgebreide zakelijke continuïteit en noodherstelplan (BC/DR). Is een goed uitgangspunt [ontwerpen robuuste toepassingen voor Azure](https://docs.microsoft.com/azure/architecture/resiliency), waarmee u algemene patronen en procedures voor de beschikbaarheid van toepassingen en tolerantie.

>[!IMPORTANT]
> Test uw plannen voor herstel van back-up en herstel na noodgevallen voortdurend. U moet dit ervoor te zorgen dat:
> * Het werk plannen
> * De abonnementen is nog steeds voldoen aan de behoeften die ze zijn ontworpen.

## <a name="azure-stack-infrastructure-recovery"></a>Herstel van Azure Stack-infrastructuur

Gebruikers zijn verantwoordelijk voor het beveiligen van de bijbehorende virtuele machines afzonderlijk van de Azure-Stack-infrastructuurservices.

Het herstelplan voor Azure-Stack infrastructuurservices **heeft geen** herstel van virtuele machines voor gebruikers, storage-accounts of databases bevatten. Als de eigenaar van de toepassing bent u verantwoordelijk voor het implementeren van een herstelplan voor uw toepassingen en gegevens.

Als de Azure-Stack cloud voor een langere periode offline is of permanent onherstelbare, u beschikken over een herstel plannen moet plaats dat:

* Zorgt ervoor dat de minimale downtime
* Houdt kritieke virtuele machines, zoals databaseservers, waarop wordt uitgevoerd
* Kunnen toepassingen in onderhoud van aanvragen van gebruikers behouden

De operator van de cloud met Azure-Stack is verantwoordelijk voor het maken van een herstelplan voor de onderliggende Azure Stack-infrastructuur en -services. Lees voor meer informatie het artikel [herstellen vanaf onherstelbaar gegevensverlies](https://docs.microsoft.com/azure/azure-stack/azure-stack-backup-recover-data).

## <a name="sourcetarget-combinations"></a>Combinaties bron/doel

Elke Azure-Stack cloud wordt geïmplementeerd naar een datacenter. Er is een afzonderlijke omgeving vereist zodat u uw toepassingen kunt herstellen. De Herstelomgeving mag een andere Azure-Stack cloud in een ander datacenter of de openbare Azure-cloud. Uw onafhankelijkheid van gegevens en privacy-vereisten voor gegevens, bepalen de Herstelomgeving voor uw toepassing. Als u beveiliging voor elke toepassing inschakelt, kunt u de flexibiliteit om te kiezen, een specifieke hersteloptie voor elk criterium hebben. U kunt toepassingen in één back-ups van gegevens naar een ander datacenter-abonnement hebben. In een ander abonnement kunt u gegevens repliceren naar de openbare Azure-cloud.

Plan de strategie voor herstel van back-up en herstel na noodgevallen voor elke toepassing om te bepalen van het doel voor elke toepassing. Een herstelplan helpt uw organisatie goed grootte van de opslag vereiste capaciteit on-premises en project weer in de openbare cloud.

|  | Globale Azure | Azure-Stack in de CSP-datacenter geïmplementeerd en beheerd door de CSP | Azure Stack naar het datacenter van de klant is geïmplementeerd en beheerd door de klant |
|------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **Azure-Stack in de CSP-datacenter geïmplementeerd en beheerd door de CSP** | Virtuele machines voor gebruikers worden gedistribueerd naar de CSP beheerd Azure-Stack. Virtuele machines voor gebruikers zijn teruggezet vanuit een back-up of een failover naar Azure rechtstreeks. | CSP werkt de primaire en secundaire exemplaren van Azure-Stack in hun eigen datacenters. Virtuele machines voor gebruikers worden hersteld of failover tussen de twee exemplaren van de Azure-Stack. | CSP werkt Azure-Stack in de primaire site. Datacenter van de klant is het terugzetten of failover. |
| **Azure Stack naar het datacenter van de klant is geïmplementeerd en beheerd door de klant** | Virtuele machines voor gebruikers worden geïmplementeerd op de klant beheerd Azure-Stack. Virtuele machines voor gebruikers zijn teruggezet vanuit een back-up of een failover naar Azure rechtstreeks. | De klant werkt de primaire en secundaire exemplaren van Azure-Stack in hun eigen datacenters. Virtuele machines voor gebruikers worden hersteld of failover tussen de twee exemplaren van de Azure-Stack. | De klant werkt Azure-Stack in de primaire site. De CSP-datacenter is het terugzetten of failover. |

![Combinaties van de bron-doel](media\azure-stack-manage-vm-backup\vm_backupdataflow_01.png)

## <a name="application-recovery-objectives"></a>Herstel de doelstellingen van toepassing

U moet de hoeveelheid uitvaltijd en verlies van gegevens die van uw organisatie voor elke toepassing tolereren kan bepalen. U kunt een herstelplan dat de impact van een noodgeval op uw organisatie minimaliseert maken door uitvaltijd en gegevensverlies kwantificeren. U kunt voor elke toepassing:

 - **Beoogde hersteltijd (RTO)**  
RTO is de maximale toegestane tijd die een toepassing niet beschikbaar nadat een incident zijn kan. Een RTO van 90 minuten betekent bijvoorbeeld dat u kunt de toepassing naar een actieve status binnen 90 minuten vanaf het begin van een noodgeval herstellen moet zijn. Als u een laag RTO hebt, kunt u een tweede implementatie voortdurend uitgevoerd op stand-by als bescherming tegen een regionale uitval bewaren.
 - **Beoogd herstelpunt (RPO)**  
RPO is de maximale duur van het verlies van gegevens die tijdens een noodgeval aanvaardbaar is. Als u bijvoorbeeld gegevens opslaat in één database, zonder replicatie in een andere database en elk uur een back-up, kunt u maximaal een uur gegevensverlies hebben.

RTO en RPO zijn bedrijfsvereisten. Uitvoeren van een risicoanalyse uit te voeren om de RTO en de RPO van de toepassing te definiëren.

Een andere meetwaarde is **Mean Time herstellen** (MTTR) Dit is de gemiddelde tijd die nodig is voor het herstellen van de toepassing na een storing. MTTR is een empirische waarde voor een systeem. Als de MTTR langer is dan de RTO dan treedt na een storing in het systeem een onacceptabele bedrijfsonderbreking op omdat het niet mogelijk is het systeem binnen de gedefinieerde RTO te herstellen.

### <a name="backup-restore"></a>Back-up terugzetten

De meest voorkomende schema beveiliging voor VM-toepassingen is het gebruik van back-upsoftware. Doorgaans back-ups van een virtuele machine bevat het besturingssysteem, besturingssysteemconfiguratie, binaire bestanden van toepassingen en toepassingsgegevens. De back-ups worden gemaakt door het maken van een momentopname van de volumes, schijven of de hele virtuele machine. Met Azure-Stack u beschikt over de flexibiliteit van een back-up uit in de context van het gastbesturingssysteem of de Stack van Azure-opslag en berekeningen van API's. Azure-Stack biedt geen ondersteuning voor back-ups van nemen op het niveau van de hypervisor.
 
![Back-up restor](media\azure-stack-manage-vm-backup\vm_backupdataflow_03.png)

Herstellen van de toepassing vereist een of meer virtuele machines herstellen naar de cloud met dezelfde of naar een nieuwe cloud. U kunt een cloud richten in uw datacenter of de openbare cloud. De cloud die u kiest is volledig binnen het besturingselement en is gebaseerd op de privacy- en onafhankelijkheid vereisten van uw gegevens.
 
 - RTO: Uitvaltijd gemeten in uren
 - RPO: Variabele gegevensverlies (afhankelijk van de back-upfrequentie)
 - Implementatietopologie: actief/passief-

#### <a name="planning-your-backup-strategy"></a>Uw strategie voor back-up plannen

Uw strategie voor back-up plannen en het definiëren van vereisten voor scale begint met het aantal VM-exemplaren die moeten worden beveiligd kwantificeren. Back-ups van alle VM's op alle servers in een omgeving is een algemene strategie. Met de Azure-Stack zijn er echter enkele virtuele machines die hoeven te worden back-up. Bijvoorbeeld: tijdelijke bronnen die kunnen worden geleverd en gaat, soms zonder voorafgaande kennisgeving worden beschouwd als virtuele machines in een schaalset. Duurzame gegevens die moet worden beveiligd, wordt opgeslagen in een afzonderlijke opslagplaats, zoals een database of het object store.

Belangrijke aandachtspunten voor de back-ups van virtuele machines op Azure-Stack:

 - **Categorisatie**
    - U kunt een model waarin gebruikers aan het VM-back-up deelneemt.
    - Definieer een recovery service level agreement (SLA) op basis van de prioriteit van de toepassingen of de impact op het bedrijf.
 - **Schalen**
    - Houd rekening met gespreide back-ups wanneer op twee locaties een groot aantal nieuwe virtuele machines (als de back-up is vereist).
    - Back-up van de producten die efficiënt kunnen vastleggen en verzenden van back-upgegevens om te beperken van de inhoud van de resource op de oplossing evalueren.
    - Evalueer back-producten die efficiënt opslaan back-upgegevens incrementele of differentiële back-ups met de noodzaak van volledige back-ups over alle VM's in de omgeving te minimaliseren.
 - **Herstellen**
    - Back-producten kunnen herstellen voor virtuele schijven, toepassingsgegevens binnen een bestaande VM of de hele VM-resource en de bijbehorende virtuele schijven. Het herstel u moet afhankelijk van hoe u van plan bent om te herstellen van de toepassing en dat invloed op de tijd van uw toepassing op herstel. Het kan bijvoorbeeld makkelijker kunnen SQL server vanuit een sjabloon implementeren en zet de databases in plaats van het herstellen van de hele virtuele machine of een set van virtuele machines zijn.

### <a name="replicationmanual-failover"></a>Replicatie/handmatige failover

Een alternatieve methode om ondersteuning van hoge beschikbaarheid is uw toepassing virtuele machines repliceren naar een andere cloud en zijn afhankelijk van een handmatige failover. De replicatie van het besturingssysteem, binaire bestanden van toepassingen en toepassingsgegevens kan worden uitgevoerd op de VM niveau of Gast OS. De failover wordt beheerd met aanvullende software die geen deel uitmaakt van de toepassing.

Met deze methode voert de toepassing wordt geïmplementeerd in een cloud en de VM is gerepliceerd naar de andere cloud. Als een failover wordt geactiveerd, de secundaire virtuele machines moeten worden ingeschakeld in de tweede cloud. In sommige scenario's maakt de failover de VM's en wordt schijven toe. Dit proces kan lang duren om te voltooien, met name een toepassing met meerdere lagen die een specifieke opstartprocedure vereist. Er kan ook worden stappen beschreven die moeten worden uitgevoerd voordat de toepassing is om te beginnen met aanvragen worden verwerkt.

![Replicatie-handmatige failover](media\azure-stack-manage-vm-backup\vm_backupdataflow_02.png)

 - RTO: Uitvaltijd uitgedrukt in minuten
 - RPO: Variabele gegevensverlies (afhankelijk van de replicatiefrequentie)
 - Implementatietopologie: actief/passief-stand-by
 
### <a name="high-availabilityautomatic-failover"></a>Hoge beschikbaarheid en automatische failover

Voor toepassingen waarin uw bedrijf een paar seconden of minuten uitvaltijd en minimaal gegevensverlies tolereert, moet u rekening houden met een configuratie met hoge beschikbaarheid. Toepassingen met hoge beschikbaarheid zijn ontworpen om snel en automatisch herstellen van fouten. Voor lokale hardwarefouten implementeert Azure Stack infrastructuur hoge beschikbaarheid in het fysieke netwerk door middel van twee bovenaan rack-switches. Voor niveau compute-fouten gebruikt Azure Stack meerdere knooppunten in een schaaleenheid. Op de VM-niveau, kunt u schaalsets in combinatie met domeinen met fouten om ervoor te zorgen knooppuntfouten worden pas van kracht uw toepassing.

In combinatie met schaalsets moet uw toepassing systeemeigen ondersteuning voor hoge beschikbaarheid of ondersteuning voor het gebruik van de clustersoftware. Microsoft SQL Server biedt hoge beschikbaarheid bijvoorbeeld systeemeigen ondersteuning voor databases met synchrone doorvoermodus. Echter als u biedt slechts ondersteuning voor asynchrone replicatie, wordt er een aantal gegevens verloren gaan. Toepassingen kunnen ook worden geïmplementeerd in een failovercluster waarop de clustersoftware omgaat met automatische failover van de toepassing.

Met deze benadering de toepassing is alleen beschikbaar in een cloud, maar de software wordt geïmplementeerd op meerdere clouds. De andere clouds zijn in standby-modus om te beginnen met de toepassing wanneer de failover wordt geactiveerd.

 - RTO: Uitvaltijd gemeten in seconden
 - RPO: De minimale gegevensverlies
 - Implementatietopologie: actief/actief stand-by

### <a name="fault-tolerance"></a>Fouttolerantie

Azure Stack fysieke redundantie en infrastructuur servicebeschikbaarheid alleen bescherming tegen hardware niveau fouten/fouten dergelijke een schijf, voeding, netwerkpoort of een knooppunt. Echter, als uw toepassing moet altijd beschikbaar zijn en kan geen gegevens nooit verliezen, u moet fouttolerantie systeemeigen implementeren in uw toepassing of aanvullende software gebruiken om in te schakelen fouttolerantie.

U moet eerst, om te controleren of de toepassing die virtuele machines zijn geïmplementeerd met behulp van de schaal wordt ingesteld om te beschermen tegen fouten in de knooppunt-niveau. Ter bescherming tegen de cloud gaat offline, moet dezelfde toepassing al worden geïmplementeerd op een andere cloud, zodat deze aanvragen worden verwerkt zonder onderbreking kunt doorgaan. Dit model wordt doorgaans verwezen naar een actieve implementatie.

Houd er rekening mee dat elke cloud Azure-Stack onafhankelijk van elkaar, is zodat de clouds worden altijd beschouwd als actief vanuit het perspectief van een infrastructuur. In dit geval zijn meerdere actieve exemplaren van de toepassing geïmplementeerd op een of meer actieve clouds.

 - RTO: Geen uitvaltijd
 - RPO: Zonder verlies van gegevens
 - Implementatietopologie: actief/actief

### <a name="no-recovery"></a>Geen herstel

Sommige toepassingen in uw omgeving hoeft waarschijnlijk geen bescherming tegen niet-geplande uitvaltijd of verlies van gegevens. Bijvoorbeeld, virtuele machines gebruikt voor ontwikkeling en testen normaal gesproken hoeft niet te worden hersteld. Het is uw beslissing te doen zonder de beveiliging voor een toepassing of een specifieke virtuele machine. Azure-Stack biedt geen back-up of replicatie van virtuele machines van de onderliggende infrastructuur. Net als bij Azure, u moet zich aanmelden bij de beveiliging voor elke virtuele machine in elk van uw abonnementen.

 - RTO: onherstelbare
 - RPO: Het volledig gegevensverlies

## <a name="recommended-topologies"></a>Aanbevolen topologieën

Belangrijke aandachtspunten voor de implementatie van uw Azure-Stack:

|     | Aanbeveling | Opmerkingen |
|-------------------------------------------------------------------------------------------------|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Back-up/herstel virtuele machines naar een externe back-updoel al geïmplementeerd in uw datacenter | Aanbevolen | Profiteren van bestaande back-upinfrastructuur en operationele vaardigheden. Zorg ervoor dat het formaat van de back-upinfrastructuur zodat deze gereed om te beschermen aanvullende VM-exemplaren is. Zorg ervoor dat de back-upinfrastructuur is niet dicht in de bron. U kunt virtuele machines herstellen naar de bron-Stack van Azure naar een secundair Azure-Stack-exemplaar of Azure. |
| Back-up/herstel virtuele machines naar een externe back-updoel toegewezen aan Azure-Stack | Aanbevolen | U kunt nieuwe back-upinfrastructuur of speciale back-upinfrastructuur inrichten aanschaffen voor Azure-Stack. Zorg ervoor dat de back-upinfrastructuur is niet dicht in de bron. U kunt virtuele machines herstellen naar de bron-Stack van Azure naar een secundair Azure-Stack-exemplaar of Azure. |
| Back-up/herstel van virtuele machines rechtstreeks naar de globale Azure of een vertrouwde serviceprovider | Aanbevolen | Zolang u kunt voldoen aan uw privacy van gegevens en wettelijke vereisten, kunt u uw back-ups opslaan in globale Azure of een vertrouwde serviceprovider. De serviceprovider wordt in het ideale geval ook Azure Stack uitgevoerd zodat u consistentie in praktijkervaring wanneer u herstelt. |
| Virtuele machines repliceren/failover naar een ander exemplaar van de Azure-Stack | Aanbevolen | In het geval failover moet u een tweede Stack Azure-cloud volledig operationeel hebben, zodat u kunt uitgebreide toepassing uitvaltijd voorkomen. |
| Virtuele machines repliceren/failover rechtstreeks naar Azure of een vertrouwde serviceprovider | Aanbevolen | Zolang u kunt voldoen aan uw privacy van gegevens en wettelijke vereisten, kunt u uw gegevens repliceren naar de globale Azure of een vertrouwde serviceprovider. In het ideale geval is de serviceprovider Azure Stack ook uitgevoerd zodat u de consistentie in praktijkervaring na een failover downloaden. |
| Back-updoel op dezelfde Azure-Stack cloud met de toepassingsgegevens van uw implementeren | Niet aanbevolen | Vermijd het opslaan van back-ups in dezelfde Azure-Stack cloud. Niet-geplande uitvaltijd van de cloud kunt houden van uw primaire gegevensbron en back-upgegevens. Als u kiest voor het implementeren van een back-updoel als een virtueel apparaat (voor de doeleinden van optimalisatie voor back-up en herstel), kunt u alle gegevens continu worden gekopieerd naar een externe back-uplocatie moet zorgen. |
| Implementeren van fysieke back-apparaat in het hetzelfde rack waarop de Azure-Stack-oplossing is geïnstalleerd | Niet ondersteund | U kunt andere apparaten op dit moment, kan geen verbinding met de bovenkant van rack-switches die geen deel uitmaken van de oorspronkelijke oplossing. |

## <a name="next-steps"></a>Volgende stappen

In dit artikel opgegeven algemene richtlijnen voor het beveiligen van virtuele machines op Azure-Stack is geïmplementeerd voor gebruikers. Zie voor informatie over het gebruik van Azure-services voor het beveiligen van virtuele machines voor gebruikers:

 - [Gebruik Azure back-up naar back-up van bestanden en toepassingen op Azure-Stack](https://docs.microsoft.com/azure/backup/backup-mabs-files-applications-azure-stack)
 - [Ondersteuning van Azure Backup-Server voor Azure-Stack](https://docs.microsoft.com/azure/backup/ ) 
 - [Azure Site Recovery-ondersteuning voor Azure-Stack](https://docs.microsoft.com/azure/site-recovery/)  

Raadpleeg voor meer informatie over de partner-producten die bescherming van de VM op Azure-Stack bieden '[bescherming van toepassingen en gegevens op Azure Stack](https://azure.microsoft.com/blog/protecting-applications-and-data-on-azure-stack/). "
