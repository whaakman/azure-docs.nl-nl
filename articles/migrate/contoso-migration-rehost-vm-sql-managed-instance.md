---
title: Een Contoso-app in Azure door te migreren naar een virtuele machine van Azure en Azure SQL-beheerde exemplaar rehost | Microsoft Docs
description: Meer informatie over hoe Contoso naamconflicten voor een lokale app op Azure VM's en Azure SQL beheerd-exemplaar
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: raynew
ms.openlocfilehash: c7dc9e8406494739aa5d8f21397a606e0b74a617
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301249"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms-and-azure-sql-managed-instance"></a>Migratie van Contoso: Rehost van een lokale app waarmee Azure VM's en Azure SQL beheerd-exemplaar

Dit artikel ziet u hoe Contoso de SmartHotel app frontend VM migreert naar Azure VM's met de Azure Site Recovery-service en de app-database naar een Azure SQL beheerd-exemplaar.

> [!NOTE]
> Azure SQL beheerd-exemplaar is momenteel in preview.

Dit document is het vierde in een serie artikelen met hoe de lokale bronnen in het fictieve bedrijf Contoso worden gemigreerd naar de Microsoft Azure-cloud-document. De reeks bevat achtergrondinformatie en een aantal scenario's die aangeven hoe u een migratie-infrastructuur instellen en uitvoeren van verschillende soorten migraties. Scenario's toenemen in complexiteit en worden er nog meer artikelen gedurende een bepaalde periode.


**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de serie artikelen en de voorbeeld-apps we gebruiken. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. De infrastructuur wordt gebruikt voor alle Contoso migratiescenario's. | Beschikbaar
[Artikel 3: Beoordelen lokale bronnen](contoso-migration-assessment.md)  | Toont hoe een beoordeling van de lokale twee lagen SmartHotel app uitgevoerd in VMware in Contoso wordt uitgevoerd. Evalueren van de virtuele machines van een app met de [Azure migreren](migrate-overview.md) -service en de app SQL Server-database met de [Azure Database migratie-assistent](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
Artikel 4: Rehost Azure VM's en een exemplaar van SQL beheerd (in dit artikel) | Demonstreert hoe Contoso de app SmartHotel migreert naar Azure. Migreren van de app frontend virtuele machine met behulp [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database met de [Azure databasemigratie](https://docs.microsoft.com/azure/dms/dms-overview) service om te migreren naar een SQL-exemplaar worden beheerd. | Beschikbaar
[Artikel 5: Rehost voor virtuele machines in Azure](contoso-migration-rehost-vm.md) | Toont hoe de app SmartHotel met Site Recovery alleen virtuele machines worden gemigreerd in Contoso.
[Artikel 6: Rehost virtuele machines in Azure en SQL Server-beschikbaarheidsgroepen](contoso-migration-rehost-vm-sql-ag.md) | Toont hoe de app SmartHotel in Contoso worden gemigreerd. Deze Site Recovery gebruiken voor het migreren van virtuele machines van de app en de migratie van de Database-service om te migreren van de app-database naar een SQL Server-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een app Linux virtuele machines in Azure Rehost](contoso-migration-rehost-linux-vm.md) | Demonstreert hoe Contoso de osTicket Linux app migreert naar Azure VM's met Site Recovery. | Beschikbaar
[Artikel 8: Rehost een Linux-app voor virtuele machines in Azure en Azure MySQL-Server](contoso-migration-rehost-linux-vm-mysql.md) | Demonstreert hoe Contoso de osTicket Linux app virtuele Azure-machines met Site Recovery worden en gemigreerd naar een Azure MySQL-Server-exemplaar met behulp van MySQL-Workbench. | Beschikbaar

Als u de SmartHotel voorbeeldapp gebruikt in dit artikel gebruiken wilt, kunt u het downloaden van [github](https://github.com/Microsoft/SmartHotel360).

## <a name="on-premises-architecture"></a>Lokale-architectuur

Hier volgt een diagram die de huidige on-premises infrastructuur voor Contoso.

![Architectuur van Contoso](./media/contoso-migration-rehost-vm-sql-managed-instance/contoso-architecture.png)  

- Contoso heeft één belangrijkste datacenter zich bevinden in de plaats van New York in het oostelijk deel van de Verenigde Staten.
- Drie extra lokale takken hebben ze in de Verenigde Staten.
- Het belangrijkste datacenter is verbonden met internet via een Fiber metro ethernet-verbinding (500 mbps).
- Elke vertakking is lokaal verbonden met internet met behulp van zakelijke klasse-verbindingen met IPSec VPN-tunnels terug naar het belangrijkste datacenter. Hierdoor hun gehele netwerk permanent zijn verbonden en optimaliseert de verbinding met internet.
- Het belangrijkste datacenter wordt volledig gevirtualiseerd met VMware. Ze hebben twee ESXi 6.5 virtualisatie-hosts worden beheerd door de vCenter-Server 6.5.
- Contoso maakt gebruik van Active Directory voor identiteitsbeheer en DNS-servers in het interne netwerk.
- Domeincontrollers in het datacenter wordt uitgevoerd op virtuele VMware-machines. De domeincontrollers op lokale vertakkingen uitgevoerd op fysieke servers.



## <a name="business-drivers"></a>Zakelijke drijfveren

De IT-leiding heeft nauw samengewerkt met hun zakelijke partners om te begrijpen wat het bedrijf wil bereiken met deze migratie:

- **Zakelijke groei adres**: Contoso groeit en als gevolg hiervan er druk op de on-premises systemen en infrastructuur.
- **De efficiëntie verhogen**: Contoso moet verwijderen van onnodige procedures en processen voor hun ontwikkelaars en gebruikers te stroomlijnen.  De bedrijfsbehoeften IT snel en niet afval tijd of geld, dus sneller leveren op klantvereisten.
- **Flexibiliteit te vergroten**: Contoso IT moet reageert op de behoeften van uw bedrijf. Moet mogelijk sneller dan de wijzigingen in de marketplace, zodat het slagen van een globale economie reageren.  Het mag niet ophalen in de manier waarop of een zakelijke upblokkering geworden.
- **Schaal**: als het bedrijf is groeit, Contoso IT-systemen die kunnen worden vergroot hetzelfde tempo moet opgeven.

## <a name="migration-goals"></a>Doelstellingen van de migratie

Het team van de cloud Contoso is vastgemaakt omlaag doelstellingen voor deze migratie. Deze doelstellingen worden gebruikt om te bepalen van de beste migratiemethode:

- Na de migratie, moet de app in Azure dezelfde prestatiemogelijkheden hebben als in hun on-premises VMWare-omgeving.  Verplaatsen naar de cloud betekent niet dat de appprestaties minder kritiek is.
- Contoso wil niet investeren in deze app.  Het is essentieel en belangrijk voor het bedrijf, maar in de huidige vorm ze gewoon wilt verplaatsen naar de cloud is.
- Database administratieve taken moeten worden geminimaliseerd nadat de app is gemigreerd.
- Contoso wil niet opnieuw gebruiken van een Azure SQL Database voor deze app en is op zoek naar alternatieven.

## <a name="proposed-architecture"></a>Voorgestelde architectuur

In dit scenario geldt het volgende:

- Contoso wil migreren van de twee lagen lokale reizen app.
- De app is gelaagd over twee VM's (WEBVM en SQLVM) en zich op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5)
- De VMware-omgeving wordt beheerd met vCenter Server 6.5 (**vcenter.contoso.com**), uitgevoerd op een virtuele machine.
- Ze migreert de app-database (SmartHotelDB) naar een beheerd van Azure SQL-exemplaar.
- Ze migreren de lokale VMware-machines naar een Azure VM.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een lokale domeincontroller (**contosodc1**).
- De lokale virtuele machines in het datacenter Contoso wordt buiten gebruik worden gesteld nadat de migratie is voltooid.

![Scenario-architectuur](media/contoso-migration-rehost-vm-sql-managed-instance/architecture.png) 

### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Database-Management-Service](https://docs.microsoft.com/azure/dms/dms-overview) | DMS kunt naadloze migraties uit meerdere databasebronnen naar Azure data-platforms, met minimale downtime. | Meer informatie over [ondersteunde regio's](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) voor DMS en get [prijsinformatie](https://azure.microsoft.com/pricing/details/database-migration/).
[Exemplaar van Azure SQL beheerd](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | Beheerde exemplaar is een beheerde databaseservice die staat voor een volledig beheerde SQL Server-exemplaar in de Azure-cloud. Het dezelfde code deelt met de nieuwste versie van SQL Server Database Engine en heeft de nieuwste functies, verbeterde prestaties en beveiligingspatches. | Met Azure SQL Database beheerd instanties die wordt uitgevoerd in Azure worden kosten in rekening op basis van capaciteit. [Meer informatie](https://azure.microsoft.com/pricing/details/sql-database/managed/). 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | De service ingedeeld en beheert de migratie en herstel na noodgevallen voor Azure VM's en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, zijn Azure Storage kosten verbonden.  Virtuele machines in Azure worden gemaakt en kosten wanneer failover plaatsvindt. [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over kosten en prijzen.

 

## <a name="migration-process"></a>migratieproces

Contoso wordt lagen van de web- en de gegevens van de app SmartHotel migreren naar Azure.

1. Contoso heeft al het Azure-infrastructuur aanwezig is, zodat deze hoeft alleen nog een aantal specifieke Azure onderdelen voor dit scenario toevoegen.
2. De gegevenslaag worden gemigreerd met behulp van de gegevens migratie Service (DMS).  DMS verbindt met het lokale SQL Server VM via een site-naar-site VPN-verbinding tussen het datacenter van de Contoso- en Azure en vervolgens de database wordt gemigreerd.
3. De weblaag worden gemigreerd met behulp van een migratie lift en shift met Azure Site Recovery. Deze opheffing voorbereiden van de on-premises VMware-omgeving instellen en inschakelen van replicatie en migreren van de virtuele machines met failover naar Azure.

     ![Migratie-architectuur](media/contoso-migration-rehost-vm-sql-managed-instance/migration-architecture.png) 


## <a name="prerequisites"></a>Vereisten

Hier volgt Contoso (en u) voor dit scenario moet.

**Vereisten** | **Details**
--- | ---
**Deelnemen aan het preview** | U moet zijn geregistreerd bij de SQL beheerd exemplaar beperkt openbare Preview. U moet een Azure-abonnement om [aanmelden](https://portal.azure.com#create/Microsoft.SQLManagedInstance). Registratie kan duren voordat een paar dagen om te voltooien, dus zorg ervoor dat u dit doen voordat u begint met het implementeren van dit scenario.
**Azure-abonnement** | U moet al hebt gemaakt een abonnement als u de evaluatie uitgevoerd in de eerste artikel in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement en u niet de beheerder bent, moet u samen met de beheerder u eigenaar of bijdrager machtigingen toewijzen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Site recovery (op locatie)** | Uw lokale vCenter-server moet versie 5.5, 6.0 of 6.5 worden uitgevoerd<br/><br/> Een ESXi-host waarop versie 5.5, 6.0 of 6.5<br/><br/> Een of meer virtuele VMware-machines uitgevoerd op de ESXi-host.<br/><br/> Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Ondersteund [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) configuratie.
**DMS** | DMS moet u een [compatibel on-premises VPN-apparaat](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> U moet mogelijk de on-premises VPN-apparaat configureren. Er moet een extern gericht openbaar IPv4-adres en het adres kan zich niet achter een NAT-apparaat.<br/><br/> Zorg ervoor dat u toegang hebt tot uw lokale SQL Server-database.<br/><br/> De Windows Firewall moet toegang kunnen krijgen tot de bron-database-engine. [Meer informatie](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Als er een firewall voor uw database-machine, kunt u regels voor toegang tot de database en bestanden via het SMB-poort 445 toevoegen.<br/><br/> De referenties waarmee verbinding maken met de bron van SQL Server en de doelinstantie worden beheerd, moeten lid zijn van de serverrol sysadmin.<br/><br/> U moet een netwerk delen in uw lokale database die DMS gebruiken kunt voor back-up van de brondatabase.<br/><br/> Zorg ervoor dat het serviceaccount met een SQL Server-exemplaar van bron schrijfrechten op de netwerkshare heeft.<br/><br/> Maak een notitie van een Windows-gebruiker (en wachtwoord) die de machtiging Volledig beheer op de netwerkshare heeft. De migratie van Azure databaseservice imiteert deze gebruikersreferenties voor het uploaden van back-upbestanden naar de Azure storage-container.<br/><br/> Het installatieproces van SQL Server Express het TCP/IP-protocol wordt ingesteld op **uitgeschakelde** standaard. Zorg ervoor dat deze ingeschakeld.


## <a name="scenario-steps"></a>Scenariostappen

Dit is hoe Contoso gaat voor het instellen van de implementatie:

> [!div class="checklist"]
> * **Stap 1: Een SQL-exemplaar voor het beheren van Azure instellen**: ze nodig hebben een vooraf gemaakte beheerde exemplaar dat de lokale SQL Server-database wilt migreren.
> * **Stap 2: Bereid DMS**: ze moeten Registreer de provider van de Database wilt migreren, geen exemplaar maken en vervolgens een DMS-project maken. Ze moeten ook naar het SA-URI voor DMS instellen. Een shared access signature (SAS) Uniform Resource Identifier (URI) biedt gedelegeerde toegang tot bronnen in uw opslagaccount, zodat u beperkte machtigingen voor opslagobjecten verlenen kunt. Ze een SAS-URI ingesteld zodat de DMS toegang heeft tot de storage-account-container waarnaar de service de SQL Server-back-up van bestanden geüpload.
> * **Stap 3: Voorbereiden op Azure Site Recovery**: voor Site-Recovery ze moeten een Azure storage-account voor het opslaan van gerepliceerde gegevens maken en een Recovery Services-kluis maken.
> * **Stap 4: Lokale VMware voorbereiden voor siteherstel**: Contoso wordt accounts voorbereiden voor VM-detectie en agent-installatie en verbinding maken met virtuele Azure-machines na een failover worden voorbereid.
> * **Stap 5: Repliceren VMs**: replicatie stelt ze configureert u de Site Recovery-bron en doel-omgeving, instellen van een beleid voor wachtwoordreplicatie en virtuele machines repliceren naar Azure-opslag.
> * **Stap 6: Migreren van de database met DMS**: nu ze van de database migreren.
> * **Stap 7: Migreer de virtuele machines met Site Recovery**: ze een testfailover om te controleren of alles werkt uitvoeren en voer vervolgens een volledige failover naar de virtuele machines migreren naar Azure.


## <a name="step-1-prepare-an-azure-sql-managed-instance"></a>Stap 1: Een beheerde Azure SQL-exemplaar voorbereiden

Als u een Azure SQL beheerd exemplaar instelt, moet Contoso een subnet:

- Het subnet moet worden toegewezen. Moet niet leeg zijn en geen andere cloud Services mag niet, en het een gatewaysubnet.
- Nadat het beheerde-exemplaar is gemaakt, kunt u resources mag niet aan toevoegen.
- Het subnet mag niet beschikken over een NSG is gekoppeld.
- Het subnet moet een gebruiker Route-tabel (UDR) met de volgende Hop Internet 0.0.0.0/0 hebben als de enige route toegewezen. 
- Optionele aangepaste DNS: als aangepaste DNS is opgegeven in het VNet, Azure recursieve resolvers IP-adres (zoals 168.63.129.16) moet worden toegevoegd aan de lijst. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).
- Het subnet mag niet beschikken over een service-eindpunt gekoppeld (opslag of SQL). Service-eindpunten moeten worden uitgeschakeld op het virtuele netwerk.
- Het subnet moet minimaal 16 IP-adressen hebben. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#determine-the-size-of-subnet-for-managed-instances) over het formaat van het subnet beheerde exemplaar.
- Aangepaste DNS-instellingen zijn in hun omgeving hybride nodig. Contoso configureert DNS-instellingen voor het gebruik van een of meer van de Azure DNS-servers. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns) over DNS-aanpassing.

### <a name="set-up-a-virtual-network-for-the-managed-instance"></a>Een virtueel netwerk voor het beheerde exemplaar instellen

Contoso heeft ingesteld het VNet als volgt: 

1. Contoso maakt een nieuw VNet (VNET SQLMI EU2) in de primaire regio VS-Oost 2 in de resourcegroep ContosoNetworkingRG.
2. Contoso wijst een adresruimte van 10.235.0.0/24, waarbij u ervoor zorgt dat het bereik niet met andere netwerken in de Contoso-onderneming overlappen.
2. Deze toevoegen twee subnetten aan het netwerk:
    - SQLMI DS EUS2 (10.235.0.0.25)
    - SQLMI-ZAG-EUS2 (10.235.0.128/29). Dit subnet wordt gebruikt voor het koppelen van de directory aan het beheerde exemplaar (SQLMI).

    ![Beheerde exemplaar netwerk](media/contoso-migration-rehost-vm-sql-managed-instance/mi-vnet.png)

6. Nadat de VNet en subnetten worden geïmplementeerd, samenwerkt Contoso netwerken als volgt:

    - VNET-SQLMI-EUS2 van de peers met VNET HUB EUS2 (de hub VNet voor de VS-Oost 2).
    - VNET-SQLMI-EUS2 van de peers met VNET PROD EUS2 (het productienetwerk).

    ![Netwerk-peering](media/contoso-migration-rehost-vm-sql-managed-instance/mi-peering.png)

7. Contoso stelt aangepaste DNS-instellingen. DNS verwijst eerst naar de Azure-DC's. Azure DNS worden secundaire. De Contoso Azure DC's zich bevinden als volgt:

    - Bevindt zich in het subnet van de PROD-DC-EUS2 in de VS-Oost 2 productie-netwerk (VNET-PROD-EUS2).
    - CONTOSODC3 adres: 10.245.42.4
    - CONTOSODC4 adres: 10.245.42.5
    - Azure DNS-resolver: 168.63.129.16

     ![Netwerk DNS](media/contoso-migration-rehost-vm-sql-managed-instance/mi-dns.png)

**Meer hulp nodig?**

- [Een overzicht](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) van beheerde exemplaren van Azure SQL.
- [Meer informatie over](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#create-a-new-virtual-network-for-managed-instances) over het maken van een VNet voor SQL-exemplaar worden beheerd.
- [Meer informatie over](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering) peering in te stellen.
- [Meer informatie over](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-dns) Azure AD-DNS-instellingen worden bijgewerkt.



### <a name="set-up-routing"></a>Routering instellen

Het beheerde-exemplaar wordt geplaatst in een persoonlijke VNET zodat Contoso moet een routetabel om te communiceren met de Azure Management-Service. Als deze niet kan met de service die wordt beheerd communiceren, wordt deze niet toegankelijk.

- De routetabel bevat een reeks regels (routes) die aangeeft hoe pakketten worden verzonden vanuit beheerde exemplaar in het VNet moeten worden gerouteerd.
- De routetabel is gekoppeld aan subnetten waarin exemplaren beheerd zijn geïmplementeerd. Elk pakket verlaten van een subnet wordt uitgevoerd op basis van de routetabel gekoppeld.
- Een subnet kan alleen worden gekoppeld aan één routetabel.
- Er zijn geen extra kosten voor het routetabellen maken in Microsoft Azure.

1. Contoso maakt een tabel van de gebruiker gedefinieerde route. De routetabel is gemaakt in de resourcegroep ContosoNetworkingRG.

    ![Routetabel](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table.png)

2. Om te voldoen aan vereisten voor beheerde exemplaar na de implementatie van de routetabel (MIRouteTable), Contoso een route met adresvoorvoegsel 0.0.0.0/0, toevoegen en **volgende hop type** ingesteld op **Internet**.

    ![Tabel routeprefix](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-prefix.png)
    
3. De routetabel Contoso koppelen aan het subnet SQLMI-DB-EUS2 (in het netwerk van de VNET-SQLMI-EUS2). 

    ![Route tabel subnet](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-subnet.png)
    
**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal#create-new-route-table-and-a-route) routes voor een beheerde exemplaar in te stellen.

### <a name="create-a-managed-instance"></a>Maak een beheerde exemplaar

Contoso kan nu een SQL-Database beheerd exemplaar inrichten.

1. Omdat het exemplaar beheerd fungeert een business-app, Contoso implementeren in hun primaire regio VS-Oost 2, in de resourcegroep ContosoRG 
2. Ze selecteren een prijscategorie laag, en de grootte van berekening en opslag voor het exemplaar. [Meer informatie](https://azure.microsoft.com/pricing/details/sql-database/managed/) over prijzen.

    ![Beheerd exemplaar](media/contoso-migration-rehost-vm-sql-managed-instance/mi-create.png)

3. Nadat het exemplaar beheerd is geïmplementeerd, worden twee nieuwe resources weergegeven in de resourcegroep ContosoRG:

    - Een virtueel cluster als er meerdere beheerde exemplaren,
    - De SQL-Server beheerd exemplaar. 

    ![Beheerd exemplaar](media/contoso-migration-rehost-vm-sql-managed-instance/mi-resources.png)

**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal) inrichten van een exemplaar beheerd.

## <a name="step-2-prepare-dms"></a>Stap 2: DMS voorbereiden

Moeten een aantal dingen doen om voor te bereiden DMS Contoso:

- De DMS-provider in Azure registreren
- DMS toegang bieden tot Azure-opslag voor het uploaden van de back-upbestanden gebruikt voor het migreren van een database. Ze dit doen door het maken van een blob storage-container en een Shared Access Signature (SAS) URI voor het genereren. 
- Maak een DMS-project.


Volg de stappen als volgt:

1. Contoso Registreer de provider van de migratie van de Database onder het abonnement.
    ![DMS registreren](media/contoso-migration-rehost-vm-sql-managed-instance/dms-subscription.png)

2. Ze maken van een blob storage-container en een SAS-URI genereren zodat DMS toegang kunnen hebben.

    ![DE SAS-URI](media/contoso-migration-rehost-vm-sql-managed-instance/dms-sas.png)

3. Ten slotte maakt ze een DMS-exemplaar. 

    ![DMS-exemplaar](media/contoso-migration-rehost-vm-sql-managed-instance/dms-instance.png)

4. Contoso plaatst het DMS-exemplaar in het subnet PROD-DC-EUS2 van de VNET-PROD-DC-EUS2 VNet.
    - Ze plaatst u het er omdat het moet zich binnen een VNet dat toegang heeft tot de lokale SQL Server VM via een VPN-gateway.
    - VNET-PROD-EUS2 is ingesteld als peer voor VNET-HUB-EUS2 en gebruiken van externe gateways is toegestaan.  Dit zorgt ervoor dat de DMS zo nodig kan communiceren.

        ![DMS-netwerk](media/contoso-migration-rehost-vm-sql-managed-instance/dms-network.png)

**Meer hulp nodig?**
- [Meer informatie over](https://docs.microsoft.com/azure/dms/quickstart-create-data-migration-service-portal) DMS in te stellen.
- [Meer informatie](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-2) over het maken en via SAS.


## <a name="step-3-prepare-azure-for-the-site-recovery-service"></a>Stap 3: Voorbereiden op Azure Site Recovery-service

Er zijn een aantal Azure elementen die Contoso behoefte heeft aan om het instellen van Site Recovery voor de migratie van hun weblaag VM (WEBMV)

- Failover van een VNet waarin resources zich bevinden.
- Een Azure storage-account voor gerepliceerde gegevens. 
- Een Recovery Services-kluis in Azure.

Deze instellen Site Recovery als volgt:

1. Aangezien de virtuele machine een web-frontend naar de app SmartHotel is, mislukken deze Apps via de virtuele machine aan hun bestaande productienetwerk (VNET-PROD-EUS2) en het subnet (PROD-FE-EUS2) in de primaire regio van de VS-Oost 2. Ze ingesteld op dit netwerk wanneer ze [geïmplementeerd de Azure-infrastructuur](contoso-migration-infrastructure.md)
2. Het maken van een Azure storage-account (contosovmsacc20180528). Ze een algemeen-account gebruikt met de standard-opslag en LRS-replicatie.

    ![Site Recovery-opslag](media/contoso-migration-rehost-vm-sql-managed-instance/asr-storage.png)

3. Met de netwerk- en storage-account in plaats kan Contoso nu een kluis (ContosoMigrationVault) maken en plaats deze in de resourcegroep ContosoFailoverRG in de primaire regio van de VS-Oost 2.

    ![Recovery Services-kluis](media/contoso-migration-rehost-vm-sql-managed-instance/asr-vault.png)

**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Azure instellen voor de Site Recovery.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>Stap 4: On-premises VMware voor herstel van sites voorbereiden

Als u VMware voorbereiden voor herstel van sites, is dit wat Contoso moet doen:

- Bereid een account op de vCenter-server of vSphere ESXi-host, VM detectie automatiseren.
- Bereid een account waarmee de automatische installatie van de Mobility-service op de VMware-machines die u wilt repliceren.
- Voorbereiden op de lokale virtuele machines, zodat ze verbinding met virtuele Azure-machines maken kunnen wanneer ze zijn gemaakt na een failover.


### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Virtuele machines automatisch detecteren. Er is minimaal een alleen-lezen-account vereist.
- Replicatie, failover en failback orkestreren. U moet een account die bewerkingen zoals het maken en schijven verwijderen en het inschakelen van virtuele machines kan worden uitgevoerd.

Contoso stelt u het account als volgt:

1. Maakt een rol op de vCenter-niveau.
2. De vereiste machtigingen toekent aan die rol.

**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) maken en toewijzen van een functie voor automatische detectie.

### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op de VM die u wilt repliceren.

- Site Recovery kan een automatische push-installatie van dit onderdeel kunt doen wanneer u replicatie voor de virtuele machine inschakelt.
- Voor automatische push-installatie moet u een account die door Site Recovery wordt gebruikt voor toegang tot de virtuele machine voorbereiden.
- U geeft dit account bij het instellen van de replicatie in de Azure-console.
- U moet een domein of lokale account met machtigingen op de virtuele machine te installeren.

**Meer hulp nodig hebt**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) maken van een account voor push-installatie van de Mobility-service.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Na een failover naar Azure wil Contoso verbinding kunnen maken met de gerepliceerde virtuele machines in Azure. Om dit te doen, is er een aantal dingen die ze moeten doen op de lokale virtuele machine, voordat ze de migratie worden uitgevoerd: 

1. Voor toegang via het internet, schakelt u RDP op de lokale virtuele machine voordat failover wordt uitgevoerd en zorg ervoor dat TCP en UDP-regels zijn toegevoegd voor de **openbare** profiel, en dat RDP is toegestaan in **Windows Firewall**  >  **Apps toegestaan** voor alle profielen.
2. Voor toegang via hun site-naar-site VPN, schakelt u RDP in op de on-premises machine en toestaan dat RDP in de **Windows Firewall** -> **toegestane apps en functies** voor **domein en Persoonlijke** netwerken.
3. Ze SAN-beleid van het besturingssysteem op de lokale virtuele machine ingesteld **OnlineAll**.

Wanneer ze een failover uitvoert moeten deze bovendien controleert u het volgende:

- Er moet geen Windows-updates in behandeling op de virtuele machine bij activering van een failover. Als er, is ze niet kunnen aanmelden bij de virtuele machine totdat de update is voltooid.
- Na een failover moeten ze controleren **opstarten diagnostics** om een schermopname van de virtuele machine weer te geven. Als dit niet werkt, moeten ze controleren dat de virtuele machine wordt uitgevoerd en deze [tips voor probleemoplossing](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>Stap 5: De on-premises virtuele machines repliceren naar Azure met Site Recovery

Voordat u een migratie naar Azure, moet Contoso de replicatie instellen en inschakelen van replicatie voor de lokale virtuele machine.

### <a name="set-a-replication-goal"></a>Doelstelling replicatie instellen

1. In de kluis onder de kluisnaam van de (ContosoVMVault) ze ingesteld een replicatie-doel (**aan de slag** > **siteherstel** > **infrastructuur voorbereiden**.
2. Deze opgeven dat hun computers zich op locatie bevinden, dat ze zijn virtuele VMware-machines, en dat ze wilt repliceren naar Azure.

    ![Replicatiedoel](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Controleer de implementatie plannen

Als u wilt doorgaan, ze nodig hebben om te bevestigen dat ze hebben voltooid implementatie plannen door te selecteren **Ja, ik dit hebt gedaan**. In deze implementatie Contoso zijn slechts één VM migreren en hoeft niet-implementatie plannen.

### <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Nu moeten ze configureren hun bronomgeving. Om dit te doen ze een OVF-sjabloon downloaden en gebruiken voor het implementeren van de configuratieserver en de bijbehorende onderdelen als een maximaal beschikbare, lokale VMware VM. Onderdelen op de server zijn onder andere:

- De configuratieserver die de communicatie tussen de on-premises en Azure coördineert en beheert gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag.
- De processerver installeert ook Mobility Service op VM's die u wilt repliceren, en detecteert automatisch on-premises VMware-VM's.
- Na de configuratieserver VM is gemaakt en gestart, kan Contoso registreren in de kluis.


Contoso uitvoeren als volgt te werk:

1. Ze de OVF-sjabloon downloaden van de Azure-portal (**infrastructuur voorbereiden** > **bron** > **configuratieserver**).
    
    ![OVF downloaden](./media/contoso-migration-rehost-vm-sql-managed-instance/add-cs.png)

2. Ze importeren de sjabloon in VMware maken en implementeren van de virtuele machine.

    ![OVF-sjabloon](./media/contoso-migration-rehost-vm-sql-managed-instance/vcenter-wizard.png)

3.  Wanneer ze de virtuele machine voor het eerst inschakelt, wordt deze opgestart naar een Windows Server 2016-installatie-ervaring. Ze de licentieovereenkomst accepteren en voer een administrator-wachtwoord.
4. Nadat de installatie is voltooid, ze zich aanmelden bij de virtuele machine als de beheerder. Bij het eerst aanmelden, wordt standaard de Azure Site Recovery Configuration Tool uitgevoerd.
5. In het hulpmiddel Geef ze een naam moet worden gebruikt bij de configuratieserver in de kluis te registreren.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, selecteren ze **aanmelden**, aan te melden bij de Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin de configuratieserver wordt geregistreerd. 

    [Configuratieserver registreren](./media/contoso-migration-rehost-vm-sql-managed-instance/config-server-register2.png)

7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op. Ze opnieuw aan te melden bij de computer en de configuratiewizard voor het beheer van Server automatisch wordt gestart.
8. In de wizard selecteren ze de NIC van binnenkomend replicatieverkeer. Deze instelling kan niet worden gewijzigd nadat deze geconfigureerd.
9. Ze selecteert het abonnement, de resourcegroep en de kluis waarin de configuratieserver registreren.
        ![Kluis](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz1.png) 

10. Ze vervolgens download en installeer MySQL-Server en VMWare PowerCLI. Vervolgens valideren zij de serverinstellingen.
11. Na de validatie Geef ze de FQDN-naam of IP-adres van de vCenter-server of vSphere-host. Ze laat de standaardpoort en geef een beschrijvende naam voor de vCenter-server in Azure.
12. Ze moeten naar het account opgeven dat ze eerder hebt gemaakt, zodat de Site Recovery kan automatisch detecteren voor virtuele VMware-machines die beschikbaar voor replicatie zijn. 
13. Ze geven de referenties voor de Mobility-Service automatisch geïnstalleerd wanneer replicatie is ingeschakeld. Voor Windows-machines moet het account dat lokale administrator-bevoegdheden op de virtuele machines. 

    ![vCenter](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz2.png)

7. Nadat de registratie is voltooid, wordt in de Azure portal Contoso dubbele controleert of de configuratieserver en de VMware-server aanwezig zijn op de **bron** pagina in de kluis. Detectie kan 15 minuten of langer duren. 
8. Site Recovery vervolgens verbinding maakt met de VMware-servers met de opgegeven instellingen en virtuele machines worden gedetecteerd.

### <a name="set-up-the-target"></a>Het doel instellen

Contoso moet nu de doelomgeving replicatie configureren.

1. In **infrastructuur voorbereiden** > **doel**, ze de doelinstellingen selecteren.
2. Site Recovery controleert of er een Azure storage-account en het netwerk in het opgegeven doel is.

### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

Nadat de bron en doel zijn ingesteld, is Contoso gereed om te maken van een beleid voor wachtwoordreplicatie en deze koppelen aan de configuratieserver.

1. In **infrastructuur voorbereiden** > **replicatie-instellingen** > **replicatiebeleid** >  **maken en Koppelen**, maak een beleid **ContosoMigrationPolicy**.
2. Ze de standaardinstellingen gebruiken:
    - **Drempelwaarde voor RPO**: standaard 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
    - **Herstel bewaarperiode**. De standaardwaarde van 24 uur. Deze waarde geeft aan hoe lang de bewaarperiode voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
    - **App-consistente momentopname frequentie**. De standaardwaarde van één uur. Deze waarde bepaalt de frequentie waarmee toepassingsconsistente momentopnamen worden gemaakt.
 
        ![Replicatiebeleid maken](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy.png)

5. Het beleid wordt automatisch gekoppeld aan de configuratieserver. 

    ![Beleid voor wachtwoordreplicatie koppelen](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy2.png)


**Meer hulp nodig?**

- U kunt een overzicht van deze stappen in lezen [herstel na noodgevallen instellen voor de lokale virtuele VMware-machines](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Gedetailleerde instructies zijn beschikbaar om u te helpen [de bronomgeving instellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementeren van de configuratieserver](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), en [replicatie-instellingen configureren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).

### <a name="enable-replication"></a>Replicatie inschakelen

Contoso kunt nu beginnen de WebVM repliceren.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze selecteert u de instellingen van de bronserver.
2. Ze geven aan dat ze willen, kunnen virtuele machines, selecteert u de vCenter-server en de configuratieserver.

 ![Replicatie inschakelen](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication1.png)
 
3. Nu ze de doelinstellingen opgeven, met inbegrip van de resourcegroep en netwerk waarin de virtuele machine in Azure geplaatst na een failover worden en het opslagaccount waarin de gerepliceerde gegevens worden opgeslagen.

     ![Replicatie inschakelen](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication2.png)

4. Contoso selecteert de WebVM voor replicatie. Site Recovery installeert de Mobility-Service op elke virtuele machine wanneer u replicatie inschakelt. 

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication3.png)

5. Contoso controleert of het juiste replicatiebeleid is geselecteerd, en Hiermee schakelt u replicatie voor WEBVM. Bijhouden van de Replicatievoortgang van de in **taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.
6. In **Essentials** in de Azure-portal Contoso ziet de structuur voor de virtuele machines repliceren naar Azure.

    ![Weergave van de infrastructuur](./media/contoso-migration-rehost-vm-sql-managed-instance/essentials.png)


**Meer hulp nodig?**

U kunt een overzicht van deze stappen in lezen [replicatie inschakelen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).

## <a name="step-6-migrate-the-database-with-dms"></a>Stap 6: De database met DMS migreren

Contoso moet een DMS-project maken en migreren van de database.

### <a name="create-a-dms-project"></a>Een DMS-project maken
1. Ze maken een DMS-project. Deze opgeven het brontype van de server als de SQL Server en het doel als beheerd-exemplaar van Azure SQL Database.

     ![DMS-project](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-project.png)

2. Nadat het project is gemaakt, is de migratiewizard wordt geopend.

### <a name="migrate-the-database"></a>Migreren van de database 

1. In de Wizard migratie Contoso Hiermee geeft u de bron-VM waarop de lokale-database zich bevindt en referenties voor toegang.

    ![DMS-bron](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source.png)

2. Ze selecteert u de database te migreren (SmartHotel.Registration).

    ![DMS-brondatabase](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-sourcedb.png)

3. Als doel Geef ze de naam van het exemplaar beheerd in Azure en referenties voor toegang.

    ![DMS-doel](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-target-details.png)

4. Klik op **+ nieuwe activiteit** > **migratie uitvoeren**, ze instellingen opgeven voor de migratie uitvoeren:
    - Referenties voor de bron en doel.
    - De database te migreren.
    - De netwerkshare die ze op de lokale virtuele machine hebt gemaakt. DMS gaat bron back-ups naar deze share.
        - Het serviceaccount met de bron van SQL Server exemplaar moet hebben schrijfrechten op deze share.
        - Geef de FQDN-pad naar de share.
    - De SAS-URI die de DMS biedt toegang tot de storage-account-container waarnaar de service de back-upbestanden voor de migratie uploadt.

        ![DMS-instellingen](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-migration-settings.png)

5. Ze opslaan van de migratie en voer dit.
6. In **overzicht**, controleren ze de migratiestatus van de.

    ![DMS-monitor](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor1.png)

7. Nadat de migratie is voltooid, controleren ze dat de doeldatabases op de beheerde exemplaar bestaat.

    ![DMS-monitor](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor2.png)

## <a name="step-7-migrate-the-vm-with-site-recovery"></a>Stap 7: Migreer de virtuele machine met Site Recovery

Contoso een snelle testfailover wordt uitgevoerd en vervolgens de virtuele machine te migreren.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Voordat u migreert WEBVM, een testfailover helpt Zorg ervoor dat alles werkt zoals verwacht. 

1. Ze een testfailover uitvoeren naar het laatste punt in tijd (**meest recente verwerkte**).
2. Ze selecteren **machine afsluiten voordat u begint met failover**, zodat de Site Recovery probeert de bron-VM afsluiten voordat de failover. De failover wordt voortgezet zelfs als het afsluiten is mislukt. 
3. Test failover worden uitgevoerd: 

    - Controle wordt uitgevoerd om te controleren of alle van de voorwaarden die zijn vereist voor de migratie zijn in plaats van een vereisten.
    - De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als u het meest recente herstelpunt selecteert, wordt een herstelpunt van de gegevens gemaakt.
    - Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.
3. Nadat de failover is voltooid, wordt de replica virtuele machine in Azure weergegeven in de Azure portal. Ze controleren of alles correct werkt. De virtuele machine is de juiste grootte, verbonden met het juiste netwerk en deze wordt uitgevoerd. 
4. Na controle van de testfailover Contoso de failover- en records ruimt en eventuele opmerkingen worden opgeslagen. 

### <a name="migrate-the-vm"></a>De virtuele machine migreren

1. Contoso maakt nadat u hebt gecontroleerd of de testfailover heeft gewerkt zoals verwacht, kan een herstelplan voor migratie. Ze toevoegen WEBVM aan de planning.

     ![Plan voor herstel](./media/contoso-migration-rehost-vm-sql-managed-instance/recovery-plan.png)

2. Vervolgens zij een failover uitgevoerd op het plan. Deze het meest recente herstelpunt selecteren en opgeven dat de Site Recovery proberen moet de lokale virtuele machine afsluiten voordat de failover.

    ![Failover](./media/contoso-migration-rehost-vm-sql-managed-instance/failover1.png)

3. Na de failover Contoso controleren dat de virtuele machine in Azure wordt weergegeven zoals verwacht in de Azure portal.

   ![Plan voor herstel](./media/contoso-migration-rehost-vm-sql-managed-instance/failover2.png)

4. Ze voltooien van de migratie het migratieproces voltooien en stoppen van Site Recovery facturering voor de virtuele machine replicatie stoppen voor de virtuele machine na controle van de virtuele machine in Azure.

    ![Failover](./media/contoso-migration-rehost-vm-sql-managed-instance/failover3.png)

### <a name="update-the-connection-string"></a>De verbindingsreeks bijwerken

Als de laatste stap in het migratieproces updates Contoso de verbindingsreeks van de toepassing om te verwijzen naar de gemigreerde uitgevoerd op hun MI SQL-database.

1. In de Azure portal ze de verbindingsreeks kunt vinden door te klikken op **instellingen** > **verbindingsreeksen**.

    ![Failover](./media/contoso-migration-rehost-vm-sql-managed-instance/failover4.png)  

2. Ze bijwerken de tekenreeks met de gebruikersnaam en wachtwoord van het SQL-exemplaar worden beheerd.
3. Nadat de tekenreeks is geconfigureerd, vervangt u de huidige verbindingsreeks in het bestand web.config van de toepassing.
4. Na het bijwerken van het bestand en opgeslagen, ze IIS opnieuw starten op WEBVM. Ze doen dit met **IISRESET /RESTART** vanaf een opdrachtprompt.
5. Nadat IIS opnieuw is opgestart, wordt de app gebruiken de database die wordt uitgevoerd op de SQL-exemplaar worden beheerd.
6. Op dit moment Contoso de SQLVM machine on-premises kunt afsluiten en de migratie is voltooid.

**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) een testfailover uitgevoerd. 
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) een herstelplan maken.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) failover wordt uitgevoerd naar Azure.

## <a name="clean-up-after-migration"></a>Na de migratie opschonen

Met de migratie is voltooid, de SmartHotel-app wordt uitgevoerd op een virtuele machine van Azure en de database SmartHotel is beschikbaar op de Azure SQL beheerd-exemplaar.  

Nu moet Contoso sommige opruimen, als volgt:  

- Verwijder de WEBVM-machine uit de vCenter-inventaris.
- Verwijder de SQLVM-machine uit de vCenter-inventaris.
- WEBVM en SQLVM verwijderen uit lokale back-uptaken.
- De nieuwe locatie, IP-adres voor WEBVM update interne documentatie weergeven
- Verwijder SQLVM van documentatie. Ook kunnen ze Markeer deze als u wilt weergeven als verwijderd en niet langer in de virtuele machine inventariseren.
- Controleer alle bronnen die met de buiten gebruik gestelde VM's communiceren en eventuele relevante instellingen of documentatie in overeenstemming met de nieuwe configuratie bijwerken.

## <a name="review-the-deployment"></a>Controleer de implementatie

Met de gemigreerde bronnen in Azure moet Contoso volledig operationeel maken en hun nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

De Contoso-beveiligingsteam controleert de Azure VM's en de SQL-exemplaar worden beheerd, om te bepalen van beveiligingsproblemen met de implementatie.

- Ze controleren de Netwerkbeveiligingsgroepen (nsg's) voor de virtuele machine, om toegang te beheren. Nsg's worden gebruikt om ervoor te zorgen dat alleen verkeer toegestaan bij de app kan worden doorgegeven.
- Ze ook overwegen de beveiliging van de gegevens op de schijf met behulp van Azure Disk Encryption en Azure KeyVault.
- Ze inschakelen detectie van dreigingen op SQL-beheerde exemplaar (SQLMI). Ze stuurt waarschuwingen naar hun system security team/service helpdesk een ticket openen als een bedreiging wordt gedetecteerd. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-threat-detection).

     ![Beveiliging van beheerde exemplaar](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-security.png)  

[Lees meer](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) over procedures voor beveiliging voor virtuele machines.

### <a name="backups"></a>Back-ups
Contoso gaat naar de back-up van de gegevens op WEBVM via de Azure Backup-service. [Meer informatie](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Licenties en kosten-optimalisatie

1. Contoso heeft een bestaande licentie voor WEBVM en maken gebruik van het voordeel van de hybride Azure.  Deze worden de bestaande virtuele machine van Azure om te profiteren van deze prijscategorie wordt geconverteerd.
2. Contoso kunnen Azure kosten Management door Cloudyn, een Microsoft-vestiging in licentie gegeven. Het is een oplossing voor het beheer van meerdere cloudkosten waarmee kunnen gebruiken en beheren van Azure en andere cloudbronnen.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over het beheer van Azure kosten. 


## <a name="conclusion"></a>Conclusie

In dit artikel rehosted Contoso SmartHotel-app in Azure door de app-frontend VM migreren naar Azure met behulp van de Site Recovery-service. Deze lokale-database naar een Azure SQL beheerd exemplaar met gebruikmaking van DMS gemigreerd.

## <a name="next-steps"></a>Volgende stappen

In het volgende artikel in de reeks laten we zien hoe Contoso rehost voor de app SmartHotel virtuele Azure-machines met alleen de Azure Site Recovery-service.

