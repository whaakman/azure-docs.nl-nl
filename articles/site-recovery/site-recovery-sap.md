---
title: Een meerlaagse SAP NetWeaver toepassingsimplementatie met Azure Site Recovery beveiligen | Microsoft Docs
description: In dit artikel wordt beschreven hoe het beveiligen van SAP NetWeaver toepassingsimplementaties met behulp van Azure Site Recovery.
services: site-recovery
documentationcenter: ''
author: asgang
manager: rochakm
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2018
ms.author: asgang
ms.openlocfilehash: 27dfdec4e833a2f30963157ba2f4d95232e21270
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35267329"
---
# <a name="protect-a-multi-tier-sap-netweaver-application-deployment-by-using-site-recovery"></a>Een implementatie voor meerdere lagen SAP NetWeaver beveiligen met behulp van Site Recovery

De meeste implementaties voor grote en middelgrote SAP gebruiken een vorm van noodherstel. Het belang van oplossingen voor herstel na noodgevallen voor robuuste en testable is toegenomen omdat meer kernprocessen worden verplaatst naar toepassingen zoals SAP. Azure Site Recovery is getest en geïntegreerd met SAP-toepassingen. Site Recovery is groter dan de mogelijkheden van de meeste lokale noodhersteloplossingen en op een lagere totale eigendomskosten (TCO) dan concurrerende oplossingen.

Met Site Recovery kunt u het volgende doen:
* **Schakel de beveiliging van de SAP NetWeaver en niet-NetWeaver productie-toepassingen die on-premises uitgevoerd** door onderdelen repliceren naar Azure.
* **Schakel de beveiliging van SAP NetWeaver en niet-NetWeaver productie-toepassingen die worden uitgevoerd op Azure** door onderdelen repliceren naar een andere Azure-datacenter.
* **Vereenvoudig de cloudmigratie** met behulp van Site Recovery voor uw SAP-implementatie te migreren naar Azure.
* **Vereenvoudig SAP project upgrades, testen en maken van een prototype** door het maken van een productie klonen op verzoek voor het testen van SAP-toepassingen.

Dit artikel wordt beschreven voor het beveiligen van SAP NetWeaver toepassingsimplementaties met behulp van [Azure Site Recovery](site-recovery-overview.md). Het artikel bevat informatie over aanbevolen procedures voor het beveiligen van een implementatie van de drie lagen SAP NetWeaver in Azure door repliceren naar een andere Azure-datacenter met behulp van Site Recovery. Beschrijft de ondersteunde scenario's en configuraties en het uitvoeren van testfailovers (noodhersteloefeningen) en de werkelijke failovers.

## <a name="prerequisites"></a>Vereisten
Voordat u begint, zorg ervoor dat u weet hoe u de volgende taken uitvoeren:

* [Een virtuele machine repliceren naar Azure](azure-to-azure-walkthrough-enable-replication.md)
* [Een recovery netwerk ontwerpen](site-recovery-azure-to-azure-networking-guidance.md)
* [Voer een testfailover naar Azure](azure-to-azure-walkthrough-test-failover.md)
* [Voer een failover naar Azure](site-recovery-failover.md)
* [Een domeincontroller worden gerepliceerd.](site-recovery-active-directory.md)
* [SQL Server repliceren](site-recovery-sql.md)

## <a name="supported-scenarios"></a>Ondersteunde scenario's
U kunt Site Recovery kunt gebruiken voor het implementeren van een noodherstel in de volgende scenario's:
* SAP-systemen uitgevoerd in een Azure-datacenter die worden gerepliceerd naar een andere Azure-datacenter (noodherstel Azure naar Azure). Zie voor meer informatie [Azure naar Azure replicatie architectuur](https://aka.ms/asr-a2a-architecture).
* SAP-systemen die zijn uitgevoerd in VMware (of fysieke) servers lokale die repliceren naar een site van het herstel na noodgevallen in een Azure-datacenter (noodherstel VMware naar Azure). Dit scenario is een aantal extra onderdelen vereist. Zie voor meer informatie [VMware naar Azure replicatie architectuur](https://aka.ms/asr-v2a-architecture).
* SAP-systemen is uitgevoerd op Hyper-V on-premises die worden gerepliceerd naar een site van het herstel na noodgevallen in een Azure-datacenter (Hyper-V-Azure-noodherstel). Dit scenario is een aantal extra onderdelen vereist. Zie voor meer informatie [architectuur van Hyper-V-Azure-replicatie](https://aka.ms/asr-h2a-architecture).

In dit artikel gebruiken we een **Azure naar Azure** noodherstelscenario ter illustratie van de mogelijkheden voor herstel na noodgevallen SAP van Site Recovery. Omdat de replicatie van Site Recovery niet toepassingsspecifieke, wordt het proces dat wordt beschreven verwacht gelden ook voor andere scenario's.

### <a name="required-foundation-services"></a>Vereiste foundation-services
In het scenario dat wordt besproken in dit artikel, worden de volgende foundation-services geïmplementeerd:
* Azure ExpressRoute of Azure VPN-Gateway
* Ten minste één Active Directory-domeincontroller en DNS-server, worden uitgevoerd in Azure

Het is raadzaam dat u deze infrastructuur maakt voordat u Site Recovery implementeert.

## <a name="reference-sap-application-deployment"></a>Verwijzing SAP-toepassingsimplementatie

Deze verwijzende architectuur toont SAP NetWeaver uitgevoerd in een Windows-omgeving op Azure met hoge beschikbaarheid.  Deze architectuur wordt geïmplementeerd met grootten van specifieke virtuele machine (VM) die kunnen worden gewijzigd zodat de behoeften van uw organisatie.

![Diagram van een doorsnee patroon voor SAP-implementatie](./media/site-recovery-sap/reference_sap.png)

## <a name="disaster-recovery-considerations"></a>Overwegingen voor herstel na noodgevallen

Voor noodherstel (DR) moet u het volgende kunnen failover naar een secundaire regio. Voor elke laag wordt een andere strategie gebruikt om DR (Disaster Recovery, herstel na noodgeval) te bieden.

#### <a name="vms-running-sap-web-dispatcher-pool"></a>Virtuele machines met SAP Web Dispatcher van toepassingen 
Het onderdeel Web Dispatcher wordt gebruikt als een load balancer voor SAP-verkeer tussen de SAP-toepassingsservers. Azure Load Balancer wordt om te zorgen voor hoge beschikbaarheid voor het onderdeel Web Dispatcher, gebruikt de parallelle Web Dispatcher-instellingen implementeren in een round robin-configuratie voor de distributie van HTTP (S)-verkeer tussen de beschikbare Web coördinatoren in de groep balancer. Dit wordt gerepliceerd met behulp van Azure Site Recovery (ASR) en automatiseringsscripts wordt gebruikt voor het load balancer configureren op het gebied van de herstel na noodgevallen. 

####<a name="vms-running-application-servers-pool"></a>Virtuele machines met servers toepassingsgroep
Voor het beheren van groepen van de aanmelding voor ABAP toepassingsservers, wordt de transactie SMLG gebruikt. De functie binnen de berichtenserver van de centrale Services voor de taakverdeling wordt gebruikt voor het distribueren van werkbelasting tussen servers SAP groep van toepassingen voor SAPGUIs en RFC verkeer. Dit wordt gerepliceerd met Azure Site Recovery 

####<a name="vms-running-sap-central-services-cluster"></a>Virtuele machines met SAP-Services voor centrale cluster
Deze verwijzende architectuur centrale Services worden uitgevoerd op virtuele machines in de toepassingslaag. De centrale Services is een potentieel storingspunt (SPOF) bij de implementatie op een enkele virtuele machine, normale implementatie als hoge beschikbaarheid geen vereiste is.<br>

Voor het implementeren van een oplossing voor hoge beschikbaarheid, kan een failovercluster gedeelde schijf of een cluster van share worden gebruikt. Gebruik Windows Server Failover Cluster voor meer informatie over het configureren van virtuele machines voor een cluster met gedeelde schijf. Cloud-Witness wordt aanbevolen als een quorumwitness. 
 > [!NOTE]
 > Azure Site Recovery wordt niet gerepliceerd van de cloud-witness wordt daarom aanbevolen voor het implementeren van de cloud-witness in regio herstel na noodgevallen.

Ter ondersteuning van de failover-clusteromgeving [SIOS DataKeeper Cluster Edition](https://azuremarketplace.microsoft.com/marketplace/apps/sios_datakeeper.sios-datakeeper-8) werkt cluster shared volume door te repliceren van onafhankelijke schijven die eigendom zijn van de clusterknooppunten. Azure biedt geen systeemeigen ondersteuning voor gedeelde schijven en daarom vereist SIOS-oplossingen. 

Een andere manier om af te handelen clustering is het implementeren van een cluster met delen. [SAP](https://blogs.sap.com/2018/03/19/migration-from-a-shared-disk-cluster-to-a-file-share-cluster) recent gewijzigde het patroon van de implementatie centrale Services voor toegang tot de algemene mappen /sapmnt via een UNC-pad. Deze wijziging Hiermee verwijdert u de vereiste voor SIOS of andere oplossingen gedeelde schijf op de centrale Services virtuele machines. Het is nog steeds aanbevolen om ervoor te zorgen dat de /sapmnt UNC-share maximaal beschikbaar is. Dit kan worden gedaan op de centrale Services-exemplaar met behulp van Windows Server-failovercluster met Scale Out File Server (SOFS) en de functie opslagruimten Direct (S2D) in Windows Server 2016. 
 > [!NOTE]
 > Ondersteuning voor Azure Site Recovery crash momenteel alleen consistent punt replicatie van virtuele machines met opslagruimten direct 


## <a name="disaster-recovery-considerations"></a>Overwegingen voor herstel na noodgeval

Azure Site Recovery kunt u de failover via indelen van volledige SAP-implementatie via de Azure-regio's.
Hieronder vindt u de stappen voor het instellen van het herstel na noodgevallen 

1. Virtuele machines repliceren 
2. Een recovery netwerk ontwerpen
3.  Een domeincontroller worden gerepliceerd.
4.  Base gegevenslaag repliceren 
5.  Voer een testfailover 
6.  Voer een failover 

Hieronder ziet u de aanbeveling voor herstel na noodgevallen van elke laag in dit voorbeeld gebruikt. 

 **SAP-lagen** | **Aanbeveling**
 --- | ---
**SAP Web dispatchergroep** |  Repliceren met Site recovery 
**SAP-toepassingsgroep server** |  Repliceren met Site recovery 
**SAP Services centrale cluster** |  Repliceren met Site recovery 
**Active directory virtuele machines** |  Active directory-replicatie 
**SQL database-servers** |  SQL altijd op replicatie

##<a name="replicate-virtual-machines"></a>Virtuele machines repliceren

Volg de instructies in voor het starten van alle SAP toepassing virtuele machines repliceren naar het datacenter van Azure disaster recovery [een virtuele machine repliceren naar Azure](azure-to-azure-walkthrough-enable-replication.md).


* Zie voor instructies over het beveiligen van Active Directory en DNS [beveiligen van Active Directory en DNS](site-recovery-active-directory.md) document.

* Zie voor instructies over het beveiligen van databaselaag uitgevoerd op de SQL server [SQL Server beveiligen](site-recovery-active-directory.md) document.

## <a name="networking-configuration"></a>Netwerkconfiguratie

Als u een statisch IP-adres gebruikt, kunt u de IP-adres dat u wilt dat de virtuele machine te laten worden. Als u wilt het IP-adres instellen, gaat u naar **berekening en netwerk instellingen** > **netwerkinterfacekaart**.

![Schermafbeelding van het instellen van een particuliere IP-adres in het deelvenster met Site Recovery Network interface-kaart](./media/site-recovery-sap/sap-static-ip.png)


## <a name="creating-a-recovery-plan"></a>Een herstelplan maken
Een herstelplan ondersteunt het sequentiëren van verschillende niveaus in een toepassing met meerdere lagen tijdens een failover. Sequentiëren helpt consistentie van de toepassing. Wanneer u een herstelplan voor een webtoepassing met meerdere lagen maakt, volledige de stappen beschreven in [een herstelplan maken met behulp van Site Recovery](site-recovery-create-recovery-plans.md).

### <a name="adding-virtual-machines-to-failover-groups"></a>Virtuele machines worden toegevoegd aan failover-groepen

1.  Een herstelplan maken door de toepassingsserver, web dispatcher en SAP centrale services VM's toe te voegen.
2.  Klik op 'Aanpassen' voor het groeperen van de virtuele machines. Standaard uitmaken alle virtuele machines deel van 'Groep 1'.



### <a name="add-scripts-to-the-recovery-plan"></a>Voeg scripts toe aan het herstelplan
Voor uw toepassingen goed te laten functioneren, moet u mogelijk dat er bepaalde bewerkingen op de virtuele Azure-machines na de failover of tijdens een testfailover. U kunt bepaalde na een failover-bewerkingen kunt automatiseren. U kunt bijvoorbeeld de DNS-vermelding bijwerken en bindingen en verbindingen met het bijbehorende scripts toevoegen aan het herstelplan wijzigen.


U kunt de meest gebruikte Azure Site Recovery-scripts implementeren in uw Automation-account te klikken op de knop 'Deploy to Azure' hieronder. Wanneer u een script voor gepubliceerde gebruikt, zorg ervoor dat u Volg de instructies in het script.

[![Implementeren in Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

1. Een script vóór actie toevoegen aan 'Groep 1' bij failover-SQL-beschikbaarheidsgroep. Het is gepubliceerd in de voorbeeldscripts 'ASR SQL FailoverAG'-script gebruiken. Zorg ervoor dat u Volg de instructies in het script en breng de gewenste wijzigingen in het script op de juiste wijze.
2. Een script post actie om te koppelen van een load balancer op de mislukte toevoegen via de virtuele machines van de weblaag (groep 1). Het is gepubliceerd in de voorbeeldscripts 'ASR AddSingleLoadBalancer'-script gebruiken. Zorg ervoor dat u Volg de instructies in het script en breng de gewenste wijzigingen in het script op de juiste wijze.

![Het herstelplan SAP](./media/site-recovery-sap/sap_recovery_plan.png)


## <a name="run-a-test-failover"></a>Een testfailover uitvoeren

1.  Selecteer in de Azure-portal de Recovery Services-kluis.
2.  Selecteer het herstelplan die u hebt gemaakt voor SAP-toepassingen.
3.  Selecteer **Testfailover**.
4.  U start de failover-test, selecteer het herstelpunt en het Azure-netwerk.
5.  Wanneer de secundaire-omgeving actief is, voert u validaties.
6.  Wanneer validaties voltooid zijn, om op te ruimen van de failover-omgeving, selecteer **opschonen testfailover**.

Zie voor meer informatie [testfailover naar Azure in Site Recovery](site-recovery-test-failover-to-azure.md).

## <a name="run-a-failover"></a>Een failover uitvoeren

1.  Selecteer in de Azure-portal de Recovery Services-kluis.
2.  Selecteer het herstelplan die u hebt gemaakt voor SAP-toepassingen.
3.  Selecteer **Failover**.
4.  U start de failover, selecteer het herstelpunt.

Zie voor meer informatie [-Failover in Site Recovery](site-recovery-failover.md).

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het bouwen van een noodherstel voor SAP NetWeaver implementaties met behulp van Site Recovery, het downloadbare witboek [SAP NetWeaver: bouwen van een noodherstel met Azure Site Recovery](http://aka.ms/asr-sap). Het witboek aanbevelingen voor diverse SAP-architecturen wordt beschreven, een lijst van ondersteunde toepassingen en VM-typen voor SAP op Azure en beschrijft testen plan opties voor uw noodherstel.
* Meer informatie over [repliceren van andere werkbelastingen](site-recovery-workload.md) met behulp van Site Recovery.
