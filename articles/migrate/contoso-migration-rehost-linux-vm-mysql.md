---
title: Een Contoso Linux helpdesk-app naar Azure en Azure MySQL rehost | Microsoft Docs
description: Informatie over hoe Contoso een on-premises Linux app naamconflicten door te migreren naar Azure VM's en Azure MySQL.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: fbb70bd20b89bb1b711630ba54fe31806292385c
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39002225"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms-and-azure-mysql"></a>Migratie van Contoso: een on-premises Linux-app op Azure VM's en Azure MySQL Rehost

In dit artikel leest u hoe Contoso de on-premises twee lagen Linux servicedesk-app (osTicket), naamconflicten door te migreren naar Azure en Azure MySQL.

Dit document is een in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso de on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en scenario's die laten zien hoe u een migratie-infrastructuur instellen en uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit en we hierna zullen toevoegen aanvullende artikelen na verloop van tijd.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de artikel-serie en de voorbeeld-apps die we gebruiken. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. Dezelfde infrastructuur wordt gebruikt voor alle Contoso migratiescenario's. | Beschikbaar
[Artikel 3: On-premises resources evalueren](contoso-migration-assessment.md)  | Laat zien hoe een evaluatie van hun on-premises twee lagen SmartHotel app waarop VMware wordt uitgevoerd in Contoso. Beoordeling van de virtuele machines van een app met de [Azure Migrate](migrate-overview.md) -service en de SQL Server-database van de app met de [Azure Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
[Artikel 4: Hosten op Azure VM's en een beheerd exemplaar van SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Ziet u hoe Contoso de SmartHotel-app naar Azure migreert. Migreren van de app web virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database via de [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview) service, om te migreren naar een SQL beheerd exemplaar. | Beschikbaar
[Artikel 5: Opnieuw hosten naar virtuele Azure-machines](contoso-migration-rehost-vm.md) | Ziet u hoe Contoso hun SmartHotel migreren naar Azure IaaS-VM's, met behulp van de Site Recovery-service. | Beschikbaar
[Artikel 6: Opnieuw hosten naar virtuele machines in Azure en SQL Server-beschikbaarheidsgroepen](contoso-migration-rehost-vm-sql-ag.md) | Laat zien hoe de app SmartHotel door Contoso worden gemigreerd. Deze Site Recovery gebruiken voor het migreren van de VM's van de app en de Database Migration service de app-database migreren naar een SQL Server-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Ziet u hoe Contoso hun osTicket Linux-app naar Azure IaaS-VM's met Azure Site Recovery migreert. | Beschikbaar
Artikel 8: Een Linux-app op Azure VM's en Azure MySQL-Server opnieuw hosten | Ziet u hoe Contoso de osTicket Linux app migreert. Deze Site Recovery voor migratie van virtuele machine en MySQL Workbench gebruiken om te migreren naar een Azure MySQL-Server-exemplaar. | In dit artikel.
[Artikel 9: Een app op Azure Web Apps en Azure SQL database herstructureren](contoso-migration-refactor-web-app-sql.md) | Laat zien hoe Contoso de app SmartHotel migreert naar een Azure-Web-App en de app-database migreert naar Azure SQL Server-exemplaar | Beschikbaar
[Artikel 10: Een Linux-app op Azure-Web-Apps en Azure MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | Ziet u hoe Contoso de osTicket Linux app migreert naar Azure Web Apps op meerdere locaties, geïntegreerd met GitHub voor continue levering. Zij migreren de app-database naar een Azure MySQL-exemplaar. | Beschikbaar
[Artikel 11: Herstructureren TFS op VSTS](contoso-migration-tfs-vsts.md) | Ziet u hoe Contoso hun Team Foundation Server (TFS) on-premises implementatie door te migreren migreert het naar Visual Studio Team Services (VSTS) in Azure. | Beschikbaar
[Artikel 12: Opnieuw ontwerpen van een app op Azure-containers en Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Ziet u hoe Contoso migreert en rearchitects hun SmartHotel app naar Azure. Ze opnieuw ontwerpen voor de laag van de web-app als een Windows-container en de app-database in een Azure SQL Database. | Beschikbaar
[Artikel 13: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Ziet u hoe Contoso hun SmartHotel-app met een scala aan mogelijkheden van Azure en -services, waaronder App Services, Azure Kubernetes, Azure Functions, Cognitive services en Cosmos DB opnieuw. | Beschikbaar


In dit artikel migreert Contoso een app met twee lagen Linux Apache MySQL PHP (LAMP) servicedesk (osTicket) naar Azure. Als u wilt deze open-source-app wilt gebruiken, kunt u het downloaden van [GitHub](https://github.com/osTicket/osTicket).



## <a name="business-drivers"></a>Zakelijke drijfveren

Het team voor IT-leidinggevenden heeft nauw samengewerkt met hun zakelijke partners om te begrijpen wat ze willen bereiken:

- **Adres van de groei van het bedrijf**: Contoso groeit en er is als gevolg hiervan druk op de on-premises systemen en de infrastructuur.
- **Beperk risico**: de service-app helpdesk is essentieel voor het bedrijf Contoso. Ze willen verplaatsen naar Azure met nul risico.
- **Uitbreiden**: Contoso niet wilt wijzigen van de app nu. Ze gewoon wilt bewaren en stabiel.


## <a name="migration-goals"></a>Migratie-doelen

Het Contoso-cloud-team heeft vastgemaakt omlaag doelstellingen voor deze migratie kan om te bepalen van de beste migratiemethode:

- Na de migratie, moet de app in Azure dezelfde prestatiemogelijkheden hebben als in hun on-premises VMWare-omgeving.  De app blijven als kritiek in de cloud als on-premises. 
- Contoso wil niet investeren in deze app.  Het is belangrijk voor het bedrijf, maar in de huidige vorm ze gewoon wilt verplaatsen veilig naar de cloud.
- Nadat een aantal migraties van Windows-app is voltooid, wil Contoso voor meer informatie over het gebruik van een infrastructuur op basis van Linux in Azure.
- Contoso wil minimaliseren beheertaken database nadat de toepassing is verplaatst naar de cloud.

## <a name="proposed-architecture"></a>Voorgestelde architectuur

In dit scenario geldt het volgende:

- De app is gelaagd over twee virtuele machines (OSTICKETWEB en OSTICKETMYSQL).
- De virtuele machines bevinden zich op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5).
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een on-premises domeincontroller (**contosodc1**).
- De web-app-laag op OSTICKETWEB worden gemigreerd naar een Azure IaaS-VM.
- De app-database worden, gemigreerd naar de Azure Database for MySQL-PaaS-service.
- Omdat ze een productie-werkbelasting bent gemigreerd, de resources worden opgeslagen in de resourcegroep van de productie **ContosoRG**.
- De resources worden gerepliceerd naar de primaire regio (VS-Oost 2), en worden in de productienetwerk (VNET-PROD-EUS2):
    - De web virtuele machine bevindt zich in het frontendsubnet (PROD-FE-EUS2).
    - Het database-exemplaar bevindt zich in de database-subnet (PROD-DB-EUS2).
- De app-database worden, gemigreerd naar Azure MySQL met behulp van de MySQL-hulpprogramma's.
- De on-premises machines in het Contoso-datacenter wordt buiten gebruik gesteld nadat de migratie is voltooid.


![Scenario-architectuur](./media/contoso-migration-rehost-linux-vm-mysql/architecture.png) 


## <a name="migration-process"></a>Migratieproces

Contoso wordt als volgt het migratieproces voltooid:

Voor het migreren van het web virtuele machine:

1. Als een eerste stap Contoso stelt u de Azure en on-premises infrastructuur die nodig zijn voor het implementeren van Site Recovery.
2. Na het voorbereiden van de onderdelen van Azure en on-premises, ze instellen en inschakelen van replicatie voor de web-VM.
3. Nadat de replicatie is up-en-die wordt uitgevoerd, migreren zij de virtuele machine met failover naar Azure.

Voor het migreren van de database:

1. Contoso richt een MySQL-exemplaar in Azure.
2. Deze MySQL workbench instellen en back-up van de database lokaal.
3. Ze zet de database van de lokale back-up naar Azure.

![Migratieproces](./media/contoso-migration-rehost-linux-vm-mysql/migration-process.png) 


### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | De service coördineert en beheert de migratie en herstel na noodgevallen voor Azure-VM's, en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, rekening Azure Storage-kosten in gebracht.  Azure-VM's worden gemaakt en kosten, wanneer een failover optreedt. [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) kosten in rekening gebracht en prijzen.
[Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) | De database is gebaseerd op de open-source MySQL-Server-engine. Het biedt een volledig beheerde, bedrijfsklare community MySQL-database als een service voor ontwikkeling en implementatie. 

 
## <a name="prerequisites"></a>Vereisten

Als u (en Contoso) uitvoeren van dit scenario wilt, is hier wat u moet hebben.

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | U moet al een abonnement hebt gemaakt tijdens de eerste artikelen in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-infrastructuur** | Instellen van hun Azure-infrastructuur, zoals beschreven in Contoso [Azure-infrastructuur voor migratie](contoso-migration-infrastructure.md).<br/><br/> Meer informatie over specifieke [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) vereisten voor Site Recovery.
**On-premises servers** | Uw on-premises vCenter-server moet versie 5.5, 6.0 of 6.5 worden uitgevoerd<br/><br/> Een ESXi-host waarop versie 5.5, 6.0 of 6.5<br/><br/> Een of meer virtuele VMware-machines die worden uitgevoerd op de ESXi-host.
**On-premises VM 's** | [Bekijk de vereisten voor Linux-VM](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) die worden ondersteund voor migratie met Site Recovery.<br/><br/> Controleer of ondersteund [Linux-systemen voor bestands- en opslagservices](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage).<br/><br/> Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Scenariostappen

Dit is hoe Azure de migratie wordt voltooid:

> [!div class="checklist"]
> * **Stap 1: Azure voorbereiden voor Site Recovery**: U maakt een Azure storage-account voor het opslaan van gerepliceerde gegevens en een Recovery Services-kluis maken.
> * **Stap 2: On-premises VMware voorbereiden voor Site Recovery**: U accounts voor VM-detectie- en agent-installatie voorbereiden en voorbereidingen voor het verbinding maken met virtuele Azure-machines na een failover.
 * **Stap 3: inrichting van de database]**: In Azure, een exemplaar van Azure MySQL-database inricht.
> * **Stap 4:-Machines repliceren**: U configureert u de omgeving van bron- en Site Recovery, een replicatiebeleid instellen en beginnen met het repliceren van virtuele machines naar Azure storage.
> * **Stap 5: Migreren van de database**: instellen van de migratie met MySQL-hulpprogramma's.
> * **Stap 6: Migreer de virtuele machines met Site Recovery**: voert een testfailover uit om te controleren of alles werkt en voer een volledige failover voor de virtuele machines migreren naar Azure.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Stap 1: Azure voorbereiden voor de Site Recovery-service

Contoso heeft een aantal Azure-onderdelen nodig voor Site Recovery:

- Failover van een VNet waarin resources zich bevinden (Contoso gebruikt de productie VNet ze al geïmplementeerd).
- Een nieuw Azure storage-account voor het opslaan van gerepliceerde gegevens. 
- Een Recovery Services-kluis in Azure.

Contoso is al gemaakt het VNet tijdens [implementatie van de Azure-infrastructuur](contoso-migration-infrastructure.md), zodat ze alleen wilt maken van een storage-account en de kluis.


1. Contoso maakt u een Azure storage-account (**contosovmsacc20180528**) in de regio VS-Oost 2.

    - Het opslagaccount moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
    - Contoso maakt gebruik van een account voor algemeen gebruik, met de standard-opslag en LRS-replicatie.

    ![Site Recovery-opslag](./media/contoso-migration-rehost-linux-vm-mysql/asr-storage.png)

3. Met de netwerk- en storage-account in plaats Contoso maakt u een kluis (ContosoMigrationVault) en plaatst het in de **ContosoFailoverRG** resourcegroep in de primaire regio in de VS-Oost 2.

    ![Recovery Services-kluis](./media/contoso-migration-rehost-linux-vm-mysql/asr-vault.png)

**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) instellen van Azure voor Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Stap 2: On-premises VMware voorbereiden voor Site Recovery

Contoso bereidt de on-premises VMware-infrastructuur als volgt:

- Hiermee maakt u een account op de vCenter-server voor het automatiseren van VM-detectie.
- Hiermee maakt u een account waarmee de automatische installatie van de Mobility-service op VMware-VM's die u wilt repliceren.
- Zorgt ervoor dat on-premises VM's, zodat ze verbinding met Azure-VM's maken kunnen wanneer ze worden gemaakt na de migratie.


### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Virtuele machines automatisch detecteren. Er is minimaal een alleen-lezen-account vereist.
- Replicatie, failover en failback orkestreren. U moet een account die bewerkingen zoals het maken en verwijderen van schijven en het inschakelen van virtuele machines kan worden uitgevoerd.

Contoso stelt als volgt u het account:

1. Contoso maakt een rol op vCenter-niveau.
2. Contoso vervolgens toe te wijzen die rol de vereiste machtigingen.


### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op elke virtuele machine die Contoso wil migreren.

- Site Recovery kan een automatische push-installatie van dit onderdeel kunt doen wanneer u replicatie voor de virtuele machines inschakelt.
- Voor automatische installatie. Site Recovery moet een account met machtigingen voor toegang tot de virtuele machine. 
- Accountdetails zijn ingevoerd tijdens de installatie van de replicatie. 
- Het account mag domein of lokale account, zolang deze Installatiemachtigingen heeft.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Na een failover naar Azure wil Contoso kunnen verbinding maken met de Azure VM's. Om dit te doen, moeten zij een aantal dingen doen: 

- Als u wilt openen via het internet, kunnen SSH in Linux on-premises virtuele machine vóór de migratie.  Voor Ubuntu dit kan worden voltooid met de volgende opdracht: **Sudo apt-get ssh in installeren -y**.
- Na de failover, moeten ze controleren **diagnostische gegevens over opstarten** om een schermopname van de virtuele machine weer te geven.
- Als dit niet werkt, moeten ze controleren dat de virtuele machine wordt uitgevoerd, en bekijk deze [tips voor probleemoplossing](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) maken en toewijzen van een rol voor automatische detectie.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) het maken van een account voor push-installatie van de Mobility-service.


## <a name="step-3-provision-azure-database-for-mysql"></a>Stap 3: Inrichting van Azure Database voor MySQL

Contoso bepalingen een MySQL-database-exemplaar in de primaire regio in de VS-Oost 2.

1. In de Azure-portal maken ze een Azure Database for MySQL-resource. 

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-1.png)

2. Toevoegen van de naam **contosoosticket** voor de Azure-database. Ze de database toevoegen aan de productie-resourcegroep **ContosoRG**, en geef de referenties voor deze.
3. De on-premises MySQL-database is versie 5.7 gebruikt, zodat ze deze versie voor compatibiliteit selecteren. Ze gebruiken de standaard-grootten die overeenkomen met de vereisten voor de database.

     ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-2.png)

4. Voor **redundantieopties voor back-up**, Contoso worden geselecteerd voor het gebruik van **geografisch redundante**. Deze optie kan ze de database in de secundaire regio VS-midden herstellen als er een storing optreedt. Ze kunnen deze optie alleen configureren wanneer het inrichten van de database.

     ![Redundantie](./media/contoso-migration-rehost-linux-vm-mysql/db-redundancy.png)

4. In de **VNET-PROD-EUS2** netwerk > **Service-eindpunten**, ze een service-eindpunt (een subnet van de database) voor de SQL-service toevoegen.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-3.png)

5. Na het toevoegen van het subnet, maken ze een regel voor virtuele netwerken waarmee de toegang vanaf het subnet van de database in het productienetwerk.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-4.png)


## <a name="step-4-replicate-the-on-premises-vms"></a>Stap 4: De on-premises VM's repliceren

Voordat ze het web virtuele machine naar Azure migreren kunnen, worden de Contoso ingesteld en replicatie inschakelen.

### <a name="set-a-protection-goal"></a>Stel een beveiligingsdoel

1. In de kluis onder de kluisnaam (ContosoVMVault) ze ingesteld een replicatiedoel (**aan de slag** > **siteherstel** > **infrastructuur voorbereiden**.
2. Ze opgeven dat hun computers zich op locatie bevinden, dat ze virtuele VMware-machines, en dat ze wilt repliceren naar Azure.

    ![Replicatiedoel](./media/contoso-migration-rehost-linux-vm-mysql/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

Als u wilt doorgaan, ze bevestigen dat zij implementatieplanning, door te selecteren hebt voltooid **Ja, heb ik gedaan**. Contoso zijn slechts één virtuele machine in dit scenario wordt gemigreerd, en hoeft-implementatie plannen.

### <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Contoso nodig heeft om de bronomgeving te configureren. U doet dit door met behulp van een OVF-sjabloon implementeren van een Site Recovery-configuratieserver als een maximaal beschikbare, on-premises VMware-VM. Nadat de configuratieserver actief is, Registreer ze deze in de kluis.

De configuratieserver wordt uitgevoerd voor een aantal onderdelen:

- Het configuratieserveronderdeel coördineert de communicatie tussen on-premises en Azure en beheert de gegevensreplicatie.
- Initiëren van failover met één klik in de portal of kunt u  PowerShell dat een failover wordt geactiveerd. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag.
- De processerver installeert ook Mobility Service op VM's die u wilt repliceren, en detecteert automatisch on-premises VMware-VM's.

Contoso uitvoeren als volgt te werk:


1. Ze download de OVF-sjabloon in **infrastructuur voorbereiden** > **bron** > **configuratieserver**.
    
    ![OVF downloaden](./media/contoso-migration-rehost-linux-vm-mysql/add-cs.png)

2. Ze importeert u de sjabloon in VMware om de VM te maken en implementeren van de virtuele machine.

    ![OVF-sjabloon](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-wizard.png)

3. Wanneer ze de virtuele machine voor het eerst inschakelen, deze wordt opgestart in een Windows Server 2016-installatie-ervaring. Ze accepteer de gebruiksrechtovereenkomst en voer een administrator-wachtwoord.
4. Nadat de installatie is voltooid, ze zich aanmelden bij de virtuele machine als de beheerder. De Azure Site Recovery Configuration Tool wordt op de eerste aanmelding wordt standaard uitgevoerd.
5. In het hulpprogramma Geef ze een naam te gebruiken voor het registreren van de configuratieserver in de kluis.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure.
7. Nadat de verbinding tot stand is gebracht, zich ze aanmelden bij de Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin u ze de configuratieserver registreren.

    ![Configuratieserver registreren](./media/contoso-migration-rehost-linux-vm-mysql/config-server-register2.png)

8. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
9. Ze zich aanmelden bij de machine opnieuw en de configuratiewizard voor Server Management wordt automatisch gestart.
10. In de wizard selecteert ze de NIC die replicatieverkeer moet ontvangen. Deze instelling kan niet worden gewijzigd nadat deze geconfigureerd.
11. Ze selecteert u het abonnement, resourcegroep en kluis waarin de configuratieserver registreren.

    ![Kluis](./media/contoso-migration-rehost-linux-vm-mysql/cswiz1.png) 

12. Nu downloaden en installeren van de MySQL-Server en VMWare PowerCLI. 
13. Na de validatie Geef ze de FQDN-naam of IP-adres van de vCenter-server of vSphere-host. Ze laat de standaardpoort, en geef een beschrijvende naam voor de vCenter-server.
14. Ze Voer in het account dat ze voor automatische detectie hebt gemaakt en de referenties op die Site Recovery gebruikt voor het automatisch installeren van de Mobility-Service. 

    ![vCenter](./media/contoso-migration-rehost-linux-vm-mysql/cswiz2.png)

14. Nadat de registratie is voltooid, wordt in de Azure-portal, Contoso controleert dat de configuratieserver en de VMware-server worden weergegeven op de **bron** pagina in de kluis. Detectie kan 15 minuten of langer duren. 
15. Alles wat erin, Site Recovery maakt verbinding met de VMware-servers en VM's worden gedetecteerd.

### <a name="set-up-the-target"></a>Het doel instellen

Nu doel Contoso-invoer voor de replicatie-instellingen.

1. In **infrastructuur voorbereiden** > **doel**, ze de doelinstellingen selecteren.
2. Site Recovery controleert of er een Azure storage-account en het netwerk in het opgegeven doel is.


### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

Contoso is gereed voor het maken van een replicatiebeleid met de bron en doel instellen.

1. In **infrastructuur voorbereiden** > **replicatie-instellingen** > **replicatiebeleid** >  **maken en Koppelen**, maken van een beleid **ContosoMigrationPolicy**.
2. Ze gebruiken de standaard-instellingen:
    - **RPO-drempelwaarde**: standaardwaarde van 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
    - **Bewaarperiode voor herstelpunten**. De standaardwaarde van 24 uur. Deze waarde wordt bepaald hoe lang de bewaarperiode is voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
    - **Frequentie App-consistente momentopname**. De standaardwaarde van één uur. Deze waarde bepaalt de frequentie waarmee App-consistente momentopnamen worden gemaakt.
 
        ![Replicatiebeleid maken](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy.png)

5. Het beleid wordt automatisch gekoppeld aan de configuratieserver. 

    ![Het replicatiebeleid koppelen](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy2.png)


**Meer hulp nodig?**

- U kunt een overzicht van al deze stappen in lezen [herstel na noodgevallen instellen voor on-premises VMware-machines](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Gedetailleerde instructies zijn beschikbaar om u te helpen [de bronomgeving instellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [de configuratieserver implementeren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), en [replicatie-instellingen configureren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) over de Azure-gastagent voor Linux.

### <a name="enable-replication-for-the-web-vm"></a>Replicatie inschakelen voor de Web-VM

Nu Contoso kunt beginnen met het repliceren van de **OSTICKETWEB** VM.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze de instellingen voor gegevensbron selecteren.
2. Ze geven aan dat ze willen virtuele machines inschakelen en selecteert u de instellingen voor gegevensbron, met inbegrip van de vCenter-server en de configuratieserver.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication-source.png)

3. Nu dat ze de doelinstellingen opgeven. Hieronder vallen de resourcegroep en netwerk waarin de Azure-VM na failover geplaatst worden wordt en het opslagaccount waarin de gerepliceerde gegevens worden opgeslagen. 

     ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication2.png)

3. Contoso selecteert **OSTICKETWEB** voor replicatie. 

    ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication3.png)

4. In de VM-eigenschappen selecteert ze het account dat moet worden gebruikt voor het installeren van de Mobility-Service automatisch op de virtuele machine.

     ![Mobility-service](./media/contoso-migration-rehost-linux-vm-mysql/linux-mobility.png)

5. in **replicatie-instellingen** > **replicatie-instellingen configureren**, ze controleren of het juiste replicatiebeleid toegepast, en selecteer is **inschakelen replicatie**. De Mobility-service wordt automatisch geïnstalleerd.
6.  Volgen ze Replicatievoortgang gedurende **taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.


**Meer hulp nodig?**

U kunt een overzicht van al deze stappen in lezen [inschakelen replicatie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-5-migrate-the-database"></a>Stap 5: De database migreren

Contoso kan de database met behulp van back-up en herstel, met de MySQL-hulpprogramma's worden gemigreerd. Ze MySQL Workbench installeren, back-up van de database van OSTICKETMYSQL en vervolgens herstellen met Azure Database voor MySQL-Server.

### <a name="install-mysql-workbench"></a>MySQL Workbench installeren

1. Controles van Contoso de [vereisten en downloads MySQL Workbench](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool).
2. Installatie van MySQL Workbench voor Windows in overeenstemming met de [installatie-instructies](https://dev.mysql.com/doc/workbench/en/wb-installing.html).
3. In MySQL Workbench maken ze een MySQL-verbinding met OSTICKETMYSQL. 

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench1.png)

4. Ze de database als exporteren **osticket**, naar een lokaal ingesloten bestand.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench2.png)

5. Nadat de database heeft back-ups lokaal, maken ze een verbinding met de Azure Database for MySQL-exemplaar.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench3.png)

6. Contoso kan nu (herstel) de database in het Azure MySQL-exemplaar van het ingesloten bestand importeren. Een nieuw schema (osticket) wordt voor het exemplaar gemaakt.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench4.png)

## <a name="step-6-migrate-the-vms-with-site-recovery"></a>Stap 6: Migreer de virtuele machines met Site Recovery

Contoso uitvoeren van een snelle testfailover, en vervolgens de virtuele machine te migreren.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Een testfailover uitvoeren controleren kunt of alles werkt zoals verwacht, vóór de migratie. 

1. Contoso wordt een testfailover uitgevoerd naar de laatst beschikbare punt in tijd (**laatst verwerkte**).
2. Ze selecteren **sluit de computer voordat u begint met failover**, zodat Site Recovery wordt geprobeerd de bron-VM afsluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. 
3. Test-failover wordt uitgevoerd: 

    - Een controle uitgevoerd om te controleren of alle van de voorwaarden die zijn vereist voor de migratie is voldaan.
    - De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als u het meest recente herstelpunt selecteert, wordt een herstelpunt van de gegevens gemaakt.
    - Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.

3. Nadat de failover is voltooid, wordt de replica virtuele machine in Azure weergegeven in de Azure-portal. Contoso controleert dat de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en dat deze wordt uitgevoerd. 
4. Nadat u hebt gecontroleerd, ze opschonen van de failover, en noteer eventuele opmerkingen en sla.

### <a name="migrate-the-vm"></a>De virtuele machine migreren

Als u wilt migreren van de virtuele machine, Contoso een herstelplan te gaan met de virtuele machine maakt en niet voldoen aan het abonnement over naar Azure.

1. Contoso maakt u een planning- en voegt **OSTICKETWEB** toe.

    ![Plan voor herstel](./media/contoso-migration-rehost-linux-vm-mysql/recovery-plan.png)

2. Ze een failover worden uitgevoerd op het plan. Ze het meest recente herstelpunt selecteren en opgeven dat Site Recovery proberen moet de on-premises VM afsluiten voordat de failover wordt geactiveerd. Ze kunnen de voortgang van de failover volgen op de **taken** pagina.

    ![Failover](./media/contoso-migration-rehost-linux-vm-mysql/failover1.png)

3. Tijdens de failover geeft de vCenter-Server opdrachten om te stoppen van de twee virtuele machines die worden uitgevoerd op de ESXi-host.

    ![Failover](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-failover.png)

4. Na de failover Contoso wordt gecontroleerd of de Azure-VM wordt weergegeven zoals verwacht in de Azure-portal.

    ![Failover](./media/contoso-migration-rehost-linux-vm-mysql/failover2.png)  

5. Nadat de virtuele machine is gecontroleerd, voltooien ze de migratie. Deze replicatie voor de virtuele machine wordt gestopt en Site Recovery-facturering voor de virtuele machine stopt.

    ![Failover](./media/contoso-migration-rehost-linux-vm-mysql/failover3.png)

**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) waarop een test-failover wordt uitgevoerd. 
- [Informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) over het maken van een plan voor herstel.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Failover-overschakeling uitvoeren naar Azure.


### <a name="connect-the-vm-to-the-database"></a>De virtuele machine verbinding met de database

Contoso bijwerken als de laatste stap in het migratieproces, de verbindingsreeks van de app om te verwijzen naar de Azure Database voor MySQL. 

1. Ze een SSH-verbinding maken met de VM OSTICKETWEB gebruiken van Putty of een andere SSH-client. De virtuele machine is privé, zodat ze verbinding maken met het particuliere IP-adres.

    ![Verbinding maken met database](./media/contoso-migration-rehost-linux-vm-mysql/db-connect.png)  

    ![Verbinding maken met database](./media/contoso-migration-rehost-linux-vm-mysql/db-connect2.png)  

2. Deze instellingen bijwerken zodat de **OSTICKETWEB** virtuele machine kan communiceren met de **OSTICKETMYSQL** database. De configuratie is momenteel vastgelegd met het IP-adres van on-premises 172.16.0.43.

    **Vóór de update**
    
    ![Bijwerken van IP](./media/contoso-migration-rehost-linux-vm-mysql/update-ip1.png)  

    **Na de update**
    
    ![Bijwerken van IP](./media/contoso-migration-rehost-linux-vm-mysql/update-ip2.png) 
    
    ![Bijwerken van IP](./media/contoso-migration-rehost-linux-vm-mysql/update-ip3.png) 

3. Ze start de service met **systemctl opnieuw apache2**.

    ![Opnieuw starten](./media/contoso-migration-rehost-linux-vm-mysql/restart.png) 

4. Ze werk tot slot de DNS-records voor **OSTICKETWEB**, op een van de Contoso-domeincontrollers.

    ![DNS bijwerken](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 


##  <a name="clean-up-after-migration"></a>Na de migratie opschonen

Met de migratie is voltooid, worden de lagen van de app osTicket uitgevoerd op Azure Virtual machines.

Contoso moet nu het volgende doen: 
- Verwijderen van de virtuele VMware-machines uit de vCenter-inventaris
- De on-premises VM's van lokale back-uptaken verwijderen.
- Update interne documentatie nieuwe locaties en IP-adressen weergeven. 
- Alle resources die interactie met de on-premises VM's hebben controleren en bijwerken van de relevante instellingen of documentatie om de nieuwe configuratie weer te geven.
- Contoso gebruikt de service Azure Migrate met afhankelijkheidstoewijzing om vast te stellen de **OSTICKETWEB** VM voor migratie. Ze nu de agents (Microsoft Monitoring Agent/afhankelijkheid Agent) deze geïnstalleerd voor dit doel, van de virtuele machine te verwijderen.

## <a name="review-the-deployment"></a>De implementatie controleren

Met de app nu wordt uitgevoerd, moet Contoso volledig operationeel maken en hun nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

Het Contoso security team Bekijk de virtuele machine en de database om te bepalen van alle beveiligingsproblemen met zich mee.

- Ze controleren de Netwerkbeveiligingsgroepen (nsg's) voor de virtuele machine, om toegang te beheren. Nsg's worden gebruikt om ervoor te zorgen dat alleen verkeer dat is toegestaan voor de toepassing kan worden doorgegeven.
- Ze rekening houden met het beveiligen van de gegevens op de VM-schijven met behulp van versleuteling van schijf en Azure Key Vault.
- Communicatie tussen de virtuele machine en de database-exemplaar is niet geconfigureerd voor SSL. Ze moeten doen dit om ervoor te zorgen dat databaseverkeer kan niet worden gehackt.

[Lees meer](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) over procedures voor beveiliging voor virtuele machines.

### <a name="backups"></a>Back-ups

- Contoso worden back-up van de gegevens op de virtuele machine met behulp van de Azure Backup-service. [Meer informatie](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- Ze hoeft te configureren van back-up voor de database. Azure Database for MySQL back-ups van de server automatisch gemaakt en opgeslagen. Ze geselecteerd om te gebruiken geo-redundantie voor de database, dus het is flexibel en gereed is voor productie.

### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

- Na het implementeren van resources, Contoso toegewezen Azure-tags, in overeenstemming met de beslissingen die tijdens de [Azure-infrastructuur](contoso-migration-infrastructure.md#set-up-tagging) implementatie.
- Er zijn geen licenties problemen voor de Contoso Ubuntu-servers.
- Contoso kunnen Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Het is een kostenbeheeroplossing met meerdere Clouds management-oplossing die u helpt bij het gebruiken en beheren van Azure en andere cloudresources.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over Azure Cost Management.


## <a name="next-steps"></a>Volgende stappen

In dit scenario gezien hoe de uiteindelijke rehost-scenario Contoso geoefend. Ze de frontend van de virtuele machine van de on-premises Linux osTicket app gemigreerd naar een Azure-VM en de app-database migreren naar een Azure MySQL-exemplaar.

In de volgende reeks zelfstudies in de reeks migratie gaan we om weer te geven hoe de Contoso uitgevoerd een meer complexe reeks migraties met betrekking tot app herstructurering, in plaats van eenvoudige lift-and-shift-migraties.
