---
title: Beveiligen van virtuele machines die worden geïmplementeerd in Azure Stack | Microsoft Docs
description: Richtlijnen voor het beveiligen van virtuele machines die worden geïmplementeerd in Azure Stack.
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
ms.date: 12/10/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.openlocfilehash: 45e22f19c6e2da26105615da6a775eed4f8676f0
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54243629"
---
# <a name="protect-virtual-machines-deployed-on-azure-stack"></a>Beveiligen van virtuele machines die worden geïmplementeerd in Azure Stack

Gebruik dit artikel als richtlijn bij het ontwikkelen van een plan voor het beveiligen van virtuele machines (VM's) die uw gebruikers in Azure Stack implementeren.

Ter bescherming tegen gegevensverlies en niet-geplande uitvaltijd, moet u een plan voor herstel van back-up of herstel na noodgevallen voor gebruikerstoepassingen en hun gegevens te implementeren. Dit plan kan zijn uniek voor elke toepassing, maar volgt een framework dat tot stand gebracht door uw organisatie uitgebreide zakelijke continuïteit en noodherstelplan (BC/DR). Is een goed uitgangspunt [flexibele toepassingen ontwerpen voor Azure](https://docs.microsoft.com/azure/architecture/resiliency), waarmee u algemene patronen en procedures voor de beschikbaarheid en tolerantie.

>[!IMPORTANT]
> Test uw plannen voor herstel van back-up en herstel na noodgevallen regelmatig. U moet dit om ervoor te zorgen dat:
> * De abonnementen werken
> * De abonnementen is nog steeds voldoen aan de behoeften die voor zijn ontworpen.

## <a name="azure-stack-infrastructure-recovery"></a>Herstel van Azure Stack-infrastructuur

Gebruikers zijn verantwoordelijk voor het beveiligen van hun VM's afzonderlijk in infrastructuurservices van Azure Stack.

Het herstelplan te gaan voor infrastructuurservices van Azure Stack **niet** herstel van gebruiker virtuele machines, opslagaccounts en databases bevatten. Als de eigenaar van de toepassing bent u verantwoordelijk voor het implementeren van een herstelplan voor uw toepassingen en gegevens.

Als de Azure Stack-cloud voor een langere periode offline is of permanent niet kan worden hersteld, u beschikken over een herstel van plan bent moet plaats dat:

* Zorgt ervoor dat minimale downtime
* Houdt van kritieke VM's, zoals databaseservers, waarop wordt uitgevoerd
* Hiermee kunnen toepassingen voor het onderhoud van aanvragen van gebruikers behouden

De beheerder van de Azure Stack-cloud is verantwoordelijk voor het maken van een herstelplan voor de onderliggende Azure Stack-infrastructuur en services. Lees voor meer informatie het artikel [herstel na onherstelbare gegevensverlies](https://docs.microsoft.com/azure/azure-stack/azure-stack-backup-recover-data).

## <a name="sourcetarget-combinations"></a>Bron-/ doelopslagaccount combinaties

Elke Azure Stack-cloud wordt geïmplementeerd op één datacenter. Een afzonderlijke omgeving is vereist, zodat u uw toepassingen kunt herstellen. De omgeving voor herstel is een andere Azure Stack-cloud in een ander datacenter of de openbare cloud van Azure. Uw gegevenssoevereiniteit en vereisten voor privacy, bepaalt de Herstelomgeving voor uw toepassing. Zoals u beveiliging voor elke toepassing inschakelt, beschikt u over de flexibiliteit om een specifieke hersteloptie voor elk criterium kiezen. U kunt toepassingen hebben in één abonnement back-ups van gegevens naar een ander datacenter. In een ander abonnement, kunt u gegevens kunt repliceren naar de openbare cloud van Azure.

Plan de strategie voor herstel van back-up en herstel na noodgevallen voor elke toepassing om te bepalen het doel voor elke toepassing. Een herstelplan helpt uw organisatie goed grootte van de vereiste capaciteit on-premises opslag en project verbruik in de openbare cloud.

|  | Global Azure | Azure Stack geïmplementeerd in CSP-datacenter en worden beheerd door de CSP | Azure Stack geïmplementeerd in het datacenter van de klant en worden beheerd door de klant |
|------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **Azure Stack geïmplementeerd in CSP-datacenter en worden beheerd door de CSP** | Gebruiker virtuele machines worden geïmplementeerd naar de CSP beheerd Azure Stack.<br><br>Gebruiker virtuele machines zijn teruggezet vanuit een back-up of failover rechtstreeks naar Azure. | CSP werkt de primaire en secundaire exemplaren van Azure Stack in hun eigen datacenters.<br><br>Gebruiker virtuele machines worden hersteld of failover tussen de twee exemplaren van Azure Stack. | CSP werkt Azure Stack in de primaire site.<br><br>Datacenter van de klant is het doel voor herstel en failovers. |
| **Azure Stack geïmplementeerd in het datacenter van de klant en worden beheerd door de klant** | Gebruiker virtuele machines worden geïmplementeerd naar de klant beheerd Azure Stack.<br><br>Gebruiker virtuele machines zijn teruggezet vanuit een back-up of failover rechtstreeks naar Azure. | Klant werkt Azure Stack in de primaire site.<br><br>Van de CSP-datacenter is het doel voor herstel en failovers. | Klant werkt de primaire en secundaire exemplaren van Azure Stack in hun eigen datacenters.<br><br>Gebruiker virtuele machines worden hersteld of failover tussen de twee exemplaren van Azure Stack. |

![Combinaties van bron-doel](media/azure-stack-manage-vm-backup/vm_backupdataflow_01.png)

## <a name="application-recovery-objectives"></a>De beoogde hersteltijden toepassing

U moet bepalen hoeveel downtime en gegevensverlies die uw organisatie voor elke toepassing kan tolereren. U kunt een plan voor herstel die de gevolgen van een noodgeval op uw organisatie minimaliseert maken door kwantificeren downtime en gegevensverlies. Voor elke toepassing, kunt u overwegen:

 - **Beoogde hersteltijd (RTO)**  
RTO is de maximaal acceptabele tijd die een toepassing na een incident niet beschikbaar mag zijn. Een RTO 90 minuten betekent bijvoorbeeld dat u moet de toepassing naar een actieve status binnen 90 minuten vanaf het begin van een noodgeval kunt herstellen. Als u een lage RTO hebt, kunt u een tweede implementatie continu stand-by te beschermen tegen regionale uitval.
 - **Beoogde herstelpunt (RPO)**  
RPO is de maximale duur van het verlies van gegevens tijdens een noodgeval. Als u bijvoorbeeld gegevens opslaat in één database, zonder replicatie in een andere database en elk uur een back-up, kunt u maximaal een uur gegevensverlies hebben.

RTO en RPO zijn bedrijfsvereisten. Een risicobeoordeling noodzakelijk voor het definiëren van de RTO en RPO van de toepassing uitvoeren.

Nog een metrische waarde is **Mean Time herstellen** (MTTR), dit is de gemiddelde tijd die nodig is voor het herstellen van de toepassing na een storing. De MTTR is een empirisch waarde voor een systeem. Als de MTTR langer is dan de RTO dan treedt na een storing in het systeem een onacceptabele bedrijfsonderbreking op omdat het niet mogelijk is het systeem binnen de gedefinieerde RTO te herstellen.

### <a name="backup-restore"></a>Back-up herstellen

De meest voorkomende bescherming-schema voor toepassingen op basis van een virtuele machine is het gebruik van back-upsoftware. Back-ups van een virtuele machine normaal gesproken bevat het besturingssysteem, besturingssysteemconfiguratie, binaire waarden van toepassingen en toepassingsgegevens. De back-ups worden gemaakt door het maken van een momentopname van de volumes, schijven of de hele virtuele machine. Met Azure Stack, u hebt de flexibiliteit van de back-ups van uit binnen de context van het gastbesturingssysteem te installeren of uit de Azure Stack-opslag en compute-API's. Azure Stack biedt geen ondersteuning voor back-ups van nemen op het hypervisorniveau van de.
 
![Back-up-restor](media/azure-stack-manage-vm-backup/vm_backupdataflow_03.png)

Herstellen van de toepassing, moet een of meer virtuele machines herstellen naar de cloud of naar een nieuwe cloud. U kunt een cloud doelgroep in uw datacenter of in de openbare cloud. De cloud die u kiest, is volledig binnen uw beheer en is gebaseerd op uw vereisten voor privacy en de onafhankelijkheid van gegevens.
 
 - RTO: Gemeten in uren downtime
 - RPO: Variabele gegevensverlies (afhankelijk van de back-upfrequentie)
 - Implementatietopologie: Actief/passief

#### <a name="planning-your-backup-strategy"></a>Uw strategie voor back-up plannen

Plannen van uw strategie voor back-up en het definiëren van schaalvereisten begint met het aantal VM-exemplaren die moeten worden beveiligd kwantificeren. Back-ups van alle virtuele machines voor alle servers in een omgeving is een algemene strategie. Met Azure Stack zijn er echter enkele VM's die u wilt back-up worden gemaakt. Bijvoorbeeld, virtuele machines in een schaalset worden beschouwd als tijdelijke resources die kunnen worden geleverd en gaan, soms zonder voorafgaande kennisgeving. Duurzame gegevens die moet worden beveiligd, worden opgeslagen in een afzonderlijke opslagplaats, zoals een database of het object store.

Belangrijke overwegingen voor het back-ups van virtuele machines in Azure Stack:

 - **Categorisatie**
    - Houd rekening met een model waarin gebruikers zich aan voor back-up van virtuele machine meldt.
    - Definieer een recovery service level agreement (SLA) op basis van de prioriteit van de toepassingen of de impact op het bedrijf.
 - **Schalen**
    - Houd rekening met gespreide back-ups wanneer toevoegen een groot aantal nieuwe VM's (als back-up vereist is).
    - Back-producten die efficiënt kunnen vastleggen en verzenden van back-upgegevens om te beperken van de inhoud van de resource op de oplossing worden geëvalueerd.
    - Back-producten die efficiënt back-gegevens opslaan met incrementele of differentiële back-ups om te beperken van de noodzaak van volledige back-ups voor alle virtuele machines in de omgeving worden geëvalueerd.
 - **Herstellen**
    - Back-producten kunnen virtuele schijven, toepassingsgegevens binnen een bestaande virtuele machine, of de hele VM-resource en bijbehorende virtuele schijven herstellen. Het terugzetten schema u moet afhankelijk van hoe u van plan bent om terug te zetten van de toepassing en het is van invloed op uw toepassing de tijd om te herstellen. Bijvoorbeeld, kan het zijn gemakkelijker te implementeren, SQL server vanuit een sjabloon en herstel vervolgens de databases in plaats van de volledige virtuele machine of een set van virtuele machines herstellen.

### <a name="replicationmanual-failover"></a>Replicatie/handmatige failover

Een alternatieve methode om ontwikkelaars te ondersteunen hoge beschikbaarheid is het uw toepassings-VM's repliceren naar een andere cloud en zijn afhankelijk van een handmatige failover. De replicatie van het besturingssysteem, binaire waarden van toepassingen en toepassingsgegevens kan worden uitgevoerd op het niveau van de virtuele machine of het niveau van gastbesturingssysteem. De failover wordt beheerd met behulp van aanvullende software die geen deel uitmaakt van de toepassing.

Met deze methode voert de toepassing wordt geïmplementeerd in een cloud en de virtuele machine is gerepliceerd naar de andere cloud. Als een failover wordt geactiveerd, de secundaire virtuele machines moeten worden ingeschakeld in de tweede cloud. In sommige scenario's maakt de failover de schijven voor virtuele machines en wordt aan hen. Dit proces kan lang duren om uit te voeren, vooral bij een toepassing met meerdere lagen die een reeks specifieke opstarten vereist. Er kan ook worden stappen beschreven die moeten worden uitgevoerd voordat de toepassing is gereed om te beginnen met het afhandelen van aanvragen.

![Replicatie-handmatige failover](media/azure-stack-manage-vm-backup/vm_backupdataflow_02.png)

 - RTO: Downtime, gemeten in minuten
 - RPO: Variabele preventie van gegevensverlies (afhankelijk van de replicatiefrequentie)
 - Implementatietopologie: Actief/passief stand-by
 
### <a name="high-availabilityautomatic-failover"></a>Hoge beschikbaarheid en automatische failover

Voor toepassingen waar uw bedrijf een paar seconden of minuten downtime en minimaal gegevensverlies tolereren kan, moet u rekening houden met de configuratie van een hoge beschikbaarheid. Maximaal beschikbare toepassingen zijn ontworpen om snel en automatisch herstellen van fouten. Azure Stack-infrastructuur worden voor lokale hardwarefouten, hoge beschikbaarheid in het fysieke netwerk met behulp van twee top van rack-switches geïmplementeerd. Azure Stack maakt voor compute op fouten gebruik van meerdere knooppunten in een schaaleenheid. Op het niveau van de virtuele machine, kunt u schaalsets in combinatie met domeinen met fouten om ervoor te zorgen knooppuntfouten niet uitvoert in uw toepassing.

In combinatie met schaalsets moet uw toepassing systeemeigen ondersteuning voor hoge beschikbaarheid of ondersteuning voor het gebruik van de clustersoftware. Microsoft SQL Server biedt hoge beschikbaarheid bijvoorbeeld systeemeigen ondersteuning voor databases met synchrone doorvoer-modus. Echter, als u alleen asynchrone replicatie ondersteunen kan, klikt u vervolgens zal er gegevens verloren gaan. Toepassingen kunnen ook worden geïmplementeerd in een failovercluster waarop de clustersoftware verwerkt voor de automatische failover van de toepassing.

Met deze benadering de toepassing slechts in één cloud actief is, maar de software geïmplementeerd op meerdere clouds. De andere clouds zijn in standby-modus Gereed voor de toepassing te starten wanneer de failover wordt geactiveerd.

 - RTO: Downtime, gemeten in seconden
 - RPO: Minimaal gegevensverlies
 - Implementatietopologie: Actief/actief stand-by

### <a name="fault-tolerance"></a>Fouttolerantie

Azure Stack fysieke infrastructuur voor redundantie en beschikbaarheid van de service alleen beveiligen op basis van hardware op fouten/fouten niveau dergelijke een schijf, voeding, netwerkpoort of knooppunt. Echter, als uw toepassing altijd beschikbaar zijn moet en nooit gegevens kunt verliezen, u moet fouttolerantie systeemeigen implementeren in uw toepassing of aanvullende software gebruiken om in te schakelen fouttolerantie.

U moet eerst, zorg ervoor dat de toepassing die virtuele machines worden geïmplementeerd met behulp van de schaal wordt ingesteld om te beveiligen tegen storingen in knooppunt op serverniveau. Ter bescherming tegen de cloud gaat offline, moet dezelfde toepassing al worden geïmplementeerd op een andere cloud, zodat u kunt doorgaan deze aanvragen worden verwerkt zonder onderbreking. Dit model wordt doorgaans de implementatie van een actief-actief genoemd.

Houd er rekening mee dat elke Azure Stack-cloud onafhankelijk van elkaar worden verbonden, is zodat de clouds worden altijd beschouwd als actief vanuit het perspectief van een infrastructuur. In dit geval zijn meerdere actieve instanties van de toepassing geïmplementeerd op een of meer actieve clouds.

 - RTO: Er is geen downtime
 - RPO: Zonder verlies van gegevens
 - Implementatietopologie: Actief/actief

### <a name="no-recovery"></a>Er is geen recovery

Sommige toepassingen in uw omgeving mogelijk geen bescherming tegen ongeplande downtime of gegevensverlies. Bijvoorbeeld, virtuele machines die worden gebruikt voor het ontwikkelen en testen normaal gesproken hoeft niet te worden hersteld. Het is uw beslissing doen zonder beveiliging voor een toepassing of een specifieke virtuele machine. Azure Stack biedt geen back-up of een replicatie van virtuele machines van de onderliggende infrastructuur. Net als bij Azure, moet u zich aanmelden voor de beveiliging voor elke VM in elk van uw abonnementen.

 - RTO: Niet kan worden hersteld
 - RPO: Volledig gegevensverlies

## <a name="recommended-topologies"></a>Aanbevolen topologieën

Belangrijke overwegingen voor uw Azure Stack-implementatie:

|     | Aanbeveling | Opmerkingen |
|-------------------------------------------------------------------------------------------------|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Back-up/herstel virtuele machines naar een externe back-updoel al geïmplementeerd in uw datacenter | Aanbevolen | Profiteer van de bestaande back-upinfrastructuur en operationele vaardigheden. Zorg ervoor dat u het formaat van de back-upinfrastructuur, zodat deze gereed is om te beveiligen van de aanvullende VM-exemplaren. Zorg ervoor dat een back-upinfrastructuur niet dicht in de bron. U kunt VM's herstellen naar de bron van Azure Stack, naar een secundaire Azure Stack-exemplaar of naar Azure. |
| Back-up/herstel virtuele machines naar een externe back-updoel speciaal voor Azure Stack | Aanbevolen | U kunt een nieuwe back-upinfrastructuur of een toegewezen back-upinfrastructuur inrichten voor Azure Stack kopen. Zorg ervoor dat een back-upinfrastructuur niet dicht in de bron. U kunt VM's herstellen naar de bron van Azure Stack, naar een secundaire Azure Stack-exemplaar of naar Azure. |
| Back-up/herstel-VM's rechtstreeks naar de globale Azure of een vertrouwde serviceprovider | Aanbevolen | Zolang u kunt voldoen aan de privacy van gegevens en wettelijke vereisten, kunt u uw back-ups kunt opslaan in de globale Azure of een vertrouwde serviceprovider. De service-provider wordt in het ideale geval ook Azure Stack uitgevoerd zodat u consistentie in operationele ervaring wanneer u herstelt. |
| Replicatie/failover-VM's naar een afzonderlijk exemplaar van Azure Stack | Aanbevolen | In het geval van failover moet u een tweede Azure Stack-cloud volledig operationeel zijn, zodat u downtime van uitgebreide toepassingen kunt voorkomen. |
| Replicatie/failover-VM's rechtstreeks naar Azure of een vertrouwde serviceprovider | Aanbevolen | Zolang u kunt voldoen aan de privacy van gegevens en wettelijke vereisten, kunt u uw gegevens kunt repliceren naar de globale Azure of een vertrouwde serviceprovider. De service-provider wordt in het ideale geval ook Azure Stack uitgevoerd zodat u consistentie in operationele ervaring na een failover. |
| Back-updoel op de dezelfde Azure Stack-cloud met de gegevens van uw toepassing implementeren | Niet aanbevolen | Vermijd de opslag van back-ups binnen de dezelfde Azure Stack-cloud. Niet-geplande uitvaltijd van de cloud kunt u in uw primaire gegevens en back-upgegevens behouden. Als u een back-updoel implementeren als een virtueel apparaat (voor de doeleinden van optimalisatie voor back-up en herstel), kunt u alle gegevens continu worden gekopieerd naar een externe back-uplocatie moet zorgen. |
| Fysieke back-up toe aan de dezelfde rack installatie van de oplossing Azure Stack implementeren | Niet ondersteund | U kunt alle andere apparaten op dit moment, geen verbinding met de top van rack-switches die geen deel uitmaken van de oorspronkelijke oplossing. |

## <a name="next-steps"></a>Volgende stappen

In dit artikel opgegeven algemene richtlijnen voor het beveiligen van de gebruiker virtuele machines die zijn geïmplementeerd in Azure Stack. Zie voor informatie over het gebruik van Azure-services gebruiker VM's beveiligen:

 - [Gebruik Azure back-up naar back-up van bestanden en toepassingen in Azure Stack](https://docs.microsoft.com/azure/backup/backup-mabs-files-applications-azure-stack)
 - [Azure Backup Server-ondersteuning voor Azure Stack](https://docs.microsoft.com/azure/backup/ ) 
 - [Azure Site Recovery-ondersteuning voor Azure Stack](https://docs.microsoft.com/azure/site-recovery/)  

Raadpleeg voor meer informatie over de partnerproducten die worden geboden door VM-beveiliging op Azure Stack, "[bescherming van toepassingen en gegevens op Azure Stack](https://azure.microsoft.com/blog/protecting-applications-and-data-on-azure-stack/)."
