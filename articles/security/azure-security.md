---
title: Inleiding tot Azure-beveiliging | Microsoft Docs
description: Meer informatie over Azure-beveiliging, de services en hoe het werkt.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: 934046ac4be6c0e85ee687cefb46b61ec8affca6
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57543356"
---
# <a name="introduction-to-azure-security"></a>Inleiding tot Azure-beveiliging
## <a name="overview"></a>Overzicht
We weten dat de beveiliging is taak in de cloud en hoe belangrijk is dat u nauwkeurige en tijdig informatie over Azure-beveiliging vinden. Een van de beste redenen om Azure te gebruiken voor uw toepassingen en services is om te profiteren van de grote verscheidenheid aan hulpprogramma's voor beveiliging en mogelijkheden. Deze hulpprogramma's en mogelijkheden helpt u bij het mogelijk te maken van veilige oplossingen op de beveiligde Azure-platform. Microsoft Azure biedt vertrouwelijkheid, integriteit en beschikbaarheid van gegevens van de klant, terwijl ook transparante aansprakelijkheid.

Zodat u beter inzicht in de verzameling van beveiliging maatregelen uitvoerig binnen Microsoft Azure vanuit de klant en de Microsoft-bewerkingen perspectieven, deze whitepaper, 'Inleiding voor Azure-beveiliging', is geschreven voor een uitgebreid overzicht op de beveiliging met Microsoft Azure.

### <a name="azure-platform"></a>Azure Platform
Azure is een openbare cloud service-platform die ondersteuning biedt voor een breed scala aan besturingssystemen, programmeertalen programmeertalen, frameworks, tools, databases en apparaten. Het kunt Linux-containers uitvoeren met Docker-integratie; Bouw apps met JavaScript, Python, .NET, PHP, Java en Node.js; Bouw back-ends voor iOS, Android en Windows apparaten.

Openbare Azure-cloudservices ondersteunt dezelfde technologieën waar miljoenen ontwikkelaars en IT-professionals die al afhankelijk zijn van en vertrouwen. Wanneer u baseren of IT-activa te migreren, een openbare cloud serviceprovider u, vertrouwen al op de beveiligingmogelijkheden die aan uw toepassingen en gegevens beschermen met de services en de besturingselementen deze opgeven voor het beheren van de beveiliging van uw cloud-gebaseerde activa.

De opslaginfrastructuur van Azure van opslagruimte is zo ontworpen toepassingen miljoenen klanten tegelijkertijd kunnen hosten en biedt een betrouwbare basis wordt geboden waarop bedrijven kunnen voldoen aan de beveiligingsvereisten.

Bovendien biedt Azure u een breed scala aan configureerbare beveiligingsopties en de mogelijkheid om deze te beheren zodat u de beveiliging om te voldoen aan de unieke vereisten van implementaties van uw organisatie kunt aanpassen. Dit document helpt u begrijpen hoe Azure-beveiliging mogelijkheden kunt u aan deze vereisten voldoen.

> [!Note]
> De primaire focus van dit document is op klantgerichte besturingselementen die u kunt aanpassen en betere beveiliging is voor uw toepassingen en services kunt gebruiken.
>
> We doen bepaalde overzichtsinformatie opgeven, maar voor gedetailleerde informatie over hoe Microsoft het Azure-platform zelf beveiligt, Zie de informatie in de [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx).

### <a name="abstract"></a>Samenvatting
Openbare cloud kunt migreren zijn in eerste instantie wordt aangestuurd door kostenbesparingen en flexibiliteit om te innoveren. Beveiliging werd beschouwd als een groot belang voor enige tijd en zelfs een show Stop voor migratie van de openbare cloud. Beveiliging van de openbare cloud is echter van groot belang overgeschakeld naar een van de stuurprogramma's voor cloudmigratie. De logica achter dit is de superieure mogelijkheid van grote openbare cloudserviceproviders toepassingen te beschermen en de gegevens van cloud-gebaseerde activa.

De infrastructuur van Azure is zo ontworpen dat toepassingen miljoenen klanten tegelijkertijd kunnen hosten en er daarnaast een betrouwbare basis wordt geboden waarop bedrijven kunnen voldoen aan hun beveiligingsbehoeften. Bovendien biedt Azure u een breed scala aan configureerbare beveiligingsopties en de mogelijkheid om deze te beheren zodat u kunt beveiliging om te voldoen aan de unieke vereisten van uw implementaties om te voldoen aan uw IT-afdeling toegangsbeheerbeleid en voldoen aan externe regelgeving.

In dit artikel geeft een overzicht van Microsoft-benadering tot beveiliging in de Microsoft Azure-cloud-platform:
* Beveiligingsfuncties die is geïmplementeerd door Microsoft voor het beveiligen van de Azure-infrastructuur, gegevens van de klant en toepassingen.
* Azure-services en beveiligingsfuncties die beschikbaar zijn voor u voor het beheren van de beveiliging van de Services en uw gegevens binnen uw Azure-abonnementen.

## <a name="summary-azure-security-capabilities"></a>Overzicht Azure beveiligingsmogelijkheden
De volgende tabel bieden een korte beschrijving van de beveiligingsfuncties die worden geïmplementeerd door Microsoft voor het beveiligen van de Azure-infrastructuur, gegevens van de klant en veilige toepassingen.
### <a name="security-features-implemented-to-secure-the-azure-platform"></a>Beveiligingsfuncties die is geïmplementeerd voor het beveiligen van het Azure-Platform:
De functies in de lijst te volgen zijn mogelijkheden die u bekijken kunt om te bieden de garantie dat het Azure-Platform op een veilige manier wordt beheerd. Koppelingen zijn opgegeven voor het verder inzoomen op de manier waarop Microsoft vragen van klanten vertrouwen op vier gebieden adressen: Beveilig Platform, Privacy en besturingselementen, naleving en transparantie.


| [Secure Platform](https://www.microsoft.com/en-us/trustcenter/Security/default.aspx)  | [Privacy- en besturingselementen](https://www.microsoft.com/en-us/trustcenter/Privacy/default.aspx)  |[Naleving](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx)   | [Transparantie](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx) |
| :-- | :-- | :-- | :-- |
| [Beveiliging ontwikkelingscyclus](https://www.microsoft.com/en-us/sdl/), interne controles zijn uitgevoerd | [Uw gegevens continu beheren](https://www.microsoft.com/en-us/trustcenter/Privacy/You-own-your-data) | [Vertrouwenscentrum](https://www.microsoft.com/en-us/trustcenter/default.aspx) |[Hoe Microsoft de klantgegevens in Azure-services beveiligt](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx) |
| [Verplichte beveiligingstraining, achtergrondonderzoek](https://downloads.cloudsecurityalliance.org/star/self-assessment/StandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx) |  [Op de locatie van gegevens beheren](https://www.microsoft.com/en-us/trustcenter/Privacy/Where-your-data-is-located) |  [Algemene besturingselementen Hub](https://www.microsoft.com/en-us/trustcenter/Common-Controls-Hub) |[Hoe de locatie van gegevens in Azure-services voor het beheren van Microsoft](http://azuredatacentermap.azurewebsites.net/)|
| [Indringingstests](https://downloads.cloudsecurityalliance.org/star/self-assessment/StandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx), [inbraakdetectie, DDoS](https://www.microsoft.com/en-us/trustcenter/Security/ThreatManagement), [controles & logboekregistratie](https://www.microsoft.com/en-us/trustcenter/Security/AuditingAndLogging) | [Gegevenstoegang bieden op uw voorwaarden](https://www.microsoft.com/en-us/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms) |  [De Cloud-Services vanwege de zorgvuldigheid controlelijst](https://www.microsoft.com/en-us/trustcenter/Compliance/Due-Diligence-Checklist) |[Wie er binnen Microsoft kan toegang tot uw gegevens op welke voorwaarden](https://www.microsoft.com/en-us/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms)|
| [Datacenter voor geavanceerde](https://www.microsoft.com/en-us/cloud-platform/global-datacenters), fysieke beveiliging [netwerk beveiligen](https://docs.microsoft.com/azure/security/security-network-overview) | [Reageren op Justitie en politie](https://www.microsoft.com/en-us/trustcenter/Privacy/Responding-to-govt-agency-requests-for-customer-data) |  [Naleving op service, de locatie en de branche](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx) |[Hoe Microsoft de klantgegevens in Azure-services beveiligt](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx)|
|  [Security Incident response](https://aka.ms/SecurityResponsepaper), [gedeelde verantwoordelijkheid](https://aka.ms/sharedresponsibility) |[Strikte privacystandaard](https://www.microsoft.com/en-us/TrustCenter/Privacy/We-set-and-adhere-to-stringent-standards) |  | [Certificering voor Azure-services, transparantie hub bekijken](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx)|



### <a name="security-features-offered-by-azure-to-secure-data-and-application"></a>Beveiligingsfuncties die worden aangeboden door Azure voor het beveiligen van gegevens en toepassingen
Afhankelijk van de cloud service-model, moet u er variabele verantwoordelijk is voor wie er verantwoordelijk is voor het beheren van de beveiliging van de toepassing of service is. Er zijn mogelijkheden beschikbaar in het Azure-Platform om u te helpen voldoen aan deze verantwoordelijkheden via de ingebouwde functies, en oplossingen van partners die kunnen worden geïmplementeerd in een Azure-abonnement.

De ingebouwde mogelijkheden zijn ingedeeld in zes (6) functionele aspecten: Bewerkingen, toepassingen, opslag, netwerken, rekenkracht en identiteit. Aanvullende informatie over de functies en mogelijkheden die beschikbaar zijn in het Azure-Platform op deze gebieden zes (6) worden geleverd via samenvattende informatie.

## <a name="operations"></a>Bewerkingen
Deze sectie bevat meer informatie over belangrijke functies in beveiligingsbewerkingen en algemene informatie over deze mogelijkheden.

### <a name="security-and-audit-dashboard"></a>Beveiliging en controle-Dashboard
De [oplossing beveiliging en controle](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) biedt een uitgebreid overzicht van uw organisatie de beveiligingspostuur IT met [ingebouwde zoekquery's](https://blogs.technet.microsoft.com/msoms/2016/01/21/easy-microsoft-operations-management-suite-search-queries/) voor belangrijke problemen die uw aandacht nodig hebben. De [beveiliging en controle](https://technet.microsoft.com/library/mt484091.aspx) dashboard is het startscherm voor alles met betrekking tot beveiliging in Azure Monitor-Logboeken. Dit zorgt voor hoogwaardig inzicht in de beveiligingsstatus van uw computers. Ook kunnen op het startscherm alle gebeurtenissen van de afgelopen 24 uur, 7 dagen of een ander tijdsbestek worden weergegeven.

U kunt bovendien configureren beveiliging en naleving van [automatisch bepaalde acties uitvoeren](https://blogs.technet.microsoft.com/robdavies/2016/04/20/simple-look-at-oms-alert-remediation-with-runbooks-part-1/) wanneer een bepaalde gebeurtenis wordt gedetecteerd.

### <a name="azure-resource-manager"></a>Azure Resource Manager
[Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) kunt u werken met de resources in uw oplossing als een groep. U kunt alle resources voor uw oplossing implementeren, bijwerken of verwijderen in een enkele, gecoördineerde bewerking. U gebruikt een [Azure Resource Manager-sjabloon](https://blogs.technet.microsoft.com/canitpro/2015/06/29/devops-basics-infrastructure-as-code-arm-templates/) voor implementatie en deze sjabloon voor verschillende omgevingen, zoals testen, fasering en productie werken kunnen. Resource Manager biedt beveiliging, controle en tagfuncties die u na de implementatie helpen bij het beheren van uw resources.

Azure Resource Manager-sjabloon op basis van implementaties te verbeteren van de beveiliging van oplossingen die zijn geïmplementeerd in Azure, omdat de standaard-beveiliging-instellingen beheren en kan worden geïntegreerd in gestandaardiseerde implementaties op basis van een sjabloon. Dit vermindert het risico van de security configuratiefouten die tijdens handmatige implementaties plaatsvinden mogelijk.

### <a name="application-insights"></a>Application Insights
[Application Insights](https://docs.microsoft.com/azure/application-insights/) is een uitbreidbare service voor Application Performance Management (APM) voor webontwikkelaars. U kunt met Application Insights, uw live web-apps controleren en automatisch detecteren van prestatieafwijkingen. Het bevat krachtige analysehulpmiddelen om u te helpen u problemen identificeren en om te begrijpen wat gebruikers daadwerkelijk doen met uw apps. Deze bewaakt uw toepassing voortdurend die wordt uitgevoerd, zowel tijdens het testen en nadat u hebt gepubliceerd of geïmplementeerd.

Application Insights maakt grafieken en tabellen waarin u u, bijvoorbeeld ziet, welke tijdstippen van de dag krijgt u de meeste gebruikers, hoe snel de app is en hoe goed wordt geleverd door de externe services die afhankelijk van is.

Als er crashes, fouten of prestatieproblemen, kunt u zoeken via de telemetrische gegevens in detail om de oorzaak te achterhalen. En de service verzendt u e-mailberichten als er wijzigingen in de beschikbaarheid en prestaties van uw app. Application Insights wordt dus een waardevolle beveiligingsprogramma omdat de service helpt met de beschikbaarheid in de vertrouwelijkheid, integriteit en beschikbaarheid van security drie vakken.

### <a name="azure-monitor"></a>Azure Monitor
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) biedt visualisatie, query, routering, waarschuwingen, automatisch schalen en automatisering van gegevens, zowel van de Azure-infrastructuur ([activiteitenlogboek](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)) en elke afzonderlijke Azure-resource ([diagnostische Registreert](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)). U kunt Azure Monitor gebruiken om te waarschuwen wanneer u beveiligingsgerelateerde gebeurtenissen die worden gegenereerd in Logboeken in Azure.

### <a name="azure-monitor-logs"></a>Azure Monitor-logboeken
[Logboeken in Azure Monitor](https://azure.microsoft.com/documentation/services/log-analytics/) : een IT-beheeroplossing biedt voor zowel on-premises en van derden cloud gebaseerde infrastructuur hebben (zoals AWS) naast de Azure-resources. Gegevens van Azure Monitor kan worden gerouteerd rechtstreeks naar Azure Monitor-Logboeken, zodat u metrische gegevens en logboeken voor uw gehele omgeving op één plek kunt zien.

Logboeken in Azure Monitor is een handig hulpmiddel in forensische en andere beveiligingsanalyse, zoals het hulpprogramma kunt u snel zoeken in grote hoeveelheden beveiliging gerelateerde items met een flexibele querymogelijkheden benadering. Bovendien on-premises [firewall en proxy logboeken kunnen worden geëxporteerd naar Azure en beschikbaar gesteld voor analyse met behulp van Azure Monitor-Logboeken.](https://docs.microsoft.com/azure/log-analytics/log-analytics-proxy-firewall)

### <a name="azure-advisor"></a>Azure Advisor
[Azure Advisor](https://docs.microsoft.com/azure/advisor/) is een persoonlijke cloudconsultant die u helpt bij het optimaliseren van uw Azure-implementaties. Advisor analyseert de configuratie van uw resources en de gebruiksgerelateerde telemetrie. Vervolgens raadt het programma in oplossingen om u te helpen verbeteren de [prestaties](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations), [security](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations), en [hoge beschikbaarheid](https://docs.microsoft.com/azure/advisor/advisor-high-availability-recommendations) van uw resources tijdens het zoeken naar mogelijkheden om [verlagen van uw totale Azure besteden](https://docs.microsoft.com/azure/advisor/advisor-cost-recommendations). Azure Advisor biedt aanbevelingen voor beveiliging, waarmee u kunnen uw algehele beveiligingsstatus voor oplossingen die u in Azure implementeert aanzienlijk verbeteren. Deze aanbevelingen zijn afkomstig van beveiligingsanalyse uitgevoerd door [Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-intro)

### <a name="azure-security-center"></a>Azure Security Center
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) helpt u bij het detecteren, voorkomen van en reageren op bedreigingen dankzij een verhoogde zichtbaarheid van en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

Bovendien helpt Azure Security Center bij de beveiligingsbewerkingen doordat u één dashboard dat oppervlakken meldingen en aanbevelingen die kunnen worden gereageerd onmiddellijk. U kunt vaak problemen met één klik in de Azure Security Center-console herstellen.
## <a name="applications"></a>Applicaties
De sectie biedt meer informatie over de belangrijkste functies in de toepassing beveiligings- en samenvatting van informatie over deze mogelijkheden.

### <a name="web-application-vulnerability-scanning"></a>Web-toepassing scannen van beveiligingsproblemen
Een van de eenvoudigste manieren om aan de slag met testen op beveiligingsproblemen op uw [App Service-app](https://docs.microsoft.com/azure/app-service/overview) is het gebruik van de [integratie met Tinfoil Security](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) beveiligingslek in één klik scannen op uw app uitvoeren. U kunt de resultaten van de weergeven in een rapport eenvoudig te begrijpen, en informatie over het oplossen van elk beveiligingsprobleem met stapsgewijze instructies.

### <a name="penetration-testing"></a>Indringingstests
Als u liever uw eigen indringingstests uit te voeren of een andere scanner suite of provider wilt gebruiken, volgt u de [Azure goedkeuringsproces voor indringingstests](https://docs.microsoft.com/azure/security/azure-security-pen-testing ) en verkrijgen van voorafgaande goedkeuring om de gewenste indringingstests uit te voeren.

### <a name="web-application-firewall"></a>Web Application firewall
De web application firewall (WAF) in [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) beschermt webtoepassingen tegen veelvoorkomende webgebaseerde aanvallen, zoals SQL-injectie, aanvallen via cross-site scripting en sessiehijacking. Dit is vooraf geconfigureerd met beveiliging tegen bedreigingen die door de [Open Web Application Security Project (OWASP) als de top 10 belangrijkste kwetsbaarheden](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project).

### <a name="authentication-and-authorization-in-azure-app-service"></a>Verificatie en autorisatie in Azure App Service
[App Service-verificatie / autorisatie](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization) is een functie waarmee u een manier voor uw toepassing aan te melden bij de gebruikers, zodat u code op de back-end te wijzigen. Het biedt een eenvoudige manier voor het beveiligen van uw toepassing en werken met gegevens per gebruiker.

### <a name="layered-security-architecture"></a>Gelaagde beveiligingsarchitectuur
Aangezien [App Service-omgevingen](https://docs.microsoft.com/azure/app-service/environment/app-service-app-service-environment-intro) voorziet in een geïsoleerde runtime-omgeving geïmplementeerd in een [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview), kunnen ontwikkelaars een gelaagde beveiligingsarchitectuur bieden verschillende niveaus van maken toegang tot het netwerk voor elke toepassingslaag. Er is een algemene wens verbergen API back-ends van algemene toegang tot Internet en API's om te worden aangeroepen door de upstream-web-apps alleen toestaan. [Netwerkbeveiligingsgroepen (nsg's)](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/) openbare toegang beperken tot de API-Apps op Azure Virtual Network-subnetten met App Service-omgevingen kunnen worden gebruikt.

### <a name="web-server-diagnostics-and-application-diagnostics"></a>Web server diagnostics en application diagnostics
App Service WebApps bieden diagnosefunctionaliteit voor waardevolle informatie uit de webserver en de web-App. Deze logisch zijn gescheiden in [web server diagnostische](https://docs.microsoft.com/azure/app-service/troubleshoot-diagnostic-logs) en [toepassingsdiagnose](https://technet.microsoft.com/library/hh530058(v=sc.12).aspx). Webserver bevat twee belangrijke ontwikkelingen bij het opsporen en oplossen van problemen sites en toepassingen.

De eerste nieuwe functie is realtime statusinformatie over groepen van toepassingen, werkprocessen, sites, toepassingsdomeinen en uitvoeren van aanvragen. De tweede nieuwe voordelen zijn de gedetailleerde traceringsgebeurtenissen die een aanvraag gedurende de volledige aanvraag / antwoord-proces bijhouden.

Om in te schakelen in de verzameling van deze traceringsgebeurtenissen, kan IIS 7 worden geconfigureerd om vast te leggen automatisch volledige traceerlogboeken in XML-indeling voor een bepaalde aanvraag op basis van de verstreken tijd of foutcodes van het antwoord.

#### <a name="web-server-diagnostics"></a>Web server diagnostische gegevens
U kunt inschakelen of uitschakelen van de volgende soorten logboeken:

-   Gedetailleerde logboekregistratie van fouten - gedetailleerde foutinformatie voor HTTP-statuscodes die duiden op een fout (statuscode 400 of hoger). Dit kan bevatten informatie die kan helpen te bepalen waarom de foutcode in de server heeft geretourneerd.

-   Tracering van mislukte aanvragen - gedetailleerde informatie over mislukte aanvragen, met inbegrip van een tracering van de IIS-componenten gebruikt voor het verwerken van de aanvraag en de tijd die in elk onderdeel. Dit is handig als u probeert te verhogen van de prestaties van de site of isolatie van wat de oorzaak is van een specifieke HTTP-fout worden geretourneerd.

-   Web Server-logboekregistratie - informatie over HTTP-transacties met behulp van de uitgebreide indeling W3C. Dit is handig bij het bepalen van de algemene metrische sitegegevens, zoals het aantal aanvragen dat is verwerkt of het aantal aanvragen afkomstig zijn van een specifiek IP-adres.

#### <a name="application-diagnostics"></a>Application diagnostics
[Application diagnostics](https://docs.microsoft.com/azure/app-service/troubleshoot-diagnostic-logs) kunt u voor het vastleggen van gegevens die worden geproduceerd door een web-App. ASP.NET-toepassingen kunnen gebruikmaken van de [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) klasse om informatie te registreren in het toepassingslogboek van diagnostische gegevens. In Application Diagnostics zijn er twee belangrijke gebeurtenistypen gebeurtenissen, die zijn gerelateerd aan de prestaties van toepassingen en die betrekking hebben op toepassingsfouten en fouten. De storingen en fouten kunnen worden onderverdeeld in problemen met connectiviteit, beveiliging en fout. Systeemfouten worden doorgaans veroorzaakt een probleem met de toepassingscode.

In Application Diagnostics, kunt u gebeurtenissen gegroepeerd op de volgende manieren bekijken:

-   Alle (alle gebeurtenissen worden weergegeven)
-   Toepassingsfouten (uitzonderingsgebeurtenissen worden weergegeven)
-   Prestaties (prestatiegebeurtenissen worden weergegeven)

## <a name="storage"></a>Opslag
De sectie biedt meer informatie over de belangrijkste functies in Azure storage-beveiligings- en samenvatting van informatie over deze mogelijkheden.

### <a name="role-based-access-control-rbac"></a>RBAC (op rollen gebaseerd toegangsbeheer)
U kunt uw storage-account met Role-Based Access Control (RBAC) kunt beveiligen. Toegang beperken op basis van de [moet weten](https://en.wikipedia.org/wiki/Need_to_know) en [minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) beveiligingsprincipes is van cruciaal belang voor organisaties die willen beveiligingsbeleid voor toegang tot gegevens afdwingen. Deze toegangsrechten worden verleend door de juiste RBAC-rol toewijzen aan groepen en toepassingen met een bepaald bereik. U kunt [ingebouwde RBAC-rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles), zoals Inzender voor Opslagaccounts, machtigingen toewijzen aan gebruikers. Toegang tot de opslag-codes voor een storage-account met de [Azure Resource Manager](https://docs.microsoft.com/azure/storage/storage-security-guide) model kan worden beheerd via op rollen gebaseerd toegangsbeheer (RBAC).

### <a name="shared-access-signature"></a>Shared Access Signature
Een [SAS (Shared Access Signature; handtekening voor gedeelde toegang)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) biedt gedelegeerde toegang tot bronnen in uw opslagaccount. De SAS betekent dat u een client beperkte machtigingen voor objecten in uw storage-account voor een opgegeven periode en met een opgegeven set machtigingen kunt verlenen. U kunt deze beperkte rechten verlenen zonder dat u hoeft voor het delen van de toegangssleutels van uw account.

### <a name="encryption-in-transit"></a>Versleuteling tijdens overdracht
Versleuteling tijdens overdracht is een mechanisme om gegevens te beveiligen wanneer deze worden verzonden tussen netwerken. Met Azure Storage, kunt u beveiligen met behulp van gegevens:
-   [Versleuteling op transportniveau](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-in-transit), zoals HTTPS wanneer u gegevens naar of uit Azure Storage overbrengen.

-   [Wire-versleuteling](https://docs.microsoft.com/azure/storage/storage-security-guide#using-encryption-during-transit-with-azure-file-shares), zoals [SMB 3.0-codering](https://docs.microsoft.com/azure/storage/storage-security-guide) voor [Azure-bestandsshares](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-files).

-   Client-side-versleuteling, voor het versleutelen van de gegevens voordat deze wordt overgedragen naar de opslag en de gegevens ontsleutelen nadat deze zijn overgebracht geen opslagruimte meer.

### <a name="encryption-at-rest"></a>Versleuteling 'at rest'
Gegevensversleuteling in rust is voor veel organisaties een verplichte stap naar de privacy van gegevens, naleving en gegevenssoevereiniteit. Er zijn drie Azure storage-beveiligingsfuncties die bieden van versleuteling van gegevens die zich 'in rust':

-   [Storage-Serviceversleuteling](https://docs.microsoft.com/azure/storage/storage-service-encryption) Hiermee kunt u aanvragen dat de storage-service gegevens automatisch versleuteld bij het schrijven van deze naar Azure Storage.

-   [Clientversleuteling](https://docs.microsoft.com/azure/storage/storage-client-side-encryption) biedt ook de functie van versleuteling-at-rest.

-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) kunt u voor het versleutelen van de besturingssysteemschijven en gegevensschijven die zijn gebruikt door een IaaS-virtuele machine.

### <a name="storage-analytics"></a>Storage Analytics
[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) logboekregistratie uitvoert en metrische gegevens biedt voor een opslagaccount. Deze gegevens kunt u gebruiken om aanvragen te traceren, gebruikstrends te analyseren en een diagnose uit te voeren voor problemen met uw opslagaccount. Storage Analytics wordt gedetailleerde informatie over geslaagde en mislukte aanvragen in een storage-service geregistreerd. Deze informatie kan worden gebruikt voor het bewaken van afzonderlijke aanvragen en om problemen met een opslagservice te diagnosticeren. Aanvragen worden geregistreerd op basis van best-effort. De volgende typen geverifieerde aanvragen worden geregistreerd:
-   Voltooide aanvragen.

-   Mislukte aanvragen, met inbegrip van time-out, beperking, netwerk, autorisatie en andere fouten.

-   Aanvragen met behulp van een Shared Access Signature (SAS), met inbegrip van mislukte en geslaagde aanvragen.

-   Aanvragen voor het analytics-gegevens.

### <a name="enabling-browser-based-clients-using-cors"></a>Browser-gebaseerde Clients met behulp van CORS inschakelen
[Cross-Origin Resource Sharing (CORS)](https://docs.microsoft.com/rest/api/storageservices/fileservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) is een mechanisme waarmee domeinen elkaar om toestemming te geven voor toegang tot elkaars bronnen. De gebruikersagent verzendt extra kopteksten om ervoor te zorgen dat de JavaScript-code geladen vanaf een bepaald domein is toegestaan voor toegang tot resources die zich bevinden op een ander domein. Het laatste domein en de antwoorden met extra kopteksten toestaan of weigeren van het oorspronkelijke domeintoegang tot de resources.

Azure storage-services bieden nu ondersteuning voor CORS, zodat nadat u de CORS-regels voor de service hebt ingesteld, wordt een juiste manier geverifieerde aanvraag indient voor de service uitgevoerd vanaf een ander domein geëvalueerd om te bepalen of het is toegestaan volgens de regels die u hebt opgegeven.
## <a name="networking"></a>Netwerken
De sectie biedt meer informatie over de belangrijkste functies in Azure-netwerk beveiligings- en samenvatting van informatie over deze mogelijkheden.

### <a name="network-layer-controls"></a>Besturingselementen voor netwerk-laag
Netwerktoegangsbeheer is de handeling van de connectiviteit van en naar specifieke apparaten of subnetten te beperken en vertegenwoordigt de kern van netwerkbeveiliging. Het doel van netwerktoegangsbeheer is om ervoor te zorgen dat uw virtuele machines en services toegankelijk is voor alleen gebruikers en apparaten die u ze toegankelijk wilt zijn.

#### <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen
Een [Netwerkbeveiligingsgroep (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) is een eenvoudige stateful pakket firewall en filteren kunt u voor het beheren van toegang op basis van een [5-tuple](https://www.techopedia.com/definition/28190/5-tuple). Nsg's bieden geen toepassing laag inspectie of geverifieerde besturingselementen voor toegang. Ze kunnen worden gebruikt voor het beheren van verkeer tussen subnetten binnen een virtueel Azure-netwerk en het verkeer tussen een Azure-netwerk en Internet.

#### <a name="route-control-and-forced-tunneling"></a>Controle van de route en geforceerde tunnels
De mogelijkheid om te bepalen van de werking van routering op uw virtuele netwerken van Azure is van een kritieke beveiligings- en de mogelijkheid van de controle. Als u wilt om ervoor te zorgen dat alle verkeer van en naar uw Azure-netwerk via dat apparaat voor de virtuele beveiliging gaat, moet u bijvoorbeeld mogelijk om te beheren en aanpassen van de werking van routering. U kunt dit doen door de gebruiker gedefinieerde Routes configureren in Azure.

[Gebruiker gedefinieerde Routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) kunt u het aanpassen van binnenkomende en uitgaande paden voor verkeer om naar te verplaatsen en buiten de afzonderlijke virtuele machines en subnetten om te zorgen dat de meest beveiligde route mogelijk. [Geforceerde tunneling](https://www.petri.com/azure-forced-tunneling) is een mechanisme dat u gebruiken kunt om ervoor te zorgen dat uw services zijn niet toegestaan om een verbinding aan apparaten op Internet te zetten.

Dit wijkt af van de mogelijkheid om binnenkomende verbindingen te accepteren en vervolgens reageren op deze. Front-end-webservers moeten reageren op aanvragen van Internet-hosts en dus Internet afkomstig verkeer is toegestaan inkomend verkeer naar deze webservers en de webservers kunnen reageren.

Geforceerde tunnels wordt meestal gebruikt om af te dwingen van uitgaand verkeer naar Internet via on-premises beveiliging proxy's en firewalls.

#### <a name="virtual-network-security-appliances"></a>Beveiliging van virtuele netwerkapparaten
Hoewel Network Security Groups, door de gebruiker gedefinieerde Routes en geforceerde tunnels u een niveau van beveiliging op het netwerk en transport lagen van bieden de [OSI-model](https://en.wikipedia.org/wiki/OSI_model), kunnen er tijden wanneer u beveiliging wilt inschakelen op een hoger niveau van de stack. U kunt toegang tot de beveiligingsfuncties van deze verbeterde netwerk met behulp van een Azure-partner-hulpprogramma voor het apparaat van de netwerk-beveiligingsoplossing. U kunt de meest recente Azure-partner-netwerk beveiligingsoplossingen vinden door naar de pagina de [Azure Marketplace](https://azure.microsoft.com/marketplace/) en zoeken naar "beveiliging" en "netwerkbeveiliging."

### <a name="azure-virtual-network"></a>Azure Virtual Network

Een virtueel Azure-netwerk (VNet) is een weergave van uw eigen netwerk in de cloud. Het is een logische isolatie van de Azure-netwerk-fabric die speciaal voor uw abonnement. U kunt de IP-adresblokken, DNS-instellingen, beveiligingsbeleidsregels en routetabellen binnen dit netwerk volledig beheren. U kunt uw VNet segmenteren in subnetten en Azure IaaS virtuele machines (VM's) en/of [Cloudservices (PaaS-rolexemplaren)](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) op Azure Virtual Networks.

Hiermee kunt u het virtuele netwerk via een van de beschikbare [verbindingsopties](https://docs.microsoft.com/azure/vpn-gateway/) in Azure verbinden met uw on-premises netwerk. In wezen kunt u uw netwerk uitbreiden naar Azure met behoud van de volledige controle over IP-adresblokken en de schaalvoordelen van Azure voor ondernemingen.

Azure-netwerken ondersteunt verschillende scenario's voor veilige externe toegang. Enkele voorbeelden zijn onder andere:

-   [Afzonderlijke werkstations verbinden met een Azure Virtual Network](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

-   [On-premises netwerk verbinden met een Azure-netwerk met een VPN-verbinding](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

-   [On-premises netwerk verbinden met een Azure-netwerk met een toegewezen WAN-verbinding](https://docs.microsoft.com/azure/expressroute/expressroute-introduction)

-   [Virtuele Azure-netwerken met elkaar verbinden](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps)

### <a name="vpn-gateway"></a>VPN Gateway
Voor het verzenden van netwerkverkeer tussen uw Azure-netwerk en uw on-premises site, moet u een VPN-gateway maken voor uw Azure Virtual Network. Een [VPN-gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) is een type virtuele netwerkgateway waarmee versleuteld verkeer via een openbare verbinding verzendt. U kunt VPN-gateways ook gebruiken voor het verzenden van verkeer tussen Azure-netwerken via de Azure-netwerk-fabric.

### <a name="express-route"></a>ExpressRoute
Microsoft Azure [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) is een speciale WAN-koppeling waarmee u uw on-premises netwerken in de Microsoft cloud uitbreiden via een speciale persoonlijke verbinding die wordt gefaciliteerd door een connectiviteitsprovider.

![ExpressRoute](./media/azure-security/azure-security-fig1.png)

Met ExpressRoute kunt u verbindingen tot stand brengen met Microsoft Cloud-services, zoals Microsoft Azure, Office 365 en CRM Online. Via een connectiviteitsprovider in een co-locatiefaciliteit is connectiviteit mogelijk vanuit een any-to-any (IP VPN) netwerk, een point-to-point Ethernet-netwerk of een virtuele overlappende verbinding.

ExpressRoute-verbindingen gaan niet via het openbare Internet en dus kunnen worden beschouwd als veiliger dan het VPN-oplossingen. Daardoor zijn ExpressRoute-verbindingen betrouwbaarder en sneller en hebben ze lagere latenties en betere beveiliging dan gewone verbindingen via internet.


### <a name="application-gateway"></a>Application Gateway
Microsoft [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) biedt een [Application Delivery Controller (ADC)](https://en.wikipedia.org/wiki/Application_delivery_controller) as a service, biedt van laag 7 voor taakverdeling van mogelijkheden voor uw toepassing.

![Application Gateway](./media/azure-security/azure-security-fig2.png)

Hiermee kunt u de productiviteit van webfarms optimaliseren door het offloaden van CPU intensieve SSL-beëindiging aan de toepassingsgateway (ook wel bekend als 'SSL-offload' of 'SSL-bridging'). Het bevat ook andere Routeringsmogelijkheden voor laag 7 met inbegrip van round robin-distributie van inkomend verkeer, cookies gebaseerde sessieaffiniteit, URL-pad gebaseerde routering en de mogelijkheid voor het hosten van meerdere websites achter één Application Gateway. Azure Application Gateway is een load balancer in laag 7.

De gateway biedt opties voor failovers en het routeren van HTTP-aanvragen tussen servers (on-premises en in de cloud).

Toepassing biedt veel Application Delivery Controller (ADC)-functies, waaronder HTTP laden taakverdeling, op cookies gebaseerde sessieaffiniteit, [Secure Sockets Layer (SSL)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-powershell) offload, aangepaste statustests, ondersteuning voor meerdere locaties en vele andere.

### <a name="web-application-firewall"></a>Web Application Firewall
Web Application Firewall is een functie van [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) die beveiliging biedt voor webtoepassingen die gebruikmaken van application-gateway voor standaard Application Delivery Control (ADC)-functies. Web Application Firewall doet dit door deze te beschermen tegen het grootste deel van de algemene internetbeveiligingsproblemen uit de OWASP top 10.

![Web Application Firewall](./media/azure-security/azure-security-fig1.png)

-   Beveiliging tegen SQL-injecties

-   Beveiliging tegen veelvoorkomende aanvallen via internet, zoals opdrachtinjectie, het smokkelen van HTTP-aanvragen, het uitsplitsen van HTTP-antwoorden en aanvallen waarbij een extern bestand wordt ingesloten

-   Beveiliging tegen schendingen van het HTTP-protocol

-   Beveiliging tegen afwijkingen van het HTTP-protocol, zoals een gebruikersagent voor de host en Accept-headers die ontbreken

-   Beveiliging tegen bots, crawlers en scanners

-   Detectie van veelvoorkomende onjuiste configuraties van toepassingen (dat wil zeggen, Apache, IIS, enz.)


Een gecentraliseerde Web Application Firewall ter bescherming tegen aanvallen via internet maakt het beveiligingsbeheer veel eenvoudiger en biedt de toepassing meer veiligheid tegen de bedreigingen van indringers. Een WAF-oplossing kan ook sneller reageren op een beveiligingsrisico door een patch voor een bekend beveiligingsprobleem toe te passen op een centrale locatie in plaats van elke afzonderlijke webtoepassing te beveiligen. Bestaande toepassingsgateways kunnen eenvoudig worden geconverteerd naar een toepassingsgateway met Web Application Firewall.
### <a name="traffic-manager"></a>Traffic Manager
Microsoft [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) kunt u de distributie van gebruikersverkeer voor service-eindpunten in verschillende datacenters te bepalen. Ondersteund door Traffic Manager-service-eindpunten zijn Azure-VM's, Web-Apps en cloudservices. U kunt Traffic Manager ook gebruiken met externe eindpunten die niet van Azure zijn. Traffic Manager maakt gebruik van de Domain Name System (DNS) om te leiden aanvragen van clients op het meest geschikte eindpunt op basis van een [verkeersrouteringsmethode](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) en de status van de eindpunten.

Traffic Manager biedt een aantal methoden routering van verkeer op basis van behoeften van de andere toepassing, de gezondheid van eindpunt [bewaking](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring), en automatische failover. Traffic Manager is bestand tegen storingen, waaronder het uitvallen van een hele Azure-regio.
### <a name="azure-load-balancer"></a>Azure Load Balancer
[Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) zorgt dat uw toepassingen een hoge beschikbaarheid hebben en goede netwerkprestaties leveren. Het is een Layer 4 (TCP, UDP) load balancer die inkomend verkeer over gezonde exemplaren van services die zijn gedefinieerd in een load balancer-set distribueert. Azure Load Balancer kan worden geconfigureerd om:

-   Laden saldo inkomend internetverkeer op virtuele machines. Deze configuratie wordt ook wel [internetgerichte load balancing](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview).

-   Taakverdeling voor verkeer tussen virtuele machines in een virtueel netwerk, tussen virtuele machines in cloudservices of tussen on-premises computers en virtuele machines in een virtueel netwerk met cross-premises. Deze configuratie wordt ook wel [intern gelijke taakverdeling](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview). 

- Doorsturen van extern verkeer naar een specifieke virtuele machine

### <a name="internal-dns"></a>Interne DNS
U kunt de lijst met DNS-servers die worden gebruikt in een VNet in de Management Portal of in het netwerkconfiguratiebestand beheren. Klanten kan maximaal 12 DNS-servers toevoegen voor elke VNet. Wanneer u opgeeft DNS-servers, is het belangrijk om te controleren weer te geven die van de klant-DNS-servers in de juiste volgorde voor de omgeving van de klant. Een lijst met DNS-server werkt niet round robin. Ze worden gebruikt in de volgorde waarin ze worden opgegeven. Als de eerste DNS-server in de lijst kan worden bereikt, wordt door de client die DNS-server, ongeacht of de DNS-server goed of niet functioneert gebruikt. De volgorde van de DNS-server voor het virtuele netwerk van de klant wijzigen, verwijdert u de DNS-servers in de lijst en toevoegen ze terug in de volgorde die klant wil. DNS biedt ondersteuning voor het aspect van de beschikbaarheid van de drie vakken 'CIA'-beveiliging.

### <a name="azure-dns"></a>Azure DNS
De [Domain Name System](https://technet.microsoft.com/library/bb629410.aspx), of DNS is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een website of -service naar het IP-adres. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) is een hostingservice voor DNS-domeinen die naamomzetting met behulp van Microsoft Azure-infrastructuur biedt. Door uw domeinen in Azure te hosten, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering beheren als voor uw andere Azure-services. DNS biedt ondersteuning voor het aspect van de beschikbaarheid van de drie vakken 'CIA'-beveiliging.
### <a name="azure-monitor-logs-nsgs"></a>Azure Monitor-logboeken nsg 's
U kunt de volgende categorieën van diagnostische logboeken inschakelen voor nsg's:
-   Gebeurtenis: Bevat vermeldingen voor welke NSG regels worden toegepast op virtuele machines en rolinstanties op basis van MAC-adres. De status van deze regels worden elke 60 seconden worden verzameld.

-   Regels voor item: Bevat vermeldingen voor het aantal keren dat elke NSG-regel is toegepast om te weigeren of verkeer toestaan.

### <a name="azure-security-center"></a>Azure Security Center
Security Center helpt u bij het voorkomen, detecteren en direct reageren op bedreigingen, en biedt dat u meer inzicht in en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen die anders onopgemerkt, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen te detecteren. Aanbevelingen Netwerkcentrum rond firewalls, Netwerkbeveiligingsgroepen en regels voor binnenkomend verkeer op configureren.

Aanbevelingen voor netwerken beschikbaar zijn als volgt:

-   [Next Generation Firewall toevoegen](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall) wordt aanbevolen dat u een volgende generatie Firewall (NGFW) via een Microsoft-partner om te vergroten van uw beveiligingen toevoegen

-   [Verkeer alleen via NGFW sturen](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall#route-traffic-through-ngfw-only) aanbevolen die u configureert het netwerk group (NSG) beveiligingsregels die binnenkomend verkeer op uw virtuele machine via de NGFW afgedwongen.

-   [Netwerkbeveiligingsgroepen inschakelen op subnetten of virtuele machines](https://docs.microsoft.com/azure/security-center/security-center-enable-network-security-groups) wordt aanbevolen dat u nsg's op subnetten of virtuele machines inschakelen.

-   [Toegang via een Internetgericht eindpunt beperken](https://docs.microsoft.com/azure/security-center/security-center-restrict-access-through-internet-facing-endpoints) wordt aanbevolen dat u regels voor binnenkomend verkeer voor nsg's configureren.


## <a name="compute"></a>Compute

De sectie biedt meer informatie over de belangrijkste functies in dit gebied en samenvatting van informatie over deze mogelijkheden.

### <a name="antimalware--antivirus"></a>Anti-malware & Antivirus
Met Azure IaaS, kunt u antimalware-software van leveranciers van beveiligingsoplossingen, zoals Microsoft, Symantec, Trend Micro, McAfee en Kaspersky aan uw virtuele machines beschermen tegen schadelijke bestanden, adware en andere dreigingen. [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor Azure Cloud Services en virtuele Machines wordt de mogelijkheid protection helpt te identificeren en verwijderen van virussen, spyware en andere schadelijke software. Microsoft Antimalware biedt configureerbare meldingen wanneer bekende schadelijke of ongewenste software probeert zichzelf te installeren of uitvoeren op uw Azure-systemen. Microsoft Antimalware kunnen ook worden geïmplementeerd met behulp van Azure Security Center

### <a name="hardware-security-module"></a>Hardware Security Module
Codering en -verificatie kunnen niet de beveiliging verbeteren, tenzij de sleutels zelf worden beveiligd. U kunt het beheer en beveiliging van uw essentiële geheimen en sleutels vereenvoudigen door op te slaan in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Key Vault biedt de mogelijkheid voor het opslaan van uw sleutels in hardware Security modules (HSM's) gecertificeerd voor FIPS 140-2 Level 2 standaarden. Uw SQL Server-versleutelingssleutels voor back-up of [transparante gegevensversleuteling](https://msdn.microsoft.com/library/bb934049.aspx) kunnen allemaal worden opgeslagen in Key Vault met alle sleutels of geheimen voor uw toepassingen. Machtigingen en toegang tot deze beveiligde items worden beheerd via [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

### <a name="virtual-machine-backup"></a>Back-up van virtuele machine
[Azure Backup](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) is een oplossing die uw toepassingsgegevens met zonder enige kapitaalinvestering en minimale beschermt bedrijfskosten. Toepassingsfouten kunnen uw gegevens beschadigen, en menselijke fouten kunnen fouten introduceren in uw toepassingen die kunnen leiden tot beveiligingsproblemen met zich mee. Met Azure Backup, worden uw virtuele machines met Windows en Linux beveiligd.

### <a name="azure-site-recovery"></a>Azure Site Recovery
Een belangrijk onderdeel van uw organisatie [zakelijke continuïteit en herstel na noodgevallen (BCDR)](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) strategie is essentieel dat u weet hoe u zakelijke workloads en apps in bedrijf te houden tijdens geplande en ongeplande uitval optreedt. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) kunt indelen replicatie, failover en herstel van workloads en apps, zodat ze beschikbaar vanaf een secundaire locatie zijn als uw primaire locatie uitvalt.

### <a name="sql-vm-tde"></a>SQL VM TDE
[Transparante gegevensversleuteling (TDE)](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-keyvault) en versleuteling op kolom (CLE) zijn functies van SQL server-versleuteling. Deze vorm van versleuteling is vereist voor klanten die u wilt beheren en opslaan van de cryptografische sleutels die u voor versleuteling gebruikt.

De service Azure Key Vault (AKV) is ontworpen voor het verbeteren van de beveiliging en beheer van deze sleutels op een beveiligde en maximaal beschikbare locatie. De SQL Server-Connector kunt SQL Server om deze sleutels uit Azure Key Vault te gebruiken.

Als u SQL Server on-premises virtuele machines worden uitgevoerd, zijn er stappen die u kunt volgen om toegang tot Azure Key Vault vanuit uw on-premises SQL Server-computer. Maar voor SQL Server in virtuele Azure-machines, kunt u tijd besparen door met de functie Azure Key Vault-integratie. Met een paar Azure PowerShell-cmdlets voor deze functie inschakelt, kunt u de configuratie die nodig zijn voor een SQL-VM voor toegang tot uw key vault automatiseren.

### <a name="vm-disk-encryption"></a>VM Disk Encryption
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) is een nieuwe functie waarmee u uw Windows- en Linux IaaS-virtuele-machineschijven versleutelen. Het is van toepassing voor de branche standaard BitLocker-functie van Windows en de DM-Crypt-functie van Linux om volumeversleuteling voor het besturingssysteem en de gegevensschijven te bieden. De oplossing is geïntegreerd met Azure Key Vault om te controleren en beheren van de schijf-versleutelingssleutels en geheimen in uw Key Vault-abonnement. De oplossing zorgt er ook voor dat alle gegevens op de virtuele-machineschijven zijn versleuteld in rust in uw Azure-opslag.

### <a name="virtual-networking"></a>Virtueel netwerk
Virtuele machines moet verbinding met het netwerk. Voor de ondersteuning die vereiste vereist Azure virtuele machines worden verbonden met een Azure-netwerk. Een Azure-netwerk is een logische constructie die is gebaseerd op de fysieke netwerk van Azure-infrastructuur. Elke logische [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) is geïsoleerd van alle andere virtuele netwerken van Azure. Deze isolatie helpt ervoor gezorgd dat het netwerkverkeer in uw implementaties niet toegankelijk voor andere Microsoft Azure-klanten is.

### <a name="patch-updates"></a>Patchupdates
Patch Updates vormen de basis voor het opsporen en oplossen van problemen en het beheerproces van software-update vereenvoudigen door het aantal software-updates die u in uw onderneming implementeren moet te verminderen, en ook door de mogelijkheid om naleving te controleren.

### <a name="security-policy-management-and-reporting"></a>Beheer van beveiligingsbeleid en rapportage
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) kunt u voorkomen, detecteren, en reageren op bedreigingen en biedt u meer inzicht in en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen die anders onopgemerkt, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen te detecteren.

### <a name="azure-security-center"></a>Azure Security Center
Azure Security Center helpt u bij het detecteren, voorkomen van en reageren op bedreigingen dankzij een verhoogde zichtbaarheid van en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

Begint met de systemen, toepassingen en gegevens beveiligen met toegangsbeheer op basis van identiteit. De identiteits- en beheerfuncties die zijn ingebouwd in zakelijke Microsoft-producten en services help uw organisatie en persoonlijke gegevens beschermen tegen onbevoegde toegang bij het maken van deze beschikbaar wordt voor legitieme gebruikers overal en ze nodig.

### <a name="secure-identity"></a>Veilige identiteit
Microsoft maakt gebruik van meerdere procedures voor beveiliging en -technologieën in haar producten en services voor het beheren van identiteits- en toegangsbeheer.
-   [Multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/) moeten gebruikers verschillende methoden gebruiken voor toegang tot on-premises en in de cloud. Het biedt sterke verificatie met een scala aan gebruiksvriendelijke verificatieopties, terwijl voor gebruikers met een eenvoudige aanmeldprocedure.

-   [Microsoft Authenticator](https://aka.ms/authenticator) biedt een gebruiksvriendelijke ervaring voor de multi-factor Authentication die compatibel is met Microsoft Azure Active Directory en Microsoft-accounts en biedt ondersteuning voor wearables en -goedkeuringen op basis van een vingerafdruk.

-   [Wachtwoord-beleidsafdwinging](https://azure.microsoft.com/documentation/articles/active-directory-passwords-policy/) vergroot de veiligheid van traditionele wachtwoorden door wachtwoordlengte en complexiteitsvereisten vereisten, periodieke rotatie en accountvergrendeling geforceerde na mislukte verificatiepogingen vast te stellen.

-   [Verificatie op basis van tokens](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/) Hiermee kunt u verificatie via Azure Active Directory.

-   [Op rollen gebaseerd toegangsbeheer (RBAC)](https://azure.microsoft.com/documentation/articles/role-based-access-built-in-roles/) kunt u toegangsmachtigingen op basis van de rol van de gebruiker, zodat u eenvoudig kunt, zodat gebruikers alleen de mate van toegang die ze nodig hebben om uit te voeren van hun taken. U kunt RBAC per bedrijfsmodel van uw organisatie en het omgaan met risico's aanpassen.

-   [Geïntegreerd identiteitsbeheer (hybride identiteit)](https://azure.microsoft.com/documentation/articles/active-directory-hybrid-identity-design-considerations-overview/) kunt u beheer van toegang van gebruikers voor interne-datacenters en cloud platformen, het maken van een enkele gebruikers-id voor verificatie en autorisatie om alle resources te onderhouden.

### <a name="secure-apps-and-data"></a>Apps en gegevens beveiligen
[Azure Active Directory](https://azure.microsoft.com/services/active-directory/), een uitgebreide identiteits- en toegangsbeheer cloudoplossing, wordt de beveiligde toegang tot gegevens in toepassingen op de site en in de cloud en vereenvoudigt het beheer van gebruikers en groepen. Het kernadreslijstservices, Geavanceerd identiteitsbestuur, beveiliging en toegangsbeheer voor toepassingen, en kunt u eenvoudig voor ontwikkelaars voor het bouwen van identiteitsbeheer in hun apps op basis van beleid. Om uw Azure Active Directory te verbeteren, kunt u betaalde mogelijkheden toevoegen met de Azure Active Directory Basic-, Premium P1- en Premium P2-edities.

| Gratis / algemene functies     | Basic-functies    |Premium P1-functies |Premium P2-functies | Azure Active Directory Join: Windows 10 alleen gerelateerde functies|
| :------------- | :------------- |:------------- |:------------- |:------------- |
|   [Directory-objecten](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [gebruiker/groepsbeheer (toevoegen/bijwerken/verwijderen) / gebruikersgebaseerde inrichting, apparaatregistratie](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [eenmalige aanmelding (SSO)](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [Self-Service wachtwoord Wijziging voor cloudgebruikers](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [Connect (synchronisatie-engine waarmee on-premises adreslijsten strekt zich uit tot Azure Active Directory)](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [beveiliging / gebruiksrapporten](https://docs.microsoft.com/azure/active-directory/active-directory-editions)       |     [Groepsgebaseerd toegangsbeheer / inrichting](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [selfservice voor wachtwoordherstel voor cloudgebruikers](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [(aanmelden pagina's / Toegangspaneel aanpassen) huisstijl van uw bedrijf](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [ Toepassingsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [SLA 99,9%](https://docs.microsoft.com/azure/active-directory/active-directory-editions) |  [Selfservice groeps- en app Management/selfservice toepassing toevoegingen/dynamische groepen](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [wachtwoord opnieuw instellen/wijzigen/ontgrendelen van de Self-Service met on-premises write-back](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [basis van meerdere factoren Verificatie (Cloud en On-premises (MFA-Server))](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [MIM CAL + MIM-Server](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [Cloud App Discovery](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [Connect Health](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [Automatische wachtwoordrollover voor groepsaccounts](https://docs.microsoft.com/azure/active-directory/active-directory-editions)|  [Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection), [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure)|  [Een apparaat toevoegen aan Azure AD, Desktop-SSO, Microsoft Passport voor Azure AD, beheerder van BitLocker-herstel](https://docs.microsoft.com/azure/active-directory/active-directory-editions), [MDM automatische inschrijving, selfservice BitLocker-herstel, extra lokale beheerders voor Windows 10-apparaten via Azure AD Join](https://docs.microsoft.com/azure/active-directory/active-directory-editions)|


- [Cloud App Discovery](https://docs.microsoft.com/azure/active-directory/active-directory-cloudappdiscovery-whatis) is een premium-functie van Azure Active Directory die u kunt cloudtoepassingen die worden gebruikt door de werknemers in uw organisatie identificeren.

- [Azure Active Directory Identity Protection](https://azure.microsoft.com/documentation/articles/active-directory-identityprotection/) is een beveiligingsservice die gebruikmaakt van Azure Active Directory-mogelijkheden voor het detecteren van afwijkingen om te voorzien in een geconsolideerde weergave risicogebeurtenissen en mogelijke beveiligingsproblemen die invloed kunnen zijn op uw identiteiten van de organisatie.

- [Azure Active Directory Domain Services](https://azure.microsoft.com/services/active-directory-ds/) kunt u Azure-VM's toevoegen aan een domein zonder domeincontrollers te implementeren. Gebruikers zich aanmelden bij deze virtuele machines met behulp van hun bedrijfsreferenties van Active Directory en naadloos toegang tot bronnen.

- [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) is een maximaal beschikbare, wereldwijde identiteitsbeheerservice voor consumentgerichte toepassingen die opschalen naar honderden miljoenen identiteiten te integreren in mobiele en web-platforms. Uw klanten kunnen zich aanmelden bij uw apps aanpasbare manier die gebruikmaken van bestaande accounts voor sociale media, of u kunt nieuwe zelfstandige referenties maken.

- [Azure Active Directory B2B-samenwerking](https://aka.ms/aad-b2b-collaboration) is een veilige integratie partneroplossing die ondersteuning biedt voor uw externe bedrijfsrelaties door in te schakelen van partners voor toegang tot uw zakelijke toepassingen en gegevens selectief met behulp van hun zelf worden beheerd identiteiten.

- [Azure Active Directory Join](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/) kunt u cloudmogelijkheden voor Windows 10-apparaten voor gecentraliseerd beheer uit te breiden. Dit maakt het mogelijk dat gebruikers verbinding maken met de cloud bedrijf of organisatie via Azure Active Directory en vereenvoudigt de toegang tot apps en resources.

- [Azure Active Directory Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/) biedt eenmalige aanmelding en veilige externe toegang voor webtoepassingen die on-premises gehost.

## <a name="next-steps"></a>Volgende stappen
- [Aan de slag met Microsoft Azure-beveiliging](https://docs.microsoft.com/azure/security/azure-security-getting-started)

Azure-services en -functies die u kunt gebruiken voor het beveiligen van uw services en gegevens binnen Azure

- [Azure Security Center](https://azure.microsoft.com/services/security-center/)

Voorkom, detecteer en reageer op bedreigingen met verbeterde zichtbaarheid en controle over de beveiliging van uw Azure-resources

- [Beveiligingsstatus bewaken in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-monitoring)

De bewakingsmogelijkheden in Azure Security Center voor het bewaken van naleving van beleid.
