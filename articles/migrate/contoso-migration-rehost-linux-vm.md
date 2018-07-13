---
title: Rehost migreren en een on-premises Linux-app op Azure-VM's opnieuw hosten | Microsoft Docs
description: Meer informatie over hoe een on-premises Linux-app in Contoso opnieuw hosten door te migreren naar Azure-VM's.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: 6c96beee347a7a36a3dc04ecf8cd994484fd6bb7
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007246"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms"></a>Migratie van Contoso: een Linux-app van de on-premises naar Azure VM's opnieuw hosten

In dit artikel leest u hoe Contoso opnieuw hosten een on-premises service op basis van Linux helpdesk-app (**osTicket**), naar Azure IaaS VM's.

Dit document is een in een reeks artikelen waarin hoe het fictieve bedrijf Contoso de on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en een set van scenario's die laat zien hoe u een migratie-infrastructuur instellen en uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit en we hierna zullen toevoegen aanvullende artikelen na verloop van tijd.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de artikel-serie en de voorbeeld-apps die we gebruiken. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. Dezelfde infrastructuur wordt gebruikt voor alle Contoso migratiescenario's. | Beschikbaar
[Artikel 3: On-premises resources evalueren](contoso-migration-assessment.md)  | Laat zien hoe een evaluatie van hun on-premises twee lagen SmartHotel app waarop VMware wordt uitgevoerd in Contoso. Beoordeling van de virtuele machines van een app met de [Azure Migrate](migrate-overview.md) -service en de SQL Server-database van de app met de [Azure Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
[Artikel 4: Hosten op Azure VM's en een beheerd exemplaar van SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Ziet u hoe Contoso de SmartHotel-app naar Azure migreert. Migreren van de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database via de [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview) service, om te migreren naar een SQL beheerd exemplaar. | Beschikbaar
[Artikel 5: Opnieuw hosten naar virtuele Azure-machines](contoso-migration-rehost-vm.md) | Ziet u hoe Contoso hun app SmartHotel virtuele machines migreren naar Azure-VM's. | Beschikbaar
[Artikel 6: Opnieuw hosten naar virtuele machines in Azure en SQL Server-beschikbaarheidsgroepen](contoso-migration-rehost-vm-sql-ag.md) | Laat zien hoe de app SmartHotel door Contoso worden gemigreerd. Deze Site Recovery gebruiken voor het migreren van de VM's van de app en de Database Migration service de app-database migreren naar een SQL Server-beschikbaarheidsgroep. | Beschikbaar
Artikel 7: Een Linux-app op Azure VM's opnieuw hosten | Ziet u hoe Contoso hun osService Linux-app met Azure Site Recovery migreert. | In dit artikel.
[Artikel 8: Een Linux-app op Azure VM's en Azure MySQL-Server opnieuw hosten](contoso-migration-rehost-linux-vm-mysql.md) | Laat zien hoe de Contoso migreert de osService Linux-app met behulp van Site Recovery voor migratie van virtuele machine en MySQL Workbench om te migreren (naar een Azure MySQL-Server-exemplaar. | Beschikbaar
[Artikel 9: Een app op Azure Web Apps en Azure SQL database herstructureren](contoso-migration-refactor-web-app-sql.md) | Laat zien hoe Contoso de app SmartHotel migreert naar een Azure-Web-App en de app-database migreert naar Azure SQL Server-exemplaar | Beschikbaar
[Artikel 10: Een Linux-app op Azure-Web-Apps en Azure MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | Ziet u hoe Contoso de osTicket Linux app migreert naar Azure Web Apps op meerdere locaties, geïntegreerd met GitHub voor continue levering. Zij migreren de app-database naar een Azure MySQL-exemplaar. | Beschikbaar
[Artikel 11: Herstructureren TFS op VSTS](contoso-migration-tfs-vsts.md) | Ziet u hoe Contoso hun Team Foundation Server (TFS) on-premises implementatie door te migreren migreert het naar Visual Studio Team Services (VSTS) in Azure. | Beschikbaar
[Artikel 12: Opnieuw ontwerpen van een app op Azure-containers en Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Ziet u hoe Contoso migreert en rearchitects hun SmartHotel app naar Azure. Ze opnieuw ontwerpen voor de laag van de web-app als een Windows-container en de app-database in een Azure SQL Database. | Beschikbaar
[Artikel 13: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Ziet u hoe Contoso hun SmartHotel-app met een scala aan mogelijkheden van Azure en -services, waaronder App Services, Azure Kubernetes, Azure Functions, Cognitive services en Cosmos DB opnieuw. | Beschikbaar

In dit artikel, Contoso de twee lagen worden gemigreerd **osTicket** app, die wordt uitgevoerd op Linux Apache MySQL PHP (LAMP) naar Azure. De VM's van de app worden, gemigreerd met behulp van de Azure Site Recovery-service. Als u wilt deze open-source-app wilt gebruiken, kunt u het downloaden van [GitHub](https://github.com/osTicket/osTicket).

## <a name="business-drivers"></a>Zakelijke drijfveren

Het team voor IT-leidinggevenden heeft nauw samengewerkt met hun zakelijke partners om te begrijpen wat ze willen bereiken bij deze migratie:

- **Adres van de groei van het bedrijf**: Contoso groeit en er is als gevolg hiervan druk op de on-premises systemen en de infrastructuur.
- **Beperk risico**: de service-app helpdesk is essentieel voor het bedrijf Contoso. Ze willen verplaatsen naar Azure met nul risico.
- **Uitbreiden**: ze niet wilt dat de app nu wijzigen. Ze willen gewoon om ervoor te zorgen dat het stabiel is.


## <a name="migration-goals"></a>Migratie-doelen

Het Contoso-cloud-team heeft vastgemaakt omlaag doelstellingen voor deze migratie, om te bepalen van de beste migratiemethode:

- Na de migratie, moet de app in Azure dezelfde prestatiemogelijkheden hebben als in hun on-premises VMWare-omgeving.  De app blijven als kritiek in de cloud als on-premises. 
- Contoso wil niet investeren in deze app.  Het is belangrijk voor het bedrijf, maar in de huidige vorm ze gewoon wilt verplaatsen veilig naar de cloud.
- Contoso wilt niet wijzigen van de ops-model voor deze app. Willen ze ermee in de cloud op dezelfde manier als nu.
- Contoso wilt niet wijzigen van app-functionaliteit. Alleen de locatie van de app wordt gewijzigd.
- Nadat een aantal migraties van Windows-app is voltooid, wil Contoso voor meer informatie over het gebruik van een infrastructuur op basis van Linux in Azure.

## <a name="proposed-architecture"></a>Voorgestelde architectuur

In dit scenario geldt het volgende:

- De app is gelaagd over twee virtuele machines (OSTICKETWEB en OSTICKETMYSQL).
- De virtuele machines bevinden zich op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5).
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacenter (**contoso-datacenter**), met een on-premises domeincontroller (**contosodc1**).
- Omdat de app een productie-werkbelasting is, de virtuele machines in Azure bevindt zich in de resourcegroep van de productie **ContosoRG**.
- De virtuele machines worden gemigreerd naar de primaire regio (VS-Oost 2) en worden in de productienetwerk (VNET-PROD-EUS2):
    - De web virtuele machine bevindt zich in het frontendsubnet (PROD-FE-EUS2).
    - De virtuele machine van de database bevindt zich in de database-subnet (PROD-DB-EUS2).
- De on-premises machines in het Contoso-datacenter wordt buiten gebruik gesteld nadat de migratie is voltooid.

![Scenario-architectuur](./media/contoso-migration-rehost-linux-vm/architecture.png) 

## <a name="migration-process"></a>Migratieproces

Contoso worden als volgt gemigreerd:

1. Als een eerste stap Contoso stelt u de Azure en on-premises infrastructuur die nodig zijn voor het implementeren van Site Recovery.
2. Na het voorbereiden van Azure en on-premises onderdelen, ze instellen en inschakelen van replicatie voor de virtuele machines.
3. Nadat de replicatie werkt, migreren zij de virtuele machines met failover naar Azure.

![Migratieproces](./media/contoso-migration-rehost-linux-vm/migration-process.png)

### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | De service coördineert en beheert de migratie en herstel na noodgevallen voor Azure-VM's, en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, rekening Azure Storage-kosten in gebracht.  Azure-VM's worden gemaakt en kosten, wanneer een failover optreedt. [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) kosten in rekening gebracht en prijzen.

 
## <a name="prerequisites"></a>Vereisten

Dit is wat u (en Contoso) nodig voor dit scenario.

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | U moet al een abonnement hebt gemaakt tijdens de eerste artikelen in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-infrastructuur** | Instellen van hun Azure-infrastructuur, zoals beschreven in Contoso [Azure-infrastructuur voor migratie](contoso-migration-infrastructure.md).<br/><br/> Meer informatie over specifieke [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) vereisten voor Site Recovery.
**On-premises servers** | Uw on-premises vCenter-server moet versie 5.5, 6.0 of 6.5 worden uitgevoerd<br/><br/> Een ESXi-host waarop versie 5.5, 6.0 of 6.5<br/><br/> Een of meer virtuele VMware-machines die worden uitgevoerd op de ESXi-host.
**On-premises VM 's** | [Linux-machines controleren](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) die worden ondersteund voor migratie met Site Recovery.<br/><br/> Controleer of ondersteund [Linux-systemen voor bestands- en opslagservices](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage).<br/><br/> Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Scenariostappen

Dit is hoe Azure de migratie wordt voltooid:

> [!div class="checklist"]
> * **Stap 1: Azure voorbereiden voor Site Recovery**: ze maken van een Azure storage-account voor het opslaan van gerepliceerde gegevens en een Recovery Services-kluis maken.
> * **Stap 2: On-premises VMware voorbereiden voor Site Recovery**: ze accounts moet worden gebruikt voor VM-detectie- en agent-installatie voorbereiden en voorbereidingen voor het verbinding maken met virtuele Azure-machines na een failover.
> * **Stap 3:-Machines repliceren**: ze instellen van de bron- en migratie-omgeving, maakt een replicatiebeleid en beginnen met het repliceren van virtuele machines naar Azure storage.
> * **Stap 4: Migreer de virtuele machines met Site Recovery**: ze voert een testfailover uit om te controleren of alles goed werkt, en voer een volledige failover voor de virtuele machines migreren naar Azure.


## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Stap 1: Azure voorbereiden voor de Site Recovery-service

Contoso heeft een aantal Azure-onderdelen nodig voor Site Recovery:

- Failover van een VNet waarin resources zich bevinden (Contoso gebruikt de productie VNet ze al geïmplementeerd)
- Een nieuw Azure storage-account voor het opslaan van gerepliceerde gegevens. 
- Een Recovery Services-kluis in Azure.

Contoso is al gemaakt het VNet tijdens [implementatie van de Azure-infrastructuur](contoso-migration-infrastructure.md), zodat ze alleen wilt maken van een storage-account en de kluis.

1. Contoso maakt een Azure storage-account (contosovmsacc20180528) in de regio VS-Oost 2.

    - Het opslagaccount moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
    - Ze gebruikt een account voor algemeen gebruik, met de standard-opslag en LRS-replicatie.

    ![Site Recovery-opslag](./media/contoso-migration-rehost-linux-vm/asr-storage.png)

2. Met de netwerk- en storage-account in plaats Contoso een kluis (ContosoMigrationVault) maken en plak deze in de **ContosoFailoverRG** resourcegroep in de primaire regio in de VS-Oost 2.

    ![Recovery Services-kluis](./media/contoso-migration-rehost-linux-vm/asr-vault.png)


**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) instellen van Azure voor Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Stap 2: On-premises VMware voorbereiden voor Site Recovery

Contoso bereidt de on-premises VMware-infrastructuur als volgt:

- Maak een account op de vCenter-server of vSphere ESXi-host voor het automatiseren van VM-detectie.
- Maak een account waarmee de automatische installatie van de Mobility-service op VMware-VM's die u wilt repliceren.
- Bereid de on-premises VM's, zodat ze verbinding met Azure-VM's maken kunnen wanneer ze worden gemaakt na de migratie.


### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Virtuele machines automatisch detecteren. Er is minimaal een alleen-lezen-account vereist.
- Replicatie, failover en failback orkestreren. U moet een account die bewerkingen zoals het maken en verwijderen van schijven en het inschakelen van virtuele machines kan worden uitgevoerd.

Contoso stelt als volgt u het account:

1. Contoso maakt een rol op vCenter-niveau.
2. Contoso vervolgens toe te wijzen die rol de vereiste machtigingen.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op de Linux-VM's die het migreren van Contoso:

- Site Recovery kan een automatische push-installatie van dit onderdeel kunt doen wanneer u replicatie voor de virtuele machines inschakelt.
- Voor automatische push-installatie moet u een account voorbereiden waarmee Site Recovery wordt gebruikt voor toegang tot de virtuele machines.
- Details van de accounts zijn ingevoerd tijdens de installatie van de replicatie. 
- Het account kan worden domein of lokale account met machtigingen om te installeren op virtuele machines.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Na een failover naar Azure wil Contoso kunnen verbinding maken met de gerepliceerde virtuele machines in Azure. Om dit te doen, is er een aantal dingen die ze moeten gaan doen:

- Als u wilt openen via het internet, kunnen SSH in Linux on-premises virtuele machine vóór de migratie.  Voor Ubuntu dit kan worden voltooid met de volgende opdracht: **Sudo apt-get ssh in installeren -y**.
- Nadat ze de migratie (failover) uitvoert, moeten ze controleren **diagnostische gegevens over opstarten** om een schermopname van de virtuele machine weer te geven.
- Als dit niet werkt, moeten ze controleren dat de virtuele machine wordt uitgevoerd, en bekijk deze [tips voor probleemoplossing](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) maken en toewijzen van een rol voor automatische detectie.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) het maken van een account voor push-installatie van de Mobility-service.


## <a name="step-3-replicate-the-on-premises-vms"></a>Stap 3: De on-premises VM's repliceren

Voordat ze het web virtuele machine naar Azure migreren kunnen, worden de Contoso ingesteld en replicatie inschakelen.

### <a name="set-a-protection-goal"></a>Stel een beveiligingsdoel

1. In de kluis onder de kluisnaam (ContosoVMVault) ze ingesteld een replicatiedoel (**aan de slag** > **siteherstel** > **infrastructuur voorbereiden**.
2. Ze opgeven dat hun computers zich op locatie bevinden, dat ze virtuele VMware-machines, en dat ze wilt repliceren naar Azure.
    ![Replicatiedoel](./media/contoso-migration-rehost-linux-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

Als u wilt doorgaan, ze bevestigen dat zij implementatieplanning, door te selecteren hebt voltooid **Ja, heb ik gedaan**. Contoso zijn slechts één virtuele machine in dit scenario wordt gemigreerd, en hoeft-implementatie plannen.

### <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Contoso nodig heeft om de bronomgeving te configureren. U doet dit door ze een OVF-sjabloon downloaden en gebruiken om te implementeren van de Site Recovery-configuratieserver als een maximaal beschikbaar, on-premises VMware-VM. Nadat de configuratieserver actief is, Registreer ze deze in de kluis.

De configuratieserver wordt uitgevoerd voor een aantal onderdelen:

- Het configuratieserveronderdeel coördineert de communicatie tussen on-premises en Azure en beheert de gegevensreplicatie.
- Initiëren van failover met één klik in de portal of kunt u  PowerShell dat een failover wordt geactiveerd. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag.
- De processerver installeert ook Mobility Service op VM's die u wilt repliceren, en detecteert automatisch on-premises VMware-VM's.

Contoso uitvoeren als volgt te werk:

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

14. Nadat de registratie is voltooid, wordt in de Azure-portal, Contoso controleert dat de configuratieserver en de VMware-server worden weergegeven op de **bron** pagina in de kluis. Detectie kan 15 minuten of langer duren. 
15. Site Recovery maakt verbinding met de VMware-servers, en VM's worden gedetecteerd.

### <a name="set-up-the-target"></a>Het doel instellen

Nu Contoso Hiermee configureert u de replicatie-instellingen van het doel.

1. In **infrastructuur voorbereiden** > **doel**, ze de doelinstellingen selecteren.
2. Site Recovery controleert of er een Azure storage-account en het netwerk in het opgegeven doel is.

### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

Nadat de bron en het doel zijn ingesteld, is het Contoso gereed voor het maken van beleid voor replicatie.

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

Nu Contoso kunt beginnen met het repliceren van de **OSTICKETWEB** VM.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze de instellingen voor gegevensbron selecteren.
2. Ze selecteren die ze willen virtuele machines inschakelen, selecteert u de instellingen voor gegevensbron, met inbegrip van de vCenter-server en de configuratieserver.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm/enable-replication-source.png)

3. Ze geven de doelinstellingen, met inbegrip van de resourcegroep en VNet waarin de Azure-VM na failover geplaatst worden wordt en het opslagaccount waarin de gerepliceerde gegevens worden opgeslagen.

     ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm/enable-replication2.png)

3. Contoso selecteert **OSTICKETWEB** voor replicatie. 

    - In dit stadium Contoso selecteert alleen **OSTICKETWEB** omdat VNet en subnet moeten worden geselecteerd en de virtuele machines worden niet in hetzelfde subnet.
    - Site Recovery installeert automatisch de Mobility-service wanneer replicatie is ingeschakeld voor de virtuele machine.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm/enable-replication3.png)

4. Contoso selecteert in de VM-eigenschappen, het account dat wordt gebruikt door de processerver voor de Mobility-Service automatisch op de computer installeren.

     ![Mobility-service](./media/contoso-migration-rehost-linux-vm/linux-mobility.png)

5. in **replicatie-instellingen** > **replicatie-instellingen configureren**, ze controleren of het juiste replicatiebeleid toegepast, en selecteer is **inschakelen replicatie**.
6.  Volgen ze Replicatievoortgang gedurende **taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.



### <a name="enable-replication-for-osticketmysql"></a>Replicatie inschakelen voor OSTICKETMYSQL

Nu Contoso kunt beginnen met het repliceren **OSTICKETMYSQL**.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze de instellingen voor bron en doel selecteren.
2. Contoso selecteert **OSTICKETMYSQL** voor replicatie, en selecteert u het account moet worden gebruikt voor de installatie van de Mobility-service.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm/mysql-enable.png)

3. Contoso is van toepassing de hetzelfde replicatiebeleid dat is gebruikt voor OSTICKETWEB en replicatie inschakelen.  

**Meer hulp nodig?**

U kunt een overzicht van al deze stappen in lezen [inschakelen replicatie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>Stap 4: De virtuele machines migreren 

Contoso uitvoeren van een snelle testfailover en migreer de virtuele machines.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Een testfailover uitvoeren om te controleren of alles werkt zoals verwacht vóór de migratie. 

1. Contoso wordt een testfailover uitgevoerd naar de laatst beschikbare punt in tijd (**laatst verwerkte**).
2. Ze selecteren **sluit de computer voordat u begint met failover**, zodat Site Recovery wordt geprobeerd de bron-VM afsluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. 
3. Test-failover wordt uitgevoerd: 
    - Een controle uitgevoerd om te controleren of alle van de voorwaarden die zijn vereist voor de migratie is voldaan.
    - De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als de meest recente herstelpunt is geselecteerd, wordt een herstelpunt wordt gemaakt van de gegevens.
    - Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.
3. Nadat de failover is voltooid, wordt de replica virtuele machine in Azure weergegeven in de Azure-portal. Zij controleren dat de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en dat deze wordt uitgevoerd. 
4. Nadat u hebt gecontroleerd, ze opschonen van de failover, en noteer eventuele opmerkingen en sla.

### <a name="create-and-customize-a-recovery-plan"></a>Maken en aanpassen van een herstelplan

 Nadat u hebt gecontroleerd of de testfailover heeft gewerkt zoals verwacht, Contoso maken van een herstelplan voor migratie. 

- Een herstelplan Hiermee geeft u de volgorde in met een failover optreedt, hoe de Azure-VM's wordt geopend om in Azure.
- Omdat ze migreren van een app met twee lagen wilt, moeten ze het herstelplan te gaan aanpassen zodat de virtuele machine (SQLVM) van de gegevens wordt gestart voordat de front-end (WEBVM).


1. In **herstelplannen (Site Recovery)** > **+ herstelplan**, ze een plan maken en de virtuele machines aan toe te voegen.

    ![Plan voor herstel](./media/contoso-migration-rehost-linux-vm/recovery-plan.png)

2. Na het maken van het abonnement, ze selecteert u deze voor aanpassing (**herstelplannen** > **OsTicketMigrationPlan** > **aanpassen**.
3.  Ze verwijderen **OSTICKETWEB** van **groep 1: Start**.  Dit zorgt ervoor dat de eerste startactie is van invloed op **OSTICKETMYSQL** alleen.

    ![Groep voor gegevensherstel](./media/contoso-migration-rehost-linux-vm/recovery-group1.png)

4.  In **+ groep** > **toevoegen beveiligde items**, het toevoegen van **OSTICKETWEB** naar **groep 2: Start**.  Contoso moet deze in twee verschillende groepen.

    ![Groep voor gegevensherstel](./media/contoso-migration-rehost-linux-vm/recovery-group2.png)


### <a name="migrate-the-vms"></a>De virtuele machines migreren


Contoso zijn gereed voor het uitvoeren van een failover op het herstelplan te gaan, voor het migreren van de virtuele machines.

1. Ze selecteert u het plan > **Failover**.
2.  Ze selecteren fungeren als failover voor de meest recente herstelpunt, en opgeven dat Site Recovery proberen moet om de on-premises VM af voordat de failover wordt geactiveerd te. Ze kunnen de voortgang van de failover volgen op de **taken** pagina.

    ![Failover](./media/contoso-migration-rehost-vm/failover1.png)

3. Tijdens de failover geeft de vCenter-Server opdrachten om te stoppen van de twee virtuele machines die worden uitgevoerd op de ESXi-host.

    ![Failover](./media/contoso-migration-rehost-linux-vm/vcenter-failover.png)

3. Na de failover Contoso controleren of de Azure-VM wordt weergegeven zoals verwacht in de Azure-portal.

    ![Failover](./media/contoso-migration-rehost-linux-vm/failover2.png)  

3. Nadat u hebt de virtuele machine in Azure gecontroleerd, voltooien ze de migratie voor het voltooien van het migratieproces voor elke virtuele machine. Deze replicatie voor de virtuele machine wordt gestopt en Site Recovery-facturering voor de virtuele machine stopt.

    ![Failover](./media/contoso-migration-rehost-linux-vm/failover3.png)


### <a name="connect-the-vm-to-the-database"></a>De virtuele machine verbinding met de database

Als de laatste stap in het migratieproces, Contoso bijwerken van de verbindingsreeks van de toepassing om te verwijzen naar de app-database op de **OSTICKETMYSQL** VM. 

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

Contoso moet nu enkele opruiming uitvoeren:  

- Ze verwijderen de on-premises VM's uit de vCenter-inventaris.
- Ze verwijderen de on-premises VM's uit lokale back-uptaken.
- Bijwerken van hun interne documentatie om weer te geven van de nieuwe locatie en IP-adressen voor OSTICKETWEB en OSTICKETMYSQL.
- Ze alle resources die interactie met de virtuele machines hebben controleren en bijwerken van de relevante instellingen of documentatie om de nieuwe configuratie weer te geven.
- Contoso gebruikt de service Azure Migrate met afhankelijkheidstoewijzing om te beoordelen van de VM's voor migratie. Ze moeten de Microsoft Monitoring Agent en de Agent voor afhankelijkheden ze geïnstalleerd voor dit doel, van de virtuele machine verwijderen.

## <a name="review-the-deployment"></a>De implementatie controleren

Met de app nu wordt uitgevoerd, moet Contoso volledig operationeel maken en hun nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

Het Contoso security team Bekijk de OSTICKETWEB en OSTICKETMYSQLVMs om beveiligingsproblemen te bepalen.

- Ze controleren de Netwerkbeveiligingsgroepen (nsg's) voor de virtuele machines om toegang te beheren. Nsg's worden gebruikt om ervoor te zorgen dat alleen verkeer dat is toegestaan voor de toepassing kan worden doorgegeven.
- Ze zijn ook overwegen de gegevens op de VM-schijven met behulp van versleuteling van schijf en Azure Key Vault te beveiligen.

[Lees meer](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) over procedures voor beveiliging voor virtuele machines.

### <a name="backups"></a>Back-ups

Contoso back-up van de gegevens op de virtuele machines met behulp van de Azure Backup-service. [Meer informatie](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

- Na het implementeren van resources, Contoso wijst Azure tags zoals gedefinieerd tijdens de [implementatie van de Azure-infrastructuur](contoso-migration-infrastructure.md#set-up-tagging).
- Contoso heeft geen licentie problemen met de Ubuntu-servers.
- Contoso kunnen Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Het is een kostenbeheeroplossing met meerdere Clouds management-oplossing die u helpt bij het gebruiken en beheren van Azure en andere cloudresources.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over Azure Cost Management. 


## <a name="next-steps"></a>Volgende stappen

In dit artikel die we hebben laten zien hoe Contoso gemigreerd gelaagd een on-premises servicedesk-app op twee Linux-VM's naar Azure IaaS-VM's, met behulp van Azure Site Recovery.

In het volgende artikel in de reeks leert u hoe Contoso de dezelfde servicedesk-app migreren naar Azure. Deze keer Contoso maakt gebruik van Site Recovery voor het migreren van de front-end VM voor de app en migreren van de app-database met behulp van back-up en herstellen met Azure Database voor MySQL, met behulp van het hulpprogramma MySQL workbench. [Aan de slag](contoso-migration-rehost-linux-vm-mysql.md).
