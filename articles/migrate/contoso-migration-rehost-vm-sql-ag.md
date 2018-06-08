---
title: Een app Contoso rehost door te migreren naar Azure VM's en SQL Server AlwaysOn-beschikbaarheidsgroep | Microsoft Docs
description: Meer informatie over hoe Contoso een lokale app rehost door te migreren naar Azure VM's en SQL Server AlwaysOn-beschikbaarheidsgroep
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: raynew
ms.openlocfilehash: 97c8430ab5d4e08e52790b898051d5985c3df03c
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839880"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms-and-sql-server-alwayson-availability-group"></a>Migratie van Contoso: Rehost van een lokale app aan de Azure VM's en SQL Server AlwaysOn-beschikbaarheidsgroep

In dit artikel laat zien hoe Contoso rehost hun SmartHotel-app in Azure. Ze migreren de app-frontend VM naar een virtuele machine van Azure en de app-database naar een Azure SQL Server VM, in een Windows Server-failovercluster met SQL Server AlwaysOn-beschikbaarheidsgroepen wordt uitgevoerd.

Dit document is een in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso hun lokale bronnen in de Microsoft Azure-cloud migreert. De reeks bevat achtergrondinformatie en scenario's die aangeven instellen van een migratie-infrastructuur, beoordeling van de lokale bronnen voor migratie en het uitvoeren van verschillende soorten migraties. Scenario's toenemen in complexiteit en gaan we meer artikelen toevoegen gedurende een bepaalde periode.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de serie artikelen en de voorbeeld-apps we gebruiken. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. De infrastructuur wordt gebruikt voor alle artikelen voor migratie. | Beschikbaar
[Artikel 3: Beoordelen lokale bronnen](contoso-migration-assessment.md)  | Toont hoe een evaluatie van een lokale twee lagen SmartHotel app uitgevoerd in VMware in Contoso wordt uitgevoerd. Contoso evalueert virtuele machines van app met de [Azure migreren](migrate-overview.md) -service en de app SQL Server-database met de [Database migratie-assistent](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
[Artikel 4: Een app voor virtuele machines in Azure en een SQL-exemplaar voor beheerde Rehost](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstreert hoe Contoso wordt uitgevoerd een lift en shift migratie naar Azure voor de app SmartHotel. Contoso migreert u de app frontend virtuele machine met behulp [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database naar een exemplaar SQL beheerd met behulp van de [Azure migratie databaseservice](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar
[Artikel 5: Een app naar virtuele machines in Azure Rehost](contoso-migration-rehost-vm.md) | Toont hoe Contoso de SmartHotel-app met Site Recovery alleen virtuele machines migreert.
Artikel 6: Rehost een app in Azure VM's en SQL Server AlwaysOn-beschikbaarheidsgroep (in dit artikel) | Toont hoe de app SmartHotel in Contoso worden gemigreerd. Contoso maakt gebruik van Site Recovery voor het migreren van virtuele machines van de app en de migratie van de Database-service voor het migreren van de database van de app naar een SQL Server-cluster beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar
Artikel 7: Een Linux-app voor virtuele machines in Azure en Azure MySQL Server Rehost | Laat zien hoe de osTicket Linux-app in Contoso worden gemigreerd. Ze migreren van de frontend-VM met Site Recovery en migreren (back-up en terugzetten van) de database naar een exemplaar van Azure MySQL-Server, MySQL Workbench gebruiken | Gepland
Artikel 8: Rehost een Linux-app voor virtuele machines in Azure | Toont hoe Contoso heeft een migratie lift en shift van VERIGE osTicket virtuele machines van app naar Azure met Site Recovery | Gepland


In dit artikel migreren Contoso de twee lagen Windows. NET SmartHotel app uitgevoerd op virtuele VMware-machines naar Azure. Als u wilt deze app wilt gebruiken, als open-source opgegeven en u kunt downloaden via [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>Zakelijke drijfveren

De IT-leiding heeft nauw samengewerkt met hun zakelijke partners om te begrijpen wat ze willen bereiken met deze migratie:

- **Zakelijke groei adres**: Contoso groeit en er is als gevolg hiervan druk op de on-premises systemen en infrastructuur.
- **De efficiëntie verhogen**: Contoso moet verwijderen van onnodige procedures en processen voor ontwikkelaars en gebruikers te stroomlijnen.  De bedrijfsbehoeften IT snel en niet afval tijd of geld, dus sneller leveren op klantvereisten.
- **Flexibiliteit te vergroten**: Contoso IT moet reageert op de behoeften van uw bedrijf. Moet mogelijk sneller dan de wijzigingen in de marketplace, zodat het slagen van een globale economie reageren.  Het mag niet ophalen in de manier waarop of een zakelijke upblokkering geworden.
- **Schaal**: als het bedrijf is groeit, Contoso IT-systemen die kunnen worden vergroot hetzelfde tempo moet opgeven.

## <a name="migration-goals"></a>Doelstellingen van de migratie

Het team van de cloud Contoso is vastgemaakt omlaag doelstellingen voor deze migratie. Deze doelstellingen werden gebruikt om te bepalen van de beste migratiemethode:

- Na de migratie, moet de app in Azure dezelfde prestatiemogelijkheden hebben als in VMWare.  De app blijft in de cloud als kritiek omdat deze lokale.
- Contoso wil niet investeren in deze app.  Het is belangrijk voor het bedrijf zijn, maar in de huidige vorm ze gewoon wilt veilig te verplaatsen naar de cloud.
- De lokale database voor de app heeft problemen met de beschikbaarheid. Ze graag dat deze geïmplementeerd in Azure als een cluster met hoge beschikbaarheid, met mogelijkheden voor failover.
- Contoso wil upgrade uitvoeren van hun huidige platform van de SQL Server 2008 R2 naar SQL Server 2017.
- Contoso wil niet opnieuw gebruiken van een Azure SQL Database voor deze app en en alternatieven zoekt.

## <a name="proposed-architecture"></a>Voorgestelde architectuur

In dit scenario geldt het volgende:

- De app is gelaagd over twee VM's (WEBVM en SQLVM).
- De virtuele machines zich bevinden op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5)
- De VMware-omgeving wordt beheerd met vCenter Server 6.5 (**vcenter.contoso.com**), uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een lokale domeincontroller (**contosodc1**).
- De lokale virtuele machines in het datacenter Contoso wordt buiten gebruik worden gesteld nadat de migratie is voltooid.

![Scenario-architectuur](media/contoso-migration-rehost-vm-sql-ag/architecture.png) 

### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Database-Management-Service](https://docs.microsoft.com/azure/dms/dms-overview) | Contoso wordt de app gegevenslaag DMS met migreren. DMS verbinding maken met de on-premises SQLVM machine via een site-naar-site-VPN en DMS schakelt naadloze migraties uit meerdere databasebronnen migreren naar Azure data-platforms, met minimale downtime. | Meer informatie over [ondersteunde regio's](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) voor DMS en get [prijsinformatie](https://azure.microsoft.com/pricing/details/database-migration/).
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Contoso gebruikt voor een migratie lift en shift van de app-frontend VM Site Recovery. Site Recovery ingedeeld en beheert de migratie en herstel na noodgevallen voor Azure VM's en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, zijn Azure Storage kosten verbonden.  Virtuele machines in Azure worden gemaakt en kosten wanneer failover plaatsvindt. [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over kosten en prijzen.

 

## <a name="migration-process"></a>migratieproces

- Contoso wordt de app virtuele machines migreren naar Azure.
- Deze moeten de frontend-VM migreren naar Azure virtuele machine met Site Recovery:
    - Als een eerste stap hebt ze voorbereiden en Azure onderdelen instellen en de on-premises VMware-infrastructuur voorbereiden.
    - Ze kunnen met alles voorbereid starten voor het repliceren van de virtuele machine.
    - Nadat replicatie is ingeschakeld en werkt, ze de virtuele machine door failover naar Azure migreren.
- Deze moeten de database migreren naar een SQL Server-cluster in Azure, met behulp van de gegevens migratie Service (DMS).
    - Als eerste stap hebt ze voor het inrichten van SQL Server-machines in Azure, instellen van het cluster en een interne load balancer en AlwaysOn-beschikbaarheidsgroepen configureren.
    - Met deze in plaats kunnen ze de database migreren
- Na de migratie hebt ze Schakel AlwaysOn-beveiliging voor de database.

![migratieproces](media/contoso-migration-rehost-vm-sql-ag/migration-process.png) 
 
## <a name="prerequisites"></a>Vereisten

Dit is wat u (en Contoso) wilt uitvoeren van dit scenario:

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | U moet al hebt gemaakt een abonnement als u de evaluatie uitgevoerd in de eerste artikel in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement en u niet de beheerder bent, moet u samen met de beheerder u eigenaar of bijdrager machtigingen toewijzen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-infrastructuur** | [Meer informatie over hoe](contoso-migration-infrastructure.md) Contoso instellen van een Azure-infrastructuur.<br/><br/> Meer informatie over specifieke [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) vereisten voor de Site Recovery.
**Site recovery (op locatie)** | Uw lokale vCenter-server moet versie 5.5, 6.0 of 6.5 worden uitgevoerd<br/><br/> Een ESXi-host waarop versie 5.5, 6.0 of 6.5<br/><br/> Een of meer virtuele VMware-machines uitgevoerd op de ESXi-host.<br/><br/> Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Ondersteund [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) configuratie.<br/><br/> Virtuele machines die u wilt repliceren, moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).
**DMS** | DMS moet u een [compatibel on-premises VPN-apparaat](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> U moet mogelijk de on-premises VPN-apparaat configureren. Er moet een extern gericht openbaar IPv4-adres en het adres kan zich niet achter een NAT-apparaat.<br/><br/> Zorg ervoor dat u toegang hebt tot uw lokale SQL Server-database.<br/><br/> De Windows Firewall moet toegang kunnen krijgen tot de bron-database-engine. [Meer informatie](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Als er een firewall voor uw database-machine, kunt u regels voor toegang tot de database en bestanden via het SMB-poort 445 toevoegen.<br/><br/> De referenties waarmee verbinding maken met de bron van SQL Server en de doelinstantie worden beheerd, moeten lid zijn van de serverrol sysadmin.<br/><br/> U moet een netwerk delen in uw lokale database die DMS gebruiken kunt voor back-up van de brondatabase.<br/><br/> Zorg ervoor dat het serviceaccount met een SQL Server-exemplaar van bron schrijfrechten op de netwerkshare heeft.<br/><br/> Maak een notitie van een Windows-gebruiker (en wachtwoord) die de machtiging Volledig beheer op de netwerkshare heeft. De migratie van Azure databaseservice imiteert deze gebruikersreferenties voor het uploaden van back-upbestanden naar de Azure storage-container.<br/><br/> Het installatieproces van SQL Server Express het TCP/IP-protocol wordt ingesteld op **uitgeschakelde** standaard. Zorg ervoor dat deze ingeschakeld.


## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Contoso de migratie wordt uitgevoerd:

> [!div class="checklist"]
> * **Stap 1: De SQL Server-VM's maken in Azure**: voor hoge beschikbaarheid, Contoso wilt implementeren van een geclusterde database in Azure. Deze implementeren twee SQL Server-VM's en een Azure interne load balancer.
> * **Stap 2: Implementeer het cluster**: na implementatie van de VM's van SQL Server, voorbereiden van een Azure SQL Server-cluster.  Deze moeten de database in deze vooraf gemaakte cluster migreren.
> * **Stap 3: Bereid DMS**: als u de migratie van de Database-provider voorbereiden DMS ze registreren, een DMS-exemplaar en een project maken. Ze ingesteld een shared access signature (SAS) Uniform Resource Identifier (URI). DMS gebruikt de SA-URI voor toegang tot de storage-account-container waarnaar de service de SQL Server-back-up van bestanden geüpload.
> * **Stap 4: Voorbereiden op Azure Site Recovery**: maken van een Azure storage-account voor het opslaan van gerepliceerde gegevens en een Recovery Services-kluis.
> * **Stap 5: Lokale VMware voorbereiden voor siteherstel**: ze accounts voorbereiden voor VM-detectie en agent-installatie en lokale virtuele machines voorbereiden, zodat ze verbinding virtuele Azure-machines na een failover maken kunnen.
> * **Stap 6: Replicatie VMs**: ze replicatie-instellingen configureren en inschakelen van replicatie.
> * **Stap 7: Migreren van de database met DMS**: migreren van de database.
> * **Stap 8: Migreer de virtuele machines met Site Recovery**: ze eerst een testfailover om te controleren of alles werkt, gevolgd door een volledige failover voor het migreren van de virtuele machine worden uitgevoerd.


## <a name="step-1-prepare-a-sql-server-alwayson-availability-group-cluster"></a>Stap 1: Een SQL Server AlwaysOn availability group cluster voorbereiden

Contoso wil vooruit te plannen door het implementeren van de database in een cluster in Azure. Dit cluster kan vervolgens worden gebruikt voor andere databases. 

- De SQL Server-VM's worden in de resourcegroep voor ContosoRG (gebruikt voor productiebronnen) geïmplementeerd.
- Afgezien van unieke namen voor beide VM dezelfde instellingen gebruikt.
- De interne load balancer worden geïmplementeerd in de ContosoNetworkingRG (gebruikt voor netwerkresources).
- Virtuele machines wordt uitgevoerd van Windows Server 2016 met SQL Server 2017 Enterprise Edition. Contoso geen licenties voor dit besturingssysteem, dus gebruik van een installatiekopie in Azure Marketplace met de licentie als kosten met zich mee hun Azure EA vastlegging.

Hier ziet u hoe Contoso instellen van het cluster:

1. Deze maken twee SQL Server-VM's door SQL Server 2017 Enterprise Windows Server 2016 installatiekopie te selecteren in Azure Marketplace. 

    ![SQL VM SKU](media/contoso-migration-rehost-vm-sql-ag/sql-vm-sku.png)

2. In de **maken van virtuele machine Wizard** > **basisbeginselen**, ze configureren:

    - Namen voor de virtuele machines: **SQLAOG1** en **SQLAOG2**.
    - Aangezien machines bedrijfskritische, inschakelen ze SSD voor het type van de schijf VM.
    - Deze opgeven machine referenties.
    - Implementatie van de virtuele machines in de primaire VS-Oost 2 regio in de resourcegroep ContosoRG.

3. In **grootte**, ze beginnen met D2s_V3 SKU voor beide virtuele machines. Ze je later naar te behoefte schalen.
4. In **instellingen**, zij het volgende doen:

    - Omdat deze virtuele machines kritieke databases voor de app zijn, worden ze beheerde schijven gebruikt.
    - Ze plaats de machines in het productienetwerk van de VS-Oost 2 primaire regio (**VNET-PROD-EUS2**), in het subnet van de database (**PROD-DB-EUS2**).
    - Maken van een nieuwe beschikbaarheidsset: **SQLAOGAVSET**met de twee domeinen met fouten en vijf update-domeinen.

    ![VIRTUELE SQL-MACHINE](media/contoso-migration-rehost-vm-sql-ag/sql-vm-settings.png)

4. In **SQL Server-instellingen**, ze SQL-connectiviteit met het virtuele netwerk (particuliere), de limiet voor de standaardpoort 1433. Voor verificatie ze dezelfde referenties gebruiken omdat ze gebruikmaken van op locatie (**contosoadmin**).

    ![VIRTUELE SQL-MACHINE](media/contoso-migration-rehost-vm-sql-ag/sql-vm-db.png)

**Meer hulp nodig?**

- [Help opvragen](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision#1-configure-basic-settings) inrichten van een virtuele machine van SQL Server.
- [Meer informatie over](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-prereq#create-sql-server-vms) VM's configureren voor verschillende SQL Server-SKU's.

## <a name="step-2-deploy-the-failover-cluster"></a>Stap 2: Het failover-cluster implementeren

Hier ziet u hoe Contoso instellen van het cluster:

1. Ze Azure storage-account hebt ingesteld om te fungeren als de witness cloud.
2. Ze toevoegen de VM's van SQL Server aan het Active Directory-domein in de Contoso-on-premises datacentrum.
3. Ze maken het cluster in Azure.
4. Ze configureren de cloud-witness.
5. Ten slotte kunnen SQL AlwaysOn-beschikbaarheidsgroepen.

### <a name="set-up-a-storage-account-as-cloud-witness"></a>Een opslagaccount als witness voor cloud instellen

Als u een cloud-witness instelt, moet Contoso een Azure Storage-account dat de blob-bestand dat is gebruikt voor het cluster arbitrage kan bevatten. Hetzelfde opslagaccount kan worden gebruikt voor het instellen van cloud-witness voor meerdere clusters. 

Contoso maakt een opslagaccount als volgt:

1. Ze een herkenbare naam voor het account opgeven (**contosocloudwitness**).
2. Ze implementeren een algemene universeel rekening met LRS.
3. Ze plaatsen het account in een derde regio - Zuid-centraal VS. Ze plaatst u het buiten de primaire en secundaire regio zodat deze beschikbaar is in geval van regionale storing blijft.
4. Ze plaats deze in de resourcegroep van de infrastructuurresources - **ContosoInfraRG**.

    ![Cloud-witness](media/contoso-migration-rehost-vm-sql-ag/witness-storage.png)

5. Wanneer ze maken de storage-account, primaire en secundaire sleutels worden gegenereerd voor deze. Ze moeten de primaire toegangssleutel voor het maken van de cloud-witness. De sleutel wordt weergegeven onder de naam van het opslagaccount > **toegangstoetsen**.

    ![Toegangssleutel](media/contoso-migration-rehost-vm-sql-ag/access-key.png)

### <a name="add-sql-server-vms-to-contoso-domain"></a>SQL Server-VM's toevoegen aan Contoso-domein

1. Contoso voegt SQLAOG1 en SQLAOG2 aan domein contoso.com.
2. Op elke virtuele machine installeer ze vervolgens de functie Windows Failover Clustering en hulpprogramma's.

## <a name="set-up-the-cluster"></a>De cluster instellen

Contoso maakt voordat u het cluster instelt, een momentopname van de schijf met het besturingssysteem op elke machine.

![Momentopname](media/contoso-migration-rehost-vm-sql-ag/snapshot.png)

2. Voer vervolgens zij een script dat zij hun hebt samen voor het maken van het Windows-failovercluster.

    ![Cluster maken](media/contoso-migration-rehost-vm-sql-ag/create-cluster1.png)

3. Nadat ze het cluster hebt gemaakt, controleren ze dat de virtuele machines worden weergegeven als clusterknooppunten.

     ![Cluster maken](media/contoso-migration-rehost-vm-sql-ag/create-cluster2.png)

## <a name="configure-the-cloud-witness"></a>De cloud-witness configureren

1. Contoso configureren in de cloud-witness met behulp van de **Quorum-configuratiewizard** in Failoverclusterbeheer.
2. Ze selecteren in de wizard een witness cloud maken met de storage-account.
3. Nadat de cloud-witness is geconfigureerd, weergegeven in in de module Failoverclusterbeheer.

    ![Cloud-witness](media/contoso-migration-rehost-vm-sql-ag/cloud-witness.png)
            

## <a name="enable-sql-server-always-on-availability-groups"></a>SQL Server AlwaysOn-beschikbaarheidsgroepen inschakelen

Contoso kunt nu inschakelen altijd op:

1. In SQL Server Configuration Manager ze inschakelen **AlwaysOn Availability Groups** voor de **SQL Server (MSSQLSERVER)** service.

    ![Schakel AlwaysOn](media/contoso-migration-rehost-vm-sql-ag/enable-alwayson.png)

2. Ze start de service om wijzigingen te laten treden.

Met AlwaysOn inschakelen, kunt Contoso de AlwaysOn-beschikbaarheidsgroep die de database SmartHotel beveiligt instellen.

**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness) witness en het instellen van een opslagaccount voor deze cloud.
- [Instructies](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial) voor het instellen van een cluster en het maken van een beschikbaarheidsgroep.

## <a name="step-3-deploy-the-azure-load-balancer"></a>Stap 3: De Azure Load Balancer implementeren

Contoso nu wilt implementeren van een interne load balancer die zich voor de clusterknooppunten bevindt. De load balancer wordt geluisterd naar verkeer en stuurt deze naar het juiste knooppunt.

![Taakverdeling](media/contoso-migration-rehost-vm-sql-ag/architecture-lb.png)

Ze maken als volgt de load balancer:

1. In Azure portal > **Networking** > **Load Balancer**, ze instellen van een nieuwe interne load balancer: **ILB-PROD-DB-EUS2-SQLAOG**.
2. Ze de load balancer plaats in het productienetwerk **VNET-PROD-EUS2**, in het subnet van de database **PROD-DB-EUS2**.
3. Wijs ze een statisch IP-adres: 10.245.40.100.
4. Als een VPN-element ze de load balancer in de resourcegroep netwerken implementeren **ContosoNetworkingRG**.

    ![Taakverdeling](media/contoso-migration-rehost-vm-sql-ag/lb-create.png)

Na de implementatie van de interne load balancer moet Contoso instellen. Ze een back-end-adresgroep maken, instellen van een health test en een regel voor de taakverdeling configureren.

### <a name="add-a-backend-pool"></a>Toevoegen van een back-endpool

Voor het distribueren van het verkeer naar de virtuele machines in het cluster instellen Contoso van een back-end-adresgroep die het IP-adressen van de NIC's voor virtuele machines die netwerkverkeer van de load balancer ontvangen bevat.

1. In de load balancer-instellingen in de portal, Contoso een back-endpool toevoegen: **ILB-PROD-DB-EUS-SQLAOG-BEPOOL**.
2. Ze koppelen de pool aan de beschikbaarheidsset SQLAOGAVSET. De virtuele machines in de set (**SQLAOG1** en **SQLAOG2**) worden toegevoegd aan de groep.

    ![Back-endpool](media/contoso-migration-rehost-vm-sql-ag/backend-pool.png)

### <a name="create-a-health-probe"></a>Een statustest maken

Contoso maakt een health test, zodat de load balancer kunt de status van de app controleren. De test wordt dynamisch toegevoegd of verwijderd van virtuele machines van de load balancer draaiing, op basis van het reageren op statuscontroles.

Ze maakt u de test als volgt: 

1. In de load balancer-instellingen in de portal, Contoso een health test maakt: **SQLAlwaysOnEndPointProbe**.
2. Ze de test voor het bewaken van virtuele machines op TCP-poort 59999 ingesteld.
3. Ze ingesteld met een interval van vijf seconden tussen tests en een drempelwaarde van 2. Als twee tests mislukken, wordt de virtuele machine niet in orde worden overwogen.

    ![Test](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

### <a name="configure-the-load-balancer-to-receive-traffic"></a>De load balancer voor het ontvangen verkeer configureren


Nu Contoso moet die een load balancer-regel defins hoe verkeer wordt gedistribueerd naar de virtuele machines.

- Binnenkomende verkeer wordt verwerkt door het front-end-IP-adres.
- De backend-IP-adresgroep ontvangt het verkeer.

Ze maken als volgt de regel:

1. In de load balancer-instellingen in de portal, ze een nieuw taakverdelingsregels toevoegen: **SQLAlwaysOnEndPointListener**.
2. Contoso stelt een front-end-listener ontvangen van binnenkomende SQL-client-verkeer op TCP 1433.
3. Deze opgeven met de back-endpool welk verkeer wordt doorgestuurd, en de poort waarop virtuele machines voor verkeer luistert.
4. Contoso kunt zwevend IP (direct server return). Dit is altijd vereist voor de SQL AlwaysOn.

    ![Test](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

**Meer hulp nodig?**

- [Een overzicht](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) Azure Load Balancer.
- [Meer informatie over](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-basic-internal-portal) maken van een load balancer.



## <a name="step-4-prepare-azure-for-the-site-recovery-service"></a>Stap 4: Voorbereiden op Azure Site Recovery-service

Hier volgen de Azure-onderdelen die Contoso moet Site Recovery implementeert:

- Een VNet waarin virtuele machines geplaatst worden moeten nadat ze tijdens de failover maakt.
- Een Azure storage-account voor gerepliceerde gegevens. 
- Een Recovery Services-kluis in Azure.

Ze stelt deze als volgt:

1.  Contoso is al een netwerk-en subnet kunnen worden gebruikt om de Site Recovery gemaakt wanneer ze [geïmplementeerd de Azure-infrastructuur](contoso-migration-rehost-vm-sql-ag.md).

    - De SmartHotel een productie-app is en WEBVM worden gemigreerd naar de Azure productienetwerk (VNET-PROD-EUS2) in de primaire Oost-US2 regio.
    - WEBVM worden geplaatst in de resourcegroep ContosoRG, die wordt gebruikt voor productiebronnen, en in het subnet productie (PROD FE EUS2).

2. Contoso maakt een Azure-opslag-acount (contosovmsacc20180528) in de primaire regio.

    - Ze een algemene account gebruiken met de standard-opslag en LRS-replicatie.
    - De account moet zich in dezelfde regio bevinden als de kluis.

    ![Site Recovery-opslag](media/contoso-migration-rehost-vm-sql-ag/asr-storage.png)

3. Met de netwerk- en storage-account in plaats ze nu een Recovery Services-kluis maken (**ContosoMigrationVault**), en plaats deze in de **ContosoFailoverRG** resourcegroep in de primaire regio van de VS-Oost 2 .

    ![Recovery Services-kluis](media/contoso-migration-rehost-vm-sql-ag/asr-vault.png)

**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Azure instellen voor de Site Recovery.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>Stap 4: On-premises VMware voor herstel van sites voorbereiden

Dit is wat Contoso lokale wordt voorbereid:

- Een account op de vCenter-server of vSphere ESXi-host, VM detectie automatiseren.
- Een account waarmee automatische installatie van de Mobility-service op de VMware-machines die u wilt repliceren.
- Lokale instellingen van de virtuele machine, zodat Contoso verbinding met de gerepliceerde Azure VM na een failover maken kan.


### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Virtuele machines automatisch detecteren. 
- Replicatie, failover en failback orkestreren.
- Er is minimaal een alleen-lezen-account vereist. U moet een account die bewerkingen zoals het maken en schijven verwijderen en het inschakelen van virtuele machines kan worden uitgevoerd.

Contoso stelt u het account als volgt:

1. Contoso maakt een rol op de vCenter-niveau.
2. Contoso wijst die rol vervolgens de vereiste machtigingen.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op elke virtuele machine.

- Site Recovery kan een automatische push-installatie van dit onderdeel kunt doen wanneer replicatie is ingeschakeld voor de virtuele machine.
- U moet een account met Site Recovery gebruiken kunt voor toegang tot de virtuele machine voor de pushinstallatie. U geeft dit account bij het instellen van de replicatie in de Azure-console.
- Het account kan worden domein of lokale met machtigingen om te installeren op de virtuele machine.




### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Contoso wil verbinding maken met virtuele Azure-machines na een failover. U doet dit door ze als volgt vóór de migratie:

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


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>Stap 5: De on-premises virtuele machines repliceren naar Azure met Site Recovery

Voordat ze kunnen een migratie naar Azure uitvoert, Contoso moet instellen en inschakelen van replicatie.

### <a name="set-a-replication-goal"></a>Doelstelling replicatie instellen

1. In de kluis onder de kluisnaam van de (ContosoVMVault) ze selecteren een replicatie-doel (**aan de slag** > **siteherstel** > **infrastructuur voorbereiden** .
2. Deze opgeven dat hun computers zich bevinden op locatie, uitgevoerd op de VMware en repliceren naar Azure.

    ![Replicatiedoel](./media/contoso-migration-rehost-vm-sql-ag/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Controleer de implementatie plannen

Als u wilt doorgaan, ze nodig hebben om te bevestigen dat ze hebben voltooid implementatie plannen door te selecteren **Ja, ik dit hebt gedaan**. In dit scenario Contoso zijn alleen een virtuele machine wordt gemigreerd en hoeft niet-implementatie plannen.

### <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Contoso moet configureren hun bronomgeving. U doet dit door ze een OVF-sjabloon downloaden en gebruiken voor het implementeren van de Site Recovery-configuratieserver als een maximaal beschikbare, lokale VMware VM. Nadat de configuratieserver actief en werkend is, registreren ze in deze kluis.

De configuratieserver wordt uitgevoerd voor een aantal onderdelen:

- Het configuratieserveronderdeel die coördineert de communicatie tussen de on-premises en Azure en beheert gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag.
- De processerver installeert ook Mobility Service op VM's die u wilt repliceren, en detecteert automatisch on-premises VMware-VM's.

Contoso uitvoeren als volgt te werk:


1. In de kluis downloaden ze de OVF-sjabloon van **infrastructuur voorbereiden** > **bron** > **configuratieserver**.
    
    ![OVF downloaden](./media/contoso-migration-rehost-vm-sql-ag/add-cs.png)

2. Ze importeren de sjabloon in VMware maken en implementeren van de virtuele machine.

    ![OVF-sjabloon](./media/contoso-migration-rehost-vm-sql-ag/vcenter-wizard.png)

3. Wanneer ze de virtuele machine voor het eerst inschakelt, wordt deze opgestart naar een Windows Server 2016-installatie-ervaring. Ze de licentieovereenkomst accepteren en voer een administrator-wachtwoord.
4. Nadat de installatie is voltooid, ze zich aanmelden bij de virtuele machine als de beheerder. Bij de eerste aanmelding, wordt standaard de Azure Site Recovery Configuration Tool uitgevoerd.
5. In het hulpmiddel Geef ze een naam voor de configuratieserver in de kluis te registreren.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, ze zich aanmelden bij de Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin u de configuratieserver wilt registreren.

    ![Configuratieserver registreren](./media/contoso-migration-rehost-vm-sql-ag/config-server-register2.png)

7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
8. Ze opnieuw aan te melden bij de computer en de configuratiewizard voor het beheer van Server automatisch wordt gestart.
9. In de wizard selecteren ze de NIC van binnenkomend replicatieverkeer. Deze instelling kan niet worden gewijzigd nadat deze geconfigureerd.
10. Ze selecteert het abonnement, de resourcegroep en de kluis waarin de configuratieserver registreren.
        ![Kluis](./media/contoso-migration-rehost-vm-sql-ag/cswiz1.png) 

10. Ze vervolgens download en installeer MySQL-Server en VMWare PowerCLI. 
11. Na de validatie Geef ze de FQDN-naam of IP-adres van de vCenter-server of vSphere-host. Ze laat de standaardpoort en geef een beschrijvende naam voor de vCenter-server.
12. Deze opgeven dat het account dat ze voor automatische detectie hebt gemaakt en de referenties die worden gebruikt voor de Mobility-Service automatisch te installeren. Voor Windows-machines moet het account dat lokale administrator-bevoegdheden op de virtuele machines.

    ![vCenter](./media/contoso-migration-rehost-vm-sql-ag/cswiz2.png)

7. Nadat de registratie is voltooid, wordt in de Azure portal Contoso dubbele controleert of de configuratieserver en de VMware-server aanwezig zijn op de **bron** pagina in de kluis. Detectie kan 15 minuten of langer duren. 
8. Site Recovery maakt verbinding met de VMware-servers met de opgegeven instellingen, en virtuele machines worden gedetecteerd.

### <a name="set-up-the-target"></a>Het doel instellen

Contoso bevat nu doel replicatie-instellingen.

1. In **infrastructuur voorbereiden** > **doel**, ze de doelinstellingen selecteren.
2. Site Recovery controleert of er een Azure storage-account en het netwerk in het opgegeven doel is.

### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

Nee, kan Contoso een replicatiebeleid maken.

1. In **infrastructuur voorbereiden** > **replicatie-instellingen** > **replicatiebeleid** >  **maken en Koppelen**, maak een beleid **ContosoMigrationPolicy**.
2. Ze de standaardinstellingen gebruiken:
    - **Drempelwaarde voor RPO**: standaard 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
    - **Herstel bewaarperiode**. De standaardwaarde van 24 uur. Deze waarde geeft aan hoe lang de bewaarperiode voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
    - **App-consistente momentopname frequentie**. De standaardwaarde van één uur. Deze waarde bepaalt de frequentie waarmee toepassingsconsistente momentopnamen worden gemaakt.
 
        ![Replicatiebeleid maken](./media/contoso-migration-rehost-vm-sql-ag/replication-policy.png)

5. Het beleid wordt automatisch gekoppeld aan de configuratieserver. 

    ![Beleid voor wachtwoordreplicatie koppelen](./media/contoso-migration-rehost-vm-sql-ag/replication-policy2.png)



### <a name="enable-replication"></a>Replicatie inschakelen

Contoso kan nu starten WebVM repliceren.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze selecteert u de instellingen van de bronserver.
2. Ze geven aan dat ze wilt inschakelen, VM's, selecteert u de vCenter-server en de configuratieserver.

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm-sql-ag/enable-replication1.png)

3. Nu ze de doelinstellingen opgeven, met inbegrip van de resourcegroep en VNet en het opslagaccount waarin de gerepliceerde gegevens worden opgeslagen.

     ![Replicatie inschakelen](./media/contoso-migration-rehost-vm-sql-ag/enable-replication2.png)

3. Contoso de WebVM voor replicatie selecteert, controleert het replicatiebeleid en replicatie inschakelen. Site Recovery installeert de Mobility-Service op de virtuele machine wanneer replicatie is ingeschakeld.
 
    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm-sql-ag/enable-replication3.png)

4. Bijhouden van de Replicatievoortgang van de in **taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.
5. In **Essentials** in de Azure-portal Contoso ziet de structuur voor de virtuele machines repliceren naar Azure.

    ![Weergave van de infrastructuur](./media/contoso-migration-rehost-vm-sql-ag/essentials.png)


**Meer hulp nodig?**

- U kunt een overzicht van deze stappen in lezen [herstel na noodgevallen instellen voor de lokale virtuele VMware-machines](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Gedetailleerde instructies zijn beschikbaar om u te helpen [de bronomgeving instellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementeren van de configuratieserver](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), en [replicatie-instellingen configureren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- U kunt meer lezen over [inschakelen van replicatie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-5-install-the-database-migration-assistant-dma"></a>Stap 5: De Database migratie assistent (DMA) installeren

Contoso de SmartHotel-database wilt migreren naar Azure virtuele machine **SQLAOG1** met behulp van de DMA. Ze instellen DMA als volgt:

1. Downloaden van het hulpprogramma van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) aan de lokale SQL Server-VM (**SQLVM**).
2. Ze setup (DownloadMigrationAssistant.msi) op de virtuele machine worden uitgevoerd.
3. Op de **voltooien** pagina ze selecteren **Start Microsoft Data migratie-assistent** voordat u de wizard voltooit.

## <a name="step-6-migrate-the-database-with-dma"></a>Stap 6: De database met DMA migreren

1. In de DMA ze een nieuwe migratie - uitgevoerd **SmartHotel**.
2. Ze selecteert de **server doeltype** als **SQL Server op Azure Virtual Machines**. 

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-1.png)

3. In de migratie, die ze toevoegen **SQLVM** als de bronserver en **SQLAOG1** als doel. Deze opgeven referenties voor elke machine.

     ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-2.png)

4. Het maken van een lokale share voor de database en configuratie-informatie. Het moet toegankelijk zijn met schrijftoegang voor het SQL-serviceaccount op SQLVM en SQLAOG1.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-3.png)

5. Contoso selecteert de aanmeldingen die moeten worden gemigreerd en de migratie start. Nadat deze is voltooid, ziet u DMA de migratie als geslaagd.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-4.png)

6. Ze controleren of de database wordt uitgevoerd op **SQLAOG1**.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-5.png)

DMS verbindt met het lokale SQL Server VM via een site-naar-site VPN-verbinding tussen het datacenter van de Contoso- en Azure en vervolgens de database wordt gemigreerd.

## <a name="step-7-protect-the-database"></a>Stap 7: De database beveiligen

Met de app-database op **SQLAOG1**, Contoso kan nu beveiligen met behulp van AlwaysOn-beschikbaarheidsgroepen. Deze Alwayson met SQL Management Studio configureren en vervolgens een listener met behulp van Windows-clustering toewijzen. 

### <a name="create-an-alwayson-availability-group"></a>Maken van een AlwaysOn-beschikbaarheidsgroep

1. In SQL Management Studio ze Klik met de rechtermuisknop op **altijd op hoge beschikbaarheid** starten de **nieuwe Wizard beschikbaarheidsgroep**.
2. In **opties opgeven**, ze naam van de beschikbaarheidsgroep **SHAOG**. In **Databases selecteren**, ze de SmartHotel-database selecteren.

    ![AlwaysOn-beschikbaarheidsgroep](media/contoso-migration-rehost-vm-sql-ag/aog-1.png)

3. In **replica's opgeven**, ze de twee SQL-knooppunten als beschikbaarheidsreplica's toevoegen en configureren voor automatische failover te bieden met synchrone doorvoer.

     ![AlwaysOn-beschikbaarheidsgroep](media/contoso-migration-rehost-vm-sql-ag/aog-2.png)

4. Configureren van een listener voor de groep (**SHAOG**) en poort. Het IP-adres van de interne load balancer wordt toegevoegd als een statisch IP-adres (10.245.40.100).

    ![AlwaysOn-beschikbaarheidsgroep](media/contoso-migration-rehost-vm-sql-ag/aog-3.png)

5. In **gegevenssynchronisatie selecteren**, ze automatische seeding inschakelen. Met deze optie maakt SQL Server automatisch de secundaire replica's voor elke database in de groep zodat Contoso niet hoeft te handmatig back-up en herstel deze. Na de validatie van wordt de beschikbaarheidsgroep gemaakt.

    ![AlwaysOn-beschikbaarheidsgroep](media/contoso-migration-rehost-vm-sql-ag/aog-4.png)

6. Contoso is een probleem gedetecteerd tijdens het maken van de groep. Ze worden niet met behulp van Active Directory geïntegreerde Windows-beveiliging en moeten dus verlenen van machtigingen aan de SQL-aanmelding voor het maken van de functies van Windows-failovercluster.

    ![AlwaysOn-beschikbaarheidsgroep](media/contoso-migration-rehost-vm-sql-ag/aog-5.png)

6. Nadat de groep is gemaakt, kan deze in Contoso worden weergegeven in SQL Management Studio.

### <a name="configure-a-listener-on-the-cluster"></a>Een listener op het cluster configureren

Als een laatste stap bij het instellen van de SQL-implementatie, Contoso configureert de interne load balancer als de listener op het cluster en de listener online brengt. Ze een script gebruiken om dit te doen.

![Cluster-listener](media/contoso-migration-rehost-vm-sql-ag/cluster-listener.png)


### <a name="verify-the-configuration"></a>Controleer de configuratie

Met Alles instellen hebt Contoso nu een functionele beschikbaarheidsgroep in Azure die gebruikmaakt van de gemigreerde database. Ze controleren dit door verbinding te maken met de interne load balancer in SQL Management Studio.

![ILB verbinding maken](media/contoso-migration-rehost-vm-sql-ag/ilb-connect.png)

**Meer hulp nodig?**
- Meer informatie over het maken van een [beschikbaarheidsgroep](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#create-the-availability-group) en [listener](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#configure-listener).
- Handmatig [het cluster instellen voor gebruik van het IP-adres van de load balancer](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener#configure-the-cluster-to-use-the-load-balancer-ip-address).
- [Meer informatie](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-2) over het maken en via SAS.


## <a name="step-8-migrate-the-vm-with-site-recovery"></a>Stap 8: Migreer de virtuele machine met Site Recovery

Contoso uitvoeren van een snelle failover testen en vervolgens de virtuele machine te migreren.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Een testfailover uitgevoerd ervoor zorgt dat alles werkt zoals verwacht vóór de migratie. 

1. Contoso wordt een testfailover uitgevoerd naar het laatste punt in tijd (**meest recente verwerkte**).
2. Ze selecteren **machine afsluiten voordat u begint met failover**, zodat de Site Recovery probeert de bron-VM afsluiten voordat de failover. De failover wordt voortgezet zelfs als het afsluiten is mislukt. 
3. Test failover worden uitgevoerd: 

    - Controle wordt uitgevoerd om te controleren of alle van de voorwaarden die zijn vereist voor de migratie zijn in plaats van een vereisten.
    - De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als u het meest recente herstelpunt selecteert, wordt een herstelpunt van de gegevens gemaakt.
    - Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.

3. Nadat de failover is voltooid, wordt de replica virtuele machine in Azure weergegeven in de Azure portal. Contoso controleert of de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en dat deze wordt uitgevoerd. 
4. Nadat u hebt gecontroleerd, Contoso opschonen van de failover en vastleggen en opslaan van eventuele opmerkingen. 

### <a name="run-a-failover"></a>Een failover uitvoeren

1. Nadat u hebt gecontroleerd of de testfailover heeft gewerkt zoals verwacht, Contoso maken van een herstelplan voor migratie en WEBVM toevoegen aan de planning.

     ![Plan voor herstel](./media/contoso-migration-rehost-vm-sql-ag/recovery-plan.png)

2. Ze een failover worden uitgevoerd op het plan. Deze het meest recente herstelpunt selecteren en opgeven dat de Site Recovery proberen moet de lokale virtuele machine afsluiten voordat de failover.

    ![Failover](./media/contoso-migration-rehost-vm-sql-ag/failover1.png)

3. Na de failover controleren ze dat de virtuele machine in Azure wordt weergegeven zoals verwacht in de Azure portal.

    ![Plan voor herstel](./media/contoso-migration-rehost-vm-sql-ag/failover2.png)

6. Ze voltooien van de migratie het migratieproces voltooien en stoppen van Site Recovery facturering voor de virtuele machine replicatie stoppen voor de virtuele machine na controle van de virtuele machine in Azure.

    ![Failover](./media/contoso-migration-rehost-vm-sql-ag/failover3.png)

### <a name="update-the-connection-string"></a>De verbindingsreeks bijwerken

Als de laatste stap in het migratieproces bijwerken Contoso de verbindingsreeks van de toepassing om te verwijzen naar de gemigreerde database uitgevoerd op de listener SHAOG. Deze configuratie wordt gewijzigd op de WEBVM nu uitgevoerd in Azure.  Deze configuratie bevindt zich in het bestand web.config van de ASP-toepassing. 

1. Zoek het bestand op C:\inetpub\SmartHotelWeb\web.config.  Wijzig de naam van de server in overeenstemming met de FQDN van de AOG: shaog.contoso.com.

    ![Failover](./media/contoso-migration-rehost-vm-sql-ag/failover4.png)  

2. Na het bijwerken van het bestand en opgeslagen, ze IIS opnieuw starten op WEBVM. Ze doen dit met de IISRESET /RESTART vanaf een opdrachtprompt.
2. Nadat IIS opnieuw is opgestart, de toepassing is nu gebruikmaken van de database die wordt uitgevoerd op de SQL-MI.


**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) een testfailover uitgevoerd. 
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) een herstelplan maken.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) failover wordt uitgevoerd naar Azure.

## <a name="clean-up-after-migration"></a>Na de migratie opschonen

Na de migratie de SmartHotel-app wordt uitgevoerd op een virtuele machine van Azure en de database SmartHotel bevindt zich in de Azure SQL-cluster.

Nu moet Contoso om deze stappen opschonen te voltooien:  

- Verwijder de lokale virtuele machines van de vCenter-inventarisatie.
- Verwijder de virtuele machines van de lokale back-uptaken.
- Interne documentatie om weer te geven van de nieuwe locaties en IP-adressen voor virtuele machines bijwerken.
- Controleer alle bronnen die met de buiten gebruik gestelde VM's communiceren en eventuele relevante instellingen of documentatie in overeenstemming met de nieuwe configuratie bijwerken.
- Voeg dat de twee nieuwe virtuele machines (SQLAOG1 en SQLAOG2) moeten worden toegevoegd aan bewaking van systemen productie.

## <a name="review-the-deployment"></a>Controleer de implementatie

Met de gemigreerde bronnen in Azure moet Contoso volledig operationeel maken en hun nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

De Contoso-beveiligingsteam controleert de Azure VM's WEBVM, SQLAOG1 en SQLAOG2 om te bepalen van beveiligingsproblemen. 

- Ze bekijken de Netwerkbeveiligingsgroepen (nsg's) voor de virtuele machine om toegang te beheren. Nsg's worden gebruikt om ervoor te zorgen dat alleen verkeer toegestaan voor de toepassing kan worden doorgegeven.
- Ze overwegen de beveiliging van de gegevens op de schijf met behulp van Azure Disk Encryption en KeyVault.
- Ze moeten evalueren transparante gegevensversleuteling (TDE) en daarna inschakelen op de SmartHotel-database op de nieuwe SQL AOG. [Meer informatie](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017).

[Lees meer](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) over procedures voor beveiliging voor virtuele machines.

### <a name="backups"></a>Back-ups

Contoso wil back-up van de gegevens op WEBVM, SQLAOG1 en SQLAOG2 via de Azure Backup-service. [Meer informatie](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Licenties en kosten-optimalisatie

1. Contoso heeft een bestaande licentie voor hun WEBVM en maken gebruik van het voordeel van de hybride Azure.  Deze moeten de bestaande Azure VM's om te profiteren van deze prijzen worden geconverteerd.
2. Contoso kunnen Azure kosten Management door Cloudyn, een Microsoft-vestiging in licentie gegeven. Het is een oplossing voor het beheer van meerdere cloud kosten die u helpt te kunnen gebruiken en beheren van Azure en andere cloudbronnen.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over het beheer van Azure kosten. 

## <a name="conclusion"></a>Conclusie

In dit artikel rehosted Contoso SmartHotel-app in Azure door de app-frontend VM migreren naar Azure met behulp van de Site Recovery-service. Ze de app-database gemigreerd naar een SQL Server-cluster dat is ingericht in Azure en beveiligd in een SQL Server AlwaysOn-beschikbaarheidsgroep.

## <a name="next-steps"></a>Volgende stappen

In het volgende artikel in de reeks we laten zien hoe Contoso rehost hun service helpdesk osTicket app waarop Linux en geïmplementeerd met een MySQL-database.


