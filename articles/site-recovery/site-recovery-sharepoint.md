---
title: Een toepassing met meerdere lagen SharePoint met Azure Site Recovery repliceren | Microsoft Docs
description: In dit artikel wordt beschreven hoe een toepassing met meerdere lagen SharePoint met de mogelijkheden van Azure Site Recovery worden gerepliceerd.
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: sutalasi
ms.openlocfilehash: 55323df68715c80d5e8535199cd739921a3baad9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="replicate-a-multi-tier-sharepoint-application-for-disaster-recovery-using-azure-site-recovery"></a>Een toepassing met meerdere lagen SharePoint voor herstel na noodgevallen met Azure Site Recovery repliceren

Dit artikel wordt beschreven in de details van het beveiligen van een SharePoint-toepassing met [Azure Site Recovery](site-recovery-overview.md).


## <a name="overview"></a>Overzicht

Microsoft SharePoint is een krachtige toepassing waarmee u een groep kunt of afdeling organiseren, samenwerken en informatie delen. SharePoint kan bieden intranet-portals, document en bestandsbeheer, samenwerking, sociale netwerken, extranetten, websites, zoeken enterprise en business intelligence. Heeft ook integratie, integratie en werkstroom automatiseringsmogelijkheden worden uitgebreid. Normaal gesproken beschouwen organisaties als een laag 1-toepassing gevoelig zijn voor de uitvaltijd en gegevensverlies.

Microsoft SharePoint biedt vandaag de dag geen eventuele herstelfuncties out-of-the-box-noodherstel. Ongeacht het type en de schaal van een noodgeval moet voor herstel het gebruik van een stand-by-datacenter dat u van de farm kunt herstellen. Stand-by-datacenters zijn vereist voor scenario's waarbij lokaal redundante systemen en back-ups niet vanuit de onderbreking op het primaire Datacenter herstellen.

Een goede noodhersteloplossing kunt modellering van herstelplannen rond de architecturen voor complexe toepassingen zoals SharePoint. Deze moet ook de mogelijkheid toevoegen van aangepaste stappen voor het afhandelen van toepassingstoewijzingen tussen verschillende lagen en het geven van een failover met één Klik daarom met een lagere RTO er in een noodsituatie hebben.

Dit artikel wordt beschreven in de details van het beveiligen van een SharePoint-toepassing met [Azure Site Recovery](site-recovery-overview.md). Dit artikel wordt uitgelegd aanbevolen procedures voor het repliceren van een drie servicetier SharePoint-toepassing naar Azure, hoe u een herstel na noodgevallen detailanalyse kunt doen en hoe kunt u de toepassing in Azure.

U kunt bekijken de onderstaande video over het herstellen van een toepassing met meerdere lagen naar Azure.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/Disaster-Recovery-of-load-balanced-multi-tier-applications-using-Azure-Site-Recovery/player]


## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg er dan voor dat u het volgende weten:

1. [Een virtuele machine repliceren naar Azure](site-recovery-vmware-to-azure.md)
2. Hoe [ontwerpen van een netwerk herstel](site-recovery-network-design.md)
3. [Een testfailover uitvoeren naar Azure](site-recovery-test-failover-to-azure.md)
4. [U een failover naar Azure](site-recovery-failover.md)
5. Hoe [repliceren van een domeincontroller](site-recovery-active-directory.md)
6. Hoe [SQL-Server repliceren](site-recovery-sql.md)

## <a name="sharepoint-architecture"></a>SharePoint-architectuur

SharePoint kan worden geïmplementeerd op een of meer servers met gelaagde topologieën en serverfuncties voor het implementeren van een farm ontwerp dat voldoet aan specifieke doelen en doelstellingen. Een typische grote, hoogwaardige aanvraag SharePoint-serverfarm die ondersteuning biedt voor een groot aantal gelijktijdige gebruikers en een groot aantal inhoudsitems gebruik service groeperen als onderdeel van hun strategie schaalbaarheid. Deze methode omvat de services uitgevoerd op speciale servers, deze services te groeperen en vervolgens de servers uitbreiden als een groep. De volgende topologie ziet u de service en de server voor een drielaagse SharePoint-serverfarm groeperen. Raadpleeg de documentatie van SharePoint en product line architecturen voor gedetailleerde richtlijnen voor verschillende SharePoint-topologieën. U vindt meer informatie over de implementatie van SharePoint 2013 in [dit document](https://technet.microsoft.com/en-us/library/cc303422.aspx).



![Implementatie-patroon 1](./media/site-recovery-sharepoint/sharepointarch.png)


## <a name="site-recovery-support"></a>Site Recovery-ondersteuning

Virtuele VMware-machines met Windows Server 2012 R2 Enterprise zijn voor het maken van dit artikel gebruikt. SharePoint 2013 Enterprise edition en Enterprise-editie van SQL server 2014 werden gebruikt. Als de replicatie van Site Recovery networkdirect van toepassing is, worden de aanbevelingen die om op voor de volgende scenario's evenals verwacht.

### <a name="source-and-target"></a>Bron en doel

**Scenario** | **Een secundaire site** | **Naar Azure**
--- | --- | ---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja
**Fysieke server** | Ja | Ja

### <a name="sharepoint-versions"></a>SharePoint-versies
De volgende versies van de SharePoint-server worden ondersteund.

* SharePoint server 2013 standaard
* SharePoint server 2013 Enterprise
* SharePoint server 2016 standaard
* SharePoint server 2016 Enterprise

### <a name="things-to-keep-in-mind"></a>Houd er rekening mee houden

Als u een gedeelde cluster op basis van schijven als een laag in uw toepassing vervolgens zich u niet kunnen gebruiken van replicatie van Site Recovery voor replicatie van die virtuele machines. U kunt gebruik systeemeigen replicatie geleverd door de toepassing en gebruik vervolgens een [herstelplan](site-recovery-create-recovery-plans.md) failover alle lagen.

## <a name="replicating-virtual-machines"></a>Repliceren van virtuele machines

Ga als volgt [in deze richtlijnen](site-recovery-vmware-to-azure.md) om te repliceren van de virtuele machine in Azure.

* Zodra de replicatie voltooid is, zorg ervoor dat u gaat u naar elke virtuele machine van elke laag en selecteert u dezelfde beschikbaarheidsset voor ' gerepliceerde item > Instellingen > Eigenschappen > berekening en netwerk '. Bijvoorbeeld, als uw weblaag 3 VM's heeft, zorg ervoor dat alle 3 VM's zijn geconfigureerd voor het onderdeel zijn van dezelfde beschikbaarheidsset in Azure.

    ![Set Beschikbaarheidsset](./media/site-recovery-sharepoint/select-av-set.png)

* Zie voor instructies over het beveiligen van Active Directory en DNS [beveiligen van Active Directory en DNS](site-recovery-active-directory.md) document.

* Zie voor instructies over het beveiligen van databaselaag uitgevoerd op de SQL server [SQL Server beveiligen](site-recovery-active-directory.md) document.

## <a name="networking-configuration"></a>Netwerkconfiguratie

### <a name="network-properties"></a>Eigenschappen van het netwerk

* Voor de App en Web laag VMs netwerkinstellingen in Azure-portal zodanig configureren dat de virtuele machines ophalen gekoppeld aan het juiste DR-netwerk na een failover.

    ![Netwerk selecteren](./media/site-recovery-sharepoint/select-network.png)


* Als u een statisch IP-adres gebruikt, geeft u het IP-adres dat u wilt dat de virtuele machine moet worden uitgevoerd de **IP-adres doel** veld

    ![Vaste IP-adres instellen](./media/site-recovery-sharepoint/set-static-ip.png)

### <a name="dns-and-traffic-routing"></a>DNS- en verkeersroutering

Voor sites, internetgericht [maken van een Traffic Manager-profiel van het type 'Prioriteit'](../traffic-manager/traffic-manager-create-profile.md) in de Azure-abonnement. En configureer vervolgens uw DNS- en Traffic Manager-profiel in de volgende manier.


| **Waar** | **Bron** | **Doel**|
| --- | --- | --- |
| Openbare DNS-server | Openbare DNS-server voor SharePoint-sites <br/><br/> Voorbeeld: sharepoint.contoso.com | Traffic Manager <br/><br/> contososharepoint.trafficmanager.NET |
| On-premises DNS | sharepointonprem.contoso.com | Openbare IP-adres op de lokale farm |


In de Traffic Manager-profiel [maken van de primaire en herstelsites eindpunten](../traffic-manager/traffic-manager-configure-priority-routing-method.md). Het externe eindpunt gebruiken voor lokale eindpunt en openbare IP-adres voor de Azure-eindpunt. Zorg ervoor dat de prioriteit is ingesteld om te kunnen lokaal eindpunt.

Een testpagina op een specifieke poort (bijvoorbeeld 800) worden gehost in de SharePoint-endweblaag in volgorde voor Traffic Manager beschikbaarheid na failover automatisch detecteren. Dit is een tijdelijke oplossing als u anonieme verificatie niet voor een van uw SharePoint-sites inschakelen.

[Het Traffic Manager-profiel configureert](../traffic-manager/traffic-manager-configure-priority-routing-method.md) met de onderstaande instellingen.

* Routeringsmethode - 'Prioriteit'
* DNS-tijd voor live (TTL) - 30 seconden
* Instellingen voor eindpunt monitor - als u anonieme verificatie kunt inschakelen, kunt u een specifieke website-eindpunt kunt geven. Of u kunt een testpagina gebruiken op een specifieke poort (bijvoorbeeld 800).

## <a name="creating-a-recovery-plan"></a>Een herstelplan maken

Een herstelplan kunt de failover van de verschillende lagen in een toepassing met meerdere lagen, daarom toepassing consistentie van de sequentiëring. Volg de onderstaande stappen te volgen bij het maken van een herstelplan voor een webtoepassing met meerdere lagen. [Meer informatie over het maken van een herstelplan](site-recovery-runbook-automation.md#customize-the-recovery-plan).

### <a name="adding-virtual-machines-to-failover-groups"></a>Virtuele machines worden toegevoegd aan failover-groepen

1. Een herstelplan maken door de App en Web laag VM's toe te voegen.
2. Klik op 'Aanpassen' voor het groeperen van de virtuele machines. Standaard uitmaken alle virtuele machines deel van 'Groep 1'.

    ![RP aanpassen](./media/site-recovery-sharepoint/rp-groups.png)

3. Maken van een andere groep (groep 2) en de weblaag virtuele machines verplaatsen naar de nieuwe groep. Uw virtuele machines van de App-laag moeten deel uitmaken van 'Groep 1' en weblaag virtuele machines moeten deel uitmaken van 'Groep 2'. Dit is ervoor te zorgen dat het opstarten virtuele machines van App-laag van eerst gevolgd door de weblaag virtuele machines.


### <a name="adding-scripts-to-the-recovery-plan"></a>Scripts toe te voegen aan het herstelplan

U kunt de meest gebruikte Azure Site Recovery-scripts implementeren in uw Automation-account te klikken op de knop 'Deploy to Azure' hieronder. Wanneer u een script voor gepubliceerde gebruikt, zorg ervoor dat u Volg de instructies in het script.

[![Implementeren in Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

1. Een script vóór actie toevoegen aan 'Groep 1' bij failover-SQL-beschikbaarheidsgroep. Het is gepubliceerd in de voorbeeldscripts 'ASR SQL FailoverAG'-script gebruiken. Zorg ervoor dat u Volg de instructies in het script en breng de gewenste wijzigingen in het script op de juiste wijze.

    ![Toevoegen-AG-Script-stap-1](./media/site-recovery-sharepoint/add-ag-script-step1.png)

    ![Toevoegen-AG-Script-stap-2](./media/site-recovery-sharepoint/add-ag-script-step2.png)

2. Een script post actie om te koppelen van een load balancer op de mislukte toevoegen via de virtuele machines van de weblaag (groep 2). Het is gepubliceerd in de voorbeeldscripts 'ASR AddSingleLoadBalancer'-script gebruiken. Zorg ervoor dat u Volg de instructies in het script en breng de gewenste wijzigingen in het script op de juiste wijze.

    ![Toevoegen-LB-Script-stap-1](./media/site-recovery-sharepoint/add-lb-script-step1.png)

    ![Toevoegen-LB-Script-stap-2](./media/site-recovery-sharepoint/add-lb-script-step2.png)

3. Toevoegen van een handmatige stap voor het bijwerken van de DNS-records om te verwijzen naar de nieuwe farm in Azure.

    * Er is geen DNS-updates zijn voor internetgericht sites vereist na failover. Volg de stappen in de sectie 'Netwerken richtlijnen' voor het configureren van Traffic Manager. Als de Traffic Manager-profiel is ingesteld zoals beschreven in de vorige sectie, moet u een script om te openen dummy-poort (800 in het voorbeeld) op de Azure VM toevoegen.

    * Voor interne gerichte sites, voegt u een handmatige stap voor het bijwerken van de DNS-record om te verwijzen naar de nieuwe Web-laag van de virtuele machine load balancer IP.

4. Toevoegen van een handmatige stap zoektoepassing terugzetten vanaf een back-up of een nieuwe search-service starten.

5. Voor de Search-servicetoepassing is teruggezet vanaf een back-up, volgt u onderstaande stappen te volgen.

    * Deze methode wordt ervan uitgegaan dat een back-up van de servicetoepassing zoeken voordat de onherstelbare gebeurtenis is uitgevoerd en dat de back-up beschikbaar op de DR-site is.
    * Dit kan eenvoudig worden bereikt door het plannen van de back-up (bijvoorbeeld, eenmaal per dag) en een kopieerprocedure voor het plaatsen van de back-up op de DR-site. Procedures kopie kunnen programma's systeemscript zoals AzCopy (Azure Copy)- of DFSR (Distributed File Services Replication) instellen.
    * Nu dat de SharePoint-farm wordt uitgevoerd, gaat u de Centraalbeheersite 'Back-up en herstellen' en selecteer herstellen. Het terugzetten van de back-uplocatie opgegeven interrogates (mogelijk moet u de waarde bijwerken). Selecteer de Search-servicetoepassing back-up die u wilt herstellen.
    * Zoeken wordt teruggezet. Houd er rekening mee dat het terugzetten van verwacht te zoeken naar dezelfde topologie (hetzelfde aantal servers) en dezelfde vaste stationsletters toegewezen aan deze servers. Zie voor meer informatie ['Herstellen Search-service-toepassing in SharePoint 2013'](https://technet.microsoft.com/library/ee748654.aspx) document.


6. Voor het beginnen met een nieuwe toepassing van de Search-service, volgt u onderstaande stappen te volgen.

    * Deze methode wordt ervan uitgegaan dat er een back-up van de database 'Search-beheer' beschikbaar op de DR-site is.
    * Aangezien de Search-servicetoepassing-databases worden niet gerepliceerd, moeten ze opnieuw worden gemaakt. Navigeer naar de Centraalbeheersite en de Search-Service-toepassing verwijderen om dit te doen. Op servers waarop de Search-Index wordt gehost, de indexbestanden te verwijderen.
    * Opnieuw maken van de servicetoepassing en de databases opnieuw gemaakt. Het is raadzaam een voorbereide script die deze servicetoepassing opnieuw worden gemaakt omdat het is niet mogelijk alle acties via de gebruikersinterface uit te voeren. Bijvoorbeeld, zijn de locatie van de index schijf en het configureren van de topologie voor het zoeken alleen mogelijk met SharePoint PowerShell-cmdlets. Gebruik de Windows PowerShell-cmdlet terugzetten SPEnterpriseSearchServiceApplication en geef het logboek is verzonden en gerepliceerde Search-beheer database, Search_Service__DB. Deze cmdlet geeft de zoekconfiguratie, schema, beheerde eigenschappen, regels en bronnen en maakt u een standaardset aan de andere onderdelen.
    * Zodra de Search-servicetoepassing is niet opnieuw worden gemaakt, moet u een volledige verkenning voor elke inhoudsbron om terug te zetten van de Search-Service starten. Er verloren gegevens gaan analytics uit de lokale farm, zoals zoeken aanbevelingen.

7. Wanneer alle stappen zijn voltooid, sla het herstelplan en de uiteindelijke herstelplan volgende eruit.

    ![Opgeslagen RP](./media/site-recovery-sharepoint/saved-rp.png)

## <a name="doing-a-test-failover"></a>Een testfailover uitvoeren
Ga als volgt [in deze richtlijnen](site-recovery-test-failover-to-azure.md) een testfailover uitvoeren.

1.  Ga naar de Azure-portal en selecteer de Recovery Services-kluis.
2.  Klik op het herstelplan gemaakt voor de SharePoint-toepassing.
3.  Klik op 'Testfailover'.
4.  Selecteer het herstelpunt en virtuele Azure-netwerk om de test failover-proces te starten.
5.  Als de secundaire-omgeving is, kunt u uw validaties kunt uitvoeren.
6.  Als de validaties voltooid zijn, kunt u 'Opschonen testfailover' op het herstelplan en de testfailoveromgeving is opgeschoond.

Voor hulp bij de testfailover uitvoeren voor AD en DNS, verwijzen naar [Testfailover-overwegingen voor AD- en DNS-](site-recovery-active-directory.md#test-failover-considerations) document.

Raadpleeg voor hulp bij de testfailover doet voor SQL altijd op beschikbaarheidsgroepen, [doen testfailover voor SQL Server Always On](site-recovery-sql.md#steps-to-do-a-test-failover) document.

## <a name="doing-a-failover"></a>U een failover uitvoert
Ga als volgt [in deze richtlijnen](site-recovery-failover.md) om een failover uit te voeren.

1.  Ga naar de Azure-portal en selecteer de Recovery Services-kluis.
2.  Klik op het herstelplan gemaakt voor de SharePoint-toepassing.
3.  Klik op 'Failover'.
4.  Selecteer het herstelpunt om de failoverproces te starten.

## <a name="next-steps"></a>Volgende stappen
U kunt meer lezen over [andere toepassingen repliceren](site-recovery-workload.md) met Site Recovery.
