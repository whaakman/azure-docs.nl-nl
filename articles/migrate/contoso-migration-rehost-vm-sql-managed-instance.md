---
title: Een on-premises Contoso app rehost door te migreren naar een virtuele machine van Azure en Azure SQL Managed Instance | Microsoft Docs
description: Informatie over hoe Contoso naamconflicten met een on-premises-app op Azure VM's en Azure SQL Managed Instance
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: 49a5fb13a881b206c36dcd08a4c2945880e9a4bd
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39002293"
---
# <a name="contoso-migration-rehost-an-on-premises-app-on-azure-vms-and-azure-sql-managed-instance"></a>Migratie van Contoso: een on-premises-app op Azure VM's en Azure SQL Managed Instance Rehost

Dit artikel leest u hoe Contoso de SmartHotel app front-end virtuele machine migreert naar Azure-VM's met behulp van de Azure Site Recovery-service en de app-database naar een Azure SQL Managed Instance.

> [!NOTE]
> Azure SQL Managed Instance is momenteel in preview.

Dit document is een in een reeks artikelen waarin hoe het fictieve bedrijf Contoso de on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en een aantal scenario's die laten zien hoe u een migratie-infrastructuur instellen en uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit en we hierna zullen toevoegen aanvullende artikelen na verloop van tijd.


**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de artikel-serie en de voorbeeld-apps die we gebruiken. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen. | Beschikbaar
[Artikel 3: Evalueer on-premises bronnen voor migratie naar Azure](contoso-migration-assessment.md)  | Laat zien hoe een evaluatie van een on-premises twee lagen SmartHotel app waarop VMware wordt uitgevoerd in Contoso. Contoso beoordeelt de virtuele machines van een app met de [Azure Migrate](migrate-overview.md) -service en de SQL Server-database van de app met de [Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
Artikel 4: Een app op Azure VM's en een beheerde SQL-exemplaar opnieuw hosten | Ziet u hoe Contoso een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel app uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database naar een SQL beheerd exemplaar, met behulp van de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | In dit artikel.
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | Ziet u hoe Contoso de app SmartHotel virtuele machines migreren naar Azure-VM's, met behulp van de Site Recovery-service. | Beschikbaar
[Artikel 6: Een app op Azure VM's en SQL Server Always On Availability Group opnieuw hosten](contoso-migration-rehost-vm-sql-ag.md) | Laat zien hoe de app SmartHotel door Contoso worden gemigreerd. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app en de Database Migration service de app-database migreren naar een SQL Server-cluster dat is beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Ziet u hoe Contoso heeft een lift-and-shift-migratie van de Linux-osTicket-app op virtuele machines van Azure met Site Recovery | Beschikbaar
[Artikel 8: Een Linux-app op Azure VM's en Azure MySQL Rehost](contoso-migration-rehost-linux-vm-mysql.md) | Ziet u hoe Contoso de osTicket Linux app overzet naar virtuele Azure-machines met behulp van Site Recovery en de app-database migreert naar een Azure MySQL-Server-exemplaar met behulp van MySQL Workbench. | Beschikbaar
[Artikel 9: Een app op Azure Web Apps en Azure SQL database herstructureren](contoso-migration-refactor-web-app-sql.md) | Laat zien hoe Contoso de app SmartHotel migreert naar een Azure-Web-App en de app-database migreert naar Azure SQL Server-exemplaar | Beschikbaar
[Artikel 10: Een Linux-app op Azure-Web-Apps en Azure MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | Ziet u hoe Contoso de osTicket Linux app migreert naar Azure Web Apps op meerdere locaties, geïntegreerd met GitHub voor continue levering. Zij migreren de app-database naar een Azure MySQL-exemplaar. | Beschikbaar
[Artikel 11: Herstructureren TFS op VSTS](contoso-migration-tfs-vsts.md) | Ziet u hoe Contoso hun Team Foundation Server (TFS) on-premises implementatie door te migreren migreert het naar Visual Studio Team Services (VSTS) in Azure. | Beschikbaar
[Artikel 12: Opnieuw ontwerpen van een app op Azure-containers en Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Ziet u hoe Contoso migreert en rearchitects hun SmartHotel app naar Azure. Ze opnieuw ontwerpen voor de laag van de web-app als een Windows-container en de app-database in een Azure SQL Database. | Beschikbaar
[Artikel 13: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Ziet u hoe Contoso hun SmartHotel-app met een scala aan mogelijkheden van Azure en -services, waaronder App Services, Azure Kubernetes, Azure Functions, Cognitive services en Cosmos DB opnieuw. | Beschikbaar


Als u de SmartHotel voorbeeldapp gebruikt in dit artikel gebruiken wilt, kunt u het downloaden van [github](https://github.com/Microsoft/SmartHotel360).

## <a name="on-premises-architecture"></a>On-premises-architectuur

Hier volgt een diagram van de huidige on-premises infrastructuur voor Contoso.

![Contoso-architectuur](./media/contoso-migration-rehost-vm-sql-managed-instance/contoso-architecture.png)  

- Contoso heeft een hoofddatacenter zich bevindt in de plaats van New York in het oostelijk deel van de Verenigde Staten.
- Deze hebt u drie extra lokale branches in de Verenigde Staten.
- Het belangrijkste datacenter is verbonden met internet met een Fiber metro Ethernet-verbinding (500 mbps).
- Elke vertakking is lokaal verbonden met internet met behulp van zakelijke klasse verbindingen met IPSec VPN-tunnels terug naar het primaire datacenter. Hiermee wordt het hele netwerk permanent zijn verbonden, en optimaliseert de verbinding met internet.
- Het belangrijkste datacenter is volledig gevirtualiseerd met VMware. Ze hebben twee ESXi 6.5-virtualisatiehosts, beheerd door vCenter Server 6.5.
- Contoso maakt gebruik van Active Directory voor identiteits- en DNS-servers in het interne netwerk.
- De domeincontrollers in het datacenter worden uitgevoerd op virtuele VMware-machines. De domeincontrollers op lokale branches uitgevoerd op fysieke servers.



## <a name="business-drivers"></a>Zakelijke drijfveren

Het team voor IT-leidinggevenden heeft nauw samengewerkt met hun zakelijke partners om te begrijpen wat het bedrijf wil bereiken met deze migratie:

- **Adres van de groei van het bedrijf**: Contoso wordt uitgebreid en er wordt als gevolg hiervan druk op de on-premises systemen en infrastructuur.
- **De efficiëntie verhogen**: Contoso moet verwijderen van onnodige procedures en processen voor ontwikkelaars en gebruikers van de stroomlijnen.  De bedrijfsbehoeften IT snel en niet afval tijd of geld, dus sneller leveren op de eisen van klanten.
- **De veerkracht vergroten**: Contoso IT moet sneller te reageren op de behoeften van het bedrijf. Het moet mogelijk zijn om te reageren sneller dan de wijzigingen in de marketplace, waarmee het succes in een globale economie.  Het kan niet ophalen in de manier waarop of een zakelijke upblokkering worden.
- **Schaal**: wanneer het bedrijf met succes groeit, Contoso IT-systemen die zijn kunnen groeien met de snelheid die dezelfde moet opgeven.

## <a name="migration-goals"></a>Migratie-doelen

Het Contoso-cloud-team heeft vastgemaakt omlaag doelstellingen voor deze migratie. Deze doelstellingen worden gebruikt om te bepalen van de beste migratiemethode:

- Na de migratie, moet de app in Azure dezelfde prestatiemogelijkheden hebben als in hun on-premises VMWare-omgeving.  Verplaatsen naar de cloud betekent niet dat de app-prestaties minder kritiek is.
- Contoso wil niet investeren in deze app.  Het essentiële en belangrijk voor het bedrijf is, maar in de huidige vorm ze gewoon wilt verplaatsen naar de cloud is.
- Beheertaken voor database moeten worden geminimaliseerd nadat de app is gemigreerd.
- Contoso-niet wilt gebruiken van een Azure SQL Database voor deze app en er wordt gezocht naar alternatieven.

## <a name="proposed-architecture"></a>Voorgestelde architectuur

In dit scenario geldt het volgende:

- Contoso wil de twee lagen on-premises reis-app migreren.
- De app is gelaagd over twee VM's (WEBVM en SQLVM) en zich op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5)
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) uitgevoerd op een virtuele machine.
- Zij migreren de app-database (SmartHotelDB) naar een Azure SQL beheerd exemplaar.
- Zij migreren de on-premises VMware-machines naar een Azure-VM.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een on-premises domeincontroller (**contosodc1**).
- De on-premises machines in het Contoso-datacenter wordt buiten gebruik gesteld nadat de migratie is voltooid.

![Scenario-architectuur](media/contoso-migration-rehost-vm-sql-managed-instance/architecture.png) 

### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Database Management-Service](https://docs.microsoft.com/azure/dms/dms-overview) | DMS kunt naadloze migratie van meerdere databasebronnen naar Azure-gegevensplatforms, met minimale downtime. | Meer informatie over [ondersteunde regio's](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) voor DMS en get [prijsinformatie](https://azure.microsoft.com/pricing/details/database-migration/).
[Azure SQL beheerd exemplaar](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | Managed Instance is een beheerde databaseservice die staat voor een volledig beheerde SQL Server-exemplaar in de Azure-cloud. Het dezelfde code deelt met de nieuwste versie van SQL Server Database Engine en heeft de nieuwste functies, prestatieverbeteringen en beveiligingspatches. | Met behulp van Azure SQL Database beheerde instanties uitgevoerd in Azure kosten in rekening op basis van capaciteit. [Meer informatie](https://azure.microsoft.com/pricing/details/sql-database/managed/). 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | De service coördineert en beheert de migratie en herstel na noodgevallen voor Azure-VM's, en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, rekening Azure Storage-kosten in gebracht.  Azure-VM's worden gemaakt en kosten, wanneer een failover optreedt. [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) kosten in rekening gebracht en prijzen.

 

## <a name="migration-process"></a>Migratieproces

Contoso wordt zowel de web- en gegevenslagen van de app SmartHotel migreren naar Azure.

1. Contoso heeft al de Azure-infrastructuur aanwezig is, zodat ze alleen wilt toevoegen van een aantal specifieke Azure-onderdelen voor dit scenario.
2. De gegevenslaag worden, gemigreerd met behulp van de Data Migration Service (DMS).  DMS maakt verbinding met de on-premises SQL Server-VM via een site-naar-site VPN-verbinding tussen de Contoso-datacenter en Azure, en worden vervolgens de database.
3. De weblaag worden, met behulp van een lift-and-shift-migratie met Azure Site Recovery gemigreerd. Dit zal leiden tot voorbereiden van de on-premises VMware-omgeving, instellen en inschakelen van replicatie en de virtuele machines met failover naar Azure migreren.

     ![Migratiearchitectuur](media/contoso-migration-rehost-vm-sql-managed-instance/migration-architecture.png) 


## <a name="prerequisites"></a>Vereisten

Hier volgt Contoso (en u) voor dit scenario moet.

**Vereisten** | **Details**
--- | ---
**Schrijf u in de Preview-versie** | U moet zijn geregistreerd bij de SQL beheerd exemplaar beperkte openbare Preview. U moet een Azure-abonnement om [aanmelden](https://portal.azure.com#create/Microsoft.SQLManagedInstance). Meld u aan een paar dagen om te voltooien, dus zorg ervoor dat u dit doen voordat u begint met het implementeren van dit scenario kan duren.
**Azure-abonnement** | U moet al een abonnement hebt gemaakt wanneer u de evaluatie uitgevoerd in de eerste artikel in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Site recovery (on-premises)** | Uw on-premises vCenter-server moet versie 5.5, 6.0 of 6.5 worden uitgevoerd<br/><br/> Een ESXi-host waarop versie 5.5, 6.0 of 6.5<br/><br/> Een of meer virtuele VMware-machines die worden uitgevoerd op de ESXi-host.<br/><br/> Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Ondersteund [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) configuratie.
**DMS** | Voor DMS moet u een [compatibel on-premises VPN-apparaat](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> U moet de on-premises VPN-apparaat configureren. Er moet een extern gericht openbaar IPv4-adres en het adres kan zich niet achter een NAT-apparaat.<br/><br/> Zorg ervoor dat u toegang hebt tot uw on-premises SQL Server-database.<br/><br/> De Windows Firewall moet toegang hebben tot de bron-database-engine. [Meer informatie](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Als er een firewall voor uw databasemachine, kunt u regels voor toegang tot de database, en bestanden via SMB-poort 445 toevoegen.<br/><br/> De referenties waarmee verbinding maken met de bron-SQL Server en beheerd exemplaar als doel, moeten lid zijn van de serverrol sysadmin.<br/><br/> U moet een netwerk delen in uw on-premises database die DMS gebruiken kunt om back-up van de brondatabase.<br/><br/> Zorg ervoor dat het serviceaccount waarop de SQL Server-bronexemplaar wordt uitgevoerd schrijfbevoegdheden op de netwerkshare heeft.<br/><br/> Maak een notitie van een Windows-gebruiker (en wachtwoord) die het volledige beheer op de netwerkshare heeft. Azure Database Migration Service imiteert de referenties van deze gebruiker voor het uploaden van back-upbestanden naar de Azure storage-container.<br/><br/> Het installatieproces van SQL Server Express wordt het TCP/IP-protocol ingesteld op **uitgeschakelde** standaard. Zorg ervoor dat deze ingeschakeld.


## <a name="scenario-steps"></a>Scenariostappen

Hier volgen enkele redenen hoe Contoso gaat voor het instellen van de implementatie:

> [!div class="checklist"]
> * **Stap 1: Een SQL Azure Managed Instance instellen**: ze nodig hebben een vooraf gemaakte beheerd exemplaar waarnaar de on-premises SQL Server-database wilt migreren.
> * **Stap 2: Bereid DMS**: ze nodig hebben voor de migratie van de Database-provider hebt geregistreerd, een exemplaar maken en vervolgens een DMS-project maken. Ze moeten ook SA-URI voor DMS instellen. Een shared access signature (SAS) Uniform Resource Identifier (URI) biedt gedelegeerde toegang tot bronnen in uw storage-account, zodat u machtigingen beperkt voor storage-objecten verlenen kunt. Ze een SAS-URI ingesteld zodat de DMS toegang heeft tot de opslagaccountcontainer waarop SQL Server back-up maken van bestanden voor de service worden geüpload.
> * **Stap 3: Azure voorbereiden voor Site Recovery**: voor Site Recovery, ze moeten maken van een Azure storage-account voor het opslaan van gerepliceerde gegevens, en een Recovery Services-kluis maken.
> * **Stap 4: On-premises VMware voorbereiden voor Site Recovery**: Contoso wordt accounts voor VM-detectie- en agent-installatie voorbereiden en voorbereidingen voor het verbinding maken met virtuele Azure-machines na een failover.
> * **Stap 5:-Machines repliceren**: als u replicatie instelt, ze de Site Recovery-bron en doel-omgeving configureren, instellen van een replicatiebeleid en beginnen met het repliceren van virtuele machines naar Azure storage.
> * **Stap 6: Migreren van de database met DMS**: nu ze de database kunnen migreren.
> * **Stap 7: Migreer de virtuele machines met Site Recovery**: ze voert een testfailover uit om te controleren of alles goed werkt, en voer een volledige failover voor de virtuele machines migreren naar Azure.


## <a name="step-1-prepare-an-azure-sql-managed-instance"></a>Stap 1: Een Azure SQL beheerd exemplaar voorbereiden

Als u een Azure SQL Managed Instance instelt, moet Contoso een subnet:

- Het subnet moet worden toegewezen. Moet leeg zijn en bevat een andere cloudservice en deze mag niet een gatewaysubnet.
- Nadat het beheerde exemplaar is gemaakt, kunt u resources kan niet aan toevoegen.
- Het subnet kan niet beschikken over een NSG die is gekoppeld.
- Het subnet moet een gebruiker Route tabel (UDR) met 0.0.0.0/0 Next Hop Internet hebben als de enige route die is toegewezen. 
- Optionele aangepaste DNS: als aangepaste DNS is opgegeven in het VNet, recursieve resolvers IP-adres (zoals 168.63.129.16) van Azure moet worden toegevoegd aan de lijst. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).
- Het subnet kan niet beschikken over een service-eindpunt dat is gekoppeld aan deze (storage of SQL). Service-eindpunten moeten worden uitgeschakeld op het virtuele netwerk.
- Het subnet moet minimaal 16 IP-adressen hebben. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#determine-the-size-of-subnet-for-managed-instances) over de grootte van het subnet van het beheerde exemplaar.
- In hun hybride omgeving, zijn aangepaste DNS-instellingen nodig. Contoso configureert DNS-instellingen voor het gebruik van een of meer van de Azure DNS-servers. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns) over DNS-aanpassingen.

### <a name="set-up-a-virtual-network-for-the-managed-instance"></a>Instellen van een virtueel netwerk voor het beheerde exemplaar

Contoso stelt als volgt u de VNet: 

1. Contoso maakt u een nieuw VNet (VNET-SQLMI-EU2) in de primaire regio VS-Oost 2, in de resourcegroep ContosoNetworkingRG.
2. Contoso wijst een adresruimte van 10.235.0.0/24, ervoor te zorgen dat het bereik geen overlap heeft met andere netwerken in de Contoso-onderneming.
2. Ze toevoegen twee subnetten aan het netwerk:
    - SQLMI-DS-EUS2 (10.235.0.0.25)
    - SQLMI-SAW-EUS2 (10.235.0.128/29). Dit subnet wordt directory koppelen aan het beheerde exemplaar (SQLMI) worden gebruikt.

    ![Beheerd exemplaar netwerk](media/contoso-migration-rehost-vm-sql-managed-instance/mi-vnet.png)

6. Nadat de VNet en subnetten worden geïmplementeerd, Contoso van collega's netwerken als volgt:

    - VNET-SQLMI-EUS2 van de peers met VNET-HUB-EUS2 (de hub VNet voor de VS-Oost 2).
    - VNET-SQLMI-EUS2 van de peers met VNET-PROD-EUS2 (het productienetwerk).

    ![Netwerkpeering](media/contoso-migration-rehost-vm-sql-managed-instance/mi-peering.png)

7. Contoso Hiermee stelt u aangepaste DNS-instellingen. DNS wordt eerst verwijzen naar hun Azure-DC's. Azure DNS wordt niet secundair zijn. De Contoso Azure DC's zich bevinden als volgt:

    - Netwerk (VNET-PROD-EUS2) zich in de productie-DC-EUS2-subnet bevinden, in de VS-Oost 2-productie.
    - CONTOSODC3 adres: 10.245.42.4
    - CONTOSODC4 adres: 10.245.42.5
    - Azure DNS-resolver: 168.63.129.16

     ![Netwerk DNS](media/contoso-migration-rehost-vm-sql-managed-instance/mi-dns.png)

**Meer hulp nodig?**

- [Bekijk een overzicht](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) van Azure SQL Managed Instances.
- [Meer informatie over](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#create-a-new-virtual-network-for-managed-instances) over het maken van een VNet voor SQL Managed Instance.
- [Meer informatie over](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering) peering instellen.
- [Meer informatie over](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-dns) Azure AD-DNS-instellingen worden bijgewerkt.



### <a name="set-up-routing"></a>Instellen van Routering

Het beheerde exemplaar wordt geplaatst in een persoonlijke VNET, dus Contoso moet een routetabel voor deze om te communiceren met de Azure Management-Service. Als deze niet kan met de service die wordt beheerd communiceren, wordt deze niet toegankelijk.

- De routetabel bevat een reeks regels (routes) waarmee wordt aangegeven hoe pakketten worden verzonden vanaf een beheerd exemplaar in het VNet moeten worden gerouteerd.
- De routetabel is gekoppeld aan subnetten waarin beheerde instanties zijn geïmplementeerd. Elk pakket een subnet verlaat wordt verwerkt op basis van de bijbehorende routetabel.
- Een subnet kan alleen worden gekoppeld aan één routetabel.
- Er zijn geen extra kosten voor het routetabellen maken in Microsoft Azure.

1. Contoso maakt een tabel van de gebruiker gedefinieerde route. De routetabel is gemaakt in de resourcegroep ContosoNetworkingRG.

    ![Routetabel](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table.png)

2. Om te voldoen aan vereisten voor beheerd exemplaar, nadat de routetabel (MIRouteTable) is geïmplementeerd, Contoso een route met het adresvoorvoegsel 0.0.0.0/0, toevoegen en **volgende hoptype** ingesteld op **Internet**.

    ![Voorvoegsel voor route-tabel](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-prefix.png)
    
3. De routetabel Contoso koppelen aan het subnet SQLMI-DB-EUS2 (in het netwerk VNET-SQLMI-EUS2). 

    ![Tabel subnet routeren](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-subnet.png)
    
**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal#create-new-route-table-and-a-route) instellen van routes voor een beheerd exemplaar.

### <a name="create-a-managed-instance"></a>Een beheerd exemplaar maken

Contoso kan nu een SQL Database Managed Instance inrichten.

1. Omdat het beheerde exemplaar fungeert een business-app, Contoso implementeren in hun primaire regio VS-Oost 2, in de resourcegroep ContosoRG 
2. Ze selecteren een prijzen-laag en grootte rekenen en opslag voor het exemplaar. [Meer informatie](https://azure.microsoft.com/pricing/details/sql-database/managed/) over prijzen.

    ![Beheerd exemplaar](media/contoso-migration-rehost-vm-sql-managed-instance/mi-create.png)

3. Nadat het beheerde exemplaar is geïmplementeerd, twee nieuwe resources worden weergegeven in de resourcegroep ContosoRG:

    - Een virtueel cluster als er meerdere beheerde exemplaren,
    - De SQL Server Managed Instance. 

    ![Beheerd exemplaar](media/contoso-migration-rehost-vm-sql-managed-instance/mi-resources.png)

**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal) inrichten van een beheerd exemplaar.

## <a name="step-2-prepare-dms"></a>Stap 2: Bereid DMS

Om voor te bereiden DMS, Contoso moet een aantal dingen doen:

- De DMS-provider registreren in Azure
- DMS voorzien van toegang tot Azure storage voor het uploaden van de back-upbestanden gebruikt voor het migreren van een database. Ze dit doen door het maken van een blob storage-container en een Shared Access Signature (SAS) URI voor het genereren. 
- Maak een project DMS.


Volg de stappen als volgt:

1. Contoso registreert de migratie van de Database-provider onder hun abonnement.
    ![DMS registreren](media/contoso-migration-rehost-vm-sql-managed-instance/dms-subscription.png)

2. Ze een opslag-blob-container maken en een SAS-URI genereren zodat deze toegankelijk in de DMS.

    ![SAS-URI](media/contoso-migration-rehost-vm-sql-managed-instance/dms-sas.png)

3. Ten slotte maakt ze een DMS-exemplaar. 

    ![DMS-exemplaar](media/contoso-migration-rehost-vm-sql-managed-instance/dms-instance.png)

4. Contoso plaatst de DMS-exemplaar in de productie-DC-EUS2 subnet van de VNET-PROD-DC-EUS2 VNet.
    - Ze plaatsen er omdat het moet zich binnen een VNet dat toegang heeft tot de on-premises SQL Server-VM via een VPN-gateway.
    - VNET-PROD-EUS2 is gekoppeld aan VNET-HUB-EUS2, en is toegestaan op het externe gateways gebruiken.  Dit zorgt ervoor dat de DMS behoefte kan communiceren.

        ![DMS-netwerk](media/contoso-migration-rehost-vm-sql-managed-instance/dms-network.png)

**Meer hulp nodig?**
- [Meer informatie over](https://docs.microsoft.com/azure/dms/quickstart-create-data-migration-service-portal) instellen van DMS.
- [Meer informatie](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2) over het maken en met behulp van SAS.


## <a name="step-3-prepare-azure-for-the-site-recovery-service"></a>Stap 3: Azure voorbereiden voor de Site Recovery-service

Er zijn een aantal Azure-elementen die Contoso behoefte heeft aan om het instellen van Site Recovery voor migratie van de weblaag VM (WEBMV)

- Failover van een VNet waarin resources zich bevinden.
- Azure storage-account voor het opslaan van gerepliceerde gegevens. 
- Een Recovery Services-kluis in Azure.

Deze instellen Site Recovery als volgt:

1. Aangezien de virtuele machine een webfrontend naar de app SmartHotel is, mislukken deze Apps via de virtuele machine met hun bestaande productienetwerk (VNET-PROD-EUS2) en subnet (PROD-FE-EUS2), in de primaire regio in de VS-Oost 2. Ze ingesteld met dit netwerk wanneer ze [geïmplementeerd van de Azure-infrastructuur](contoso-migration-infrastructure.md)
2. Het maken van een Azure storage-account (contosovmsacc20180528). Ze gebruikt een account voor algemeen gebruik, met de standard-opslag en LRS-replicatie.

    ![Site Recovery-opslag](media/contoso-migration-rehost-vm-sql-managed-instance/asr-storage.png)

3. Met de netwerk- en storage-account in plaats kunt Contoso nu een kluis (ContosoMigrationVault) maken en plaats deze in de resourcegroep ContosoFailoverRG, in de primaire regio in de VS-Oost 2.

    ![Recovery Services-kluis](media/contoso-migration-rehost-vm-sql-managed-instance/asr-vault.png)

**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) instellen van Azure voor Site Recovery.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>Stap 4: On-premises VMware voorbereiden voor Site Recovery

Om voor te bereiden VMware voor Site Recovery, moet u dit is wat Contoso moet doen:

- Een account voorbereiden op de vCenter-server of vSphere ESXi-host voor het automatiseren van VM-detectie.
- Bereid een account waarmee de automatische installatie van de Mobility-service op VMware-VM's die u wilt repliceren.
- Bereid de on-premises VM's, zodat ze verbinding met Azure-VM's maken kunnen wanneer ze na een failover worden gemaakt.


### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Virtuele machines automatisch detecteren. Er is minimaal een alleen-lezen-account vereist.
- Replicatie, failover en failback orkestreren. U moet een account die bewerkingen zoals het maken en verwijderen van schijven en het inschakelen van virtuele machines kan worden uitgevoerd.

Contoso stelt als volgt u het account:

1. Hiermee maakt u een rol op vCenter-niveau.
2. De vereiste machtigingen aan die rol toegewezen.

**Meer hulp nodig?**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) maken en toewijzen van een rol voor automatische detectie.

### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op de VM die u wilt repliceren.

- Site Recovery kan een automatische push-installatie van dit onderdeel kunt doen wanneer u replicatie voor de virtuele machine inschakelt.
- Voor automatische push-installatie moet u een account voorbereiden waarmee Site Recovery wordt gebruikt voor toegang tot de virtuele machine.
- U geeft dit account bij het instellen van replicatie in de Azure-console.
- U moet een domein of lokale account met machtigingen om te installeren op de virtuele machine.

**Meer hulp nodig hebt**

[Meer informatie over](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) het maken van een account voor push-installatie van de Mobility-service.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Na een failover naar Azure wil Contoso kunnen verbinding maken met de gerepliceerde virtuele machines in Azure. Om dit te doen, is er een aantal dingen die ze moeten gaan doen op de on-premises VM, voordat ze de migratie worden uitgevoerd: 

1. Voor toegang via internet, schakelt u RDP in op de on-premises virtuele machine voordat de failover en zorg ervoor dat TCP en UDP-regels worden toegevoegd voor de **openbare** profiel, en dat RDP is toegestaan in **Windows Firewall**  >  **Apps toegestaan** voor alle profielen.
2. Voor toegang via hun site-naar-site-VPN, schakelt u RDP in op de on-premises computer en toestaan dat RDP in de **Windows Firewall** -> **toegestane apps en functies** voor **domein en Persoonlijke** netwerken.
3. Ze SAN-beleid van het besturingssysteem op de on-premises virtuele machine ingesteld **OnlineAll**.

Wanneer ze een failover uitvoert moeten ze bovendien controleert u het volgende:

- Er mogen geen Windows-updates in behandeling op de virtuele machine wanneer u een failover activeert. Als er, zich ze niet aanmelden bij de virtuele machine totdat de update is voltooid.
- Na een failover, moeten ze controleren **diagnostische gegevens over opstarten** om een schermopname van de virtuele machine weer te geven. Als dit niet werkt, moeten ze controleren dat de virtuele machine wordt uitgevoerd en bekijk deze [tips voor probleemoplossing](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>Stap 5: De on-premises VM's repliceren naar Azure met Site Recovery

Voordat u een migratie naar Azure uitvoert, moet Contoso instellen van replicatie en replicatie inschakelen voor hun on-premises virtuele machine.

### <a name="set-a-replication-goal"></a>Een replicatiedoel instellen

1. In de kluis onder de kluisnaam (ContosoVMVault) ze ingesteld een replicatiedoel (**aan de slag** > **siteherstel** > **infrastructuur voorbereiden**.
2. Ze opgeven dat hun computers zich op locatie bevinden, dat ze virtuele VMware-machines, en dat ze wilt repliceren naar Azure.

    ![Replicatiedoel](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

Als u wilt doorgaan, ze nodig hebben om te bevestigen dat ze hebben de implementatieplanning uitgevoerd, door te selecteren **Ja, heb ik gedaan**. In deze implementatie Contoso zijn slechts één virtuele machine wordt gemigreerd, en hoeft-implementatie plannen.

### <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Nu moeten ze configureren hun bronomgeving. Om dit te doen ze een OVF-sjabloon downloaden en gebruiken voor het implementeren van de configuratieserver en de bijbehorende onderdelen als een maximaal beschikbaar, on-premises VMware-VM. Onderdelen op de server zijn onder andere:

- Failover wordt niet automatisch uitgevoerd.
- Initiëren van failover met één klik in de portal of kunt u  PowerShell dat een failover wordt geactiveerd. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag.
- De processerver installeert ook Mobility Service op VM's die u wilt repliceren, en detecteert automatisch on-premises VMware-VM's.
- Na de configuratieserver VM is gemaakt en gestart, kunt Contoso registreren in de kluis.


Contoso uitvoeren als volgt te werk:

1. Ze de OVF-sjabloon downloaden via de Azure portal (**infrastructuur voorbereiden** > **bron** > **configuratieserver**).
    
    ![OVF downloaden](./media/contoso-migration-rehost-vm-sql-managed-instance/add-cs.png)

2. Ze Importeer de sjabloon in VMware om te maken en implementeren van de virtuele machine.

    ![OVF-sjabloon](./media/contoso-migration-rehost-vm-sql-managed-instance/vcenter-wizard.png)

3.  Wanneer ze de virtuele machine voor het eerst inschakelen, deze wordt opgestart in een Windows Server 2016-installatie-ervaring. Ze accepteer de gebruiksrechtovereenkomst en voer een administrator-wachtwoord.
4. Nadat de installatie is voltooid, ze zich aanmelden bij de virtuele machine als de beheerder. Bij het eerst aanmelden, wordt het configuratieprogramma van Azure Site Recovery voert standaard.
5. In het hulpprogramma Geef ze een naam te gebruiken bij het registreren van de configuratieserver in de kluis.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, selecteren ze **aanmelden**, zich aanmelden bij de Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin de configuratieserver wordt geregistreerd. 

    [Configuratieserver registreren](./media/contoso-migration-rehost-vm-sql-managed-instance/config-server-register2.png)

7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op. Ze zich aanmelden bij de machine opnieuw en de configuratiewizard voor Server Management wordt automatisch gestart.
8. In de wizard selecteert ze de NIC die replicatieverkeer moet ontvangen. Deze instelling kan niet worden gewijzigd nadat deze geconfigureerd.
9. Ze selecteert u het abonnement, resourcegroep en kluis waarin de configuratieserver registreren.
        ![Kluis](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz1.png) 

10. Ze vervolgens downloaden en MySQL-Server en VMWare PowerCLI installeren. Vervolgens, ze de serverinstellingen te valideren.
11. Na de validatie Geef ze de FQDN-naam of IP-adres van de vCenter-server of vSphere-host. Ze laat de standaardpoort, en geef een beschrijvende naam voor de vCenter-server in Azure.
12. Ze moeten het account opgeven dat ze eerder hebt gemaakt, zodat Site Recovery kan automatisch detecteren van VMware-VM's die beschikbaar voor replicatie zijn. 
13. Deze geeft u de referenties voor het automatisch installeren van de Mobility-Service wanneer replicatie is ingeschakeld. Voor Windows-machines moet het account lokale administrator-bevoegdheden op de virtuele machines. 

    ![vCenter](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz2.png)

7. Nadat de registratie is voltooid, wordt in de Azure-portal, Contoso dubbele controleert dat de configuratieserver en de VMware-server worden weergegeven op de **bron** pagina in de kluis. Detectie kan 15 minuten of langer duren. 
8. Site Recovery wordt vervolgens verbinding maakt met de VMware-servers met de opgegeven instellingen en VM's worden gedetecteerd.

### <a name="set-up-the-target"></a>Het doel instellen

Contoso moet nu de replicatiedoelomgeving configureren.

1. In **infrastructuur voorbereiden** > **doel**, ze de doelinstellingen selecteren.
2. Site Recovery controleert of er een Azure storage-account en het netwerk in het opgegeven doel is.

### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

Nadat de bron en het doel zijn ingesteld, is Contoso gereed voor een replicatiebeleid maken, en koppel deze aan de configuratieserver.

1. In **infrastructuur voorbereiden** > **replicatie-instellingen** > **replicatiebeleid** >  **maken en Koppelen**, maken van een beleid **ContosoMigrationPolicy**.
2. Ze gebruiken de standaard-instellingen:
    - **RPO-drempelwaarde**: standaardwaarde van 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
    - **Bewaarperiode voor herstelpunten**. De standaardwaarde van 24 uur. Deze waarde wordt bepaald hoe lang de bewaarperiode is voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
    - **Frequentie App-consistente momentopname**. De standaardwaarde van één uur. Deze waarde bepaalt de frequentie waarmee App-consistente momentopnamen worden gemaakt.
 
        ![Replicatiebeleid maken](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy.png)

5. Het beleid wordt automatisch gekoppeld aan de configuratieserver. 

    ![Het replicatiebeleid koppelen](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy2.png)


**Meer hulp nodig?**

- U kunt een overzicht van al deze stappen in lezen [herstel na noodgevallen instellen voor on-premises VMware-machines](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Gedetailleerde instructies zijn beschikbaar om u te helpen [de bronomgeving instellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [de configuratieserver implementeren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), en [replicatie-instellingen configureren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).

### <a name="enable-replication"></a>Replicatie inschakelen

Nu kunt Contoso beginnen met de WebVM repliceren.

1. In **toepassing repliceren** > **bron** > **+ repliceren** ze de instellingen voor gegevensbron selecteren.
2. Ze geven aan dat ze willen virtuele machines inschakelen, selecteert u de vCenter-server en de configuratieserver.

 ![Replicatie inschakelen](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication1.png)
 
3. Ze geven nu de doelinstellingen, met inbegrip van de resourcegroep en netwerk waarin de Azure-VM na failover geplaatst worden wordt en het opslagaccount waarin de gerepliceerde gegevens worden opgeslagen.

     ![Replicatie inschakelen](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication2.png)

4. Contoso selecteert de WebVM voor replicatie. Site Recovery installeert de Mobility-Service op elke virtuele machine wanneer u replicatie inschakelt. 

    ![Replicatie inschakelen](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication3.png)

5. Contoso controleert die het juiste replicatiebeleid is geselecteerd en schakelt replicatie voor WEBVM. Volgen ze Replicatievoortgang gedurende **taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.
6. In **Essentials** in Azure portal, Contoso de structuur kunt bekijken voor de virtuele machines repliceren naar Azure.

    ![Infrastructuurweergave](./media/contoso-migration-rehost-vm-sql-managed-instance/essentials.png)


**Meer hulp nodig?**

U kunt een overzicht van al deze stappen in lezen [inschakelen replicatie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).

## <a name="step-6-migrate-the-database-with-dms"></a>Stap 6: Migreren van de database met DMS

Contoso moet een DMS-project maken en migreren van de database.

### <a name="create-a-dms-project"></a>Een DMS-project maken
1. Ze maken een DMS-project. Het brontype van de server worden opgegeven als SQL Server en het doel als Azure SQL Database Managed Instance.

     ![DMS-project](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-project.png)

2. Nadat het project is gemaakt, de Wizard wordt geopend.

### <a name="migrate-the-database"></a>Migreren van de database 

1. Hiermee geeft u de bron-VM waarop de on-premises database zich bevindt en referenties voor toegang tot deze in de Wizard migratie op Contoso.

    ![DMS-bron](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source.png)

2. Ze selecteert u de database te migreren (SmartHotel.Registration).

    ![DMS-brondatabase](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-sourcedb.png)

3. Als doel Geef deze de naam van het beheerde exemplaar in Azure en referenties voor toegang.

    ![DMS-doel](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-target-details.png)

4. Klik op **+ nieuwe activiteit** > **migratie uitvoeren**, ze de instellingen voor het uitvoeren van de migratie opgeven:
    - Referenties voor de bron en het doel.
    - De database te migreren.
    - De netwerkshare die ze gemaakt op de on-premises VM. DMS gaat bron back-ups naar deze share.
        - De serviceaccount met de bron-SQL Server exemplaar moet hebben schrijfrechten op deze share.
        - Geef de FQDN-pad naar de share.
    - De SAS-URI die de DMS biedt toegang tot de container van het opslagaccount waarnaar de service wordt geüpload de back-upbestanden voor de migratie.

        ![DMS-instellingen](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-migration-settings.png)

5. Ze opslaan van de migratie en de App uitvoeren.
6. In **overzicht**, controleren ze de migratiestatus van de.

    ![DMS-monitor](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor1.png)

7. Nadat de migratie is voltooid, wordt ze controleren dat de doeldatabase op het beheerde exemplaar bestaat.

    ![DMS-monitor](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor2.png)

## <a name="step-7-migrate-the-vm-with-site-recovery"></a>Stap 7: Migreer de virtuele machine met Site Recovery

Contoso-wordt uitgevoerd een snelle testfailover uit en vervolgens de virtuele machine te migreren.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Voordat u migreert WEBVM, een testfailover helpt Zorg ervoor dat alles werkt zoals verwacht. 

1. Ze een testfailover uitvoeren naar de laatst beschikbare punt in tijd (**laatst verwerkte**).
2. Ze selecteren **sluit de computer voordat u begint met failover**, zodat Site Recovery wordt geprobeerd de bron-VM afsluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. 
3. Test-failover wordt uitgevoerd: 

    - Een controle uitgevoerd om te controleren of alle van de voorwaarden die zijn vereist voor de migratie is voldaan.
    - De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als u het meest recente herstelpunt selecteert, wordt een herstelpunt van de gegevens gemaakt.
    - Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.
3. Nadat de failover is voltooid, wordt de replica virtuele machine in Azure weergegeven in de Azure-portal. Ze controleren of alles goed werkt. De virtuele machine is de juiste grootte heeft, verbonden met het juiste netwerk en deze wordt uitgevoerd. 
4. Nadat u hebt de testfailover gecontroleerd, Contoso ruimt de failover- en -records en eventuele opmerkingen worden opgeslagen. 

### <a name="migrate-the-vm"></a>De virtuele machine migreren

1. Contoso maakt nadat u hebt gecontroleerd of de testfailover heeft gewerkt zoals verwacht, een plan voor herstel voor migratie. Ze toevoegen WEBVM aan het abonnement.

     ![Plan voor herstel](./media/contoso-migration-rehost-vm-sql-managed-instance/recovery-plan.png)

2. Ze worden vervolgens een failover uitgevoerd op het plan. Ze het meest recente herstelpunt selecteren en opgeven dat Site Recovery proberen moet de on-premises VM afsluiten voordat de failover wordt geactiveerd.

    ![Failover](./media/contoso-migration-rehost-vm-sql-managed-instance/failover1.png)

3. Na de failover Contoso controleren of de Azure-VM wordt weergegeven zoals verwacht in de Azure-portal.

   ![Plan voor herstel](./media/contoso-migration-rehost-vm-sql-managed-instance/failover2.png)

4. Nadat u hebt de virtuele machine in Azure gecontroleerd, voltooien ze van de migratie voor het voltooien van het migratieproces, replicatie voor de virtuele machine stoppen en Site Recovery-facturering voor de virtuele machine stoppen.

    ![Failover](./media/contoso-migration-rehost-vm-sql-managed-instance/failover3.png)

### <a name="update-the-connection-string"></a>De verbindingsreeks bijwerken

Contoso bijgewerkt als de laatste stap in het migratieproces, de verbindingsreeks van de toepassing om te verwijzen naar de gemigreerde database die wordt uitgevoerd op de SQL-MI.

1. In de Azure-portal, ze de connection string vinden door te klikken op **instellingen** > **verbindingsreeksen**.

    ![Failover](./media/contoso-migration-rehost-vm-sql-managed-instance/failover4.png)  

2. De tekenreeks worden bijgewerkt met de gebruikersnaam en het wachtwoord van het beheerde exemplaar van SQL.
3. Nadat de tekenreeks die is geconfigureerd, vervangt u de huidige verbindingsreeks in het bestand web.config van hun toepassing.
4. Na het bijwerken van het bestand en op te slaan, ze IIS opnieuw starten op WEBVM. Ze doen dit met **IISRESET /RESTART** vanaf een opdrachtprompt.
5. Nadat IIS opnieuw is gestart, wordt de app gebruikt de database die wordt uitgevoerd op de SQL Managed Instance.
6. Op dit moment Contoso de SQLVM machine on-premises kunt afsluiten en de migratie is voltooid.

**Meer hulp nodig?**

- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) waarop een test-failover wordt uitgevoerd. 
- [Informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) over het maken van een plan voor herstel.
- [Meer informatie over](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Failover-overschakeling uitvoeren naar Azure.

## <a name="clean-up-after-migration"></a>Na de migratie opschonen

Met de migratie is voltooid, de SmartHotel-app wordt uitgevoerd op een Azure VM en de database SmartHotel is beschikbaar op de Azure SQL Managed Instance.  

Contoso moet nu enkele opschonen van de volgende handelingen uit:  

- Verwijder de WEBVM-machine uit de vCenter-inventaris.
- Verwijder de SQLVM-machine uit de vCenter-inventaris.
- WEBVM en SQLVM verwijderen uit lokale back-uptaken.
- De nieuwe locatie, IP-adres voor WEBVM update interne documentatie weergeven
- SQLVM verwijderen uit de documentatie bij. U kunt ook kunnen ze Markeer deze als u wilt weergeven als verwijderd en niet meer in de virtuele machine inventariseren.
- Alle resources die interactie met de uit bedrijf genomen VM's hebben controleren en bijwerken van de relevante instellingen of documentatie om de nieuwe configuratie weer te geven.

## <a name="review-the-deployment"></a>De implementatie controleren

Met de gemigreerde resources in Azure moet Contoso volledig operationeel maken en hun nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

Het Contoso security team controleert de Azure-VM's en SQL Managed Instance, om beveiligingsproblemen met met de uitvoering ervan te bepalen.

- Ze controleren de Netwerkbeveiligingsgroepen (nsg's) voor de virtuele machine, om toegang te beheren. Nsg's worden gebruikt om ervoor te zorgen dat alleen verkeer dat is toegestaan naar de app kan worden doorgegeven.
- Ze zijn ook overwegen de gegevens op de schijf met behulp van Azure Disk Encryption en Azure Key Vault te beveiligen.
- Ze inschakelen detectie van bedreigingen op SQL-beheerd exemplaar (SQLMI). Ze stuurt waarschuwingen naar hun system security team/service helpdesk een ticket openen als een bedreiging wordt gedetecteerd. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-threat-detection).

     ![Beveiliging voor beheerd exemplaar](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-security.png)  

[Lees meer](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) over procedures voor beveiliging voor virtuele machines.

### <a name="backups"></a>Back-ups
Contoso gaat naar de back-up van de gegevens op WEBVM met behulp van de Azure Backup-service. [Meer informatie](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

1. Contoso heeft een bestaande licentie voor WEBVM en gebruik van Azure Hybrid Benefit.  Ze zullen de bestaande VM in Azure om te profiteren van deze prijzen converteren.
2. Contoso kunnen Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Het is een kostenbeheeroplossing met meerdere Clouds management-oplossing waarmee u kunt gebruiken en beheren van Azure en andere cloudresources.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over Azure Cost Management. 


## <a name="conclusion"></a>Conclusie

In dit artikel rehosted Contoso de SmartHotel-app in Azure door het app-front-end virtuele machine migreren naar Azure met behulp van de Site Recovery-service. Ze de on-premises database gemigreerd naar een Azure SQL Managed Instance met behulp van DMS.

## <a name="next-steps"></a>Volgende stappen

In de [volgend artikel](contoso-migration-rehost-vm.md) in de reeks, laten we zien hoe Contoso naamconflicten voor de app SmartHotel op Azure Virtual machines met behulp van de Azure Site Recovery-service.

