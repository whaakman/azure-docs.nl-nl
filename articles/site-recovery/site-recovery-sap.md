---
title: Herstel na noodgevallen voor de implementatie van meerdere lagen SAP NetWeaver-app met Azure Site Recovery instellen | Microsoft Docs
description: Dit artikel wordt beschreven hoe u herstel na noodgevallen voor implementaties van SAP NetWeaver-toepassingen met Azure Site Recovery kunt instellen.
author: asgang
manager: rochakm
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: asgang
ms.openlocfilehash: 68efc039c5de5d7f61b7ce34e74c6c2cf4bad027
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56670873"
---
# <a name="set-up-disaster-recovery-for-a-multi-tier-sap-netweaver-app-deployment"></a>Herstel na noodgevallen voor een implementatie met meerdere lagen SAP NetWeaver-app instellen

De meeste grote en middelgrote SAP-implementaties gebruiken een vorm van een oplossing voor noodherstel. Het belang van oplossingen voor herstel na noodgevallen voor robuuste en testable is toegenomen omdat meer bedrijfsprocessen worden verplaatst naar toepassingen als SAP. Azure Site Recovery is getest en geïntegreerd met SAP-toepassingen. Site Recovery is groter dan de mogelijkheden van de meeste on-premises disaster recovery-oplossingen, en op een lagere totale eigendomskosten (TCO) dan concurrerende oplossingen.

Met Site Recovery, kunt u het volgende doen:
* **Schakel de beveiliging van SAP NetWeaver en niet-NetWeaver-productietoepassingen die on-premises uitvoert** door onderdelen te repliceren naar Azure.
* **Schakel de beveiliging van SAP NetWeaver en niet-NetWeaver-productietoepassingen die worden uitgevoerd op Azure** door onderdelen te repliceren naar een andere Azure-datacenter.
* **Vereenvoudig de cloudmigratie** met behulp van Site Recovery voor het migreren van uw SAP-implementatie naar Azure.
* **Vereenvoudig SAP-projectupgrades, testen en ontwikkelen van prototypen** door het maken van een productie klonen op aanvraag voor het testen van SAP-toepassingen.

In dit artikel wordt beschreven hoe u implementaties van SAP NetWeaver-toepassingen beveiligen met behulp van [Azure Site Recovery](site-recovery-overview.md). Het artikel bevat informatie over aanbevolen procedures voor het beveiligen van een implementatie van de drie lagen SAP NetWeaver op Azure door te repliceren naar een andere Azure-datacenter met Site Recovery. Beschrijft de ondersteunde scenario's en configuraties en het uitvoeren van testfailovers (noodhersteloefeningen) en een werkelijke failover.

## <a name="prerequisites"></a>Vereisten
Voordat u begint, zorg ervoor dat u weet hoe u de volgende taken uitvoeren:

* [Een virtuele machine repliceren naar Azure](azure-to-azure-walkthrough-enable-replication.md)
* [Een herstelnetwerk ontwerpen](site-recovery-azure-to-azure-networking-guidance.md)
* [Voer een failovertest uit naar Azure](azure-to-azure-walkthrough-test-failover.md)
* [Voer een failover naar Azure](site-recovery-failover.md)
* [Een domeincontroller worden gerepliceerd](site-recovery-active-directory.md)
* [SQL Server repliceren](site-recovery-sql.md)

## <a name="supported-scenarios"></a>Ondersteunde scenario's
U kunt Site Recovery gebruiken voor het implementeren van een oplossing voor herstel na noodgevallen in de volgende scenario's:
* SAP-systemen die worden uitgevoerd in een Azure-datacenter die worden gerepliceerd naar een andere Azure-datacenter (noodherstel van Azure naar Azure). Zie voor meer informatie, [Azure-naar-Azure-replicatiearchitectuur](https://aka.ms/asr-a2a-architecture).
* SAP-systemen met op VMware (of fysieke) servers on-premises die repliceren naar een site voor noodherstel in een Azure-datacenter (noodherstel van VMware naar Azure). Dit scenario moet enkele extra onderdelen. Zie voor meer informatie, [VMware-naar-Azure-replicatiearchitectuur](https://aka.ms/asr-v2a-architecture).
* SAP-systemen die worden uitgevoerd op Hyper-V on-premises die worden gerepliceerd naar een site voor noodherstel in een Azure-datacenter (Hyper-V-naar-Azure-noodherstel). Dit scenario moet enkele extra onderdelen. Zie voor meer informatie, [Hyper-V-naar-Azure-replicatiearchitectuur](https://aka.ms/asr-h2a-architecture).

In dit artikel gebruiken we een **Azure naar Azure** noodherstelscenario ter illustratie van de mogelijkheden voor herstel na noodgevallen SAP van Site Recovery. Omdat de Site Recovery-replicatie niet toepassingsspecifieke, wordt het proces dat wordt beschreven verwacht gelden ook voor andere scenario's.

### <a name="required-foundation-services"></a>Vereiste foundation services
In het scenario dat in dit artikel we bespreken worden de volgende foundation-services geïmplementeerd:
* Met Azure ExpressRoute of Azure VPN-Gateway
* Ten minste één Active Directory-domeincontroller en DNS-server, die worden uitgevoerd in Azure

Het is raadzaam dat u deze infrastructuur maakt voordat u Site Recovery implementeren.

## <a name="reference-sap-application-deployment"></a>Naslaginformatie over SAP-toepassingsimplementatie

Deze referentiearchitectuur toont het uitvoeren van SAP NetWeaver in een Windows-omgeving op Azure met hoge beschikbaarheid.  Deze architectuur wordt geïmplementeerd met grootten voor specifieke virtuele machine (VM) die kunnen worden gewijzigd om te voldoen aan de behoeften van uw organisatie.

![Diagram van een doorsnee patroon voor SAP-implementatie](./media/site-recovery-sap/sap-netweaver_latest.png)

## <a name="disaster-recovery-considerations"></a>Overwegingen voor herstel na noodgevallen

Voor herstel na noodgevallen (DR), moet u een failover uitvoeren naar een secundaire regio kunnen zijn. Voor elke laag wordt een andere strategie gebruikt om DR (Disaster Recovery, herstel na noodgeval) te bieden.

#### <a name="vms-running-sap-web-dispatcher-pool"></a>Virtuele machines met SAP-Webdispatcher van toepassingen 
Het onderdeel Web Dispatcher wordt gebruikt als een load balancer voor SAP-verkeer tussen de SAP-toepassingsservers. Azure Load Balancer wordt voor het bereiken van hoge beschikbaarheid voor het onderdeel Web Dispatcher gebruikt voor het implementeren van de parallelle Webdispatcher-installatie in een round robin-configuratie voor de distributie van HTTP (S)-verkeer tussen de beschikbare Web coördinatoren in de groep balancer. Dit zal worden gerepliceerd met behulp van Azure Site Recovery (ASR) en automatiseringsscripts wordt gebruikt voor het configureren van load balancer op de Dr-regio. 

#### <a name="vms-running-application-servers-pool"></a>Virtuele machines met servers van toepassingen
Voor het beheren van aanmeldingsgroepen voor ABAP-toepassingsservers, wordt de transactie SMLG gebruikt. De load balancer-functie binnen de berichtenserver van de centrale Services wordt gebruikt voor het distribueren van werkbelasting tussen SAP-toepassingen voor servers voor SAPGUIs en RFC verkeer. Dit zal worden gerepliceerd met behulp van Azure Site Recovery 

#### <a name="vms-running-sap-central-services-cluster"></a>Virtuele machines met SAP Central Services cluster
Deze referentiearchitectuur centrale Services worden uitgevoerd op virtuele machines in de toepassingslaag. De centrale Services is een potentieel single point of failure (SPOF) bij de implementatie op een enkele virtuele machine, normale implementatie als hoge beschikbaarheid geen vereiste is.<br>

Voor het implementeren van een oplossing voor hoge beschikbaarheid, kan een gedeelde schijfcluster of een cluster van de bestandsshare worden gebruikt. Voor meer informatie over het configureren van virtuele machines voor een gedeelde schijfcluster gebruik van Windows Server-failovercluster. Cloud-Witness wordt aanbevolen als een quorumwitness. 
 > [!NOTE]
 > Azure Site Recovery repliceert niet de cloud-witness wordt daarom aanbevolen voor het implementeren van de cloud-witness in de Dr-regio.

Ter ondersteuning van de failover-clusteromgeving [SIOS DataKeeper Cluster Edition](https://azuremarketplace.microsoft.com/marketplace/apps/sios_datakeeper.sios-datakeeper-8) het cluster shared volume-functie wordt uitgevoerd door onafhankelijke schijven die eigendom zijn van de clusterknooppunten te repliceren. Azure biedt geen systeemeigen ondersteuning voor gedeelde schijven en daarom vereist SIOS-oplossingen. 

Een andere manier om af te handelen clustering is het implementeren van een share bestandscluster. [SAP](https://blogs.sap.com/2018/03/19/migration-from-a-shared-disk-cluster-to-a-file-share-cluster) het patroon van de implementatie Central Services voor toegang tot de algemene mappen /sapmnt via een UNC-pad die onlangs zijn gewijzigd. Het is echter nog steeds aanbevolen om ervoor te zorgen dat de /sapmnt UNC-share maximaal beschikbaar is. Dit kan worden gedaan op de centrale Services-exemplaar met behulp van Windows Server Failover Cluster met Scale Out bestandsserver (SOFS) en de Storage Spaces Direct (S2D)-functie in Windows Server 2016. 
 > [!NOTE]
 > Ondersteuning voor Azure Site Recovery vastloopt op dit moment alleen toepassingsconsistente punt replicatie van virtuele machines met behulp van storage spaces direct- en passieve knooppunt van SIOS Datakeeper


## <a name="disaster-recovery-considerations"></a>Overwegingen voor herstel na noodgeval

U kunt Azure Site Recovery gebruiken voor het indelen van de failover via van de volledige SAP-implementatie van Azure-regio's.
Hieronder vindt u de stappen voor het instellen van herstel na noodgevallen 

1. Virtuele machines repliceren 
2. Een herstelnetwerk ontwerpen
3.  Een domeincontroller worden gerepliceerd
4.  De basislaag gegevens repliceren 
5.  Een testfailover uitvoeren 
6.  Een failover uitvoeren 

Hieronder ziet u de aanbeveling voor herstel na noodgeval voor elke laag in dit voorbeeld gebruikt. 

 **SAP-lagen** | **Aanbeveling**
 --- | ---
**SAP-Webdispatcher pool** |  Repliceren met Site recovery 
**SAP-server-groep van toepassingen** |  Repliceren met Site recovery 
**SAP Central Services cluster** |  Repliceren met Site recovery 
**Active directory virtuele machines** |  Active directory-replicatie 
**SQL database-servers** |  SQL AlwaysOn replicatie

## <a name="replicate-virtual-machines"></a>Virtuele machines repliceren

Als u wilt beginnen met het repliceren van alle SAP-toepassing virtuele machines naar het datacenter van Azure disaster recovery, volg de instructies in [een virtuele machine repliceren naar Azure](azure-to-azure-walkthrough-enable-replication.md).


* Raadpleeg voor meer informatie over het beveiligen van Active Directory en DNS [beveiligen van Active Directory en DNS](site-recovery-active-directory.md) document.

* Raadpleeg voor meer informatie over het beveiligen van databaselaag wordt uitgevoerd in SQL server [SQL-Server beveiligen](site-recovery-active-directory.md) document.

## <a name="networking-configuration"></a>Netwerkconfiguratie

Als u een statisch IP-adres gebruikt, kunt u de IP-adres dat u wilt dat de virtuele machine op te nemen. Om het IP-adres, gaat u naar **berekening en netwerk instellingen** > **netwerkinterfacekaart**.

![Schermopname die laat zien hoe u een privé IP-adres instellen in het deelvenster met Site Recovery Network interface-kaart](./media/site-recovery-sap/sap-static-ip.png)


## <a name="creating-a-recovery-plan"></a>Het maken van een herstelplan
Een herstelplan ondersteunt de sequentiëren van de verschillende lagen in een toepassing met meerdere lagen tijdens een failover. Sequentiëren helpt consistentie van de toepassing. Wanneer u een plan voor herstel voor een webtoepassing met meerdere lagen maakt, voltooid de stappen beschreven in [een herstelplan maken met behulp van Site Recovery](site-recovery-create-recovery-plans.md).

### <a name="adding-virtual-machines-to-failover-groups"></a>VM's toevoegt aan de failover-groepen

1.  Maak een plan voor herstel door toe te voegen van de toepassingsserver, -webdispatcher en SAP Central services VM's.
2.  Klik op 'Aanpassen' aan de groep van de virtuele machines. Standaard worden alle virtuele machines deel uit van 'Groep 1'.



### <a name="add-scripts-to-the-recovery-plan"></a>Voeg scripts toe aan het herstelplan te gaan
Voor uw toepassingen goed te laten functioneren, moet u bepaalde bewerkingen op de virtuele Azure-machines uitvoeren na de failover of tijdens een test-failover. U kunt bepaalde bewerkingen na een failover automatiseren. U kunt bijvoorbeeld de DNS-vermelding bijwerken en bindingen en verbindingen wijzigen door de bijbehorende scripts toevoegen aan het herstelplan te gaan.


U kunt de meestgebruikte scripts voor de Azure Site Recovery implementeren in uw Automation-account te klikken op de knop 'Implementeren in Azure' hieronder. Wanneer u een script voor gepubliceerde gebruikt, zorg ervoor dat u Volg de instructies in het script.

[![Implementeren in Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

1. Een actie voorafgaand aan de-script toevoegen aan 'Groep 1' bij failover-SQL-beschikbaarheidsgroep. Gebruik het script 'ASR-SQL-FailoverAG' gepubliceerd in de voorbeelden van scripts. Zorg ervoor dat u Volg de instructies in het script en breng de benodigde wijzigingen in het script op de juiste wijze.
2. Toevoegen van een script van de actie bericht als u wilt koppelen van een load balancer op de failover van virtuele machines van de weblaag (groep 1). Gebruik het script 'ASR-AddSingleLoadBalancer' gepubliceerd in de voorbeelden van scripts. Zorg ervoor dat u Volg de instructies in het script en breng de benodigde wijzigingen in het script op de juiste wijze.

![Herstelplan voor SAP](./media/site-recovery-sap/sap_recovery_plan.png)


## <a name="run-a-test-failover"></a>Een testfailover uitvoeren

1.  Selecteer uw Recovery Services-kluis in de Azure-portal.
2.  Selecteer het herstelplan te gaan die u hebt gemaakt voor SAP-toepassingen.
3.  Selecteer **Failover testen**.
4.  Selecteer het herstelpunt en de Azure-netwerk voor het starten van de test-failover-proces.
5.  Wanneer de secundaire-omgeving is, validatieprocedures.
6.  Wanneer validaties voltooid zijn, voor het opschonen van de failover-omgeving, selecteer **failovertest**.

Zie voor meer informatie, [testfailover naar Azure in Site Recovery](site-recovery-test-failover-to-azure.md).

## <a name="run-a-failover"></a>Een failover uitvoeren

1.  Selecteer uw Recovery Services-kluis in de Azure-portal.
2.  Selecteer het herstelplan te gaan die u hebt gemaakt voor SAP-toepassingen.
3.  Selecteer **Failover**.
4.  Selecteer het herstelpunt dat voor het starten van het failoverproces.

Zie voor meer informatie, [Failover in Site Recovery](site-recovery-failover.md).

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het bouwen van een oplossing voor noodherstel voor SAP NetWeaver-implementaties met behulp van Site Recovery, de downloadbare whitepaper [SAP NetWeaver: Het bouwen van een oplossing voor noodherstel met Azure Site Recovery](https://aka.ms/asr-sap). Het technische document worden besproken aanbevelingen voor verschillende SAP-architecturen, geeft een lijst van ondersteunde toepassingen en VM-typen voor SAP op Azure en beschrijft testen plan opties voor uw oplossing voor noodherstel.
* Meer informatie over [replicatie van andere workloads](site-recovery-workload.md) met behulp van Site Recovery.
