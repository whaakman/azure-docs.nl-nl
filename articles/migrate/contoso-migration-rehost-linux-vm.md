---
title: Rehost migreren en een app lokale Linux virtuele Azure-machines rehost | Microsoft Docs
description: Meer informatie over hoe Contoso een lokale Linux-app rehost door te migreren naar Azure VM's.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 8f039884ca0ea46c128078984d6cab6fd29ac9af
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36220547"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms"></a>Migratie van Contoso: Rehost van een app lokale Linux virtuele Azure-machines

Dit artikel laat zien hoe Contoso is configureren (rehost) een lokale service op basis van Linux helpdesk-app (**osTicket**), virtuele Azure IaaS-machines.

Dit document is de zevende in een serie artikelen met hoe de lokale bronnen in het fictieve bedrijf Contoso worden gemigreerd naar de Microsoft Azure-cloud-document. De reeks bevat achtergrondinformatie en een aantal scenario's die laat zien hoe u een migratie-infrastructuur instellen en uitvoeren van verschillende soorten migraties. Scenario's toenemen in complexiteit en worden er nog meer artikelen gedurende een bepaalde periode.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de serie artikelen en de voorbeeld-apps we gebruiken. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. De infrastructuur wordt gebruikt voor alle Contoso migratiescenario's. | Beschikbaar
[Artikel 3: Beoordelen lokale bronnen](contoso-migration-assessment.md)  | Toont hoe een beoordeling van de lokale twee lagen SmartHotel app uitgevoerd in VMware in Contoso wordt uitgevoerd. Evalueren van de virtuele machines van een app met de [Azure migreren](migrate-overview.md) -service en de app SQL Server-database met de [Azure Database migratie-assistent](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
[Artikel 4: Rehost virtuele machines in Azure en een beheerde SQL-exemplaar](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstreert hoe Contoso de app SmartHotel migreert naar Azure. Migreren van de app frontend virtuele machine met behulp [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database met de [Azure databasemigratie](https://docs.microsoft.com/azure/dms/dms-overview) service om te migreren naar een SQL-exemplaar worden beheerd. | Beschikbaar
[Artikel 5: Rehost voor virtuele machines in Azure](contoso-migration-rehost-vm.md) | Toont hoe Contoso hun app SmartHotel met Site Recovery alleen virtuele machines migreert.
[Artikel 6: Rehost virtuele machines in Azure en SQL Server-beschikbaarheidsgroepen](contoso-migration-rehost-vm-sql-ag.md) | Toont hoe de app SmartHotel in Contoso worden gemigreerd. Deze Site Recovery gebruiken voor het migreren van virtuele machines van de app en de migratie van de Database-service om te migreren van de app-database naar een SQL Server-beschikbaarheidsgroep. | Beschikbaar
Artikel 7: Rehost een app Linux virtuele Azure-machines (in dit artikel) | Toont hoe Contoso hun osService Linux-app met Azure Site Recovery wordt gemigreerd.
[Artikel 8: Rehost een Linux-app voor virtuele machines in Azure en Azure MySQL-Server](contoso-migration-rehost-linux-vm-mysql.md) | Laat zien hoe de Contoso migreert de osService Linux-app met behulp van Site Recovery voor VM-migratie en MySQL-Workbench om te migreren (naar een exemplaar van Azure MySQL-Server. | Beschikbaar

In dit artikel Contoso de twee lagen migreert **osTicket** app, die wordt uitgevoerd op Linux Apache MySQL PHP (licht) naar Azure. De app virtuele machines worden gemigreerd met de Azure Site Recovery-service. Als u deze app open source gebruiken wilt, kunt u het downloaden van [GitHub](https://github.com/osTicket/osTicket).

## <a name="business-drivers"></a>Zakelijke drijfveren

De IT-leiding heeft nauw samengewerkt met hun zakelijke partners om te begrijpen wat ze willen bereiken met deze migratie:

- **Zakelijke groei adres**: Contoso groeit en er is als gevolg hiervan druk op de on-premises systemen en de infrastructuur.
- **Beperk risico**: de app service-helpdesk is essentieel voor het bedrijf Contoso. Ze willen verplaatsen naar Azure met nul risico.
- **Uitbreiden**: ze niet wilt dat de app nu wijzigen. Ze willen gewoon om ervoor te zorgen dat het stabiel is.


## <a name="migration-goals"></a>Doelstellingen van de migratie

Het team van de cloud Contoso is vastgemaakt omlaag doelstellingen voor deze migratie kan om te bepalen van de beste migratiemethode:

- Na de migratie, moet de app in Azure dezelfde prestatiemogelijkheden hebben als in hun on-premises VMWare-omgeving.  De app blijft in de cloud als kritiek omdat deze lokale. 
- Contoso wil niet investeren in deze app.  Het is belangrijk voor het bedrijf zijn, maar in de huidige vorm ze gewoon wilt veilig te verplaatsen naar de cloud.
- Contoso wil niet het ops-model voor deze app wijzigen. Ze willen ermee in de cloud op dezelfde manier die ze nu doen.
- Contoso wilt niet wijzigen van app-functionaliteit. Alleen de locatie van de app wordt gewijzigd.
- Nadat een aantal migraties van Windows-app is voltooid, wil Contoso meer informatie over het gebruik van een infrastructuur op basis van Linux in Azure.

## <a name="proposed-architecture"></a>Voorgestelde architectuur

In dit scenario geldt het volgende:

- De app is gelaagd over twee VM's (OSTICKETWEB en OSTICKETMYSQL).
- De virtuele machines zich bevinden op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5).
- De VMware-omgeving wordt beheerd met vCenter Server 6.5 (**vcenter.contoso.com**), uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacenter (**contoso datacenter**), met een lokale domeincontroller (**contosodc1**).
- Omdat de app een productie-werkbelasting is, de virtuele machines in Azure bevinden zich in de resourcegroep productie **ContosoRG**.
- De virtuele machines worden gemigreerd naar de primaire regio (VS-Oost 2) en worden in het productienetwerk (VNET-PROD-EUS2):
    - Het web VM zich bevindt in het subnet frontend (PROD FE EUS2).
    - De VM-database wordt opgeslagen in de database-subnet (PROD-DB-EUS2).
- De lokale virtuele machines in het datacenter Contoso wordt buiten gebruik worden gesteld nadat de migratie is voltooid.

![Scenario-architectuur](./media/contoso-migration-rehost-linux-vm/architecture.png) 

## <a name="migration-process"></a>migratieproces

Contoso migreert:

1. Als een eerste stap Contoso stelt de Azure en on-premises infrastructuur die nodig zijn voor de Site Recovery implementeert.
2. Na het voorbereiden van Azure en on-premises onderdelen ze instellen en inschakelen van replicatie voor de virtuele machines.
3. Nadat de replicatie werkt, moet deze de virtuele machines door failover naar Azure migreren.

![migratieproces](./media/contoso-migration-rehost-linux-vm/migration-process.png)

### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | De service ingedeeld en beheert de migratie en herstel na noodgevallen voor Azure VM's en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, zijn Azure Storage kosten verbonden.  Virtuele machines in Azure worden gemaakt en kosten wanneer failover plaatsvindt. [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over kosten en prijzen.

 
## <a name="prerequisites"></a>Vereisten

Dit is wat u (en Contoso) voor dit scenario moet.

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | U moet al hebt gemaakt een abonnement tijdens vroege artikelen in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement en u niet de beheerder bent, moet u samen met de beheerder u eigenaar of bijdrager machtigingen toewijzen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-infrastructuur** | Instellen van Azure-infrastructuur, zoals beschreven in Contoso [Azure-infrastructuur voor de migratie](contoso-migration-infrastructure.md).<br/><br/> Meer informatie over specifieke [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) vereisten voor de Site Recovery.
**On-premises servers** | Uw lokale vCenter-server moet versie 5.5, 6.0 of 6.5 worden uitgevoerd<br/><br/> Een ESXi-host waarop versie 5.5, 6.0 of 6.5<br/><br/> Een of meer virtuele VMware-machines uitgevoerd op de ESXi-host.
**On-premises virtuele machines** | [Bekijk de Linux-machines](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) die worden ondersteund voor migratie met Site Recovery.<br/><br/> Controleer of ondersteund [Linux-systemen voor bestands- en opslagservices](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage).<br/><br/> Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Azure voltooit de migratie:

> [!div class="checklist"]
> * **Stap 1: Voorbereiden op Azure Site Recovery**: ze maken van een Azure storage-account gerepliceerde gegevens kan bevatten en een Recovery Services-kluis maken.
> * **Stap 2: Lokale VMware voorbereiden voor siteherstel**: ze worden gebruikt voor VM-agent voor het opsporen en installatie-accounts voorbereiden en verbinding maken met virtuele Azure-machines na een failover worden voorbereid.
> * **Stap 3: Replicatie VMs**: ze instellen van de bron en doel migratie-omgeving, maak een replicatiebeleid voor en virtuele machines repliceren naar Azure-opslag.
> * **Stap 4: Migreer de virtuele machines met Site Recovery**: ze een testfailover om te controleren of alles werkt uitvoeren en voer vervolgens een volledige failover naar de virtuele machines migreren naar Azure.


## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Stap 1: Voorbereiden op Azure Site Recovery-service

Contoso heeft een aantal Azure-onderdelen voor Site Recovery nodig:

- Failover van een VNet waarin resources zich bevinden (Contoso gebruikt het VNet ze al geïmplementeerd voor productie)
- Een nieuwe Azure storage-account voor gerepliceerde gegevens. 
- Een Recovery Services-kluis in Azure.

Contoso is al gemaakt het VNet tijdens [Azure-infrastructuur implementatie](contoso-migration-infrastructure.md), zodat ze hoeft aan te maken van een opslagaccount en de kluis.

1. Contoso maakt een Azure storage-account (contosovmsacc20180528) in de regio VS-Oost 2.

    - Het opslagaccount moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
    - Ze een algemeen-account gebruikt met de standard-opslag en LRS-replicatie.

    ![Site Recovery-opslag](./media/contoso-migration-rehost-linux-vm/asr-storage.png)

2. Met de netwerk- en storage-account in plaats Contoso een kluis (ContosoMigrationVault) maken en plaats deze in de **ContosoFailoverRG** resourcegroep in de primaire regio van de VS-Oost 2.

    ![Recovery Services-kluis](./media/contoso-migration-rehost-linux-vm/asr-vault.png)


**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Azure instellen voor de Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Stap 2: On-premises VMware voor herstel van sites voorbereiden

Contoso bereidt de on-premises VMware-infrastructuur als volgt:

- Maak een account op de vCenter-server of vSphere ESXi host, VM-detectie automatiseren.
- Maak een account waarmee de automatische installatie van de Mobility-service op de VMware-machines die u wilt repliceren.
- Voorbereiden op de lokale virtuele machines, zodat ze verbinding met virtuele Azure-machines maken kunnen wanneer ze zijn gemaakt na de migratie.


### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Virtuele machines automatisch detecteren. Er is minimaal een alleen-lezen-account vereist.
- Replicatie, failover en failback orkestreren. U moet een account die bewerkingen zoals het maken en schijven verwijderen en het inschakelen van virtuele machines kan worden uitgevoerd.

Contoso stelt u het account als volgt:

1. Contoso maakt een rol op de vCenter-niveau.
2. Contoso wijst die rol vervolgens de vereiste machtigingen.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op de virtuele Linux-machines die Contoso de migreren:

- Site Recovery kan een automatische push-installatie van dit onderdeel kunt doen wanneer u replicatie voor de virtuele machines inschakelen.
- Voor automatische push-installatie moet u een account die door Site Recovery wordt gebruikt voor toegang tot de virtuele machines voorbereiden.
- Details van de accounts worden ingevoerd tijdens de installatie van de replicatie. 
- Het account kan worden domein of lokale account met machtigingen om te installeren op virtuele machines.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Na een failover naar Azure wil Contoso verbinding kunnen maken met de gerepliceerde virtuele machines in Azure. Om dit te doen, is er een aantal dingen die om te kunnen doen:

- Als u wilt openen via het internet, kunnen SSH op de lokale Linux VM vóór de migratie.  Voor Ubuntu kan dit worden uitgevoerd met de volgende opdracht: **apt Sudo get-ssh in installeren -y**.
- Nadat ze (failover) van de migratie hebt uitgevoerd, moeten ze controleren **opstarten diagnostics** om een schermopname van de virtuele machine weer te geven.
- Als dit niet werkt, moeten ze controleren dat de virtuele machine wordt uitgevoerd en deze [tips voor probleemoplossing](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) maken en toewijzen van een functie voor automatische detectie.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) maken van een account voor push-installatie van de Mobility-service.


## <a name="step-3-replicate-the-on-premises-vms"></a>Stap 3: De on-premises virtuele machines repliceren

Voordat ze het web VM naar Azure migreren kunnen, stelt u Contoso- en replicatie inschakelen.

### <a name="set-a-protection-goal"></a>Een beveiligingsdoel instellen

1. In de kluis onder de kluisnaam van de (ContosoVMVault) ze ingesteld een replicatie-doel (**aan de slag** > **siteherstel** > **infrastructuur voorbereiden**.
2. Deze opgeven dat hun computers zich op locatie bevinden, dat ze zijn virtuele VMware-machines, en dat ze wilt repliceren naar Azure.
    ![Replicatie-doel](./media/contoso-migration-rehost-linux-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Controleer de implementatie plannen

Als u wilt doorgaan, zij bevestigen dat ze implementatie plannen door te selecteren hebt voltooid **Ja, ik dit hebt gedaan**. Contoso zijn slechts één VM in dit scenario wordt gemigreerd en hoeft niet-implementatie plannen.

### <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Contoso moet configureren hun bronomgeving. U doet dit door ze een OVF-sjabloon downloaden en gebruiken voor het implementeren van de Site Recovery-configuratieserver als een maximaal beschikbare, lokale VMware VM. Nadat de configuratieserver actief en werkend is, registreren ze in de kluis.

De configuratieserver wordt uitgevoerd voor een aantal onderdelen:

- Het configuratieserveronderdeel die coördineert de communicatie tussen de on-premises en Azure en beheert gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag.
- De processerver installeert ook Mobility Service op VM's die u wilt repliceren, en detecteert automatisch on-premises VMware-VM's.

Contoso uitvoeren als volgt te werk:

1. Downloaden van de sjabloon OVF **infrastructuur voorbereiden** > **bron** > **configuratieserver**.
    
    ![OVF downloaden](./media/contoso-migration-rehost-linux-vm/add-cs.png)

2. Ze importeren van de sjabloon in VMware voor de virtuele machine maken en implementeren van de virtuele machine.

    ![OVF-sjabloon](./media/contoso-migration-rehost-linux-vm/vcenter-wizard.png)

3. Wanneer ze de virtuele machine voor het eerst inschakelt, wordt deze opgestart naar een Windows Server 2016-installatie-ervaring. Ze de licentieovereenkomst accepteren en voer een administrator-wachtwoord.
4. Nadat de installatie is voltooid, ondertekenen ze in de virtuele machine als beheerder. Bij de eerste aanmelding, wordt standaard de Azure Site Recovery Configuration Tool uitgevoerd.
5. In het hulpmiddel Geef ze een naam voor de configuratieserver in de kluis te registreren.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, ze zich aanmelden bij de Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin u de configuratieserver wilt registreren.

    ![Configuratieserver registreren](./media/contoso-migration-rehost-linux-vm/config-server-register2.png)

7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
8. Ze opnieuw aan te melden bij de computer en de configuratiewizard voor het beheer van Server automatisch wordt gestart.
9. In de wizard selecteren ze de NIC van binnenkomend replicatieverkeer. Deze instelling kan niet worden gewijzigd nadat deze geconfigureerd.
10. Ze selecteert het abonnement, de resourcegroep en de kluis waarin de configuratieserver registreren.

    ![Kluis](./media/contoso-migration-rehost-linux-vm/cswiz1.png) 

11. Ze vervolgens download en installeer MySQL-Server en VMWare PowerCLI. 
12. Na de validatie Geef ze de FQDN-naam of IP-adres van de vCenter-server of vSphere-host. Ze laat de standaardpoort en geef een beschrijvende naam voor de vCenter-server.
13. Ze geven de account die ze voor automatische detectie hebt gemaakt en de referenties op die moeten worden gebruikt voor de Mobility-Service automatisch worden geïnstalleerd.

    ![vCenter](./media/contoso-migration-rehost-linux-vm/cswiz2.png)

14. Nadat de registratie is voltooid, wordt in de Azure portal Contoso controleert of de configuratieserver en de VMware-server aanwezig zijn op de **bron** pagina in de kluis. Detectie kan 15 minuten of langer duren. 
15. Site Recovery maakt verbinding met de VMware-servers, en virtuele machines worden gedetecteerd.

### <a name="set-up-the-target"></a>Het doel instellen

Nu de doel-replicatie-instellingen worden geconfigureerd in Contoso.

1. In **infrastructuur voorbereiden** > **doel**, ze de doelinstellingen selecteren.
2. Site Recovery controleert of er een Azure storage-account en het netwerk in het opgegeven doel is.

### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

Nadat de bron en doel zijn ingesteld, is Contoso gereed voor het maken van een replicatiebeleid voor.

1. In **infrastructuur voorbereiden** > **replicatie-instellingen** > **replicatiebeleid** >  **maken en Koppelen**, maak een beleid **ContosoMigrationPolicy**.
2. Ze de standaardinstellingen gebruiken:
    - **Drempelwaarde voor RPO**: standaard 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
    - **Herstel bewaarperiode**. De standaardwaarde van 24 uur. Deze waarde geeft aan hoe lang de bewaarperiode voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
    - **App-consistente momentopname frequentie**. De standaardwaarde van één uur. Deze waarde bepaalt de frequentie waarmee toepassingsconsistente momentopnamen worden gemaakt.
 
        ![Replicatiebeleid maken](./media/contoso-migration-rehost-linux-vm/replication-policy.png)

5. Het beleid wordt automatisch gekoppeld aan de configuratieserver. 

    ![Beleid voor wachtwoordreplicatie koppelen](./media/contoso-migration-rehost-linux-vm/replication-policy2.png)

**Meer hulp nodig?**

- U kunt een overzicht van deze stappen in lezen [herstel na noodgevallen instellen voor de lokale virtuele VMware-machines](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Gedetailleerde instructies zijn beschikbaar om u te helpen [de bronomgeving instellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementeren van de configuratieserver](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), en [replicatie-instellingen configureren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) over de Azure-gastagent voor Linux.

**Meer hulp nodig?**

- U kunt een overzicht van deze stappen in lezen [herstel na noodgevallen instellen voor de lokale virtuele VMware-machines](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Gedetailleerde instructies zijn beschikbaar om u te helpen [de bronomgeving instellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementeren van de configuratieserver](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), en [replicatie-instellingen configureren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) over de Azure-gastagent voor Linux.



### <a name="enable-replication-for-osticketweb"></a>Replicatie inschakelen voor OSTICKETWEB

Nu Contoso kunt beginnen met het repliceren van de **OSTICKETWEB** VM.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze selecteert u de instellingen van de bronserver.
2. Ze selecteren dat ze willen, kunnen virtuele machines, selecteert u de broninstellingen, inclusief de vCenter-server en de configuratieserver.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm/enable-replication-source.png)

3. Ze de doelinstellingen opgeven, met inbegrip van de resourcegroep en VNet waarin de virtuele machine in Azure geplaatst na een failover worden en het opslagaccount waarin de gerepliceerde gegevens worden opgeslagen.

     ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm/enable-replication2.png)

3. Contoso selecteert **OSTICKETWEB** voor replicatie. 

    - In deze fase Contoso only selecteert **OSTICKETWEB** omdat VNet en het subnet moeten worden geselecteerd en de virtuele machines worden niet in hetzelfde subnet.
    - Site Recovery installeert automatisch de Mobility-service wanneer replicatie is ingeschakeld voor de virtuele machine.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm/enable-replication3.png)

4. In de VM-eigenschappen selecteert Contoso het account dat wordt gebruikt door de processerver voor de Mobility-Service automatisch worden geïnstalleerd op de machine.

     ![Mobility-service](./media/contoso-migration-rehost-linux-vm/linux-mobility.png)

5. in **replicatie-instellingen** > **replicatie-instellingen configureren**, ze controleren dat de juiste replicatiegroep beleid toegepast en selecteer is **replicatie inschakelen**.
6.  Bijhouden van de Replicatievoortgang van de in **taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.



### <a name="enable-replication-for-osticketmysql"></a>Replicatie inschakelen voor OSTICKETMYSQL

Nu Contoso kunt beginnen met het repliceren van **OSTICKETMYSQL**.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze de instellingen voor de bron en doel selecteren.
2. Contoso selecteert **OSTICKETMYSQL** voor replicatie, en selecteert u het account moet worden gebruikt voor de installatie van de Mobility-service.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-linux-vm/mysql-enable.png)

3. Contoso geldt hetzelfde replicatiebeleid die is gebruikt voor OSTICKETWEB en replicatie inschakelen.  

**Meer hulp nodig?**

U kunt een overzicht van deze stappen in lezen [replicatie inschakelen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>Stap 4: De virtuele machines migreren 

Contoso uitvoeren van een snelle failover testen en vervolgens de virtuele machines te migreren.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Een testfailover uitgevoerd ervoor zorgt dat alles werkt zoals verwacht vóór de migratie. 

1. Contoso wordt een testfailover uitgevoerd naar het laatste punt in tijd (**meest recente verwerkte**).
2. Ze selecteren **machine afsluiten voordat u begint met failover**, zodat de Site Recovery probeert de bron-VM afsluiten voordat de failover. De failover wordt voortgezet zelfs als het afsluiten is mislukt. 
3. Test failover worden uitgevoerd: 
    - Controle wordt uitgevoerd om te controleren of alle van de voorwaarden die zijn vereist voor de migratie zijn in plaats van een vereisten.
    - De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als het laatste herstelpunt is geselecteerd, wordt een herstelpunt wordt gemaakt van de gegevens.
    - Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.
3. Nadat de failover is voltooid, wordt de replica virtuele machine in Azure weergegeven in de Azure portal. Ze Controleer of de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en dat deze wordt uitgevoerd. 
4. Nadat u hebt gecontroleerd, ze opschonen van de failover en vastleggen en opslaan van eventuele opmerkingen.

### <a name="create-and-customize-a-recovery-plan"></a>Maken en aanpassen van een herstelplan

 Nadat u hebt gecontroleerd of de testfailover heeft gewerkt zoals verwacht, Contoso maken van een herstelplan voor migratie. 

- Geeft de volgorde van een herstelplan met failover plaatsvindt, hoe Azure VM's worden gezet in Azure.
- Omdat ze migreren van een app met twee lagen wilt, moeten ze het herstelplan aanpassen zodat de gegevens van virtuele machine (SQLVM) wordt gestart voordat de frontend (WEBVM).


1. In **herstelplannen (Site Recovery)** > **+ herstelplan**, ze een plan maken en de virtuele machines aan toe te voegen.

    ![Plan voor herstel](./media/contoso-migration-rehost-linux-vm/recovery-plan.png)

2. Na het maken van het plan ze selecteren om aan te passen (**herstelplannen** > **OsTicketMigrationPlan** > **aanpassen**.
3.  Ze verwijderen **OSTICKETWEB** van **groep 1: Start**.  Dit zorgt ervoor dat de eerste startactie betrekking heeft op **OSTICKETMYSQL** alleen.

    ![Groep voor gegevensherstel](./media/contoso-migration-rehost-linux-vm/recovery-group1.png)

4.  In **+ groep** > **toevoegen beveiligde items**, het toevoegen van **OSTICKETWEB** naar **groep 2: Start**.  Contoso moet deze in twee verschillende groepen.

    ![Groep voor gegevensherstel](./media/contoso-migration-rehost-linux-vm/recovery-group2.png)


### <a name="migrate-the-vms"></a>De virtuele machines migreren


Contoso bent klaar om een failover uitgevoerd op het herstelplan, voor het migreren van de virtuele machines.

1. Ze selecteert het plan > **Failover**.
2.  Ze selecteren via failover naar de meest recente herstelpunt, en opgeven dat Site Recovery proberen moet om de lokale virtuele machine af voordat de failover te. Kan de voortgang van de failover volgen op de **taken** pagina.

    ![Failover](./media/contoso-migration-rehost-vm/failover1.png)

3. VCenter-Server geeft tijdens de failover opdrachten om te stoppen van de twee virtuele machines die wordt uitgevoerd op de ESXi-host.

    ![Failover](./media/contoso-migration-rehost-linux-vm/vcenter-failover.png)

3. Na de failover Contoso controleren dat de virtuele machine in Azure wordt weergegeven zoals verwacht in de Azure portal.

    ![Failover](./media/contoso-migration-rehost-linux-vm/failover2.png)  

3. De migratie voor het voltooien van het migratieproces voor elke VM voltooid na controle van de virtuele machine in Azure. Dit wordt gestopt replicatie voor de virtuele machine en Hiermee stopt u Site Recovery facturering voor de virtuele machine.

    ![Failover](./media/contoso-migration-rehost-linux-vm/failover3.png)


### <a name="connect-the-vm-to-the-database"></a>De virtuele machine verbinden met de database

Als de laatste stap in het migratieproces updates Contoso de verbindingsreeks van de toepassing om te verwijzen naar de app-database op de **OSTICKETMYSQL** VM. 

1. Indienen van een SSH-verbinding met de **OSTICKETWEB** VM die gebruikmaakt van Putty of een andere SSH-client. De virtuele machine is een privéverbinding zodat ze verbinding maken met behulp van de privé IP-adres.

    ![Verbinding maken met database](./media/contoso-migration-rehost-linux-vm/db-connect.png)  

    ![Verbinding maken met database](./media/contoso-migration-rehost-linux-vm/db-connect2.png)  

2. Ze nodig hebben om ervoor te zorgen dat de **OSTICKETWEB** VM kan communiceren met de **OSTICKETMYSQL** VM. De configuratie is momenteel vastgelegd met het lokale IP-adres 172.16.0.43.

    **Vóór de update**
    
    ![IP bijwerken](./media/contoso-migration-rehost-linux-vm/update-ip1.png)  

    **Na de update**
    
    ![IP bijwerken](./media/contoso-migration-rehost-linux-vm/update-ip2.png) 
    
3. Ze start de service met **systemctl opnieuw apache2**.

    ![Opnieuw starten](./media/contoso-migration-rehost-linux-vm/restart.png) 

4. Ten slotte bijwerken van de DNS-records voor **OSTICKETWEB** en **OSTICKETMYSQL**, op een van de Contoso-domeincontrollers.

    ![DNS bijwerken](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

    ![DNS bijwerken](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) een testfailover uitgevoerd. 
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) een herstelplan maken.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) failover wordt uitgevoerd naar Azure.

## <a name="clean-up-after-migration"></a>Na de migratie opschonen

Met de migratie is voltooid, worden de lagen van de app osTicket nu uitgevoerd op Azure Virtual machines.

Nu moet Contoso sommige opruiming uitvoeren:  

- Ze verwijderen de lokale virtuele machines uit de vCenter-inventaris.
- Ze verwijderen de lokale virtuele machines uit lokale back-uptaken.
- Bijwerken van hun interne documentatie om weer te geven van de nieuwe locatie en IP-adressen voor OSTICKETWEB en OSTICKETMYSQL.
- Deze bronnen die met de virtuele machines communiceren controleren en eventuele relevante instellingen of documentatie in overeenstemming met de nieuwe configuratie bijwerken.
- Contoso gebruikt de service Azure migreren met afhankelijkheid toewijzing voor het evalueren van de VM's voor migratie. Ze moeten Microsoft Monitoring Agent en de afhankelijkheid Agent ze geïnstalleerd voor dit doel van de virtuele machine verwijderen.

## <a name="review-the-deployment"></a>Controleer de implementatie

Met de app nu uitgevoerd, moet Contoso volledig operationeel maken en hun nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

De Contoso-beveiligingsteam Lees de OSTICKETWEB en OSTICKETMYSQLVMs om te bepalen van beveiligingsproblemen.

- Ze controleren de Netwerkbeveiligingsgroepen (nsg's) voor de virtuele machines om toegang te beheren. Nsg's worden gebruikt om ervoor te zorgen dat alleen verkeer toegestaan voor de toepassing kan worden doorgegeven.
- Ze ook overwegen de beveiliging van de gegevens op de VM-schijven met versleuteling van schijf en Azure KeyVault.

[Lees meer](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) over procedures voor beveiliging voor virtuele machines.

### <a name="backups"></a>Back-ups

Contoso back-up van de gegevens op de virtuele machines met behulp van de Azure Backup-service. [Meer informatie](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Licenties en kosten-optimalisatie

- Na implementatie van resources, Contoso Azure labels wijst zoals gedefinieerd tijdens het [Azure-infrastructuur implementatie](contoso-migration-infrastructure.md#set-up-tagging).
- Contoso heeft geen licentie problemen met hun Ubuntu-servers.
- Contoso kunnen Azure kosten Management door Cloudyn, een Microsoft-vestiging in licentie gegeven. Het is een oplossing voor het beheer van meerdere cloud kosten die u helpt te kunnen gebruiken en beheren van Azure en andere cloudbronnen.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over het beheer van Azure kosten. 


## <a name="next-steps"></a>Volgende stappen

In dit artikel die we hebt u geleerd hoe Contoso gemigreerd gelaagde een lokale service helpdesk-app op twee virtuele machines van Linux virtuele Azure IaaS-machines, met Azure Site Recovery.

In het volgende artikel in de reeks leert u hoe Contoso dezelfde service helpdesk app migreren naar Azure. Deze tijd Contoso maakt gebruik van Site Recovery voor het migreren van de frontend-VM voor de app en ze migreren van de app-database met behulp van back-up en herstel met Azure-Database voor MySQL, met het hulpprogramma MySQL-workbench. [Aan de slag](contoso-migration-rehost-linux-vm-mysql.md).
