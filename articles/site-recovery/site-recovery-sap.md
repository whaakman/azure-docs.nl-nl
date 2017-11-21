---
title: Een meerlaagse SAP NetWeaver toepassingsimplementatie met Azure Site Recovery beveiligen | Microsoft Docs
description: Dit artikel wordt beschreven voor het beveiligen van SAP NetWeaver toepassingsimplementaties met behulp van Azure Site Recovery
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: manayar
ms.openlocfilehash: 5a47acab598e113ef7ed968dd3a6429ac3bc1ec3
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="protect-a-multi-tier-sap-netweaver-application-deployment-using-azure-site-recovery"></a>Een meerlaagse SAP NetWeaver toepassingsimplementatie met Azure Site Recovery beveiligen

De meeste implementaties van grote en middelgrote SAP hebben enige vorm van oplossing voor herstel na noodgevallen.  Het belang van oplossingen voor robuuste en testable herstel na noodgevallen is toegenomen omdat meer kernprocessen worden verplaatst naar toepassingen zoals SAP.  Azure Site Recovery is getest en geïntegreerd met SAP-toepassingen, en de mogelijkheden van de meeste oplossingen on-premises herstel na noodgevallen, op een lagere totale eigendomskosten (TCO) dan concurrerende oplossingen overschrijdt.
Met Azure Site Recovery kunt u het volgende doen:
* Schakel beveiliging van SAP NetWeaver en niet-NetWeaver-productietoepassingen die on-premises worden uitgevoerd in door onderdelen naar Azure te repliceren.
* Schakel beveiliging van SAP NetWeaver en niet-NetWeaver-productietoepassingen die in Azure worden uitgevoerd in door onderdelen naar een ander Azure-datacenter te repliceren.
* Vereenvoudig de cloudmigratie door Site Recovery te gebruiken om uw SAP-implementatie te migreren naar Azure.
* Vereenvoudig SAP-projectupgrades, tests en het maken van prototypen door een on-demand een productiekloon te maken voor het testen van SAP-toepassingen.

Dit artikel wordt beschreven voor het beveiligen van SAP NetWeaver toepassingsimplementaties met behulp van [Azure Site Recovery](site-recovery-overview.md). In dit artikel bevat informatie over de aanbevolen procedures voor het beveiligen van een implementatie van de drie lagen SAP NetWeaver in Azure door repliceren naar een andere Azure-datacenter met Azure Site Recovery, de ondersteunde scenario's en configuraties, en het uitvoeren van failovers, beide failovers (noodhersteloefeningen) en een werkelijke failover testen.


## <a name="prerequisites"></a>Vereisten
Voordat u begint, zorg er dan voor dat u het volgende weten:

1. [Een virtuele machine repliceren naar Azure](azure-to-azure-walkthrough-enable-replication.md)
2. Hoe [ontwerpen van een netwerk herstel](site-recovery-azure-to-azure-networking-guidance.md)
3. [Een testfailover uitvoeren naar Azure](azure-to-azure-walkthrough-test-failover.md)
4. [U een failover naar Azure](site-recovery-failover.md)
5. Hoe [repliceren van een domeincontroller](site-recovery-active-directory.md)
6. Hoe [SQL-Server repliceren](site-recovery-sql.md)

## <a name="supported-scenarios"></a>Ondersteunde scenario's
Met Azure Site Recovery kunt u een oplossing voor het herstel na noodgevallen voor de volgende scenario's implementeren:
* SAP-systemen die zijn uitgevoerd in een Azure-datacenter repliceren naar een andere Azure-datacenter (Azure naar Azure DR), zoals ontworpen [hier](https://aka.ms/asr-a2a-architecture).
* SAP-systemen met op VMWare (of fysiek) servers on-premises repliceren naar een DR-site in een Azure-datacenter (VMware naar Azure DR), waarvoor een aantal extra onderdelen zoals ontworpen [hier](https://aka.ms/asr-v2a-architecture).
* SAP-systemen met op Hyper-V on-premises repliceren naar een DR-site in een Azure-datacenter (Hyper-V-Azure DR), waarvoor een aantal extra onderdelen zoals ontworpen [hier](https://aka.ms/asr-h2a-architecture).

Dit document maakt gebruik van het eerste geval - Azure naar Azure DR - ter illustratie van Azure Site Recovery SAP disaster recovery mogelijkheden. Als Azure Site Recovery replicatie networkdirect van toepassing is, wordt het proces beschreven om voor andere scenario's ook verwacht.

### <a name="required-foundation-services"></a>Vereiste foundation-services
Dit scenario documentatie alle is geïmplementeerd met de volgende foundation-services geïmplementeerd:
* ExpressRoute of Site-naar-Site virtueel particulier netwerk (VPN)
* Ten minste één Active Directory-domeincontroller en DNS-server worden uitgevoerd in Azure

Het wordt aanbevolen dat de infrastructuur die hierboven is gemaakt voordat u Azure Site Recovery implementeert.


## <a name="typical-sap-application-deployment"></a>Typische toepassing SAP-implementatie
Grote SAP-klanten wordt meestal tussen 6 en 20 afzonderlijke SAP-toepassingen implementeren.  De meeste van deze toepassingen zijn gebaseerd op de SAP NetWeaver ABAP of Java-engines.  Ondersteuning van deze toepassingen van de NetWeaver core zijn veel kleiner specifieke niet - NetWeaver SAP zelfstandige motoren en doorgaans een aantal niet-SAP-toepassingen.  

Het is essentieel inventarisatie van alle de SAP-toepassingen met een liggend en om te bepalen van de implementatiemodus (laag 2 of 3-laagse), versies, patches, grootte en verloop tarieven en schijfvereisten voor persistentie.

![Patroon van de implementatie](./media/site-recovery-sap/sap-typical-deployment.png)

De laag voor gegevenspersistentie van SAP-Database moet worden beveiligd via de systeemeigen DBMS-hulpprogramma's zoals SQL Server AlwaysOn, Oracle DataGuard of HANA System Replication. De clientlaag ook niet wordt beveiligd door Azure Site Recovery, maar het is belangrijk rekening houden met onderwerpen die invloed zijn op deze laag zoals DNS-servers vertraging, beveiliging en externe toegang tot het DR-datacenter.

Azure Site Recovery is de aanbevolen oplossing voor de toepassingslaag, met inbegrip van (A) SCS. Andere toepassingen zoals NetWeaver SAP-toepassingen en toepassingen niet SAP deel uitmaken van de algehele SAP-implementatie-omgeving en moet ook worden beveiligd met Azure Site Recovery.

## <a name="replicate-virtual-machines"></a>Virtuele machines repliceren
Ga als volgt [in deze richtlijnen](azure-to-azure-walkthrough-enable-replication.md) alle SAP toepassing virtuele machines repliceren naar het datacenter Azure DR starten.

Als u een statisch IP-adres gebruikt, kunt u het IP-adres dat u wilt dat de virtuele machine moet worden uitgevoerd in de sectie Network interface-kaarten in de berekenings- en -instellingen opgeven.

![Doel-IP](./media/site-recovery-sap/sap-static-ip.png)


## <a name="creating-a-recovery-plan"></a>Een herstelplan maken
Een herstelplan kunt de failover van de verschillende lagen in een toepassing met meerdere lagen, daarom toepassing consistentie van de sequentiëring. Volg de stappen beschreven [hier](site-recovery-create-recovery-plans.md) tijdens het maken van een herstelplan voor een webtoepassing met meerdere lagen.

### <a name="adding-scripts-to-the-recovery-plan"></a>Scripts toe te voegen aan het herstelplan
U moet wellicht enkele bewerkingen op het failovercluster van de virtuele Azure-machines na failover en testen voor uw toepassingen goed te laten functioneren. U kunt de failoverbewerking post zoals het bijwerken van DNS-vermelding en bindingen en verbindingen, wijzigen door de bijbehorende scripts toevoegen in het herstelplan, zoals beschreven in automatiseren [in dit artikel](site-recovery-create-recovery-plans.md#add-scripts).

### <a name="dns-update"></a>DNS-updates
Als de DNS-server is geconfigureerd voor dynamische DNS-updates, wordt de virtuele machines normaal gesproken moet u de DNS-server bijwerken met het nieuwe IP-zodra ze starten. Als u toevoegen van een expliciete stap wilt voor het bijwerken van DNS met het nieuwe IP-adressen van de virtuele machines en vervolgens voegt u dit [script voor het bijwerken van IP-adres in DNS](https://aka.ms/asr-dns-update) als een post-actie op herstel planningsgroepen.  

## <a name="example-azure-to-azure-deployment"></a>Voorbeeld van een Azure-Azure-implementatie
In het diagram hieronder de Azure Site Recovery Azure naar Azure DR wordt scenario beschreven:
* Het primaire Datacenter Singapore (Azure Zuidoost-Azië) en het datacenter DR Hongkong (Azure Oost-Azië).  In dit scenario wordt lokale beschikbaarheid verstrekt wanneer er twee virtuele machines waarop SQL Server AlwaysOn wordt uitgevoerd in synchrone modus in Singapore.
* Het bestandsshare ASC's kan worden gebruikt voor HA voor de SAP individuele foutpunten. File Share ASC's vereist een gedeelde clusterschijf geen en toepassingen zoals SIOS zijn niet vereist.
* DR-beveiliging voor de DBMS-laag wordt bereikt met behulp van asynchrone replicatie.
* Dit scenario bevat 'symmetrisch DR' – een term die wordt gebruikt om een DR-oplossing die een exacte replica van de productie te beschrijven, daarom de DR SQL Server-oplossing heeft voor lokale beschikbaarheid. Het gebruik van symmetrische Noodherstel is niet verplicht voor de Database-laag en veel klanten gebruikmaken van de flexibiliteit van cloudimplementaties voor het bouwen van een lokaal hoge Beschikbaarheidsknooppunt snel na een DR-gebeurtenis.
* Het diagram ziet u de SAP NetWeaver ASC's en toepassingen server laag gerepliceerd door Azure Site Recovery.

![Replicatiescenario](./media/site-recovery-sap/sap-replication-scenario.png)

## <a name="doing-a-test-failover"></a>Een testfailover uitvoeren
Ga als volgt [in deze richtlijnen](azure-to-azure-walkthrough-test-failover.md) een testfailover uitvoeren.

1.  Ga naar de Azure-portal en selecteer de Recovery Services-kluis.
2.  Klik op het herstelplan gemaakt voor SAP-toepassingen (s).
3.  Klik op 'Testfailover'.
4.  Selecteer het herstelpunt en virtuele Azure-netwerk om de test failover-proces te starten.
5.  Als de secundaire-omgeving is, kunt u uw validaties kunt uitvoeren.
6.  Zodra de validaties voltooid zijn, klikt u op op 'Opschonen testfailover' en voor het opschonen van de failover-omgeving.

## <a name="doing-a-failover"></a>U een failover uitvoert
Ga als volgt [in deze richtlijnen](site-recovery-failover.md) bij het uitvoeren van een failover.

1.  Ga naar de Azure-portal en selecteer de Recovery Services-kluis.
2.  Klik op het herstelplan gemaakt voor SAP-toepassingen.
3.  Klik op 'Failover'.
4.  Selecteer het herstelpunt om de failoverproces te starten.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het bouwen van een noodherstel voor SAP NetWeaver implementaties met Azure Site Recovery in [dit technisch document](http://aka.ms/asr-sap). In dit technisch document ook aanbevelingen voor verschillende SAP-architecturen wordt beschreven, een lijst van ondersteunde toepassingen en VM-typen voor SAP op Azure en beschrijft mogelijke Testplannen voor uw noodherstel.

Meer informatie over [repliceren van andere werkbelastingen](site-recovery-workload.md) met Site Recovery.
