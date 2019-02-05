---
title: Een on-premises Contoso app rehost door te migreren naar Azure VM's en Azure SQL Database Managed Instance | Microsoft Docs
description: Meer informatie over hoe Contoso naamconflicten met een on-premises app op Azure Virtual machines en met behulp van Azure SQL Database Managed Instance.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: 76af79cc9ef1ebea30b30a291f451b7b0a4f3ba6
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55694479"
---
# <a name="contoso-migration-rehost-an-on-premises-app-on-an-azure-vm-and-sql-database-managed-instance"></a>Migratie van Contoso: Opnieuw hosten van een on-premises-app op een Azure-VM en het beheerde exemplaar van SQL Database

In dit artikel, Contoso de SmartHotel360-app migreert front-end virtuele machine met een Azure-VM met behulp van de Azure Site Recovery-service. Contoso wordt ook de app-database migreert naar Azure SQL Database Managed Instance.

> [!NOTE]
> Azure SQL Database Managed Instance is momenteel in preview.

In dit artikel is een in een reeks artikelen die documenten hoe het fictieve bedrijf Contoso de on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en een aantal scenario's die laten zien hoe u een migratie-infrastructuur instellen en uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit. Artikelen wordt toegevoegd aan de reeks na verloop van tijd.


**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: Overzicht](contoso-migration-overview.md) | Overzicht van de strategie voor de migratie van Contoso, de artikel-serie en de voorbeeld-apps die worden gebruikt in de reeks. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Contoso bereidt u de on-premises infrastructuur en de Azure-infrastructuur voor migratie. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen in de reeks. | Beschikbaar
[Artikel 3: On-premises resources voor migratie naar Azure evalueren](contoso-migration-assessment.md) | Contoso wordt uitgevoerd een evaluatie van de on-premises twee lagen SmartHotel app die wordt uitgevoerd op VMware. Contoso beoordeelt virtuele machines van app met behulp van de [Azure Migrate](migrate-overview.md) service. Contoso beoordeelt de app SQL Server-database met behulp van [Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
Artikel 4: Opnieuw hosten van een app op een Azure-VM en het beheerde exemplaar van SQL Database | Contoso wordt een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel app uitgevoerd. Contoso worden gemigreerd van de app front-end virtuele machine met behulp van [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso de app-database migreert naar een Azure SQL Database Managed Instance met behulp van de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | In dit artikel
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | Contoso migreert zijn app SmartHotel virtuele machines naar Azure-VM's met behulp van de Site Recovery-service. | Beschikbaar
[Artikel 6: Een app op Azure Virtual machines en in een SQL Server AlwaysOn-beschikbaarheidsgroep rehost](contoso-migration-rehost-vm-sql-ag.md) | De app SmartHotel, Contoso worden gemigreerd. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app. De Database Migration Service wordt gebruikt voor het migreren van de app-database naar een SQL Server-cluster dat wordt beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar
[7-artikel: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Contoso is een lift-and-shift-migratie van de app Linux osTicket virtuele Azure-machines met behulp van Site Recovery voltooid. | Beschikbaar
[8-artikel: Een Linux-app op Azure VM's en Azure Database for MySQL rehost](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migreert de Linux-osTicket-app naar Azure-VM's met behulp van Site Recovery. Deze migreert de app-database naar Azure Database for MySQL via MySQL Workbench. | Beschikbaar
[9-artikel: Een app in een Azure-web-app en Azure SQL Database herstructureren](contoso-migration-refactor-web-app-sql.md) | Contoso zijn app SmartHotel migreert naar een Azure-web-app en de app-database migreert naar een Azure SQL Server-exemplaar. | Beschikbaar
[Artikel 10: Een Linux-app in een Azure-web-app en de Azure Database for MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | De Linux-app osTicket migreert Contoso naar een Azure-web-app op meerdere sites. De web-app is geïntegreerd met GitHub voor continue levering. Contoso migreert de app-database naar een Azure Database for MySQL-exemplaar. | Beschikbaar
[11-artikel: Herstructureren van Team Foundation Server op Azure DevOps-Services](contoso-migration-tfs-vsts.md) | Contoso worden gemigreerd van de on-premises Team Foundation Server-implementatie door te migreren naar Azure DevOps-Services in Azure. | Beschikbaar
[12-artikel: Een app in Azure-containers en Azure SQL Database opnieuw modelleren](contoso-migration-rearchitect-container-sql.md) | Contoso zijn app SmartHotel is gemigreerd naar Azure en rearchitects vervolgens de app. Contoso-rearchitects van de laag van de web-app als een Windows-container en rearchitects van de app-database met behulp van Azure SQL Database. | Beschikbaar
[13-artikel: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Contoso wordt opnieuw gemaakt zijn SmartHotel-app met een scala aan mogelijkheden van Azure en services, waaronder Azure App Service, Azure Kubernetes Service, Azure Functions, Azure Cognitive Services en Azure Cosmos DB. | Beschikbaar
[14-artikel: Schalen van een migratie naar Azure](contoso-migration-scale.md) | Na het proberen van migratie combinaties, bereidt Contoso worden uitgebreid naar een volledige migratie naar Azure. | Beschikbaar




U kunt het voorbeeld SmartHotel360-app die wordt gebruikt in dit artikel downloaden [GitHub](https://github.com/Microsoft/SmartHotel360).



## <a name="business-drivers"></a>Zakelijke drijfveren

Het Contoso IT de leiding heeft nauw samengewerkt met zakelijke partners van het bedrijf om te begrijpen wat het bedrijf wil bereiken met deze migratie:

- **Adres van de groei van het bedrijf**: Contoso groeit. Als gevolg toegenomen druk op de on-premises systemen en de infrastructuur van het bedrijf.
- **De efficiëntie verhogen**: Contoso heeft nodig om te verwijderen van onnodige procedures en processen stroomlijnen voor de ontwikkelaars en gebruikers. De zakelijke behoeften IT zijn snel en om niet afval tijd of geld, dus het bedrijf kunnen sneller leveren op eisen van klanten.
- **De veerkracht vergroten**:  Contoso IT moet sneller te reageren op de behoeften van het bedrijf. Het moet mogelijk sneller dan de wijzigingen die plaatsvinden in de marketplace voor het bedrijf om in een globale economie succesvol te kunnen reageren. IT bij Contoso moet niet ophalen in de manier waarop of een zakelijke upblokkering worden.
- **Schaal**: Als van het bedrijf is groeit, moet: Contoso IT-systemen die hetzelfde tempo kunnen groeien opgeven.

## <a name="migration-goals"></a>Migratiedoelen

Het Contoso-cloud-team heeft de doelstellingen voor deze migratie geïdentificeerd. Doelstellingen van de migratie van het bedrijf gebruikt om te bepalen van de beste migratiemethode.

- Na de migratie, moet de app in Azure hebben dezelfde als de prestatiemogelijkheden die de app vandaag in on-premises VMWare-omgeving van Contoso heeft. Verplaatsen naar de cloud betekent niet dat de app-prestaties minder kritiek is.
- Contoso wil niet investeren in de app. De app is essentiële en belangrijk voor het bedrijf, maar Contoso gewoon wil de app in de huidige vorm overstappen naar de cloud.
- Beheertaken voor de database moeten worden geminimaliseerd nadat de app is gemigreerd.
- Contoso wilt niet dat met een Azure SQL Database voor deze app. Er op zoek naar alternatieven.


## <a name="solution-design"></a>Het ontwerp van oplossing

Na het vastmaken omlaag hun doelstellingen en vereisten, Contoso ontwerpen en beoordelingen van een implementatieoplossing, en identificeert het migratieproces, met inbegrip van de Azure-services die worden gebruikt voor de migratie.

### <a name="current-architecture"></a>Huidige architectuur 

- Contoso heeft een hoofddatacenter (**contoso-datacenter**). Het datacenter bevindt zich in de plaats van New York in het oostelijk deel van de Verenigde Staten.
- Contoso heeft drie extra lokale branches in de Verenigde Staten.
- Het belangrijkste datacenter is verbonden met internet met een fiber Metro Ethernet-verbinding (500 MBps).
- Elke vertakking is lokaal verbonden met het internet met behulp van kwalitatief hoogwaardige verbindingen met IPsec VPN-tunnels terug naar het primaire datacenter. De installatie, kan de hele netwerk van Contoso permanent zijn verbonden en optimaliseert de verbinding met internet.
- Het belangrijkste datacenter is volledig gevirtualiseerd met VMware. Contoso heeft twee ESXi 6.5-virtualisatiehosts die worden beheerd door vCenter Server 6.5.
- Contoso maakt gebruik van Active Directory voor identiteitsbeheer. Contoso maakt gebruik van DNS-servers in het interne netwerk.
- Contoso heeft een on-premises domeincontroller (**contosodc1**).
- De domeincontrollers worden uitgevoerd op virtuele VMware-machines. De domeincontrollers op lokale branches uitgevoerd op fysieke servers.
- De SmartHotel360-app is gelaagd over twee virtuele machines (**WEBVM** en **SQLVM**) die zich op een VMware ESXi-host met versie 6.5 (**contosohost1.contoso.com**). 
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) die worden uitgevoerd op een virtuele machine.

![Huidige Contoso-architectuur](./media/contoso-migration-rehost-vm-sql-managed-instance/contoso-architecture.png)  

### <a name="proposed-architecture"></a>Voorgestelde architectuur

In dit scenario wil Contoso voor het migreren van de twee lagen on-premises reis-app als volgt:

- Migreren van de app-database (**SmartHotelDB**) naar een Azure SQL Database Managed Instance.
- Migreren van de front-end **WebVM** met een Azure-VM.
- De on-premises machines in het Contoso-datacenter wordt gesteld wanneer de migratie is voltooid.

![Scenario-architectuur](media/contoso-migration-rehost-vm-sql-managed-instance/architecture.png) 

### <a name="database-considerations"></a>Database-overwegingen

Contoso heeft als onderdeel van het ontwerpproces, een vergelijking tussen Azure SQL Database en SQL Server Managed Instance. De volgende overwegingen kon ze om te bepalen aan de slag met Managed Instance.

- Beheerd exemplaar is erop gericht om te leveren bijna 100% compatibiliteit met de meest recente on-premises SQL Server-versie. Microsoft raadt aan voor het beheerde exemplaar voor klanten die on-premises SQL Server of op IaaS-VM die willen hun apps migreren naar een volledig beheerde service met minimale wijzigingen.
- Contoso is van plan om te migreren van een groot aantal apps van on-premises naar IaaS. Veel van deze zijn ISV opgegeven. Contoso beseft dat gebruik van Managed Instance zorgt u voor de compatibiliteit van de database voor deze apps, in plaats van met behulp van SQL-Database die wordt mogelijk niet ondersteund.
- Contoso kan gewoon een lift-and-shift-migratie naar Managed Instance met behulp van de volledig geautomatiseerde Data Migration Service (DMS). Met deze service in plaats, Contoso deze opnieuw kunt gebruiken voor toekomstige database-migraties.
- SQL Managed Instance biedt ondersteuning voor SQL Server Agent die een belangrijk probleem voor de SmartHotel360-app is. Contoso moet deze compatibiliteit, anders wordt er opnieuw wilt vormgeven, onderhoudsplannen vereist zijn voor de app.
- Contoso kan hun bestaande licenties voor de kortingstarieven in een SQL Database Managed Instance met Azure Hybrid Benefit voor SQL Server uitwisselen met Software Assurance. Hierdoor kan Contoso om op te slaan tot 30% op Managed Instance.
- Beheerd exemplaar is volledig zijn opgenomen in het virtuele netwerk, zodat er een hoge mate van isolatie en beveiliging van gegevens van Contoso. Contoso kan profiteren van de openbare cloud, terwijl de omgeving die geïsoleerd van het openbare Internet.
- Beheerd exemplaar biedt ondersteuning voor veel beveiligingsfuncties, inclusief altijd versleuteld, dynamische gegevensmaskering, beveiliging op rijniveau en detectie van bedreigingen.


### <a name="solution-review"></a>Beoordelingen van oplossing

Contoso evalueert het ontwerp van de voorgestelde door het samenstellen van een lijst met voor- en nadelen.

**Overweging** | **Details**
--- | ---
**Pros** |  WEBVM wordt verplaatst naar Azure zonder wijzigingen, waardoor de migratie eenvoudig.<br/><br/> SQL Managed Instance biedt ondersteuning voor de technische vereisten en de doelstellingen van Contoso.<br/><br/> Beheerd exemplaar biedt 100% compatibiliteit met de huidige implementatie, terwijl u ze verplaatst van SQL Server 2008 R2.<br/><br/>  Ze kunnen gebruikmaken van hun investering in Software Assurance en het gebruik van de Azure Hybrid Benefit forSQL Server en Windows Server.<br/><br/> Opnieuw kan worden gebruikt de Database Migration Service voor extra toekomstige migraties.<br/><br/> Beheerd exemplaar van SQL heeft ingebouwde fouttolerantie die Contoso hoeft niet te configureren. Dit zorgt ervoor dat de gegevenslaag niet langer een single point of failover is.
**Nadelen** | De WEBVM wordt Windows Server 2008 R2 uitgevoerd.  Hoewel dit besturingssysteem wordt ondersteund door Azure, is het niet langer ondersteund platform. [Meer informatie](https://support.microsoft.com/en-us/help/956893).<br/><br/> De weblaag blijft een single point of failover met alleen WEBVM met services.<br/><br/> Contoso moet doorgaan met ondersteuning van de laag van de web-app als een virtuele machine, in plaats van verplaatsen naar een beheerde service, zoals Azure App Service.<br/><br/> Voor de gegevenslaag Managed Instance mogelijk niet de beste oplossing als Contoso wilt aanpassen van het besturingssysteem of de database-server, of als ze willen uitvoeren van apps van derden samen met SQL Server. Met SQL Server op een IaaS-VM, kan deze flexibiliteit bieden. 

### <a name="migration-process"></a>Migratieproces

Contoso wordt de web- en gegevenslagen van de SmartHotel360-app migreren naar Azure door deze stappen te voltooien:

1. Contoso heeft al de Azure-infrastructuur aanwezig is, zodat deze moet een aantal specifieke Azure-onderdelen voor dit scenario toevoegen.
2. De gegevenslaag worden, gemigreerd met behulp van de Data Migration Service. De Data Migration Service maakt verbinding met de on-premises SQL Server-VM via een site-naar-site VPN-verbinding tussen de Contoso-datacenter en Azure. De Data Migration Service migreert vervolgens de database.
3. De weblaag worden, met behulp van een lift-and-shift-migratie met behulp van Site Recovery gemigreerd. Het proces omvat het voorbereiden van de on-premises VMware-omgeving, instellen en inschakelen van replicatie en de virtuele machines met failover naar Azure migreren.

     ![Migratiearchitectuur](media/contoso-migration-rehost-vm-sql-managed-instance/migration-architecture.png) 

### <a name="azure-services"></a>Azure-services

Service | Description | Kosten
--- | --- | ---
[Database migratieservice](https://docs.microsoft.com/azure/dms/dms-overview) | De Database Migration Service kunt naadloze migratie van meerdere databasebronnen naar Azure data-platforms met minimale downtime. | Meer informatie over [ondersteunde regio's](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) en [Database Migration Service-prijzen](https://azure.microsoft.com/pricing/details/database-migration/).
[Beheerd exemplaar voor Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | Managed Instance is een beheerde databaseservice die staat voor een volledig beheerde SQL Server-exemplaar in de Azure-cloud. Het maakt gebruik van dezelfde code als de meest recente versie van SQL Server Database Engine en heeft de nieuwste functies, prestatieverbeteringen en beveiligingspatches. | Met behulp van een SQL Database Managed Instance die wordt uitgevoerd in Azure, worden kosten in rekening gebracht op basis van capaciteit. Meer informatie over [prijzen van Managed Instance](https://azure.microsoft.com/pricing/details/sql-database/managed/). 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | De service Site Recovery coördineert en beheert de migratie en herstel na noodgevallen voor Azure-VM's en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, rekening Azure Storage-kosten in gebracht.  Azure-VM's worden gemaakt en kosten in rekening gebracht wanneer een failover optreedt. Meer informatie over [Site Recovery-kosten en prijzen](https://azure.microsoft.com/pricing/details/site-recovery/).

 

## <a name="prerequisites"></a>Vereisten

Contoso en andere gebruikers moeten voldoen aan de volgende vereisten voor dit scenario:

Vereisten | Details
--- | ---
**Schrijf u in de Managed Instance-preview** | U moet zijn geregistreerd bij de beperkte openbare preview voor SQL Database Managed Instance. U moet een Azure-abonnement [aanmelden](https://portal.azure.com#create/Microsoft.SQLManagedInstance). Registreren duurt een paar dagen om te voltooien, dus zorg ervoor dat u zich kunt registreren voordat u begint met het implementeren van dit scenario.
**Azure-abonnement** | U moet al een abonnement hebt gemaakt wanneer u de beoordeling in het eerste artikel in deze reeks uitvoeren. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder van het abonnement bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [op rollen gebaseerd toegangsbeheer gebruiken voor het beheren van Site Recovery toegang](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Site Recovery (on-premises)** | Uw on-premises vCenter Server-exemplaar moet versie 5.5, 6.0 of 6.5 worden uitgevoerd<br/><br/> Een ESXi-host waarop versie 5.5, 6.0 of 6.5<br/><br/> Een of meer virtuele VMware-machines die worden uitgevoerd op de ESXi-host.<br/><br/> Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Ondersteund [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) configuratie.
**Database migratieservice** | Voor de Database Migration Service, moet u een [compatibel on-premises VPN-apparaat](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> U moet de on-premises VPN-apparaat configureren. Een extern gericht openbaar IPv4-adres moet hebben. Het adres kan zich niet achter een NAT-apparaat.<br/><br/> Zorg ervoor dat u toegang hebt tot uw on-premises SQL Server-database.<br/><br/> Windows Firewall moet toegang hebben tot de bron-database-engine. Meer informatie over het [Windows Firewall configureren voor toegang tot de Database-Engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Als er een firewall voor uw databasemachine, kunt u regels voor toegang tot de database en de bestanden via SMB-poort 445 toevoegen.<br/><br/> De referenties die worden gebruikt voor het verbinding maken met de SQL Server-bronexemplaar en die beheerd exemplaar als doel, moeten lid zijn van de serverrol sysadmin.<br/><br/> U moet een netwerk delen in uw on-premises database die de Database Migration Service gebruiken kunt om back-up van de brondatabase.<br/><br/> Zorg ervoor dat de serviceaccount met de SQL Server-bronexemplaar schrijfmachtigingen voor de netwerkshare heeft.<br/><br/> Maak een notitie van een Windows-gebruiker en wachtwoord waarmee machtigingen voor volledig beheer op de netwerkshare heeft. De Database Migration Service imiteert de referenties van deze gebruiker voor het uploaden van back-upbestanden naar de Azure Storage-container.<br/><br/> Het installatieproces van SQL Server Express wordt het TCP/IP-protocol ingesteld op **uitgeschakelde** standaard. Zorg ervoor dat deze ingeschakeld.

## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Contoso voor het instellen van de implementatie plannen:

> [!div class="checklist"]
> * **Stap 1: Instellen van een SQL Database Managed Instance**: Contoso moet een vooraf gemaakte beheerd exemplaar waarnaar de on-premises SQL Server-database wilt migreren.
> * **Stap 2: Voorbereiden van de Database migratieservice**: Contoso moet de database migration-provider hebt geregistreerd, een exemplaar maken en maak vervolgens een Database Migration Service-project. Contoso moet ook instellen van een shared access signature (SAS) Uniform Resource Identifier (URI) voor de Database Migration Service. Een SAS-URI biedt gedelegeerde toegang tot resources in het Contoso-storage-account, zodat Contoso beperkte machtigingen voor opslagobjecten kunt toewijzen. Contoso stelt u een SAS-URI, waardoor de Database Migration Service toegang heeft tot de container van het opslagaccount waarnaar de service de SQL Server-back-upbestanden uploadt.
> * **Stap 3: Azure voorbereiden voor Site Recovery**: Contoso moet maken van een storage-account voor het opslaan van gerepliceerde gegevens voor Site Recovery. Het moet ook een Azure Recovery Services-kluis maken.
> * **Stap 4: On-premises VMware voorbereiden voor Site Recovery**: Contoso bereidt accounts voor VM-detectie- en agent-installatie verbinding maken met virtuele Azure-machines na een failover.
> * **Stap 5: Virtuele machines repliceren**: Als u replicatie instelt, Contoso configureren van de Site Recovery-bron en doel-omgevingen kunt inschakelen, stelt u een replicatiebeleid en start virtuele machines te repliceren naar Azure Storage.
> * **Stap 6: Migreren van de database met behulp van de Database Migration Service**: Contoso worden gemigreerd van de database.
> * **Stap 7: De virtuele machines migreren met behulp van Site Recovery**: Contoso voert een testfailover uit om te controleren of alles werkt. Vervolgens wordt een volledige failover voor de virtuele machines migreren naar Azure uitgevoerd door Contoso.

## <a name="step-1-prepare-a-sql-database-managed-instance"></a>Stap 1: Een beheerd exemplaar van SQL Database voorbereiden

Als u een Azure SQL Database Managed Instance instelt, moet Contoso een subnet dat voldoet aan de volgende vereisten:

- Het subnet moet worden toegewezen. Deze moet leeg zijn en mag geen andere cloudservice. Het subnet kan niet een gatewaysubnet.
- Nadat het beheerde exemplaar is gemaakt, moet Contoso resources niet toevoegen aan het subnet.
- Het subnet kan niet een netwerkbeveiligingsgroep die is gekoppeld aan deze hebben.
- Het subnet moet een gebruiker gedefinieerde routering (UDR) routeringstabel hebben. De enige route toegewezen moet 0.0.0.0/0 van volgende hop internet. 
- Optionele aangepaste DNS: Als aangepaste DNS is opgegeven in de Azure-netwerk, moet de recursieve resolvers IP-adres (zoals 168.63.129.16) van Azure worden toegevoegd aan de lijst. Meer informatie over het [aangepaste DNS configureren voor een beheerd exemplaar](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).
- Het subnet kan niet beschikken over een service-eindpunt dat is gekoppeld aan deze (storage of SQL). Service-eindpunten moeten worden uitgeschakeld op het virtuele netwerk.
- Het subnet moet minimaal 16 IP-adressen hebben. Meer informatie over het [het formaat van de Managed Instance-subnet](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration).
- In de omgeving van Contoso hybride zijn aangepaste DNS-instellingen vereist. Contoso configureert de DNS-instellingen voor het gebruik van een of meer van de Azure DNS-servers van het bedrijf. Meer informatie over [DNS aanpassing](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).

### <a name="set-up-a-virtual-network-for-the-managed-instance"></a>Instellen van een virtueel netwerk voor het beheerde exemplaar

Contoso-beheerders instellen van het virtuele netwerk als volgt: 

1. Ze een nieuw virtueel netwerk maken (**VNET-SQLMI-EU2**) in de primaire regio in de VS-Oost 2. Het virtuele netwerk wordt toegevoegd de **ContosoNetworkingRG** resourcegroep.
2. Ze toewijzen een adresruimte van 10.235.0.0/24. Ze zorgen ervoor dat het bereik geen overlap heeft met andere netwerken in de onderneming.
3. Ze toevoegen twee subnetten aan het netwerk:
    - **SQLMI-DS-EUS2** (10.235.0.0.25)
    - **SQLMI-SAW-EUS2** (10.235.0.128/29). Dit subnet wordt gebruikt voor het koppelen van een map met het beheerde exemplaar.

      ![Managed Instance - virtueel netwerk maken](media/contoso-migration-rehost-vm-sql-managed-instance/mi-vnet.png)

4. Nadat het virtuele netwerk en subnetten worden geïmplementeerd, zij het peer-netwerken als volgt:

    - Peers **VNET-SQLMI-EUS2** met **VNET-HUB-EUS2** (het virtuele hub virtuele netwerk voor de VS-Oost 2).
    - Peers **VNET-SQLMI-EUS2** met **VNET-PROD-EUS2** (het productienetwerk).

      ![Netwerkpeering](media/contoso-migration-rehost-vm-sql-managed-instance/mi-peering.png)

5. Deze aangepaste DNS-instellingen hebt ingesteld. DNS verwijst eerst naar de Contoso Azure-domeincontrollers. Azure DNS is secundaire. De Contoso-Azure-domeincontrollers zich bevinden als volgt:

    - Bevindt zich in de **PROD-DC-EUS2** subnet in de VS-Oost 2-productienetwerk (**VNET-PROD-EUS2**)
    - **CONTOSODC3** adres: 10.245.42.4
    - **CONTOSODC4** adres: 10.245.42.5
    - Azure DNS-resolver: 168.63.129.16

      ![Netwerk DNS-servers](media/contoso-migration-rehost-vm-sql-managed-instance/mi-dns.png)

*Meer hulp nodig?*

- Bekijk een overzicht van [SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance).
- Meer informatie over het [een virtueel netwerk maken voor een SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration).
- Meer informatie over het [peering instellen](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering).
- Meer informatie over het [Azure Active Directory DNS-instellingen bijwerken](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-dns).

### <a name="set-up-routing"></a>Instellen van Routering

Het beheerde exemplaar wordt geplaatst in een particulier virtueel netwerk. Contoso moet een routetabel voor het virtuele netwerk om te communiceren met de Azure Management-Service. Als het virtuele netwerk niet kunnen met de service die wordt beheerd communiceren, wordt het virtuele netwerk niet toegankelijk.

Contoso rekening gehouden met deze factoren:

- De routetabel bevat een set regels (routes) die specificeren hoe pakketten worden verzonden vanuit het beheerde exemplaar in het virtuele netwerk moeten worden gerouteerd.
- De routetabel is gekoppeld aan subnetten waarin beheerde instanties zijn geïmplementeerd. Elk pakket dat een subnet verlaat wordt verwerkt op basis van de bijbehorende routetabel.
- Een subnet kan worden gekoppeld aan slechts één routetabel.
- Er zijn geen extra kosten voor het routetabellen maken in Microsoft Azure.

 Voor het instellen van routering Contoso doen beheerders het volgende:

1. Maken van een UDR (route) tabel in de **ContosoNetworkingRG** resourcegroep.

    ![Routetabel](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table.png)

2. Om te voldoen aan de Managed Instance, nadat de routetabel (**MIRouteTable**) is geïmplementeerd, ze een route met het adresvoorvoegsel 0.0.0.0/0 toevoegen. De **volgende hoptype** optie is ingesteld op **Internet**.

    ![Voorvoegsel voor route-tabel](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-prefix.png)
    
3. ze koppelen van de routetabel, met de **SQLMI-DB-EUS2** subnet (in de **VNET-SQLMI-EUS2** netwerk). 

    ![Tabel subnet routeren](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-subnet.png)
    
*Meer hulp nodig?*

Meer informatie over het [routes instellen voor een beheerd exemplaar](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal).

### <a name="create-a-managed-instance"></a>Een beheerd exemplaar maken

Contoso-beheerders kunnen nu een SQL Database Managed Instance inrichten:

1. Omdat het beheerde exemplaar een business-app fungeert, implementeren ze de Managed Instance in de primaire VS-Oost 2-regio van het bedrijf. Ze de beheerde instantie moet toevoegen de **ContosoRG** resourcegroep.
2. Ze selecteren een prijzen laag, grootte van berekening en opslag voor het exemplaar. Meer informatie over [prijzen van Managed Instance](https://azure.microsoft.com/pricing/details/sql-database/managed/).

    ![Beheerd exemplaar](media/contoso-migration-rehost-vm-sql-managed-instance/mi-create.png)

3. Nadat het beheerde exemplaar is geïmplementeerd, twee nieuwe resources worden weergegeven in de **ContosoRG** resourcegroep:

    - Een virtueel cluster in de aanvraag Contoso heeft meerdere beheerde exemplaren.
    - De SQL Server-Database beheerd exemplaar. 

      ![Beheerd exemplaar](media/contoso-migration-rehost-vm-sql-managed-instance/mi-resources.png)

*Meer hulp nodig?*

Meer informatie over het [inrichten van een beheerd exemplaar](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal).

## <a name="step-2-prepare-the-database-migration-service"></a>Stap 2: Voorbereiden van de Database migratieservice

Om voor te bereiden op de Database Migration Service, moet de beheerders van Contoso een paar dingen doen:

- De Database Migration Service-provider registreren in Azure.
- De Database Migration Service toegang geven tot Azure Storage voor het uploaden van de back-upbestanden die worden gebruikt voor het migreren van een database. Als u wilt toegang bieden voor Azure Storage, maken ze een Azure Blob storage-container. Genereert een SAS-URI voor de Blob storage-container. 
- Maak een Database Migration Service-project.

Vervolgens, ze de volgende stappen uitvoeren:

1. Ze zich registreren voor de database migration-provider onder het abonnement.
    ![Database migratieservice - Register](media/contoso-migration-rehost-vm-sql-managed-instance/dms-subscription.png)

2. Ze maken een Blob storage-container. Contoso genereert een SAS-URI, zodat de Database Migration Service toegang kan hebben.

    ![Database migratieservice: genereren van een SAS-URI](media/contoso-migration-rehost-vm-sql-managed-instance/dms-sas.png)

3. Ze maken een Database Migration Service-exemplaar. 

    ![Database migratieservice - exemplaar maken](media/contoso-migration-rehost-vm-sql-managed-instance/dms-instance.png)

4. Deze plaatst u de Database Migration Service-exemplaar in de **PROD-DC-EUS2** subnet van de **VNET-PROD-DC-EUS2** virtueel netwerk.
    - De Database Migration Service wordt hier geplaatst, omdat de service moet zich in een virtueel netwerk dat toegang heeft tot de on-premises SQL Server-VM via een VPN-gateway.
    - De **VNET-PROD-EUS2** is gekoppeld aan **VNET-HUB-EUS2** en mag externe gateways gebruiken. De **externe gateways gebruiken** optie zorgt ervoor dat de Database Migration Service als vereist communiceren kan.

        ![Database migratieservice - netwerk configureren](media/contoso-migration-rehost-vm-sql-managed-instance/dms-network.png)

*Meer hulp nodig?*

- Meer informatie over het [instellen van de Database Migration Service](https://docs.microsoft.com/azure/dms/quickstart-create-data-migration-service-portal).
- Meer informatie over het [maken en gebruiken van SAS](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2).


## <a name="step-3-prepare-azure-for-the-site-recovery-service"></a>Stap 3: Azure voorbereiden voor de Site Recovery-service

Verschillende Azure-elementen zijn vereist voor Contoso voor het instellen van Site Recovery voor migratie van de weblaag VM (**WEBMV**):

- Een virtueel netwerk waarin failover resources zich bevinden.
- Een opslagaccount voor gerepliceerde gegevens. 
- Een Recovery Services-kluis in Azure.

Beheerders van Contoso Site Recovery als volgt instellen:

1. Omdat de virtuele machine een webfront-end in de SmartHotel360-app, Contoso failover van de virtuele machine om de bestaande productienetwerk (**VNET-PROD-EUS2**) en het subnet (**PROD-FE-EUS2**). Het netwerk en subnet bevinden zich in de primaire regio in de VS-Oost 2. Contoso instellen van het netwerk wanneer het [geïmplementeerd van de Azure-infrastructuur](contoso-migration-infrastructure.md).
2. Maken van een storage-account (**contosovmsacc20180528**). Contoso maakt gebruik van een account voor algemeen gebruik. Contoso selecteert u standard-opslag en lokaal redundant storage-replicatie.

    ![Site Recovery - storage-account maken](media/contoso-migration-rehost-vm-sql-managed-instance/asr-storage.png)

3. Met de netwerk- en storage-account in plaats ze een kluis maken (**ContosoMigrationVault**). Contoso plaatst de kluis in de **ContosoFailoverRG** resourcegroep in de primaire regio in de VS-Oost 2.

    ![Maken van de Recovery Services - kluis](media/contoso-migration-rehost-vm-sql-managed-instance/asr-vault.png)

*Meer hulp nodig?*

Meer informatie over het [Azure instellen voor Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure).


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>Stap 4: On-premises VMware voorbereiden voor Site Recovery

Om voor te bereiden VMware voor Site Recovery, moet de Contoso-beheerders deze taken uitvoeren:

- Een account voorbereiden op de vCenter-Server-exemplaar of vSphere ESXi-host. Het account automatiseert VM-detectie.
- Een account waarmee de automatische installatie van de Mobility-Service op virtuele VMware-machines die Contoso wil repliceren voorbereiden.
- Bereid de on-premises machines verbinding maken met Azure-VM's wanneer ze na een failover worden gemaakt.


### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Virtuele machines automatisch detecteren. Minimaal een alleen-lezen-account is vereist.
- Replicatie, failover en failback orkestreren. Contoso moet een account die bewerkingen zoals het maken en verwijderen van schijven en inschakelen van virtuele machines kan worden uitgevoerd.

Contoso-beheerders het account instellen door deze taken uit te voeren:

1. Hiermee maakt u een rol op vCenter-niveau.
2. De vereiste machtigingen aan die rol toegewezen.

*Meer hulp nodig?*

Meer informatie over het [maken en toewijzen van een rol voor automatische detectie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery).

### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor de installatie van Mobility Service

De Mobility-Service moet worden geïnstalleerd op de virtuele machine die Contoso wil repliceren. Contoso rekening gehouden met deze factoren over de Mobility-Service:

- Site Recovery kan een automatische push-installatie van dit onderdeel kunt doen wanneer Contoso Hiermee schakelt u replicatie voor de virtuele machine.
- Voor automatische push-installatie, Contoso moet een account voorbereiden waarmee Site Recovery gebruikt voor toegang tot de virtuele machine.
- Dit account is opgegeven wanneer replicatie is geconfigureerd in de Azure-console.
- Contoso beschikken over een domein of lokale account met machtigingen om te installeren op de virtuele machine.

*Meer hulp nodig hebt*

Meer informatie over het [maken van een account voor push-installatie van de Mobility-Service](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation).

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Na een failover naar Azure wil Contoso kunnen verbinding maken met de gerepliceerde virtuele machines in Azure. Voor verbinding met de gerepliceerde virtuele machines in Azure, moet de beheerders van Contoso een paar taken op de on-premises VM vóór de migratie voltooien: 

1. Ze schakelt u RDP op de on-premises virtuele machine voordat de failover voor toegang via internet. Ze zorgen ervoor dat TCP en UDP-regels worden toegevoegd voor de **openbare** profiel, en dat RDP is toegestaan in **Windows Firewall** > **Apps toegestaan** voor alle profielen.
2. Voor toegang via het Contoso-site-naar-site VPN schakelt ze u RDP in op de on-premises machine. Dat RDP in **Windows Firewall** > **toegestane apps en functies** voor **domein en privé** netwerken.
3. Ze SAN-beleid van het besturingssysteem op de on-premises virtuele machine ingesteld **OnlineAll**.

Beheerders van Contoso moet ook deze items te controleren wanneer ze een failover worden uitgevoerd:

- Er mogen geen Windows-updates in behandeling op de virtuele machine wanneer een failover wordt geactiveerd. Als Windows-updates in behandeling zijn, kunnen niet gebruikers Contoso aanmelden bij de virtuele machine totdat de update is voltooid.
- Controleer na een failover, beheerders **diagnostische gegevens over opstarten** om een schermopname van de virtuele machine weer te geven. Als ze de diagnostische gegevens over het opstarten niet kunnen bekijken, moeten ze controleren dat de virtuele machine wordt uitgevoerd, en bekijk vervolgens [tips voor probleemoplossing](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

## <a name="step-5-replicate-the-on-premises-vms-to-azure"></a>Stap 5: De on-premises VM's repliceren naar Azure

Voordat u een migratie naar Azure uitvoert, moet de Contoso-beheerders instellen en inschakelen van replicatie voor de on-premises VM.

### <a name="set-a-replication-goal"></a>Een replicatiedoel instellen

1. In de kluis, onder de kluisnaam (**ContosoVMVault**), ze een replicatiedoel ingesteld (**aan de slag** > **siteherstel**  >   **Infrastructuur voorbereiden**).
2. Ze geven dat de machines zich op locatie zijn, dat ze virtuele VMware-machines repliceren naar Azure.

    ![Infrastructuur - beveiligingsdoel voorbereiden](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

Als u wilt doorgaan, bevestigen beheerders van Contoso dat ze plannen van de implementatie hebt voltooid. Ze selecteren **Ja, heb ik gedaan**. In deze implementatie Contoso is een enkele virtuele machine migreren, plannen van de implementatie is niet nodig.

### <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Nu, Contoso-beheerders de bronomgeving configureren. Om in te stellen de bronomgeving, ze een OVF-sjabloon downloaden en gebruiken om de configuratieserver en de bijbehorende onderdelen als een maximaal beschikbare te implementeren, on-premises VMware-VM. Onderdelen op de server zijn onder andere:

- De configuratieserver die coördineert de communicatie tussen de on-premises infrastructuur en Azure. De configuratieserver beheert de gegevensreplicatie.
- De processerver die als replicatiegateway fungeert. De processerver:
    - Replicatiegegevens ontvangt.
    - Datum van de replicatie optimaliseert met behulp van caching, compressie en codering.
    - Datum van de replicatie verzendt naar Azure Storage.
- De processerver installeert ook de Mobility-Service op de virtuele machines die worden gerepliceerd. De processerver detecteert automatisch on-premises VMware-VM's.
- Na de configuratieserver VM is gemaakt en gestart, wordt de server door Contoso geregistreerd in de kluis.

Voor het instellen van de bron doen Contoso omgevingsbeheerders het volgende:

1. Ze de OVF-sjabloon downloaden via de Azure portal (**infrastructuur voorbereiden** > **bron** > **configuratieserver**).
    
    ![Een configuratieserver toevoegen](./media/contoso-migration-rehost-vm-sql-managed-instance/add-cs.png)

2. Ze Importeer de sjabloon in VMware om te maken en implementeren van de virtuele machine.

    ![Implementeren van OVF-sjabloon](./media/contoso-migration-rehost-vm-sql-managed-instance/vcenter-wizard.png)

3.  Wanneer ze op de virtuele machine voor het eerst inschakelen, start deze in een Windows Server 2016-installatie-ervaring. Ze accepteer de gebruiksrechtovereenkomst en een administrator-wachtwoord invoert.
4. Wanneer de installatie is voltooid, zich ze aanmelden bij de virtuele machine als de beheerder. Op de eerste keer aanmelden, wordt de Azure Site Recovery Configuration Tool automatisch uitgevoerd.
5. In de Site Recovery Configuration Tool Voer ze een naam te gebruiken voor het registreren van de configuratieserver in de kluis.
6. Het hulpprogramma controleert of de verbinding van de virtuele machine met Azure. Nadat de verbinding tot stand is gebracht, selecteren ze **aanmelden** zich aanmeldt bij de Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin de configuratieserver is geregistreerd. 

    ![De configuratieserver registreren](./media/contoso-migration-rehost-vm-sql-managed-instance/config-server-register2.png)

7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op. Ze zich aanmelden bij de machine opnieuw. De configuratiewizard voor Server Management wordt automatisch gestart.
8. In de wizard selecteert ze de NIC die replicatieverkeer moet ontvangen. Deze instelling kan niet worden gewijzigd nadat deze geconfigureerd.
9. Ze selecteert u het abonnement, resourcegroep en Recovery Services-kluis waarin de configuratieserver registreren.

    ![Recovery Services-kluis selecteren](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz1.png)

10. Ze downloaden en installeren van de MySQL-Server en VMWare PowerCLI. Ze vervolgens valideert dat de serverinstellingen.
11. Na de validatie Geef ze de FQDN-naam of IP-adres van de vCenter-Server-exemplaar of vSphere-host. Ze laat de standaardpoort, en voer een weergavenaam voor het exemplaar van de vCenter-Server in Azure.
12. Ze geven het account dat eerder is gemaakt, zodat Site Recovery kan automatisch detecteren van VMware-VM's die beschikbaar voor replicatie zijn. 
13. Deze invoeren referenties, zodat de Mobility-Service wordt automatisch geïnstalleerd wanneer replicatie is ingeschakeld. Voor Windows-machines moet het account lokale beheerdersmachtigingen op de virtuele machines. 

    ![VCenter-Server configureren](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz2.png)

7. Wanneer de registratie is voltooid, klikt u in Azure portal, ze opnieuw te verifiëren dat de configuratieserver en de VMware-server worden weergegeven op de **bron** pagina in de kluis. Detectie kan 15 minuten of langer duren. 
8. Site Recovery maakt verbinding met de VMware-servers met behulp van de opgegeven instellingen en VM's worden gedetecteerd.

### <a name="set-up-the-target"></a>Het doel instellen

Beheerders van Contoso configureren nu de replicatiedoelomgeving:

1. In **infrastructuur voorbereiden** > **doel**, ze de doelinstellingen selecteren.
2. Site Recovery controleert of er een storage-account en het netwerk in het opgegeven doel is.

### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

Wanneer de bron en het doel zijn ingesteld, wordt beheerders van Contoso een replicatiebeleid maken en koppelt u het beleid aan de configuratieserver:

1. In **infrastructuur voorbereiden** > **replicatie-instellingen** > **replicatiebeleid** >  **maken en Koppelen**, het maken van de **ContosoMigrationPolicy** beleid.
2. Ze gebruiken de standaard-instellingen:
    - **RPO-drempelwaarde**: De standaardwaarde van 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
    - **Bewaarperiode van herstelpunt**: De standaardwaarde van 24 uur. Deze waarde wordt bepaald hoe lang de bewaarperiode is voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
    - **Frequentie van de app-consistente momentopname**: De standaardwaarde van 1 uur. Deze waarde bepaalt de frequentie waarmee App-consistente momentopnamen worden gemaakt.
 
    ![Beleid voor wachtwoordreplicatie - maken](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy.png)

3. Het beleid wordt automatisch gekoppeld aan de configuratieserver. 

    ![Beleid voor wachtwoordreplicatie - koppelen](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy2.png)

*Meer hulp nodig?*

- U kunt een overzicht van de volgende stappen uit in lezen [herstel na noodgevallen instellen voor on-premises VMware-machines](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Gedetailleerde instructies zijn beschikbaar om u te helpen [de bronomgeving instellen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [de configuratieserver implementeren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), en [replicatie-instellingen configureren](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).

### <a name="enable-replication"></a>Replicatie inschakelen

Beheerders van Contoso kunnen nu starten WebVM repliceren.

1. In **toepassing repliceren** > **bron** > **repliceren**, ze de instellingen voor gegevensbron selecteren.
2. Ze geven aan dat ze willen virtuele machines inschakelen, selecteert u de vCenter-Server-exemplaar en stel de configuratieserver.

    ![Inschakelen van replicatie - bron](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication1.png)
 
3. Ze geven de doelinstellingen, met inbegrip van de resourcegroep en een netwerk waarin de Azure-VM na failover geplaatst worden wordt. Ze geven het opslagaccount waarin de gerepliceerde gegevens worden opgeslagen.

    ![Replicatie - doel inschakelen](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication2.png)

4. Ze selecteren **WebVM** voor replicatie. Site Recovery installeert de Mobility-Service op elke virtuele machine wanneer replicatie is ingeschakeld. 

    ![Replicatie inschakelen: Selecteer de virtuele machine](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication3.png)

5. Ze controleren dat het juiste replicatiebeleid is geselecteerd en replicatie inschakelen voor **WEBVM**. Volgen ze Replicatievoortgang gedurende **taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.
6. In **Essentials** in Azure portal, zien ze de status voor de virtuele machines die naar Azure repliceren:

    ![Infrastructuurweergave](./media/contoso-migration-rehost-vm-sql-managed-instance/essentials.png)

*Meer hulp nodig?*

U kunt een overzicht van de volgende stappen uit in lezen [inschakelen replicatie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).

## <a name="step-6-migrate-the-database"></a>Stap 6: Migreren van de database 

Beheerders van Contoso moet een Database Migration Service-project maken en vervolgens migreren van de database.

### <a name="create-a-database-migration-service-project"></a>Een Database Migration Service-project maken

1. Ze maken een Database Migration Service-project. Ze selecteert de **SQL Server** bronservertype, en **Azure SQL Database Managed Instance** als het doel.

     ![Database migratieservice - nieuw migratieproject](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-project.png)

2. De Wizard wordt geopend.

### <a name="migrate-the-database"></a>Migreren van de database 

1. Deze opgeven in de Wizard migratie met de bron-VM waarop de on-premises database zich bevindt. Ze voert u de referenties voor toegang tot de database.

    ![Database migratieservice - details van gegevensbron](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source.png)

2. ze selecteert u de database te migreren (**SmartHotel.Registration**):

    ![Database migratieservice - databases van de bron selecteren](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-sourcedb.png)

3. Voor het doel Geef ze de naam van het beheerde exemplaar in Azure en de referenties voor toegang.

    ![Database migratieservice - Doeldetails](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-target-details.png)

4. In **nieuwe activiteit** > **migratie uitvoeren**, ze de instellingen voor het uitvoeren van de migratie opgeven:
    - Referenties voor de bron en het doel.
    - De database te migreren.
    - De netwerkshare die zijn gemaakt op de on-premises VM. De Database Migration Service gaat bron back-ups naar deze share. 
        - Het serviceaccount dat de SQL Server-bronexemplaar wordt uitgevoerd moet schrijfmachtigingen hebben op deze share.
        - De FQDN-pad naar de share moet worden gebruikt.
    - De SAS-URI die de Database Migration Service toegang biedt tot de container van het opslagaccount waarnaar de service wordt geüpload de back-upbestanden voor de migratie.

        ![Database migratieservice - migratie-instellingen configureren](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-migration-settings.png)

5. Deze migratie-instellingen opslaan, en voer vervolgens de migratie.
6. In **overzicht**, controleren ze de migratiestatus van de.

    ![Database migratieservice - Monitor](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor1.png)

7. Wanneer de migratie is voltooid, wordt ze controleren dat de doeldatabase op het beheerde exemplaar bestaat.

    ![Database migratieservice: de databasemigratie controleren](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor2.png)

## <a name="step-7-migrate-the-vm"></a>Stap 7: De virtuele machine migreren

Beheerders van Contoso uitvoeren van een snelle failover testen en vervolgens de virtuele machine te migreren.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Voordat u migreert WEBVM, een test-failover zorgt ervoor dat alles werkt zoals verwacht. Beheerders te laten uitvoeren in de volgende stappen:

1. Ze een testfailover uitvoeren naar de laatst beschikbare punt in tijd (**laatst verwerkte**).
2. Ze selecteren **sluit de computer voordat u begint met failover**. Met deze optie is geselecteerd, probeert Site Recovery de bron-VM wordt afgesloten voordat deze de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. 
3. Test-failover wordt uitgevoerd: 
    1. Een controle uitgevoerd om ervoor te zorgen dat de voorwaarden die zijn vereist voor de migratie voldaan is.
    2. De gegevens worden verwerkt in de failover, zodat een Azure-VM kan worden gemaakt. Als de meest recente herstelpunt is geselecteerd, wordt een herstelpunt wordt gemaakt van de gegevens.
    3.  Azure-VM is gemaakt met behulp van de gegevens verwerkt in de vorige stap.
3. Wanneer de failover is voltooid, wordt de replica virtuele machine in Azure weergegeven in de Azure-portal. ze controleren of alles goed werkt: de virtuele machine is de juiste grootte heeft, verbonden met het juiste netwerk en deze wordt uitgevoerd. 
4. Nadat u hebt de testfailover gecontroleerd, ze opschonen van de failover en noteer eventuele opmerkingen. 

### <a name="migrate-the-vm"></a>De virtuele machine migreren

1. Nadat u hebt gecontroleerd of de testfailover heeft gewerkt zoals verwacht, Contoso-beheerders maken van een herstelplan voor migratie en WEBVM toevoegen aan het abonnement:

     ![Plan voor herstel - Select-items maken](./media/contoso-migration-rehost-vm-sql-managed-instance/recovery-plan.png)

2. Ze een failover worden uitgevoerd op het abonnement, de meest recente herstelpunt selecteren. Deze opgeven dat Site Recovery proberen moet de on-premises VM afsluiten voordat deze de failover wordt geactiveerd.

    ![Failover](./media/contoso-migration-rehost-vm-sql-managed-instance/failover1.png)

3. Na de failover controleren ze of de Azure-VM wordt weergegeven zoals verwacht in de Azure-portal.

   ![Abonnementsdetails voor herstel](./media/contoso-migration-rehost-vm-sql-managed-instance/failover2.png)

4. Nadat u hebt gecontroleerd, wordt dat ze bij het voltooien van de migratie voor het voltooien van het migratieproces, replicatie voor de virtuele machine stoppen en stop de facturering voor de virtuele machine voor Site Recovery.

    ![Failover - migratie voltooien](./media/contoso-migration-rehost-vm-sql-managed-instance/failover3.png)

### <a name="update-the-connection-string"></a>De verbindingsreeks bijwerken

Beheerders van Contoso bijwerken als de laatste stap in het migratieproces, de verbindingsreeks van de toepassing om te verwijzen naar de gemigreerde database die wordt uitgevoerd op de Contoso Managed Instance.

1. In de Azure-portal, ze de connection string vinden door te selecteren **instellingen** > **verbindingsreeksen**.

    ![Verbindingsreeksen](./media/contoso-migration-rehost-vm-sql-managed-instance/failover4.png)  

2. De tekenreeks worden bijgewerkt met de gebruikersnaam en het wachtwoord van de SQL Database Managed Instance.
3. Nadat de tekenreeks die is geconfigureerd, vervangt u de huidige verbindingsreeks in het bestand web.config van de toepassing.
4. Na het bijwerken van het bestand en op te slaan, ze Start IIS opnieuw op WEBVM door uit te voeren `IISRESET /RESTART` in een opdrachtpromptvenster.
5. Nadat IIS opnieuw is opgestart, wordt in de app maakt gebruik van de database die wordt uitgevoerd op de SQL Database Managed Instance.
6. Op dit moment kunnen ze afsluiten on-premises de SQLVM-machine. De migratie is voltooid.

*Meer hulp nodig?*

- Meer informatie over het [voert een testfailover uit](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure). 
- Meer informatie over het [maken van een herstelplan](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans).
- Meer informatie over het [failover naar Azure](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover).

## <a name="clean-up-after-migration"></a>Na de migratie opschonen

Met de migratie is voltooid, de SmartHotel360-app wordt uitgevoerd op een Azure VM en de SmartHotel360-database is beschikbaar in de Azure SQL Database Managed Instance.  

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

- Het team controleert de netwerkbeveiligingsgroepen die worden gebruikt om toegang te beheren voor de virtuele machine. Netwerkbeveiliging groepen ervoor te zorgen dat alleen het verkeer dat is toegestaan naar de app kunt doorgeven.
- Het Contoso security team ook overweegt de gegevens op de schijf te beveiligen met behulp van Azure Disk Encryption en Azure Key Vault.
- Het team kunt detectie van bedreigingen op het beheerde exemplaar. Detectie van bedreigingen verzendt een waarschuwing van Contoso security team/service desk systeem een ticket openen als een bedreiging wordt gedetecteerd. Meer informatie over [bedreigingen voor de Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-threat-detection).

     ![Beheerd exemplaar beveiliging - detectie van bedreigingen](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-security.png)  

Zie voor meer informatie over procedures voor beveiliging voor virtuele machines, [aanbevolen beveiligingsprocedures voor IaaS-workloads in Azure](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms).

### <a name="bcdr"></a>BCDR

Voor bedrijfscontinuïteit en herstel na noodgevallen (BCDR) voert Contoso de volgende acties uit:

- Gegevens veilig te houden: Contoso back-ups van de gegevens op de virtuele machines met behulp van de Azure Backup-service. [Meer informatie](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- Houd apps ingesteld en geactiveerd: Contoso repliceert de virtuele machines in Azure-app naar een secundaire regio met behulp van Site Recovery. [Meer informatie](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart).
- Contoso leert meer over het beheren van SQL Managed Instance, met inbegrip van [databaseback-ups](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).


### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

- Contoso heeft een bestaande licentie voor WEBVM. Als u wilt profiteren van prijzen met Azure Hybrid Benefit, converteert Contoso de bestaande VM in Azure.
- Contoso kan Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Kostenbeheer is een kostenbeheeroplossing met meerdere Clouds management-oplossing waarmee Contoso gebruiken en beheren van Azure en andere cloudresources. Meer informatie over [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview). 


## <a name="conclusion"></a>Conclusie

In dit artikel naamconflicten Contoso de SmartHotel360-app in Azure door te migreren van de app front-end-VM naar Azure met behulp van de Site Recovery-service. Contoso migreert de on-premises database naar een Azure SQL Database Managed Instance met behulp van de Azure Database Migration Service.

## <a name="next-steps"></a>Volgende stappen

In het volgende artikel in de reeks, Contoso [de SmartHotel360-app op Azure Virtual machines naamconflicten](contoso-migration-rehost-vm.md) met behulp van de Azure Site Recovery-service.

