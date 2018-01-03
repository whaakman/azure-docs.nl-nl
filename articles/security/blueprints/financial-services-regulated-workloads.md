---
title: "Blauwdruk Azure Automation - financiële diensten voor gereglementeerde werkbelastingen"
description: "In financiële diensten blauwdruk voor gereglementeerde werkbelastingen"
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 17794288-9074-44b5-acc8-1dacceb3f56c
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2017
ms.author: frasim
ms.openlocfilehash: 19e26c16866dada8dcff04a520ce4c208d67c365
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="azure-blueprint-automation-financial-services-blueprint-for-regulated-workloads"></a>Blauwdruk Azure Automation: Financiële diensten blauwdruk voor gereglementeerde werkbelastingen

## <a name="overview"></a>Overzicht

Financiële diensten blauwdruk voor geregeld werkbelastingen kunt u een beveiligd en compatibel platform implementeren als een service (PaaS)-webtoepassing die is ontworpen voor het verwerken van gevoelige gegevens in de cloud. De blauwdruk bestaat uit geautomatiseerde scripts en richtlijnen over een eenvoudige referentiearchitectuur en een ontwerp dat helpt te vereenvoudigen, de acceptatie van Microsoft Azure-oplossingen. Deze blauwdruk ziet u een end-to-end-oplossing om te voldoen aan de behoeften van organisaties zoek naar manieren om de belasting en de kosten voor het implementeren van in de cloud te verminderen.

Deze blauwdruk is ontworpen om te voldoen aan de vereisten van strenge compatibele normen die zijn ingesteld door de Amerikaanse Institute van gecertificeerd openbare Accountants zoals - SOC 1 SOC 2, de Payment Card Industry Data Security Standards-Raad DSS 3.2 en FFIEC voor de verzamelen, opslaan en ophalen van gevoelige financiële gegevens. Laat de juiste verwerking van dergelijke gegevens door het implementeren van een oplossing voor beheer van financiële gegevens in een omgeving met veilige, voldoen aan het beleid, meerdere lagen. De oplossing is geïmplementeerd als een end-to-end PaaS Azure gebaseerde oplossing. 

De blauwdruk is bedoeld om te fungeren als basis voor klanten om te bouwen op en begrip van de vereisten van het beheer van beveiligde gegevens in de cloud. De oplossing moet niet worden gebruikt in een productie-implementatie als-is, maar om te begrijpen, ontwerpen en implementeren van Azure services; het ontworpen als basislijn waarmee klanten kunnen Microsoft Azure gebruiken op een manier beveiligd en compatibel zijn.

Auditor erkende moet een klant-oplossing voor productie is gebaseerd op deze blauwdruk certificeren. Oplossingen kunnen variëren, afhankelijk van de details van de implementatie en de locatie van elke klant.

Voor een snel overzicht van de werking van deze oplossing, bekijkt u deze [video](https://aka.ms/fsiblueprintvideo) die wordt beschreven en ziet u de implementatie ervan.

## <a name="solution-components"></a>Oplossingsonderdelen

De architectuur bestaat uit de volgende onderdelen en de implementatiemogelijkheden van de naleving Azure PCI DSS oplossing gebruikt.

- **Architectuurdiagram**. Het diagram toont de referentiearchitectuur voor de oplossing Contoso Webstore gebruikt.
- **Implementatiesjablonen**. In deze implementatie [Azure Resource Manager-sjablonen](/azure/azure-resource-manager/resource-group-overview#template-deployment) worden gebruikt voor de onderdelen van de architectuur automatisch implementeren in Microsoft Azure door te geven configuratieparameters tijdens de installatie.
- **Geautomatiseerde implementatiescripts**. Deze scripts helpen bij het implementeren van de end-to-end-oplossing. De scripts bestaan uit:
    - Een installatie van de module en [hoofdbeheerder](/azure/active-directory/active-directory-assign-admin-roles-azure-portal) installatiescript wordt gebruikt om te installeren en te controleren of vereiste PowerShell-modules en -rollen van de globale beheerder correct zijn geconfigureerd. 
    - Installatie van een PowerShell-script wordt gebruikt voor het implementeren van de end-to-end-oplossing, die beschikbaar zijn via een ZIP-bestand en een Bacpac-bestand met een vooraf samengestelde demo-webtoepassing met [SQL-databasevoorbeeld](https://github.com/Microsoft/azure-sql-security-sample). inhoud. De broncode voor deze oplossing beschikbaar is voor controle is [betaling verwerken blauwdruk code opslagplaats][code-repo]. 

## <a name="architectural-diagram"></a>Architectuurdiagram

![](images/pci-architectural-diagram.png)

## <a name="user-scenario"></a>Scenario voor gebruikers

De blauwdruk adressen van de volgende het onderstaande gebruiksvoorbeeld.

> Dit scenario ziet u hoe een fictieve webarchief verplaatst gevoelige gegevens in een PaaS-cloud op basis van een Azure-oplossing. De Voorbeeldoplossing ziet u de verwerking en het verzamelen van informatie basisgebruiker en geselecteerde gevoelige gegevens. Dit werk voortbouwt op de blauwdruk Azure Automation: betaling verwerken voor PCI DSS-compatibele omgevingen voor de verwerking van de kaart betaling. Voor meer informatie uit te breiden aan dit werk ["Bekijken en richtlijnen voor implementatie"](https://aka.ms/pciblueprintprocessingoverview) artikel biedt een overzicht van de PCI-DSS-compatibele omgevingen.

### <a name="use-case"></a>Gebruiksvoorbeeld
Een kleine webarchief aangeroepen *Contoso Webstore* is gereed om financiële gegevens met informatie van de klant betaling naar de cloud te verplaatsen. 

De beheerder van Contoso Webstore zoekt een oplossing die snel kan worden geïmplementeerd om te zijn doelstellingen te bereiken. Hij wordt dit bewijs-van-concept (POC) gebruiken om te bespreken met diens belanghebbenden hoe Azure te verzamelen, opslaan en ophalen van financiële gegevens op basis van strenge nalevingsvereisten kan worden gebruikt.

> U verantwoordelijk voor het uitvoeren van de juiste beveiliging en naleving beoordelingen van een oplossing die zijn gebouwd met de architectuur die wordt gebruikt door deze Implementatiemodel als vereisten kunnen variëren op basis van de details van uw implementatie en Geografie. 

### <a name="elements-of-the-foundational-architecture"></a>Elementen van de fundamentele-architectuur

De fundamentele architectuur is ontworpen met de volgende fictieve elementen:

Domein-site`contosowebstore.com`

Gebruikersrollen ingezet illustreren het gebruiksvoorbeeld en bieden inzicht in de gebruikersinterface.

#### <a name="role-site-and-subscription-admin"></a>Rol: De Site en abonnement admin

|Item      |Voorbeeld|
|----------|------|
|Gebruikersnaam: |`adminXX@contosowebstore.com`|
| Naam: |`Global Admin Azure PCI Samples`|
|Gebruikerstype:| `Subscription Administrator and Azure Active Directory Global Administrator`|

- De account van de beheerder kan de financiële gegevens ontmaskerd niet lezen. Alle acties worden vastgelegd.
- De account van de beheerder kan beheren of meld u aan bij de SQL-Database.
- Het beheerdersaccount kan Active Directory en abonnementen beheren.

#### <a name="role-sql-administrator"></a>-Functie: De beheerder van de SQL

|Item      |Voorbeeld|
|----------|------|
|Gebruikersnaam: |`sqlAdmin@contosowebstore.com`|
| Naam: |`SQLADAdministrator PCI Samples`|
| Voornaam: |`SQL AD Administrator`|
|Achternaam: |`PCI Samples`|
|Gebruikerstype:| `Administrator`|

- Het account sqladmin weergeven niet ongefilterde financiële gegevens. Alle acties worden vastgelegd.
- De sqladmin kan SQL-Database beheren.

#### <a name="role-clerk"></a>Rol: Clerk

|Item      |Voorbeeld|
|----------|------|
|Gebruikersnaam:| `receptionist_EdnaB@contosowebstore.com`|
| Naam: |`Edna Benson`|
| Voornaam:| `Edna`|
|Achternaam:| `Benson`|
| Gebruikerstype: |`Member`|

Edna Benson is de manager receptionist en bedrijven. Ze is verantwoordelijk voor het garanderen dat klantgegevens nauwkeurig is en facturering is voltooid. Erna is de gebruiker is aangemeld voor alle interacties met de Contoso Webstore demo-website. Erna heeft de volgende rechten: 

- Erna kunt maken en klantgegevens worden gelezen.
- Erna kunt klantgegevens wijzigen.
- Erna kunt financiële gegevens te overschrijven.
- Erna account weergeven niet ongefilterde financiële gegevens.

> In de Contoso Webstore, wordt de gebruiker automatisch de **erna** gebruiker voor het testen van de mogelijkheden van de geïmplementeerde omgeving.

### <a name="contoso-webstore---estimated-pricing"></a>Contoso Webstore - geschatte prijzen

Deze fundamentele architectuur en voorbeeld-webtoepassing hebben een maandelijkse kostenstructuur en gebruikskosten per uur dat bij het formaat van de oplossing moet worden beschouwd. Deze kosten worden geschat met behulp van de [Azure goedkoper Rekenmachine](https://azure.microsoft.com/pricing/calculator/). Vanaf September 2017 de geschatte maandelijkse kosten voor deze oplossing is ~ $2500 hierbij $1000 per maand gebruik kosten met zich mee voor v2 as-omgeving. Deze kosten variëren, afhankelijk van de hoeveelheid en nog worden gewijzigd. Het is aan de klant voor het berekenen van de geschatte kosten van het maandelijkse op het moment van implementatie voor een meer nauwkeurige schatting. 

Deze oplossing gebruikt de volgende Azure-services. Details van de architectuur voor implementatie bevinden zich in de [architectuur voor implementatie](#deployment-architecture) sectie.

>- Application Gateway
>- Azure Active Directory
>- App Service-omgeving v2
>- OMS Log Analytics
>- Azure Key Vault
>- Netwerkbeveiligingsgroepen
>- Azure SQL Database
>- Azure Load Balancer
>- Application Insights
>- Azure Security Center
>- Azure Web App
>- Azure Automation
>- Azure Automation-Runbooks
>- Azure DNS
>- Azure Virtual Network
>- Azure Virtual Machine
>- Azure-resourcegroep en beleidsregels
>- Azure Blob Storage
>- Azure Active Directory-rollen gebaseerd toegangsbeheer (RBAC)

## <a name="deployment-architecture"></a>Architectuur voor implementatie

De volgende sectie wordt de ontwikkeling en implementatie-elementen.

### <a name="network-segmentation-and-security"></a>Netwerksegmentering en beveiliging

![](images/pci-tiers-diagram.png)

#### <a name="application-gateway"></a>Application Gateway

De fundamentele architectuur vermindert het risico van beveiligingsproblemen met behulp van een toepassingsgateway met een web application firewall (WAF) en de ruleset OWASP is ingeschakeld. Aanvullende mogelijkheden zijn:

- [Einde End SSL-](/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [SSL-Offload](/azure/application-gateway/application-gateway-ssl-portal) ingeschakeld
- [TLS v1.0 en v1.1](/azure/application-gateway/application-gateway-end-to-end-ssl-powershell) uitgeschakeld
- [Web application firewall](/azure/application-gateway/application-gateway-webapplicationfirewall-overview) (WAF modus)
- [Preventie modus](/azure/application-gateway/application-gateway-web-application-firewall-portal) met OWASP 3.0 ruleset
- [Logboekregistratie van diagnostische gegevens](/azure/application-gateway/application-gateway-diagnostics) ingeschakeld
- [Aangepaste statuscontroles](/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) en [Azure Advisor](/azure/advisor/advisor-security-recommendations), die bieden extra beveiliging en meldingen. Azure Security Center biedt ook een reputatie-systeem.

#### <a name="virtual-network"></a>Virtueel netwerk

De fundamentele architectuur definieert een persoonlijke virtueel netwerk met een adresruimte van 10.0.0.0/16.

#### <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen

Elk van de netwerk-lagen is een speciale netwerkbeveiligingsgroep (NSG):

- Een DMZ netwerkbeveiligingsgroep voor de firewall en Application Gateway WAF
- Een NSG voor beheer jumpbox (bastion host)
- Een NSG voor de app-serviceomgeving

Elk van de nsg's een bepaalde poorten en protocollen die zijn geopend voor de veilige en de juiste werking van de oplossing. Zie voor meer informatie [richtlijnen PCI - Netwerkbeveiligingsgroepen](#network-security-groups).

Bovendien zijn de volgende configuraties voor elke NSG ingeschakeld:

- Ingeschakeld [logboeken met diagnostische gegevens en gebeurtenissen](/azure/virtual-network/virtual-network-nsg-manage-log) worden opgeslagen in de storage-account 
- Verbonden OMS Log Analytics naar de [NSG van diagnostische gegevens](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

 
#### <a name="subnets"></a>Subnetten
 Zorg ervoor dat elk subnet is gekoppeld aan de bijbehorende NSG.

#### <a name="custom-domain-ssl-certificates"></a>Aangepast domein SSL-certificaten
 HTTPS-verkeer is ingeschakeld met behulp van een aangepast domein SSL-certificaat.

### <a name="data-at-rest"></a>Gegevens in rust

De architectuur beveiligt de gegevens in rust met behulp van versleuteling, Databasecontrole en andere maatregelen.

#### <a name="azure-storage"></a>Azure Storage

Om te voldoen aan de vereisten van versleutelde gegevens in rust, alle [Azure Storage](https://azure.microsoft.com/services/storage/) gebruikt [Opslagversleuteling Service](/azure/storage/storage-service-encryption).

#### <a name="azure-sql-database"></a>Azure SQL Database

Het exemplaar van Azure SQL Database maakt gebruik van de volgende veiligheidsmaatregelen voor database:

- [AD-verificatie en autorisatie](/azure/sql-database/sql-database-aad-authentication)
- [Controle van SQL-database](/azure/sql-database/sql-database-auditing-get-started)
- [Transparante gegevensversleuteling](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)
- [Firewall-regels](/azure/sql-database/sql-database-firewall-configure), zodat voor werknemersgroepen as-omgeving en beheer van IP-client
- [Detectie van dreigingen SQL](/azure/sql-database/sql-database-threat-detection-get-started)
- [Altijd versleutelde kolommen](/azure/sql-database/sql-database-always-encrypted-azure-key-vault)
- [SQL-Database dynamische-gegevensmaskering](/azure/sql-database/sql-database-dynamic-data-masking-get-started), met de PowerShell-script voor na de implementatie

### <a name="logging-and-auditing"></a>Logboekregistratie en controle

[Operations Management Suite (OMS)](/azure/operations-management-suite/) kan de Contoso Webstore voorzien van uitgebreide logboekregistratie van alle systeem- en gebruikersactiviteit, logboekregistratie van financiële gegevens bevatten. Wijzigingen worden beoordeeld en gecontroleerd op juistheid. 

- **Activiteitenlogboeken van de.**  [Activiteitenlogboeken](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement.
- **Diagnostische logboeken.**  [Diagnostische logboeken](/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) zijn alle logboeken die door elke resource. Deze logboeken bevatten Windows-gebeurtenislogboeken system, Azure Blob-opslag Logboeken, tabellen en logboeken van de wachtrij.
- **De logboeken van de firewall.**  De toepassingsgateway biedt volledige diagnostische en toegang tot logboeken. Firewall logboeken zijn beschikbaar voor Application Gateway-resources met WAF ingeschakeld een.
- **Meld u wilt archiveren.**  Alle logboeken met diagnostische gegevens zijn geconfigureerd voor het schrijven naar een gecentraliseerd en versleutelde Azure Storage-account voor archivering met een gedefinieerde bewaarperiode (2 dagen). Logboeken zijn verbonden met Azure Log Analytics voor verwerken, opslaan en dashboarding. [Meld u Analytics](https://azure.microsoft.com/services/log-analytics) is een OMS-service waarmee verzamelen en analyseren van gegevens die zijn gegenereerd voor resources in uw cloud en on-premises omgevingen.

### <a name="encryption-and-secrets-management"></a>Versleuteling en geheimen management

De Contoso Webstore versleutelt alle gevoelige gegevens en maakt gebruik van Azure Sleutelkluis sleutels beheren en het ophalen van CHD voorkomen.

- [Azure Sleutelkluis](https://azure.microsoft.com/services/key-vault/) u beveiligt cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services. 
- [SQL TDE](/sql/relational-databases/security/encryption/transparent-data-encryption) wordt gebruikt voor het versleutelen van alle financiële gegevens van de klant.
- Gegevens worden opgeslagen op de schijf met behulp van [Azure Disk Encryption](/azure/security/azure-security-disk-encryption) en BitLocker.

### <a name="identity-management"></a>Identiteitsbeheer

De volgende technologieën bieden identiteit beheermogelijkheden in de Azure-omgeving.

- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) is de Microsoft multitenant cloud-gebaseerde directory en identity management-service. Alle gebruikers voor de oplossing zijn gemaakt in Azure Active Directory, inclusief gebruikers met toegang tot de SQL-Database.
- Verificatie van de toepassing wordt uitgevoerd met behulp van Azure AD. Zie voor meer informatie [toepassingen integreren met Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications). De versleuteling van de database-kolom gebruikt Azure AD bovendien ook om te verifiëren van de toepassing met Azure SQL Database. Zie voor meer informatie [altijd versleuteld: beveiligen van gevoelige gegevens in SQL-Database](/azure/sql-database/sql-database-always-encrypted-azure-key-vault). 
- [Azure Active Directory: Identity Protection](/azure/active-directory/active-directory-identityprotection) detecteert mogelijke beveiligingsproblemen die invloed kunnen zijn op de identiteiten van uw organisatie, configureert u automatische antwoorden op gedetecteerde verdachte acties met betrekking tot de identiteiten van uw organisatie, en verdachte incidenten onderzoekt en neemt nodige actie deze op te lossen.
- [Azure op rollen gebaseerde toegangsbeheer (RBAC)](/azure/active-directory/role-based-access-control-configure) kunt nauwkeurig gerichte toegangsbeheer voor Azure. Abonnement toegang is beperkt tot de abonnementsbeheerder en Azure Key Vault toegang wordt beperkt tot alle gebruikers.

Zie voor meer informatie over het gebruik van de beveiligingsfuncties van Azure SQL Database, de [Contoso kliniek Demo-toepassing](https://github.com/Microsoft/azure-sql-security-sample) voorbeeld.
   
### <a name="web-and-compute-resources"></a>Web- en compute-bronnen

#### <a name="app-service-environment"></a>App Service-omgeving

[Azure App Service](/azure/app-service/) is een beheerde service voor het implementeren van web-apps. De toepassing Contoso Webstore wordt geïmplementeerd als een [App Service-Web-App](/azure/app-service-web/app-service-web-overview).

[Azure App Service-omgeving (as-omgeving v2)](/azure/app-service/app-service-environment/intro) is een App Service-functie een volledig geïsoleerde en toegewezen omgeving biedt voor het uitvoeren van App Service-apps veilig op grote schaal. het is een Premium-service-abonnement gebruikt door deze fundamentele architectuur PCI DSS naleving inschakelen.

ASEs zijn geïsoleerd, zodat slechts één klant toepassingen uitvoeren en altijd in een virtueel netwerk worden geïmplementeerd. Klanten hebben fijnmazig controle over beide-toepassing voor binnenkomend en uitgaand netwerkverkeer en toepassingen kunnen verbinding maken snelle beveiligde verbindingen via virtuele netwerken met lokale bedrijfsbronnen.

Gebruik van ASEs voor deze architectuur die is toegestaan voor de volgende besturingselementen/configuraties:

- Hosten in een beveiligde virtuele netwerk en netwerk-beveiligingsregels voor verbindingen
- As-omgeving geconfigureerd met een zelf-ondertekend certificaat van de ILB voor HTTPS-communicatie
- [Interne Load Balancing modus](/azure/app-service-web/app-service-environment-with-internal-load-balancer) (modus 3)
- [TLS 1.0](/azure/app-service-web/app-service-app-service-environment-custom-settings) uitgeschakeld
- [TLS-codering](/azure/app-service-web/app-service-app-service-environment-custom-settings) gewijzigd
- Besturingselement [binnenkomende verkeer N/W poorten](/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic) 
- [WAF - gegevens beperken](/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- [Verkeer van de SQL-Database](/azure/app-service-web/app-service-app-service-environment-network-architecture-overview) toegestaan


#### <a name="jumpbox-bastion-host"></a>Jumpbox (bastion host)

Omdat de App-serviceomgeving is beveiligd en vergrendeld, moet er een mechanisme om toe te staan voor DevOps releases of wijzigingen die mogelijk nodig, zoals de mogelijkheid voor het bewaken van de web-app met behulp van Kudu. Een virtuele machine is beveiligd achter NAT Load Balancer, waardoor de mogelijkheid verbinding maken met de virtuele machine op een andere poort dan TCP 3389. 

Een virtuele machine is gemaakt als een jumpbox (bastion host) met de volgende configuraties:

-   [Antimalware-uitbreiding](/azure/security/azure-security-antimalware)
-   [OMS-uitbreiding](/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure extensie voor diagnostische gegevens](/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](/azure/security/azure-security-disk-encryption) met Azure Key Vault 
-   Een [automatisch afsluiten beleid](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) gebruik kan worden verkleind van bronnen voor virtuele machines wanneer deze niet in gebruik

### <a name="security-and-malware-protection"></a>Beveiliging en malware protection

[Azure Security Center](https://azure.microsoft.com/services/security-center/) biedt een gecentraliseerde weergave van de beveiligingsstatus van alle Azure-resources. In een oogopslag kunt u controleren dat de juiste beveiligingscontroles zijn geïnstalleerd en correct geconfigureerd en u alle bronnen die aandacht vereisen snel kan identificeren.  

[Azure Advisor](/azure/advisor/advisor-overview) is een persoonlijke cloud-consultant waarmee u Volg de aanbevolen procedures voor het optimaliseren van uw Azure-implementaties. Het analyseert uw resourceconfiguratie en de telemetrie van de informatie over het gebruik en vervolgens mogelijke oplossingen die kunnen helpen bij het verbeteren van de kosteneffectiviteit, prestaties, beschikbaarheid en beveiliging van uw Azure-resources.

[Microsoft Antimalware](/azure/security/azure-security-antimalware) voor Azure-cloudservices en virtuele machines is real-timebeveiliging-functie waarmee identificeren en te verwijderen van virussen, spyware en andere schadelijke software, met configureerbare waarschuwingen wanneer bekende schadelijke of ongewenste software probeert zichzelf te installeren of uitvoeren op Azure-systemen.

### <a name="operations-management"></a>Operationeel management

#### <a name="application-insights"></a>Application Insights

Gebruik [Application Insights](https://azure.microsoft.com/services/application-insights/) om bruikbare inzicht via Toepassingsbeheer prestaties en directe analyse te krijgen.

#### <a name="log-analytics"></a>Log Analytics

[Meld u Analytics](https://azure.microsoft.com/services/log-analytics/) is een service in Operations Management Suite (OMS) waarmee u kunt verzamelen en analyseren van gegevens die zijn gegenereerd voor resources in uw cloud en on-premises omgevingen.

#### <a name="oms-solutions"></a>OMS-oplossingen

Deze extra OMS-oplossingen moeten worden gezien en geconfigureerd: 
- [Activity Log Analytics](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
- [Azure Networking Analytics](/azure/log-analytics/log-analytics-azure-networking-analytics?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Azure SQL-analyses](/azure/log-analytics/log-analytics-azure-sql)
- [Tracering wijzigen](/azure/log-analytics/log-analytics-change-tracking?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Key Vault-analyse](/azure/log-analytics/log-analytics-azure-key-vault?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Serviceoverzicht](/azure/operations-management-suite/operations-management-suite-service-map)
- [Beveiliging en controle](https://www.microsoft.com/cloud-platform/security-and-compliance)
- [Antimalware](/azure/log-analytics/log-analytics-malware?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Updatebeheer](/azure/operations-management-suite/oms-solution-update-management)

### <a name="security-center-integration"></a>Security Center-integratie

Standaardimplementatie is bedoeld om een basislijn met Security Center aanbevelingen die een configuratiestatus in orde en veilige aangeven. U kunt het verzamelen van de Azure Security Center inschakelen. Zie voor meer informatie [Azure Security Center - aan de slag](/azure/security-center/security-center-get-started).

## <a name="deploy-the-solution"></a>De oplossing implementeren

De onderdelen voor implementatie van deze oplossing zijn beschikbaar in de [betaling verwerken blauwdruk code opslagplaats][code-repo]. De implementatie van de fundamentele architectuur moet verschillende stappen uitgevoerd via Microsoft PowerShell v5. Voor verbinding met de website, moet u een aangepaste domeinnaam (zoals contoso.com) opgeven. Hiermee wordt opgegeven met de `-customHostName` in stap 2-switch. Zie voor meer informatie [aanschaffen van een aangepaste domeinnaam voor Azure-Web-Apps](/azure/app-service-web/custom-dns-web-site-buydomains-web-app). Een aangepaste domeinnaam is niet met succes implementeren en uitvoeren van de oplossing vereist, maar u zult geen verbinding maken met de website voor demonstratiedoeleinden.

De scripts toevoegen domeingebruikers aan de Azure AD-tenant die u opgeeft. Microsoft adviseert het maken van een nieuwe Azure AD-tenant als een test wilt gebruiken.

Als u problemen ondervindt tijdens de implementatie, Zie [Veelgestelde vragen en probleemoplossing](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/pci-faq.md).

Microsoft raadt ten zeerste aan dat een schone installatie van PowerShell worden gebruikt voor het implementeren van de oplossing. Controleer ook of dat u van de meest recente vereiste voor een juiste uitvoering van de installatiescripts modules gebruikmaakt. Dit voorbeeld zich aanmelden bij de jumpbox (bastion host) en de volgende opdrachten worden uitgevoerd. Houd er rekening mee dat hierdoor de opdracht aangepast domein.


1. Vereiste modules installeren en de beheerdersrollen correct ingesteld.
 
    ```powershell
     .\0-Setup-AdministrativeAccountAndPermission.ps1 
        -azureADDomainName contosowebstore.com
        -tenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
        -subscriptionId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
        -configureGlobalAdmin 
        -installModules
    ```
    Zie voor gedetailleerde informatie over het gebruiksinstructies [Script instructies - beheerdersaccount van Setup en de machtiging](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/0-Setup-AdministrativeAccountAndPermission.md).
    
2. Installeer het beheer voor het bijwerken van een oplossing. 
 
    ```powershell
    .\1-DeployAndConfigureAzureResources.ps1 
        -resourceGroupName contosowebstore
        -globalAdminUserName adminXX@contosowebstore.com 
        -globalAdminPassword **************
        -azureADDomainName contosowebstore.com 
        -subscriptionID XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 
        -suffix PCIcontosowebstore
        -customHostName contosowebstore.com
        -sqlTDAlertEmailAddress edna@contosowebstore.com 
        -enableSSL
        -enableADDomainPasswordPolicy 
    ```
    
    Zie voor instructies over het gebruik van gedetailleerde, [Script instructies - implementeren en configureren van de Azure-Resources](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/1-DeployAndConfigureAzureResources.md).
    
3. OMS logboekregistratie en controle. Wanneer de oplossing is geïmplementeerd een [Microsoft Operations Management Suite (OMS)](/azure/operations-management-suite/operations-management-suite-overview) werkruimte te openen en de voorbeeldsjablonen die is opgegeven in de opslagplaats oplossing kunnen worden gebruikt om te laten zien hoe een dashboard controle kan worden geconfigureerd . Voor de OMS-voorbeeldsjablonen, raadpleegt u de [omsDashboards map](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/1-DeployAndConfigureAzureResources.md). Houd er rekening mee dat de gegevens moeten worden verzameld in OMS voor sjablonen voor het implementeren van correct. Dit kan duren een uur of langer, afhankelijk van de activiteiten op websites.
 
    Overweeg bij het instellen van uw OMS-logboekregistratie, waaronder de volgende bronnen:
 
    - Microsoft.Network/applicationGateways
    - Microsoft.Network/NetworkSecurityGroups
    - Microsoft.Web/serverFarms
    - Microsoft.Sql/servers/databases
    - Microsoft.Compute/virtualMachines
    - Microsoft.Web/sites
    - Microsoft.KeyVault/Vaults
    - Microsoft.Automation/automationAccounts
 

    
## <a name="threat-model"></a>Risicomodel

Een gegevensstroom-diagram (GSD) en een risicomodel voorbeeld voor het Contoso Webstore [risicomodel betaling verwerken blauwdruk](https://aka.ms/pciblueprintthreatmodel).

![](images/pci-threat-model.png)



## <a name="customer-responsibility-matrix"></a>Klant verantwoordelijkheid matrix

Klanten zijn verantwoordelijk voor het bewaren van een kopie van de [verantwoordelijkheid samenvatting Matrix](https://aka.ms/fsiblueprintcrm), dit geeft een overzicht van de vereisten voor FFIEC die de verantwoordelijkheid van de klant en toepassingen die de verantwoordelijkheid van de Microsoft Azure.



## <a name="disclaimer-and-acknowledgments"></a>Afwijzing en bevestigingen

*September 2017*

- Dit document is alleen ter informatie. MICROSOFT EN AVYAN, WAARDOOR GEEN ENKELE EXPLICIETE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en inzichten die in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Dit document lezen klanten draagt het risico voor het gebruik ervan.  
- Dit document biedt geen enkel wettelijk recht op enig intellectueel eigendom van oplossingen van andere producten van Microsoft of Avyan of klanten.  
- Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.  

  > [!NOTE]
  > Bepaalde aanbevelingen in dit artikel kunnen leiden tot hogere-, netwerk- of compute-Resourcegebruik in Azure en een klant Azure licentie of abonnement kosten kunnen verhogen.  

- De oplossing in dit document is bedoeld als een fundamenteel architectuur en moet niet worden gebruikt als-is bedoeld voor productie. Tot de nakoming van vereist dat klanten hun auditor valideren van de definitieve klantoplossing raadplegen.  
- Alle klantnamen van de, transactierecords en alle bijbehorende gegevens op deze pagina zijn fictief, gemaakt met het oog op deze fundamentele architectuur en slechts ter illustratie. Er is geen echte verwantschap of relatie is bedoeld en geen mag niet worden afgeleid.  
- Deze oplossing is ontwikkeld door Microsoft en Avyan advies en is beschikbaar in de [MIT-licentie](https://opensource.org/licenses/MIT).

### <a name="document-authors"></a>Auteurs van documenten

* *Frank Simorjay (Microsoft)*  

[code-repo]: https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms "Code-opslagplaats"
