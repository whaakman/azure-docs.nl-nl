---
title: Blauwdruk Azure Automation - webtoepassingen voor FedRAMP
description: Blauwdruk Azure Automation - webtoepassingen voor FedRAMP
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 8fe47cff-9c24-49e0-aa11-06ff9892a468
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: d0521d68bab8bd0b7db53a512da6d37033abd85e
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="azure-blueprint-automation---web-applications-for-fedramp"></a>Blauwdruk Azure Automation - webtoepassingen voor FedRAMP

## <a name="overview"></a>Overzicht

De [Federal risico en autorisatie Management-programma (FedRAMP)](https://www.fedramp.gov), is een US government hele-programma dat biedt een gestandaardiseerde aanpak voor beoordeling van de veiligheid, autorisatie en doorlopende bewaking voor cloud-producten en Services. Deze Azure blauwdruk Automation - webtoepassingen voor FedRAMP biedt richtlijnen voor de implementatie van een FedRAMP-compatibele infrastructuur als een geschikt is voor een eenvoudige webtoepassing voor internetgerichte Service (IaaS)-omgeving. Deze oplossing automatiseert implementatie en configuratie van Azure-resources voor een algemene referentiearchitectuur demonstreren van de manieren waarop klanten kunnen voldoen aan specifieke vereisten voor beveiliging en naleving en fungeert als basis voor klanten om op te bouwen en oplossingen voor hun eigen configureren in Azure. De oplossing implementeert een subset van besturingselementen van de basislijn FedRAMP hoge, op basis van NIST SP 800 53. Zie voor meer informatie over deze oplossing en FedRAMP hoge [FedRAMP hoge eisen - High-Level overzicht](fedramp-controls-overview.md). ***Opmerking: Deze oplossing worden geïmplementeerd op Azure Government.***

Deze architectuur is bedoeld om te fungeren als basis voor klanten om aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving. Implementeren van een toepassing in deze omgeving zonder aanpassing is niet voldoende volledig voldoen aan de vereisten van de basislijn FedRAMP hoog. Houd rekening met het volgende:
- Deze architectuur biedt een basis zodat klanten Azure in een FedRAMP-compatibele manier gebruiken.
- Klanten zijn verantwoordelijk voor het uitvoeren van de juiste beveiligingsrechten en beoordeling van compatibiliteit van een oplossing die zijn gebouwd met behulp van deze architectuur als vereisten kunnen variëren op basis van de details van de implementatie van de klant. 

Voor een snel overzicht van de werking van deze oplossing, bekijkt u deze [video](https://aka.ms/fedrampblueprintvideo) waarin wordt uitgelegd en demonstreren van de implementatie ervan.

Klik op [hier](https://aka.ms/fedrampblueprintrepo) voor implementatie-instructies.

## <a name="solution-components"></a>Oplossingsonderdelen

Deze blauwdruk Azure Automation wordt automatisch een referentiearchitectuur IaaS web application met vooraf geconfigureerde beveiligingsmechanismen klanten helpen de naleving van voorschriften FedRAMP implementeert. De oplossing bestaat uit Azure Resource Manager-sjablonen en PowerShell-scripts die resource implementatie en configuratie leiden. Bij Azure blauwdruk [naleving documentatie](#compliance-documentation) wordt geleverd, die aangeeft beveiliging besturingselement overname van Azure en de geïmplementeerde resources en configuraties die zijn afgestemd op NIST SP 800 53 beveiligingsmechanismen, waardoor waarmee organisaties fast track verplichtingen.

## <a name="architecture-diagram"></a>Architectuurdiagram

Deze oplossing implementeert een referentiearchitectuur voor een IaaS-webtoepassing met een back-end voor de database. De architectuur bevat een weblaag, gegevens servicetier, Active Directory-infrastructuur, toepassingsgateway, en de load balancer. Virtuele machines die zijn geïmplementeerd op de web- en lagen zijn geconfigureerd in een beschikbaarheidsset en SQL Server-exemplaren zijn geconfigureerd in een AlwaysOn-beschikbaarheidsgroep voor hoge beschikbaarheid. Virtuele machines zijn lid van een domein en Active Directory-groepsbeleid worden gebruikt voor het afdwingen van beveiliging en naleving configuraties op het niveau van het besturingssysteem. Een management jumpbox (bastion host) biedt een beveiligde verbinding voor beheerders voor geïmplementeerd toegang tot bronnen.

![alternatieve tekst](images/fedramp-architectural-diagram.png?raw=true "IaaS web application blauwdruk automation voor FedRAMP-compatibele omgevingen")

Deze oplossing gebruikt de volgende Azure-services. Details van de architectuur voor implementatie bevinden zich in de [architectuur voor implementatie](#deployment-architecture) sectie.

* **Virtuele Machines in Azure**
    - (1) management/bastion (Windows Server 2016 Datacenter)
    - (2) active Directory-domeincontroller (Windows Server 2016 Datacenter)
    - (2) de SQL Server-clusterknooppunt (SQL Server 2016 op Windows Server 2012 R2)
    - (1) de SQL Server-witness (Windows Server 2016 Datacenter)
    - (2) web/IIS (Windows Server 2016 Datacenter)
* **Beschikbaarheidssets**
    - (1) active Directory-domeincontrollers
    - (1) de SQL-clusterknooppunten en -witness
    - (1) web/IIS
* **Azure Virtual Network**
    - (((1) /16 virtuele netwerken
    - (5) /24 subnetten
    - DNS-instellingen zijn ingesteld op beide domeincontrollers
* **Azure Load Balancer**
    - (1) de SQL-taakverdeler
* **Azure Application Gateway**
    - (1) WAF Application Gateway is ingeschakeld
      - Firewall-modus: voorkomen
      - Regelset: OWASP 3.0
      - Listener: Poort 443
* **Azure Storage**
    - (7) Geo-redundant storage-accounts
* **Azure Backup**
    - (1) de recovery Services-kluis
* **Azure Key Vault**
    - (1) de Sleutelkluis
* **Azure Active Directory**
* **Azure Resource Manager**
* **Azure Log Analytics**
* **Azure Automation**
    - (1) de automation-account
* **Operations Management Suite**
    - (1) de OMS-werkruimte

## <a name="deployment-architecture"></a>Architectuur voor implementatie

De volgende sectie wordt de ontwikkeling en implementatie-elementen.

### <a name="network-segmentation-and-security"></a>Netwerksegmentering en beveiliging

#### <a name="application-gateway"></a>Application Gateway

De architectuur vermindert het risico van beveiligingsproblemen met behulp van een toepassingsgateway met web application firewall (WAF) en de ruleset OWASP is ingeschakeld. Aanvullende mogelijkheden zijn:

- [Einde End SSL-](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Schakel [SSL-Offload](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Schakel [TLS v1.0 en v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web application firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF modus)
- [Preventie modus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) met OWASP 3.0 ruleset

#### <a name="virtual-network"></a>Virtueel netwerk

De architectuur definieert een persoonlijke virtueel netwerk met een adresruimte van 10.200.0.0/16.

#### <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen

Deze oplossing implementeert bronnen in een architectuur met een afzonderlijke web subnet, database-subnet, Active Directory-subnet en management subnet binnen een virtueel netwerk. Subnetten worden logisch gescheiden door netwerkbeveiligingsgroepen toegepast op de afzonderlijke subnetten voor verkeer tussen subnetten alleen noodzakelijke systeem en de beheerfunctionaliteit te beperken.

Zie de configuratie voor [Netwerkbeveiligingsgroepen](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) geïmplementeerd met deze oplossing. Organisaties kunnen netwerk-beveiligingsgroepen configureren door het bewerken van het bestand dan het gebruik van [deze documentatie](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) als richtlijn.

Elk van de subnetten heeft een speciale netwerkbeveiligingsgroep (NSG):
- 1 NSG voor Application Gateway (LBNSG)
- 1 NSG voor Jumpbox (MGTNSG)
- 1 NSG voor primaire en back-domeincontrollers (ADNSG)
- 1 NSG voor SQL-Servers en bestandsshare-Witness (SQLNSG)
- 1 NSG voor weblaag (WEBNSG)

#### <a name="subnets"></a>Subnetten

Elk subnet is gekoppeld aan de bijbehorende NSG.

### <a name="data-at-rest"></a>Gegevens in rust

De architectuur beveiligt de gegevens in rust met behulp van verschillende versleuteling metingen.

#### <a name="azure-storage"></a>Azure Storage

Om te voldoen aan versleutelingsvereisten voor de van gegevens in rust, het gebruik van alle opslagaccounts [Opslagversleuteling Service](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

#### <a name="sql-database"></a>SQL Database

SQL-Database is geconfigureerd voor gebruik [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption), uitvoert, wordt er realtime versleuteling en ontsleuteling van gegevens en logboekbestanden om gegevens in rust te beveiligen. TDE biedt de zekerheid dat de opgeslagen gegevens niet is onderworpen aan onbevoegde toegang. 

#### <a name="azure-disk-encryption"></a>Azure Disk Encryption

Azure Disk Encryption wordt gebruikt om versleutelde Windows IaaS-schijven voor virtuele machine. [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows voor volumeversleuteling voor het besturingssysteem en gegevensschijven. De oplossing is geïntegreerd met Azure Key Vault voor het beheren en beheren van de versleutelingssleutels op de schijf.

### <a name="logging-and-auditing"></a>Logboekregistratie en controle

[Operations Management Suite (OMS)](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) biedt uitgebreide logboekregistratie van systeem- en gebruikersactiviteit evenals systeemstatus. 

- **Activiteitenlogboeken:**[activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement.  
- **Diagnostische logboeken:**[diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) zijn alle logboeken die door elke resource.   Deze logboeken bevatten Windows-gebeurtenislogboeken system, Azure-opslag logboeken Sleutelkluis controlelogboeken en Application Gateway toegang en firewall-Logboeken.
- **Logboek archiveren:** Azure activiteitenlogboeken en logboeken met diagnostische gegevens kunnen worden verbonden met Azure Log Analytics voor verwerking, opslaan en dashboarding. Bewaartermijn is om te voldoen aan vereisten voor de bewaarperiode van de organisatie-specifieke gebruiker configureerbare up 730 dag.

### <a name="secrets-management"></a>Geheimen management

De oplossing maakt gebruik van Azure Sleutelkluis sleutels en geheimen beheren.

- [Azure Sleutelkluis](https://azure.microsoft.com/services/key-vault/) u beveiligt cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services. 
- De oplossing is geïntegreerd met Azure Key Vault voor het beheren van IaaS virtuele machine schijfversleuteling sleutels en geheimen.

### <a name="identity-management"></a>Identiteitsbeheer

De volgende technologieën bieden identiteit beheermogelijkheden in de Azure-omgeving.
- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) van Microsoft multitenant cloud-gebaseerde directory en identity management-service is.
- Verificatie voor een webtoepassing die door de klant geïmplementeerd kan worden uitgevoerd met behulp van Azure AD. Zie voor meer informatie [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  
- [Azure op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) kunt nauwkeurig gerichte toegangsbeheer voor Azure. Abonnement toegang is beperkt tot abonnementsbeheerder en toegang tot bronnen worden beperkt op basis van gebruikersrol.
- Een geïmplementeerd exemplaar van Active Directory IaaS biedt identiteitsbeheer op de OS-niveau voor geïmplementeerde IaaS virtuele machines.
   
### <a name="compute-resources"></a>Bronnen berekenen

#### <a name="web-tier"></a>Weblaag

De oplossing implementeert web laag virtuele machines in een [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Beschikbaarheidssets Zorg ervoor dat de virtuele machines zijn verdeeld over meerdere geïsoleerde hardware clusters voor het verbeteren van beschikbaarheid.

#### <a name="database-tier"></a>Databaselaag

De oplossing implementeert database laag virtuele machines in een Beschikbaarheidsset als een [AlwaysOn-beschikbaarheidsgroep](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview). De altijd op beschikbaarheid van groep functie biedt voor hoge beschikbaarheid en herstel na noodgevallen mogelijkheden. 

#### <a name="active-directory"></a>Active Directory

Alle virtuele machines die zijn geïmplementeerd door de oplossing zijn lid van een domein en Active Directory-groepsbeleid worden gebruikt voor het afdwingen van beveiliging en naleving configuraties op het niveau van het besturingssysteem. Active Directory-virtuele machines zijn geïmplementeerd in een Beschikbaarheidsset.


#### <a name="jumpbox-bastion-host"></a>Jumpbox (bastion host)

Een management jumpbox (bastion host) biedt een beveiligde verbinding voor beheerders voor geïmplementeerd toegang tot bronnen. Het NSG die is gekoppeld aan het beheer van subnet waar de jumpbox virtuele machine zich bevindt, kunnen verbindingen alleen via TCP-poort 3389 voor RDP. 

### <a name="malware-protection"></a>Bescherming tegen schadelijke software

[Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor virtuele Machines biedt de mogelijkheid van realtime-beveiliging waarmee identificeren en te verwijderen van virussen, spyware en andere schadelijke software, met configureerbare waarschuwingen wanneer bekende schadelijke of ongewenste software probeert installeren of uitvoeren op de beveiligde virtuele machines.

### <a name="patch-management"></a>Patch management

Windows virtuele machines zijn geïmplementeerd door deze automatisering blauwdruk zijn standaard geconfigureerd voor automatische updates ontvangen van Windows Update-Service. Deze oplossing implementeert ook de OMS Azure Automation-oplossing waarmee Update-implementaties kunnen worden gemaakt voor het implementeren van patches op Windows-servers wanneer deze nodig is.

### <a name="operations-management"></a>Operationeel beheer

#### <a name="log-analytics"></a>Log Analytics

[Meld u Analytics](https://azure.microsoft.com/services/log-analytics/) is een service in Operations Management Suite (OMS) waarmee verzamelen en analyseren van gegevens die zijn gegenereerd voor resources in Azure en on-premises omgevingen.

#### <a name="oms-solutions"></a>OMS-oplossingen

De volgende OMS-oplossingen zijn vooraf geïnstalleerd als onderdeel van deze oplossing:
- [AD-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment)
- [Antimalware-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware)
- [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker)
- [Beveiliging en controle](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started)
- [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment)
- [Updatebeheer](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management)
- [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth)
- [Azure activiteitenlogboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity)
- [Tracering wijzigen](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity)

## <a name="compliance-documentation"></a>Documentatie voor naleving

### <a name="customer-responsibility-matrix"></a>Klant verantwoordelijkheid matrix

De [klant verantwoordelijkheden matrix](https://aka.ms/blueprinthighcrm) (Excel-werkmap) een lijst met alle beveiligingsmechanismen zijn vereist voor de basislijn FedRAMP hoog. De matrix geeft, voor elk besturingselement (of besturingselement subdeel) of implementatie verantwoorde voor het besturingselement de verantwoordelijkheid van Microsoft, de klant is of gedeeld tussen de twee. 

### <a name="control-implementation-matrix"></a>Besturingselement implementatie matrix

De [besturingselement implementatie matrix](https://aka.ms/blueprintwacim) (Excel-werkmap) een lijst met alle beveiligingsmechanismen zijn vereist voor de basislijn FedRAMP hoog. De matrix geeft, voor elk besturingselement (of het besturingselement subdeel) die is aangewezen een klant verantwoorde in de klant verantwoordelijkheden matrix (1) als de blauwdruk automatisering wordt geïmplementeerd voor het besturingselement en 2) een beschrijving van hoe de uitvoering wordt uitgelijnd met de controle van vereisten. Deze inhoud is ook beschikbaar [hier](fedramp-controls-overview.md).

## <a name="deploy-the-solution"></a>De oplossing implementeren

Deze oplossing Azure blauwdruk bestaat uit JSON-configuratiebestanden en PowerShell-scripts die worden verwerkt door Azure Resource Manager-API-service voor het implementeren van resources in Azure. Gedetailleerde implementatie-instructies zijn beschikbaar [hier](https://aka.ms/fedrampblueprintrepo). ***Opmerking: Deze oplossing worden geïmplementeerd op Azure Government.***

#### <a name="quickstart"></a>Snelstartgids
1. Klonen of downloaden [dit](https://aka.ms/fedrampblueprintrepo) GitHub-opslagplaats naar uw lokale werkstation.

2. Voer de PowerShell-script vóór implementatie: azure-blueprint/predeploy/Orchestration_InitialSetup.ps1.

3. Klik op de knop hieronder, meld u aan bij de Azure-portal, voer de vereiste parameters voor ARM-sjabloon en klikt u op **aankoop**.

    [![Implementeren in Azure](http://azuredeploy.net/AzureGov.png)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Ffedramp-iaas-webapp%2Fmaster%2Fazuredeploy.json)

## <a name="disclaimer"></a>Vrijwaring

- Dit document is alleen ter informatie. MICROSOFT GEEFT GEEN GARANTIES, SNELLE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en inzichten die in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Dit document lezen klanten draagt het risico voor het gebruik ervan.  
- Dit document biedt geen enkel wettelijk recht op enig intellectueel eigendom van alle Microsoft-product of oplossingen klanten.  
- Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.  
- Bepaalde aanbevelingen in dit document kunnen leiden tot hogere-, netwerk- of compute-Resourcegebruik in Azure en een klant Azure licentie of abonnement kosten kunnen verhogen.  
- Deze architectuur is bedoeld om te fungeren als basis voor klanten om aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving.
- Dit document is ontwikkeld als een verwijzing en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en -voorschriften. Klanten moeten juridische ondersteuning van hun organisatie met goedgekeurde klant implementaties zoeken.
