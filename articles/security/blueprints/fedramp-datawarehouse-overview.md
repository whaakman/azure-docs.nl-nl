---
title: Azure-beveiliging en naleving blauwdruk - datawarehouse voor het automatiseren van FedRAMP
description: Azure-beveiliging en naleving blauwdruk - datawarehouse voor het automatiseren van FedRAMP
services: security
author: jomolesk
ms.assetid: 834d1ff6-8369-455f-b052-1ef301e3d7e6
ms.service: security
ms.topic: article
ms.date: 05/02/2018
ms.author: jomolesk
ms.openlocfilehash: 3c78aed2f30ea85f5bc16a8c0fb270bb1c761be8
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57539510"
---
# <a name="azure-security-and-compliance-blueprint-data-warehouse-for-fedramp-automation"></a>Azure-beveiliging en naleving blauwdruk: Datawarehouse voor automatisering van FedRAMP

## <a name="overview"></a>Overzicht

De [Federal Risk and Authorization Management Program (FedRAMP)](https://www.fedramp.gov/) Verenigde Staten government-is een programma is dat een gestandaardiseerde benadering voor de beveiligingsbeoordeling, de autorisatie en voortdurende bewaking biedt voor cloud producten en services. Deze Azure-beveiliging en naleving blauwdruk biedt richtlijnen voor het leveren van een Microsoft Azure datawarehouse-architectuur waarmee een subset van FedRAMP High besturingselementen implementeren. Deze oplossing biedt richtlijnen voor de implementatie en configuratie van Azure-resources voor een algemene referentiearchitectuur manieren waarin klanten specifieke vereisten voor beveiliging en naleving kunnen voldoen aan te tonen en fungeert als een basis voor klanten bouw en hun eigen datawarehouse-oplossingen in Azure te configureren.

Deze referentiearchitectuur, bijbehorende besturingselement implementatiehandleidingen en bedreigingsmodellen zijn bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving. Er is onvoldoende om volledig te voldoen aan de vereisten van de basislijn FedRAMP High implementeren van een toepassing in deze omgeving zonder aanpassingen. Houd rekening met het volgende:
- De architectuur biedt een basislijn om workloads naar Azure te implementeren op een manier die compatibel is met FedRAMP klanten te helpen.
- Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiliging en naleving evaluaties van een oplossing die is gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de uitvoering van elke klant.

## <a name="architecture-diagram-and-components"></a>Diagram van architectuur en onderdelen

Deze oplossing biedt een datawarehouse-referentiearchitectuur waarmee een krachtige en veilige cloud datawarehouse wordt geïmplementeerd. Er zijn twee afzonderlijke lagen in deze architectuur: één waar gegevens worden geïmporteerd, die zijn opgeslagen, en tijdelijk worden opgeslagen in een geclusterde SQL-omgeving, en een andere voor de Azure SQL Data Warehouse waar de gegevens zijn geladen met behulp van een ETL-hulpprogramma (bijvoorbeeld [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase)T-SQL-query's) voor verwerking. Wanneer gegevens worden opgeslagen in Azure SQL Data Warehouse, kunt analytics uitvoeren op grote schaal.

Microsoft Azure biedt tal van services voor rapportage en analyse voor de klant. Deze oplossing omvat SQL Server Reporting Services (SSRS) voor het snel maken van rapporten uit de Azure SQL Data Warehouse. Alle SQL-verkeer wordt versleuteld met SSL door de toevoeging van zelfondertekende certificaten. Als een best practice raadt Azure het gebruik van een vertrouwde certificeringsinstantie voor verbeterde beveiliging.

Gegevens in de Azure SQL Data Warehouse is opgeslagen in relationele tabellen met opslag in kolommen, een indeling die de kosten voor gegevensopslag aanzienlijk verlaagt en verbetert de prestaties van query's.  Afhankelijk van gebruiksvereisten, kunnen Azure SQL Data Warehouse-rekenresources worden omhoog of omlaag geschaald of volledig afgesloten als er geen actieve processen die rekenresources.

Een SQL-load balancer beheert SQL-verkeer, ervoor te zorgen dat de hoge prestaties. Alle virtuele machines in deze referentiearchitectuur implementeert in een beschikbaarheidsset met SQL Server-exemplaren die zijn geconfigureerd in een AlwaysOn-beschikbaarheidsgroep voor mogelijkheden voor hoge beschikbaarheid en herstel na noodgevallen.

Deze referentiearchitectuur voor datawarehouse bevat ook een laag Active Directory (AD) voor het beheren van resources in de architectuur. De Active Directory-subnet kan eenvoudig acceptatie onder een groter geheel voor AD-forest, zodat voor doorlopende werking van de omgeving, zelfs wanneer de toegang tot de grotere forest niet beschikbaar is. Alle virtuele machines worden toegevoegd aan de Active Directory-laag- en Active Directory-groepsbeleid gebruiken om af te dwingen van beveiliging en naleving configuraties op het niveau van het besturingssysteem.

Een virtuele machine fungeert als een bastionhost management biedt een beveiligde verbinding voor beheerders voor toegang tot geïmplementeerd resources. De gegevens zijn geladen in het faseringsgebied via deze bastionhost management. **Azure wordt aanbevolen voor het configureren van een VPN of Azure ExpressRoute-verbinding voor beheer en de gegevens importeren in het subnet van referentie-architectuur.**

![Datawarehouse voor FedRAMP verwijzing Architectuurdiagram](images/fedramp-datawarehouse-architecture.png?raw=true "Data Warehouse voor Architectuurdiagram van FedRAMP-verwijzing")

Deze oplossing maakt gebruik van de volgende Azure-services. Informatie van de implementatiearchitectuur vindt u in de [architectuur](#deployment-architecture) sectie.

Azure Virtual Machines
-   (1) bastionhost
-   (2) active Directory-domeincontroller
-   (2) SQL Server-clusterknooppunt
-   (1) SQL Server-Witness

Beschikbaarheidssets
-   (1) active Directory-domeincontrollers
-   (1) SQL-clusterknooppunten en -witness

Virtual Network
-   (4) subnetten
-   (4) de Netwerkbeveiligingsgroepen

SQL Data Warehouse

SQL Server Reporting Services

Azure SQL Load Balancer

Azure Active Directory

Recovery Services-kluis

Azure Key Vault

Azure Monitor-logboeken

## <a name="deployment-architecture"></a>Implementatie-architectuur

De volgende sectie bevat de elementen van de ontwikkeling en implementatie.

**SQL Data Warehouse**: [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) is een Enterprise Data Warehouse (EDW) die gebruikmaakt van uiterst (Massively Parallel Processing) als u wilt snel complexe query's uitvoeren voor petabytes aan gegevens. Big data importeren in SQL Data Warehouse met eenvoudige PolyBase T-SQL-query's en de kracht van MPP gebruiken om uit te voeren analyses met hoge prestaties.

**SQL Server Reporting Services**: [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) Hiermee kunt u snel maken van rapporten met tabellen, diagrammen, kaarten, meters, matrices en meer voor Azure SQL Data Warehouse.

**Bastionhost**: De bastionhost is de enkel ingangspunt waarmee gebruikers toegang krijgen tot de geïmplementeerde resources in deze omgeving. De bastionhost biedt een beveiligde verbinding met de geïmplementeerde resources door toe te staan alleen extern verkeer vanaf openbare IP-adressen op een veilige lijst. Als u wilt toestaan dat verkeer van extern bureaublad (RDP), moet de oorzaak van het verkeer in de Netwerkbeveiligingsgroep (NSG) worden gedefinieerd.

Een virtuele machine is gemaakt als een domein bastionhost met de volgende configuraties:
-   [Antimalware-uitbreiding](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Extensie van Azure Monitor-Logboeken](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure Diagnostics-extensie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) met behulp van Azure Key Vault (respecteert Azure Government, PCI DSS, HIPAA en andere vereisten)
-   Een [beleid voor automatisch afsluiten](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) verkleind van resources van de virtuele machine als deze niet in gebruik
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) ingeschakeld die referenties en andere geheimen in een beveiligde omgeving die is geïsoleerd van het besturingssysteem wordt uitgevoerd

### <a name="virtual-network"></a>Virtueel netwerk
Deze referentiearchitectuur definieert een particulier virtueel netwerk met een adresruimte van 10.0.0.0/16.

**Netwerkbeveiligingsgroepen**: [Nsg's](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bevatten toegangsbeheerlijsten (ACL's) die verkeer binnen een VNet toestaan of weigeren. Nsg's kunnen worden gebruikt om verkeer een subnet of afzonderlijke virtuele machine te beveiligen. De volgende nsg's bestaan:
  - Een NSG voor de gegevenslaag (SQL Server-Clusters, SQL Server-Witness en SQL Load Balancer)
  - Een NSG voor het beheer van bastionhost
  - Een NSG voor Active Directory
  - Een NSG voor SQL Server Reporting Services

Elk van de nsg's zijn bepaalde poorten en protocollen openen, zodat de oplossing kunt veilig en goed werken. Bovendien zijn de volgende configuraties voor elke NSG ingeschakeld:
  - [Diagnostische logboeken en gebeurtenissen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) zijn ingeschakeld en die zijn opgeslagen in een storage-account
  - Logboeken in Azure Monitor is verbonden met de [NSG van diagnostische gegevens](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Subnetten**: Elk subnet is gekoppeld aan de bijbehorende NSG.

### <a name="data-at-rest"></a>Data-at-rest
De architectuur beveiligt gegevens in rust via versleuteling, controle-database en andere metingen.

**Azure Storage** om versleutelde data-at-rest-vereisten te voldoen aan alle [Azure Storage](https://azure.microsoft.com/services/storage/) maakt gebruik van [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption).

**Azure Disk Encryption**
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows om volumeversleuteling voor het besturingssysteem en gegevensschijven te bieden. De oplossing kan worden geïntegreerd met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

**Azure SQL Database** de Azure SQL Database-exemplaar maakt gebruik van de volgende metingen van de database-beveiliging:
-   [AD-verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) maakt identiteitsbeheer van databasegebruikers en andere Microsoft-services op één centrale locatie.
-   [SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) databasegebeurtenissen bijgehouden en geschreven naar een auditlogboek in Azure storage-account.
-   SQL-Database is geconfigureerd voor het gebruik van [transparante gegevensversleuteling (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), die voert realtime versleuteling en ontsleuteling van gegevens en logboekbestanden om gegevens in rust te beveiligen. TDE biedt de zekerheid dat de opgeslagen gegevens niet is onderworpen aan onbevoegde toegang.
-   [Firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voorkomen dat u alle toegang tot de database-servers, totdat de juiste machtigingen zijn toegekend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   [SQL Threat Detection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunt u detectie en reactie op mogelijke bedreigingen wanneer deze zich voordoen, dankzij beveiligingswaarschuwingen voor verdachte databaseactiviteiten, potentiële kwetsbaarheden, SQL-injectieaanvallen en afwijkende database-toegang patronen.
-   [Altijd versleuteld kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) ervoor te zorgen dat gevoelige gegevens nooit wordt weergegeven als tekst zonder opmaak in de database-systeem. Na het inschakelen van versleuteling van gegevens, alleen clienttoepassingen of appservers met toegang tot de sleutels gegevens zijn toegankelijk als tekst zonder opmaak.
-   [SQL Database dynamische gegevensmaskering](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) kan worden uitgevoerd nadat de referentiearchitectuur implementeert. Klanten moet aanpassen, dynamische gegevensmaskering instellingen om te voldoen aan het schema van de database.

### <a name="business-continuity"></a>Bedrijfscontinuïteit
**Hoge beschikbaarheid**: Server-workloads worden gegroepeerd in een [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) om te zorgen voor hoge beschikbaarheid van virtuele machines in Azure. Ten minste één virtuele machine beschikbaar is tijdens gepland of ongepland onderhoud, voldoen aan de 99,95% Azure SLA.

**Recovery Services-kluis**: De [Recovery Services-kluis](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) -instellingen van de back-upgegevens en alle configuraties van Azure Virtual Machines in deze architectuur beveiligt. Met een Recovery Services-kluis, klanten kunnen bestanden en mappen terugzetten vanuit een IaaS-VM zonder te herstellen van de hele virtuele machine, waardoor sneller worden hersteld.

### <a name="logging-and-audit"></a>Logboekregistratie en controle
[Logboeken in Azure Monitor](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) biedt uitgebreide logboekregistratie van het systeem- en gebruikersactiviteit, evenals de status van het bestandssysteem. De [logboeken van Azure Monitor](https://azure.microsoft.com/services/log-analytics/) oplossing verzamelt en analyseert gegevens gegenereerd door resources in Azure en on-premises omgevingen.
- **Activiteitenlogboeken**: [Activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in bewerkingen die worden uitgevoerd op resources in een abonnement.
- **Diagnostische logboeken**: [Diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bevatten alle logboeken die zijn gegenereerd door elke resource. Deze logboeken bevatten de logboeken voor Windows-systeem en Azure Blob-opslag, tabellen en wachtrijlogboeken.
- **Firewall-logboeken**: De Application Gateway biedt volledige diagnostische en toegang tot logboeken. Firewall-logboeken zijn beschikbaar voor Application Gateway WAF-functionaliteit resources.
- **Logboek archiveren**: Alle logboeken met diagnostische gegevens schrijven naar een gecentraliseerd en versleutelde Azure storage-account voor archivering met een gedefinieerde bewaarperiode van twee dagen. Deze logboeken verbinden met Azure Monitor-logboeken voor verwerking, opslag en -dashboardrapporten.

Bovendien is de volgende bewakingsoplossingen, opgenomen als onderdeel van deze architectuur:
-   [AD-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): De oplossing statuscontrole van Active Directory beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Antimalware-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): De Antimalware-oplossing rapporteert over de status van schadelijke software, bedreigingen en beveiliging.
-   [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): De oplossing Azure Automation opgeslagen, wordt uitgevoerd en runbooks worden beheerd.
-   [Beveiliging en controle](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): Het dashboard beveiliging en controle biedt een hoogwaardig inzicht in de beveiligingsstatus van resources, dankzij metrische gegevens over beveiligingsdomeinen, problemen die aandacht vereisen, detecties, bedreigingsinformatie en algemene Beveiligingsquery's.
-   [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): De oplossing SQL-statuscontrole beoordeelt het risico en de gezondheid van server-omgevingen op een vast interval en biedt klanten een geprioriteerde lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde serverinfrastructuur.
-   [Updatebeheer](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): De oplossing Update Management kunt Klantenbeheer van updates voor besturingssysteem, met inbegrip van de status van de beschikbare updates en het installatieproces van vereiste updates.
-   [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): De oplossing status van Agent rapporteert het aantal agents zijn geïmplementeerd en hun geografische verdeling, evenals hoeveel agents die niet meer reageert en het aantal agents die zijn operationele gegevens kan verzenden.
-   [Azure-activiteitenlogboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): De oplossing Activity Log Analytics biedt ondersteuning voor analyse van de Azure-activiteitenlogboeken voor alle Azure-abonnementen voor een klant.
-   [Wijzigingen bijhouden](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): De oplossing wijzigingen bijhouden kan klanten eenvoudig wijzigingen in de omgeving identificeren.

### <a name="identity-management"></a>Identiteitsbeheer
De volgende technologieën bieden identiteit beheermogelijkheden in de Azure-omgeving:
-   [Active Directory (AD)](https://azure.microsoft.com/services/active-directory/) van Microsoft cloud-gebaseerde directory- en identiteitsbeheer management service met meerdere tenants kan zijn. Alle gebruikers voor de oplossing zijn gemaakt in Azure Active Directory, met inbegrip van gebruikers met toegang tot de SQL-Database.
-   Verificatie van de toepassing wordt uitgevoerd met behulp van Azure AD. Zie voor meer informatie, [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Bovendien de versleuteling van de kolom database maakt gebruik van Azure AD om te verifiëren van de toepassing naar Azure SQL Database. Zie voor meer informatie over het [bescherming van gevoelige gegevens in SQL-Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) mogelijke beveiligingsproblemen die betrekking hebben op van een organisatie-id's detecteert, configureert u automatische antwoorden op gedetecteerde verdachte activiteit met betrekking tot een organisatie-id's en onderzoekt het probleem verdachte incidenten, onderneem gepaste actie op te lossen.
-   [Azure Role-based Access Control (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) maakt gericht beheer van toegang voor Azure. Abonnementstoegang is beperkt tot de beheerder van het abonnement.

Zie voor meer informatie over het gebruik van de beveiligingsfuncties van Azure SQL Database, de [Contoso Clinic-Demotoepassing](https://github.com/Microsoft/azure-sql-security-sample) voorbeeld.

### <a name="security"></a>Beveiliging
**Geheimen management**: De oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt.

**Bescherming tegen schadelijke software**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor virtuele Machines biedt realtime-beveiliging-functie die helpt te identificeren en verwijderen van virussen, spyware en andere schadelijke software, met configureerbare meldingen wanneer bekende schadelijke of ongewenste software wil installeren of uitvoeren op de beveiligde virtuele machines.

**Patchbeheer**: Windows virtuele machines geïmplementeerd als onderdeel van deze referentiearchitectuur worden standaard automatisch updates ontvangen van Windows Update-Service geconfigureerd. Deze oplossing bevat ook de [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) service waarmee bijgewerkte implementaties kunnen worden gemaakt voor de patch voor virtuele machines wanneer dat nodig is.


## <a name="guidance-and-recommendations"></a>Richtlijnen en aanbevelingen
### <a name="expressroute-and-vpn"></a>ExpressRoute- en VPN
[ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of een beveiligde VPN-tunnel moet worden geconfigureerd voor een veilig verbinding met de resources die zijn geïmplementeerd als onderdeel van deze referentiearchitectuur voor datawarehouse. Als het ExpressRoute-verbindingen gaan niet via het Internet, bieden deze verbindingen een meer betrouwbaarheid, hogere snelheden, kortere wachttijden en hogere beveiliging dan gebruikelijke verbindingen via Internet. Door op de juiste wijze instellen van ExpressRoute of een VPN-verbinding, kunnen klanten een beveiligingslaag voor gegevens in transit toevoegen.

### <a name="extract-transform-load-etl-process"></a>Proces extract-Transform-Load (ETL)
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) kunnen gegevens laden in Azure SQL Data Warehouse zonder de noodzaak voor een afzonderlijke ETL of hulpprogramma voor importeren. PolyBase biedt toegang tot gegevens via T-SQL-query's. Microsoft business intelligence en analyse-stack, evenals hulpprogramma's van derden die compatibel is met SQL Server, kunnen worden gebruikt met PolyBase.

### <a name="azure-active-directory-setup"></a>Installatie van de Azure Active Directory
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) is essentieel voor het beheren van de implementatie en inrichting van toegang tot personeel interactie met de omgeving. Een bestaande Windows Server Active Directory kunnen worden geïntegreerd met AAD in [vier klikken](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Klanten kunnen ook de geïmplementeerde Active Directory-infrastructuur (domeincontrollers) koppelen aan een bestaande AAD doordat de geïmplementeerde Active Directory-infrastructuur een subdomein van een AAD-forest.

### <a name="additional-services"></a>Extra services
Hoewel deze datawarehouse-architectuur is niet bedoeld voor implementatie naar de [Azure Commercial](https://azure.microsoft.com/overview/what-is-azure/) omgeving, hetzelfde doel kunnen worden bereikt via de services die worden beschreven in deze referentiearchitectuur, evenals aanvullende services die alleen beschikbaar in de commerciële Azure-omgeving. Houd er rekening mee dat Azure Commercial een FedRAMP JAB P-ATO op het niveau van matige Impact onderhoudt, overheidsinstellingen worden gesteld en partners te implementeren dat gevoelige informatie naar de cloud gebruik te maken van de commerciële Azure-omgeving.

Commerciële Azure biedt die een groot aantal services die ingang geformatteerd en opslag van niet-opgemaakte gegevens en in fasering moet worden gebruikt in de gegevensopslag, met inbegrip van:
-   [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) is een beheerde cloudservice die is gebouwd voor complexe hybride extract-transform-load (ETL), extract-load-transform (ELT), en gegevensintegratieprojecten. Met Azure Data Factory, kunnen klanten maken en plannen van gegevensgestuurde werkstromen pijplijnen die gegevens uit verschillende gegevensarchieven opnemen genoemd. Klanten kunnen vervolgens verwerken en transformeren van de gegevens voor uitvoer naar gegevensarchieven zoals Azure SQL Data Warehouse.
-   [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) kunt u het vastleggen van gegevens van elke grootte, soort en opnamesnelheid op één plek voor operationele en verkennende analyse. Azure Data Lake Store is compatibel met de meeste open source-onderdelen in het Hadoop-ecosysteem en mooi integreert met andere Azure-services zoals Azure SQL Data Warehouse.

## <a name="threat-model"></a>Risicomodel

De gegevensstroom-diagram (GSD) voor deze referentiearchitectuur is beschikbaar voor [downloaden](https://aka.ms/blueprintdwthreatmodel) of vindt u hieronder:

![Datawarehouse voor FedRAMP risicomodel](images/fedramp-datawarehouse-threat-model.png?raw=true "Data Warehouse voor risicomodel FedRAMP")

## <a name="compliance-documentation"></a>Naleving-documentatie
De [Azure-beveiliging en naleving blauwdruk – FedRAMP hoog klant verantwoordelijkheid Matrix](https://aka.ms/blueprinthighcrm) geeft een lijst van alle beveiligingsmaatregelen die zijn vereist voor de basislijn FedRAMP High. Op dezelfde manier de [Azure-beveiliging en naleving blauwdruk – FedRAMP gemiddeld klant verantwoordelijkheid Matrix](https://aka.ms/blueprintcrmmod) geeft een lijst van alle beveiligingsmaatregelen die zijn vereist voor de basislijn FedRAMP gemiddeld. Beide documenten beschreven of de implementatie van elk besturingselement de verantwoordelijkheid van Microsoft, de klant is, of gedeeld tussen de twee.

De [Azure-beveiliging en blauwdruk voor naleving - FedRAMP hoog besturingselement implementatie Matrix](https://aka.ms/blueprintdwcimhigh) en de [Azure-beveiliging en blauwdruk voor naleving - FedRAMP gemiddeld besturingselement implementatie Matrix](https://aka.ms/blueprintdwcimmod) bieden informatie over waarop besturingselementen worden gedekt door de datawarehouse-architectuur voor elke basislijn FedRAMP, met inbegrip van gedetailleerde beschrijvingen van de manier waarop de implementatie voldoet aan de vereisten van elk besturingselement vallen.

## <a name="disclaimer"></a>Vrijwaring

 - Dit document is uitsluitend ter informatie bedoeld. MICROSOFT BIEDT GEEN GARANTIES, EXPLICIETE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en meningen in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Klanten die in dit document leest draagt het risico van het gebruik ervan.
 - Dit document biedt klanten met een enkel wettelijk recht op enig intellectueel in andere Microsoft-producten of oplossingen.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot grotere hoeveelheden gegevens, netwerk- of gebruik van de compute-bronnen in Azure en de Azure-licentie of abonnement kosten van een klant kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als een basis voor klanten om aan te passen op hun specifieke behoeften en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als referentie en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en voorschriften. Klanten moeten juridische ondersteuning van hun organisatie op goedgekeurde klantimplementaties gezocht.
