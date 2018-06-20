---
title: Een app Contoso Linux service helpdesk Azure en Azure MySQL rehost | Microsoft Docs
description: Meer informatie over hoe Contoso een lokale Linux-app naamconflicten door te migreren naar Azure VM's en MySQL in Azure.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 15a429c033cfd1598dd01b5c8cd2743c397dacdb
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36225514"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms-and-azure-mysql"></a>Migratie van Contoso: Rehost van een lokale Linux-app aan Azure virtuele machines en MySQL in Azure

Dit artikel laat zien hoe Contoso zijn configureren (rehost) hun lokale twee lagen Linux service helpdesk-app (osTicket) door te migreren naar Azure en MySQL in Azure.

Dit document is de achtste in een reeks artikelen die laten zien hoe de lokale bronnen in het fictieve bedrijf Contoso worden gemigreerd naar de Microsoft Azure-cloud. De reeks bevat achtergrondinformatie en scenario's te laten zien hoe u een migratie-infrastructuur instellen en uitvoeren van verschillende soorten migraties. Scenario's toenemen in complexiteit en worden er nog meer artikelen gedurende een bepaalde periode.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de serie artikelen en de voorbeeld-apps we gebruiken. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. De infrastructuur wordt gebruikt voor alle Contoso migratiescenario's. | Beschikbaar
[Artikel 3: Beoordelen lokale bronnen](contoso-migration-assessment.md)  | Toont hoe een beoordeling van de lokale twee lagen SmartHotel app uitgevoerd in VMware in Contoso wordt uitgevoerd. Evalueren van de virtuele machines van een app met de [Azure migreren](migrate-overview.md) -service en de app SQL Server-database met de [Azure Database migratie-assistent](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
[Artikel 4: Rehost virtuele machines in Azure en een beheerde SQL-exemplaar](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstreert hoe Contoso de app SmartHotel migreert naar Azure. Migreren van de app web virtuele machine met behulp [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database met de [Azure databasemigratie](https://docs.microsoft.com/azure/dms/dms-overview) service om te migreren naar een SQL-exemplaar worden beheerd. | Beschikbaar
[Artikel 5: Rehost voor virtuele machines in Azure](contoso-migration-rehost-vm.md) | Toont hoe Contoso hun SmartHotel migreren naar Azure IaaS VM's, met behulp van de Site Recovery-service.
[Artikel 6: Rehost virtuele machines in Azure en SQL Server-beschikbaarheidsgroepen](contoso-migration-rehost-vm-sql-ag.md) | Toont hoe de app SmartHotel in Contoso worden gemigreerd. Deze Site Recovery gebruiken voor het migreren van virtuele machines van de app en de migratie van de Database-service om te migreren van de app-database naar een SQL Server-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een app Linux virtuele machines in Azure Rehost](contoso-migration-rehost-linux-vm.md) | Toont hoe Contoso hun osTicket Linux app migreert naar Azure IaaS VM's met Azure Site Recovery.
Artikel 8: Rehost een Linux-app op Azure VM's en Azure MySQL-Server (in dit artikel) | Laat zien hoe de osTicket Linux-app in Contoso worden gemigreerd. Ze Site Recovery voor VM-migratie en MySQL Workbench gebruiken om te migreren naar een Azure MySQL Server-exemplaar. | Beschikbaar

In dit artikel migreert Contoso een tweelagige Linux Apache MySQL PHP (licht) service helpdesk-app (osTicket) naar Azure. Als u deze app open source gebruiken wilt, kunt u het downloaden van [GitHub](https://github.com/osTicket/osTicket).



## <a name="business-drivers"></a>Zakelijke drijfveren

De IT-leiding heeft nauw samengewerkt met hun zakelijke partners om te begrijpen wat ze wilt bereiken:

- **Zakelijke groei adres**: Contoso groeit en er is als gevolg hiervan druk op de on-premises systemen en de infrastructuur.
- **Beperk risico**: de app service-helpdesk is essentieel voor het bedrijf Contoso. Ze willen verplaatsen naar Azure met nul risico.
- **Uitbreiden**: Contoso niet wilt wijzigen van de app nu. Ze willen gewoon blijven stabiele.


## <a name="migration-goals"></a>Doelstellingen van de migratie

Het team van de cloud Contoso heeft om te bepalen van de beste migratiemethode vastgemaakt omlaag doelstellingen voor deze migratie:

- Na de migratie, moet de app in Azure dezelfde prestatiemogelijkheden hebben als in hun on-premises VMWare-omgeving.  De app blijft in de cloud als kritiek omdat deze lokale. 
- Contoso wil niet investeren in deze app.  Het is belangrijk voor het bedrijf zijn, maar in de huidige vorm ze gewoon wilt veilig te verplaatsen naar de cloud.
- Nadat een aantal migraties van Windows-app is voltooid, wil Contoso meer informatie over het gebruik van een infrastructuur op basis van Linux in Azure.
- Contoso wil minimaliseren beheertaken database nadat de toepassing wordt verplaatst naar de cloud.

## <a name="proposed-architecture"></a>Voorgestelde architectuur

In dit scenario geldt het volgende:

- De app is gelaagd over twee VM's (OSTICKETWEB en OSTICKETMYSQL).
- De virtuele machines zich bevinden op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5).
- De VMware-omgeving wordt beheerd met vCenter Server 6.5 (**vcenter.contoso.com**), uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een lokale domeincontroller (**contosodc1**).
- De web-app-laag op OSTICKETWEB worden gemigreerd naar een Azure IaaS VM.
- De app-database worden gemigreerd naar de Azure-Database voor MySQL PaaS-service.
- Omdat ze een productie-werkbelasting migreert bent, de bronnen bevinden zich in de resourcegroep productie **ContosoRG**.
- De resources worden gerepliceerd naar de primaire regio (VS-Oost 2), en worden in het productienetwerk (VNET-PROD-EUS2):
    - Het web VM zich bevindt in het subnet frontend (PROD FE EUS2).
    - Het database-exemplaar wordt opgeslagen in de database-subnet (PROD-DB-EUS2).
- De app-database worden gemigreerd naar Azure MySQL met MySQL-hulpprogramma's.
- De lokale virtuele machines in het datacenter Contoso wordt buiten gebruik worden gesteld nadat de migratie is voltooid.


![Scenario-architectuur](./media/contoso-migration-rehost-linux-vm-mysql/architecture.png) 


## <a name="migration-process"></a>migratieproces

Contoso voltooit het migratieproces als volgt:

Het web VM migreren:

1. Als een eerste stap Contoso stelt de Azure en on-premises infrastructuur die nodig zijn voor de Site Recovery implementeert.
2. Na het voorbereiden van de onderdelen van Azure en on-premises ze instellen en inschakelen van replicatie voor de virtuele machine.
3. Als de replicatie is uitgevoerd en-up, migreer ze de virtuele machine failover naar Azure.

De database migreren:

1. Contoso levert een MySQL-exemplaar in Azure.
2. Deze MySQL workbench instellen en back-up van de database lokaal.
3. Ze vervolgens de database terugzetten vanaf de lokale back-up naar Azure.

![migratieproces](./media/contoso-migration-rehost-linux-vm-mysql/migration-process.png) 


### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | De service ingedeeld en beheert de migratie en herstel na noodgevallen voor Azure VM's en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, zijn Azure Storage kosten verbonden.  Virtuele machines in Azure worden gemaakt en kosten wanneer failover plaatsvindt. [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over kosten en prijzen.
[Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) | De database is gebaseerd op de open source-engine MySQL-Server. Het biedt een volledig beheerde, enterprise-ready community MySQL-database als een service voor app-ontwikkeling en implementatie. 

 
## <a name="prerequisites"></a>Vereisten

Als u (en Contoso) wilt dit scenario worden uitgevoerd, is wat u moet hebben.

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | U moet al hebt gemaakt een abonnement tijdens vroege artikelen in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement en u niet de beheerder bent, moet u samen met de beheerder u eigenaar of bijdrager machtigingen toewijzen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-infrastructuur** | Instellen van Azure-infrastructuur, zoals beschreven in Contoso [Azure-infrastructuur voor de migratie](contoso-migration-infrastructure.md).<br/><br/> Meer informatie over specifieke [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) vereisten voor de Site Recovery.
**On-premises servers** | Uw lokale vCenter-server moet versie 5.5, 6.0 of 6.5 worden uitgevoerd<br/><br/> Een ESXi-host waarop versie 5.5, 6.0 of 6.5<br/><br/> Een of meer virtuele VMware-machines uitgevoerd op de ESXi-host.
**On-premises virtuele machines** | [Vereisten voor Linux VM doornemen](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) die worden ondersteund voor migratie met Site Recovery.<br/><br/> Controleer of ondersteund [Linux-systemen voor bestands- en opslagservices](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage).<br/><br/> Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Azure voltooit de migratie:

> [!div class="checklist"]
> * **Stap 1: Voorbereiden op Azure Site Recovery**: U maakt een Azure storage-account gerepliceerde gegevens kan bevatten en een Recovery Services-kluis maken.
> * **Stap 2: Lokale VMware voorbereiden voor siteherstel**: U accounts voorbereiden voor VM-detectie en agent-installatie en verbinding maken met virtuele Azure-machines na een failover voorbereiden.
 * **Stap 3: inrichting van de database]**: inrichten In Azure, een exemplaar van Azure MySQL-database.
> * **Stap 4: Replicatie VMs**: configureert u de Site Recovery-bron en doel-omgeving, instellen van een beleid voor wachtwoordreplicatie en virtuele machines repliceren naar Azure-opslag.
> * **Stap 5: Migreren van de database**: instellen van de migratie met MySQL-hulpprogramma's.
> * **Stap 6: Migreer de virtuele machines met Site Recovery**: uitvoeren van een testfailover om te controleren of alles werkt en voer een volledige failover naar de virtuele machines migreren naar Azure.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Stap 1: Voorbereiden op Azure Site Recovery-service

Contoso heeft een aantal Azure-onderdelen voor Site Recovery nodig:

- Failover van een VNet waarin resources zich bevinden (Contoso gebruikt de productie VNet ze al geïmplementeerd).
- Een nieuwe Azure storage-account voor gerepliceerde gegevens. 
- Een Recovery Services-kluis in Azure.

Contoso is al gemaakt het VNet tijdens [Azure-infrastructuur implementatie](contoso-migration-infrastructure.md), zodat ze hoeft aan te maken van een opslagaccount en de kluis.


1. Contoso maakt een Azure storage-account (**contosovmsacc20180528**) in de regio VS-Oost 2.

    - Het opslagaccount moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
    - Contoso maakt gebruik van een account voor algemene doeleinden met standard-opslag en LRS-replicatie.

    ![Site Recovery-opslag](./media/contoso-migration-rehost-linux-vm-mysql/asr-storage.png)

3. Met de netwerk- en storage-account in plaats Contoso maakt een kluis (ContosoMigrationVault) en plaatst deze in de **ContosoFailoverRG** resourcegroep in de primaire regio van de VS-Oost 2.

    ![Recovery Services-kluis](./media/contoso-migration-rehost-linux-vm-mysql/asr-vault.png)

**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Azure instellen voor de Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Stap 2: On-premises VMware voor herstel van sites voorbereiden

Contoso bereidt de on-premises VMware-infrastructuur als volgt:

- Maakt een account op de vCenter-server voor het automatiseren van VM-detectie.
- Hiermee maakt u een account waarmee de automatische installatie van de Mobility-service op de VMware-machines die u wilt repliceren.
- Wordt voorbereid op de lokale virtuele machines, zodat ze verbinding met virtuele Azure-machines maken kunnen wanneer ze zijn gemaakt na de migratie.


### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Virtuele machines automatisch detecteren. Er is minimaal een alleen-lezen-account vereist.
- Replicatie, failover en failback orkestreren. U moet een account die bewerkingen zoals het maken en schijven verwijderen en het inschakelen van virtuele machines kan worden uitgevoerd.

Contoso stelt u het account als volgt:

1. Contoso maakt een rol op de vCenter-niveau.
2. Contoso wijst die rol vervolgens de vereiste machtigingen.


### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op elke virtuele machine die Contoso wil migreren.

- Site Recovery kan een automatische push-installatie van dit onderdeel kunt doen wanneer u replicatie voor de virtuele machines inschakelen.
- Automatische installatie. Site Recovery moet een account met machtigingen voor toegang tot de virtuele machine. 
- Accountgegevens zijn ingevoerd tijdens de installatie van de replicatie. 
- Het account kan worden domein of lokale account, zolang deze Installatiemachtigingen heeft.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Na een failover naar Azure wil Contoso verbinding kunnen maken met de Azure VM's. Om dit te doen, moeten zij een aantal dingen doen: 

- Als u wilt openen via het internet, kunnen SSH op de lokale Linux VM vóór de migratie.  Voor Ubuntu kan dit worden uitgevoerd met de volgende opdracht: **apt Sudo get-ssh in installeren -y**.
- Na de failover moeten ze controleren **opstarten diagnostics** om een schermopname van de virtuele machine weer te geven.
- Als dit niet werkt, moeten ze controleren dat de virtuele machine wordt uitgevoerd en deze [tips voor probleemoplossing](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) maken en toewijzen van een functie voor automatische detectie.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) maken van een account voor push-installatie van de Mobility-service.


## <a name="step-3-provision-azure-database-for-mysql"></a>Stap 3: Azure Database inrichten voor MySQL

Contoso voorziet in een MySQL-database-exemplaar in de primaire regio van de VS-Oost 2.

1. In de Azure portal maken ze een Azure-Database voor MySQL-resource. 

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-1.png)

2. Ze de naam toevoegen **contosoosticket** voor de Azure-database. Ze de database toevoegen aan de resourcegroep productie **ContosoRG**, en geef referenties op voor deze.
3. De lokale MySQL-database is versie 5.7, zodat ze deze versie voor compatibiliteit selecteren. Ze gebruiken de standaardgrootte die overeenkomen met de vereisten voor de database.

     ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-2.png)

4. Voor **back-up redundantieopties**, Contoso worden geselecteerd om te gebruiken **geografisch redundante**. Deze optie kan ze de database in de secundaire regio in de VS-midden herstellen als er een storing optreedt. Ze kunnen deze optie alleen configureren wanneer de inrichting van de database.

     ![Redundantie](./media/contoso-migration-rehost-linux-vm-mysql/db-redundancy.png)

4. In de **VNET-PROD-EUS2** netwerk > **Service-eindpunten**, het toevoegen van een service-eindpunt (een subnet van de database) voor de SQL-service.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-3.png)

5. Na het toevoegen van het subnet ze een virtueel netwerk regel maken waarmee toegang vanuit het subnet van de database in het productienetwerk toestaat.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-4.png)


## <a name="step-4-replicate-the-on-premises-vms"></a>Stap 4: De on-premises virtuele machines repliceren

Voordat ze het web VM naar Azure migreren kunnen, stelt u Contoso- en replicatie inschakelen.

### <a name="set-a-protection-goal"></a>Een beveiligingsdoel instellen

1. In de kluis onder de kluisnaam van de (ContosoVMVault) ze ingesteld een replicatie-doel (**aan de slag** > **siteherstel** > **infrastructuur voorbereiden**.
2. Deze opgeven dat hun computers zich op locatie bevinden, dat ze zijn virtuele VMware-machines, en dat ze wilt repliceren naar Azure.

    ![Replicatiedoel](./media/contoso-migration-rehost-linux-vm-mysql/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Controleer de implementatie plannen

Als u wilt doorgaan, zij bevestigen dat ze implementatie plannen door te selecteren hebt voltooid **Ja, ik dit hebt gedaan**. Contoso zijn slechts één VM in dit scenario wordt gemigreerd en hoeft niet-implementatie plannen.

### <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Contoso moet configureren hun bronomgeving. U doet dit door gebruik een OVF-sjabloon implementeren van een Site Recovery-configuratieserver als een maximaal beschikbare on-premises VMware VM. Nadat de configuratieserver actief en werkend is, registreren ze in de kluis.

De configuratieserver wordt uitgevoerd voor een aantal onderdelen:

- Het configuratieserveronderdeel die coördineert de communicatie tussen de on-premises en Azure en beheert gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag.
- De processerver installeert ook Mobility Service op VM's die u wilt repliceren, en detecteert automatisch on-premises VMware-VM's.

Contoso uitvoeren als volgt te werk:


1. Downloaden van de sjabloon OVF **infrastructuur voorbereiden** > **bron** > **configuratieserver**.
    
    ![OVF downloaden](./media/contoso-migration-rehost-linux-vm-mysql/add-cs.png)

2. Ze importeren van de sjabloon in VMware voor de virtuele machine maken en implementeren van de virtuele machine.

    ![OVF-sjabloon](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-wizard.png)

3. Wanneer ze de virtuele machine voor het eerst inschakelt, wordt deze opgestart naar een Windows Server 2016-installatie-ervaring. Ze de licentieovereenkomst accepteren en voer een administrator-wachtwoord.
4. Nadat de installatie is voltooid, ze zich aanmelden bij de virtuele machine als de beheerder. Bij de eerste aanmelding, wordt standaard de Azure Site Recovery Configuration Tool uitgevoerd.
5. In het hulpmiddel Geef ze een naam voor de configuratieserver in de kluis te registreren.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure.
7. Nadat de verbinding tot stand is gebracht, ze zich aanmelden bij de Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin zij de configuratieserver moeten registreren.

    ![Configuratieserver registreren](./media/contoso-migration-rehost-linux-vm-mysql/config-server-register2.png)

8. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
9. Ze opnieuw aan te melden bij de computer en de configuratiewizard voor het beheer van Server automatisch wordt gestart.
10. In de wizard selecteren ze de NIC van binnenkomend replicatieverkeer. Deze instelling kan niet worden gewijzigd nadat deze geconfigureerd.
11. Ze selecteert het abonnement, de resourcegroep en de kluis waarin de configuratieserver registreren.

    ![Kluis](./media/contoso-migration-rehost-linux-vm-mysql/cswiz1.png) 

12. Nu downloaden en installeren van MySQL-Server en VMWare PowerCLI. 
13. Na de validatie Geef ze de FQDN-naam of IP-adres van de vCenter-server of vSphere-host. Ze laat de standaardpoort en geef een beschrijvende naam voor de vCenter-server.
14. Ze het account dat ze voor automatische detectie hebt gemaakt en de referenties die door Site Recovery wordt gebruikt voor het installeren van de Mobility-Service automatisch worden ingevoerd. 

    ![vCenter](./media/contoso-migration-rehost-linux-vm-mysql/cswiz2.png)

14. Nadat de registratie is voltooid, wordt in de Azure portal Contoso controleert of de configuratieserver en de VMware-server aanwezig zijn op de **bron** pagina in de kluis. Detectie kan 15 minuten of langer duren. 
15. Met alles in plaats Site Recovery maakt verbinding met de VMware-servers en virtuele machines worden gedetecteerd.

### <a name="set-up-the-target"></a>Het doel instellen

Contoso invoer doel nu replicatie-instellingen.

1. In **infrastructuur voorbereiden** > **doel**, ze de doelinstellingen selecteren.
2. Site Recovery controleert of er een Azure storage-account en het netwerk in het opgegeven doel is.


### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

Contoso is gereed voor het maken van een beleid voor wachtwoordreplicatie met de bron en doel instellen.

1. In **infrastructuur voorbereiden** > **replicatie-instellingen** > **replicatiebeleid** >  **maken en Koppelen**, maak een beleid **ContosoMigrationPolicy**.
2. Ze de standaardinstellingen gebruiken:
    - **Drempelwaarde voor RPO**: standaard 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
    - **Herstel bewaarperiode**. De standaardwaarde van 24 uur. Deze waarde geeft aan hoe lang de bewaarperiode voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
    - **App-consistente momentopname frequentie**. De standaardwaarde van één uur. Deze waarde bepaalt de frequentie waarmee toepassingsconsistente momentopnamen worden gemaakt.
 
        ![Replicatiebeleid maken](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy.png)

5. Het beleid wordt automatisch gekoppeld aan de configuratieserver. 

    ![Beleid voor wachtwoordreplicatie koppelen](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy2.png)


**Meer hulp nodig?**

- U kunt een overzicht van deze stappen in lezen [herstel na noodgevallen instellen voor de lokale virtuele VMware-machines](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Gedetailleerde instructies zijn beschikbaar om u te helpen [de bronomgeving instellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementeren van de configuratieserver](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), en [replicatie-instellingen configureren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) over de Azure-gastagent voor Linux.

### <a name="enable-replication-for-the-web-vm"></a>Replicatie voor de Web-virtuele machine inschakelen

Nu Contoso kunt beginnen met het repliceren van de **OSTICKETWEB** VM.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze selecteert u de instellingen van de bronserver.
2. Ze geven aan dat ze willen, kunnen virtuele machines en selecteer de broninstellingen, met inbegrip van de vCenter-server en de configuratieserver.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication-source.png)

3. Nu dat ze de doelinstellingen opgeven. Het gaat hierbij om de resourcegroep en netwerk waarin de virtuele machine in Azure geplaatst na een failover worden en het opslagaccount waarin de gerepliceerde gegevens worden opgeslagen. 

     ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication2.png)

3. Contoso selecteert **OSTICKETWEB** voor replicatie. 

    ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication3.png)

4. In de VM-eigenschappen selecteert ze het account dat moet worden gebruikt voor het installeren van de Mobility-Service automatisch op de virtuele machine.

     ![Mobility-service](./media/contoso-migration-rehost-linux-vm-mysql/linux-mobility.png)

5. in **replicatie-instellingen** > **replicatie-instellingen configureren**, ze controleren dat de juiste replicatiegroep beleid toegepast en selecteer is **replicatie inschakelen**. De Mobility-service wordt automatisch geïnstalleerd.
6.  Bijhouden van de Replicatievoortgang van de in **taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.


**Meer hulp nodig?**

U kunt een overzicht van deze stappen in lezen [replicatie inschakelen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-5-migrate-the-database"></a>Stap 5: Migreren van de database

Contoso worden de database met backup and restore met MySQL-hulpprogramma's worden gemigreerd. Ze MySQL Workbench installeren, back-up van de database van OSTICKETMYSQL en vervolgens herstelt naar Azure-Database voor de MySQL-Server.

### <a name="install-mysql-workbench"></a>MySQL Workbench installeren

1. Contoso controleert de [vereisten en downloads MySQL Workbench](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool).
2. Installatie van MySQL Workbench voor Windows in overeenstemming met de [installatie-instructies](https://dev.mysql.com/doc/workbench/en/wb-installing.html).
3. In de MySQL Workbench maken ze een MySQL-verbinding met OSTICKETMYSQL. 

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench1.png)

4. Exporteren van de database als **osticket**, naar een lokaal zelfstandig bestand.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench2.png)

5. Nadat de database is lokaal reservekopie, maken ze een verbinding met de Azure-Database voor MySQL-exemplaar.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench3.png)

6. Contoso kan nu (herstel) van de database in het Azure MySQL-exemplaar van de ingesloten bestand importeren. Een nieuw schema (osticket) wordt voor het exemplaar gemaakt.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench4.png)

## <a name="step-6-migrate-the-vms-with-site-recovery"></a>Stap 6: Migreer de virtuele machines met Site Recovery

Contoso uitvoeren van een snelle failover testen en vervolgens de virtuele machine te migreren.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Een testfailover uitgevoerd controleren helpt of alles werkt zoals verwacht, vóór de migratie. 

1. Contoso wordt een testfailover uitgevoerd naar het laatste punt in tijd (**meest recente verwerkte**).
2. Ze selecteren **machine afsluiten voordat u begint met failover**, zodat de Site Recovery probeert de bron-VM afsluiten voordat de failover. De failover wordt voortgezet zelfs als het afsluiten is mislukt. 
3. Test failover worden uitgevoerd: 

    - Controle wordt uitgevoerd om te controleren of alle van de voorwaarden die zijn vereist voor de migratie zijn in plaats van een vereisten.
    - De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als u het meest recente herstelpunt selecteert, wordt een herstelpunt van de gegevens gemaakt.
    - Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.

3. Nadat de failover is voltooid, wordt de replica virtuele machine in Azure weergegeven in de Azure portal. Contoso controleert of de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en dat deze wordt uitgevoerd. 
4. Nadat u hebt gecontroleerd, ze opschonen van de failover en vastleggen en opslaan van eventuele opmerkingen.

### <a name="migrate-the-vm"></a>De virtuele machine migreren

Contoso maakt u een herstelplan met de virtuele machine en het plan via naar Azure, niet voor het migreren van de virtuele machine.

1. Contoso maakt een plan en voegt **OSTICKETWEB** aan.

    ![Plan voor herstel](./media/contoso-migration-rehost-linux-vm-mysql/recovery-plan.png)

2. Ze een failover worden uitgevoerd op het plan. Deze het meest recente herstelpunt selecteren en opgeven dat de Site Recovery proberen moet de lokale virtuele machine afsluiten voordat de failover. Kan de voortgang van de failover volgen op de **taken** pagina.

    ![Failover](./media/contoso-migration-rehost-linux-vm-mysql/failover1.png)

3. VCenter-Server geeft tijdens de failover opdrachten om te stoppen van de twee virtuele machines die wordt uitgevoerd op de ESXi-host.

    ![Failover](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-failover.png)

4. Na de failover verifieert Contoso dat de virtuele machine in Azure wordt weergegeven zoals verwacht in de Azure portal.

    ![Failover](./media/contoso-migration-rehost-linux-vm-mysql/failover2.png)  

5. Nadat de virtuele machine is gecontroleerd, voltooien ze de migratie. Dit wordt gestopt replicatie voor de virtuele machine en Hiermee stopt u Site Recovery facturering voor de virtuele machine.

    ![Failover](./media/contoso-migration-rehost-linux-vm-mysql/failover3.png)

**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) een testfailover uitgevoerd. 
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) een herstelplan maken.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) failover wordt uitgevoerd naar Azure.


### <a name="connect-the-vm-to-the-database"></a>De virtuele machine verbinden met de database

Als de laatste stap in het migratieproces bijwerken Contoso de verbindingsreeks van de app om te verwijzen naar de Azure-Database voor MySQL. 

1. Zij een SSH-verbinding voor VM OSTICKETWEB Putty gebruiken of een andere SSH-client. De virtuele machine is een privéverbinding zodat ze verbinding maken met behulp van de privé IP-adres.

    ![Verbinding maken met database](./media/contoso-migration-rehost-linux-vm-mysql/db-connect.png)  

    ![Verbinding maken met database](./media/contoso-migration-rehost-linux-vm-mysql/db-connect2.png)  

2. Deze instellingen worden bijgewerkt zodat de **OSTICKETWEB** VM kan communiceren met de **OSTICKETMYSQL** database. De configuratie is momenteel vastgelegd met het lokale IP-adres 172.16.0.43.

    **Vóór de update**
    
    ![IP bijwerken](./media/contoso-migration-rehost-linux-vm-mysql/update-ip1.png)  

    **Na de update**
    
    ![IP bijwerken](./media/contoso-migration-rehost-linux-vm-mysql/update-ip2.png) 
    
    ![IP bijwerken](./media/contoso-migration-rehost-linux-vm-mysql/update-ip3.png) 

3. Ze start de service met **systemctl opnieuw apache2**.

    ![Opnieuw starten](./media/contoso-migration-rehost-linux-vm-mysql/restart.png) 

4. Ten slotte bijwerken van de DNS-records voor **OSTICKETWEB**, op een van de Contoso-domeincontrollers.

    ![DNS bijwerken](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 


##  <a name="clean-up-after-migration"></a>Na de migratie opschonen

Met de migratie is voltooid, worden de lagen van de app osTicket uitgevoerd op Azure Virtual machines.

Contoso moet nu het volgende doen: 
- De virtuele VMware-machines verwijderen uit de vCenter-inventaris
- Verwijder de lokale virtuele machines van de lokale back-uptaken.
- Interne documentatie update nieuwe locaties en IP-adressen weergeven. 
- Controleer alle bronnen die met de lokale virtuele machines communiceren en eventuele relevante instellingen of documentatie in overeenstemming met de nieuwe configuratie bijwerken.
- Contoso gebruikt de service Azure migreren met de afhankelijkheid toewijzing om vast te stellen de **OSTICKETWEB** VM voor migratie. Ze moeten nu de agents (Microsoft Monitoring Agent/afhankelijkheid Agent) ze geïnstalleerd voor dit doel van de virtuele machine verwijderen.

## <a name="review-the-deployment"></a>Controleer de implementatie

Met de app nu uitgevoerd, moet Contoso volledig operationeel maken en hun nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

De Contoso-beveiligingsteam Controleer de virtuele machine en de database om te bepalen van beveiligingsproblemen.

- Ze controleren de Netwerkbeveiligingsgroepen (nsg's) voor de virtuele machine, om toegang te beheren. Nsg's worden gebruikt om ervoor te zorgen dat alleen verkeer toegestaan voor de toepassing kan worden doorgegeven.
- Ze Houd rekening met het beveiligen van de gegevens op de VM-schijven met versleuteling van schijf en Azure KeyVault.
- Communicatie tussen de virtuele machine en de database-exemplaar is niet geconfigureerd voor SSL. Ze moeten doen om ervoor te zorgen dat databaseverkeer kan niet worden hacked.

[Lees meer](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) over procedures voor beveiliging voor virtuele machines.

### <a name="backups"></a>Back-ups

- Contoso wordt back-up de gegevens op de virtuele machine met behulp van de Azure Backup-service. [Meer informatie](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- Ze hoeft niet te configureren, back-up voor de database. Azure MySQL-Database maakt automatisch een back-ups server en wordt opgeslagen. Deze geografische redundantie gebruiken voor de database, zodat deze robuuste en gereed voor productie is geselecteerd.

### <a name="licensing-and-cost-optimization"></a>Licenties en kosten-optimalisatie

- Na implementatie van resources, wijst Contoso Azure tags, in overeenstemming met beslissingen ze tijdens de [Azure-infrastructuur](contoso-migration-infrastructure.md#set-up-tagging) implementatie.
- Er zijn geen licenties problemen voor de Contoso Ubuntu-servers.
- Contoso kunnen Azure kosten Management door Cloudyn, een Microsoft-vestiging in licentie gegeven. Het is een oplossing voor het beheer van meerdere cloud kosten die u helpt te kunnen gebruiken en beheren van Azure en andere cloudbronnen.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over het beheer van Azure kosten.


## <a name="next-steps"></a>Volgende stappen

In dit scenario bleek we de uiteindelijke rehost-scenario dat Contoso geprobeerd uit. Ze de frontend VM van de lokale Linux osTicket app gemigreerd naar een Azure-virtuele machine en de app-database naar een exemplaar van Azure MySQL gemigreerd.

In de volgende reeks zelfstudies in de reeks migratie gaan we om weer te geven hoe de Contoso uitgevoerd een complexe reeks migraties met betrekking tot app refactoring, in plaats van eenvoudige lift en shift-migraties.
