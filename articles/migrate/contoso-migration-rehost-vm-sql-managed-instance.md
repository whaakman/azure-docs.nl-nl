---
title: Een on-premises Contoso app rehost door te migreren naar Azure VM's en Azure SQL Database Managed Instance | Microsoft Docs
description: Meer informatie over hoe Contoso naamconflicten met een on-premises app op Azure Virtual machines en met behulp van Azure SQL Database Managed Instance.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: 3e3f8dffbaa7109423aacdbfbaa658bada8bb84a
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215336"
---
# <a name="contoso-migration-rehost-an-on-premises-app-on-an-azure-vm-and-sql-database-managed-instance"></a>Migratie van Contoso: een on-premises-app op een virtuele machine van Azure en SQL Database Managed Instance Rehost

In dit artikel, Contoso zijn app SmartHotel migreert front-end virtuele machine met een Azure-VM met behulp van de Azure Site Recovery-service. Contoso wordt ook de app-database migreert naar Azure SQL Database Managed Instance.

> [!NOTE]
> Azure SQL Database Managed Instance is momenteel in preview.

In dit artikel is een in een reeks artikelen die documenten hoe het fictieve bedrijf Contoso de on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en een aantal scenario's die laten zien hoe u een migratie-infrastructuur instellen en uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit. Artikelen wordt toegevoegd aan de reeks na verloop van tijd.


Artikel | Details | Status
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Overzicht van de strategie voor de migratie van Contoso, de artikel-serie en de voorbeeld-apps die worden gebruikt in de reeks. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Contoso bereidt u de on-premises infrastructuur en de Azure-infrastructuur voor migratie. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen in de reeks. | Beschikbaar
[Artikel 3: Evalueer on-premises bronnen voor migratie naar Azure](contoso-migration-assessment.md) | Contoso wordt uitgevoerd een evaluatie van de on-premises twee lagen SmartHotel app die wordt uitgevoerd op VMware. Contoso beoordeelt virtuele machines van app met behulp van de [Azure Migrate](migrate-overview.md) service. Contoso beoordeelt de app SQL Server-database met behulp van [Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
Artikel 4: Opnieuw hosten van een app op een Azure-VM en het beheerde exemplaar van SQL Database | Contoso wordt een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel app uitgevoerd. Contoso worden gemigreerd van de app front-end virtuele machine met behulp van [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso de app-database migreert naar een Azure SQL Database Managed Instance met behulp van de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | In dit artikel
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | Contoso migreert zijn app SmartHotel virtuele machines naar Azure-VM's met behulp van de Site Recovery-service. | Beschikbaar
[Artikel 6: Een app op Azure Virtual machines en in een SQL Server AlwaysOn-beschikbaarheidsgroep opnieuw hosten](contoso-migration-rehost-vm-sql-ag.md) | De app SmartHotel, Contoso worden gemigreerd. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app. De Database Migration Service wordt gebruikt voor het migreren van de app-database naar een SQL Server-cluster dat wordt beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Contoso is een lift-and-shift-migratie van de app Linux osTicket virtuele Azure-machines met behulp van Site Recovery voltooid. | Beschikbaar
[Artikel 8: Een Linux-app op Azure VM's en Azure Database for MySQL Rehost](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migreert de Linux-osTicket-app naar Azure-VM's met behulp van Site Recovery. Deze migreert de app-database naar Azure Database for MySQL via MySQL Workbench. | Beschikbaar
[Artikel 9: Een app in een Azure-web-app en Azure SQL Database herstructureren](contoso-migration-refactor-web-app-sql.md) | Contoso zijn app SmartHotel migreert naar een Azure-web-app en de app-database migreert naar een Azure SQL Server-exemplaar. | Beschikbaar
[Artikel 10: Een Linux-app in een Azure-web-app en de Azure Database for MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | De Linux-app osTicket migreert Contoso naar een Azure-web-app op meerdere sites. De web-app is geïntegreerd met GitHub voor continue levering. Contoso migreert de app-database naar een Azure Database for MySQL-exemplaar. | Beschikbaar
[Artikel 11: Herstructureren Team Foundation Server in Visual Studio teamservices](contoso-migration-tfs-vsts.md) | Contoso worden gemigreerd van de on-premises Team Foundation Server-implementatie door te migreren naar Visual Studio Team Services in Azure. | Beschikbaar
[Artikel 12: Opnieuw ontwerpen van een app in Azure-containers en Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Contoso zijn app SmartHotel is gemigreerd naar Azure en rearchitects vervolgens de app. Contoso-rearchitects van de laag van de web-app als een Windows-container en rearchitects van de app-database met behulp van Azure SQL Database. | Beschikbaar
[Artikel 13: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Contoso wordt opnieuw gemaakt zijn SmartHotel-app met een scala aan mogelijkheden van Azure en services, waaronder Azure App Service, Azure Kubernetes Service, Azure Functions, Azure Cognitive Services en Azure Cosmos DB. | Beschikbaar

U kunt de voorbeeldapp SmartHotel die wordt gebruikt in dit artikel downloaden [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="on-premises-architecture"></a>On-premises-architectuur


In dit diagram ziet u de huidige on-premises infrastructuur voor Contoso:

![Huidige Contoso-architectuur](./media/contoso-migration-rehost-vm-sql-managed-instance/contoso-architecture.png)  

- Contoso heeft een belangrijkste datacenter. Het datacenter bevindt zich in de plaats van New York in het oostelijk deel van de Verenigde Staten.
- Contoso heeft drie extra lokale branches in de Verenigde Staten.
- Het belangrijkste datacenter is verbonden met internet met een fiber Metro Ethernet-verbinding (500 MBps).
- Elke vertakking is lokaal verbonden met het internet met behulp van kwalitatief hoogwaardige verbindingen met IPsec VPN-tunnels terug naar het primaire datacenter. De installatie, kan de hele netwerk van Contoso permanent zijn verbonden en optimaliseert de verbinding met internet.
- Het belangrijkste datacenter is volledig gevirtualiseerd met VMware. Contoso heeft twee ESXi 6.5-virtualisatiehosts die worden beheerd door vCenter Server 6.5.
- Contoso maakt gebruik van Active Directory voor identiteitsbeheer. Contoso maakt gebruik van DNS-servers in het interne netwerk.
- De domeincontrollers in het datacenter worden uitgevoerd op virtuele VMware-machines. De domeincontrollers op lokale branches uitgevoerd op fysieke servers.

## <a name="business-drivers"></a>Zakelijke drijfveren

Het Contoso IT de leiding heeft nauw samengewerkt met zakelijke partners van het bedrijf om te begrijpen wat het bedrijf wil bereiken met deze migratie:

- **Adres van de groei van het bedrijf**: Contoso groeit. Als gevolg toegenomen druk op de on-premises systemen en de infrastructuur van het bedrijf.
- **De efficiëntie verhogen**: Contoso nodig heeft om te verwijderen van onnodige procedures en processen stroomlijnen voor de ontwikkelaars en gebruikers. De zakelijke behoeften IT zijn snel en om niet afval tijd of geld, dus het bedrijf kunnen sneller leveren op eisen van klanten.
- **De veerkracht vergroten**: Contoso IT moet sneller te reageren op de behoeften van het bedrijf. Het moet mogelijk sneller dan de wijzigingen die plaatsvinden in de marketplace voor het bedrijf om in een globale economie succesvol te kunnen reageren. IT bij Contoso moet niet ophalen in de manier waarop of een zakelijke upblokkering worden.
- **Schaal**: als van het bedrijf met succes groeit, Contoso IT-systemen die hetzelfde tempo kunnen groeien moet opgeven.

## <a name="migration-goals"></a>Migratie-doelen

Het Contoso-cloud-team heeft de doelstellingen voor deze migratie geïdentificeerd. Doelstellingen van de migratie van het bedrijf gebruikt om te bepalen van de beste migratiemethode.

- Na de migratie, moet de app in Azure hebben dezelfde als de prestatiemogelijkheden die de app vandaag in on-premises VMWare-omgeving van Contoso heeft. Verplaatsen naar de cloud betekent niet dat de app-prestaties minder kritiek is.
- Contoso wil niet investeren in de app. De app is essentiële en belangrijk voor het bedrijf, maar Contoso gewoon wil de app in de huidige vorm overstappen naar de cloud.
- Beheertaken voor de database moeten worden geminimaliseerd nadat de app is gemigreerd.
- Contoso wilt niet dat met een Azure SQL Database voor deze app. Er op zoek naar alternatieven.

## <a name="proposed-architecture"></a>Voorgestelde architectuur

In dit scenario geldt het volgende:

- Contoso wil de twee lagen on-premises reis-app migreren.
- De app is gelaagd over twee virtuele machines (**WEBVM** en **SQLVM**) en bevindt zich op een VMware ESXi-host met versie 6.5 (**contosohost1.contoso.com**). 
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) die worden uitgevoerd op een virtuele machine.
- Contoso worden gemigreerd van de app-database (**SmartHotelDB**) naar een Azure SQL Database Managed Instance.
- Contoso migreert de on-premises VMware-machines naar een Azure-VM.
- Contoso heeft een on-premises datacenter (**contoso-datacenter**) en een on-premises domeincontroller (**contosodc1**).
- De on-premises machines in het Contoso-datacenter wordt gesteld wanneer de migratie is voltooid.

![Scenario-architectuur](media/contoso-migration-rehost-vm-sql-managed-instance/architecture.png) 

### <a name="azure-services"></a>Azure-services

Service | Beschrijving | Kosten
--- | --- | ---
[Database Management-Service](https://docs.microsoft.com/azure/dms/dms-overview) | De Database-Service kunt naadloze migratie van meerdere databasebronnen naar Azure data-platforms met minimale downtime. | Meer informatie over [ondersteunde regio's](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) en [Database Management-Service-prijzen](https://azure.microsoft.com/pricing/details/database-migration/).
[Beheerd exemplaar voor Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | Managed Instance is een beheerde databaseservice die staat voor een volledig beheerde SQL Server-exemplaar in de Azure-cloud. Het maakt gebruik van dezelfde code als de meest recente versie van SQL Server Database Engine en heeft de nieuwste functies, prestatieverbeteringen en beveiligingspatches. | Met behulp van een SQL Database Managed Instance die wordt uitgevoerd in Azure, worden kosten in rekening gebracht op basis van capaciteit. Meer informatie over [prijzen van Managed Instance](https://azure.microsoft.com/pricing/details/sql-database/managed/). 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | De service Site Recovery coördineert en beheert de migratie en herstel na noodgevallen voor Azure-VM's en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, rekening Azure Storage-kosten in gebracht.  Azure-VM's worden gemaakt en kosten in rekening gebracht wanneer een failover optreedt. Meer informatie over [Site Recovery-kosten en prijzen](https://azure.microsoft.com/pricing/details/site-recovery/).

 ## <a name="migration-process"></a>Migratieproces

Contoso wordt de web- en gegevenslagen van de app SmartHotel migreren naar Azure door deze stappen te voltooien:

1. Contoso heeft al de Azure-infrastructuur aanwezig is, zodat deze moet een aantal specifieke Azure-onderdelen voor dit scenario toevoegen.
2. De gegevenslaag worden, gemigreerd met behulp van de Data Migration Service. De Data Migration Service maakt verbinding met de on-premises SQL Server-VM via een site-naar-site VPN-verbinding tussen de Contoso-datacenter en Azure. De Data Migration Service migreert vervolgens de database.
3. De weblaag worden, met behulp van een lift-and-shift-migratie met behulp van Site Recovery gemigreerd. Het proces omvat het voorbereiden van de on-premises VMware-omgeving, instellen en inschakelen van replicatie en de virtuele machines met failover naar Azure migreren.

     ![Migratiearchitectuur](media/contoso-migration-rehost-vm-sql-managed-instance/migration-architecture.png) 

## <a name="prerequisites"></a>Vereisten

Contoso en andere gebruikers moeten voldoen aan de volgende vereisten voor dit scenario:

Vereisten | Details
--- | ---
**Schrijf u in de Managed Instance-preview** | U moet zijn geregistreerd bij de beperkte openbare preview voor SQL Database Managed Instance. U moet een Azure-abonnement [aanmelden](https://portal.azure.com#create/Microsoft.SQLManagedInstance). Registreren duurt een paar dagen om te voltooien, dus zorg ervoor dat u zich kunt registreren voordat u begint met het implementeren van dit scenario.
**Azure-abonnement** | U moet al een abonnement hebt gemaakt wanneer u de beoordeling in het eerste artikel in deze reeks uitvoeren. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder van het abonnement bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [op rollen gebaseerd toegangsbeheer gebruiken voor het beheren van Site Recovery toegang](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Site Recovery (on-premises)** | Uw on-premises vCenter Server-exemplaar moet versie 5.5, 6.0 of 6.5 worden uitgevoerd<br/><br/> Een ESXi-host waarop versie 5.5, 6.0 of 6.5<br/><br/> Een of meer virtuele VMware-machines die worden uitgevoerd op de ESXi-host.<br/><br/> Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Ondersteund [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) configuratie.
**Database Management-Service** | Voor de Database-Management-Service, moet u een [compatibel on-premises VPN-apparaat](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> U moet de on-premises VPN-apparaat configureren. Een extern gericht openbaar IPv4-adres moet hebben. Het adres kan zich niet achter een NAT-apparaat.<br/><br/> Zorg ervoor dat u toegang hebt tot uw on-premises SQL Server-database.<br/><br/> Windows Firewall moet toegang hebben tot de bron-database-engine. Meer informatie over het [Windows Firewall configureren voor toegang tot de Database-Engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Als er een firewall voor uw databasemachine, kunt u regels voor toegang tot de database en de bestanden via SMB-poort 445 toevoegen.<br/><br/> De referenties die worden gebruikt voor het verbinding maken met de SQL Server-bronexemplaar en die beheerd exemplaar als doel, moeten lid zijn van de serverrol sysadmin.<br/><br/> U moet een netwerk delen in uw on-premises database die de Database Management-Service gebruiken kunt om back-up van de brondatabase.<br/><br/> Zorg ervoor dat de serviceaccount met de SQL Server-bronexemplaar schrijfmachtigingen voor de netwerkshare heeft.<br/><br/> Maak een notitie van een Windows-gebruiker en wachtwoord waarmee machtigingen voor volledig beheer op de netwerkshare heeft. De Database Management-Service imiteert de referenties van deze gebruiker voor het uploaden van back-upbestanden naar de Azure Storage-container.<br/><br/> Het installatieproces van SQL Server Express wordt het TCP/IP-protocol ingesteld op **uitgeschakelde** standaard. Zorg ervoor dat deze ingeschakeld.

## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Contoso voor het instellen van de implementatie plannen:

> [!div class="checklist"]
> * **Stap 1: Een SQL Database Managed Instance instellen**: Contoso moet een vooraf gemaakte beheerd exemplaar waarnaar de on-premises SQL Server-database wilt migreren.
> * **Stap 2: Bereid de Database Management-Service**: Contoso moet de database migration-provider hebt geregistreerd, een exemplaar maken en vervolgens een Database Management-Service-project maken. Contoso moet ook instellen van een shared access signature (SAS) Uniform Resource Identifier (URI) voor de Database Management-Service. Een SAS-URI biedt gedelegeerde toegang tot resources in het Contoso-storage-account, zodat Contoso beperkte machtigingen voor opslagobjecten kunt toewijzen. Contoso stelt u een SAS-URI, waardoor de Database Management-Service toegang heeft tot de container van het opslagaccount waarnaar de service de SQL Server-back-upbestanden uploadt.
> * **Stap 3: Azure voorbereiden voor Site Recovery**: Contoso een storage-account voor het opslaan van gerepliceerde gegevens voor Site Recovery moet maken. Het moet ook een Azure Recovery Services-kluis maken.
> * **Stap 4: On-premises VMware voorbereiden voor Site Recovery**: Contoso-accounts voor VM-detectie- en agent-installatie verbinding maken met virtuele Azure-machines na een failover wordt voorbereid.
> * **Stap 5:-Machines repliceren**: als u replicatie instelt, Contoso configureren van de Site Recovery-bron en doel-omgevingen, stelt u een replicatiebeleid, en start virtuele machines te repliceren naar Azure Storage.
> * **Stap 6: Migreren van de database met behulp van de Database Management-Service**: Contoso worden gemigreerd van de database.
> * **Stap 7: De virtuele machines migreren met behulp van Site Recovery**: Contoso voert een testfailover uit om te controleren of alles werkt. Vervolgens wordt een volledige failover voor de virtuele machines migreren naar Azure uitgevoerd door Contoso.

## <a name="step-1-prepare-a-sql-database-managed-instance"></a>Stap 1: Een beheerd exemplaar van SQL Database voorbereiden

Als u een Azure SQL Database Managed Instance instelt, moet Contoso een subnet dat voldoet aan de volgende vereisten:

- Het subnet moet worden toegewezen. Deze moet leeg zijn en mag geen andere cloudservice. Het subnet kan niet een gatewaysubnet.
- Nadat het beheerde exemplaar is gemaakt, moet Contoso resources niet toevoegen aan het subnet.
- Het subnet kan niet een netwerkbeveiligingsgroep die is gekoppeld aan deze hebben.
- Het subnet moet een gebruiker gedefinieerde routering (UDR) routeringstabel hebben. De enige route toegewezen moet 0.0.0.0/0 van volgende hop internet. 
- Optionele aangepaste DNS: als aangepaste DNS is opgegeven in de Azure-netwerk, recursieve resolvers IP-adres (zoals 168.63.129.16) van Azure moet worden toegevoegd aan de lijst. Meer informatie over het [aangepaste DNS configureren voor een beheerd exemplaar](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).
- Het subnet kan niet beschikken over een service-eindpunt dat is gekoppeld aan deze (storage of SQL). Service-eindpunten moeten worden uitgeschakeld op het virtuele netwerk.
- Het subnet moet minimaal 16 IP-adressen hebben. Meer informatie over het [het formaat van de Managed Instance-subnet](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#determine-the-size-of-subnet-for-managed-instances).
- In de omgeving van Contoso hybride zijn aangepaste DNS-instellingen vereist. Contoso configureert de DNS-instellingen voor het gebruik van een of meer van de Azure DNS-servers van het bedrijf. Meer informatie over [DNS aanpassing](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).

### <a name="set-up-a-virtual-network-for-the-managed-instance"></a>Instellen van een virtueel netwerk voor het beheerde exemplaar

Contoso stelt als volgt u het virtuele netwerk: 

1. Contoso maakt een nieuw virtueel netwerk (**VNET-SQLMI-EU2**) in de primaire regio in de VS-Oost 2. Het virtuele netwerk wordt toegevoegd de **ContosoNetworkingRG** resourcegroep.
2. Contoso wijst een adresruimte van 10.235.0.0/24 toe. Contoso zorgt ervoor dat het bereik geen overlap heeft met andere netwerken in de onderneming.
2. Contoso voegt twee subnetten toe met het netwerk:
    - **SQLMI-DS-EUS2** (10.235.0.0.25)
    - **SQLMI-SAW-EUS2** (10.235.0.128/29). Dit subnet wordt gebruikt voor het koppelen van een map met het beheerde exemplaar.

    ![Managed Instance - virtueel netwerk maken](media/contoso-migration-rehost-vm-sql-managed-instance/mi-vnet.png)

6. Nadat het virtuele netwerk en subnetten worden geïmplementeerd, Contoso van collega's netwerken als volgt:

    - Peers **VNET-SQLMI-EUS2** met **VNET-HUB-EUS2** (het virtuele hub virtuele netwerk voor de VS-Oost 2).
    - Peers **VNET-SQLMI-EUS2** met **VNET-PROD-EUS2** (het productienetwerk).

    ![Netwerkpeering](media/contoso-migration-rehost-vm-sql-managed-instance/mi-peering.png)

7. Contoso Hiermee stelt u aangepaste DNS-instellingen. DNS verwijst eerst naar de Contoso Azure-domeincontrollers. Azure DNS is secundaire. De Contoso-Azure-domeincontrollers zich bevinden als volgt:

    - Bevindt zich in de **PROD-DC-EUS2** subnet in de VS-Oost 2-productienetwerk (**VNET-PROD-EUS2**)
    - **CONTOSODC3** adres: 10.245.42.4
    - **CONTOSODC4** adres: 10.245.42.5
    - Azure DNS-resolver: 168.63.129.16

     ![Netwerk DNS-servers](media/contoso-migration-rehost-vm-sql-managed-instance/mi-dns.png)

*Meer hulp nodig?*

- Bekijk een overzicht van [SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance).
- Meer informatie over het [een virtueel netwerk maken voor een SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#create-a-new-virtual-network-for-managed-instances).
- Meer informatie over het [peering instellen](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering).
- Meer informatie over het [Azure Active Directory DNS-instellingen bijwerken](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-dns).

### <a name="set-up-routing"></a>Instellen van Routering

Het beheerde exemplaar wordt geplaatst in een particulier virtueel netwerk. Contoso moet een routetabel voor het virtuele netwerk om te communiceren met de Azure Management-Service. Als het virtuele netwerk niet kunnen met de service die wordt beheerd communiceren, wordt het virtuele netwerk niet toegankelijk.

Contoso rekening gehouden met deze factoren:

- De routetabel bevat een set regels (routes) die specificeren hoe pakketten worden verzonden vanuit het beheerde exemplaar in het virtuele netwerk moeten worden gerouteerd.
- De routetabel is gekoppeld aan subnetten waarin beheerde instanties zijn geïmplementeerd. Elk pakket dat een subnet verlaat wordt verwerkt op basis van de bijbehorende routetabel.
- Een subnet kan worden gekoppeld aan slechts één routetabel.
- Er zijn geen extra kosten voor het routetabellen maken in Microsoft Azure.

 Om in te stellen routering:

1. Contoso maakt een UDR-tabel. Contoso maakt de routetabel in de **ContosoNetworkingRG** resourcegroep.

    ![Routetabel](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table.png)

2. Om te voldoen aan de Managed Instance, nadat de routetabel (**MIRouteTable**) is geïmplementeerd, Contoso een route met het adresvoorvoegsel 0.0.0.0/0 toevoegen. De **volgende hoptype** optie is ingesteld op **Internet**.

    ![Voorvoegsel voor route-tabel](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-prefix.png)
    
3. Contoso wordt gekoppeld aan de routetabel, met de **SQLMI-DB-EUS2** subnet (in de **VNET-SQLMI-EUS2** netwerk). 

    ![Tabel subnet routeren](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-subnet.png)
    
*Meer hulp nodig?*

Meer informatie over het [routes instellen voor een beheerd exemplaar](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal#create-new-route-table-and-a-route).

### <a name="create-a-managed-instance"></a>Een beheerd exemplaar maken

Contoso kan nu een SQL Database Managed Instance inrichten:

1. Omdat het beheerde exemplaar een business-app fungeert, implementeert u Contoso het beheerde exemplaar in de primaire VS-Oost 2-regio van het bedrijf. Contoso voegt het beheerde exemplaar op de **ContosoRG** resourcegroep.
2. Contoso selecteert een prijzen laag, grootte van berekening en opslag voor het exemplaar. Meer informatie over [prijzen van Managed Instance](https://azure.microsoft.com/pricing/details/sql-database/managed/).

    ![Beheerd exemplaar](media/contoso-migration-rehost-vm-sql-managed-instance/mi-create.png)

3. Nadat het beheerde exemplaar is geïmplementeerd, twee nieuwe resources worden weergegeven in de **ContosoRG** resourcegroep:

    - Een virtueel cluster in de aanvraag Contoso heeft meerdere beheerde exemplaren.
    - De SQL Server-Database beheerd exemplaar. 

    ![Beheerd exemplaar](media/contoso-migration-rehost-vm-sql-managed-instance/mi-resources.png)

*Meer hulp nodig?*

Meer informatie over het [inrichten van een beheerd exemplaar](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal).

## <a name="step-2-prepare-the-database-management-service"></a>Stap 2: Bereid de Database Management-Service

Als u wilt voorbereiden van de Database Management-Service, moet Contoso een paar dingen doen:

- De Database Management-Service-provider registreren in Azure.
- De Database Management-Service toegang geven tot Azure Storage voor het uploaden van de back-upbestanden die worden gebruikt voor het migreren van een database. Contoso maakt om toegang te bieden met Azure Storage, een Azure Blob storage-container. Contoso genereert een SAS-URI voor de Blob storage-container. 
- Maak een project-Database Management-Service.

Contoso voert vervolgens de volgende stappen uit:

1. Contoso registreert de database migration-provider onder het abonnement.
    ![Database Management-Service - Register](media/contoso-migration-rehost-vm-sql-managed-instance/dms-subscription.png)

2. Contoso maakt u een Blob storage-container. Contoso genereert een SAS-URI, zodat de Database Management-Service toegang kan hebben.

    ![Database-Management-Service: genereren van een SAS-URI](media/contoso-migration-rehost-vm-sql-managed-instance/dms-sas.png)

3. Contoso maakt een exemplaar van Database Management-Service. 

    ![Database-Management-Service - exemplaar maken](media/contoso-migration-rehost-vm-sql-managed-instance/dms-instance.png)

4. Contoso plaatst de Database Management Service-exemplaar in de **PROD-DC-EUS2** subnet van de **VNET-PROD-DC-EUS2** virtueel netwerk.
    - Contoso wordt de Database Management-Service er geplaatst omdat de service moet zich in een virtueel netwerk dat toegang heeft tot de on-premises SQL Server-VM via een VPN-gateway.
    - De **VNET-PROD-EUS2** is gekoppeld aan **VNET-HUB-EUS2** en mag externe gateways gebruiken. De **externe gateways gebruiken** optie zorgt ervoor dat de Database Management-Service als vereist communiceren kan.

        ![Management-Service-database - netwerk configureren](media/contoso-migration-rehost-vm-sql-managed-instance/dms-network.png)

*Meer hulp nodig?*

- Meer informatie over het [instellen van de Database Management-Service](https://docs.microsoft.com/azure/dms/quickstart-create-data-migration-service-portal).
- Meer informatie over het [maken en gebruiken van SAS](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2).


## <a name="step-3-prepare-azure-for-the-site-recovery-service"></a>Stap 3: Azure voorbereiden voor de Site Recovery-service

Verschillende Azure-elementen zijn vereist voor Contoso voor het instellen van Site Recovery voor migratie van de weblaag VM (**WEBMV**):

- Een virtueel netwerk waarin failover resources zich bevinden.
- Een opslagaccount voor gerepliceerde gegevens. 
- Een Recovery Services-kluis in Azure.

Contoso stelt als volgt u de Site Recovery:

1. Omdat de virtuele machine een webfront-end in de app SmartHotel, Contoso, failover van de virtuele machine om de bestaande productienetwerk (**VNET-PROD-EUS2**) en het subnet (**PROD-FE-EUS2**). Het netwerk en subnet bevinden zich in de primaire regio in de VS-Oost 2. Contoso instellen van het netwerk wanneer het [geïmplementeerd van de Azure-infrastructuur](contoso-migration-infrastructure.md).
2. Contoso maakt een storage-account (**contosovmsacc20180528**). Contoso maakt gebruik van een account voor algemeen gebruik. Contoso selecteert u standard-opslag en lokaal redundant storage-replicatie.

    ![Site Recovery - storage-account maken](media/contoso-migration-rehost-vm-sql-managed-instance/asr-storage.png)

3. Met de netwerk- en storage-account in plaats Contoso een kluis maakt (**ContosoMigrationVault**). Contoso plaatst de kluis in de **ContosoFailoverRG** resourcegroep in de primaire regio in de VS-Oost 2.

    ![Maken van de Recovery Services - kluis](media/contoso-migration-rehost-vm-sql-managed-instance/asr-vault.png)

*Meer hulp nodig?*

Meer informatie over het [Azure instellen voor Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure).


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>Stap 4: On-premises VMware voorbereiden voor Site Recovery

Om voor te bereiden VMware voor Site Recovery, moet de Contoso deze taken uitvoeren:

- Een account voorbereiden op de vCenter-Server-exemplaar of vSphere ESXi-host. Het account automatiseert VM-detectie.
- Een account waarmee de automatische installatie van de Mobility-Service op virtuele VMware-machines die Contoso wil repliceren voorbereiden.
- Bereid de on-premises machines verbinding maken met Azure-VM's wanneer ze na een failover worden gemaakt.


### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Virtuele machines automatisch detecteren. Minimaal een alleen-lezen-account is vereist.
- Replicatie, failover en failback orkestreren. Contoso moet een account die bewerkingen zoals het maken en verwijderen van schijven en inschakelen van virtuele machines kan worden uitgevoerd.

Contoso stelt u het account door deze taken uit te voeren:

1. Hiermee maakt u een rol op vCenter-niveau.
2. De vereiste machtigingen aan die rol toegewezen.

*Meer hulp nodig?*

Meer informatie over het [maken en toewijzen van een rol voor automatische detectie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery).

### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor de installatie van Mobility Service

De Mobility-Service moet worden geïnstalleerd op de virtuele machine die Contoso wil repliceren. Contoso rekening gehouden met deze factoren over de Mobility-Service:

- Site Recovery kan een automatische push-installatie van dit onderdeel kunt doen wanneer Contoso Hiermee schakelt u replicatie voor de virtuele machine.
- Voor automatische push-installatie, Contoso moet een account voorbereiden waarmee Site Recovery gebruikt voor toegang tot de virtuele machine.
- Contoso geeft dit account wanneer het programma replicatie in de Azure-console stelt.
- Contoso beschikken over een domein of lokale account met machtigingen om te installeren op de virtuele machine.

*Meer hulp nodig hebt*

Meer informatie over het [maken van een account voor push-installatie van de Mobility-Service](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation).

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Na een failover naar Azure wil Contoso kunnen verbinding maken met de gerepliceerde virtuele machines in Azure. Voor verbinding met de gerepliceerde virtuele machines in Azure, moet de Contoso een paar taken op de on-premises VM vóór de migratie voltooien: 

1. Voor toegang via internet kan Contoso RDP in op de on-premises virtuele machine voordat de failover. Contoso zorgt ervoor dat TCP en UDP-regels worden toegevoegd voor de **openbare** profiel, en dat RDP is toegestaan in **Windows Firewall** > **Apps toegestaan** voor alle profielen .
2. Voor toegang via het Contoso-site-naar-site VPN schakelt Contoso RDP in op de on-premises machine. Contoso kan RDP in **Windows Firewall** > **toegestane apps en functies** voor **domein en privé** netwerken.
3. Contoso Hiermee stelt u de SAN-beleid van het besturingssysteem op de on-premises virtuele machine om **OnlineAll**.

Contoso heeft ook nodig om te controleren of deze items wanneer er een failover wordt uitgevoerd:

- Er mogen geen Windows-updates in behandeling op de virtuele machine wanneer een failover wordt geactiveerd. Als Windows-updates in behandeling zijn, kan niet Contoso aanmelden bij de virtuele machine totdat de update is voltooid.
- Controleer na een failover, Contoso **diagnostische gegevens over opstarten** om een schermopname van de virtuele machine weer te geven. Als u Contoso kan de diagnostische gegevens over het opstarten niet weergeven, het moet inchecken dat de virtuele machine wordt uitgevoerd, en bekijk vervolgens [tips voor probleemoplossing](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

## <a name="step-5-replicate-the-on-premises-vms-to-azure-by-using-site-recovery"></a>Stap 5: De on-premises VM's repliceren naar Azure met behulp van Site Recovery

Voordat u een migratie naar Azure uitvoert, moet Contoso instellen van replicatie en replicatie inschakelen voor de on-premises virtuele machine.

### <a name="set-a-replication-goal"></a>Een replicatiedoel instellen

1. In de kluis, onder de kluisnaam (**ContosoVMVault**), Contoso Hiermee stelt u een replicatiedoel (**aan de slag** > **siteherstel**  >   **Infrastructuur voorbereiden**).
2. Contoso geeft aan dat de machines zich op locatie zijn, dat ze virtuele VMware-machines, en dat Contoso wil repliceren naar Azure.

    ![Infrastructuur - beveiligingsdoel voorbereiden](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

Als u wilt doorgaan, moet de Contoso om te bevestigen dat het plannen van de implementatie is voltooid. Als u wilt controleren, selecteert u Contoso **Ja, heb ik gedaan**. In deze implementatie is slechts één VM, migreren van Contoso, zodat deze niet met het plannen van de implementatie hoeft.

### <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Contoso moet nu het configureren van de bronomgeving. Als u wilt de bronomgeving instellen, downloadt Contoso een OVF-sjabloon. Contoso maakt gebruik van de sjabloon voor het implementeren van de configuratieserver en de bijbehorende onderdelen als een maximaal beschikbaar, on-premises VMware-VM. Onderdelen op de server zijn onder andere:

- De configuratieserver die coördineert de communicatie tussen de on-premises infrastructuur en Azure. De configuratieserver beheert de gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. De processerver:
    - Replicatiegegevens ontvangt.
    - Datum van de replicatie optimaliseert met behulp van caching, compressie en codering.
    - Datum van de replicatie verzendt naar Azure Storage.
- De processerver installeert ook de Mobility-Service op de virtuele machines die Contoso wil repliceren. De processerver detecteert automatisch on-premises VMware-VM's.
- Na de configuratieserver VM is gemaakt en gestart, wordt de server door Contoso geregistreerd in de kluis.

De bronomgeving instellen:

1. Contoso downloadt de OVF-sjabloon van de Azure-portal (**infrastructuur voorbereiden** > **bron** > **configuratieserver**).
    
    ![Een configuratieserver toevoegen](./media/contoso-migration-rehost-vm-sql-managed-instance/add-cs.png)

2. Contoso importeert de sjabloon in VMware maken en implementeren van de virtuele machine.

    ![Implementeren van OVF-sjabloon](./media/contoso-migration-rehost-vm-sql-managed-instance/vcenter-wizard.png)

3.  Wanneer Contoso de virtuele machine voor de eerste keer inschakelt, wordt de virtuele machine gestart in een Windows Server 2016-installatie-ervaring. Contoso accepteert de gebruiksrechtovereenkomst en voert een administrator-wachtwoord.
4. Wanneer de installatie is voltooid, Contoso meldt zich aan bij de virtuele machine als de beheerder. De eerste keer is dat Contoso zich aanmeldt bij de virtuele machine, de Azure Site Recovery Configuration Tool wordt automatisch uitgevoerd.
5. In de Site Recovery Configuration Tool voert Contoso een naam moet worden gebruikt voor het registreren van de configuratieserver in de kluis.
6. Het hulpprogramma controleert of de verbinding van de virtuele machine met Azure. Nadat de verbinding tot stand is gebracht, selecteert u Contoso **aanmelden** zich aanmeldt bij de Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin de configuratieserver is geregistreerd. 

    ![De configuratieserver registreren](./media/contoso-migration-rehost-vm-sql-managed-instance/config-server-register2.png)

7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op. Contoso meldt zich aan bij de machine opnieuw. De configuratiewizard voor Server Management wordt automatisch gestart.
8. In de wizard selecteert Contoso de NIC die replicatieverkeer moet ontvangen. Deze instelling kan niet worden gewijzigd nadat deze geconfigureerd.
9. Contoso selecteert het abonnement, resourcegroep en Recovery Services-kluis waarin de configuratieserver registreren.

    ![Recovery Services-kluis selecteren](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz1.png)

10. Contoso downloadt en installeert MySQL-Server en VMWare PowerCLI. Contoso evalueert vervolgens de server-instellingen.
11. Na de validatie krijgt de Contoso de FQDN-naam of IP-adres van de vCenter-Server-exemplaar of vSphere-host. Contoso verlaat de standaardpoort en voert een weergavenaam voor de vCenter-Server-exemplaar in Azure.
12. Contoso moet het account opgeven dat het eerder hebt gemaakt zodat Site Recovery kan automatisch detecteren van VMware-VM's die beschikbaar voor replicatie zijn. 
13. Contoso krijgt de referenties, zodat de Mobility-Service wordt automatisch geïnstalleerd wanneer replicatie is ingeschakeld. Voor Windows-machines moet het account lokale beheerdersmachtigingen op de virtuele machines. 

    ![VCenter-Server configureren](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz2.png)

7. Wanneer de registratie is voltooid, klikt u in Azure portal, Contoso opnieuw gecontroleerd dat de configuratieserver en de VMware-server worden weergegeven op de **bron** pagina in de kluis. Detectie kan 15 minuten of langer duren. 
8. Site Recovery maakt verbinding met de VMware-servers met behulp van de opgegeven instellingen. VM's worden gedetecteerd door site Recovery.

### <a name="set-up-the-target"></a>Het doel instellen

Contoso moet nu de replicatiedoelomgeving configureren:

1. In **infrastructuur voorbereiden** > **doel**, Contoso de doelinstellingen geselecteerd.
2. Site Recovery controleert of er een storage-account en het netwerk in het opgegeven doel is.

### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

Als de bron en het doel zijn ingesteld, wordt Contoso maakt een replicatiebeleid en koppelt u het beleid aan de configuratieserver:

1. In **infrastructuur voorbereiden** > **replicatie-instellingen** > **replicatiebeleid** >  **maken en Koppelen**, Contoso maakt de **ContosoMigrationPolicy** beleid.
2. Contoso maakt gebruik van de standaardinstellingen:
    - **RPO-drempelwaarde**: standaardwaarde van 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
    - **Bewaarperiode voor herstelpunten**: standaardwaarde van 24 uur. Deze waarde wordt bepaald hoe lang de bewaarperiode is voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
    - **Frequentie App-consistente momentopname**: de standaardwaarde van 1 uur. Deze waarde bepaalt de frequentie waarmee App-consistente momentopnamen worden gemaakt.
 
    ![Beleid voor wachtwoordreplicatie - maken](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy.png)

3. Het beleid wordt automatisch gekoppeld aan de configuratieserver. 

    ![Beleid voor wachtwoordreplicatie - koppelen](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy2.png)

*Meer hulp nodig?*

- U kunt een overzicht van de volgende stappen uit in lezen [herstel na noodgevallen instellen voor on-premises VMware-machines](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Gedetailleerde instructies zijn beschikbaar om u te helpen [de bronomgeving instellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [de configuratieserver implementeren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), en [replicatie-instellingen configureren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).

### <a name="enable-replication"></a>Replicatie inschakelen

Nu kunt Contoso beginnen met het repliceren WebVM.

1. In **toepassing repliceren** > **bron** > **repliceren**, Contoso selecteert u de instellingen voor gegevensbron.
2. Contoso geeft aan dat het wil virtuele machines inschakelen, selecteert u het vCenter Server-exemplaar en stelt de configuratieserver.

    ![Inschakelen van replicatie - bron](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication1.png)
 
3. Contoso geeft de doelinstellingen, met inbegrip van de resourcegroep en een netwerk waarin de Azure-VM na failover geplaatst worden wordt. Contoso Hiermee geeft u het opslagaccount waarin de gerepliceerde gegevens worden opgeslagen.

    ![Replicatie - doel inschakelen](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication2.png)

4. Contoso selecteert **WebVM** voor replicatie. Site Recovery installeert de Mobility-Service op elke virtuele machine wanneer replicatie is ingeschakeld. 

    ![Replicatie inschakelen: Selecteer de virtuele machine](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication3.png)

5. Contoso controleert of het juiste replicatiebeleid is geselecteerd. Er wordt vervolgens, replicatie voor **WEBVM**. Contoso houdt de voortgang van de replicatie in **taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.
6. In **Essentials** in Azure portal, Contoso de structuur kunt bekijken voor de virtuele machines die naar Azure repliceren:

    ![Infrastructuurweergave](./media/contoso-migration-rehost-vm-sql-managed-instance/essentials.png)

*Meer hulp nodig?*

U kunt een overzicht van de volgende stappen uit in lezen [inschakelen replicatie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).

## <a name="step-6-migrate-the-database-by-using-the-database-management-service"></a>Stap 6: Migreren van de database met behulp van de Database Management-Service

Contoso moet een Database Management-Service-project maken en vervolgens migreren van de database.

### <a name="create-a-database-management-service-project"></a>Een Database Management-Service-project maken

1. Contoso maakt een project-Database Management-Service. Contoso selecteert de **SQL Server** bronservertype. Contoso selecteert **Azure SQL Database Managed Instance** als het doel.

     ![Database Management-Service - nieuw migratieproject](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-project.png)

2. De Wizard wordt geopend.

### <a name="migrate-the-database"></a>Migreren van de database 

1. Hiermee geeft u de bron-VM waarop de on-premises database zich bevindt in de Wizard migratie op Contoso. Contoso voert de referenties voor toegang tot de database.

    ![Database Management-Service - details van gegevensbron](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source.png)

2. Contoso selecteert u de database te migreren (**SmartHotel.Registration**):

    ![Database Management-Service - databases van de bron selecteren](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-sourcedb.png)

3. Voor het doel krijgt de naam van het beheerde exemplaar de Contoso in Azure. Contoso krijgt de referenties voor toegang voor het beheerde exemplaar.

    ![Database Management-Service - Doeldetails](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-target-details.png)

4. In **nieuwe activiteit** > **migratie uitvoeren**, Contoso Hiermee geeft u de instellingen voor het uitvoeren van de migratie:
    - Referenties voor de bron en het doel.
    - De database te migreren.
    - De netwerkshare die Contoso gemaakt op de on-premises VM. De Database Management-Service gaat bron back-ups naar deze share. 
        - Het serviceaccount dat de SQL Server-bronexemplaar wordt uitgevoerd moet schrijfmachtigingen hebben op deze share.
        - De FQDN-pad naar de share moet worden gebruikt.
    - De SAS-URI die de Database Management-Service toegang biedt tot de container van het opslagaccount waarnaar de service wordt geüpload de back-upbestanden voor de migratie.

        ![Management-Service-database - migratie-instellingen configureren](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-migration-settings.png)

5. Contoso-Hiermee slaat u de migratie en vervolgens uitgevoerd.
6. In **overzicht**, Contoso bewaakt de migratiestatus.

    ![Database-Service Management - Monitor](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor1.png)

7. Wanneer de migratie is voltooid, controleert Contoso of dat de doeldatabase op het beheerde exemplaar bestaat.

    ![Database-Management-Service: de databasemigratie controleren](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor2.png)

## <a name="step-7-migrate-the-vm-by-using-site-recovery"></a>Stap 7: De virtuele machine migreren met behulp van Site Recovery

Contoso-wordt uitgevoerd een snelle testfailover uit en klikt u vervolgens de virtuele machine migreert.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Voordat u migreert WEBVM, een test-failover zorgt ervoor dat alles werkt zoals verwacht. Contoso is voltooid de volgende stappen uit:

1. Contoso wordt een testfailover uitgevoerd naar de laatst beschikbare punt in tijd (**laatst verwerkte**).
2. Contoso selecteert **sluit de computer voordat u begint met failover**. Met deze optie is geselecteerd, probeert Site Recovery de bron-VM wordt afgesloten voordat deze de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. 
3. Test-failover wordt uitgevoerd: 
    1. Een controle uitgevoerd om ervoor te zorgen dat de voorwaarden die zijn vereist voor de migratie voldaan is.
    2. De gegevens worden verwerkt in de failover, zodat een Azure-VM kan worden gemaakt. Als de meest recente herstelpunt is geselecteerd, wordt een herstelpunt wordt gemaakt van de gegevens.
    3.  Azure-VM is gemaakt met behulp van de gegevens verwerkt in de vorige stap.
3. Wanneer de failover is voltooid, wordt de replica virtuele machine in Azure weergegeven in de Azure-portal. Contoso wordt gecontroleerd of alles goed werkt: de virtuele machine is de juiste grootte heeft, verbonden met het juiste netwerk en deze wordt uitgevoerd. 
4. Nadat u hebt de testfailover gecontroleerd, opschonen Contoso van de failover. Vervolgens registreert en eventuele opmerkingen worden opgeslagen. 

### <a name="migrate-the-vm"></a>De virtuele machine migreren

1. Contoso maakt nadat u hebt gecontroleerd of de testfailover heeft gewerkt zoals verwacht, een plan voor herstel voor migratie. WEBVM Contoso toegevoegd aan het abonnement:

     ![Plan voor herstel - Select-items maken](./media/contoso-migration-rehost-vm-sql-managed-instance/recovery-plan.png)

2. Contoso wordt een failover uitgevoerd op het plan. Contoso selecteert u het meest recente herstelpunt. Contoso geeft aan dat Site Recovery proberen moet de on-premises VM afsluiten voordat deze de failover wordt geactiveerd.

    ![Failover](./media/contoso-migration-rehost-vm-sql-managed-instance/failover1.png)

3. Na de failover Contoso wordt gecontroleerd of de Azure-VM wordt weergegeven zoals verwacht in de Azure-portal.

   ![Abonnementsdetails voor herstel](./media/contoso-migration-rehost-vm-sql-managed-instance/failover2.png)

4. Na het verifiëren van de virtuele machine in Azure, wordt de migratie voor het voltooien van het migratieproces, replicatie voor de virtuele machine stoppen en Site Recovery-facturering voor de virtuele machine stoppen met Contoso voltooid.

    ![Failover - migratie voltooien](./media/contoso-migration-rehost-vm-sql-managed-instance/failover3.png)

### <a name="update-the-connection-string"></a>De verbindingsreeks bijwerken

Contoso bijgewerkt als de laatste stap in het migratieproces, de verbindingsreeks van de toepassing om te verwijzen naar de gemigreerde database die wordt uitgevoerd op de Contoso Managed Instance.

1. Contoso gevonden in de Azure-portal, de verbindingsreeks door te selecteren **instellingen** > **verbindingsreeksen**.

    ![Verbindingsreeksen](./media/contoso-migration-rehost-vm-sql-managed-instance/failover4.png)  

2. Contoso werkt de tekenreeks met de gebruikersnaam en het wachtwoord van de SQL Database Managed Instance.
3. Nadat de tekenreeks die is geconfigureerd, wordt de huidige verbindingsreeks in het bestand web.config van de toepassing vervangen door Contoso.
4. Na het bijwerken van het bestand en op te slaan, Contoso opnieuw wordt opgestart IIS op WEBVM. U Start IIS opnieuw, Contoso wordt uitgevoerd `IISRESET /RESTART` in een opdrachtpromptvenster.
5. Nadat IIS opnieuw is opgestart, wordt in de app maakt gebruik van de database die wordt uitgevoerd op de SQL Database Managed Instance.
6. Op dit moment kunt Contoso afsluiten on-premises de SQLVM-machine. De migratie is voltooid.

*Meer hulp nodig?*

- Meer informatie over het [voert een testfailover uit](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure). 
- Meer informatie over het [maken van een herstelplan](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans).
- Meer informatie over het [failover naar Azure](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover).

## <a name="clean-up-after-migration"></a>Na de migratie opschonen

Met de migratie is voltooid, de SmartHotel-app wordt uitgevoerd op een Azure VM en de database SmartHotel is beschikbaar in de Azure SQL Database Managed Instance.  

Contoso moet nu de volgende opschoningstaken uitvoeren:  

- Verwijder de WEBVM-machine uit de vCenter-Server-inventaris.
- Verwijder de SQLVM-machine uit de vCenter-Server-inventaris.
- WEBVM en SQLVM verwijderen uit lokale back-uptaken.
- Interne documentatie om weer te geven van de nieuwe locatie en het IP-adres voor WEBVM bijwerken.
- Verwijder SQLVM uit interne documentatie. Contoso kan ook de documentatie voor de SQLVM weergeven als verwijderd en niet meer in de virtuele machine inventariseren wijzigen.
- Controleer alle resources die met de uit bedrijf genomen virtuele machines communiceren. Alle relevante instellingen of documentatie aanleiding van de nieuwe configuratie bijwerken.

## <a name="review-the-deployment"></a>De implementatie controleren

Met de gemigreerde resources in Azure moet Contoso volledig operationeel maken en de nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

Het Contoso security team controleert de Azure-VM's en SQL Database Managed Instance om te controleren op beveiligingsproblemen met de implementatie:

- Het beveiligingsteam beoordeelt de netwerkbeveiligingsgroepen die worden gebruikt om toegang te beheren voor de virtuele machine. Netwerkbeveiliging groepen ervoor te zorgen dat alleen het verkeer dat is toegestaan naar de app kunt doorgeven.
- Het Contoso security team ook overweegt de gegevens op de schijf te beveiligen met behulp van Azure Disk Encryption en Azure Key Vault.
- Het security team kunt detectie van bedreigingen op het beheerde exemplaar. Detectie van bedreigingen verzendt een waarschuwing van Contoso security team/service desk systeem een ticket openen als een bedreiging wordt gedetecteerd. Meer informatie over [bedreigingen voor de Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-threat-detection).

     ![Beheerd exemplaar beveiliging - detectie van bedreigingen](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-security.png)  

Zie voor meer informatie over procedures voor beveiliging voor virtuele machines, [aanbevolen beveiligingsprocedures voor IaaS-workloads in Azure](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control).

### <a name="backups"></a>Back-ups

Contoso back-ups van de gegevens op WEBVM met behulp van de Azure Backup-service. Meer informatie over [Azure Backup](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

- Contoso heeft een bestaande licentie voor WEBVM. Als u wilt profiteren van prijzen met Azure Hybrid Benefit, converteert Contoso de bestaande VM in Azure.
- Contoso kan Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Kostenbeheer is een kostenbeheeroplossing met meerdere Clouds management-oplossing waarmee Contoso gebruiken en beheren van Azure en andere cloudresources. Meer informatie over [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview). 


## <a name="conclusion"></a>Conclusie

In dit artikel naamconflicten Contoso de SmartHotel-app in Azure door te migreren van de app front-end-VM naar Azure met behulp van de Site Recovery-service. Contoso migreert de on-premises database naar een Azure SQL Database Managed Instance met behulp van de Azure-Database Management-Service.

## <a name="next-steps"></a>Volgende stappen

In het volgende artikel in de reeks, Contoso [de SmartHotel-app op Azure Virtual machines naamconflicten](contoso-migration-rehost-vm.md) met behulp van de Azure Site Recovery-service.

