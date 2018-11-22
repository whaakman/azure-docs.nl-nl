---
title: Contoso-schaal een migratie naar Azure | Microsoft Docs
description: Meer informatie over hoe Contoso omgaat met een schaal migratie naar Azure.
services: azure-migrate
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: raynew
ms.openlocfilehash: 37f0ba800cca4b096691a8bb6b43eb33a636d833
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52284860"
---
# <a name="contoso---scale-a-migration-to-azure"></a>Contoso - schaal een migratie naar Azure

In dit artikel voert Contoso een migratie op schaal naar Azure. Ze Houd rekening met het plannen en uitvoeren van een migratie van meer dan 3000 werkbelastingen, 8000 databases en meer dan 10.000 VM's.


In dit artikel is een in een reeks artikelen waarin hoe het fictieve bedrijf Contoso de on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrond en planningsinformatie en implementatiescenario's die laten hoe u zien voor het instellen van de infrastructuur van een migratie, de geschiktheid van on-premises bronnen voor migratie beoordelen en verschillende typen migraties worden uitgevoerd. Scenario's toeneemt in complexiteit. Na verloop van tijd, gaan we artikelen toevoegen aan de reeks.


**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Overzicht van de serie artikelen, strategie voor de migratie van Contoso en de voorbeeld-apps die worden gebruikt in de reeks. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Contoso bereidt u de on-premises infrastructuur en de Azure-infrastructuur voor migratie. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen in de reeks. | Beschikbaar.
[Artikel 3: Evalueer on-premises bronnen voor migratie naar Azure](contoso-migration-assessment.md)  | Contoso wordt uitgevoerd een evaluatie van de on-premises SmartHotel360-app die wordt uitgevoerd op VMware. Contoso beoordeelt virtuele machines van app met behulp van de Azure Migrate-service en de app SQL Server-database met behulp van Data Migration Assistant. | Beschikbaar
[Artikel 4: Opnieuw hosten van een app op een Azure-VM en het beheerde exemplaar van SQL Database](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso wordt een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel360-app uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso de app-database migreert naar een Azure SQL Database Managed Instance met de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar   
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | De VM's van de SmartHotel360-app migreert Contoso naar Azure-VM's met behulp van de Site Recovery-service. | Beschikbaar
[Artikel 6: Een app op Azure Virtual machines en in een SQL Server AlwaysOn-beschikbaarheidsgroep opnieuw hosten](contoso-migration-rehost-vm-sql-ag.md) | Contoso, worden gemigreerd met de app met behulp van Site Recovery voor het migreren van virtuele machines van de app en de Database Migration Service voor het migreren van de app-database naar een SQL Server-cluster dat wordt beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Contoso is een lift-and-shift-migratie van de Linux-osTicket-app naar Azure-VM's, met behulp van de Site Recovery-service voltooid. | Beschikbaar
[Artikel 8: Een Linux-app op Azure VM's en Azure Database for MySQL Rehost](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migreert de Linux-osTicket-app naar Azure-VM's met behulp van Site Recovery. Deze migreert de app-database naar Azure Database for MySQL via MySQL Workbench. | Beschikbaar
[Artikel 9: Een app in een Azure-web-app en Azure SQL Database herstructureren](contoso-migration-refactor-web-app-sql.md) | Contoso de SmartHotel360-app migreert naar een Azure-web-app en de app-database migreert naar een Azure SQL Server-exemplaar met de Database Migration Assistant. | Beschikbaar    
[Artikel 10: Een Linux-app in een Azure-web-app en de Azure Database for MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | De Linux-app osTicket migreert Contoso naar een Azure-web-app op meerdere sites. De web-app is geïntegreerd met GitHub voor continue levering. Deze migreert de app-database naar een Azure Database for MySQL-exemplaar. | Beschikbaar
[Artikel 11: Herstructureren Team Foundation Server op Azure DevOps-Services](contoso-migration-tfs-vsts.md) | Contoso migreert de on-premises Team Foundation Server-implementatie naar Azure DevOps-Services in Azure. | Beschikbaar
[Artikel 12: Opnieuw ontwerpen van een app in Azure-containers en Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | De app SmartHotel migreert Contoso naar Azure. Vervolgens rearchitects wordt de weblaag app als een Windows-container die wordt uitgevoerd in Azure Service Fabric en app-databases met Azure SQL Database. | Beschikbaar    
[Artikel 13: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Contoso wordt opnieuw gemaakt zijn SmartHotel-app met een scala aan mogelijkheden van Azure en services, waaronder Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services en Azure Cosmos DB. | Beschikbaar 
Artikel 14: Schaal van een migratie naar Azure | Na het proberen van migratie combinaties, bereidt Contoso worden uitgebreid naar een volledige migratie naar Azure. | In dit artikel

## <a name="business-drivers"></a>Zakelijke drijfveren

De IT-team leiderschap heeft nauw samengewerkt met zakelijke partners om te begrijpen wat ze willen bereiken bij deze migratie:

- **Adres van de groei van het bedrijf**: Contoso groeit, waardoor de druk op on-premises systemen en infrastructuur.
- **De efficiëntie verhogen**: Contoso moet verwijderen van onnodige procedures en processen stroomlijnen voor ontwikkelaars en gebruikers. De bedrijfsbehoeften IT snel en niet afval tijd of geld, dus sneller leveren op de eisen van klanten.
- **De veerkracht vergroten**: Contoso IT moet sneller te reageren op de behoeften van het bedrijf. Het moet mogelijk zijn om te reageren sneller dan de wijzigingen in de marketplace, waarmee het succes in een globale economie. Het kan niet ophalen in de manier waarop of een zakelijke upblokkering worden.
- **Schaal**: wanneer het bedrijf is groeit, het Contoso IT-team kunnen groeien hetzelfde tempo systemen moet opgeven.
- **Kostenmodellen verbeteren**: Contoso wil standaardprocedures kapitaal vereisten in de IT-budget.  Contoso wil cloud-vaardigheden gebruiken om te schalen en beperken van de behoefte aan dure hardware.
- **Lagere licentiekosten**: Contoso wil cloudkosten kunt minimaliseren.


## <a name="migration-goals"></a>Migratie-doelen

Het Contoso-cloud-team heeft vastgemaakt omlaag doelstellingen voor deze migratie. Deze doelstellingen zijn gebruikt voor het bepalen van de beste migratiemethode.

**Vereisten** | **Details**
--- | --- 
**Snel naar Azure verplaatsen** | Contoso wil zo snel mogelijk apps en VM's naar Azure verplaatsen.
**Een volledige inventarisatie compileren** | Contoso wil een volledige inventarisatie van alle apps, databases en virtuele machines in de organisatie.
**Beoordelen en classificeren van apps** | Contoso wil volledig gebruikmaken van de cloud. Als een standaard Contoso wordt ervan uitgegaan dat alle services wordt uitgevoerd als PaaS. IaaS zal worden gebruikt waar PaaS niet juist. 
**Trainen en te verplaatsen naar DevOps** | Contoso wil verplaatsen naar een DevOps-model. Contoso bieden Azure en DevOps-training en teams opnieuw naar behoefte. 


Na het vastmaken omlaag doelstellingen en vereisten, Contoso beoordeelt de IT-footprint en identificeert het migratieproces.

## <a name="current-deployment"></a>Huidige implementatie

Na het plannen en instellen van een [Azure-infrastructuur](contoso-migration-infrastructure.md) en uitproberen van verschillende combinaties van proof of concept (POC) migratie zoals beschreven in de bovenstaande tabel, Contoso is gereed om te zetten op een volledige migratie naar Azure op schaal. Hier volgt wat Contoso wil migreren.

**Item** | **volume** | **Details**
--- | --- | ---
**Workloads** | Meer dan 3.000 apps | Apps worden uitgevoerd op virtuele machines.<br/><br/>  Apps zijn Windows, op basis van SQL en OSS LAMP.
**Databases** | Ongeveer 8,500 | Databases omvatten SQL Server, MySQL, PostgreSQL.
**VM's** | Meer dan 10.000 | Virtuele machines op VMware-hosts worden uitgevoerd en beheerd door vCenter-Servers.


## <a name="migration-process"></a>Migratieproces

Nu dat Contoso business stuurprogramma's en de doelstellingen van de migratie hebt vastgemaakt, bepaalt een vier aders benadering voor het migratieproces:

- **Fase 1-beoordelen**: de huidige-assets detecteren en te achterhalen of ze geschikt voor migratie naar Azure.
- **Fase 2-migreren**: de activa te verplaatsen naar Azure. Hoe deze apps worden verplaatst en objecten naar Azure is afhankelijk van de app, en wat ze willen bereiken.
- **Fase 3-optimaliseren**: na het verplaatsen van resources naar Azure, Contoso moet te verbeteren en deze voor maximale prestaties en efficiëntie te stroomlijnen.
- **Fase 4-beveiligde en beheer**: alles wat erin, Contoso nu maakt gebruik van Azure-beveiliging en beheer van resources en services om te bepalen, beveiligen en bewaken van de cloud-apps in Azure.


De fasen zijn seriële niet in de hele organisatie. Elk onderdeel van het migratieproject van Contoso is op een ander tijdstip van het proces voor evaluatie en migratie. Optimalisatie, beveiliging en beheer wordt wordt er na verloop van tijd.


## <a name="phase-1-assess"></a>Fase 1: beoordelen

Contoso begint het proces door te detecteren en beoordelen van on-premises toepassingen, gegevens en infrastructuur. Dit is wat Contoso doet:

- Contoso moet voor het detecteren van apps, afhankelijkheden toegewezen in apps, en moet u besluiten welke migratievolgorde en prioriteit.
- Als Contoso beoordeelt, wordt het bouwen van een uitgebreide inventaris van apps en resources. Samen met de nieuw-inventaris, Contoso gebruikt en bijwerken van de bestaande configuratie configuratiebeheerdatabase (CMDB) en de Servicecatalogus.
    - De CMDB bevat technische configuraties voor Contoso-apps.
    - De Servicecatalogus worden de operationele gegevens van apps, met inbegrip van bijbehorende zakelijke partners en Service Level Agreements (Sla's)

### <a name="discover-apps"></a>Apps ontdekken

Duizenden apps Contoso uitgevoerd op verschillende servers. Naast de CMDB en Servicecatalogus moet Contoso hulpprogramma's voor detectie en evaluatie. 

- De hulpprogramma's moeten bieden een mechanisme waarin van de evaluatiegegevens in het migratieproces.
- Analyseprogramma's moeten zorgen dat gegevens die u helpt met het bouwen van een intelligente inventaris van de fysieke en virtuele resources van Contoso. Gegevens bevatten profielgegevens en metrische gegevens voor prestaties.
- Wanneer detectie voltooid is, moet een volledige inventarisatie van assets en metagegevens die zijn gekoppeld aan deze hebben in Contoso. Deze inventarisatie wordt gebruikt voor het definiëren van het migratieplan.

### <a name="identify-classifications"></a>Identificeren van classificaties

Contoso identificeert algemene categorieën voor het classificeren van assets in de inventaris. Deze classificaties zijn essentieel voor het Contoso beslissingen voor de migratie. De lijst met probleemclassificaties helpt bij het stellen van prioriteiten voor migratie en complexe problemen te identificeren.

**Categorie** | **Toegewezen waarde** | **Details**
--- | --- | ---
Bedrijfsgroep | Lijst met namen van bedrijven | Welke groep is verantwoordelijk voor het inventarisitem?
POC kandidaat | J/N | De app kan worden gebruikt als een POC of een van de eersten voor cloudmigratie?
Technische inspanningen | Geen/sommige/ernstige | Het inventarisitem wordt uitgevoerd of met behulp van een out-van-ondersteuning voor product, platform of besturingssysteem?
Gevolgen van de firewall | J/N | De app communiceren met Internet/buiten verkeer?  Is deze dan geïntegreerd met een firewall?
Beveiligingsproblemen | J/N | Er problemen zijn bekende beveiligingsproblemen met de app?  Gebruikt de app niet-versleutelde gegevens of verouderde platforms?


### <a name="discover-app-dependencies"></a>App-afhankelijkheden ontdekken

Als onderdeel van de evaluatie moet Contoso om te bepalen waar apps worden uitgevoerd, en de afhankelijkheden en de verbindingen tussen appservers te achterhalen. Contoso Hiermee wijst u de omgeving in stappen.

1. Als een eerste stap detecteert Contoso hoe servers en virtuele machines aan afzonderlijke apps, netwerklocaties en groepen toegewezen.
2. Met deze informatie kunt Contoso duidelijk apps die enkele afhankelijkheden, en zijn dus geschikt voor een snelle migratie identificeren.
3. Contoso kunt toewijzen voor het identificeren van meer complexe afhankelijkheden en de communicatie tussen servers met Apps. Contoso kan vervolgens deze servers logisch om weer te geven apps groeperen en plannen van een strategie voor de migratie op basis van deze groepen.


Met de toewijzing is voltooid, kunt Contoso ervoor zorgen dat alle app-onderdelen worden geïdentificeerd en rekening gehouden bij het bouwen van het migratieplan. 

![Afhankelijkheidstoewijzing](./media/contoso-migration-scale/dependency-map.png)


### <a name="evaluate-apps"></a>Evalueren van apps

Als de laatste stap in het proces van detectie en evaluatie kunt Contoso evalueren van de evaluatie- en -toewijzing resultaten bepalen hoe u voor het migreren van elke app in de Servicecatalogus. 

Voor het vastleggen van dit evaluatieproces, toevoegen ze een aantal extra classificaties aan de inventaris.

**Categorie** | **Toegewezen waarde** | **Details**
--- | --- | ---
Bedrijfsgroep | Lijst met namen van bedrijven | Welke groep is verantwoordelijk voor het inventarisitem?
POC kandidaat | J/N | De app kan worden gebruikt als een POC of een van de eersten voor cloudmigratie?
Technische inspanningen | Geen/sommige/ernstige | Het inventarisitem wordt uitgevoerd of met behulp van een out-van-ondersteuning voor product, platform of besturingssysteem?
Gevolgen van de firewall | J/N | De app communiceren met Internet/buiten verkeer?  Is deze dan geïntegreerd met een firewall?
Beveiligingsproblemen | J/N | Er problemen zijn bekende beveiligingsproblemen met de app?  Gebruikt de app niet-versleutelde gegevens of verouderde platforms?
Een strategie voor clientmigratie | Rehost/herstructureren/Hermodellering/opnieuw maken | Wat voor soort migratie is nodig voor de app? Hoe wordt de app worden geïmplementeerd in Azure? [Meer informatie](contoso-migration-overview.md#migration-strategies).
Technische complexiteit | 1-5 | Hoe complex is de migratie? Deze waarde moet worden gedefinieerd door Contoso DevOps en relevante partners.
Bedrijfscontinuïteit | 1-5 | Hoe belangrijk is de app voor het bedrijf? Bijvoorbeeld, een kleine werkgroep-app kan worden toegewezen aan een score van een, terwijl een essentiële app die wordt gebruikt in de organisatie kan worden toegewezen aan een score van vijf. Deze score is van invloed op het niveau van de migratie van prioriteit.
Prioriteit van de migratie | 1/2/3 | Wat de migratie van prioriteit voor de app?
Risico's voor migratie  | 1-5 | Wat is het risiconiveau voor het migreren van de app? Deze waarde moet worden overeengekomen door Contoso DevOps en relevante partners.




### <a name="figure-out-costs"></a>Kosten berekenen

Als u wilt weten kosten en de potentiële besparingen van Azure-migratie, Contoso kunt gebruiken de [totale kosten van eigendom (TCO) calculator](https://azure.microsoft.com/pricing/tco/calculator/) om te berekenen en vergelijken van de totale Eigendomskosten voor Azure met een vergelijkbare on-premises implementatie.

### <a name="identify-assessment-tools"></a>Analyseprogramma's identificeren

Contoso besluit welk hulpprogramma moet worden gebruikt voor detectie, beoordeling en bouwen de voorraad. Contoso identificeert een combinatie van Azure-hulpprogramma's en services, hulpprogramma's voor systeemeigen app en scripts en hulpprogramma's. Contoso is met name geïnteresseerd in hoe Azure Migrate kan worden gebruikt om te beoordelen op schaal.


#### <a name="azure-migrate"></a>Azure Migrate


De service Azure Migrate helpt u om te detecteren en evalueren van on-premises VMware-machines, ter voorbereiding voor migratie naar Azure. Dit is wat Azure Migrate doet:

1. Detecteren: On-premises VMware-VM's detecteren.
    - Azure Migrate biedt ondersteuning voor detectie van meerdere vCenter-Servers (serially), en detecties kan worden uitgevoerd in afzonderlijke Azure Migrate-projecten.
    - Azure Migrate wordt detectie uitgevoerd op een manier voor het uitvoeren van de Collector migreren van een VMware-VM. De dezelfde collector kunt VM's detecteren op verschillende vCenter-servers en gegevens verzenden naar verschillende projecten.
1. Gereedheid beoordelen: Bepaal of het on-premises virtuele machines zijn geschikt voor het uitvoeren in Azure. Evaluatie bevat de volgende:
    - Aanbevolen waarden: aanbevolen groottes voor virtuele machines van Azure, op basis van de prestatiegeschiedenis van on-premises VM's.
    - De geschatte maandelijkse kosten: de geschatte kosten voor het uitvoeren van on-premises machines in Azure.
2. Identificeren van afhankelijkheden: visualiseren afhankelijkheden van on-premises computers, het maken van groepen van de optimale machines voor evaluatie en migratie.


![Azure Migrate](./media/contoso-migration-scale/azure-migrate.png)


##### <a name="migrate-at-scale"></a>Migreren op schaal

Contoso-behoeften te gebruiken Azure Migrate geeft de schaal van deze migratie. 

- Contoso doet een evaluatie van de app-per-app met Azure Migrate. Dit zorgt ervoor dat Azure Migrate actuele gegevens terug naar de Azure-portal.
- Beheerders van Contoso meer informatie over [implementeren van Azure Migrate op schaal](how-to-scale-assessment.md)
- Contoso opmerkingen bij de Azure Migrate limieten in de volgende tabel samengevat.


**Actie** | **Limiet**
--- | ---
Azure Migrate-project maken | 1500 VM 's
Detectie | 1500 VM 's
Beoordeling | 1500 VM 's

Contoso wordt Azure Migrate gebruiken als volgt:

- In vCenter wordt Contoso virtuele machines ordenen in mappen. Dit maakt het eenvoudig voor ze zich kunnen concentreren als ze een evaluatie uitvoeren op virtuele machines in een specifieke map.
- Azure Migrate maakt gebruik van Azure servicetoewijzing om afhankelijkheden tussen machines vast te stellen. Dit is vereist voor agents worden geïnstalleerd op virtuele machines moeten worden beoordeeld. 
    - Contoso wordt geautomatiseerde scripts gebruiken om de vereiste Windows- of Linux-agents te installeren.
    - Door het uitvoeren van scripts, kunt Contoso pushen de installatie naar virtuele machines in een vCenter-map.


#### <a name="database-tools"></a>Databasehulpprogramma's

Contoso naast Azure Migrate, wordt de nadruk gelegd op het gebruik van hulpprogramma's speciaal voor de evaluatie van database. Hulpprogramma's zoals de [Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) wordt kunt beoordelen voor migratie van SQL Server-databases.

Contoso om te achterhalen of on-premises databases zijn compatibel met een verscheidenheid aan door Azure-database-oplossingen, zoals Azure SQL Database, SQL Server wordt uitgevoerd op een Azure IaaS VM en Azure SQL Managed Instance kunt u de Data Migration Assistant (DMA). 

Naast de DMS heeft Contoso enkele andere scripts die ze gebruiken voor het detecteren en documenteren van de SQL Server-databases. Deze bevinden zich in de GitHub-opslagplaats.

#### <a name="partner-tools"></a>Hulpprogramma 's

Er zijn verschillende andere hulpprogramma's die Contoso helpen kunnen bij het beoordelen van de on-premises omgeving voor migratie naar Azure. [Meer informatie](https://azure.microsoft.com/migration/partners/) over migratiepartners van Azure.  

## <a name="phase-2-migrate"></a>Fase 2: migreren

Met hun evaluatie moet volledig Contoso hulpprogramma's voor het verplaatsen van hun apps, gegevens en infrastructuur naar Azure bepalen. 




### <a name="migration-strategies"></a>Migratiestrategieën

Er zijn vier brede migratiestrategieën die Contoso kunt overwegen. 

**Strategie** | **Details** | **Gebruik**
--- | --- | ---
**Opnieuw hosten**  | Vaak aangeduid als 'lift and shift'-migratie, is dit snel een optie zonder code voor het migreren van bestaande apps naar Azure.<br/><br/> Een app worden gemigreerd als-is, met de voordelen van de cloud, zonder de risico's of kosten voor de code te wijzigen. | Contoso kan opnieuw hosten minder strategische apps, vereisen van zonder code te wijzigen.
**Herstructureren** |  Ook wel "opnieuw verpakken", deze strategie vereist minimale app-code of wijzigingen in de configuratie moeten de app verbinden met Azure PaaS en beter profiteren van de mogelijkheden van de cloud. | Contoso kan herstructureren strategische apps als u wilt de dezelfde basisfunctionaliteit behouden, maar ze worden uitgevoerd op een Azure-platform, zoals Azure App Services te verplaatsen.<br/><br/> Hiervoor moet minimale code hoeft te wijzigen.<br/><br/> Contoso hebben daarentegen, onderhouden van een VM-platform, omdat dit wordt niet beheerd door Microsoft.
**Opnieuw modelleren** | Deze strategie wordt gewijzigd of uitbreiden van een app-codebasis het optimaliseren van de app-architectuur voor de mogelijkheden van de cloud en de schaal.<br/><br/> Deze moderniseert een app in de architectuur van een robuuste, schaalbare, onafhankelijk implementeerbare.<br/><br/> Azure-services kunnen het proces versnellen, u kunt toepassingen schalen met vertrouwen en apps met gemak beheren.
**Opnieuw maken** | Deze strategie wordt opnieuw gemaakt met een volledig nieuwe met behulp van technologieën voor cloud-native app.<br/><br/> Azure-platform als een service (PaaS) biedt een complete ontwikkel- en implementatieomgeving in de cloud. Het wordt voorkomen dat bepaalde kosten en complexiteit van softwarelicenties en hoeven zich niet voor een onderliggende app-infrastructuur, middleware en andere bronnen. | Contoso kan essentiële apps vanaf de grond herschrijven, om te profiteren van cloud-technologieën, zoals serverloze computer of microservices.<br/><br/> Contoso locatie beheert u de app en services die deze ontwikkelt en Azure beheert alle andere.


De gegevens moeten ook worden gezien, vooral bij het volume van databases die Contoso heeft. Contoso standaard aanpak is het gebruik van PaaS-services zoals Azure SQL Database om te profiteren van cloud-functies. Door te verplaatsen naar een PaaS-service voor databases, moet Contoso alleen onderhouden, waardoor het onderliggende platform naar Microsoft.

### <a name="evaluate-migration-tools"></a>Evalueren van hulpprogramma's voor migratie

Contoso zijn voornamelijk via diverse Azure-services en hulpprogramma's voor de migratie:

- [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview): Orkestreert noodherstel en on-premises machines naar Azure migreert.
- [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview): on-premises database zoals SQL Server, MySQL en Oracle naar Azure migreert.


#### <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery is de primaire Azure-service voor het organiseren van herstel na noodgevallen en migratie van binnen Azure en on-premises-sites naar Azure.

1. Site Recovery kunt, regelt de replicatie van uw on-premises sites naar Azure.
2. Wanneer replicatie is ingesteld en wordt uitgevoerd, on-premises machines failover naar Azure, de migratie is voltooid.

Contoso al [voltooid een POC](contoso-migration-rehost-vm.md) om te zien hoe Site Recovery kunnen helpen bij het migreren naar de cloud.

##### <a name="using-site-recovery-at-scale"></a>Met behulp van Site Recovery op schaal

Contoso plannen voor het uitvoeren van meerdere lift-and-shift-migraties. Als u wilt controleren of dat dit werkt, moet op Site Recovery batches met ongeveer 100 virtuele machines tegelijk worden gerepliceerd. Als u wilt weten hoe dit werkt, moet Contoso capaciteitsplanning van de voorgestelde Site Recovery-migratie.

- Contoso moet voor het verzamelen van informatie over hun verkeer volumes. In het bijzonder:
    - Contoso nodig heeft om te bepalen van de snelheid van de wijziging voor VM's die het bedrijf wil repliceren.
    - Contoso moet ook rekening met de netwerkverbinding van de on-premises site naar Azure.
- In reactie op capaciteit en vereisten voor het hostvolume moet Contoso voldoende bandbreedte op basis van de dagelijkse veranderingssnelheid van gegevens voor de vereiste virtuele machines, om te voldoen aan de beoogde herstelpunt (RPO) toewijzen.
- Daarnaast moeten ze om te achterhalen hoeveel servers nodig zijn er om uit te voeren van de Site Recovery-onderdelen die nodig zijn voor de implementatie.

###### <a name="gather-on-premises-information"></a>On-premises informatie verzamelen
Contoso kunt gebruiken de [Site Recovery Deployment Planner](https://docs.microsoft.com/azure/site-recovery/site-recovery-deployment-planner) hulpprogramma voor het voltooien van deze stappen:

- Contoso kunt het hulpprogramma op afstand profileren virtuele machines zonder een impact op de productie-omgeving. Dit helpt de vereisten voor bandbreedte en opslag voor replicatie en failover met deze functie.
- Contoso kan het hulpprogramma uitvoeren zonder te installeren van een Site Recovery on-premises onderdelen.
- Het hulpprogramma verzamelt informatie over compatibele en niet-compatibele virtuele machines, schijven per VM en gegevensverloop per schijf. Het identificeert ook netwerkbandbreedtevereisten en de Azure-infrastructuur die nodig zijn voor succesvolle replicatie en failover.
- Contoso heeft nodig om ervoor te zorgen dat de tool voor implementatieplanning vervolgens uitvoeren op een Windows Server-machines die overeenkomt met de minimale vereisten voor de Site Recovery-configuratieserver. De configuratieserver is een Site Recovery-machine die nodig is om te repliceren van on-premises VMware-machines.


###### <a name="identify-site-recovery-requirements"></a>Site Recovery-vereisten identificeren

Naast de virtuele machines die worden gerepliceerd, wordt met Site Recovery een aantal onderdelen vereist voor VMware-migratie.

**Onderdeel** | **Details**
--- | ---
**Configuratieserver** | Een VMware-VM wordt meestal ingesteld met behulp van een OVF-sjabloon.<br/><br/> Het configuratieserveronderdeel coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.
**Processerver** | standaard geïnstalleerd op de configuratieserver.<br/><br/> Het processerveronderdeel ontvangt replicatiegegevens; Met caching, compressie en versleuteling, optimaliseert en verzendt dit naar Azure storage.<br/><br/> De processerver installeert ook Azure Site Recovery Mobility Service op virtuele machines die u repliceren wilt, en wordt automatische detectie van on-premises machines uitgevoerd.<br/><br/> Geschaalde implementaties moet extra, zelfstandige processervers voor de verwerking van grote hoeveelheden replicatieverkeer.
**Mobility-Service** | De Mobility-Service-agent is geïnstalleerd op elke VMware-VM die zullen worden gemigreerd met Site Recovery.  

Contoso moet weten over het implementeren van deze onderdelen, op basis van capaciteitsoverwegingen.

**Onderdeel** | **Capaciteitsvereisten**
--- | ---
**Maximale dagelijkse veranderingssnelheid** | Een dagelijkse kan worden verwerkt door een enkele processerver wijzigen beoordelen tot 2 TB. Omdat een virtuele machine slechts één processerver gebruiken kan, is de maximale dagelijkse veranderingssnelheid van gegevens die wordt ondersteund voor een gerepliceerde virtuele machine 2 TB.
**Maximale doorvoer** | Een standaard Azure storage-account kan maximaal 20.000 aanvragen per seconde worden verwerkt en i/o-bewerkingen per seconde (IOP's) voor een replicerende virtuele machine moet binnen deze limiet. Bijvoorbeeld, als een virtuele machine 5 schijven heeft, en elke schijf 120 IOPS (8 kB grootte) op de virtuele machine genereert, zal klikt u vervolgens het zijn binnen de door Azure per schijf-IOPS-limiet van 500.<br/><br/> Houd er rekening mee dat het aantal opslagaccounts die nodig gelijk aan de totale bronmachine IOP's is, gedeeld door 20.000. Een gerepliceerde machine kan alleen deel uitmaken van een enkel opslagaccount in Azure.
**Configuratieserver** | Op basis van het Contoso-schatting van het repliceren van 100 = 200 virtuele machines die samen, en de [configuratieserver formaat vereisten](../site-recovery/site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-configuration-server), Contoso schatten moet als volgt een configuratie-servercomputer is:<br/><br/> CPU: 16 vcpu's (2-sockets * @ 2,5 GHz-8 kernen)<br/><br/> Geheugen: 32 GB<br/><br/> Cacheschijf: 1 TB<br/><br/> Veranderingssnelheid van gegevens: 1 TB tot 2 TB.<br/><br/> Naast het formaat van de vereisten voor moet Contoso om ervoor te zorgen dat de configuratieserver is optimaal bevinden, in het hetzelfde netwerk en LAN-segment als de virtuele machines die worden gemigreerd.
**Processerver** | Contoso wordt een speciaal toegewezen proces zelfstandige server implementeren met de mogelijkheid voor het repliceren van 100-200 virtuele machines:<br/><br/> CPU: 16 vcpu's (2-sockets * @ 2,5 GHz-8 kernen)<br/><br/> Geheugen: 32 GB<br/><br/> Cacheschijf: 1 TB<br/><br/> Veranderingssnelheid van gegevens: 1 TB tot 2 TB.<br/><br/> De processerver is hard werken en als zodanig moet zich bevinden op een ESXi-host die de schijf-i/o, het netwerkverkeer en de CPU nodig is voor de replicatie kan verwerken. Contoso worden beschouwd als een toegewezen host voor dit doel. 
**Netwerken** | Contoso heeft gecontroleerd van de huidige site-naar-site VPN-infrastructuur en besloten voor het implementeren van Azure ExpressRoute. De implementatie is kritiek omdat het wordt lagere latentie en bandbreedte naar de primaire East US 2 Azure-regio van Contoso te verbeteren.<br/><br/> **Bewaking**: Contoso moet zorgvuldig controleren gegevensstromen van de processerver. Als de gegevens de bandbreedte van het netwerk worden beschouwd als Contoso overloads [beperking van de bandbreedte van de server proces](../site-recovery/site-recovery-plan-capacity-vmware.md#control-network-bandwidth).
**Azure Storage** | Voor de migratie, moet het identificeren van Contoso het juiste type en aantal doel-Azure storage-accounts.  Site Recovery repliceert de VM-gegevens naar Azure storage.<br/><br/> Site Recovery kunt repliceren naar standard of premium (SSD) storage-accounts.<br/><br/> Om te beslissen over storage, Contoso moet doornemen [opslaglimieten](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage), en factor van de verwachte groei en toegenomen gebruik na verloop van tijd. Gezien de snelheid en de prioriteit van de migratie, heeft Contoso besloten om het gebruik van premium storage-accounts.<br/><br/> Ze zullen maken en meerdere opslagaccounts gebruiken tijdens het migratieproces.
Contoso heeft gemaakt de beslissing om beheerde schijven gebruiken voor alle virtuele machines die zijn geïmplementeerd naar Azure.  De vereiste IOPS-waarde bepaalt als de schijven standaard (HDD) of Premium (SSD worden).<br/>.<br/>


#### <a name="data-migration-service"></a>Data migratieservice 

Azure Database Migration Service (DMS) is een volledig beheerde service waarmee naadloze migratie van meerdere databasebronnen naar Azure-gegevensplatforms, met minimale downtime.

- DMS kan worden geïntegreerd met de functionaliteit van bestaande hulpprogramma's en services. De Data Migration Assistant (DMA), wordt gebruikt voor het genereren van van de evaluatierapporten die met deze functie aanbevelingen over de databasecompatibiliteit van de en alle vereiste wijzigingen.
- DMS maakt gebruik van een eenvoudig, onbegeleid migratieproces met intelligente beoordeling waarmee potentiële problemen adres vóór de migratie.
- DMS kunt op schaal uit meerdere bronnen naar de doel-Azure-database migreren.
- DMS biedt ondersteuning van SQL Server 2005 naar SQL Server 2017.

DMS is niet het enige Migratiehulpmiddel voor Microsoft-database. Krijgen een [vergelijking van hulpprogramma's en services](https://blogs.msdn.microsoft.com/datamigration/2017/10/13/differentiating-microsofts-database-migration-tools-and-services/).

###### <a name="using-dms-at-scale"></a>Met behulp van DMS op schaal

Contoso kan DMS wordt gebruikt bij het migreren van SQL Server.  

- Bij het inrichten van DMS moet Contoso om ervoor te zorgen dat deze heeft de juiste grootte hebben en ingesteld op de prestaties optimaliseren voor migraties. Contoso selecteert de optie 'bedrijfskritieke laag met 4 vCores', zodat de service om te profiteren van meerdere vcpu's voor parallelle uitvoering en snellere overdracht van gegevens.

    ![DMS schalen](./media/contoso-migration-scale/dms.png)

- Een andere vergroten/verkleinen tactiek voor Contoso is tijdelijk schaal het exemplaar van de doel-Azure SQL- of MySQL-Database naar de laag Premium SKU tijdens de gegevensmigratie. Dit verkleint database beperken dat kan van invloed zijn op activiteiten van bestandsoverdracht gegevens bij het gebruik van lager niveau SKU's.



##### <a name="using-other-tools"></a>Met andere hulpprogramma 's

Naast de DMS, Contoso andere hulpprogramma's en services gebruiken om informatie van de virtuele machine te identificeren.

- Ze hebben scripts om te helpen bij handmatige migraties. Deze zijn beschikbaar in de GitHub-opslagplaats.
- Een aantal [hulpprogramma's van partners](https://azure.microsoft.com/migration/partners/) kan ook worden gebruikt voor de migratie.


## <a name="phase-3-optimize"></a>Fase 3: optimaliseren

Na Contoso resources naar Azure verplaatst, moet de gebruiker voor het stroomlijnen van zodat ze betere prestaties en maximaliseer het rendement met tools voor kostenbeheer. Opgegeven dat Azure is een service met betalen voor gebruik, is het essentieel voor Contoso om te begrijpen hoe systemen worden uitgevoerd en om te controleren of dat ze nu de juiste grootte hebben.


### <a name="azure-cost-management"></a>Kostenbeheer van Azure

Om optimaal te profiteren van hun investeringen in de cloud, Contoso gebruik van het gratis Azure Cost Management-hulpprogramma.

- Met deze licentie oplossing die zijn gemaakt door Cloudyn, een dochteronderneming van Microsoft, kunt beheren met transparantie en precisie uitgaven Contoso.  Het biedt hulpprogramma's om te controleren, toewijzen en trim cloud-kosten.
- Kostenbeheer van Azure biedt eenvoudige dashboardrapporten voor hulp met kostentoewijzing, showbacks en toerekeningen mogelijk.
- Cost Management kunt optimaliseren door het identificeren van weinig gebruikte bronnen die Contoso kan vervolgens beheren en aanpassen voor de uitgavelimiet cloud.
- [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over Azure Cost Management.

    
![Kostenbeheer](./media/contoso-migration-scale/cost-management.png)  
    
 
### <a name="native-tools"></a>Systeemeigen hulpprogramma 's

Contoso wordt ook scripts gebruiken om niet-gebruikte bronnen te vinden.

- Tijdens migraties van grote zijn er vaak overgebleven delen van gegevens, zoals virtuele harde schijven (VHD's), die een in rekening gebracht, maar er is geen waarde opgeven voor het bedrijf. Scripts zijn beschikbaar in de GitHub-opslagplaats.
- Contoso werk dat door Microsoft gebruik van de IT-afdeling en Overweeg de implementatie van de Azure Resource Optimization (ARO)-Toolkit.
- Contoso kan implementeren van een Azure Automation-account met de vooraf geconfigureerde runbooks en schema's voor het abonnement en begin met besparen van geld. Azure-resource-optimalisatie wordt automatisch uitgevoerd op een abonnement nadat een schema is ingeschakeld of, met inbegrip van optimalisatie van nieuwe resources gemaakt.
- Dit biedt gedecentraliseerde automatiseringsfunctionaliteit om kosten te verlagen. Inclusief:
    - Azure VM's automatisch uitstellen op basis van lage CPU.
    - Virtuele Azure-machines uitstellen en unsnooze plannen.
    - Virtuele Azure-machines uitstellen of unsnooze in oplopende of aflopende volgorde met behulp van Azure labels plannen.
    - Bulksgewijs verwijderen van de resource-groepen op de aanvraag.
- Aan de slag met de toolkit ARO in deze [GitHub-opslagplaats](https://github.com/Azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit).

### <a name="partner-tools"></a>Hulpprogramma 's

Hulpprogramma's, zoals partner [Hanu](https://hanu.com/insight/) en [Scalr]( https://www.scalr.com/cost-optimization/) kunnen worden gebruikt.


## <a name="phase-4-secure--manage"></a>Fase 4: Beveiligen en beheren

In deze fase gebruikt Contoso Azure-beveiliging en beheer resources te beheren, beveiligen en bewaken van cloud-apps in Azure. Deze resources kunnen u een veilige, goed beheerde omgeving worden uitgevoerd tijdens het gebruik van producten die beschikbaar zijn in Azure portal. Contoso begint met behulp van deze services tijdens de migratie en, nog steeds gebruik van veel van deze voor een consistente ervaring voor de hybride cloud met hybride ondersteuning van Azure.


### <a name="security"></a>Beveiliging
Contoso zal zijn afhankelijk van de Azure Security Center voor geïntegreerd beveiligingsbeheer en geavanceerde bedreigingsbeveiliging voor hybride cloudworkloads.

- De Security Center biedt volledige zichtbaarheid in en controle, de beveiliging van cloud-apps in Azure.
- Contoso kan snel detecteren en actie ondernemen in reactie op bedreigingen en beveiligingsrisico te verkleinen doordat adaptieve bescherming tegen bedreigingen.

[Meer informatie](https://azure.microsoft.com/services/security-center/) over het Beveiligingscentrum. 


### <a name="monitoring"></a>Bewaking

Contoso moet inzicht in de status en prestaties van de gemigreerde apps, infrastructuur en gegevens nu met Azure. Contoso gebruik van ingebouwde Azure-cloud controlehulpprogramma's zoals Azure Monitor, Log Analytics en Application Insights.
 
- Met deze hulpprogramma's kunt Contoso eenvoudig gegevens verzamelen van bronnen en Verkrijg uitgebreide inzichten. Contoso kan bijvoorbeeld CPU-schijf en het geheugen gebruik voor virtuele machines, Bekijk toepassingen en netwerkafhankelijkheden in meerdere virtuele machines, meten en bijhouden van prestaties van toepassingen.
- Contoso gebruikt deze cloud controlehulpprogramma's om actie ondernemen en integreren met service-oplossingen.
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) over het controleren van Azure.

### <a name="bcdr"></a>BCDR 

Contoso moet een strategie voor zakelijke continuïteit en noodherstel (BCDR) herstel voor hun Azure-resources.

- Azure biedt [ingebouwde functies voor BCDR](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications) houden gegevens veilig en apps/services ingesteld en geactiveerd. 
- Contoso wil naast de ingebouwde functies om ervoor te zorgen dat deze kunt van fouten herstellen, kostbare bedrijfsonderbrekingen te voorkomen dat aan nalevingsvereisten voldoet en beveilig gegevens tegen ransomware en menselijke fouten. Om dit te doen
    - Contoso implementeert Azure back-up als een kostenbesparende oplossing voor back-up van Azure-resources. Omdat deze ingebouwd is, kunt Contoso cloudback-ups in een paar eenvoudige stappen instellen.
    - Contoso wordt herstel na noodgevallen instellen voor virtuele Azure-machines met Azure Site Recovery voor replicatie, failover en failback tussen Azure-regio's die deze bevat.  Dit zorgt ervoor dat apps die worden uitgevoerd op Azure Virtual machines in een secundaire regio van het Contoso kiezen als er een storing optreedt in de primaire regio beschikbaar zal blijven. [Meer informatie](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart).

 
## <a name="conclusion"></a>Conclusie

In dit artikel, Contoso gepland voor een Azure-migratie op schaal.  Ze onderverdeeld het migratieproces in vier fasen. Van de evaluatie en migratie voltooid via optimalisatie, beveiliging en beheer nadat de migratie is. Voornamelijk, is het belangrijk om te plannen van een migratieproject als een hele proces, maar voor het migreren van systemen binnen een organisatie door sets splitsen in classificaties en getallen die geschikt zijn voor het bedrijf. Door gegevens vast te stellen en toepassen van classificaties, en het project kan worden opgesplitst in een reeks kleinere migraties die snel en veilig kunnen worden uitgevoerd.  De som van deze kleinere migraties omgezet snel in een grote geslaagde migratie naar Azure.
