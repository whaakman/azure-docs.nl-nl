---
title: Contoso-Set van een infrastructuur voor migratie | Microsoft Docs
description: Meer informatie over hoe Contoso stelt u een Azure-infrastructuur voor migratie naar Azure.
services: azure-migrate
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: bf861dc6317a8cc3a3ed862dfd6c133a1dcbe685
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232369"
---
# <a name="contoso---deploy-a-migration-infrastructure"></a>Contoso - implementeren van een migratie-infrastructuur

In dit artikel wordt gecontroleerd hoe Contoso stelt u een on-premises en Azure-infrastructuur ter voorbereiding voor migratie naar Azure, en voor het uitvoeren van het bedrijf in een hybride omgeving.

- Het is een voorbeeldarchitectuur die specifiek is voor Contoso.
- Of u alle elementen die worden beschreven in het artikel moet is afhankelijk van uw strategie voor migratie. Bijvoorbeeld, als u alleen cloud-systeemeigen apps in Azure maakt, moet u mogelijk een minder complexe netwerken structuur.

Dit document is de tweede reeks artikelen die hoe het fictieve bedrijf dat Contoso migreert lokale bronnen in de Microsoft Azure-cloud-document. De reeks bevat achtergrondinformatie en een set van implementatiescenario's die laat zien hoe u een migratie-infrastructuur, beoordelen de geschiktheid van lokale bronnen voor migratie en verschillende soorten migraties worden uitgevoerd. Scenario's toenemen in complexiteit en worden er nog meer artikelen gedurende een bepaalde periode.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de serie artikelen en de voorbeeld-apps we gebruiken. | Beschikbaar
Artikel 2: Een Azure-infrastructuur (in dit artikel) implementeren | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. De infrastructuur wordt gebruikt voor alle Contoso migratiescenario's. | Beschikbaar
[Artikel 3: Beoordelen lokale bronnen](contoso-migration-assessment.md) | Toont hoe een beoordeling van de lokale twee lagen SmartHotel app uitgevoerd in VMware in Contoso wordt uitgevoerd. Evalueren van de virtuele machines van een app met de [Azure migreren](migrate-overview.md) -service en de app SQL Server-database met de [Azure Database migratie-assistent](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
[Artikel 4: Rehost virtuele machines in Azure en een beheerde SQL-exemplaar](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstreert hoe Contoso de app SmartHotel migreert naar Azure. Migreren van de app frontend virtuele machine met behulp [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database met de [Azure databasemigratie](https://docs.microsoft.com/azure/dms/dms-overview) service om te migreren naar een SQL-exemplaar worden beheerd. | Beschikbaar
[Artikel 5: Rehost voor virtuele machines in Azure](contoso-migration-rehost-vm.md) | Toont hoe Contoso hun app SmartHotel met Site Recovery alleen virtuele machines migreert.
[Artikel 6: Rehost virtuele machines in Azure en SQL Server-beschikbaarheidsgroepen](contoso-migration-rehost-vm-sql-ag.md) | Toont hoe de app SmartHotel in Contoso worden gemigreerd. Deze Site Recovery gebruiken voor het migreren van virtuele machines van de app en de migratie van de Database-service om te migreren van de app-database naar een SQL Server-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een app Linux virtuele machines in Azure Rehost](contoso-migration-rehost-linux-vm.md) | Demonstreert hoe Contoso de osTicket Linux app migreert naar Azure VM's. | Beschikbaar
[Artikel 8: Rehost een Linux-app voor virtuele machines in Azure en Azure MySQL-Server](contoso-migration-rehost-linux-vm-mysql.md) | Demonstreert hoe Contoso de Linux osTicket-app met Site Recovery en MySQL-Workbench om te migreren (back-up en herstel) migreert naar een Azure MySQL Server-exemplaar. | Beschikbaar

In dit artikel die Contoso alle Infrastructuurelementen instellen, moeten ze de migratiescenario's worden voltooid. 


## <a name="overview"></a>Overzicht

Voordat u ze kunnen migreren naar Azure, is het essentieel dat Contoso hun infrastructuur voorbereiden.  Er zijn in het algemeen vijf brede gebieden die ze nodig hebben bij het nadenken over:

**Stap 1: Azure-abonnementen**: hoe worden ze kopen van Azure en communiceren met de Azure-platform en -services?  
**Stap 2: Hybride identiteit**: hoe worden ze beheren en toegang tot on-premises en Azure-resources na de migratie controleren? Hoe ze uitbreiden of identiteitsbeheer verplaatsen naar de cloud?  
**Stap 3: Herstel na noodgevallen en herstelmogelijkheden**: hoe zorgt ze ervoor dat de apps en de infrastructuur robuuste zijn als stroomstoringen en noodsituaties optreedt?  
**Stap 4: Networking**: hoe moeten ze het ontwerpen van uw netwerkinfrastructuur en stel de verbinding tussen hun on-premises datacentrum en Azure?  
**Stap 5: Beveiliging**: hoe worden ze hun hybride/Azure-implementatie beveiligen?  
**Stap 6: Governance**: hoe behoudt ze hun implementatie die is afgestemd op beveiliging en beheeracties vereisten?

## <a name="before-you-start"></a>Voordat u begint

Voordat we kijken naar de infrastructuur, kunt u sommige achtergrondinformatie over de mogelijkheden van Azure in dit artikel besproken lezen:

- Er zijn een aantal opties beschikbaar op de aanschaf van Azure toegang, met inbegrip van betalen naar gebruik, Enterprise overeenkomsten (EA), of Open licenties van Microsoft resellers of van Microsoft-Partners weten als Cloud Solution Providers (CSP's). Meer informatie over [aanschafopties](https://azure.microsoft.com/pricing/purchase-options/), en meer informatie over het [Azure-abonnementen zijn ingedeeld](https://azure.microsoft.com/blog/organizing-subscriptions-and-resource-groups-within-the-enterprise/).
- Een overzicht van Azure [identiteits-en](https://www.microsoft.com/en-us/trustcenter/security/identity). Met name informatie over [Azure AD en uitbreiden van on-premises AD naar de cloud](https://docs.microsoft.com/azure/active-directory/identity-fundamentals). Er is een nuttig downloadbare e-book over [identiteits- en toegangsbeheer management (IAM) in een hybride omgeving](https://azure.microsoft.com/resources/hybrid-cloud-identity/).
- Azure biedt een robuuste netwerkinfrastructuur met opties voor hybride verbindingen. Een overzicht van [netwerk- en toegangsbeheer](https://docs.microsoft.com/azure/security/security-network-overview).
- Maak kennis met [Azure-beveiliging](https://docs.microsoft.com/azure/security/azure-security), en meer informatie over het maken van een plan voor [governance](https://docs.microsoft.com/azure/security/governance-in-azure).


## <a name="on-premises-architecture"></a>Lokale-architectuur

Hier volgt een diagram die de huidige on-premises infrastructuur voor Contoso.

 ![Architectuur van Contoso](./media/contoso-migration-infrastructure/contoso-architecture.png)  

- Contoso heeft één belangrijkste datacenter zich bevinden in de plaats van New York in het oostelijk deel van de Verenigde Staten.
- Drie extra lokale takken hebben ze in de Verenigde Staten.
- Het belangrijkste datacenter is verbonden met internet via een fiber metro ethernet-verbinding (500 mbps).
- Elke vertakking is lokaal verbonden met internet met behulp van zakelijke klasse-verbindingen met IPSec VPN-tunnels terug naar het belangrijkste datacenter. Hierdoor hun gehele netwerk permanent zijn verbonden en optimaliseert de verbinding met internet.
- Het belangrijkste datacenter wordt volledig gevirtualiseerd met VMware. Ze hebben twee ESXi 6.5 virtualisatie-hosts worden beheerd door de vCenter-Server 6.5.
- Contoso maakt gebruik van Active Directory voor identiteitsbeheer en DNS-servers in het interne netwerk.
- Domeincontrollers in het datacenter wordt uitgevoerd op virtuele VMware-machines. De domeincontrollers op lokale vertakkingen uitgevoerd op fysieke servers.


## <a name="step-1-buy-and-subscribe-to-azure"></a>Stap 1: Kopen en zich abonneert op Azure

Contoso moet Azure aanschaffen, het bouwen van abonnementen en services en bronnen licentie te achterhalen.

### <a name="buy-azure"></a>Azure aanschaffen

Contoso gaat met een [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/). Dit houdt in dat een maandbedrag naar Azure, voldoen om te behalen voordelen op, met inbegrip van flexibele opties voor facturering en prijzen geoptimaliseerd.

- Contoso geschatte wat hun jaarlijkse te besteden aan de Azure zijn. Wanneer ze de overeenkomst ondertekend, worden ze voor het eerste jaar volledig betaald.
- Contoso moet alle hun verplichtingen gebruiken voordat het jaar verlopen is of ze de waarde voor deze bedragen gaat verloren.
- Als voor een bepaalde reden ze hun toezegging overschrijden en meer uitgeven, wordt deze in Microsoft worden gefactureerd voor het verschil.
- Eventuele kosten boven de vastlegging worden op de dezelfde tarieven en die in het contract. Er zijn geen sancties limiet overschrijdt.

### <a name="manage-subscriptions"></a>Abonnementen beheren

Nadat u betaalt voor Azure, moet Contoso bepalen hoe hun abonnementen wilt beheren. Hebben ze een EA, en dus geen limiet voor het aantal Azure-abonnementen in te stellen.

- Een Azure Enterprise-inschrijving wordt gedefinieerd hoe de vorm van een bedrijf maakt gebruik van Azure-services en definieert een bestuursstructuur core.
- Als een eerste stap hebt Contoso een structuur (ook wel een enterprise-scaffold voor hun Enterprise-inschrijving vastgesteld. Ze gebruikt [in dit artikel](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-subscription-governance) om te begrijpen en ontwerpen van een scaffold.
- Contoso heeft nu besloten om een functionele benadering gebruiken voor het beheren van hun abonnementen.
    - Binnen hun onderneming hebben ze een enkele IT-afdeling die de Azure budget bepaalt. Dit is de enige groep met abonnementen.
    - Ze je dit model in de toekomst uitbreiden zodat andere zakelijke groepen als afdelingen in de Enterprise-inschrijving nemen kunnen.
    - Contoso heeft twee abonnementen, productie- en gestructureerde binnen de IT-afdeling.
    - Als Contoso aanvullende abonnementen in de toekomst vereist, moet zij toegang, beleid en naleving voor deze abonnementen beheren. Kan deze persoon zich kunt uitvoeren die door de introductie [Azure beheergroepen](https://docs.microsoft.com/azure/azure-resource-manager/management-groups-overview), als een extra laag hierboven abonnementen.

    ![Enterprise-structuur](./media/contoso-migration-infrastructure/enterprise-structure.png) 

### <a name="examine-licensing"></a>Licentieverlening onderzoeken

Met abonnementen die zijn geconfigureerd, kan Contoso bekijken hun Microsoft-volumelicentieovereenkomsten. Een strategie voor hun licentieverlening afhankelijk van de resources die ze willen migreren naar Azure, en hoe Azure VM's en services zijn ingeschakeld en geïmplementeerd. 

#### <a name="azure-hybrid-benefit"></a>Azure Hybrid Benefit

Bij het implementeren van virtuele machines in Azure omvatten standaard installatiekopieën van een licentie die brengt Contoso per minuut voor de software die wordt gebruikt. Echter Contoso is een Microsoft-klant op lange termijn, en onderhoudt EAs en licenties openen met software assurance (SA). 

Azure hybride Benefit biedt een rendabele methode voor migratie van Contoso, doordat ze op te slaan op Azure VM's en SQL Server werkbelastingen door converteren of Windows Server Datacenter en Standard edition-licenties volstaan met Software Assurance hergebruiken. Hiermee schakelt u Contoso een lagere gebaseerde compute-snelheid betalen voor virtuele machines en SQL Server. [Meer informatie](https://azure.microsoft.com/pricing/hybrid-benefit/).


#### <a name="license-mobility"></a>Mobiliteit van licenties

Licentiemobiliteit via Software Assurance biedt klanten Microsoft Volume Licensing zoals Contoso de flexibiliteit om in aanmerking komende server apps met actieve Software Assurance in Azure te implementeren. Hierdoor hoeven nieuwe licenties te kopen. Met geen kosten gekoppeld mobility worden bestaande licenties gemakkelijk geïmplementeerd in Azure. [Meer informatie](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="reserve-instances-for-predictable-workloads"></a>Reserveren van exemplaren voor voorspelbare werkbelastingen

Voorspelbare werkbelastingen zijn die altijd beschikbaar moeten zijn met VM's worden uitgevoerd. Bijvoorbeeld line-of-business-apps, zoals een SAP ERP-systeem.  Aan de andere kant zijn onvoorspelbare werkbelastingen die zijn van de variabele. Bijvoorbeeld virtuele machines die op tijdens hoge eisen en uitgeschakeld op niet-piekuren tijdstippen.

![Gereserveerde instantie](./media/contoso-migration-infrastructure/reserved-instance.png) 

Voor het gereserveerde exemplaren voor specifieke VM-exemplaren die ze weten dat moet worden onderhouden voor grote duur van de tijd gebruikt, krijgt Console zowel een korting en prioriteit capaciteit. Met behulp van [Azure gereserveerde exemplaren](https://azure.microsoft.com/pricing/reserved-vm-instances/), samen met Azure hybride Benefit, Contoso kunt besparen tot 82% korting reguliere betalen naar gebruik (April 2018) prijzen.


## <a name="step-2-manage-hybrid-identity"></a>Stap 2: Beheren van hybride identiteit

Geeft en beheren van gebruikerstoegang tot Azure-resources met identiteits- en toegangsbeheer management (IAM) is een belangrijke stap in het binnenhalen van uw Azure-infrastructuur.  

- Contoso besluiten in hun lokale Active Directory in de cloud uitbreiden in plaats van een nieuwe afzonderlijk systeem in Azure bouwen.
- Het maken van een op basis van Azure Active Directory om dit te doen.
- Contoso niet beschikken over Office 365, zodat ze nodig hebben voor het inrichten van een nieuwe Azure AD.
- Office 365 maakt gebruik van Azure AD voor gebruikersbeheer. Als Contoso Office 365, zou ze al een Azure AD-basisprincipe hebben en gebruiken als hun primaire AD.
- [Meer informatie](https://support.office.com/article/understanding-office-365-identity-and-azure-active-directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9) over Azure AD voor Office 365, en meer informatie over [het toevoegen van een abonnement](https://docs.microsoft.com/azure/active-directory/active-directory-how-subscriptions-associated-directory) naar een bestaande Azure AD.

### <a name="create-an-azure-ad"></a>Maken van een Azure AD

Contoso gebruikmaakt van de gratis Azure AD-editie die is opgenomen met een Azure-abonnement. Een nieuw AD-directory als volgt toevoegen:

1. In de [Azure-portal](http://portal.azure.com/), gaat u naar Contoso **maken van een resource** > **identiteit** > **Azure Active Directory**.
2. In **Directory maken**, ze Geef een naam voor de map, een initiële domeinnaam en de regio waarin de Azure AD-directory moet worden gemaakt.

    ![Azure AD maken](./media/contoso-migration-infrastructure/azure-ad-create.png) 

    > [!NOTE]
    > Maken van de map heeft een initiële domeinnaam in het formulier domainname.onmicrosoft.com. De naam kan niet worden gewijzigd of verwijderd. Ze moeten in plaats daarvan naar hun geregistreerde domeinnaam toevoegen aan Azure AD.

### <a name="add-the-domain-name"></a>De domeinnaam toevoegen

Voor het gebruik van hun standaard domeinnaam moet Contoso als een aangepaste naam toevoegen aan Azure AD. Deze optie kan beheerders bekend gebruikersnamen toewijzen. Bijvoorbeeld, een gebruiker kunt aanmelden met e-mailadres billg@contoso.com, in plaats van dat billg@contosomigration.onmicrosoft.com. 

Voor het instellen van de aangepaste naam ze toe te voegen aan de map, een DNS-vermelding toevoegen en controleer vervolgens de naam in Azure AD.

1. In **aangepaste domeinnamen** > **aangepaste domein toevoegen**, het domein worden toegevoegd.
2. Gebruik een DNS-vermelding in Azure die ze nodig hebben om te registreren bij hun domeinregistrar. 

    - In de **aangepaste domeinnamen** lijst Let op de DNS-gegevens voor de naam. Een MX-vermelding maken gebruik van Contoso.
    - Ze nodig toegang tot de naamserver om dit te doen. In het geval van Contoso, ze vastgelegd in het domein Contoso.com en een nieuwe MX-record voor de DNS-vermelding opgegeven door Azure AD, met de details hebt genoteerd gemaakt.  
1. Nadat de DNS-records worden doorgegeven, in de naam van de details voor het domein, klikt de gebruiker op **controleren** om de aangepaste naam te controleren.

     ![Azure AD-DNS-](./media/contoso-migration-infrastructure/azure-ad-dns.png) 

### <a name="set-up-on-premises-and-azure-groups-and-users"></a>Instellen van on-premises en Azure groepen en gebruikers

Nu dat hun Azure AD actief en werkend is, Contoso moeten toevoegen werknemers van het lokale AD-groepen die worden gesynchroniseerd naar Azure AD. Het is raadzaam om gebruik van lokale namen die overeenkomen met de namen van resourcegroepen in Azure. Dit maakt het gemakkelijker te identificeren komt overeen met het oog op synchronisatie.

#### <a name="create-resource-groups-in-azure"></a>Maken van resourcegroepen in Azure

Azure-resourcegroepen verzamelen Azure-resources. Met behulp van een resource-ID kan Azure bewerkingen uitvoeren op de bronnen in de groep.

- Een Azure-abonnement kan meerdere resourcegroepen hebben, maar een resourcegroep kan alleen binnen een abonnement van één bestaan.
- Bovendien één resourcegroep kan meerdere resources hebben, maar een resource kan slechts aan één groep.

Contoso instellen van Azure-resourcegroepen, zoals samengevat in de volgende tabel.

**Resourcegroep** | **Details**
--- | ---
**ContosoCobRG** | Deze groep bevat alle bronnen die betrekking hebben op de continuïteit van de zakelijke (USAB).  Dit omvat kluizen die Contoso maakt wanneer u de Azure Site Recovery-service en de Azure Backup-service.<br/><br/> Deze omvatten ook de resources die worden gebruikt voor migratie, waaronder de Azure migreren en de Database-Services voor migratie.
**ContosoDevRG** | Deze groep bevat resources voor ontwikkeling en tests.
**ContosoFailoverRG** | Deze groep fungeert als een zone landingspagina voor failover is uitgevoerd resources.
**ContosoNetworkingRG** | Deze groep bevat alle netwerkresources.
**ContosoRG** | Deze groep bevat bronnen die betrekking hebben op de productie-apps en de database.

Ze maken als volgt resourcegroepen:

1. In de Azure portal > **resourcegroepen**, ze een groep toevoegen.
2. Deze opgeven voor elke groep een naam, het abonnement waarvoor de groep behoort en de regio.
3. Resourcegroepen worden weergegeven in de **resourcegroepen** lijst.

    ![Resourcegroepen](./media/contoso-migration-infrastructure/resource-groups.png) 


#### <a name="create-matching-security-groups-on-premises"></a>Overeenkomende beveiliging groepen lokale maken

1. Contoso instellen beveiligingsgroepen met namen die overeenkomen met de namen van de Azure-resourcegroepen in hun lokale Active Directory.
 
    ![Lokale AD-beveiligingsgroepen](./media/contoso-migration-infrastructure/on-prem-ad.png) 

2. Voor beheerdoeleinden maken ze een andere groep die wordt toegevoegd aan alle andere groepen. Deze groep hebben machtigingen voor alle resourcegroepen in Azure. Een beperkt aantal globale beheerders wordt toegevoegd aan deze groep.

### <a name="synchronize-ad"></a>AD synchroniseren

Contoso willen een algemene identiteit bieden voor toegang tot lokale en in de cloud. U doet dit door integreren ze hun lokale Active Directory met Azure AD. Met dit model:

- Gebruikers en organisaties profiteert van één identiteit voor toegang tot on-premises toepassingen en cloudservices zoals Office 365 of duizenden andere sites op internet.
- Beheerders kunnen gebruikmaken van de groepen in AD voor het implementeren van [rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) in Azure.

Gebruik van de Contoso-integratie mogelijk te maken de [Azure AD Connect-hulpprogramma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect). Wanneer u installeert en configureert het hulpprogramma op een domeincontroller, het synchroniseren van de lokale lokale AD identiteiten met Azure AD. 

### <a name="download-the-tool"></a>Het hulpprogramma voor downloaden

1. In de Azure portal, Azure overschakelt naar de **Azure Active Directory** > **Azure AD Connect**, en de meest recente versie van het hulpprogramma wordt gedownload naar de server dat wordt gebruikt voor synchronisatie.

    ![Download AD Connect](./media/contoso-migration-infrastructure/download-ad-connect.png) 

2. Ze start de **AzureADConnect.msi** installatie met behulp van **expresinstellingen gebruiken**. Dit is de installatie van de meest voorkomende en kan worden gebruikt voor een topologie met één forest met synchronisatie van wachtwoordhash voor verificatie.

    ![AD Connect-Wizard](./media/contoso-migration-infrastructure/ad-connect-wiz1.png) 

3. In **verbinding maken met Azure AD**, ze geven de referenties voor het verbinden met de Azure AD (in de vorm CONTOSO\admin of contoso.com\admin).

    ![AD Connect-Wizard](./media/contoso-migration-infrastructure/ad-connect-wiz2.png) 

4. In **verbinding maken met AD DS**, ze Geef referenties op voor hun on-premises AD.

     ![AD Connect-Wizard](./media/contoso-migration-infrastructure/ad-connect-wiz3.png) 

5. In **klaar om te configureren**, klikt de gebruiker op **Start het synchronisatieproces wanneer configuratie is voltooid** onmiddellijk de synchronisatie starten. Ze installeert.


- Contoso heeft een directe verbinding met Azure. Als uw on-premises AD zich achter een proxy, Lees dit [artikel](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-connectivity).
- Na de eerste synchronisatie een on-premises AD-objecten kunnen worden weergegeven in de Azure AD.

    ![On-premises AD in Azure](./media/contoso-migration-infrastructure/on-prem-ad-groups.png) 

- Contoso IT-team worden weergegeven in elke groep, op basis van hun rol.

    ![Lokale AD leden in Azure](./media/contoso-migration-infrastructure/on-prem-ad-group-members.png) 

### <a name="set-up-rbac"></a>Instellen van RBAC

Azure [op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) kunt Geavanceerd toegangsbeheer voor Azure. Met RBAC kunt u alleen de toegangsrechten aan gebruikers verlenen die ze nodig hebben om hun taken uit te voeren. U toewijzen de juiste RBAC-rol aan gebruikers, groepen en toepassingen op een scopeniveau. Het bereik van een roltoewijzing kan dit een abonnement, resourcegroep of één resource. 

Contoso nu rollen toewijzen aan de AD-groepen die ze on-premises gesynchroniseerd.

1. In de **ControlCobRG** resourcegroep, klikt de gebruiker op **toegangsbeheer (IAM)** > **toevoegen**.
2. In **machtigingen toevoegen** > **rol**, selecteren ze **Inzender**, en selecteer de **ContosoCobRG** AD-groep uit de lijst. De groep wordt weergegeven **leden geselecteerd** lijst. 
3. Ze Herhaal deze stap met dezelfde machtigingen voor de resourcegroepen (met uitzondering van **ContosoAzureAdmins**), door de Inzender-rechten toe te voegen aan de AD-account dat overeenkomt met de resourcegroep.
4. Voor de **ContosoAzureAdmins** AD-groep die ze wijzen de **eigenaar** rol.

    ![Lokale AD leden in Azure](./media/contoso-migration-infrastructure/on-prem-ad-groups.png) 


## <a name="step-3-design-for-resilience-and-disaster"></a>Stap 3: Ontwerp voor herstelmogelijkheden en noodherstel

Azure-resources worden binnen de regio's geïmplementeerd.
- Regio's worden ingedeeld in de locaties en vereisten voor de hand van vestigingsplaats, onafhankelijkheid, naleving en tolerantie van gegevens binnen de geografische grenzen worden gehonoreerd.
- Een regio bestaat uit een set van datacenters. Deze datacenters zijn geïmplementeerd in een perimeternetwerk latentie gedefinieerd en verbonden zijn via een speciaal regionale traag netwerk.
- Elke Azure-regio is gekoppeld aan een andere regio voor tolerantie.
- Meer informatie over [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/), en u begrijpt [hoe regio's zijn gekoppeld](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).


Contoso hebt besloten om door te gaan met de VS-Oost 2 (te vinden in Virginia) als hun primaire regio en VS-midden als de secundaire regio. Er zijn een aantal redenen:

- Het Contoso-datacenter bevindt zich in New York en deze beschouwd als latentie naar het dichtstbijzijnde datacenter.
- De regio VS-Oost 2 heeft de service en producten die ze nodig hebben. Niet alle Azure-regio's zijn hetzelfde in termen van de producten en services die beschikbaar zijn. U kunt bekijken [Azure producten per regio](https://azure.microsoft.com/global-infrastructure/services/).
- VS-midden, is de gekoppelde Azure-regio voor VS-Oost 2.

Als ze nadenken over hun hybride omgeving, Contoso moet rekening houden met het bouwen van herstelmogelijkheden en een strategie voor noodherstel in hun ontwerp voor een regio. Ruime zin is strategieën bereik van een implementatie met één regio, die op Azure-platform functies zoals als domeinen met fouten en regionale vertrouwt via voor herstelbaarheid, koppelen aan een volledig actief / actief in welke cloud-services en -database modelleren geïmplementeerd en onderhoud zijn gebruikers uit twee regio's.

Contoso hebt besloten om een middelste weg te nemen. Deze zult implementeren hun apps en resources in een primaire regio en een volledige infrastructuur in de secundaire regio behouden, zodat deze gereed om te fungeren als een volledige back-up in geval van een volledige app na noodgevallen of regio mislukken.


## <a name="step-4-design-a-network-infrastructure"></a>Stap 4: Een netwerkinfrastructuur ontwerpen

Contoso zijn met hun ontwerp regio aanwezig is gereed om te overwegen een strategie voor netwerken. Ze moeten bij het nadenken over hun on-premises datacentrum en Azure verbinding en met elkaar communiceren, en het ontwerp van uw netwerkinfrastructuur in Azure. Specifiek hoeven te worden:

**Hybride netwerkverbinding plannen**: weten hoe ze verbinding maken van netwerken via on-premises en Azure gaat.
**Een Azure-netwerk-infrastructuur ontwerpen**: Bepaal hoe ze netwerken via hun regio's gaat implementeren. Hoe netwerken communiceert binnen dezelfde regio en tussen regio's.
**Ontwerpen en Azure-netwerken instellen**: instellen van Azure-netwerken en subnetten, en bepaal wat wordt opgeslagen in deze.

### <a name="plan-hybrid-network-connectivity"></a>Hybride netwerkverbinding plannen

Contoso beschouwd als een [aantal architecturen](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/) voor hybride netwerken tussen Azure en hun on-premises datacentrum. [Lees meer](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/considerations) over het vergelijken van de opties.

Als een herinnering Contoso on-premises netwerkinfrastructuur bestaat momenteel uit hun datacenter in New York en lokale filialen in het oostelijk deel van de Verenigde Staten.  Alle locaties business klasse verbinding hebben met internet.  Elk van de filialen van is vervolgens naar het datacenter via een tunnel met IPSec VPN via internet verbonden.

![Contoso-netwerk](./media/contoso-migration-infrastructure/contoso-networking.png) 

Hier ziet u hoe Contoso besloten voor het implementeren van hybride verbindingen:

1. Een nieuwe site-naar-site VPN-verbinding tussen het datacenter Contoso in New York en twee Azure-regio's in VS-Oost 2 en VS-midden instellen.
2. Branch office verkeer dat is gebonden voor virtuele netwerken in Azure wordt gerouteerd via het belangrijkste Contoso-datacenter. 
3. Als ze hun Azure-implementatie opschalen, moeten deze een ExpressRoute-verbinding tussen hun datacenter en Azure-regio's tot stand brengen. Als dit gebeurt, wordt ze de site-naar-site VPN-verbinding alleen ter failover behouden.
    - [Meer informatie](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/considerations) kiezen tussen een hybride oplossing van VPN en ExpressRoute.
    - Controleer of [ExpressRoute locaties en ondersteuning](https://docs.microsoft.com/azure/expressroute/expressroute-locations-providers).


**Alleen VPN**

![Contoso VPN-](./media/contoso-migration-infrastructure/hybrid-vpn.png) 


**VPN- en ExpressRoute**

![Contoso VPN en ExpressRoute](./media/contoso-migration-infrastructure/hybrid-vpn-expressroute.png) 


### <a name="design-the-azure-network-infrastructure"></a>Het netwerk van Azure-infrastructuur ontwerpen

Het is essentieel dat Contoso netwerken in de plaats op een manier waardoor hun hybride implementatie veilige en schaalbare plaatst. U doet dit door Contoso duurt een lange termijn benadering en virtuele netwerken (vnet's) worden herstelmogelijkheden en enterprise-ready ontwerpt. [Meer informatie](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm) over het plannen van VNets.

Contoso heeft om verbinding maken met hun twee regio's, besloten voor het implementeren van een model hub-naar-hub netwerk:

- Contoso gebruikt in elke regio een hub en spoke-model.
- Als u wilt verbinden van netwerken en hubs, Contoso netwerk van Azure-peering gebruikt.

#### <a name="network-peering"></a>Netwerk-peering

Azure biedt netwerk peer verbinding VNets en hubs. Globale peering, kunt verbindingen tussen VNets/hubs in verschillende regio's. Lokale peering verbindt VNets in dezelfde regio. VNet-peering bieden een aantal voordelen:

- Er is een privéverbinding netwerkverkeer tussen VNets peer is ingesteld.
- Verkeer tussen de VNets die wordt opgeslagen in het Microsoft-backbone-netwerk. Er is geen openbare internet, gateways of versleuteling is vereist in de communicatie tussen de VNets.
- Peering voorziet in een standaard, lage latentie en hoge bandbreedte verbinding tussen resources in andere vnet's.

[Meer informatie](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) over het netwerk peering.

#### <a name="hub-to-hub-across-regions"></a>Hub-naar-hub tussen regio 's

Contoso implementeert een hub in elke regio. Er is een virtueel netwerk (VNet) in Azure die als een centraal punt voor de verbinding met uw on-premises netwerk fungeert hub. De hub VNets maakt verbinding met elkaar met behulp van globale VNet-peering. Globale VNet-peering verbindt VNets via Azure-regio's.

- De hub in elke regio is ingesteld als peer met de partner-hub in andere regio.
- De hub is ingesteld als peer voor elk netwerk in de regio en verbinding kan maken met alle netwerkbronnen.

    ![Globale peering](./media/contoso-migration-infrastructure/global-peering.png) 

#### <a name="hub-and-spoke-within-a-region"></a>Hub en spoke-binnen een regio

Binnen elke regio implementeert Contoso VNets voor verschillende doeleinden als spoke netwerken van de regio-hub. Vnet's binnen een regio met peering verbinding maken met hun hub en met elkaar.

#### <a name="design-the-hub-network"></a>Het hub-netwerk ontwerpen

In het hub en spoke-model dat Contoso hebt gekozen, ze nodig hebben bij het nadenken over hoe u vanaf hun on-premises datacentrum en vanaf internet verkeer, worden doorgestuurd. Hier ziet u hoe Contoso hebt besloten om af te handelen routering voor de VS-Oost 2 en de VS-midden hubs:

- Ze ontwerpen van een netwerk bekend als 'omgekeerde c', omdat dit het pad dat de pakketten die vanaf het netwerk aan uitgaande inkomende volgen.
- Hun netwerkarchitectuur heeft twee grenzen, een niet-vertrouwde front-perimeter-zone en een back-end vertrouwde zone.
- Een firewall heeft een netwerkadapter in elke zone beheren van toegang tot vertrouwde zones.
- Vanaf het internet:
    - Internetverkeer bereikt een taakverdeling openbare IP-adres in het perimeternetwerk.
    - Dit verkeer wordt doorgestuurd via de firewall, en onderhevig aan de firewall-regels.
    - Nadat de netwerkfuncties toegang zijn geïmplementeerd, wordt verkeer doorgestuurd naar de juiste locatie in de vertrouwde zone.
    - Uitgaand verkeer van het VNet wordt gerouteerd naar internet met behulp van de gebruiker gedefinieerde routes (udr's). Het verkeer wordt afgedwongen via de firewall en geïnspecteerd in overeenstemming met de Contoso-beleid.
- Van het datacenter Contoso:
    - Binnenkomend verkeer via site-naar-site VPN-(of ExpressRoute) treffers in het openbare IP-adres van de Azure VPN-gateway.
    - Verkeer wordt doorgestuurd via de firewall en onderworpen aan de firewall-regels.
    - Na het toepassen van regels wordt verkeer doorgestuurd naar een interne load balancer (standaard SKU) op de zone met vertrouwde interne subnet.
    - Uitgaand verkeer van de vertrouwde subnet naar de on-premises datacentrum via VPN wordt doorgestuurd via de firewall en regels die zijn toegepast, voordat u doorgaat via de site-naar-site VPN-verbinding.



### <a name="design-and-set-up-azure-networks"></a>Ontwerpen en Azure-netwerken instellen

Met een netwerk en routering-topologie in place Contoso gaan hun Azure-netwerken en subnetten instellen.

- Contoso wordt een particulier netwerk van klasse A implementeren in Azure (0.0.0.0-127.255.255.255). Dit werkt, sinds het lokale huidige hebben een klasse B persoonlijk adresruimte 172.160.0/16 zodat ze zeker van te zijn dat niet alle overlapping tussen adresbereiken.
- Ze gaan VNets in hun primaire en secundaire regio's implementeren.
- Gebruik van een naamgevingsconventie met het voorvoegsel **VNET** en de regio afkorting **EUS2** of **CUS**. Deze standaard gebruikt, worden de netwerken hub genoemd **VNET-HUB-EUS2** (VS-Oost 2), en **VNET-HUB-CUS** (VS-midden).
- Contoso beschikt niet over een [IPAM oplossing](https://docs.microsoft.com/windows-server/networking/technologies/ipam/ipam-top), zodat ze nodig hebben om te plannen voor netwerkroutering zonder NAT bevinden.


#### <a name="virtual-networks-in-east-us-2"></a>Virtuele netwerken in VS-Oost 2

VS-Oost 2 is de primaire regio die Contoso gebruiken voor het implementeren van resources en services. Dit is hoe ze gaat architect netwerken:

- **Hub**: de hub VNet in VS-Oost 2 wordt het middelpunt van de primaire verbinding met hun on-premises datacentrum.
- **VNets**: spaak VNets in VS-Oost 2 kan worden gebruikt voor het isoleren van werkbelastingen, indien nodig. Contoso heeft twee spoke VNets in VS-Oost 2 naast het VNet Hub:
    - **VNET-DEV-EUS2**. Dit VNet biedt dat de ontwikkeling en tests team wordt een volledig werkend netwerk voor dev-projecten. Het fungeert als een test-gebied voor productie en vertrouwt op de productie-infrastructuur voor de functie.
    - **VNET-PROD-EUS2**: Azure IaaS productieonderdelen bevindt zich in dit netwerk. 
    -  Elke VNet hebben een eigen unieke adresruimte zonder overlap. Ze willen zonder NAT routering configureren
- **Subnetten**:
    - Er is een subnet in elk netwerk voor elke app-laag
    - Elk subnet in het productienetwerk, een overeenkomend subnet heeft in het Dev VNet.
    - Het productienetwerk is bovendien een subnet voor domeincontrollers.

VNets in VS-Oost 2 worden samengevat in de volgende tabel.

**VNet** | **bereik** | **peer**
--- | --- | ---
**VNET-HUB-EUS2** | 10.240.0.0/20 | VNET-HUB-CUS2, VNET-DEV-EUS2, VNET-PROD-EUS2
**VNET-DEV-EUS2** | 10.245.16.0/20 | VNET-HUB-EUS2
**VNET-PROD-EUS2** | 10.245.32.0/20 | VNET-HUB-EUS2 VNET-PROD-CUS

![Hub/spoke in primaire regio](./media/contoso-migration-infrastructure/primary-hub-peer.png) 


#### <a name="subnets-in-the-east-us-2-hub-network-vnet-hub-eus2"></a>Subnetten in Oost ons 2 Hub-netwerk (VNET-HUB-EUS2)

**Subnet/zone** | **CIDR** | ** Bruikbare IP-adressen
--- | --- | ---
**IB-UntrustZone** | 10.240.0.0/24 | 251
**IB-TrustZone** | 10.240.1.0/24 | 251
**OB UntrustZone** | 10.240.2.0/24 | 251
**OB TrustZone** | 10.240.3.0/24 | 251
**GatewaySubnets** | 10.240.10.0/24 | 251


#### <a name="subnets-in-the-east-us-2-dev-network-vnet-dev-eus2"></a>Subnetten in Oost ons 2 Dev-netwerk (VNET-DEV-EUS2)

Het Dev VNet wordt gebruikt door het ontwikkelingsteam als een gebied van productie-test. Deze heeft drie subnetten.

**Subnet** | **CIDR** | **adressen** | **In het subnet**
--- | --- | --- | ---
**DEV-FE-EUS2** | 10.245.16.0/22 | 1019 | Frontends/weblaag virtuele machines
**DEV-APP-EUS2** | 10.245.20.0/22 | 1019 | Virtuele machines van App-laag
**DEV-DB-EUS2** | 10.245.24.0/23 | 507 | Database-virtuele machines


#### <a name="subnets-in-the-east-us-2-production-network-vnet-prod-eus2"></a>Subnetten in Oost ons 2 productie-netwerk (VNET-PROD-EUS2)

Azure IaaS-onderdelen bevinden zich in het productienetwerk. Elke laag app heeft een eigen subnet. Subnetten overeen met die in het netwerk Dev, klik met de toevoeging van een subnet voor domeincontrollers.

**Subnet** | **CIDR** | **adressen** | **In het subnet**
--- | --- | --- | ---
**PROD-FE-EUS2** | 10.245.32.0/22 | 1019 | Frontends/weblaag virtuele machines
**PROD-APP-EUS2** | 10.245.36.0/22 | 1019 | Virtuele machines van App-laag
**PROD-DB-EUS2** | 10.245.40.0/23 | 507 | Database-virtuele machines
**PROD-DC-EUS2** | 10.245.42.0/23 | 251 | Domeincontroller virtuele machines


![Hub-netwerkarchitectuur](./media/contoso-migration-infrastructure/azure-networks-eus2.png)


#### <a name="virtual-networks-in-central-us-secondary-region"></a>Virtuele netwerken in VS-midden (secundaire regio)

VS-midden, is de secundaire regio van Contoso. Dit is hoe ze gaat architect netwerken:

- **Hub**: de hub Vnet in VS-Oost 2 het middelpunt van de verbinding met hun on-premises datacentrum en de VNets in VS-Oost 2 kan worden gebruikt wordt voor het isoleren werkbelastingen indien nodig, spoke afzonderlijk beheerd vanaf andere spaken.
- **VNets**: twee VNets in VS-midden hebben:
    - VNET-PROD-CUS. Dit VNet is een productienetwerk, vergelijkbaar met VNET PROD_EUS2. 
    - VNET ASR CUS. Dit VNet fungeert als een locatie waarin de virtuele machines die zijn gemaakt na een failover van on-premises of als een locatie voor Azure virtuele machines die worden overgenomen van de primaire naar de secundaire regio. Dit netwerk is vergelijkbaar met de productienetwerken, maar zonder de domeincontrollers erop.
    -  Elke VNet in de regio heeft een eigen adresruimte zonder overlap. Ze willen zonder NAT routering configureren
- **Subnetten**: de subnetten op vergelijkbare wijze als bij die in VS-Oost 2 zal worden ontworpen. De uitzondering hierop is dat ze een subnet niet nodig voor domeincontrollers.

De VNets in VS-midden worden samengevat in de volgende tabel.

**VNet** | **bereik** | **peer**
--- | --- | ---
**VNET-HUB-CUS** | 10.250.0.0/20 | VNET-HUB-EUS2, VNET-ASR-CUS, VNET-PROD-CUS
**VNET-ASR-CUS** | 10.255.16.0/20 | VNET-HUB-CUS PROD-VNET-CUS
**VNET-PROD-CUS** | 10.255.32.0/20 | VNET-HUB-CUS, VNET-ASR-CUS, VNET-PROD-EUS2  


![Hub/spoke in gekoppelde regio](./media/contoso-migration-infrastructure/paired-hub-peer.png)


#### <a name="subnets-in-the--central-us-hub-network-vnet-hub-cus"></a>Subnetten in het centrale Hub ons netwerk (VNET-HUB-CUS)

**Subnet** | **CIDR** | **Bruikbare IP-adressen**
--- | --- | ---
**IB-UntrustZone** | 10.250.0.0/24 | 251
**IB-TrustZone** | 10.250.1.0/24 | 251
**OB UntrustZone** | 10.250.2.0/24 | 251
**OB TrustZone** | 10.250.3.0/24 | 251
**GatewaySubnet** | 10.250.2.0/24 | 251


#### <a name="subnets-in-the-central-us-production-network-vnet-prod-cus"></a>Subnetten in het centrale ons productienetwerk (VNET-PROD-CUS)

Parallel het productienetwerk in de primaire regio van de VS-Oost 2, moet u er een productienetwerk is in de secundaire regio in de VS-midden. 

**Subnet** | **CIDR** | **adressen** | **In het subnet**
--- | --- | --- | ---
**PROD-FE-CUS** | 10.255.32.0/22 | 1019 | Frontends/weblaag virtuele machines
**PROD-APP-CUS** | 10.255.36.0/22 | 1019 | Virtuele machines van App-laag
**PROD-DB-CUS** | 10.255.40.0/23 | 507 | Database-virtuele machines
**PROD-DC-CUS** | 10.255.42.0/24 | 251 | Domeincontroller virtuele machines

#### <a name="subnets-in-the-central-us-failoverrecovery-network-in-central-us-vnet-asr-cus"></a>Subnetten in het netwerk van de failover/herstel VS-midden in VS-midden (VNET ASR CUS)

De VNET-ASR-CUS-netwerk wordt gebruikt voor de doeleinden van failover tussen regio's. Site Recovery wordt gebruikt voor het repliceren en failover van Azure VM's tussen de regio's. Deze fungeert ook als een Contoso-datacenter met Azure-netwerk voor beveiligde werkbelastingen die lokale blijven, maar via failover naar Azure voor herstel na noodgevallen.

VNET-ASR-CUS is basic hetzelfde subnet als de productie VNET in VS-Oost 2, maar zonder de noodzaak van een domeincontroller subnet.

**Subnet** | **CIDR** | **adressen** | **In het subnet**
--- | --- | --- | ---
**ASR-FE-CUS** | 10.255.16.0/22 | 1019 | Frontends/weblaag virtuele machines
**ASR-APP-CUS** | 10.255.20.0/22 | 1019 | Virtuele machines van App-laag
**ASR-DB-CUS** | 10.255.24.0/23 | 507 | Database-virtuele machines

![Hub-netwerkarchitectuur](./media/contoso-migration-infrastructure/azure-networks-cus.png)

#### <a name="configure-peered-connections"></a>Brengen verbindingen configureren

De hub in elke regio wordt peer worden ingesteld voor de hub in andere regio en voor alle vnet's binnen de regio van de hub. Hierdoor hubs om te communiceren en om weer te geven van alle vnet's binnen een regio. Houd rekening met het volgende:

- Peering een dubbelzijdige verbinding maakt. Een van de peer gang worden gezet op het eerste VNet en een andere naam op het tweede VNet.
- In een hybride implementatie moet het verkeer dat wordt doorgegeven tussen peers worden gezien vanaf de VPN-verbinding tussen de on-premises datacentrum en Azure. Om dit mogelijk zijn er enkele specifieke instellingen moeten worden ingesteld voor verbindingen peer is ingesteld.

Contoso moet voor alle verbindingen van spoke VNets via de hub met de on-premises datacentrum, waarmee verkeer naar de doorgestuurde en transverse de VPN-gateways.

##### <a name="domain-controller"></a>Domeincontroller

Contoso wil voor de domeincontrollers in het netwerk PROD-VNET-EUS2 verkeer tussen het EUS2 hub/productienetwerk, zowel via de VPN-verbinding met on-premises. U kunt dit doen nodig ze hebben om toe te staan de volgende:

1. **Toestaan dat verkeer wordt doorgestuurd** en **gateway onderweg configuraties toestaan** op de peered verbinding. In ons voorbeeld zou dit de VNET-HUB-EUS2 naar VNET-PROD-EUS2 verbinding.

    ![Peering](./media/contoso-migration-infrastructure/peering1.png)

2. **Toestaan van doorgestuurd verkeer** en **externe gateways gebruiken** aan de andere kant van de peering, op de VNET-PROD-EUS2 naar VNET-HUB-EUS2 verbinding.

    ![Peering](./media/contoso-migration-infrastructure/peering2.png)

3. Lokale ze een statische route die stuurt het lokale verkeer routeren via de VPN-tunnel naar het VNet wordt ingesteld. De configuratie van de zou worden uitgevoerd op de gateway die voor de VPN-tunnel van Contoso naar Azure biedt. Contoso maakt gebruik van Windows Routing and Remote Access voor deze.

    ![Peering](./media/contoso-migration-infrastructure/peering3.png)

##### <a name="production-networks"></a>Productienetwerken 

Een spaken peer-netwerk kan een spaken peer-netwerk in een andere regio via een hub niet zien.

Voor Contoso productienetwerken in beide regio's om te zien op elkaar, die ze willen maken van een directe verbinding peered voor VNET-PROD-EUS2 en hergebruik-PROD-CUS. 

![Peering](./media/contoso-migration-infrastructure/peering4.png)

### <a name="set-up-dns"></a>DNS instellen

Wanneer u resources in virtuele netwerken implementeert, hebt u een aantal opties voor Domeinnaamomzetting. U kunt de naamomzetting verstrekt door Azure gebruiken of DNS-servers opgeven voor de omzetting. Het type van naamomzetting die u gebruikt, is afhankelijk van hoe uw resources moeten met elkaar communiceren. Ophalen van [meer informatie](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#azure-provided-name-resolution) over de Azure DNS-service.

Contoso hebt besloten dat de Azure DNS-service een goede keuze in hun omgeving hybride niet. In plaats daarvan, gaan ze gebruikmaken van hun lokale DNS-servers.

- Omdat dit een hybride netwerk alle de virtuele machines on-premises is en in Azure moeten kunnen omzetten van namen te laten functioneren. Dit betekent dat de aangepaste DNS-instellingen moeten worden toegepast op alle VNets.
- Contoso hebt momenteel DC's die zijn geïmplementeerd in het datacenter Contoso en in de filialen. De primaire DNS-servers zijn CONTOSODC1(172.16.0.10) en CONTOSODC2(172.16.0.1)
- Wanneer de VNets die zijn geïmplementeerd, wordt de lokale domeincontrollers worden ingesteld als DNS-server in de netwerken moet worden gebruikt. 
- Als u wilt configureren, wanneer u aangepaste DNS op het VNet, moet Azure recursieve resolvers IP-adres (zoals 168.63.129.16) worden toegevoegd aan de lijst met DNS.  U doet dit door configureert Contoso DNS-serverinstellingen voor elk VNet. De aangepaste DNS-instellingen voor het VNET-HUB-EUS2 netwerk zou bijvoorbeeld als volgt zijn:
    
    ![Aangepaste DNS](./media/contoso-migration-infrastructure/custom-dns.png)

Naast hun domeincontrollers lokale Contoso gaat implementeren vier meer ter ondersteuning van de Azure-netwerken, twee voor elke regio. Dit is wat ze in Azure implementeert.

**Regio** | **DC** | **VNet** | **Subnet** | **IP-adres**
--- | --- | --- | --- | ---
EUS2 | CONTOSODC3 | VNET-PROD-EUS2 | PROD-DC-EUS2 | 10.245.42.4
EUS2 | CONTOSODC4 | VNET-PROD-EUS2 | PROD-DC-EUS2 | 10.245.42.5
CUS | CONTOSODC5 | VNET-PROD-CUS | PROD-DC-CUS | 10.255.42.4
CUS | CONTOSODC6 | VNET-PROD-CUS | PROD-DC-CUS | 10.255.42.4

Na implementatie van de lokale domeincontrollers, moet Contoso bijwerken van de DNS-instellingen op de netwerken van beide regio zodanig dat de nieuwe domeincontrollers in de lijst met DNS-server.



#### <a name="set-up-domain-controllers-in-azure"></a>Instellen van domeincontrollers in Azure

Na het bijwerken van de netwerkinstellingen zijn Contoso gereed voor het bouwen van hun domeincontrollers in Azure.

1. In de Azure portal implementeren een nieuwe virtuele machine voor Windows-Server ze naar het juiste VNet.
2. Ze maken beschikbaarheidssets in elke locatie voor de virtuele machine. Beschikbaarheidssets doen het volgende:
    - Zorg ervoor dat de Azure-infrastructuur worden gescheiden voor de virtuele machines in verschillende infrastructuur in de Azure-regio. 
    -  Hiermee kunt u Contoso in aanmerking komt voor de SLA van 99,95% voor virtuele machines in Azure.  [Meer informatie](https://docs.microsoftcom/azure/virtual-machines/windows/regions-and-availability#availability-sets).

    ![Beschikbaarheidsgroep](./media/contoso-migration-infrastructure/availability-group.png) 
3. Nadat de virtuele machine is geïmplementeerd, pen ze de netwerkinterface van de virtuele machine. Hier ze ingesteld dat de privé-IP-adres naar statisch en geef een geldig adres.

    ![VM-NIC](./media/contoso-migration-infrastructure/vm-nic.png)

4. Ze nu koppelen een nieuwe gegevensschijf aan de virtuele machine. Deze schijf bevat het Active Directory-database en de sysvol-share. 
    - De grootte van de schijf, bepalen het aantal IOPS dat het ondersteunt.
    - De schijfgrootte mogelijk verhogen wanneer de omgeving groeit gedurende een bepaalde periode.
    - Het station mag niet worden ingesteld op lezen/schrijven voor hostcaching. Active Directory-databases kan deze niet ondersteunen.

     ![Active Directory-schijf](./media/contoso-migration-infrastructure/ad-disk.png)

5. Nadat de schijf is toegevoegd, ze verbinding maken met de virtuele machine via Extern bureaublad en open Serverbeheer.
6. Klik dan in **File and Storage Services**, ze de Wizard Nieuw Volume, waarbij u ervoor zorgt dat het station gegeven de letter F: of hoger is worden uitgevoerd op de lokale virtuele machine.

     ![Wizard Nieuw Volume](./media/contoso-migration-infrastructure/volume-wizard.png)

7. Klik in Serverbeheer het toevoegen van de **Active Directory Domain Services** rol. Ze klikt u vervolgens configureren de virtuele machine als een domeincontroller.

      ![Serverfunctie](./media/contoso-migration-infrastructure/server-role.png)  

9. Nadat de virtuele machine is geconfigureerd als een domeincontroller en opnieuw opgestart, wordt deze DNS-beheer openen en de Azure DNS-resolver als een doorstuurserver configureren.  Hiermee wordt de domeincontroller voor het doorsturen van DNS-query's die kan in de Azure DNS-server kan niet worden omgezet.

    ![DNS-doorstuurserver](./media/contoso-migration-infrastructure/dns-forwarder.png)

10. Nu werk vervolgens de aangepaste DNS-instellingen voor elk VNet met de juiste domeincontroller voor het VNet regio. Ze bevatten lokale DC's in de lijst.

### <a name="set-up-active-directory"></a>Active Directory instellen

AD is een kritieke service in netwerken en moet correct worden geconfigureerd. Contoso bouwt Active Directory: sites voor het Contoso-datacenter, en voor de EUS2 en CUS gebieden.  

1. Deze maken twee nieuwe sites (AZURE EUS2 en AZURE CUS) samen met de site datacenter (ContosoDatacenter).
2. Na het maken van de sites, maken subnetten ze in de sites, overeenkomen met de VNets en datacenter.

    ![DC-subnetten](./media/contoso-migration-infrastructure/dc-subnets.png)

3. Vervolgens maken zij twee sitekoppelingen om alles verbinding te. De domeincontrollers moeten vervolgens worden verplaatst naar de locatie.

    ![DC-koppelingen](./media/contoso-migration-infrastructure/dc-links.png)

4. Nadat alles is geconfigureerd, wordt de Active Directory-replicatietopologie aanwezig is.
    
    ![DC-replicatie](./media/contoso-migration-infrastructure/ad-resolution.png)

5. Een lijst met domeincontrollers en sites worden met Alles voltooid weergegeven in het lokale Active Directory-beheercentrum.

    ![AD-beheercentrum](./media/contoso-migration-infrastructure/ad-center.png)

## <a name="step-5-plan-for-governance"></a>Stap 5: Plannen voor toezicht

Azure biedt een reeks beheeracties besturingselementen voor services en de Azure-platform. [Lees meer](https://docs.microsoft.com/azure/security/governance-in-azure) voor een basiskennis van opties.

Configureer de identiteit en toegangsbeheer, Contoso al begonnen met het voor het opzetten van bepaalde aspecten van bestuur en beveiliging. Algemeen, zijn er drie gebieden die ze moeten rekening houden met:

- **Beleid**: beleid in Azure is van toepassing en regels en effecten zodat bronnen voldoen aan de vereisten van uw bedrijf en sla's blijven wordt afgedwongen via uw resources.
- **Hiermee vergrendelt u**: Azure kunt u vergrendelen abonnementen, resourcegroepen en andere resources, zodat ze kunnen alleen worden gewijzigd door die met autoriteit om dit te doen.
- **Labels**: Resources kunnen worden beheerd, gecontroleerd en beheerd met labels. Labels toevoegen metagegevens tot bronnen, bieden informatie over de resources of eigenaars.

### <a name="set-up-policies"></a>Instellen van het beleid

De service Azure beleid evalueert uw resources, scannen op die niet compatibel zijn met de beleidsdefinities aanwezig is. Wellicht is bijvoorbeeld een beleid dat alleen bepaalde typen virtuele machines kunt of vereist bronnen voor een specifiek label hebben. 

Azure beleidsregels Geef een beleidsdefinitie van een en toewijzing van beleid voor het bereik opgeven waarop een beleid moet worden toegepast. Het bereik kan variëren van een beheergroep aan een resourcegroep. [Meer informatie over](https://docs.microsoft.com/azure/azure-policy/create-manage-policy) over het maken en beheren van beleid.

Contoso wil aan de slag met een aantal beleid:

- Ze willen een beleid om ervoor te zorgen dat bronnen kunnen alleen worden geïmplementeerd in de regio's EUS2 en CUS.
- Ze willen VM-SKU's om goedgekeurde SKU's te beperken. De bedoeling is om ervoor te zorgen dat dure VM-SKU's worden niet gebruikt.

#### <a name="limit-resources-to-regions"></a>Limiet voor bronnen met regio 's

Contoso gebruiken de ingebouwde beleidsdefinitie **toegestaan locaties** resource regio's te beperken.

1. Klik in de Azure-portal op **alle Services**, en zoek naar **beleid**.
2. Selecteer **toewijzingen** > **beleid toewijzen**.
3. Selecteer in de lijst met **toegestaan locaties**.
4. Stel **bereik** op de naam van de Azure-abonnement en selecteert u de twee gebieden in de lijst met toegestane.

    ![Beleid voor toegestane regio 's](./media/contoso-migration-infrastructure/policy-region.png)

5. Het beleid is standaard ingesteld met **weigeren**, wat betekent dat als iemand een implementatie is gestart in het abonnement dat niet in EUS2 of CUS, mislukt de implementatie. Dit is wat er gebeurt als iemand in de Contoso-abonnement probeert voor het instellen van een implementatie in VS-West.

    ![Beleid is mislukt](./media/contoso-migration-infrastructure/policy-failed.png)

#### <a name="allow-specific-vm-skus"></a>Specifieke VM-SKU's toestaan

Contoso gebruikt de ingebouwde beleidsdefinitie **zodat virtuele machines SKU's** te beperken van het type van virtuele machines dat kan worden gemaakt in het abonnement. 

![Beleid SKU](./media/contoso-migration-infrastructure/policy-sku.png)



#### <a name="check-policy-compliance"></a>Controleren op beleidsnaleving

Beleid van kracht onmiddellijk en Contoso-resources voor naleving kunt controleren. 

1. Klik in de Azure-portal op de **naleving** koppeling.
2. Het dashboard naleving wordt weergegeven. U kunt inzoomen voor meer informatie.

    ![Beleidsnaleving](./media/contoso-migration-infrastructure/policy-compliance.png)


### <a name="set-up-locks"></a>Vergrendelingen instellen

Contoso heeft lang is met behulp van het framework ITIL voor het beheer van hun systemen. Een van de belangrijkste aspecten van het framework is het besturingselement wijzigen en Contoso wil ervoor zorgen dat beheer van wijzigingen is geïmplementeerd in Azure-implementatie.

Contoso gaat implementeren vergrendelingen als volgt:

- Productie- of failover-component moet een resourcegroep die een vergrendeling van het kenmerk alleen-lezen heeft.  Dit betekent dat als u wilt wijzigen of verwijderen van items in productie, de vergrendeling moet worden verwijderd. 
- Niet-productieve resourcegroepen hebben CanNotDelete vergrendelingen. Dit betekent dat geautoriseerde gebruikers kunnen lezen of wijzigen van een bron, maar kan niet worden verwijderd.

[Meer informatie](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) over vergrendelingen.

### <a name="set-up-tagging"></a>Labels instellen

Als u wilt bijhouden resources zoals ze zijn toegevoegd, wordt het steeds belangrijker voor Contoso resources koppelen aan een passende afdeling, de klant en de omgeving zijn. 

Naast het bieden van informatie over de resources en eigenaren kunnen labels Contoso aan cumulatieve en groep resources en het gebruik van die gegevens voor doorbelastingdoeleinden.

Contoso moet visualiseren hun Azure activa op een manier die zinvol is voor hun bedrijf. Bijvoorbeeld, maar de functie of afdeling. Houd er rekening mee dat resources niet zich bevinden in dezelfde resourcegroep moeten voor het delen van een label. U doet dit door maakt Contoso u een eenvoudige tagtaxonomie zodat iedereen met behulp van dezelfde tags.

**Codenaam** | **Waarde**
--- | ---
Kostenplaats | 12345: dit moet een geldige kostenplaats van SAP.
Business Unit | De naam van business unit (van SAP). Komt overeen met CostCenter.
ApplicationTeam | E-mailalias op van het team dat eigenaar is van ondersteuning voor de app.
Catalogusnaam | De naam van de app of ShareServices, per de Servicecatalogus die ondersteuning biedt voor de resource.
Service Manager | E-mailalias op van de ITIL Service Manager voor de resource.
COBPriority | De prioriteit is ingesteld door het bedrijf voor BCDR. Waarden van 1-5.
ENV | De PROD DEV-, STG, de mogelijke waarden zijn. Die ontwikkelt, Faseren en productie.

Bijvoorbeeld: 

 ![Azure labels](./media/contoso-migration-infrastructure/azure-tag.png)

Na het maken van de tag wordt Contoso Ga terug en maak nieuwe beleidsdefinities van de Azure-en toewijzingen af te dwingen het gebruik van de vereiste labels in de hele organisatie.


## <a name="step-6-consider-security"></a>Stap 6: Overweeg beveiliging

Beveiliging is van cruciaal belang in de cloud en Azure biedt een breed scala aan mogelijkheden en hulpprogramma's voor beveiliging. Deze helpen u veilige oplossingen maken op de beveiligde Azure-platform. Lees [vertrouwen in de cloud vertrouwde](https://azure.microsoft.com/overview/trusted-cloud/) voor meer informatie over Azure-beveiliging.

Er enkele belangrijke aspecten voor Contoso rekening moet houden

- **Azure Security Center**: Azure Security Center biedt geïntegreerde beveiligingsbewaking en geavanceerde threat protection over hybride cloudwerkbelastingen. Met Security Center kunt u beveiligingsbeleid toepassen voor verschillende workloads, blootstelling aan bedreigingen beperken en aanvallen detecteren en afwenden.  [Meer informatie](https://docs.microsoft.com/azure/security-center/security-center-intro).
- **Netwerkbeveiligingsgroepen (nsg's)**: een NSG is een filter (firewall) die een lijst van beveiliging bevat regels die, wanneer toegepast, toestaan of weigeren van netwerkverkeer naar bronnen die zijn verbonden met Azure VNets. [Meer informatie](https://docs.microsoft.com/azure/virtual-network/security-overview).
- **Gegevensversleuteling**: Azure Disk Encryption is een functie waarmee u uw Windows- en Linux IaaS-schijven voor virtuele machine versleutelen. [Meer informatie](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest).

### <a name="work-with-the-azure-security-center"></a>Werken met het Azure Security Center

Contoso is op zoek naar een snel overzicht van de beveiligingsstatus van hun nieuwe hybride cloud- en specifiek hun Azure werkbelastingen.  Als gevolg hiervan heeft Contoso besloten voor het implementeren van Azure Security Center beginnen met de volgende functies: 

- Gecentraliseerd beleidsbeheer
- Continue assessment
- Uitvoerbare aanbevelingen 

#### <a name="centralize-policy-management"></a>Centraliseren beleidsbeheer

Met gecentraliseerd beheer van Contoso wordt ervoor zorgen dat aan de beveiligingsvereisten door het centraal beheren van beveiligingsbeleid in hun gehele omgeving. Ze kunnen eenvoudig en snel een beleid dat van toepassing op alle Azure-resources implementeren.

![Beveiligingsbeleid](./media/contoso-migration-infrastructure/security-policy.png)

#### <a name="assess-and-action"></a>Beoordelen en de actie

Contoso zal gebruikmaken van de beoordeling van continue beveiliging die wordt bewaakt de beveiliging van computers, netwerken, opslag, gegevens en toepassingen. voor het detecteren van mogelijke beveiligingsproblemen. 

- Security Center analyseert de beveiligingsstatus van de Contoso compute, infrastructuur en gegevens resources en Azure apps en services.
- Continue assessment kunt het operationele team van Contoso voor het detecteren van mogelijke beveiligingsproblemen, zoals systemen met ontbrekende beveiligingsupdates of netwerkpoorten weergegeven. 
- Contoso wil met name om ervoor te zorgen dat alle bijbehorende virtuele machines worden beveiligd. Security Center helpt met dit VM-status controleren en prioriteit en bruikbare aanbevelingen beveiligingsproblemen oplossen voordat ze bent misbruikt te.

![Bewaking](./media/contoso-migration-infrastructure/monitoring.png)

### <a name="work-with-nsgs"></a>Werken met nsg 's

Contoso kan verkeer beperkt tot bronnen in een virtueel netwerk met netwerkbeveiligingsgroepen.

- Een netwerkbeveiligingsgroep bevat een lijst met beveiligingsregels die binnenkomend of uitgaand netwerkverkeer toestaan of weigeren op basis van het bron- of doel-IP-adres, de poort en het protocol.
- Wanneer toegepast op een subnet, worden de regels toegepast op alle resources in het subnet. Naast de netwerkinterfaces bevat dit exemplaren van Azure-services geïmplementeerd in het subnet.
- Beveiligingsgroepen toepassing (ASGs) kunnen u netwerkbeveiliging configureren als een natuurlijke extensie van een app, zodat u kunt een groep VM's en beleidsregels voor netwerkbeveiliging op basis van die groepen definiëren.
    - App-beveiligingsgroepen betekenen dat u het beveiligingsbeleid op grote schaal zonder handmatige onderhoudswerkzaamheden van expliciete IP-adressen kunt hergebruiken. Het platform verwerkt de complexiteit van expliciete IP-adressen en meerdere regelsets, zodat u zich kunt richten op uw bedrijfslogica.
    - U kunt een toepassingsbeveiligingsregel opgeven als bron en doel in een beveiligingsregel. Nadat uw beveiligingsbeleid is gedefinieerd, kunt u virtuele machines maken en de VM-NIC's toewijzen aan een groep. 


Contoso wordt een combinatie van nsg's en ASGs implementeren. Zij zich zorgen maakt over het beheer van het NSG. Ze zijn vastgesteld over overmatig gebruik van het nsg's en de complexiteit kan het zijn dat voor het personeel bewerkingen.  Met dat gegeven in gedachte, hebben ze de twee belangrijkste principals die ze in het algemeen gebruiken vastgesteld:

- Al het verkeer van en naar alle subnetten (Noord Zuid), is onderworpen aan een regel NSG, met uitzondering van de GatewaySubnets in het Hub-netwerken.
- Alle firewalls of de domeincontroller worden beveiligd door zowel subnet nsg's en NIC nsg's.
- Alle productietoepassingen moeten ASGs toegepast.


Contoso heeft een model van hoe dit voor hun toepassingen eruitziet gebouwd.

![Beveiliging](./media/contoso-migration-infrastructure/asg.png)


Het nsg's die zijn gekoppeld aan de ASGs zullen worden geconfigureerd met minimale bevoegdheden om ervoor te zorgen dat pakketten alleen is toegestaan kan stromen van het ene deel van het netwerk naar de bestemming.

**Actie** | **Naam** | **Bron** | **doel** | **Poort**
--- | --- | --- | --- | --- 
Toestaan | AllowiInternetToFE | VNET-HUB-EUS1/IB-TrustZone | APP1-FE 80, 443
Toestaan | AllowWebToApp | APP1-FE | APP1-DB | 1433
Toestaan | AllowAppToDB | APP1-APP | Alle | Alle
Weigeren | DenyAllInbound | Alle | Alle | Alle

### <a name="encrypt-data"></a>Gegevens versleutelen

Azure Disk Encryption integreert met Azure Key Vault voor het beheren en beheren van de schijfversleuteling sleutels en geheimen in een sleutelkluis-abonnement. Hiermee zorgt u ervoor dat alle gegevens op de VM-schijven zijn versleuteld in rust in Azure-opslag.  

- Contoso heeft vastgesteld dat bepaalde VMs versleuteling vereisen.
- Ze gaan versleuteling voor VM's met de klant, vertrouwelijk of pixels per inch gegevens toe te passen.


## <a name="conclusion"></a>Conclusie

In dit artikel Contoso Azure-infrastructuur of instellen en instellen van geplande infrastructuur beleid voor Azure-abonnement, hybride hebt geïdentificeerd, herstel na noodgevallen, netwerken, beheer en beveiliging. 

Niet alle de stappen beschreven die Contoso voltooid hier zijn vereist voor een migratie naar de cloud. In het geval ze wilden het plannen van een netwerkinfrastructuur die kan worden gebruikt voor alle typen migraties en veilige, schaalbare en robuuste is. 

Met deze infrastructuur aanwezig nu ze verplaatsen op en probeer uit migratie.

## <a name="next-steps"></a>Volgende stappen

Als een eerste migratiescenario Contoso gaat [evalueren hun lokale SmartHotel twee lagen app voor migratie naar Azure](contoso-migration-assessment.md). 
