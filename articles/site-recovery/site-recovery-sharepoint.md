---
title: Herstel na noodgevallen voor een toepassing met meerdere lagen SharePoint met behulp van Azure Site Recovery instellen | Microsoft Docs
description: In dit artikel wordt beschreven hoe u herstel na noodgevallen voor een toepassing met meerdere lagen SharePoint met behulp van Azure Site Recovery-mogelijkheden.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: sutalasi
ms.openlocfilehash: 1f3168cef503fed0aea09228c9bc11dfb456f2ab
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848473"
---
# <a name="set-up-disaster-recovery-for-a-multi-tier-sharepoint-application-for-disaster-recovery-using-azure-site-recovery"></a>Instellen van herstel na noodgevallen voor een toepassing met meerdere lagen SharePoint voor herstel na noodgevallen met Azure Site Recovery

Dit artikel wordt beschreven in de details van het beveiligen van een SharePoint-toepassing met [Azure Site Recovery](site-recovery-overview.md).


## <a name="overview"></a>Overzicht

Microsoft SharePoint is een krachtige toepassing waarmee u een groep kunt of afdeling delen, samenwerken en delen van gegevens. SharePoint krijgt u een intranet-portals, document en bestandsbeheer, samenwerking, sociale netwerken, extranetten, websites, enterprise search en business intelligence. Het bevat ook systeemintegratie, procesintegratie en automatiseringsmogelijkheden van de werkstroom. Normaal gesproken beschouwen organisaties als een Tier-1-toepassing gevoelig zijn voor de downtime en gegevensverlies.

Microsoft SharePoint biedt vandaag de dag niet alle herstelfuncties out-of-the-box na noodgevallen. Ongeacht het type en de schaal van een noodgeval moet herstel u het gebruik van een stand-by-datacenter dat u van de farm kunt herstellen. Stand-by-datacenters zijn vereist voor scenario's waarbij lokaal redundante systemen en back-ups van de serviceonderbreking op het primaire Datacenter niet herstellen.

Een goede noodhersteloplossing mag modellering van herstelplannen rond de architecturen voor complexe toepassingen zoals SharePoint. Het moet ook de mogelijkheid het toevoegen van aangepaste stappen voor het afhandelen van toepassingstoewijzingen tussen verschillende lagen en kan daarom voorzien van een failover met één klik een lagere RTO in het geval van een noodgeval.

Dit artikel wordt beschreven in de details van het beveiligen van een SharePoint-toepassing met [Azure Site Recovery](site-recovery-overview.md). In dit artikel wordt beschreven aanbevolen procedures voor het repliceren van een SharePoint-toepassing naar Azure, hoe u een Dr-herstelanalyse kunt doen en hoe kunt u de toepassing in Azure een failover met lagen.

U kunt controleren of de onder de video over het herstellen van een toepassing met meerdere lagen naar Azure.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/Disaster-Recovery-of-load-balanced-multi-tier-applications-using-Azure-Site-Recovery/player]


## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg er dan voor dat u het volgende weten:

1. [Een virtuele machine repliceren naar Azure](site-recovery-vmware-to-azure.md)
2. Hoe u [een herstelnetwerk ontwerpen](site-recovery-network-design.md)
3. [Een testfailover uitvoeren naar Azure](site-recovery-test-failover-to-azure.md)
4. [Een failover uitvoert naar Azure](site-recovery-failover.md)
5. Hoe u [repliceren van een domeincontroller](site-recovery-active-directory.md)
6. Hoe u [SQL Server repliceren](site-recovery-sql.md)

## <a name="sharepoint-architecture"></a>SharePoint-architectuur

SharePoint kan worden geïmplementeerd op een of meer servers gebruik van gelaagde topologieën en server-functies voor het implementeren van een farm ontwerp dat voldoet aan de specifieke doelen en doelstellingen. Groepering van de service een typische grote, veelgevraagde SharePoint server-farm die ondersteuning biedt voor een groot aantal gelijktijdige gebruikers en een groot aantal inhoudsitems gebruiken als onderdeel van hun strategie voor schaalbaarheid. Deze benadering omvat het services uitgevoerd op toegewezen servers, groeperen van deze services, en vervolgens de servers uitbreiden als een groep. De volgende topologie ziet u de service en de server voor een drie lagen SharePoint-serverfarm groeperen. Raadpleeg SharePoint-documentatie en architecturen voor product-regel voor gedetailleerde richtlijnen voor verschillende SharePoint-topologieën. U vindt meer informatie over de implementatie van SharePoint 2013 in [dit document](https://technet.microsoft.com/library/cc303422.aspx).



![Implementatie-patroon 1](./media/site-recovery-sharepoint/sharepointarch.png)


## <a name="site-recovery-support"></a>Ondersteuning voor Site Recovery

Voor het maken van dit artikel, zijn VMware-machines met Windows Server 2012 R2 Enterprise gebruikt. SharePoint 2013 Enterprise edition en Enterprise-editie van SQL server 2014 werden gebruikt. Site Recovery-replicatie is neutraal van toepassing, zijn de aanbevelingen die hier beschikbaar zijn om op te slaan op voor de volgende scenario's evenals verwacht.

### <a name="source-and-target"></a>Bron en doel

**Scenario** | **Naar een secundaire site** | **Naar Azure**
--- | --- | ---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja
**Fysieke server** | Ja | Ja
**Azure** | N.v.t. | Ja

### <a name="sharepoint-versions"></a>SharePoint-versies
De volgende versies van de SharePoint-server worden ondersteund.

* SharePoint server 2013 Standard
* SharePoint server 2013 Enterprise
* SharePoint server 2016 Standard
* SharePoint server 2016 Enterprise

### <a name="things-to-keep-in-mind"></a>Houd er rekening mee

Als u een gedeelde cluster op basis van een schijf als een laag in uw toepassing vervolgens kunt u zich niet kan Site Recovery-replicatie gebruiken voor het repliceren van deze virtuele machines. U kunt gebruik systeemeigen replicatie geleverd door de toepassing en gebruik vervolgens een [herstelplan](site-recovery-create-recovery-plans.md) failover alle lagen.

## <a name="replicating-virtual-machines"></a>Replicatie van virtuele machines

Ga als volgt [deze richtlijnen](site-recovery-vmware-to-azure.md) om te beginnen met het repliceren van de virtuele machine naar Azure.

* Nadat de replicatie voltooid is, zorg ervoor dat u gaat u naar elke virtuele machine van elke laag en selecteert u dezelfde beschikbaarheidsset ' gerepliceerd item > Instellingen > Eigenschappen > berekening en netwerk '. Bijvoorbeeld, als uw weblaag 3 virtuele machines, zorg ervoor dat alle 3 virtuele machines zijn geconfigureerd als onderdeel van dezelfde beschikbaarheidsset in Azure.

    ![Set-Availability-Set](./media/site-recovery-sharepoint/select-av-set.png)

* Raadpleeg voor meer informatie over het beveiligen van Active Directory en DNS [beveiligen van Active Directory en DNS](site-recovery-active-directory.md) document.

* Raadpleeg voor meer informatie over het beveiligen van databaselaag wordt uitgevoerd in SQL server [SQL-Server beveiligen](site-recovery-active-directory.md) document.

## <a name="networking-configuration"></a>Netwerkconfiguratie

### <a name="network-properties"></a>Netwerkeigenschappen

* Voor de App en Web-laag virtuele machines, netwerkinstellingen configureren in Azure portal zodat de virtuele machines ophalen die is gekoppeld aan het juiste DR-netwerk na een failover.

    ![Netwerk selecteren](./media/site-recovery-sharepoint/select-network.png)


* Als u een statisch IP-adres gebruikt, geeft u de IP-adres dat u wilt dat de virtuele machine op te nemen de **doel-IP** veld

    ![Statisch IP-adres instellen](./media/site-recovery-sharepoint/set-static-ip.png)

### <a name="dns-and-traffic-routing"></a>DNS- en routering van verkeer

Voor sites, internetgerichte [maken van een Traffic Manager-profiel van het type 'Prioriteit'](../traffic-manager/traffic-manager-create-profile.md) in het Azure-abonnement. En configureer vervolgens uw DNS- en Traffic Manager-profiel in de volgende manier.


| **waar** | **Bron** | **Doel**|
| --- | --- | --- |
| Openbare DNS | Openbare DNS voor SharePoint-sites <br/><br/> Bijvoorbeeld: sharepoint.contoso.com | Traffic Manager <br/><br/> contososharepoint.trafficmanager.net |
| On-premises DNS | sharepointonprem.contoso.com | Openbaar IP-adres op de on-premises-farm |


In de Traffic Manager-profiel, [maken van de primaire en eindpunten](../traffic-manager/traffic-manager-configure-priority-routing-method.md). Gebruik het externe eindpunt is voor het eindpunt van de on-premises en openbare IP-adres voor Azure-eindpunt. Zorg ervoor dat de prioriteit hoger naar on-premises-eindpunt is ingesteld.

Een testpagina op een specifieke poort (bijvoorbeeld 800) hosten in de weblaag SharePoint in volgorde voor Traffic Manager voor het automatisch detecteren van beschikbaarheid na failover. Dit is een tijdelijke oplossing in het geval u anonieme verificatie niet op een van uw SharePoint-sites inschakelen.

[Traffic Manager-profiel configureren](../traffic-manager/traffic-manager-configure-priority-routing-method.md) met de onderstaande instellingen.

* Routeringsmethode - 'Prioriteit'
* DNS-tijd voor live (TTL) - 30 seconden
* Instellingen voor eindpunt monitor - als u anonieme verificatie kunt inschakelen, kunt u een bepaalde website-eindpunt kunt bieden. Of u kunt een testpagina gebruiken op een specifieke poort (bijvoorbeeld 800).

## <a name="creating-a-recovery-plan"></a>Het maken van een herstelplan

Een plan voor herstel kunt de failover van de verschillende lagen in een toepassing met meerdere lagen, daarom onderhouden van de toepassingsconsistentie te sequentiëren. Volg de onderstaande stappen te volgen bij het maken van een plan voor herstel voor een webtoepassing met meerdere lagen. [Meer informatie over het maken van een herstelplan](site-recovery-runbook-automation.md#customize-the-recovery-plan).

### <a name="adding-virtual-machines-to-failover-groups"></a>VM's toevoegt aan de failover-groepen

1. Maak een plan voor herstel door toe te voegen van de App en Web-laag virtuele machines.
2. Klik op 'Aanpassen' aan de groep van de virtuele machines. Standaard worden alle virtuele machines deel uit van 'Groep 1'.

    ![RP aanpassen](./media/site-recovery-sharepoint/rp-groups.png)

3. Maak een andere groep (groep 2) en de weblaag virtuele machines verplaatsen naar de nieuwe groep. Uw App-laag virtuele machines moeten deel uitmaken van 'Groep 1' en weblaag virtuele machines moeten deel uitmaken van 'Groep 2'. Dit is om ervoor te zorgen dat de App-laag virtuele machines opstarten om eerst gevolgd door de weblaag VM's.


### <a name="adding-scripts-to-the-recovery-plan"></a>Scripts toe te voegen aan het herstelplan te gaan

U kunt de meestgebruikte scripts voor de Azure Site Recovery implementeren in uw Automation-account te klikken op de knop 'Implementeren in Azure' hieronder. Wanneer u een script voor gepubliceerde gebruikt, zorg ervoor dat u Volg de instructies in het script.

[![Implementeren in Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

1. Een actie voorafgaand aan de-script toevoegen aan 'Groep 1' bij failover-SQL-beschikbaarheidsgroep. Gebruik het script 'ASR-SQL-FailoverAG' gepubliceerd in de voorbeelden van scripts. Zorg ervoor dat u Volg de instructies in het script en breng de benodigde wijzigingen in het script op de juiste wijze.

    ![Toevoegen-AG-Script-stap-1](./media/site-recovery-sharepoint/add-ag-script-step1.png)

    ![Toevoegen-AG-Script-stap-2](./media/site-recovery-sharepoint/add-ag-script-step2.png)

2. Toevoegen van een script van de actie bericht als u wilt koppelen van een load balancer op de failover van virtuele machines van de weblaag (groep 2). Gebruik het script 'ASR-AddSingleLoadBalancer' gepubliceerd in de voorbeelden van scripts. Zorg ervoor dat u Volg de instructies in het script en breng de benodigde wijzigingen in het script op de juiste wijze.

    ![Toevoegen-LB-Script-stap-1](./media/site-recovery-sharepoint/add-lb-script-step1.png)

    ![Toevoegen-LB-Script-stap-2](./media/site-recovery-sharepoint/add-lb-script-step2.png)

3. Toevoegen van een handmatige stap voor het bijwerken van de DNS-records om te verwijzen naar de nieuwe farm configureren in Azure.

    * Voor internetservices van sites, zijn er geen DNS-updates vereist na failover. Volg de stappen in de sectie 'Netwerkondersteuning' voor het configureren van Traffic Manager. Als de Traffic Manager-profiel is ingesteld zoals wordt beschreven in de vorige sectie, voegt u een script om dummy poort (800 in het voorbeeld) op de Azure VM te openen.

    * Voor interne gerichte sites, voegt u een handmatige stap voor het bijwerken van de DNS-record om te verwijzen naar de nieuwe Web-laag van de virtuele machine load balancer IP toe.

4. Een handmatige stap als u wilt zoektoepassing herstellen vanuit een back-up of start een nieuwe zoekservice toevoegen.

5. Voor het herstellen van Search service-toepassing vanuit een back-up, volg de onderstaande stappen.

    * Deze methode wordt ervan uitgegaan dat een back-up van de Search Service-toepassing is uitgevoerd voordat de catastrofale gebeurtenis plaatsvindt en dat de back-up beschikbaar op de DR-site is.
    * Dit kan eenvoudig worden bereikt door het plannen van de back-up (bijvoorbeeld, eenmaal per dag) en een kopieerprocedure voor het plaatsen van de back-up op de DR-site. Kopie procedures omvat bijvoorbeeld scripts programma's zoals AzCopy (Azure-kopie) of instellen van DFS-replicatie (Distributed File Services Replication).
    * Nu dat de SharePoint-farm wordt uitgevoerd, gaat u de centrale beheersite, 'Back-up en herstellen' en selecteer herstellen. Het terugzetten van de back-uplocatie opgegeven interrogates (mogelijk moet u de waarde bijwerken). Selecteer de Search Service-toepassing back-up die u wilt herstellen.
    * Zoeken is hersteld. Houd er rekening mee dat het terugzetten van verwacht te vinden van dezelfde topologie (hetzelfde aantal servers) en hetzelfde harde schijf letters die zijn toegewezen aan deze servers. Zie voor meer informatie, ['Herstellen Search service-toepassing in SharePoint 2013'](https://technet.microsoft.com/library/ee748654.aspx) document.


6. Beginnen met een nieuwe toepassing van de Search-service, volgt u onderstaande stappen.

    * Deze methode wordt ervan uitgegaan dat er een back-up van de database "Search-beheer" beschikbaar op de DR-site is.
    * Omdat de Search Service-toepassing-databases niet zijn gerepliceerd, moeten ze opnieuw worden gemaakt. Om dit te doen, gaat u naar de centrale beheersite en verwijder de Search Service-toepassing. Op alle servers waarop de Search-Index wordt gehost, de indexbestanden te verwijderen.
    * De Search Service-toepassing opnieuw te maken en de databases opnieuw gemaakt. Het wordt aanbevolen dat een voorbereide script die deze servicetoepassing opnieuw worden gemaakt omdat het is niet mogelijk alle acties via de gebruikersinterface uit te voeren. Bijvoorbeeld, zijn de locatie van de schijf index en configureren van de topologie voor het zoeken alleen mogelijk met behulp van de SharePoint PowerShell-cmdlets. Gebruik de Windows PowerShell-cmdlet Restore-SPEnterpriseSearchServiceApplication en geef het logboek verzonden en gerepliceerd Search-beheer database, Search_Service__DB. Deze cmdlet biedt de zoekconfiguratie, schema, beheerde eigenschappen, regels en bronnen en maakt u een standaardset aan de andere onderdelen.
    * Zodra de Search Service-toepassing is niet opnieuw worden gemaakt, kunt u een volledige verkenning voor elke inhoudsbron voor het herstellen van de Search-Service moet starten. Er verloren gegevens gaan analytics uit de on-premises-farm, zoals aanbevelingen zoeken.

7. Wanneer alle stappen zijn voltooid, sla het herstelplan te gaan en het laatste herstelplan wordt als volgt uitzien.

    ![Opgeslagen RP](./media/site-recovery-sharepoint/saved-rp.png)

## <a name="doing-a-test-failover"></a>Een testfailover uitvoeren
Ga als volgt [deze richtlijnen](site-recovery-test-failover-to-azure.md) doet een test-failover.

1.  Ga naar Azure portal en selecteert u de Recovery Services-kluis.
2.  Klik op het herstelplan te gaan voor SharePoint-toepassing gemaakt.
3.  Klik op 'Testfailover'.
4.  Selecteer herstelpunt en Azure-netwerk om de test-failover-proces te starten.
5.  Nadat de secundaire-omgeving is, kunt u uw validaties kunt uitvoeren.
6.  Zodra de validaties voltooid zijn, kunt u 'Failovertest' in het herstelplan en de testfailoveromgeving is opgeschoond.

Voor hulp bij het testfailover uitvoert voor AD en DNS, verwijzen naar [Testfailover-overwegingen voor AD- en DNS](site-recovery-active-directory.md#test-failover-considerations) document.

Raadpleeg voor informatie over de testfailover organiseert voor SQL Always ON-beschikbaarheidsgroepen, [failover testen uitvoeren voor SQL Server Always On](site-recovery-sql.md#steps-to-do-a-test-failover) document.

## <a name="doing-a-failover"></a>Een failover uitvoert
Ga als volgt [deze richtlijnen](site-recovery-failover.md) voor het uitvoeren van een failover.

1.  Ga naar Azure portal en selecteert u de Recovery Services-kluis.
2.  Klik op het herstelplan te gaan voor SharePoint-toepassing gemaakt.
3.  Klik op 'Failover'.
4.  Selecteer het herstelpunt om de failoverproces te starten.

## <a name="next-steps"></a>Volgende stappen
Vindt u meer informatie over [andere toepassingen repliceren](site-recovery-workload.md) met Site Recovery.
