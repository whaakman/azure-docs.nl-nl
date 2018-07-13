---
title: Contoso-Set om een migratie-infrastructuur | Microsoft Docs
description: Meer informatie over hoe Contoso stelt u een Azure-infrastructuur voor migratie naar Azure.
services: azure-migrate
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: 6f535d98ea9e1312e4d5f197d121c8d12c109449
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39002276"
---
# <a name="contoso---deploy-a-migration-infrastructure"></a>Contoso - een migratie-infrastructuur implementeren

In dit artikel wordt bekeken hoe Contoso stelt u een on-premises en Azure-infrastructuur ter voorbereiding voor migratie naar Azure, en voor het uitvoeren van het bedrijf in een hybride omgeving.

- Er is een voorbeeldarchitectuur die specifiek is voor Contoso.
- Of u alle elementen in het artikel wordt beschreven moet, is afhankelijk van uw strategie voor clientmigratie. Bijvoorbeeld, als u alleen cloud-eigen apps in Azure bouwt, moet u mogelijk een minder complexe netwerken structuur.

Dit document is de tweede reeks artikelen waarin hoe het fictieve bedrijf dat Contoso migreert on-premises bronnen naar de Microsoft Azure-cloud. De reeks bevat informatie en implementatiescenario's die laten zien hoe u voor het instellen van de infrastructuur van een migratie, de geschiktheid van on-premises bronnen voor migratie beoordelen en verschillende typen migraties worden uitgevoerd. Scenario's toeneemt in complexiteit en we hierna zullen toevoegen aanvullende artikelen na verloop van tijd.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de artikel-serie en de voorbeeld-apps die we gebruiken. | Beschikbaar
Artikel 2: Een Azure-infrastructuur implementeren | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen. | In dit artikel.
[Artikel 3: Evalueer on-premises bronnen voor migratie naar Azure](contoso-migration-assessment.md)  | Laat zien hoe een evaluatie van een on-premises twee lagen SmartHotel app waarop VMware wordt uitgevoerd in Contoso. Contoso beoordeelt de virtuele machines van een app met de [Azure Migrate](migrate-overview.md) -service en de SQL Server-database van de app met de [Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
[Artikel 4: Een app op Azure VM's en een beheerde SQL-exemplaar opnieuw hosten](contoso-migration-rehost-vm-sql-managed-instance.md) | Ziet u hoe Contoso een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel app uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database naar een SQL beheerd exemplaar, met behulp van de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | Ziet u hoe Contoso de app SmartHotel virtuele machines migreren naar Azure-VM's, met behulp van de Site Recovery-service. | Beschikbaar
[Artikel 6: Een app op Azure VM's en SQL Server Always On Availability Group opnieuw hosten](contoso-migration-rehost-vm-sql-ag.md) | Laat zien hoe de app SmartHotel door Contoso worden gemigreerd. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app en de Database Migration service de app-database migreren naar een SQL Server-cluster dat is beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Ziet u hoe Contoso heeft een lift-and-shift-migratie van de Linux-osTicket-app op virtuele machines van Azure met Site Recovery | Beschikbaar
[Artikel 8: Een Linux-app op Azure VM's en Azure MySQL Rehost](contoso-migration-rehost-linux-vm-mysql.md) | Ziet u hoe Contoso de osTicket Linux app overzet naar virtuele Azure-machines met behulp van Site Recovery en de app-database migreert naar een Azure MySQL-Server-exemplaar met behulp van MySQL Workbench. | Beschikbaar
[Artikel 9: Een app op Azure Web Apps en Azure SQL database herstructureren](contoso-migration-refactor-web-app-sql.md) | Laat zien hoe Contoso de app SmartHotel migreert naar een Azure-Web-App en de app-database migreert naar Azure SQL Server-exemplaar | Beschikbaar
[Artikel 10: Een Linux-app op Azure-Web-Apps en Azure MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | Ziet u hoe Contoso de osTicket Linux app migreert naar Azure Web Apps op meerdere locaties, geïntegreerd met GitHub voor continue levering. Zij migreren de app-database naar een Azure MySQL-exemplaar. | Beschikbaar
[Artikel 11: Herstructureren TFS op VSTS](contoso-migration-tfs-vsts.md) | Ziet u hoe Contoso hun Team Foundation Server (TFS) on-premises implementatie door te migreren migreert het naar Visual Studio Team Services (VSTS) in Azure. | Beschikbaar
[Artikel 12: Opnieuw ontwerpen van een app op Azure-containers en Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Ziet u hoe Contoso migreert en rearchitects hun SmartHotel app naar Azure. Ze opnieuw ontwerpen voor de laag van de web-app als een Windows-container en de app-database in een Azure SQL Database. | Beschikbaar
[Artikel 13: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Ziet u hoe Contoso hun SmartHotel-app met een scala aan mogelijkheden van Azure en -services, waaronder App Services, Azure Kubernetes, Azure Functions, Cognitive services en Cosmos DB opnieuw. | Beschikbaar

Ze moeten alle scenario's voor migratie voltooien in dit artikel die Contoso u alle Infrastructuurelementen stelt. 


## <a name="overview"></a>Overzicht

Voordat u ze kunnen migreren naar Azure, is het essentieel dat Contoso een Azure-infrastructuur bereidt.  Er zijn vijf brede gebieden die ze nodig hebben om na te denken over:

**Stap 1: Azure-abonnementen**: hoe ze Azure koopt, en communiceren met de Azure-platform en services?  
**Stap 2: Hybride identiteit**: hoe worden ze beheren en toegang tot on-premises en Azure-resources beheren na de migratie? Hoe ze uitbreiden of identiteitsbeheer verplaatsen naar de cloud?  
**Stap 3: Herstel na noodgevallen en flexibiliteit**: hoe worden ze ervoor te zorgen dat hun apps en infrastructuur flexibele als uitval en noodgevallen optreedt?  
**Stap 4: Netwerken**: hoe moeten ze uw netwerkinfrastructuur ontwerpen en verbinding tussen hun on-premises datacenter en Azure?  
**Stap 5: Beveiliging**: hoe worden ze hun hybride/Azure-implementatie beveiligen?  
**Stap 6: Governance**: hoe blijven ze hun implementatie die is afgestemd op de vereisten voor beveiliging en governance?

## <a name="before-you-start"></a>Voordat u begint

Voordat we kijken naar de infrastructuur, kunt u sommige achtergrondinformatie over de mogelijkheden van Azure we in dit artikel bespreken lezen:

- Er zijn een aantal opties beschikbaar voor het aanschaffen van Azure toegang, met inbegrip van betalen per gebruik, Enterprise Agreements (EA), of Open Licensing van Microsoft-resellers of van Microsoft-Partners weten als Cloud Solution Providers (CSP's). Meer informatie over [Aankoopopties](https://azure.microsoft.com/pricing/purchase-options/), en meer informatie over hoe u [Azure-abonnementen zijn ingedeeld](https://azure.microsoft.com/blog/organizing-subscriptions-and-resource-groups-within-the-enterprise/).
- Bekijk een overzicht van Azure [identiteits-en toegang](https://www.microsoft.com/en-us/trustcenter/security/identity). In het bijzonder meer informatie over [Azure AD en uitbreiding van on-premises AD naar de cloud](https://docs.microsoft.com/azure/active-directory/identity-fundamentals). Er is een nuttig downloadbare e-book over [identiteits- en toegangsbeheer management (IAM) in een hybride omgeving](https://azure.microsoft.com/resources/hybrid-cloud-identity/).
- Azure biedt een robuuste netwerkinfrastructuur met opties voor hybride verbindingen. Bekijk een overzicht van [netwerk- en toegangsbeheer](https://docs.microsoft.com/azure/security/security-network-overview).
- Een inleiding tot [Azure-beveiliging](https://docs.microsoft.com/azure/security/azure-security), en meer informatie over het maken van een plan voor [governance](https://docs.microsoft.com/azure/security/governance-in-azure).


## <a name="on-premises-architecture"></a>On-premises-architectuur

Hier volgt een diagram van de huidige on-premises infrastructuur voor Contoso.

 ![Contoso-architectuur](./media/contoso-migration-infrastructure/contoso-architecture.png)  

- Contoso heeft een hoofddatacenter zich bevindt in de plaats van New York in het oostelijk deel van de Verenigde Staten.
- Deze hebt u drie extra lokale branches in de Verenigde Staten.
- Het belangrijkste datacenter is verbonden met internet met een fiber metro Ethernet-verbinding (500 mbps).
- Elke vertakking is lokaal verbonden met internet met behulp van zakelijke klasse verbindingen met IPSec VPN-tunnels terug naar het primaire datacenter. Hiermee wordt het hele netwerk permanent zijn verbonden, en optimaliseert de verbinding met internet.
- Het belangrijkste datacenter is volledig gevirtualiseerd met VMware. Ze hebben twee ESXi 6.5-virtualisatiehosts, beheerd door vCenter Server 6.5.
- Contoso maakt gebruik van Active Directory voor identiteits- en DNS-servers in het interne netwerk.
- De domeincontrollers in het datacenter worden uitgevoerd op virtuele VMware-machines. De domeincontrollers op lokale branches uitgevoerd op fysieke servers.


## <a name="step-1-buy-and-subscribe-to-azure"></a>Stap 1: Kopen en zich abonneren op Azure

Contoso moet weten over het aanschaffen van Azure, hoe u abonnementen en hoe u licenties voor services en resources.

### <a name="buy-azure"></a>Azure kopen

Contoso is gaan met een [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/). Dit houdt in dat een monetaire toezegging voor Azure, voldoen om te winnen grote voordelen, waaronder flexibele factureringsopties en geoptimaliseerd prijzen.

- Contoso geschatte wat hun jaarlijkse Azure-uitgaven is. Wanneer ze de overeenkomst ondertekend, worden ze voor het eerste jaar volledig betaald.
- Contoso moet alle toezeggingen gebruiken voordat het jaar verlopen is of ze gaan over de waarde van deze bedragen verloren.
- Als voor een of andere reden die groter zijn dan de toezegging en meer, wordt deze voor het verschil Microsoft factureert.
- Geen kosten berekend boven de toezegging is op de dezelfde tarieven en die in hun overeenkomst. Er zijn geen sancties voor het gebruik.

### <a name="manage-subscriptions"></a>Abonnementen beheren

Contoso moet na betalen voor Azure, om te achterhalen hoe u Azure-abonnementen beheren. Ze hebben een EA en dus geen limiet voor het aantal Azure-abonnementen in te stellen.

- Een Azure Enterprise-inschrijving wordt gedefinieerd hoe de vorm van een bedrijf en Azure-services gebruikt en een bestuursstructuur core definieert.
- Als een eerste stap heeft Contoso vastgesteld dat een structuur (bekend als een enterprise-platform voor hun Enterprise-inschrijving. Ze gebruikt [in dit artikel](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-subscription-governance) om te begrijpen en ontwerpen van een scaffold.
- Contoso heeft nu besloten om een functionele benadering gebruiken om hun abonnementen te beheren.
    - In hun onderneming hebben ze een enkele IT-afdeling die het Azure budget bepaalt. Dit is de enige groep met abonnementen.
    - Ze gaat dit model in de toekomst uitbreiden zodat andere zakelijke groepen kunnen deelnemen aan als afdelingen in de Enterprise-inschrijving.
    - Contoso heeft twee abonnementen, productie en ontwikkeling gestructureerde binnen de IT-afdeling.
    - Als Contoso extra abonnementen in de toekomst vereist, moet ze voor het beheren van toegang, beleid en naleving voor deze abonnementen. Ze kunnen dat doen door de introductie van zijn [Azure beheergroepen](https://docs.microsoft.com/azure/azure-resource-manager/management-groups-overview), als een extra beveiligingslaag boven abonnementen.

    ![Enterprise-structuur](./media/contoso-migration-infrastructure/enterprise-structure.png) 

### <a name="examine-licensing"></a>Controleren-licentieverlening

Met de abonnementen die zijn geconfigureerd, kunt Contoso kijken hun Microsoft-licenties. De strategie voor licentieverlening afhankelijk van de resources die ze willen migreren naar Azure, en hoe Azure-VM's en services zijn ingeschakeld en geïmplementeerd. 

#### <a name="azure-hybrid-benefit"></a>Azure Hybrid Benefit

Bij het implementeren van virtuele machines in Azure, zijn standaard installatiekopieën een licentie die in rekening op Contoso per minuut voor de software die wordt gebruikt. Echter Contoso is een op de lange termijn klant van Microsoft, en EAs bijgehouden en licenties met software assurance (SA) openen. 

Azure Hybrid Benefit biedt een rendabele methode voor het Contoso-migratie, door ze op te slaan om workloads op Azure VM's en SQL Server door converteren of hergebruiken van Windows Server Datacenter en Standard edition-licenties met Software Assurance wordt gedekt. Hiermee schakelt u Contoso voor een lagere op basis van compute-tarief voor virtuele machines en SQL Server. [Meer informatie](https://azure.microsoft.com/pricing/hybrid-benefit/).


#### <a name="license-mobility"></a>Mobiliteit van licenties

License Mobility through Software Assurance geeft klanten met Microsoft Volume Licensing, zoals Contoso de flexibiliteit om in aanmerking komende serverapps met actieve Software Assurance op Azure te implementeren. Dit elimineert de noodzaak nieuwe licenties te kopen. Met geen extra mobiliteitskosten, kunnen eenvoudig bestaande licenties worden geïmplementeerd in Azure. [Meer informatie](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="reserve-instances-for-predictable-workloads"></a>Gereserveerde instanties voor voorspelbare workloads

Voorspelbare workloads zijn die altijd beschikbaar moeten zijn met de machines die worden uitgevoerd. Bijvoorbeeld, line-of-business-apps, zoals een SAP ERP-systeem.  Aan de andere kant zijn onvoorspelbare workloads die variabele. Bijvoorbeeld virtuele machines die zich op tijdens de hoge vraag en op niet-piekuren uitschakelen.

![Gereserveerde instantie](./media/contoso-migration-infrastructure/reserved-instance.png) 

Ruil voor informatie over het gebruik van gereserveerde instanties voor specifieke VM-exemplaren die ze weten dat moeten worden bijgehouden voor grote duur van de tijd krijgt Console een korting van, en capaciteit met prioriteit. Met behulp van [Azure Reserved Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/), samen met Azure Hybrid Benefit, Contoso bespaart tot 82% op de normale betalen per gebruik prijzen (April 2018).


## <a name="step-2-manage-hybrid-identity"></a>Stap 2: Beheren voor hybride identiteit

Verlenen en beheren van gebruikerstoegang tot Azure-resources met beheer van identiteits- en toegangsbeheer (IAM) is een belangrijke stap in het binnenhalen van uw Azure-infrastructuur.  

- Contoso wil hun on-premises Active Directory in de cloud uitbreiden, in plaats van een nieuwe afzonderlijk systeem in Azure bouwen.
- Ze maken een op basis van Azure Active Directory om dit te doen.
- Contoso heeft geen Office 365 aanwezig is, zodat ze nodig hebben voor het inrichten van een nieuwe Azure AD.
- Office 365 maakt gebruik van Azure AD voor beheer van gebruikers. Contoso is Office 365 gebruikt, zouden ze al hebben een Azure AD-grondbeginsel en gebruiken als hun primaire AD.
- [Meer informatie](https://support.office.com/article/understanding-office-365-identity-and-azure-active-directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9) over Azure AD voor Office 365, en leer [toevoegen van een abonnement](https://docs.microsoft.com/azure/active-directory/active-directory-how-subscriptions-associated-directory) met een bestaande Azure AD.

### <a name="create-an-azure-ad"></a>Een Azure AD maken

Contoso maakt gebruik van de gratis versie van Azure AD die is opgenomen met een Azure-abonnement. Voeg een nieuwe AD-map als volgt toe:

1. In de [Azure-portal](http://portal.azure.com/), gaat u naar Contoso **een resource maken** > **identiteit** > **Azure Active Directory**.
2. In **map maken**, ze Geef een naam voor de map, een initiële domeinnaam en de regio waarin de Azure AD-directory moet worden gemaakt.

    ![Azure AD maken](./media/contoso-migration-infrastructure/azure-ad-create.png) 

    > [!NOTE]
    > Bij het maken van de map heeft deze een initiële domeinnaam in het formulier domainname.onmicrosoft.com. De naam kan niet worden gewijzigd of verwijderd. In plaats daarvan moeten ze hun geregistreerde domeinnaam toevoegen aan Azure AD.

### <a name="add-the-domain-name"></a>De domeinnaam toevoegen

Voor het gebruik van hun standaard domeinnaam moet Contoso als een aangepaste naam toevoegen aan Azure AD. Deze optie kan beheerders bekend gebruikersnamen toewijzen. Bijvoorbeeld, een gebruiker zich aanmelden met e-mailadres billg@contoso.com, in plaats van dat billg@contosomigration.onmicrosoft.com. 

Voor het instellen van aangepaste naam ze toe te voegen aan de directory, een DNS-vermelding toevoegen en controleer vervolgens of de naam in Azure AD.

1. In **aangepaste-domeinnamen** > **aangepast domein toevoegen**, zij het domein toevoegen.
2. Ze moeten registreren bij de domeinregistrar voor het gebruik van een DNS-vermelding in Azure. 

    - In de **aangepaste-domeinnamen** lijst, ze de DSN-informatie voor de naam van de opmerking. Contoso maakt gebruik van een MX-vermelding.
    - Ze nodig hebben toegang tot de naamserver om dit te doen. In het geval van Contoso, ze aangemeld bij het domein Contoso.com en gemaakt van een nieuwe MX-record voor de DNS-vermelding die is geleverd door Azure AD, met behulp van de details die u hebt genoteerd.  
1. Nadat de DNS-records worden doorgegeven, in de naam van de details voor het domein, klikt de gebruiker op **controleren** om te controleren of de aangepaste naam.

     ![Azure AD DNS](./media/contoso-migration-infrastructure/azure-ad-dns.png) 

### <a name="set-up-on-premises-and-azure-groups-and-users"></a>Instellen van on-premises en Azure-groepen en gebruikers

Hun Azure AD is actief en werkend, Contoso behoeften om toe te voegen werknemers op lokale AD-groepen die worden gesynchroniseerd naar Azure AD. U wordt aangeraden dat ze on-premises groepsnamen die overeenkomen met de namen van resourcegroepen in Azure gebruiken. Dit maakt het gemakkelijker om te identificeren komt overeen met het oog op synchronisatie.

#### <a name="create-resource-groups-in-azure"></a>Resourcegroepen in Azure maken

Azure-resourcegroepen verzamelen Azure-resources. Met behulp van een resourcegroep-ID, kunt Azure bewerkingen op de resources binnen de groep uit te voeren.

- Een Azure-abonnement kan meerdere resourcegroepen toewijzen, maar een resourcegroep kan alleen bestaan binnen één abonnement.
- Bovendien één resourcegroep bestaan meerdere resources kunt hebben, maar een resource kan alleen deel uitmaken van één groep.

Contoso stelt u de Azure-resourcegroepen, zoals samengevat in de volgende tabel.

**Resourcegroep** | **Details**
--- | ---
**ContosoCobRG** | Deze groep bevat alle resources met betrekking tot de continuïteit van de business (USAB).  Dit omvat kluizen dat voor Contoso wordt gemaakt bij gebruik van de Azure Site Recovery-service en de Azure Backup-service.<br/><br/> Het bevat ook de resources die worden gebruikt voor de migratie, met inbegrip van de Azure Migrate en Database Migration service.
**ContosoDevRG** | Deze groep bevat ontwikkelings- en -resources.
**ContosoFailoverRG** | Deze groep fungeert als een zone landingspagina voor failover van resources.
**ContosoNetworkingRG** | Deze groep bevat alle netwerkresources.
**ContosoRG** | Deze groep bevat resources met betrekking tot de productie-apps en databases.

Ze maken resourcegroepen als volgt:

1. In Azure portal > **resourcegroepen**, ze een groep toevoegen.
2. Ze opgeven voor elke groep een naam, het abonnement waartoe de groep behoort en de regio.
3. Resourcegroepen worden weergegeven in de **resourcegroepen** lijst.

    ![Resourcegroepen](./media/contoso-migration-infrastructure/resource-groups.png) 


#### <a name="create-matching-security-groups-on-premises"></a>Overeenkomende security groepen on-premises maken

1. In hun on-premises Active Directory stelt Contoso u beveiligingsgroepen met namen die overeenkomen met de namen van de Azure-resourcegroepen.
 
    ![On-premises AD-beveiligingsgroepen](./media/contoso-migration-infrastructure/on-prem-ad.png) 

2. Voor beheerdoeleinden maken ze een andere groep die wordt toegevoegd aan alle andere groepen. Deze groep heeft rechten voor alle resourcegroepen in Azure. Een beperkt aantal globale beheerders wordt toegevoegd aan deze groep.

### <a name="synchronize-ad"></a>AD synchroniseren

Contoso wil een algemene identiteit bieden voor toegang tot on-premises bronnen en in de cloud. U doet dit door integreren ze hun on-premises Active Directory met Azure AD. Met dit model:

- Gebruikers en organisaties kunnen profiteren van één identiteit voor toegang tot on-premises toepassingen en cloudservices zoals Office 365, of zelfs duizenden andere sites op internet.
- Beheerders kunnen gebruikmaken van de groepen in AD voor het implementeren van [rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) in Azure.

Integratie in het kader, Contoso maakt gebruik van de [hulpprogramma Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect). Wanneer u installeert en configureert het hulpprogramma op een domeincontroller, het synchroniseren van de lokale on-premises AD-identiteiten met Azure AD. 

### <a name="download-the-tool"></a>Het hulpprogramma downloaden

1. In de Azure-portal, Azure gaat naar **Azure Active Directory** > **Azure AD Connect**, en de meest recente versie van het hulpprogramma wordt gedownload naar de server dat wordt gebruikt voor synchronisatie.

    ![AD Connect downloaden](./media/contoso-migration-infrastructure/download-ad-connect.png) 

2. Ze start de **AzureADConnect.msi** installatie met behulp van **expresinstellingen gebruiken**. Dit is de meest voorkomende installatie en kan worden gebruikt voor een topologie met één forest met wachtwoord-hashsynchronisatie voor verificatie.

    ![AD Connect-Wizard](./media/contoso-migration-infrastructure/ad-connect-wiz1.png) 

3. In **verbinding maken met Azure AD**, ze geeft u de referenties voor het verbinden met de Azure AD (in het formulier CONTOSO\admin of contoso.com\admin).

    ![AD Connect-Wizard](./media/contoso-migration-infrastructure/ad-connect-wiz2.png) 

4. In **verbinding maken met AD DS**, ze Geef referenties op voor hun on-premises AD.

     ![AD Connect-Wizard](./media/contoso-migration-infrastructure/ad-connect-wiz3.png) 

5. In **klaar om te configureren**, klikt de gebruiker op **Start het synchronisatieproces wanneer de configuratie is voltooid** onmiddellijk de synchronisatie starten. Ze installeren.

Houd rekening met het volgende:
- Contoso heeft een directe verbinding met Azure. Als uw on-premises AD zich achter een proxy, Lees dit [artikel](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-connectivity).
- Na de eerste synchronisatie van on-premises AD-objecten kunnen worden weergegeven in de Azure AD.

    ![On-premises AD in Azure](./media/contoso-migration-infrastructure/on-prem-ad-groups.png) 

- Contoso IT-team wordt in elke groep, op basis van de rol aangegeven.

    ![On-premises AD-leden in Azure](./media/contoso-migration-infrastructure/on-prem-ad-group-members.png) 

### <a name="set-up-rbac"></a>Instellen van RBAC

Azure [Role-Based Access Control (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) kunt u over Geavanceerd toegangsbeheer voor Azure. Met RBAC kunt u alleen de toegangsrechten aan gebruikers verlenen die ze nodig hebben om hun taken uit te voeren. U kunt de juiste RBAC-rol toewijzen aan gebruikers, groepen en toepassingen op het bereikniveau van een. Het bereik van een roltoewijzing kan een abonnement, een resourcegroep of één resource zijn. 

Rollen Contoso nu toegewezen aan de AD-beveiligingsgroepen waartoe deze gesynchroniseerd van on-premises.

1. In de **ControlCobRG** resourcegroep, klikken ze op **toegangsbeheer (IAM)** > **toevoegen**.
2. In **machtigingen toevoegen** > **rol**, selecteren ze **Inzender**, en selecteer de **ContosoCobRG** AD-groep in de lijst. De groep wordt vervolgens weergegeven in **geselecteerde leden** lijst. 
3. Ze Herhaal deze stap met dezelfde machtigingen voor de andere resourcegroepen (met uitzondering van **ContosoAzureAdmins**), door de Inzender-rechten toe te voegen aan de AD-account dat overeenkomt met de resourcegroep.
4. Voor de **ContosoAzureAdmins** AD-groep, ze wijzen de **eigenaar** rol.

    ![On-premises AD-leden in Azure](./media/contoso-migration-infrastructure/on-prem-ad-groups.png) 


## <a name="step-3-design-for-resilience-and-disaster"></a>Stap 3: Ontwerpen voor tolerantie en herstel na noodgeval

Azure-resources worden geïmplementeerd in de regio's.
- Regio's zijn geordend in geografieën en gegevenslocatie, soevereiniteit, naleving en tolerantie vereisten binnen geografische grenzen worden herkend.
- Een regio bestaat uit een set datacenters. Deze datacenters worden geïmplementeerd in een latentie gedefinieerde perimeter en verbonden via een toegewezen regionaal netwerk voor lage latentie.
- Elke Azure-regio is gekoppeld aan een andere regio voor tolerantie.
- Meer informatie over [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/), en krijg inzicht in [hoe regio's zijn gekoppeld](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).


Contoso heeft besloten om te gaan met de VS-Oost 2 (te vinden in Virginia) als de primaire regio en VS-midden als de secundaire regio. Er zijn een aantal redenen:

- De Contoso-datacenter bevindt zich in New York bevinden en deze in overweging genomen latentie naar de dichtstbijzijnde datacentrum.
- De regio VS-Oost 2 heeft de service en producten die ze nodig hebben. Niet alle Azure-regio's zijn hetzelfde in termen van de producten en services die beschikbaar zijn. U kunt bekijken [Azure-producten per regio](https://azure.microsoft.com/global-infrastructure/services/).
- VS-midden, is de gekoppelde Azure-regio VS-Oost 2.

Als zij over hun hybride omgeving denken, moet Contoso Houd rekening met het maken van veerkracht en een strategie voor noodherstel in hun ontwerp van uw regio. Ruime zin is strategieën voor het bereik van een implementatie met één regio, die op Azure-platform functies zoals de domeinen met fouten en regionale koppelen voor flexibiliteit vertrouwt, door naar een volledig actief / actief-model in welke cloudservices en -database geïmplementeerd en onderhoud zijn gebruikers van twee regio's.

Contoso heeft besloten om een middelste weg te nemen. Ze implementeren hun apps en resources in een primaire regio, en een volledige infrastructuur niet behouden in de secundaire regio, zodat deze klaar is om te fungeren als een volledige back-up in het geval van noodherstel voor volledige app of een storing van de regio.


## <a name="step-4-design-a-network-infrastructure"></a>Stap 4: Een netwerkinfrastructuur ontwerpen

Contoso is gereed om te overwegen een strategie voor netwerken met hun ontwerp regio in plaats. Ze moeten nadenken over hun on-premises datacenter en Azure verbinding maakt en met elkaar communiceren, en over het ontwerpen van uw netwerkinfrastructuur in Azure. Ze moeten in het bijzonder naar:

**Hybride verbinding met het netwerk van plan bent**: bepalen hoe ze gaan om netwerken te verbinden via on-premises en Azure.
**Een Azure-netwerk-infrastructuur ontwerpen**: bepalen hoe ze netwerken via hun regio gaat implementeren. Hoe communiceert netwerken binnen dezelfde regio en tussen regio's.
**Ontwerpen en Azure-netwerken instellen**: Azure-netwerken en subnetten, instellen en beslissen wat erin worden geplaatst.

### <a name="plan-hybrid-network-connectivity"></a>Plannen van hybride verbinding met het netwerk

Contoso beschouwd als een [aantal architecturen](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/) voor hybride netwerken tussen Azure en hun on-premises datacentrum. [Lees meer](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/considerations) over het vergelijken van opties.

Als een herinnering Contoso on-premises netwerkinfrastructuur bestaat momenteel uit hun datacenter in New York en lokale branches in het oostelijk deel van de Verenigde Staten.  Alle locaties business klasse verbinding hebben met internet.  Elk van de branches is vervolgens naar het datacenter via een IPSec VPN-tunnel via internet verbonden.

![Contoso-netwerk](./media/contoso-migration-infrastructure/contoso-networking.png) 

Hier ziet u hoe Contoso besloten voor het implementeren van hybride verbindingen:

1. Instellen van een nieuwe site-naar-site VPN-verbinding tussen de Contoso-datacenter in New York en twee Azure-regio's VS-Oost 2 en VS-midden.
2. Branch office verkeer dat is gebonden voor virtuele Azure-netwerken wordt gerouteerd via het hoofddatacenter van Contoso. 
3. Als ze worden geschaald van hun Azure-implementatie, moeten deze een ExpressRoute-verbinding tussen hun datacenter en de Azure-regio's tot stand brengen. Als dit gebeurt, wordt ze de site-naar-site VPN-verbinding voor failover uitsluitend behouden.
    - [Meer informatie](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/considerations) over het kiezen tussen een hybride oplossing van VPN en ExpressRoute.
    - Controleer of [ExpressRoute-locaties en ondersteuning](https://docs.microsoft.com/azure/expressroute/expressroute-locations-providers).


**Alleen VPN**

![Contoso VPN](./media/contoso-migration-infrastructure/hybrid-vpn.png) 


**VPN en ExpressRoute**

![Contoso VPN/ExpressRoute](./media/contoso-migration-infrastructure/hybrid-vpn-expressroute.png) 


### <a name="design-the-azure-network-infrastructure"></a>De Azure-netwerk-infrastructuur ontwerpen

Het is essentieel dat de Contoso netwerken in plaats op een manier die hun hybride implementatie, veilige en schaalbare maakt geplaatst. U doet dit door Contoso op lange termijn benadering en het ontwerpen van virtuele netwerken (VNets) veerkracht en geschikt voor bedrijven. [Meer informatie](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm) over het plannen van vnet's.

Contoso heeft verbinding willen maken hun twee regio's, besloten om een model van de virtuele hub-hub-netwerk implementeren:

- Binnen elke regio wordt Contoso een hub en spoke-model gebruiken.
- Contoso gebruikt om te verbinden van netwerken en hubs, Azure-netwerk-peering.

#### <a name="network-peering"></a>Netwerkpeering

Azure biedt een netwerkpeering voor het verbinding maken met VNets en hubs. Wereldwijde peering, kunt verbindingen tussen VNets/hubs in verschillende regio's. Lokale peering maakt verbinding met VNets in dezelfde regio. VNet-peering bieden een aantal voordelen:

- Netwerkverkeer tussen gekoppelde VNets is privé.
- Verkeer tussen de VNets wordt opgeslagen in de Microsoft-backbone-netwerk. Er zijn geen openbare internet, gateways of versleuteling is vereist in de communicatie tussen de VNets.
- Peering biedt een standaard, verbinding met lage latentie en hoge bandbreedte tussen resources in verschillende VNets.

[Meer informatie](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) over de netwerkpeering.

#### <a name="hub-to-hub-across-regions"></a>Hub-naar-hub in regio 's

Contoso implementeert een hub in elke regio. Een hub is een virtueel netwerk (VNet) in Azure die als een centraal punt van connectiviteit naar uw on-premises netwerk fungeert. De hub vnet's maakt verbinding met elkaar met behulp van wereldwijde VNet-peering. Wereldwijde VNet-peering verbindt VNets Azure-regio's.

- De hub in elke regio is gekoppeld aan de partner-hub in de andere regio.
- De hub is gekoppeld aan elk netwerk in de regio, en kan verbinding maken met alle netwerkbronnen.

    ![Wereldwijde peering](./media/contoso-migration-infrastructure/global-peering.png) 

#### <a name="hub-and-spoke-within-a-region"></a>Hub en spoke-binnen een regio

Binnen elke regio implementeert Contoso VNets voor verschillende doeleinden als spoke-netwerken van de hub regio. Vnet's binnen een regio gebruiken peering om naar de hub, en met elkaar te verbinden.

#### <a name="design-the-hub-network"></a>Ontwerp het hub-netwerk

In de hub en spoke-model die Contoso heeft gekozen, die ze nodig hebt om na te denken over het verkeer van hun on-premises datacenter en vanaf het internet, worden doorgestuurd. Hier ziet u hoe Contoso heeft besloten om te zorgen voor de routering voor de VS-Oost 2 en VS-midden hubs:

- Ze het ontwerpen van een netwerk bekend als 'omgekeerde c', omdat dit het pad dat de pakketten van het netwerk inkomend in uitgaande volgen.
- De netwerkarchitectuur heeft twee grenzen, een niet-vertrouwde front-end perimeter-zone en een zone met vertrouwde back-end.
- Een firewall hebben een netwerkadapter in elke zone beheren van toegang tot vertrouwde zones.
- Vanaf het internet:
    - Internetverkeer wordt bereikt met load balancing openbaar IP-adres in het perimeternetwerk.
    - Dit verkeer wordt doorgestuurd via de firewall en firewall-regels.
    - Nadat netwerktoegangsbeheer zijn geïmplementeerd, wordt verkeer doorgestuurd naar de gewenste locatie in de vertrouwde zone.
    - Uitgaand verkeer van het VNet wordt gerouteerd naar internet met behulp van de gebruiker gedefinieerde routes (udr's). Het verkeer wordt afgedwongen via de firewall en gecontroleerd in overeenstemming met de Contoso-beleid.
- Vanuit het datacenter Contoso:
    - Inkomend verkeer via site-naar-site VPN-(of ExpressRoute) komt binnen via het openbare IP-adres van de Azure VPN-gateway.
    - Verkeer wordt doorgestuurd via de firewall en kan worden firewall-regels.
    - Na het toepassen van regels wordt verkeer doorgestuurd naar een interne load balancer (Standard-SKU) op de interne zone met vertrouwde-subnet.
    - Uitgaand verkeer van de vertrouwde subnet naar het on-premises datacenter via VPN wordt doorgestuurd via de firewall en regels die zijn toegepast, voordat u doorgaat op de site-naar-site VPN-verbinding.



### <a name="design-and-set-up-azure-networks"></a>Ontwerpen en Azure-netwerken instellen

Contoso is gereed voor het instellen van hun Azure-netwerken en subnetten met een netwerk en de routeringstopologie in plaats.

- Contoso wordt een particulier netwerk van klasse A implementeren in Azure (0.0.0.0-127.255.255.255). Dit werkt, sinds de on-premises ze huidige een klasse B-privéadres ruimte 172.160.0/16 hebben zodat ze ervoor dat er zich een overlapping tussen adresbereiken niet kunnen worden.
- Ze willen implementeren van vnet's in hun primaire en secundaire regio.
- Gebruik een naamgevingsconventie die het voorvoegsel bevat **VNET** en de regio afkorting **EUS2** of **Cu's**. Met behulp van deze standaard, worden de hub networks benoemde **VNET-HUB-EUS2** (VS-Oost 2), en **VNET-HUB-CUS** (VS centraal).
- Contoso beschikt niet over een [IPAM oplossing](https://docs.microsoft.com/windows-server/networking/technologies/ipam/ipam-top), zodat ze nodig hebben om te plannen voor de routering zonder NAT bevinden.


#### <a name="virtual-networks-in-east-us-2"></a>Virtuele netwerken in VS-Oost 2

VS-Oost 2 is de primaire regio die Contoso gebruiken voor het implementeren van resources en services. Hier volgt hoe netwerken architect ze gaan:

- **Hub**: de hub VNet in VS-Oost 2 is het middelpunt van primaire verbinding met hun on-premises datacentrum.
- **VNets**: knooppunt VNets in VS-Oost 2 kunnen worden gebruikt om workloads te isoleren indien nodig. Contoso heeft twee knooppunt VNets in VS-Oost 2, naast de Hub VNet, doen:
    - **VNET-DEV-EUS2**. Dit VNet vindt u dat de ontwikkeling en het testteam wordt een volledig werkend netwerk voor ontwikkelprojecten. Het fungeert als een leider gebied van productie, en zal afhankelijk zijn van de productie-infrastructuur om te functie.
    - **VNET-PROD-EUS2**: Azure IaaS productieonderdelen bevindt zich in dit netwerk. 
    -  Elk VNet heeft een eigen unieke adresruimte, met elkaar niet overlappen. Ze willen routering zonder NAT configureren
- **Subnetten**:
    - Er is een subnet in elk netwerk voor elke app-laag
    - Elk subnet in het productienetwerk heeft een overeenkomend subnet in het Dev-VNet.
    - Het productienetwerk is bovendien een subnetmasker voor domeincontrollers.

Vnet's in VS-Oost 2 worden samengevat in de volgende tabel.

**VNet** | **Bereik** | **Peer**
--- | --- | ---
**VNET-HUB-EUS2** | 10.240.0.0/20 | VNET-HUB-CUS2, VNET-DEV-EUS2, VNET-PROD-EUS2
**VNET-DEV-EUS2** | 10.245.16.0/20 | VNET-HUB-EUS2
**VNET-PROD-EUS2** | 10.245.32.0/20 | VNET-HUB-EUS2, VNET-PROD-CU 'S

![Ster in primaire regio](./media/contoso-migration-infrastructure/primary-hub-peer.png) 


#### <a name="subnets-in-the-east-us-2-hub-network-vnet-hub-eus2"></a>Subnetten in het oostelijke VS 2 Hub-netwerk (VNET-HUB-EUS2)

**Subnet/zone** | **CIDR** | ** Bruikbare IP-adressen
--- | --- | ---
**IB-UntrustZone** | 10.240.0.0/24 | 251
**IB-TrustZone** | 10.240.1.0/24 | 251
**O UntrustZone** | 10.240.2.0/24 | 251
**O TrustZone** | 10.240.3.0/24 | 251
**GatewaySubnets** | 10.240.10.0/24 | 251


#### <a name="subnets-in-the-east-us-2-dev-network-vnet-dev-eus2"></a>Subnetten in het oostelijke VS 2 Dev-netwerk (VNET-DEV-EUS2)

Het Dev VNet wordt door het ontwikkelingsteam gebruikt als een leider gebied van productie. Er worden drie subnetten.

**Subnet** | **CIDR** | **Adressen** | **In het subnet**
--- | --- | --- | ---
**DEV-FE-EUS2** | 10.245.16.0/22 | 1019 | Web-front-ends laag virtuele machines
**DEV-APP-EUS2** | 10.245.20.0/22 | 1019 | App-laag virtuele machines
**DEV-DB-EUS2** | 10.245.24.0/23 | 507 | Database-VM 's


#### <a name="subnets-in-the-east-us-2-production-network-vnet-prod-eus2"></a>Subnetten in het oostelijke VS 2 productie-netwerk (VNET-PROD-EUS2)

Onderdelen van Azure IaaS bevinden zich in het productienetwerk. Elke app-laag heeft een eigen subnet. Subnetten overeen met die in het netwerk ontwikkelen met de toevoeging van een subnetmasker voor domeincontrollers.

**Subnet** | **CIDR** | **Adressen** | **In het subnet**
--- | --- | --- | ---
**PROD-FE-EUS2** | 10.245.32.0/22 | 1019 | Web-front-ends laag virtuele machines
**PRODUCTIE-APP-EUS2** | 10.245.36.0/22 | 1019 | App-laag virtuele machines
**PROD-DB-EUS2** | 10.245.40.0/23 | 507 | Database-VM 's
**PROD-DC-EUS2** | 10.245.42.0/23 | 251 | Domeincontroller VM 's


![Hub-netwerkarchitectuur](./media/contoso-migration-infrastructure/azure-networks-eus2.png)


#### <a name="virtual-networks-in-central-us-secondary-region"></a>Virtuele netwerken in VS-midden (secundaire regio)

VS-midden is de secundaire regio van Contoso. Hier volgt hoe netwerken architect ze gaan:

- **Hub**: de hub Vnet in VS-Oost 2 het middelpunt van connectiviteit met hun on-premises datacentrum en het knooppunt VNets in VS-Oost 2 kunnen worden gebruikt om te isoleren werkbelastingen is indien nodig, afzonderlijk beheerd vanaf andere knooppunten.
- **VNets**: ze heeft twee VNets in VS-midden:
    - VNET-PROD-CU 'S. Dit VNet is een productienetwerk, vergelijkbaar met VNET-PROD_EUS2. 
    - VNET-ASR-CU 'S. Dit VNet fungeert als een locatie waarin de virtuele machines die zijn gemaakt na een failover van on-premises of als een locatie voor Azure-VM's waarvoor een failover van de primaire naar de secundaire regio is. Dit netwerk is vergelijkbaar met de productienetwerken, maar zonder dat alle domeincontrollers erop.
    -  Elk VNet in de regio heeft een eigen adresruimte, met elkaar niet overlappen. Ze willen routering zonder NAT configureren
- **Subnetten**: de subnetten wordt op een soortgelijke manier om deze in VS-Oost 2 te worden ontworpen. De uitzondering is dat ze geen subnet nodig voor domeincontrollers.

De VNets in VS centraal worden samengevat in de volgende tabel.

**VNet** | **Bereik** | **Peer**
--- | --- | ---
**VNET-HUB-CU 'S** | 10.250.0.0/20 | VNET-HUB-EUS2, VNET-ASR-CU 'S, VNET-PROD-CU 'S
**VNET-ASR-CU 'S** | 10.255.16.0/20 | VNET-HUB-CU 'S, VNET-PROD-CU 'S
**VNET-PROD-CU 'S** | 10.255.32.0/20 | VNET-HUB-CU 'S, VNET-ASR-CU 'S, VNET-PROD-EUS2  


![Ster in gekoppelde regio](./media/contoso-migration-infrastructure/paired-hub-peer.png)


#### <a name="subnets-in-the--central-us-hub-network-vnet-hub-cus"></a>Subnetten in het centrale Hub ons netwerk (VNET-HUB-CUS)

**Subnet** | **CIDR** | **Bruikbare IP-adressen**
--- | --- | ---
**IB-UntrustZone** | 10.250.0.0/24 | 251
**IB-TrustZone** | 10.250.1.0/24 | 251
**O UntrustZone** | 10.250.2.0/24 | 251
**O TrustZone** | 10.250.3.0/24 | 251
**GatewaySubnet** | 10.250.2.0/24 | 251


#### <a name="subnets-in-the-central-us-production-network-vnet-prod-cus"></a>Subnetten in het centrale productie ons netwerk (VNET-PROD-CUS)

Parallel met het productienetwerk in de primaire regio in de VS-Oost 2, moet u er een productienetwerk is in de secundaire regio VS-midden. 

**Subnet** | **CIDR** | **Adressen** | **In het subnet**
--- | --- | --- | ---
**PROD-FE-CU 'S** | 10.255.32.0/22 | 1019 | Web-front-ends laag virtuele machines
**PRODUCTIE-APP-CU 'S** | 10.255.36.0/22 | 1019 | App-laag virtuele machines
**PROD-DB-CU 'S** | 10.255.40.0/23 | 507 | Database-VM 's
**PROD-DC-CU 'S** | 10.255.42.0/24 | 251 | Domeincontroller VM 's

#### <a name="subnets-in-the-central-us-failoverrecovery-network-in-central-us-vnet-asr-cus"></a>Subnetten in de VS-midden failoverherstel/netwerk in VS centraal (CUS-ASR-VNET)

De VNET-ASR-CUS-netwerk wordt gebruikt ten behoeve van failover tussen regio's. Site Recovery wordt gebruikt voor het repliceren en failover van virtuele Azure-machines tussen de regio's. Het fungeert ook als een Contoso-datacenter met Azure-netwerk voor beveiligde werkbelastingen die on-premises blijven, maar schakelt over naar Azure voor herstel na noodgevallen.

VNET-ASR-Cu's is standaard hetzelfde subnet als de productie VNET in VS-Oost 2, maar zonder de noodzaak van een domein-controller-subnet.

**Subnet** | **CIDR** | **Adressen** | **In het subnet**
--- | --- | --- | ---
**ASR-FE-CU 'S** | 10.255.16.0/22 | 1019 | Web-front-ends laag virtuele machines
**ASR-APP-CU 'S** | 10.255.20.0/22 | 1019 | App-laag virtuele machines
**ASR-DB-CU 'S** | 10.255.24.0/23 | 507 | Database-VM 's

![Hub-netwerkarchitectuur](./media/contoso-migration-infrastructure/azure-networks-cus.png)

#### <a name="configure-peered-connections"></a>Gekoppelde verbindingen configureren

De hub in elke regio zal worden gekoppeld aan de hub in de andere regio en alle vnet's binnen de hubregio. Hierdoor hubs om te communiceren en om alle vnet's binnen een regio weer te geven. Houd rekening met het volgende:

- Peering, maakt een tweezijdige-verbinding. Een van de initiërende peer op de eerste VNet en een andere naam op het tweede VNet.
- In een hybride implementatie moet het verkeer dat wordt doorgegeven tussen peers zien door de VPN-verbinding tussen de on-premises datacenter en Azure. U kunt deze inschakelen, zijn er enkele specifieke instellingen die moeten worden ingesteld op gekoppelde verbindingen.

Voor verbindingen vanuit het knooppunt VNets via de hub aan de on-premises datacentrum moet Contoso verkeer toe te staan om te worden doorgestuurd en transversale de VPN-gateways.

##### <a name="domain-controller"></a>Domeincontroller

Voor de domeincontrollers in het netwerk VNET-PROD-EUS2 wil Contoso verkeer toe tussen het netwerk van de hub/productie EUS2, zowel via de VPN-verbinding met on-premises. Om dit te doen die ze nodig hebben om toe te staan de volgende:

1. **Doorgestuurd verkeer toestaan** en **configuraties voor gateway-doorvoer toestaan** op de gekoppelde verbinding. In ons voorbeeld zou dit de VNET-HUB-EUS2 voor VNET-PROD-EUS2 verbinding.

    ![Peering](./media/contoso-migration-infrastructure/peering1.png)

2. **Doorgestuurd verkeer toestaan** en **externe gateways gebruiken** aan de andere kant van de peering, op de VNET-PROD-EUS2 voor VNET-HUB-EUS2 verbinding.

    ![Peering](./media/contoso-migration-infrastructure/peering2.png)

3. On-premises ze hebben een statische route die zorgt ervoor het lokale verkeer dat routeren via de VPN-tunnel naar het VNet wordt ingesteld. De configuratie van de zou worden uitgevoerd op de gateway waarmee de VPN-tunnel van Contoso naar Azure. Contoso maakt gebruik van Windows Routering en externe toegang hiervoor.

    ![Peering](./media/contoso-migration-infrastructure/peering3.png)

##### <a name="production-networks"></a>Productienetwerken 

Een spaken peer-netwerk kan een spaken peer-netwerk in een andere regio via een hub niet zien.

Voor het Contoso productienetwerken in beide regio's om te zien van elkaar worden verbonden, moeten ze een directe verbinding van de gekoppelde voor VNET-PROD-EUS2 en hergebruik-PROD-Cu's maken. 

![Peering](./media/contoso-migration-infrastructure/peering4.png)

### <a name="set-up-dns"></a>Instellen van DNS

Wanneer u resources in virtuele netwerken implementeert, hebt u een aantal opties voor het omzetten van domein. U kunt door Azure geleverd naamomzetting gebruiken of DNS-servers bieden voor resolutie. Het type van de naamomzetting die u gebruikt, is afhankelijk van hoe uw resources moeten met elkaar communiceren. Ophalen [informatie](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#azure-provided-name-resolution) over de Azure DNS-service.

Contoso hebt besloten dat de Azure DNS-service een goede keuze in hun hybride omgeving niet. In plaats daarvan, gaan ze gebruikmaken van hun on-premises DNS-servers.

- Aangezien dit een hybride netwerk dat alle de virtuele machines on-premises en in Azure moeten kunnen omzetten van namen te laten functioneren. Dit betekent dat aangepaste DNS-instellingen moeten worden toegepast op alle vnet's.
- Contoso heeft momenteel DC's die zijn geïmplementeerd in de Contoso-datacenter en in de filialen. De primaire DNS-servers zijn CONTOSODC1(172.16.0.10) en CONTOSODC2(172.16.0.1)
- Wanneer de VNets die zijn geïmplementeerd, wordt de on-premises domeincontrollers worden ingesteld als DNS-server in de netwerken moet worden gebruikt. 
- Als u wilt configureren, bij het gebruik van aangepaste DNS in het VNet, moet de recursieve resolvers IP-adres (zoals 168.63.129.16) van Azure worden toegevoegd aan de lijst met DNS.  U doet dit door configureren Contoso DNS-serverinstellingen op elke VNet. De aangepaste DNS-instellingen voor de VNET-HUB-EUS2 netwerk zou bijvoorbeeld als volgt zijn:
    
    ![Aangepaste DNS](./media/contoso-migration-infrastructure/custom-dns.png)

Naast hun on-premises-domeincontrollers, Contoso gaan voor het implementeren van vier meer ter ondersteuning van hun Azure-netwerken, twee voor elke regio. Hier volgt wat ze implementeert in Azure.

**Regio** | **DC** | **VNet** | **Subnet** | **IP-adres**
--- | --- | --- | --- | ---
EUS2 | CONTOSODC3 | VNET-PROD-EUS2 | PROD-DC-EUS2 | 10.245.42.4
EUS2 | CONTOSODC4 | VNET-PROD-EUS2 | PROD-DC-EUS2 | 10.245.42.5
CU 'S | CONTOSODC5 | VNET-PROD-CU 'S | PROD-DC-CU 'S | 10.255.42.4
CU 'S | CONTOSODC6 | VNET-PROD-CU 'S | PROD-DC-CU 'S | 10.255.42.4

Na de implementatie van de on-premises domeincontrollers, moet de Contoso DNS-instellingen voor netwerken in beide regio om op te nemen van de nieuwe domeincontrollers in de lijst met DNS-server bijwerken.



#### <a name="set-up-domain-controllers-in-azure"></a>Instellen van domeincontrollers in Azure

Contoso is na het bijwerken van netwerkinstellingen, gereed voor het bouwen van de domeincontrollers in Azure.

1. In de Azure-portal implementeren ze een nieuwe virtuele machine voor Windows-Server op het betreffende VNet.
2. Beschikbaarheidssets maken ze op elke locatie voor de virtuele machine. Beschikbaarheidssets doen het volgende:
    - Zorg ervoor dat de Azure-infrastructuur worden gescheiden voor de virtuele machines in verschillende infrastructuur in de Azure-regio. 
    -  Hiermee kunt u Contoso in aanmerking komt voor de SLA van 99,95% voor virtuele machines in Azure.  [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets).

    ![Beschikbaarheidsgroep](./media/contoso-migration-infrastructure/availability-group.png) 
3. Nadat de virtuele machine is geïmplementeerd, pen ze de netwerkinterface voor de virtuele machine. Hier ze stellen de privé-IP-adressen op statisch en geef een geldig adres.

    ![VM NIC](./media/contoso-migration-infrastructure/vm-nic.png)

4. Nu dat ze een nieuwe gegevensschijf koppelen aan de virtuele machine. Deze schijf bevat het Active Directory-database en de sysvol-share. 
    - De grootte van de schijf bepaalt het aantal IOP's worden ondersteund.
    - Na verloop van tijd mogelijk de grootte van de schijf te verhogen als de omgeving groeit.
    - Het station mag niet worden ingesteld op lezen/schrijven voor opslaan in cache. Active Directory-databases kan dit niet ondersteund.

     ![Active Directory-schijf](./media/contoso-migration-infrastructure/ad-disk.png)

5. Nadat de schijf is toegevoegd, ze verbinding maken met de virtuele machine via Extern bureaublad en opent u Serverbeheer.
6. Klik vervolgens in **File and Storage Services**, ze de Wizard Nieuw Volume, ervoor te zorgen dat het station opgegeven de letter F: of hoger is worden uitgevoerd op de lokale virtuele machine.

     ![Wizard Nieuw Volume](./media/contoso-migration-infrastructure/volume-wizard.png)

7. In Serverbeheer, het toevoegen van de **Active Directory Domain Services** rol. Ze vervolgens configureren de virtuele machine als een domeincontroller.

      ![Serverfunctie](./media/contoso-migration-infrastructure/server-role.png)  

9. Nadat de virtuele machine is geconfigureerd als een domeincontroller en opnieuw opgestart, worden ze openen van DNS-beheer en de Azure DNS-resolver configureren als een doorstuurserver.  Hiermee wordt de domeincontroller voor het doorsturen van DNS-query's die kan niet worden omgezet in de Azure DNS.

    ![DNS-doorstuurserver](./media/contoso-migration-infrastructure/dns-forwarder.png)

10. Vervolgens nu de aangepaste DNS-instellingen voor elk VNet bijwerken met de juiste domeincontroller voor de VNet-regio. Ze opnemen on-premises DC's in de lijst.

### <a name="set-up-active-directory"></a>Active Directory instellen

AD is een kritieke service in netwerken, en moet correct zijn geconfigureerd. Contoso bouwt AD-sites voor het Contoso-datacenter en de regio's EUS2 en Cu's.  

1. Het maken van twee nieuwe sites (AZURE-EUS2 en AZURE-Cu's), samen met de datacenter-site (ContosoDatacenter).
2. Na het maken van de sites, maken subnetten ze in de sites, zodat deze overeenkomen met de vnet's en het datacenter.

    ![DC-subnetten](./media/contoso-migration-infrastructure/dc-subnets.png)

3. Ze vervolgens maakt u twee site-koppelingen om alles verbinding te maken. De domeincontrollers moeten vervolgens worden verplaatst naar de locatie.

    ![DC-koppelingen](./media/contoso-migration-infrastructure/dc-links.png)

4. Nadat alles is geconfigureerd, wordt de Active Directory-replicatietopologie aanwezig is.
    
    ![DC-replicatie](./media/contoso-migration-infrastructure/ad-resolution.png)

5. Een lijst met domeincontrollers en sites worden met Alles voltooid weergegeven in de on-premises Active Directory-beheercentrum.

    ![AD-beheercentrum](./media/contoso-migration-infrastructure/ad-center.png)

## <a name="step-5-plan-for-governance"></a>Stap 5: Plannen voor beheer

Azure biedt een scala aan besturingselementen governance verschillende services en het Azure-platform. [Lees meer](https://docs.microsoft.com/azure/security/governance-in-azure) voor een basiskennis hebt van de opties.

Zoals ook het configureren van de identiteit en toegangsbeheer, is al Contoso gestart voor het opzetten van sommige aspecten van beheer en beveiliging. Algemeen, zijn er drie gebieden die ze nodig hebben om te overwegen:

- **Beleid**: beleid in Azure is van toepassing en worden afgedwongen regels en effecten voor uw resources, zodat resources voldoen aan de vereisten van het bedrijf en sla's blijven.
- **Hiermee vergrendelt u**: Azure kunt u vergrendelen abonnementen, resourcegroepen en andere resources, zodat ze kunnen alleen worden gewijzigd door degenen met bevoegdheid om dit te doen.
- **Tags**: Resources kunnen worden beheerd, gecontroleerd en beheerd met tags. Metagegevens toevoegen tags aan resources, het aanbieden van informatie over resources of eigenaren.

### <a name="set-up-policies"></a>Beleid instellen

De Azure Policy-service beoordeelt wat uw resources, scannen op die niet compatibel zijn met de beleidsdefinities die u geïmplementeerd hebt. Bijvoorbeeld, is mogelijk een beleid dat alleen bepaalde typen virtuele machines kunt opgeven of resources aan een specifieke tag vereist. 

Azure-beleid een beleidsdefinitie opgeven en de toewijzing van configuratiebeleid opgeven het bereik waarin een beleid moet worden toegepast. Het bereik kan variëren van een beheergroep tot een resourcegroep. [Informatie over](https://docs.microsoft.com/azure/azure-policy/create-manage-policy) over het maken en beheren van beleid.

Aan de slag met een paar van het beleid is Contoso wilt:

- Ze willen een beleid om ervoor te zorgen dat resources kunnen alleen worden geïmplementeerd in de regio's EUS2 en Cu's.
- Ze willen beperken van VM-SKU's naar goedgekeurde SKU's. De bedoeling is om ervoor te zorgen dat dure VM-SKU's worden niet gebruikt.

#### <a name="limit-resources-to-regions"></a>Limiet voor resources in regio 's

Contoso maakt gebruik van de ingebouwde beleidsdefinitie **locaties toegestaan** resource regio's te beperken.

1. Klik in de Azure-portal op **alle Services**, en zoek naar de **beleid**.
2. Selecteer **toewijzingen** > **beleid toewijzen**.
3. Selecteer in de lijst met beleidsregels **locaties toegestaan**.
4. Stel **bereik** op de naam van de Azure-abonnement en selecteert u de twee regio's in de lijst met toegestane.

    ![Beleid voor toegestane regio 's](./media/contoso-migration-infrastructure/policy-region.png)

5. Het beleid is standaard ingesteld met **weigeren**, wat betekent dat als iemand een implementatie is gestart in het abonnement die zich niet in EUS2 of Cu's, mislukt de implementatie. Hier ziet u wat er gebeurt als iemand zich in het abonnement Contoso wil instellen van een implementatie in VS-West.

    ![Beleid is mislukt](./media/contoso-migration-infrastructure/policy-failed.png)

#### <a name="allow-specific-vm-skus"></a>Specifieke VM-SKU's toestaan

Contoso gebruikt de ingebouwde beleidsdefinitie **zodat virtuele machines SKU's** om te beperken van het type van de virtuele machines dat kan worden gemaakt in het abonnement. 

![Beleid SKU](./media/contoso-migration-infrastructure/policy-sku.png)



#### <a name="check-policy-compliance"></a>Controleren op beleidsnaleving

Beleid van kracht onmiddellijk en Contoso-resources voor compatibiliteit kunt controleren. 

1. Klik in de Azure-portal op de **naleving** koppeling.
2. Het dashboard voor apparaatnaleving wordt weergegeven. U kunt inzoomen voor meer informatie.

    ![Beleidsnaleving](./media/contoso-migration-infrastructure/policy-compliance.png)


### <a name="set-up-locks"></a>Vergrendelingen instellen

Contoso heeft lang al gebruikt het ITIL-framework voor het beheer van hun systemen. Een van de belangrijkste aspecten van het framework is het besturingselement wijzigen en Contoso wil ervoor zorgen dat wijzigingsbeheer wordt geïmplementeerd in hun Azure-implementatie.

Contoso is gaan wordt vergrendeld als volgt implementeren:

- Productie- of failover-component moet zich in een resourcegroep met een alleen-lezenvergrendeling.  Dit betekent dat als u wilt wijzigen of verwijderen van de productie-items, de vergrendeling moet worden verwijderd. 
- Niet-productie resourcegroepen hebt CanNotDelete wordt vergrendeld. Dit betekent dat gemachtigde gebruikers kunnen lezen of wijzigen van een resource, maar deze niet verwijderen.

[Meer informatie](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) over wordt vergrendeld.

### <a name="set-up-tagging"></a>Tags instellen

Als u wilt bijhouden resources zoals ze worden toegevoegd, wordt het steeds belangrijker wordt voor Contoso resources koppelen aan een juiste afdeling, de klant en de omgeving zijn. 

Naast het verstrekken van informatie over resources en eigenaren, kunnen labels Contoso te aggregeren en groep resources, en het gebruik van die gegevens voor terugstortingsdoeleinden.

Contoso moet voor het visualiseren van hun Azure-assets op een manier die zinvol is voor hun bedrijf. Bijvoorbeeld, maar de rol of afdeling. Houd er rekening mee dat resources hoeft te bevinden zich in dezelfde resourcegroep voor het delen van een tag. U doet dit door maakt Contoso u een eenvoudige tagtaxonomie zodat iedereen dezelfde tags gebruikt.

**Naam van de tag** | **Waarde**
--- | ---
Kostenplaats | 12345: dit moet een geldige kostenplaats van SAP.
Business Unit | Naam van business unit (van SAP). Komt overeen met kostenplaats.
ApplicationTeam | E-mailalias van het team dat eigenaar is van ondersteuning voor de app.
CatalogName | De naam van de app of ShareServices, per de Servicecatalogus die ondersteuning biedt voor de resource.
Service Manager | E-mailalias van de ITIL Service Manager voor de resource.
COBPriority | De prioriteit is ingesteld door het bedrijf voor BCDR. Waarden van 1-5.
ENV | PROD ontwikkelen, STG, zijn de mogelijke waarden. Die ontwikkeling, fasering en productie.

Bijvoorbeeld: 

 ![Azure-tags](./media/contoso-migration-infrastructure/azure-tag.png)

Contoso wordt na het maken van de tag, gaat u terug en nieuwe Azure policy-definities en toewijzingen, om af te dwingen van het gebruik van de vereiste labels in de hele organisatie maken.


## <a name="step-6-consider-security"></a>Stap 6: Houd rekening met beveiliging

Beveiliging is van cruciaal belang in de cloud en Azure biedt een breed scala aan mogelijkheden en hulpprogramma's voor beveiliging. Dit helpt u bij het maken van veilige oplossingen op de beveiligde Azure-platform. Lezen [vertrouwen in de vertrouwde cloud](https://azure.microsoft.com/overview/trusted-cloud/) voor meer informatie over Azure-beveiliging.

Er enkele belangrijke aspecten voor Contoso te overwegen:

- **Azure Security Center**: Azure Security Center biedt geïntegreerd beveiligingsbeheer en geavanceerde bedreigingsbeveiliging voor hybride cloudworkloads. Met Security Center kunt u beveiligingsbeleid toepassen voor verschillende workloads, blootstelling aan bedreigingen beperken en aanvallen detecteren en afwenden.  [Meer informatie](https://docs.microsoft.com/azure/security-center/security-center-intro).
- **Netwerkbeveiligingsgroepen (nsg's)**: een NSG is een filter (firewall) die een lijst van beveiliging bevat regels die, wanneer toegepast, toestaan of weigeren van netwerkverkeer naar resources die zijn verbonden met Azure vnet's. [Meer informatie](https://docs.microsoft.com/azure/virtual-network/security-overview).
- **Gegevensversleuteling**: Azure Disk Encryption is een functie waarmee u uw Windows- en Linux IaaS VM-schijven te versleutelen. [Meer informatie](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest).

### <a name="work-with-the-azure-security-center"></a>Werken met de Azure Security Center

Contoso is op zoek naar een snel inzicht krijgt in de beveiligingsstatus van de nieuwe hybride cloud en specifiek hun Azure-workloads.  Als gevolg hiervan heeft Contoso besloten voor het implementeren van Azure Security Center starten met de volgende functies: 

- Gecentraliseerd beleidsbeheer
- Continue evaluatie
- Uitvoerbare aanbevelingen 

#### <a name="centralize-policy-management"></a>Centraliseer het beleidsbeheer

Met gecentraliseerd beleidsbeheer, Contoso wordt ervoor zorgen dat aan de beveiligingsvereisten door beveiligingsbeleid centraal te beheren binnen de gehele omgeving. Ze kunnen eenvoudig en snel een beleid dat van toepassing op alle van de Azure-resources implementeren.

![Beveiligingsbeleid](./media/contoso-migration-infrastructure/security-policy.png)

#### <a name="assess-and-action"></a>Beoordelen en de actie

Contoso gebruik van de continue beveiligingsevaluatie waarmee de beveiliging van computers, netwerken, opslag, gegevens en toepassingen, wordt gecontroleerd voor het detecteren van mogelijke beveiligingsproblemen. 

- Security Center analyseert de beveiligingsstatus van compute, infrastructuur en gegevensresources van Contoso en van Azure-apps en services.
- Continue evaluatie kunt het operationele team van Contoso voor het detecteren van mogelijke beveiligingsproblemen, zoals systemen met ontbrekende beveiligingsupdates of netwerkpoorten weergegeven. 
- Contoso wil met name om te controleren of dat alle van de virtuele machines zijn beveiligd. Security Center helpt met dit VM-status controleren en het doen van prioriteit gebaseerde en uitvoerbare aanbevelingen voor het herstellen van beveiligingsproblemen voordat er misbruik wordt gemaakt.

![Bewaking](./media/contoso-migration-infrastructure/monitoring.png)

### <a name="work-with-nsgs"></a>Werken met nsg 's

Contoso beperken van netwerkverkeer tot resources in een virtueel netwerk met behulp van netwerkbeveiligingsgroepen.

- Een netwerkbeveiligingsgroep bevat een lijst met beveiligingsregels die binnenkomend of uitgaand netwerkverkeer toestaan of weigeren op basis van het bron- of doel-IP-adres, de poort en het protocol.
- Wanneer toegepast op een subnet, worden regels worden toegepast op alle resources in het subnet. Naast netwerkinterfaces bevat deze exemplaren van Azure services die zijn geïmplementeerd in het subnet.
- Toepassingsbeveiligingsgroepen (asg's) kunnen u netwerkbeveiliging configureren als een natuurlijke uitbreiding van een app, zodat u kunt een groep VM's en beleidsregels voor netwerkbeveiliging op basis van deze groepen te definiëren.
    - App-beveiligingsgroepen betekenen dat u uw beveiligingsbeleid op grote schaal, zonder handmatig onderhoud van expliciete IP-adressen kunt hergebruiken. Het platform verwerkt de complexiteit van expliciete IP-adressen en meerdere regelsets, zodat u zich kunt richten op uw bedrijfslogica.
    - U kunt een toepassingsbeveiligingsregel opgeven als bron en doel in een beveiligingsregel. Nadat uw beveiligingsbeleid is gedefinieerd, kunt u virtuele machines maken en toewijzen van de VM NIC's aan een groep. 


Contoso wordt een combinatie van nsg's en asg's implementeren. Ze zich zorgen maakt over het beheer van de NSG. Ze bang over overmatig gebruik van nsg's en de complexiteit die van hun personeel kan dit betekenen.  Met dat gegeven in gedachte, hebben ze de twee belangrijkste principals die ze in het algemeen gebruiken vastgesteld:

- Al het verkeer naar en van alle subnetten (Noord-Zuid), zijn onderworpen aan een NSG-regel, met uitzondering van de GatewaySubnets in het Hub-netwerken.
- Alle firewalls of de domeincontroller worden beveiligd door zowel subnet met nsg's en NIC-nsg's.
- Alle productie-Apps hebben asg's die worden toegepast.


Contoso heeft een model van hoe dit voor hun toepassingen eruitziet worden gebouwd.

![Beveiliging](./media/contoso-migration-infrastructure/asg.png)


De nsg's die zijn gekoppeld aan de asg's zullen worden geconfigureerd met minimale bevoegdheden om ervoor te zorgen dat pakketten alleen is toegestaan naar de bestemming van het ene deel van het netwerk kunnen stromen.

**Actie** | **Naam** | **Bron** | **Doel** | **Poort**
--- | --- | --- | --- | --- 
Toestaan | AllowiInternetToFE | VNET-HUB-EUS1/IB-TrustZone | APP1-FE 80, 443
Toestaan | AllowWebToApp | APP1-FE | APP1-DB | 1433
Toestaan | AllowAppToDB | APP1-APP | Alle | Alle
Weigeren | DenyAllInbound | Alle | Alle | Alle

### <a name="encrypt-data"></a>Gegevens versleutelen

Azure Disk Encryption kan worden geïntegreerd met Azure Key Vault beheren en beheren van de schijf-versleutelingssleutels en geheimen in een key vault-abonnement. Het zorgt ervoor dat alle gegevens op de VM-schijven worden versleuteld in rust in Azure storage.  

- Contoso heeft vastgesteld dat versleuteling vereisen voor specifieke virtuele machines.
- Ze gaan versleuteling op virtuele machines met de klant, vertrouwelijk is, of pixels per inch gegevens toe te passen.


## <a name="conclusion"></a>Conclusie

In dit artikel, Contoso instellen van een Azure-infrastructuur en het beleid voor Azure-abonnement, hybride hebt geïdentificeerd, herstel na noodgevallen, netwerken, governance en beveiliging. 

Niet alle Contoso voltooid hier de stappen zijn vereist voor een migratie naar de cloud. In het geval ze wil een netwerkinfrastructuur die kan worden gebruikt voor alle typen migraties plannen en veilig, robuust en schaalbaar. 

Met deze infrastructuur aanwezig kunnen ze verplaatsen van en migratie uitproberen.

## <a name="next-steps"></a>Volgende stappen

Als een eerste migratiescenario Contoso gaat [beoordelen van de twee lagen-app van on-premises SmartHotel voor migratie naar Azure](contoso-migration-assessment.md). 
