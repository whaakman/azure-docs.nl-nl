---
title: Een meerlaagse SAP NetWeaver toepassingsimplementatie met Azure Site Recovery beveiligen | Microsoft Docs
description: In dit artikel wordt beschreven hoe het beveiligen van SAP NetWeaver toepassingsimplementaties met behulp van Azure Site Recovery.
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: manayar
ms.openlocfilehash: e2107177663163259d1f731717c4910bc986fc1f
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
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

In dit artikel gebruiken we een noodherstelscenario van Azure naar Azure ter illustratie van de mogelijkheden voor herstel na noodgevallen SAP van Site Recovery. Omdat de replicatie van Site Recovery niet toepassingsspecifieke, wordt het proces dat wordt beschreven verwacht gelden ook voor andere scenario's.

### <a name="required-foundation-services"></a>Vereiste foundation-services
In het scenario dat wordt besproken in dit artikel, worden de volgende foundation-services geïmplementeerd:
* Azure ExpressRoute of Azure VPN-Gateway
* Ten minste één Active Directory-domeincontroller en DNS-server, worden uitgevoerd in Azure

Het is raadzaam dat u deze infrastructuur maakt voordat u Site Recovery implementeert.

## <a name="typical-sap-application-deployment"></a>Typische toepassing SAP-implementatie
Grote SAP-klanten wordt meestal tussen 6 en 20 afzonderlijke SAP-toepassingen implementeren. De meeste van deze toepassingen zijn gebaseerd op SAP NetWeaver ABAP of Java-engines. Veel kleinere, specifieke niet - NetWeaver SAP zelfstandige motoren en meestal bepaalde niet-SAP-toepassingen, ondersteuning voor deze toepassingen van de NetWeaver core.  

Het is essentieel voor het inventariseren van alle SAP-toepassingen die worden uitgevoerd in uw omgeving. Bepaal vervolgens de implementatiemodus (twee lagen of drie lagen), versies, patches, grootten, verloop tarieven en schijfvereisten voor persistentie.

![Diagram van een doorsnee patroon voor SAP-implementatie](./media/site-recovery-sap/sap-typical-deployment.png)

De laag voor gegevenspersistentie database SAP beveiligen met systeemeigen DBMS-hulpprogramma's zoals SQL Server AlwaysOn, Oracle Data Guard of SAP HANA system-replicatie. De clientlaag is niet als de databaselaag SAP beveiligd door Site Recovery. Het is belangrijk rekening houden met factoren die van invloed zijn op deze laag. Factoren zijn DNS-servers vertraging, beveiliging en externe toegang tot het herstel na noodgevallen datacenter.

Site Recovery is de aanbevolen oplossing voor de toepassingslaag, inclusief voor SAP SCS en ASC's. Andere toepassingen, zoals NetWeaver SAP-toepassingen en niet-SAP-toepassingen, uitmaken deel van de algehele SAP-implementatie-omgeving. U moet ze beschermen met Site Recovery.

## <a name="replicate-virtual-machines"></a>Virtuele machines repliceren
Volg de instructies in voor het starten van alle SAP toepassing virtuele machines repliceren naar het datacenter van Azure disaster recovery [een virtuele machine repliceren naar Azure](azure-to-azure-walkthrough-enable-replication.md).

Als u een statisch IP-adres gebruikt, kunt u de IP-adres dat u wilt dat de virtuele machine te laten worden. Als u wilt het IP-adres instellen, gaat u naar **berekening en netwerk instellingen** > **netwerkinterfacekaart**.

![Schermafbeelding van het instellen van een particuliere IP-adres in het deelvenster met Site Recovery Network interface-kaart](./media/site-recovery-sap/sap-static-ip.png)

## <a name="create-a-recovery-plan"></a>Een herstelplan maken
Een herstelplan ondersteunt het sequentiëren van verschillende niveaus in een toepassing met meerdere lagen tijdens een failover. Sequentiëren helpt consistentie van de toepassing. Wanneer u een herstelplan voor een webtoepassing met meerdere lagen maakt, volledige de stappen beschreven in [een herstelplan maken met behulp van Site Recovery](site-recovery-create-recovery-plans.md).

### <a name="add-scripts-to-the-recovery-plan"></a>Voeg scripts toe aan het herstelplan
Voor uw toepassingen goed te laten functioneren, moet u mogelijk dat er bepaalde bewerkingen op de virtuele Azure-machines na de failover of tijdens een testfailover. U kunt bepaalde na een failover-bewerkingen kunt automatiseren. U kunt bijvoorbeeld de DNS-vermelding bijwerken en bindingen en verbindingen met het bijbehorende scripts toevoegen aan het herstelplan wijzigen.

### <a name="dns-update"></a>DNS-updates
Als DNS is geconfigureerd voor dynamische DNS-updates, bijwerken virtuele machines de DNS-server gewoonlijk met het nieuwe IP-adres wanneer ze worden gestart. Als u toevoegen van een expliciete stap wilt in de DNS bijwerken met het nieuwe IP-adressen van de virtuele machines, het toevoegen van een [script voor het bijwerken van het IP-adres in DNS](https://aka.ms/asr-dns-update) als een actie na een failover in recovery planningsgroepen.  

## <a name="example-azure-to-azure-deployment"></a>Voorbeeld van een Azure-Azure-implementatie
Het volgende diagram toont de noodherstelscenario van Site Recovery Azure naar Azure:

![Diagram van een replicatiescenario Azure naar Azure](./media/site-recovery-sap/sap-replication-scenario.png)

* Het primaire datacenter wordt Singapore (Azure Zuidoost-Azië). Het herstel na noodgevallen datacenter wordt Hongkong (Azure Oost-Azië). In dit scenario wordt lokale beschikbaarheid verstrekt door twee virtuele machines waarop SQL Server AlwaysOn wordt uitgevoerd in synchrone modus in Singapore.
* De bestandsshare SAP ASC's biedt maximale beschikbaarheid voor de SAP individuele foutpunten. De bestandsshare ASC's nodig niet een cluster gedeelde schijf. Toepassingen zoals SIOS zijn niet vereist.
* Disaster recovery-beveiliging voor de DBMS-laag wordt bereikt door middel van asynchrone replicatie.
* Dit scenario bevat 'symmetrisch noodherstel'. Deze term beschrijft een noodherstel die een exacte replica van de productie. Het herstel na noodgevallen SQL Server-oplossing heeft lokale beschikbaarheid. Symmetrisch noodherstel is niet verplicht voor de databaselaag. Veel klanten profiteren van de flexibiliteit van cloudimplementaties voor een lokale beschikbaarheidsknooppunt snel bouwen na een herstel na noodgevallen gebeurtenis.
* Het diagram ziet u de SAP NetWeaver ASC's en de server toepassingslaag gerepliceerd door Site Recovery.

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
