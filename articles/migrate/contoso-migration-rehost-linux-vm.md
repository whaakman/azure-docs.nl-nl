---
title: Rehost migreren en een on-premises Linux-app op Azure-VM's opnieuw hosten | Microsoft Docs
description: Meer informatie over hoe een on-premises Linux-app in Contoso opnieuw hosten door te migreren naar Azure-VM's.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: raynew
ms.openlocfilehash: 4378351d6da6943485b7f382d96094543bfffa23
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297720"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms"></a>Migratie van Contoso: een Linux-app van de on-premises naar Azure VM's opnieuw hosten

In dit artikel leest u hoe Contoso opnieuw hosten een on-premises service op basis van Linux helpdesk-app (**osTicket**), naar Azure IaaS VM's.

Dit document is een in een reeks artikelen waarin hoe het fictieve bedrijf Contoso de on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en een set van scenario's die laten zien hoe u een migratie-infrastructuur instellen en uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit. Aanvullende artikelen gaan we toevoegen na verloop van tijd.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Overzicht van de serie artikelen, strategie voor de migratie van Contoso en de voorbeeld-apps die worden gebruikt in de reeks. | Beschikbaar
[Artikel 2: Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Contoso bereidt u de on-premises infrastructuur en de Azure-infrastructuur voor migratie. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen in de reeks. | Beschikbaar
[Artikel 3: Evalueer on-premises bronnen voor migratie naar Azure](contoso-migration-assessment.md)  | Contoso wordt uitgevoerd een evaluatie van de on-premises SmartHotel360-app die wordt uitgevoerd op VMware. Contoso beoordeelt virtuele machines van app met behulp van de Azure Migrate-service en de app SQL Server-database met behulp van Data Migration Assistant. | Beschikbaar
[Artikel 4: Opnieuw hosten van een app op een Azure-VM en het beheerde exemplaar van SQL Database](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso wordt een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel360-app uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso de app-database migreert naar een Azure SQL Database Managed Instance met de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar   
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | De VM's van de SmartHotel360-app migreert Contoso naar Azure-VM's met behulp van de Site Recovery-service. | Beschikbaar
[Artikel 6: Een app op Azure Virtual machines en in een SQL Server AlwaysOn-beschikbaarheidsgroep opnieuw hosten](contoso-migration-rehost-vm-sql-ag.md) | Contoso migreert de SmartHotel360-app. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app. De Database Migration Service wordt gebruikt voor het migreren van de app-database naar een SQL Server-cluster dat wordt beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar 
Artikel 7: Een Linux-app op Azure VM's opnieuw hosten | Contoso een lift-and-shift-migratie van de app van de osTicket Linux Azure-virtuele machines, met behulp van Azure Site Recovery is voltooid | In dit artikel
[Artikel 8: Een Linux-app op Azure VM's en Azure MySQL Rehost](contoso-migration-rehost-linux-vm-mysql.md) | Contoso de osTicket Linux app overzet naar virtuele Azure-machines met Azure Site Recovery en de app-database migreert naar een Azure MySQL-Server-exemplaar met behulp van MySQL Workbench. | Beschikbaar
[Artikel 9: Een app op Azure Web Apps en Azure SQL database herstructureren](contoso-migration-refactor-web-app-sql.md) | Contoso de SmartHotel360-app is gemigreerd naar een Azure-Web-App en de app-database migreert naar een Azure SQL Server-exemplaar met Database Migration Assistant | Beschikbaar
[Artikel 10: Een Linux-app op Azure-Web-Apps en Azure MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | De Linux-app osTicket migreert Contoso naar een Azure-web-app op meerdere Azure-regio's met behulp van Azure Traffic Manager, geïntegreerd met GitHub voor continue levering. Contoso migreert de app-database naar een Azure Database for MySQL-exemplaar. | Beschikbaar 
[Artikel 11: Herstructureren TFS op Azure DevOps-Services](contoso-migration-tfs-vsts.md) | Contoso migreert de on-premises Team Foundation Server-implementatie naar Azure DevOps-Services in Azure. | Beschikbaar
[Artikel 12: Opnieuw ontwerpen van een app op Azure-containers en Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | De app SmartHotel migreert Contoso naar Azure. Vervolgens rearchitects wordt de weblaag app als een Windows-container die wordt uitgevoerd in Azure Service Fabric en de database met Azure SQL Database. | Beschikbaar
[Artikel 13: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Contoso databasebeschadiging zijn SmartHotel-app met behulp van een scala aan mogelijkheden van Azure en services, waaronder Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services en Azure Cosmos DB... | Beschikbaar



In dit artikel, Contoso de twee lagen worden gemigreerd **osTicket** app, die wordt uitgevoerd op Linux Apache MySQL PHP (LAMP) naar Azure. De VM's van de app worden, gemigreerd met behulp van de Azure Site Recovery-service. Als u wilt deze open-source-app wilt gebruiken, kunt u het downloaden van [GitHub](https://github.com/osTicket/osTicket).

## <a name="business-drivers"></a>Zakelijke drijfveren

Het team voor IT-leidinggevenden heeft nauw samengewerkt met zakelijke partners om te begrijpen wat ze willen bereiken bij deze migratie:

- **Adres van de groei van het bedrijf**: Contoso groeit en er is als gevolg hiervan druk op de on-premises systemen en de infrastructuur.
- **Beperk risico**: de service-app helpdesk is essentieel voor het bedrijf Contoso. Contoso wil verplaatsen naar Azure met nul risico.
- **Uitbreiden**: Contoso niet wilt dat de app nu wijzigen. Het gewoon wil om ervoor te zorgen dat de app stabiel is.


## <a name="migration-goals"></a>Migratie-doelen

Het Contoso-cloud-team heeft vastgemaakt omlaag doelstellingen voor deze migratie, om te bepalen van de beste migratiemethode:

- Na de migratie, moet de app in Azure dezelfde prestatiemogelijkheden hebben als in hun on-premises VMWare-omgeving.  De app blijven als kritiek in de cloud als on-premises. 
- Contoso wil niet investeren in deze app.  Het is belangrijk voor het bedrijf, maar in de huidige vorm Contoso gewoon wil deze veilig naar de cloud verplaatsen.
- Contoso wilt niet wijzigen van de ops-model voor deze app. Het bedrijf wil communiceren met de app in de cloud op dezelfde manier als nu.
- Contoso wilt niet wijzigen van app-functionaliteit. Alleen de locatie van de app wordt gewijzigd.
- Nadat een aantal migraties van Windows-app is voltooid, wil Contoso voor meer informatie over het gebruik van een infrastructuur op basis van Linux in Azure.


## <a name="solution-design"></a>Het ontwerp van oplossing

Na het vastmaken omlaag doelstellingen en vereisten, Contoso ontwerpen van een implementatieoplossing controleren en identificeert het migratieproces, met inbegrip van de Azure-services die Contoso voor de migratie wilt gebruiken.

### <a name="current-app"></a>Huidige app

- De OSTicket-app is gelaagd over twee virtuele machines (**OSTICKETWEB** en **OSTICKETMYSQL**).
- De virtuele machines bevinden zich op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5).
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacenter (**contoso-datacenter**), met een on-premises domeincontroller (**contosodc1**)

### <a name="proposed-architecture"></a>Voorgestelde architectuur

- Omdat de app een productie-werkbelasting is, de virtuele machines in Azure bevindt zich in de resourcegroep van de productie **ContosoRG**.
- De virtuele machines worden gemigreerd naar de primaire regio (VS-Oost 2) en worden in de productienetwerk (VNET-PROD-EUS2):
    - De web virtuele machine bevindt zich in het frontendsubnet (PROD-FE-EUS2).
    - De virtuele machine van de database bevindt zich in de database-subnet (PROD-DB-EUS2).
- De on-premises machines in het Contoso-datacenter wordt buiten gebruik gesteld nadat de migratie is voltooid.

![Scenario-architectuur](./media/contoso-migration-rehost-linux-vm/architecture.png) 

### <a name="solution-review"></a>Beoordelingen van oplossing

Contoso evalueert het ontwerp van de voorgestelde door het samenstellen van een lijst met voor- en nadelen.

**Overweging** | **Details**
--- | ---
**Professionals** | Zowel de virtuele machines van app worden verplaatst naar Azure zonder wijzigingen, waardoor de migratie eenvoudig.<br/><br/> Omdat Contoso lift-and-shift voor zowel virtuele machines van de app gebruikt is, zijn geen speciale configuratie of de migratie van hulpprogramma's voor de database van de app nodig.<br/><br/> Contoso, behouden die volledige controle over de virtuele machines in Azure-app. </br>/br > Ubuntu 16.04-TLS, dit is een onderschreven Linux-distributie van de app virtuele machines worden uitgevoerd. [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).
**Nadelen** | De lagen web en gegevens van de app blijft een single point of failover. <br/><br/> Contoso moet doorgaan met ondersteuning van de app als Azure-VM's in plaats van verplaatsen naar een beheerde service zoals Azure App Service en Azure Database voor MySQL.<br/><br/> Contoso is op de hoogte dat houdt dingen eenvoudig met een virtuele machine lift-and-shift-migratie, ze niet u maximaal van de functies van profiteert zijn [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/overview) (ingebouwde hoge beschikbaarheid, voorspelbare prestaties eenvoudig schalen, automatische back-ups en ingebouwde beveiliging).

### <a name="migration-process"></a>Migratieproces

Contoso worden als volgt gemigreerd:

1. Als een eerste stap Contoso stelt u de Azure en on-premises infrastructuur die nodig zijn voor het implementeren van Site Recovery.
2. Na het voorbereiden van Azure en on-premises onderdelen, Contoso ingesteld en Hiermee schakelt u replicatie voor de virtuele machines.
3. Nadat de replicatie werkt, Contoso worden gemigreerd van de virtuele machines met failover naar Azure.

![Migratieproces](./media/contoso-migration-rehost-linux-vm/migration-process.png)

### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | De service coördineert en beheert de migratie en herstel na noodgevallen voor Azure-VM's, en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, rekening Azure Storage-kosten in gebracht.  Azure-VM's worden gemaakt en kosten, wanneer een failover optreedt. [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) kosten in rekening gebracht en prijzen.

 
## <a name="prerequisites"></a>Vereiste onderdelen

Hier volgt wat Contoso nodig heeft voor dit scenario.

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | Abonnementen Contoso gemaakt in een eerdere artikel in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-infrastructuur** | Instellen van hun Azure-infrastructuur, zoals beschreven in Contoso [Azure-infrastructuur voor migratie](contoso-migration-infrastructure.md).<br/><br/> Meer informatie over specifieke [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) vereisten voor Site Recovery.
**On-premises servers** | De on-premises vCenter-server moet versie 5.5, 6.0 of 6.5 worden uitgevoerd<br/><br/> Een ESXi-host waarop versie 5.5, 6.0 of 6.5<br/><br/> Een of meer virtuele VMware-machines die worden uitgevoerd op de ESXi-host.
**On-premises VM 's** | [Linux-machines controleren](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) die worden ondersteund voor migratie met Site Recovery.<br/><br/> Controleer of ondersteund [Linux-systemen voor bestands- en opslagservices](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage).<br/><br/> Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Contoso de migratie wordt voltooid:

> [!div class="checklist"]
> * **Stap 1: Azure voorbereiden voor Site Recovery**: Contoso-maakt een Azure storage-account voor het opslaan van gerepliceerde gegevens en maakt u een Recovery Services-kluis.
> * **Stap 2: On-premises VMware voorbereiden voor Site Recovery**: Contoso-accounts moet worden gebruikt voor VM-detectie- en agent-installatie wordt voorbereid en verbinding maken met virtuele Azure-machines na een failover wordt voorbereid.
> * **Stap 3:-Machines repliceren**: Contoso stelt u de bron- en migratie-omgeving, maakt u een replicatiebeleid en start virtuele machines te repliceren naar Azure storage.
> * **Stap 4: Migreer de virtuele machines met Site Recovery**: Contoso een testfailover uit om te controleren of alles werkt en voert vervolgens een volledige failover voor de virtuele machines migreren naar Azure wordt uitgevoerd.


## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Stap 1: Azure voorbereiden voor de Site Recovery-service

Contoso heeft een aantal Azure-onderdelen nodig voor Site Recovery:

- Een nieuw Azure storage-account voor het opslaan van gerepliceerde gegevens. 
- Een Recovery Services-kluis in Azure.
- Failover van een VNet waarin resources zich bevinden. Contoso is al gemaakt het VNet tijdens [implementatie van de Azure-infrastructuur](contoso-migration-infrastructure.md), zodat ze alleen wilt maken van een storage-account en de kluis.

1. Beheerders van Contoso maken Azure storage-account (contosovmsacc20180528) in de regio VS-Oost 2.

    - Het opslagaccount moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
    - Ze een algemeen gebruik-account gebruiken met de standard-opslag en LRS-replicatie.

    ![Site Recovery-opslag](./media/contoso-migration-rehost-linux-vm/asr-storage.png)

2. Met de netwerk- en storage-account in plaats ze een kluis (ContosoMigrationVault) maken en plak deze in de **ContosoFailoverRG** resourcegroep in de primaire regio in de VS-Oost 2.

    ![Recovery Services-kluis](./media/contoso-migration-rehost-linux-vm/asr-vault.png)


**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) instellen van Azure voor Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Stap 2: On-premises VMware voorbereiden voor Site Recovery

Beheerders van Contoso bereid de on-premises VMware-infrastructuur als volgt:

- Een account maken ze op de vCenter-server of vSphere ESXi-host voor het automatiseren van VM-detectie.
- Het maken van een account waarmee de automatische installatie van de Mobility-service op VMware-VM's die u wilt repliceren.
- Ze bereid on-premises VM's, zodat ze verbinding met Azure-VM's maken kunnen wanneer ze worden gemaakt na de migratie.


### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Virtuele machines automatisch detecteren. Er is minimaal een alleen-lezen-account vereist.
- Replicatie, failover en failback orkestreren. U moet een account die bewerkingen zoals het maken en verwijderen van schijven en het inschakelen van virtuele machines kan worden uitgevoerd.

Contoso-beheerders het account is als volgt instellen:

1. Een rol maken ze op de vCenter-niveau.
2. Ze toewijzen aan die rol op de vereiste machtigingen.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op de Linux-VM's die worden gemigreerd.

- Site Recovery kan een automatische push-installatie van dit onderdeel kunt doen wanneer replicatie is ingeschakeld voor virtuele machines.
- Voor automatische push-installatie moeten ze een account voorbereiden waarmee Site Recovery wordt gebruikt voor toegang tot de virtuele machines.
- Details van de accounts zijn ingevoerd tijdens de installatie van de replicatie. 
- Het account kan worden domein of lokale account met machtigingen om te installeren op virtuele machines.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Na een failover naar Azure wil Contoso kunnen verbinding maken met de gerepliceerde virtuele machines in Azure. Om dit te doen, is er een aantal dingen die de Contoso-beheerders moet uitvoeren:

- Voor toegang tot Azure-VM's via internet, kunnen SSH in Linux on-premises virtuele machine vóór de migratie.  Voor Ubuntu dit kan worden voltooid met de volgende opdracht: **Sudo apt-get ssh in installeren -y**.
- Nadat ze de migratie (failover) worden uitgevoerd, kunnen ze controleren **diagnostische gegevens over opstarten** om een schermopname van de virtuele machine weer te geven.
- Als dit niet werkt, moet ze om te controleren dat de virtuele machine wordt uitgevoerd, en bekijk deze [tips voor probleemoplossing](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) maken en toewijzen van een rol voor automatische detectie.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) het maken van een account voor push-installatie van de Mobility-service.


## <a name="step-3-replicate-the-on-premises-vms"></a>Stap 3: De on-premises VM's repliceren

Voordat ze het web virtuele machine naar Azure migreren kunnen, kunnen beheerders van Contoso instellen en replicatie inschakelen.

### <a name="set-a-protection-goal"></a>Stel een beveiligingsdoel

1. In de kluis onder de kluisnaam (ContosoVMVault) ze ingesteld een replicatiedoel (**aan de slag** > **siteherstel** > **infrastructuur voorbereiden**.
2. Ze opgeven dat hun computers zich op locatie bevinden, dat ze virtuele VMware-machines, en dat ze wilt repliceren naar Azure.
    ![Replicatiedoel](./media/contoso-migration-rehost-linux-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

Als u wilt doorgaan, ze bevestigen dat zij implementatieplanning, door te selecteren hebt voltooid **Ja, heb ik gedaan**. Contoso zijn slechts één virtuele machine in dit scenario wordt gemigreerd, en hoeft-implementatie plannen.

### <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Beheerders van Contoso moet nu de bronomgeving configureren. U doet dit door ze een OVF-sjabloon downloaden en gebruiken om te implementeren van de Site Recovery-configuratieserver als een maximaal beschikbaar, on-premises VMware-VM. Nadat de configuratieserver actief is, Registreer ze deze in de kluis.

De configuratieserver wordt uitgevoerd voor een aantal onderdelen:

- Het configuratieserveronderdeel coördineert de communicatie tussen on-premises en Azure en beheert de gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag.
- De processerver installeert ook Mobility Service op VM's die u wilt repliceren, en detecteert automatisch on-premises VMware-VM's.

Contoso-beheerders uitvoeren deze stappen als volgt:

1. Ze download de OVF-sjabloon in **infrastructuur voorbereiden** > **bron** > **configuratieserver**.
    
    ![OVF downloaden](./media/contoso-migration-rehost-linux-vm/add-cs.png)

2. Ze importeert u de sjabloon in VMware om de VM te maken en implementeren van de virtuele machine.

    ![OVF-sjabloon](./media/contoso-migration-rehost-linux-vm/vcenter-wizard.png)

3. Wanneer ze de virtuele machine voor het eerst inschakelen, deze wordt opgestart in een Windows Server 2016-installatie-ervaring. Ze accepteer de gebruiksrechtovereenkomst en voer een administrator-wachtwoord.
4. Nadat de installatie is voltooid, ze zich aanmelden bij de virtuele machine als beheerder. De Azure Site Recovery Configuration Tool wordt op de eerste aanmelding wordt standaard uitgevoerd.
5. In het hulpprogramma Geef ze een naam te gebruiken voor het registreren van de configuratieserver in de kluis.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, zich ze aanmelden bij de Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin u de configuratieserver wilt registreren.

    ![Configuratieserver registreren](./media/contoso-migration-rehost-linux-vm/config-server-register2.png)

7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
8. Ze zich aanmelden bij de machine opnieuw en de configuratiewizard voor Server Management wordt automatisch gestart.
9. In de wizard selecteert ze de NIC die replicatieverkeer moet ontvangen. Deze instelling kan niet worden gewijzigd nadat deze geconfigureerd.
10. Ze selecteert u het abonnement, resourcegroep en kluis waarin de configuratieserver registreren.

    ![Kluis](./media/contoso-migration-rehost-linux-vm/cswiz1.png) 

11. Ze vervolgens downloaden en MySQL-Server en VMWare PowerCLI installeren. 
12. Na de validatie Geef ze de FQDN-naam of IP-adres van de vCenter-server of vSphere-host. Ze laat de standaardpoort, en geef een beschrijvende naam voor de vCenter-server.
13. Ze geven het account dat ze voor automatische detectie hebt gemaakt en de referenties die moeten worden gebruikt voor het automatisch installeren van de Mobility-Service.

    ![vCenter](./media/contoso-migration-rehost-linux-vm/cswiz2.png)

14. Na registratie is voltooid, wordt in de Azure-portal, ze controleren dat de configuratieserver en de VMware-server worden weergegeven op de **bron** pagina in de kluis. Detectie kan 15 minuten of langer duren. 
15. Site Recovery maakt verbinding met de VMware-servers, en VM's worden gedetecteerd.

### <a name="set-up-the-target"></a>Het doel instellen

Nu configureren beheerders van Contoso de doel-replicatie-instellingen.

1. In **infrastructuur voorbereiden** > **doel**, ze de doelinstellingen selecteren.
2. Site Recovery controleert of er een Azure storage-account en het netwerk in het opgegeven doel is.

### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

Nadat de bron en het doel zijn ingesteld, zijn ze klaar om te maken van een replicatiebeleid.

1. In **infrastructuur voorbereiden** > **replicatie-instellingen** > **replicatiebeleid** >  **maken en Koppelen**, maken van een beleid **ContosoMigrationPolicy**.
2. Ze gebruiken de standaard-instellingen:
    - **RPO-drempelwaarde**: standaardwaarde van 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
    - **Bewaarperiode voor herstelpunten**. De standaardwaarde van 24 uur. Deze waarde wordt bepaald hoe lang de bewaarperiode is voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
    - **Frequentie App-consistente momentopname**. De standaardwaarde van één uur. Deze waarde bepaalt de frequentie waarmee App-consistente momentopnamen worden gemaakt.
 
        ![Replicatiebeleid maken](./media/contoso-migration-rehost-linux-vm/replication-policy.png)

5. Het beleid wordt automatisch gekoppeld aan de configuratieserver. 

    ![Het replicatiebeleid koppelen](./media/contoso-migration-rehost-linux-vm/replication-policy2.png)

**Meer hulp nodig?**

- U kunt een overzicht van al deze stappen in lezen [herstel na noodgevallen instellen voor on-premises VMware-machines](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Gedetailleerde instructies zijn beschikbaar om u te helpen [de bronomgeving instellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [de configuratieserver implementeren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), en [replicatie-instellingen configureren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) over de Azure-gastagent voor Linux.

**Meer hulp nodig?**

- U kunt een overzicht van al deze stappen in lezen [herstel na noodgevallen instellen voor on-premises VMware-machines](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Gedetailleerde instructies zijn beschikbaar om u te helpen [de bronomgeving instellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [de configuratieserver implementeren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), en [replicatie-instellingen configureren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) over de Azure-gastagent voor Linux.



### <a name="enable-replication-for-osticketweb"></a>Replicatie inschakelen voor OSTICKETWEB

Nu de Contoso-beheerders kunt beginnen met het repliceren van de **OSTICKETWEB** VM.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze de instellingen voor gegevensbron selecteren.
2. Ze selecteren die ze willen virtuele machines inschakelen, selecteert u de instellingen voor gegevensbron, met inbegrip van de vCenter-server en de configuratieserver.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm/enable-replication-source.png)

3. Ze geven de doelinstellingen, met inbegrip van de resourcegroep en VNet waarin de Azure-VM na failover geplaatst worden wordt en het opslagaccount waarin de gerepliceerde gegevens worden opgeslagen.

     ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm/enable-replication2.png)

3. Ze selecteert de **OSTICKETWEB** VM voor replicatie. 

    - In dit stadium ze selecteren **OSTICKETWEB** alleen, omdat de VNet en subnet moeten beide zijn ingeschakeld en de virtuele machines worden niet in hetzelfde subnet.
    - Site Recovery installeert automatisch de Mobility-service wanneer replicatie is ingeschakeld voor de virtuele machine.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm/enable-replication3.png)

4. In de VM-eigenschappen selecteert ze het account dat wordt gebruikt door de processerver voor de Mobility-Service automatisch te installeren op de machine.

     ![Mobility-service](./media/contoso-migration-rehost-linux-vm/linux-mobility.png)

5. in **replicatie-instellingen** > **replicatie-instellingen configureren**, ze controleren of het juiste replicatiebeleid toegepast, en selecteer is **inschakelen replicatie**.
6.  Volgen ze Replicatievoortgang gedurende **taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.



### <a name="enable-replication-for-osticketmysql"></a>Replicatie inschakelen voor OSTICKETMYSQL

Nu de Contoso-beheerders kunnen beginnen met het repliceren **OSTICKETMYSQL**.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze de instellingen voor bron en doel selecteren.
2. Ze selecteert de **OSTICKETMYSQL** VM voor replicatie en selecteer het account moet worden gebruikt voor de installatie van de Mobility-service.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm/mysql-enable.png)

3. Ze toepassen van de hetzelfde replicatiebeleid dat is gebruikt voor OSTICKETWEB en replicatie inschakelen.  

**Meer hulp nodig?**

U kunt een overzicht van al deze stappen in lezen [inschakelen replicatie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>Stap 4: De virtuele machines migreren 

Beheerders van Contoso uitvoeren van een snelle testfailover en migreer de virtuele machines.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Een testfailover uitvoeren om te controleren of alles werkt zoals verwacht vóór de migratie. 

1. Ze een testfailover uitvoeren naar de laatst beschikbare punt in tijd (**laatst verwerkte**).
2. Ze selecteren **sluit de computer voordat u begint met failover**, zodat Site Recovery wordt geprobeerd de bron-VM afsluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. 
3. Test-failover wordt uitgevoerd: 
    - Een controle uitgevoerd om te controleren of alle van de voorwaarden die zijn vereist voor de migratie is voldaan.
    - De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als de meest recente herstelpunt is geselecteerd, wordt een herstelpunt wordt gemaakt van de gegevens.
    - Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.
3. Nadat de failover is voltooid, wordt de replica virtuele machine in Azure weergegeven in de Azure-portal. Zij controleren dat de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en dat deze wordt uitgevoerd. 
4. Nadat u hebt gecontroleerd, ze opschonen van de failover, en noteer eventuele opmerkingen en sla.

### <a name="create-and-customize-a-recovery-plan"></a>Maken en aanpassen van een herstelplan

 Nadat u hebt gecontroleerd of de testfailover heeft gewerkt zoals verwacht, maken de beheerders van Contoso een herstelplan voor migratie. 

- Een herstelplan Hiermee geeft u de volgorde in met een failover optreedt, hoe de Azure-VM's wordt geopend om in Azure.
- Omdat ze migreren van een app met twee lagen wilt, moeten ze het herstelplan te gaan aanpassen zodat de virtuele machine (SQLVM) van de gegevens wordt gestart voordat de front-end (WEBVM).


1. In **herstelplannen (Site Recovery)** > **+ herstelplan**, ze een plan maken en de virtuele machines aan toe te voegen.

    ![Plan voor herstel](./media/contoso-migration-rehost-linux-vm/recovery-plan.png)

2. Na het maken van het abonnement, ze selecteert u deze voor aanpassing (**herstelplannen** > **OsTicketMigrationPlan** > **aanpassen**.
3.  Ze verwijderen **OSTICKETWEB** van **groep 1: Start**.  Dit zorgt ervoor dat de eerste startactie is van invloed op **OSTICKETMYSQL** alleen.

    ![Groep voor gegevensherstel](./media/contoso-migration-rehost-linux-vm/recovery-group1.png)

4.  In **+ groep** > **toevoegen beveiligde items**, het toevoegen van **OSTICKETWEB** naar **groep 2: Start**.  Ze moeten in twee verschillende groepen.

    ![Groep voor gegevensherstel](./media/contoso-migration-rehost-linux-vm/recovery-group2.png)


### <a name="migrate-the-vms"></a>De virtuele machines migreren


Beheerders van Contoso zijn nu gereed voor het uitvoeren van een failover op het herstelplan te gaan, voor het migreren van de virtuele machines.

1. Ze selecteert u het plan > **Failover**.
2.  Ze selecteren fungeren als failover voor de meest recente herstelpunt, en opgeven dat Site Recovery proberen moet om de on-premises VM af voordat de failover wordt geactiveerd te. Ze kunnen de voortgang van de failover volgen op de **taken** pagina.

    ![Failover](./media/contoso-migration-rehost-vm/failover1.png)

3. Tijdens de failover geeft de vCenter-Server opdrachten om te stoppen van de twee virtuele machines die worden uitgevoerd op de ESXi-host.

    ![Failover](./media/contoso-migration-rehost-linux-vm/vcenter-failover.png)

3. Na de failover controleren ze of de Azure-VM wordt weergegeven zoals verwacht in de Azure-portal.

    ![Failover](./media/contoso-migration-rehost-linux-vm/failover2.png)  

3. Nadat u hebt de virtuele machine in Azure gecontroleerd, voltooien ze de migratie voor het voltooien van het migratieproces voor elke virtuele machine. Deze replicatie voor de virtuele machine wordt gestopt en Site Recovery-facturering voor de virtuele machine stopt.

    ![Failover](./media/contoso-migration-rehost-linux-vm/failover3.png)


### <a name="connect-the-vm-to-the-database"></a>De virtuele machine verbinding met de database

Als de laatste stap in het migratieproces, Contoso adins bijwerken van de verbindingsreeks van de toepassing om te verwijzen naar de app-database op de **OSTICKETMYSQL** VM. 

1. Ze maken van een SSH-verbinding met de **OSTICKETWEB** VM met behulp van Putty of een andere SSH-client. De virtuele machine is privé, zodat ze verbinding maken met het particuliere IP-adres.

    ![Verbinding maken met database](./media/contoso-migration-rehost-linux-vm/db-connect.png)  

    ![Verbinding maken met database](./media/contoso-migration-rehost-linux-vm/db-connect2.png)  

2. Ze nodig hebben om ervoor te zorgen dat de **OSTICKETWEB** virtuele machine kan communiceren met de **OSTICKETMYSQL** VM. De configuratie is momenteel vastgelegd met het IP-adres van on-premises 172.16.0.43.

    **Vóór de update**
    
    ![Bijwerken van IP](./media/contoso-migration-rehost-linux-vm/update-ip1.png)  

    **Na de update**
    
    ![Bijwerken van IP](./media/contoso-migration-rehost-linux-vm/update-ip2.png) 
    
3. Ze start de service met **systemctl opnieuw apache2**.

    ![Opnieuw starten](./media/contoso-migration-rehost-linux-vm/restart.png) 

4. Ze werk tot slot de DNS-records voor **OSTICKETWEB** en **OSTICKETMYSQL**, op een van de Contoso-domeincontrollers.

    ![DNS bijwerken](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

    ![DNS bijwerken](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) waarop een test-failover wordt uitgevoerd. 
- [Informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) over het maken van een plan voor herstel.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Failover-overschakeling uitvoeren naar Azure.

## <a name="clean-up-after-migration"></a>Na de migratie opschonen

Met de migratie is voltooid, worden de lagen van de app osTicket nu uitgevoerd op Azure Virtual machines.

Contoso heeft nu nodig om op te schonen als volgt: 

- De on-premises VM's van de vCenter-voorraad verwijderen.
- De on-premises VM's van lokale back-uptaken verwijderen.
- Bijwerken hun interne documentatie om de nieuwe locatie en IP-adressen voor OSTICKETWEB en OSTICKETMYSQL weer te geven.
- Alle resources die interactie met de virtuele machines hebben controleren en bijwerken van de relevante instellingen of documentatie om de nieuwe configuratie weer te geven.
- Contoso gebruikt de service Azure Migrate met afhankelijkheidstoewijzing om te beoordelen van de VM's voor migratie. Beheerders moeten Microsoft Monitoring Agent en de Agent voor afhankelijkheden ze geïnstalleerd voor dit doel, van de virtuele machine verwijderen.

## <a name="review-the-deployment"></a>De implementatie controleren

Met de app nu wordt uitgevoerd, moet Contoso volledig operationeel maken en hun nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

Het Contoso security team Bekijk de OSTICKETWEB en OSTICKETMYSQLVMs om beveiligingsproblemen te bepalen.

- Het team controleert de Netwerkbeveiligingsgroepen (nsg's) voor de virtuele machines om toegang te beheren. Nsg's worden gebruikt om ervoor te zorgen dat alleen verkeer dat is toegestaan voor de toepassing kan worden doorgegeven.
- Het team is ook rekening gehouden met de beveiliging van de gegevens op de VM-schijven met behulp van versleuteling van schijf en Azure Key Vault.

[Lees meer](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) over procedures voor beveiliging voor virtuele machines.

### <a name="bcdr"></a>BCDR

Voor bedrijfscontinuïteit en herstel na noodgevallen voert Contoso de volgende acties uit:

- **Houd gegevens veilig**: Contoso back-ups van de gegevens op de virtuele machines met behulp van de Azure Backup-service. [Meer informatie](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- **Houd apps startklaar**: Contoso repliceert de virtuele machines in Azure-app naar een secundaire regio met behulp van Site Recovery. [Meer informatie](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart).

### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

- Na het implementeren van resources, Contoso wijst Azure tags zoals gedefinieerd tijdens de [implementatie van de Azure-infrastructuur](contoso-migration-infrastructure.md#set-up-tagging).
- Contoso heeft geen licentie problemen met de Ubuntu-servers.
- Contoso kunnen Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Het is een kostenbeheeroplossing met meerdere Clouds management-oplossing die u helpt bij het gebruiken en beheren van Azure en andere cloudresources.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over Azure Cost Management. 


## <a name="next-steps"></a>Volgende stappen

In dit artikel die we hebben laten zien hoe Contoso gemigreerd gelaagd een on-premises servicedesk-app op twee Linux-VM's naar Azure IaaS-VM's, met behulp van Azure Site Recovery.

In het volgende artikel in de reeks leert u hoe Contoso de dezelfde servicedesk-app naar Azure migreert. Deze tijd Contoso maakt gebruik van Site Recovery voor het migreren van de front-end VM voor de app en worden gemigreerd, de app-database via back-up maken en herstellen met Azure Database voor MySQL, met behulp van het hulpprogramma MySQL workbench. [Aan de slag](contoso-migration-rehost-linux-vm-mysql.md).
