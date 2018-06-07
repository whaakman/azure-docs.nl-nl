---
title: Een Contoso-app met de migratie naar Azure VM's met Azure Site Recovery rehost | Microsoft Docs
description: Meer informatie over hoe een lokale app rehost en met een lift en shift migratie naar Azure voor de migratie van lokale computers met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/30/2018
ms.author: raynew
ms.openlocfilehash: 11b5e2a408d3ba514753f3510b36fce02470c6e9
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825162"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms"></a>Migratie van Contoso: Rehost van een lokale app virtuele Azure-machines


In dit artikel laat zien hoe Contoso hun SmartHotel-app in Azure, rehost door de app virtuele machines te migreren virtuele Azure-machines.


Dit document is een in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso hun lokale bronnen in de Microsoft Azure-cloud migreert. De reeks bevat achtergrondinformatie en scenario's die aangeven instellen van een migratie-infrastructuur, beoordeling van de lokale bronnen voor migratie en het uitvoeren van verschillende soorten migraties. Scenario's toenemen in complexiteit en gaan we meer artikelen toevoegen gedurende een bepaalde periode.


**Artikel** | **Details** | **Status**
--- | --- | ---
Artikel 1: overzicht | Biedt een overzicht van de strategie voor de migratie van Contoso, de serie artikelen en de voorbeeld-apps we gebruiken. | Beschikbaar
Artikel 2: Een Azure-infrastructuur implementeren | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. De infrastructuur wordt gebruikt voor alle Contoso migratiescenario's. | Beschikbaar
Artikel 3: Beoordelen lokale bronnen  | Toont hoe een beoordeling van de lokale twee lagen SmartHotel app uitgevoerd in VMware in Contoso wordt uitgevoerd. Evalueren van de virtuele machines van een app met de [Azure migreren](migrate-overview.md) -service en de app SQL Server-database met de [Azure Database migratie-assistent](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
Artikel 4: Verander (lift-en-shift) naar Azure VM's en een exemplaar van SQL beheerd (in dit artikel) | Demonstreert hoe Contoso de app SmartHotel migreert naar Azure. Migreren van de app frontend virtuele machine met behulp [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database met de [Azure databasemigratie](https://docs.microsoft.com/azure/dms/dms-overview) service om te migreren naar een SQL-exemplaar worden beheerd. | Beschikbaar
Artikel 5: Verander (lift-en-shift) virtuele Azure-machines (in dit artikel) | Toont hoe Contoso hun app SmartHotel met Site Recovery alleen virtuele machines migreert.
Artikel 6: Verander (lift-en-shift) naar Azure VM's en SQL Server-beschikbaarheidsgroepen | Toont hoe de app SmartHotel in Contoso worden gemigreerd. Deze Site Recovery gebruiken voor het migreren van virtuele machines van de app en de migratie van de Database-service om te migreren van de app-database naar een SQL Server-beschikbaarheidsgroep. | Beschikbaar
Artikel 7: Verander (lift-en-shift) naar Azure VM's en Azure MySQL-Server | Demonstreert hoe Contoso de SmartHotel app virtuele machines migreren met behulp van Site Recovery en MySQL Workbench (back-up en herstel) migreert naar een Azure MySQL Server-exemplaar. | Beschikbaar

In dit artikel worden de twee lagen Windows worden gemigreerd met Contoso. NET SmartHotel app uitgevoerd op de VMware-machines naar Azure. Als u wilt deze app wilt gebruiken, als open-source opgegeven en u kunt downloaden via [github](https://github.com/Microsoft/SmartHotel360).



## <a name="business-drivers"></a>Zakelijke drijfveren

De IT-leiding heeft nauw samengewerkt met hun zakelijke partners om te begrijpen wat ze willen bereiken met deze migratie:

- **Zakelijke groei adres**: Contoso groeit en er is als gevolg hiervan druk op de on-premises systemen en infrastructuur.
- **Beperk risico**: de SmartHotel app is essentieel voor het bedrijf Contoso. Ze willen verplaatsen naar Azure met nul risico.
- **Uitbreiden**: Contoso niet wilt wijzigen van de app. Ze willen gewoon om ervoor te zorgen dat het stabiel is.


## <a name="migration-goals"></a>Doelstellingen van de migratie

Het team van de cloud Contoso is vastgemaakt omlaag doelstellingen voor deze migratie. Deze doelstellingen worden gebruikt om te bepalen van de beste migratiemethode:

- Na de migratie, moet de app in Azure dezelfde prestatiemogelijkheden hebben als in VMware.  De app blijft in de cloud als kritiek omdat deze lokale. 
- Contoso wil niet investeren in deze app.  Het is belangrijk voor het bedrijf zijn, maar in de huidige vorm ze gewoon wilt veilig te verplaatsen naar de cloud.
- Contoso wil niet het ops-model voor deze app wijzigen. Ze willen ermee in de cloud op dezelfde manier die ze nu doen.
- Contoso wil niet alle app-functionaliteit wijzigen. Alleen de locatie van de app wordt gewijzigd.

## <a name="proposed-architecture"></a>Voorgestelde architectuur

Hier wordt de huidige omgeving

- De app is gelaagd over twee VM's (**WEBVM** en **SQLVM**).
- De virtuele machines zich bevinden op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5).
- De VMware-omgeving wordt beheerd met vCenter Server 6.5 (**vcenter.contoso.com**), uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een lokale domeincontroller (**contosodc1**).
- De lokale virtuele machines in het datacenter Contoso wordt buiten gebruik worden gesteld nadat de migratie is voltooid.

![Scenario-architectuur](./media/contoso-migration-rehost-vm/architecture.png) 

## <a name="migration-process"></a>migratieproces

Contoso wordt de frontend app en database virtuele machines naar Azure VM's met Site Recovery gemigreerd:

- Als een eerste stap hebt ze voorbereiden en Azure onderdelen instellen voor herstel van sites en de on-premises VMware-infrastructuur voorbereiden.
- Ze al hun [Azure-infrastructuur](contoso-migration-infrastructure.md) aanwezig is, zodat ze hoeft toevoegen van een aantal Azure onderdelen specifiek voor de Site Recovery.
- Ze kunnen starten voor het repliceren van de virtuele machines met alles voorbereid.
-Nadat replicatie is ingeschakeld en werkt, ze de virtuele machine door failover naar Azure migreren.

![migratieproces](./media/contoso-migration-rehost-vm/migraton-process.png) 



### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | De service ingedeeld en beheert de migratie en herstel na noodgevallen voor Azure VM's en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, zijn Azure Storage kosten verbonden.  Virtuele machines in Azure worden gemaakt en kosten wanneer failover plaatsvindt. [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over kosten en prijzen.


## <a name="prerequisites"></a>Vereisten

Dit is wat u (en Contoso) wilt uitvoeren van dit scenario.

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | U moet al hebt gemaakt een abonnement tijdens vroege artikelen in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement en u niet de beheerder bent, moet u samen met de beheerder u eigenaar of bijdrager machtigingen toewijzen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-infrastructuur** | [Meer informatie over hoe](contoso-migration-infrastructure.md) Contoso instellen van een Azure-infrastructuur.<br/><br/> Meer informatie over specifieke [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) vereisten voor de Site Recovery.
**On-premises servers** | Lokale vCenter Server 5.5, 6.0-of 6.5 moeten worden uitgevoerd<br/><br/> ESXi-hosts versie 5.5, 6.0 of 6.5 moeten worden uitgevoerd<br/><br/> Een of meer virtuele VMware-machines moet worden uitgevoerd op de ESXi-host.
**On-premises virtuele machines** | Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Contoso de migratie wordt uitgevoerd:

> [!div class="checklist"]
> * **Stap 1: Voorbereiden op Azure Site Recovery**: maken van een Azure storage-account voor het opslaan van gerepliceerde gegevens en een Recovery Services-kluis.
> * **Stap 2: Lokale VMware voorbereiden voor siteherstel**: ze accounts voorbereiden voor VM-detectie en agent-installatie en verbinding maken met virtuele Azure-machines na een failover worden voorbereid.
> * **Stap 3: Replicatie VMs**: ze de replicatie instellen en virtuele machines repliceren naar Azure-opslag.
> * **Stap 4: Migreer de virtuele machines met Site Recovery**: ze een testfailover om te controleren of alles werkt uitvoeren en voer vervolgens een volledige failover naar de virtuele machines migreren naar Azure.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Stap 1: Voorbereiden op Azure Site Recovery-service

Hier volgen de Azure-onderdelen die Contoso moet de virtuele machines migreren naar Azure:

- Een VNet waarin virtuele Azure-machines geplaatst worden moeten nadat ze zijn gemaakt tijdens de failover.
- Een Azure storage-account voor gerepliceerde gegevens. 
- Een Recovery Services-kluis in Azure.

Ze stelt deze als volgt:

1. Contoso is al een netwerk kunnen worden gebruikt om de Site Recovery ingesteld wanneer ze [geïmplementeerd de Azure-infrastructuur](contoso-migration-infrastructure.md)

    - De SmartHotel een productie-app is en de virtuele machines worden gemigreerd naar de Azure productienetwerk (VNET-PROD-EUS2) in de primaire regio van de VS-Oost 2.
    - Beide virtuele machines worden geplaatst in de resourcegroep ContosoRG, die wordt gebruikt voor productieresources.
    - De app-frontend VM (WEBVM) worden gemigreerd naar de frontend-subnet (PROD-FE-EUS2), in het productienetwerk.
    - De app-database VM (SQLVM) worden gemigreerd naar de database-subnet (PROD-DB-EUS2), in het productienetwerk.

2. Contoso maakt een Azure storage-account (contosovmsacc20180528) in de primaire regio.
    - Het opslagaccount moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
    - Ze een algemene account gebruiken met de standard-opslag en LRS-replicatie. 

    ![Site Recovery-opslag](./media/contoso-migration-rehost-vm/asr-storage.png)

3. Met de netwerk- en storage-account in plaats Contoso nu een Recovery Services-kluis (ContosoMigrationVault) maakt en plaatst het in de resourcegroep ContosoFailoverRG in de primaire regio van de VS-Oost 2.

    ![Recovery Services-kluis](./media/contoso-migration-rehost-vm/asr-vault.png)

**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Azure instellen voor de Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Stap 2: On-premises VMware voor herstel van sites voorbereiden

Dit is wat Contoso lokale wordt voorbereid:

- Een account op de vCenter-server of vSphere ESXi-host, VM detectie automatiseren.
- Een account waarmee automatische installatie van de Mobility-service op de virtuele VMware-machines. 
- Lokale instellingen van de virtuele machine, zodat Contoso verbinding met de gerepliceerde virtuele Azure-machines na een failover maken kan.


### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Virtuele machines automatisch detecteren. 
- U kunt replicatie, failover en failback indeelt voor virtuele machines.
- Er is minimaal een alleen-lezen-account vereist. Het account moet kunnen uitvoeren van bewerkingen zoals het maken en schijven verwijderen en het inschakelen van virtuele machines.

Contoso stelt u het account als volgt:

1. Ze maken een rol op de vCenter-niveau.
2. Ze toewijzen aan die rol op de vereiste machtigingen.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op elke virtuele machine.

- Site Recovery kan een automatische push-installatie van de Mobility-service kunt doen wanneer replicatie is ingeschakeld.
- Er is een account vereist, zodat herstel van de Site toegang heeft tot de virtuele machines voor de pushinstallatie. U geeft dit account bij het instellen van replicatie.
- Het account kan worden domein of lokale met machtigingen om te installeren op de virtuele machines.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Na een failover wil Contoso verbinding maken met de Azure VM's. U doet dit door ze als volgt vóór de migratie:

1. Voor toegang via het internet ze:

 - Schakelt u RDP op de lokale virtuele machine voordat failover wordt uitgevoerd
 - Zorg ervoor dat TCP en UDP-regels zijn toegevoegd voor de **openbare** profiel.
 - Controleer of RDP is toegestaan in **Windows Firewall** > **Apps toegestaan** voor alle profielen.
 
2. Voor toegang via site-naar-site VPN, ze:

 - Schakelt u RDP in op de on-premises machine.
 - Toestaan dat RDP in de **Windows Firewall** -> **toegestane apps en functies**, voor **domein- en persoonlijke** netwerken.
 - SAN-beleid van het besturingssysteem op de lokale virtuele machine ingesteld **OnlineAll**.

Wanneer ze een failover uitvoert moeten deze bovendien controleert u het volgende:

- Er moet geen Windows-updates in behandeling op de virtuele machine bij activering van een failover. Als er, is ze niet kunnen aanmelden op de virtuele machine totdat de update is voltooid.
- Na een failover, ze kunnen de status **opstarten diagnostics** om een schermopname van de virtuele machine weer te geven. Als dit niet werkt, moeten ze controleren dat de virtuele machine wordt uitgevoerd en deze [tips voor probleemoplossing](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) maken en toewijzen van een functie voor automatische detectie.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) maken van een account voor push-installatie van de Mobility-service.


## <a name="step-3-replicate-the-on-premises-vms"></a>Stap 3: De on-premises virtuele machines repliceren

Voordat ze een migratie naar Azure uitvoert kunnen, moet Contoso instellen en inschakelen van replicatie.

### <a name="set-a-replication-goal"></a>Doelstelling replicatie instellen

1. In de kluis onder de kluisnaam van de (ContosoVMVault) ze selecteren een replicatie-doel (**aan de slag** > **siteherstel** > **infrastructuur voorbereiden** .
2. Deze opgeven dat hun computers zich bevinden op locatie, uitgevoerd op de VMware en repliceren naar Azure.

    ![Replicatiedoel](./media/contoso-migration-rehost-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Controleer de implementatie plannen

Als u wilt doorgaan, ze bevestigt u dat ze hebben voltooid implementatie plannen door te selecteren **Ja, ik dit hebt gedaan**. In dit scenario Contoso slechts twee virtuele machines migreert en hoeft niet-implementatie plannen.


### <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Contoso moet configureren hun bronomgeving. U doet dit door ze een OVF-sjabloon downloaden en gebruiken voor het implementeren van de Site Recovery-configuratieserver als een maximaal beschikbare, lokale VMware VM. Nadat de configuratieserver actief en werkend is, registreren ze in deze kluis.

De configuratieserver wordt uitgevoerd voor een aantal onderdelen:

- Het configuratieserveronderdeel die coördineert de communicatie tussen de on-premises en Azure en beheert gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag.
- De processerver installeert ook Mobility Service op VM's die u wilt repliceren, en detecteert automatisch on-premises VMware-VM's.



Contoso uitvoeren als volgt te werk:

1. In de kluis downloaden ze de OVF-sjabloon van **infrastructuur voorbereiden** > **bron** > **configuratieserver**.
    
    ![OVF downloaden](./media/contoso-migration-rehost-vm/add-cs.png)

2. Ze importeren de sjabloon in VMware maken en implementeren van de virtuele machine.

    ![OVF-sjabloon](./media/contoso-migration-rehost-vm/vcenter-wizard.png)

3.  Wanneer ze de virtuele machine voor het eerst inschakelt, wordt deze opgestart naar een Windows Server 2016-installatie-ervaring. Ze de licentieovereenkomst accepteren en voer een administrator-wachtwoord.
4. Nadat de installatie is voltooid, ze zich aanmelden bij de virtuele machine als de beheerder. Bij de eerste aanmelding, wordt standaard de Azure Site Recovery Configuration Tool uitgevoerd.
5. In het hulpmiddel Geef ze een naam voor de configuratieserver in de kluis registreren.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, ze zich aanmelden bij de Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin zij de configuratieserver moeten registreren.

    [Configuratieserver registreren](./media/contoso-migration-rehost-vm/config-server-register2.png)

7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
8. Ze opnieuw aan te melden bij de computer en de configuratiewizard voor het beheer van Server automatisch wordt gestart.
9. In de wizard selecteren ze de NIC van binnenkomend replicatieverkeer. Deze instelling kan niet worden gewijzigd nadat deze geconfigureerd.
10. Ze selecteert het abonnement, resourcegroep en de kluis waarin de configuratieserver registreren.
        ![Kluis](./media/contoso-migration-rehost-vm/cswiz1.png) 

10. Ze downloaden en installeren van MySQL-Server en VMWare PowerCLI. 
11. Na de validatie Geef ze de FQDN-naam of IP-adres van de vCenter-server of vSphere-host. Ze laat de standaardpoort en geef een beschrijvende naam voor de server in Azure.
12. Deze opgeven dat het account dat ze voor automatische detectie hebt gemaakt en de referenties die worden gebruikt voor de Mobility-Service automatisch worden geïnstalleerd. Voor Windows-machines moet het account dat lokale administrator-bevoegdheden op de virtuele machines.

    ![vCenter](./media/contoso-migration-rehost-vm/cswiz2.png)

7. Nadat de registratie is voltooid, wordt in de Azure portal Contoso dubbele controleert of de configuratieserver en de VMware-server aanwezig zijn op de **bron** pagina in de kluis. Detectie kan 15 minuten of langer duren. 
8. Site Recovery maakt verbinding met de VMware-servers met de opgegeven instellingen, en virtuele machines worden gedetecteerd.

### <a name="set-up-the-target"></a>Het doel instellen

Contoso bevat nu de replicatie-instellingen van het doel.

1. In **infrastructuur voorbereiden** > **doel**, ze de doelinstellingen selecteren.
2. Site Recovery controleert of er een Azure storage-account en het netwerk in de opgegeven doellocatie is.

### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

Contoso kunt nu een beleid voor wachtwoordreplicatie maken.

1. In **infrastructuur voorbereiden** > **replicatie-instellingen** > **replicatiebeleid** >  **maken en Koppelen**, maak een beleid **ContosoMigrationPolicy**.
2. Ze de standaardinstellingen gebruiken:
    - **Drempelwaarde voor RPO**: standaard 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
    - **Herstel bewaarperiode**. De standaardwaarde van 24 uur. Deze waarde geeft aan hoe lang de bewaarperiode voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
    - **App-consistente momentopname frequentie**. De standaardwaarde van één uur. Deze waarde bepaalt de frequentie waarmee toepassingsconsistente momentopnamen worden gemaakt.

        ![Replicatiebeleid maken](./media/contoso-migration-rehost-vm/replication-policy.png)

5. Het beleid wordt automatisch gekoppeld aan de configuratieserver. 

    ![Beleid voor wachtwoordreplicatie koppelen](./media/contoso-migration-rehost-vm/replication-policy2.png)

### <a name="enable-replication-for-webvm"></a>Replicatie inschakelen voor WEBVM

Met alles in plaats kunt Contoso replicatie inschakelen voor de virtuele machines. Ze start met WebVM.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze selecteert u de instellingen van de bronserver.
2. Ze geven aan dat ze wilt inschakelen, VM's, selecteert u de vCenter-server en de configuratieserver.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm/enable-replication1.png)

3. Ze selecteren de doelinstellingen, inclusief de resourcegroep en Azure-netwerk en het opslagaccount.

     ![Replicatie inschakelen](./media/contoso-migration-rehost-vm/enable-replication2.png)

4. Contoso selecteert **WebVM** voor replicatie, controleert u het replicatiebeleid en replicatie inschakelen.

    - In deze fase selecteert Contoso alleen WEBVM omdat VNet en het subnet moeten worden geselecteerd en de Contoso de app virtuele machines in verschillende subnetten wordt geplaatst.
    - Site Recovery installeert automatisch de Mobility-service op de virtuele machine wanneer replicatie is ingeschakeld.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm/enable-replication3.png)

5. Bijhouden van de Replicatievoortgang van de in **taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.
6. In **Essentials** in de Azure-portal Contoso ziet de structuur voor de virtuele machines repliceren naar Azure.


### <a name="enable-replication-for-sqlvm"></a>Replicatie inschakelen voor SQLVM

Contoso kunt nu beginnen met het repliceren van de machine SQLVM, met behulp van hetzelfde proces als hierboven.

1. Ze selecteren broninstellingen.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm/enable-replication1.png)

2. Ze vervolgens opgeven de doelinstellingen.

     ![Replicatie inschakelen](./media/contoso-migration-rehost-vm/enable-replication2-sqlvm.png)

3. Ze selecteren SQLVM voor replicatie. 

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm/enable-replication3-sqlvm.png)

4. Ze hetzelfde replicatiebeleid dat is gebruikt voor WEBVM toepassen en replicatie inschakelen.

    ![Weergave van de infrastructuur](./media/contoso-migration-rehost-vm/essentials.png)

**Meer hulp nodig?**

- U kunt een overzicht van deze stappen in lezen [herstel na noodgevallen instellen voor de lokale virtuele VMware-machines](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Gedetailleerde instructies zijn beschikbaar om u te helpen [de bronomgeving instellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementeren van de configuratieserver](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), en [replicatie-instellingen configureren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- U kunt meer lezen over [inschakelen van replicatie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>Stap 4: De virtuele machines migreren 

Contoso een snelle testfailover en een volledige failover uitvoeren voor het migreren van de virtuele machines.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Een testfailover kunt u ervoor zorgen dat alles werkt zoals verwacht. 

1. Contoso wordt een testfailover uitgevoerd naar het laatste punt in tijd (**meest recente verwerkte**).
2. Ze selecteren **machine afsluiten voordat u begint met failover**, zodat de Site Recovery probeert de bron-VM afsluiten voordat de failover. De failover wordt voortgezet zelfs als het afsluiten is mislukt. 
3. Test failover worden uitgevoerd: 

    - Controle wordt uitgevoerd om te controleren of alle van de voorwaarden die zijn vereist voor de migratie zijn in plaats van een vereisten.
    - De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als u het meest recente herstelpunt selecteert, wordt een herstelpunt van de gegevens gemaakt.
    - Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.
    
3. Nadat de failover is voltooid, wordt de replica virtuele machine in Azure weergegeven in de Azure portal. Contoso controleert of de virtuele machine de juiste grootte, verbonden met het juiste netwerk en wordt uitgevoerd. 
4. Na controle van de testfailover ze opschonen van de failover en vastleggen en opslaan van eventuele opmerkingen. 

### <a name="create-and-customize-a-recovery-plan"></a>Maken en aanpassen van een herstelplan

 Contoso maakt nadat u hebt gecontroleerd of de testfailover heeft gewerkt zoals verwacht, kan een herstelplan voor migratie. 

- De volgorde van een herstelplan aangeeft in welke failover plaatsvindt en geeft aan hoe Azure Virtual machines wordt online worden gebracht in Azure.
- Omdat de app twee lagen, aanpassen zij het herstelplan zodat de gegevens VM (SQLVM) wordt gestart voordat de frontend (WEBVM).

1. In **herstelplannen (Site Recovery)** > **+ herstelplan**, ze een plan maken en de virtuele machines aan toe te voegen.

    ![Plan voor herstel](./media/contoso-migration-rehost-vm/recovery-plan.png)

2. Nadat het plan is gemaakt, zij het aanpassen (**herstelplannen** > **SmartHotelMigrationPlan** > **aanpassen**).
2.  Ze WEBVM van verwijderen **groep 1: Start**.  Dit zorgt ervoor dat de eerste startactie SQLVM alleen beïnvloedt.
3.  In **+ groep** > **toevoegen beveiligde items**, ze WEBVM toevoegen aan groep 2: Start.  De virtuele machines moeten in twee verschillende groepen.


### <a name="migrate-the-vms"></a>De virtuele machines migreren


Contoso kunt nu een volledige failover voor het voltooien van de migratie uitvoeren.

1. Ze selecteert het herstelplan > **Failover**.
2. Ze failover naar de meest recente herstelpunt selecteren en deze Site Recovery moet proberen de lokale virtuele machine afsluiten voordat de failover. Kan de voortgang van de failover volgen op de **taken** pagina.

    ![Failover](./media/contoso-migration-rehost-vm/failover1.png)


3. Na de failover controleren ze dat de virtuele machine in Azure wordt weergegeven zoals verwacht in de Azure portal.

    ![Failover](./media/contoso-migration-rehost-vm/failover2.png)  

3. Ze voltooien na de controle van de migratie voor elke virtuele machine. Dit Hiermee stopt u replicatie voor de virtuele machine en Hiermee stopt u Site Recovery facturering.

    ![Failover](./media/contoso-migration-rehost-vm/failover3.png)

**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) een testfailover uitgevoerd. 
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) een herstelplan maken.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) failover wordt uitgevoerd naar Azure.

## <a name="clean-up-after-migration"></a>Na de migratie opschonen

Met de migratie is voltooid, worden de lagen van de app SmartHotel nu uitgevoerd op Azure Virtual machines.

Nu moet Contoso om deze stappen opschonen te voltooien:  

- Verwijder de WEBVM-machine uit de vCenter-inventaris.
- Verwijder de SQLVM-machine uit de vCenter-inventaris.
- WEBVM en SQLVM verwijderen uit lokale back-uptaken.
- Interne documentatie om weer te geven van de nieuwe locatie en het IP-adressen voor de virtuele machines bijwerken.
- Controleer alle bronnen die met de virtuele machines communiceren en eventuele relevante instellingen of documentatie in overeenstemming met de nieuwe configuratie bijwerken.

## <a name="review-the-deployment"></a>Controleer de implementatie

Met de app nu uitgevoerd, moet de Contoso nu volledig operationeel maken en deze te beveiligen in Azure.

### <a name="security"></a>Beveiliging

De Contoso-beveiligingsteam controleert de Azure-virtuele machines, om te bepalen van beveiligingsproblemen.

- Om de toegang beperken, bekijk ze de Netwerkbeveiligingsgroepen (nsg's) voor de virtuele machines. Nsg's worden gebruikt om ervoor te zorgen dat alleen verkeer toegestaan bij de app het kan bereiken.
- Ook overwegen de beveiliging van de gegevens op de schijf met behulp van Azure Disk Encryption en KeyVault.

[Lees meer](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) over procedures voor beveiliging voor virtuele machines.

### <a name="backups"></a>Back-ups

Contoso gaat naar de back-up van de gegevens op de virtuele machines met behulp van de Azure Backup-service. [Meer informatie](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Licenties en kosten-optimalisatie

1. Contoso heeft een bestaande licentie voor hun virtuele machines en maken gebruik van het voordeel van de hybride Azure.  Deze wordt de bestaande Azure VM's, om te profiteren van deze prijzen worden geconverteerd.
2. Contoso kunnen Azure kosten Management door Cloudyn, een Microsoft-vestiging in licentie gegeven. Het is een oplossing voor het beheer van meerdere cloud kosten die helpt om te kunnen gebruiken en beheren van Azure en andere cloudbronnen. [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over het beheer van Azure kosten. 

## <a name="conclusion"></a>Conclusie

In dit artikel rehosted Contoso SmartHotel-app in Azure door de app virtuele machines migreren naar Azure VM's met behulp van de Site Recovery-service. 


## <a name="next-steps"></a>Volgende stappen

In het volgende artikel in de reeks we tonen hoe Contoso rehost de SmartHotel app frontend virtuele machine in Azure met behulp van de Site Recovery-service en de database migreren naar een Azure SQL AlwaysOn-beschikbaarheidsgroep.

