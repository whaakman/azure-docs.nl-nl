---
title: Inleiding tot Azure-beveiliging | Microsoft Docs
description: Meer informatie over Azure-beveiliging, de services en hoe het werkt.
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: 68bba95e177fa8d0261b84f51b0f5285c7fb7417
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="introduction-to-azure-security"></a>Inleiding tot Azure-beveiliging
## <a name="overview"></a>Overzicht
We weten dat beveiliging taak in de cloud en hoe belangrijk is dat u tijdig en informatie over Azure-beveiliging vinden. Een van de beste redenen voor het gebruik van Azure voor uw toepassingen en services is om te profiteren van de breed scala aan mogelijkheden en hulpprogramma's voor beveiliging. Deze hulpprogramma's en mogelijkheden te maken het mogelijk om veilige oplossingen maken op de beveiligde Azure-platform. Microsoft Azure biedt vertrouwelijkheid, integriteit en beschikbaarheid van klantgegevens, maar ook transparante accountability.

Om beter inzicht in de verzameling van beveiligingsmechanismen geïmplementeerd in Microsoft Azure van zowel naar het bieden van een uitgebreide blik op de beveiliging met Microsoft Azure van de klant en de Microsoft operations perspectieven, dit witboek 'Inleiding op Azure-beveiliging', wordt geschreven.

### <a name="azure-platform"></a>Azure-Platform
Azure is een openbare cloud service-platform die ondersteuning biedt voor een brede selectie van besturingssystemen, programmeertalen, frameworks, hulpprogramma's, databases, en apparaten. Linux-containers kan worden uitgevoerd met Docker-integratie; bouwen van apps met JavaScript, Python, .NET, PHP, Java en Node.js; Build back-ends voor iOS, Android en Windows apparaten.

Openbare Azure-cloud-services ondersteunen dezelfde technologieën miljoenen ontwikkelaars en IT-professionals die al zijn afhankelijk van en vertrouwt. Wanneer u bouwen op of IT-activa te migreren, een openbare cloud-serviceprovider u vertrouwen op de mogelijkheden voor uw toepassingen en gegevens beschermen met de services en de besturingselementen die organisatie ze bieden voor het beheren van de beveiliging van uw cloud-gebaseerde activa.

Azure infrastructuur van de faciliteit is ontworpen voor toepassingen voor het hosten van miljoenen klanten tegelijkertijd en biedt een betrouwbare basis waarop bedrijven kunnen voldoen aan de beveiligingsvereisten.

Bovendien biedt Azure u een breed scala aan configureerbare beveiligingsopties en de mogelijkheid om ze te beheren zodat u kunt de beveiliging om te voldoen aan de unieke vereisten van uw organisatie implementaties aanpassen. Dit document helpt u begrijpen hoe Azure-beveiliging mogelijkheden kunt u aan deze vereisten voldoen.

> [!Note]
> Dit document richt zich voornamelijk op klantgerichte besturingselementen die u kunt aanpassen en betere beveiliging voor uw toepassingen en services.
>
> We doen overzichtsinformatie geven, maar voor gedetailleerde informatie over hoe Microsoft Azure-platform zelf beveiligt, Zie de informatie in de [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx).

### <a name="abstract"></a>Abstracte
Openbare Clouds migraties zijn in eerste instantie wordt aangestuurd door kostenbesparingen en flexibiliteit vernieuwen. Beveiliging wordt beschouwd als een groot belang voor enige tijd en zelfs een weergeven Stop voor migratie van de openbare cloud. Openbare cloud security heeft echter is overgegaan van groot belang aan een van de stuurprogramma's voor cloudmigratie. De logica achter dit is de bovenliggende mogelijkheid van grote openbare cloudserviceproviders om toepassingen en de gegevens van cloud-gebaseerde activa.

De infrastructuur van Azure is zo ontworpen dat toepassingen miljoenen klanten tegelijkertijd kunnen hosten en er daarnaast een betrouwbare basis wordt geboden waarop bedrijven kunnen voldoen aan hun beveiligingsbehoeften. Bovendien Azure vindt u een breed scala aan configureerbare beveiligingsopties en de mogelijkheid om ze te beheren zodat u kunt de beveiliging om te voldoen aan de unieke vereisten van uw implementaties om te voldoen aan uw IT-afdeling aanpassen toegangsbeheerbeleid en voldoen aan de externe voorschriften.

Dit artikel bevat een overzicht van de aanpak van Microsoft voor beveiliging in de Microsoft Azure cloud-platform:
* Beveiligingsfuncties die zijn geïmplementeerd door Microsoft voor het beveiligen van de Azure-infrastructuur, klantgegevens en toepassingen.
* Azure-services en beveiligingsfuncties die beschikbaar zijn voor u de beveiliging van de Services en uw gegevens in uw Azure-abonnementen beheren.

## <a name="summary-azure-security-capabilities"></a>Overzicht Azure beveiligingsmogelijkheden
De volgende tabel bieden een korte beschrijving van de beveiligingsfuncties geïmplementeerd door Microsoft voor het beveiligen van de Azure-infrastructuur, klantgegevens en beveiligde toepassingen.
### <a name="security-features-implemented-to-secure-the-azure-platform"></a>Beveiligingsfuncties geïmplementeerd als u wilt beveiligen, de Azure-Platform:
De functies in de lijst te volgen zijn mogelijkheden die u bekijken kunt om te waarborgen dat de Azure-Platform op een veilige manier wordt beheerd. Koppelingen zijn opgegeven voor verdere Inzoomen op hoe Microsoft vragen van klanten vertrouwen in vier gebieden adressen: Secure Platform, Privacy en besturingselementen, naleving en transparantie.


| [Veilig Platform](https://www.microsoft.com/en-us/trustcenter/Security/default.aspx)  | [Privacy en besturingselementen](https://www.microsoft.com/en-us/trustcenter/Privacy/default.aspx)  |[Naleving](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx)   | [Transparantie](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx) |
| :-- | :-- | :-- | :-- |
| [Beveiligingsontwikkeling opstellen](https://www.microsoft.com/en-us/sdl/), interne controles | [Uw gegevens altijd beheren](https://www.microsoft.com/en-us/trustcenter/Privacy/You-own-your-data) | [Vertrouwenscentrum](https://www.microsoft.com/en-us/trustcenter/default.aspx) |[Hoe Microsoft beveiligt de gegevens van de klant in de Azure-services](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx) |
| [Verplichte beveiligingstraining, achtergrond controles](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwiwsOCpganRAhWqxVQKHUdiDsMQFghAMAE&url=https%3A%2F%2Fdownloads.cloudsecurityalliance.org%2Fstar%2Fself-assessment%2FStandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx&usg=AFQjCNEYvBky4zNeDQPN6YJGPFRZA7eeZg&sig2=2kkw1lOCP_kzLzgE9RS2Tg&bvm=bv.142059868,d.amc) |  [Op de locatie van gegevens beheren](https://www.microsoft.com/en-us/trustcenter/Privacy/Where-your-data-is-located) |  [Algemene besturingselementen Hub](https://www.microsoft.com/en-us/trustcenter/Common-Controls-Hub) |[Hoe de locatie van gegevens in Azure-services voor het beheren van Microsoft](http://azuredatacentermap.azurewebsites.net/)|
| [Testen van binnendringen](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwiwsOCpganRAhWqxVQKHUdiDsMQFghAMAE&url=https%3A%2F%2Fdownloads.cloudsecurityalliance.org%2Fstar%2Fself-assessment%2FStandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx&usg=AFQjCNEYvBky4zNeDQPN6YJGPFRZA7eeZg&sig2=2kkw1lOCP_kzLzgE9RS2Tg&bvm=bv.142059868,d.amc), [inbraakdetectie, DDoS](https://www.microsoft.com/en-us/trustcenter/Security/ThreatManagement), [Audits & logboekregistratie](https://www.microsoft.com/en-us/trustcenter/Security/AuditingAndLogging) | [Gegevenstoegang bieden op uw eigen voorwaarden](https://www.microsoft.com/en-us/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms) |  [De Cloudservices vanwege toewijding inzetten controlelijst](https://www.microsoft.com/en-us/trustcenter/Compliance/Due-Diligence-Checklist) |[Wie in Microsoft kan toegang tot uw gegevens op welke voorwaarden](https://www.microsoft.com/en-us/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms)|
| [Status van de illustraties Datacenter](https://www.microsoft.com/en-us/cloud-platform/global-datacenters), fysieke beveiliging [beveiligd netwerk](https://docs.microsoft.com/azure/security/security-network-overview) | [Reageren op Justitie en politie](https://www.microsoft.com/en-us/trustcenter/Privacy/Responding-to-govt-agency-requests-for-customer-data) |  [Naleving door service, de locatie en de branche](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx) |[Hoe Microsoft beveiligt de gegevens van de klant in de Azure-services](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx)|
|  [Security Incident response](http://aka.ms/SecurityResponsepaper), [verantwoordelijkheid gedeeld](http://aka.ms/sharedresponsibility) |[Strikte privacystandaarden](https://www.microsoft.com/en-us/TrustCenter/Privacy/We-set-and-adhere-to-stringent-standards) |  | [Certificeringsinstantie voor Azure-services, transparantie hub controleren](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx)|



### <a name="security-features-offered-by-azure-to-secure-data-and-application"></a>Beveiligingsfuncties die worden aangeboden door Azure voor het beveiligen van gegevens en toepassingen
Is afhankelijk van het cloudservicemodel variabele verantwoordelijk is voor wie verantwoordelijk is voor het beheren van de beveiliging van de toepassing of service. Er zijn mogelijkheden beschikbaar in de Azure-Platform om u te helpen voldoen aan deze verantwoordelijkheden via ingebouwde functies en via partneroplossingen die kunnen worden geïmplementeerd in een Azure-abonnement.

De ingebouwde mogelijkheden zijn georganiseerd in functiegebieden zes (6): bewerkingen, toepassingen, opslag, netwerken, berekening en identiteit. Via samenvattende informatie vindt u aanvullende details over de functies en mogelijkheden beschikbaar in de Azure-Platform op deze gebieden zes (6).

## <a name="operations"></a>Bewerkingen
Deze sectie bevat overzichtsinformatie over deze mogelijkheden en aanvullende informatie met betrekking tot belangrijke functies in beveiligingsbewerkingen.

### <a name="operations-management-suite-security-and-audit-dashboard"></a>Beveiliging van operations Management Suite en Audit-Dashboard
De [OMS beveiligings- en Audit oplossing](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) biedt een uitgebreid overzicht van uw organisatie IT beveiligingspostuur met [ingebouwde zoekquery's](https://blogs.technet.microsoft.com/msoms/2016/01/21/easy-microsoft-operations-management-suite-search-queries/) voor problemen die aandacht vereisen die uw aandacht vereisen. Het dashboard [Beveiliging en controle](https://technet.microsoft.com/library/mt484091.aspx) is het startscherm voor alles wat betrekking heeft op beveiliging in OMS. Het biedt op hoog niveau inzicht in de beveiligingsstatus van uw computers. Ook kunnen op het startscherm alle gebeurtenissen van de afgelopen 24 uur, 7 dagen of een ander tijdsbestek worden weergegeven.

Bovendien kunt u OMS-beveiliging en naleving van [automatisch bepaalde acties uitvoeren](https://blogs.technet.microsoft.com/robdavies/2016/04/20/simple-look-at-oms-alert-remediation-with-runbooks-part-1/) wanneer een bepaalde gebeurtenis wordt gedetecteerd.

### <a name="azure-resource-manager"></a>Azure Resource Manager
[Azure Resource Manager ](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) kunt u werken met de resources in uw oplossing als een groep. U kunt alle resources voor uw oplossing implementeren, bijwerken of verwijderen in een enkele, gecoördineerde bewerking. U gebruikt een [Azure Resource Manager-sjabloon](https://blogs.technet.microsoft.com/canitpro/2015/06/29/devops-basics-infrastructure-as-code-arm-templates/) voor implementatie en dat de sjabloon voor verschillende omgevingen zoals testen, Faseren en productie werken kunnen. Resource Manager biedt beveiliging, controle en tagfuncties die u na de implementatie helpen bij het beheren van uw resources.

Azure Resource Manager-sjabloon gebaseerde implementaties te verbeteren de beveiliging van oplossingen die zijn geïmplementeerd in Azure, omdat de standaardbeveiliging-instellingen beheren en kan worden geïntegreerd in implementaties van gestandaardiseerde op basis van een sjabloon. Dit vermindert het risico van security configuratiefouten die tijdens handmatige implementaties uitgevoerd worden kunnen.

### <a name="application-insights"></a>Application Insights
[Application Insights](https://docs.microsoft.com/azure/application-insights/) is een uitbreidbaar Management APM (Application Performance)-service voor webontwikkelaars. Met Application Insights kunt u uw live webtoepassingen bewaken en afwijkingen automatisch detecteren. Dit omvat analytics krachtige hulpprogramma's voor hulp bij het vaststellen van problemen en om te begrijpen wat gebruikers daadwerkelijk doen met uw apps. Het bewaakt uw toepassing de tijd die zijn uitgevoerd, zowel tijdens het testen en nadat u hebt gepubliceerd of geïmplementeerd.

Application Insights maakt grafieken en tabellen die u hebt, bijvoorbeeld weergeven, welke tijdstippen krijgt u de meeste gebruikers, hoe responsief de app is en hoe goed worden geleverd door een externe services die afhankelijk zijn van.

Als er crashes, fouten of prestatieproblemen, kunt u via de telemetriegegevens in detail om de oorzaak van de oorzaak te zoeken. En de service verzendt u e-mailberichten als er wijzigingen in de beschikbaarheid en prestaties van uw app. Toepassing inzicht wordt dus een waardevolle beveiligingsprogramma omdat hiermee met de beschikbaarheid in de vertrouwelijkheid, integriteit en beschikbaarheid beveiliging drie vakken.

### <a name="azure-monitor"></a>Azure Monitor
[Monitor voor Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) biedt visualisatie, query routering, waarschuwingen, automatisch schalen en automatisering op beide gegevens van de Azure-infrastructuur ([activiteitenlogboek](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)) en elke afzonderlijke Azure-resource ([diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)). U kunt Azure Monitor gebruiken om u te waarschuwen over beveiligingsgebeurtenissen die worden gegenereerd in Logboeken op Azure.

### <a name="log-analytics"></a>Log Analytics
[Meld u Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) deel uit van [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) : biedt een oplossing voor IT-beheer voor zowel on-premises en derden cloud-gebaseerde infrastructuur hebben (zoals AWS) naast het Azure-resources. Gegevens uit Azure Monitor kunnen rechtstreeks met logboekanalyse worden gerouteerd, zodat u Logboeken en metrische gegevens voor uw hele omgeving op één plek kunt zien.

Log Analytics is een handig hulpmiddel in forensische en andere beveiligingsanalyse als het hulpprogramma kunt u snel zoeken door grote hoeveelheden beveiliging gerelateerde vermeldingen met een flexibele queryfunctionaliteit benadering. Bovendien lokale [firewall en proxy-logboeken kunnen worden geëxporteerd naar Azure en beschikbaar gesteld voor analyse met behulp van logboekanalyse.](https://docs.microsoft.com/azure/log-analytics/log-analytics-proxy-firewall)

### <a name="azure-advisor"></a>Azure Advisor
[Azure Advisor](https://docs.microsoft.com/azure/advisor/) is consultant persoonlijke cloud die u helpt bij het optimaliseren van uw Azure-implementaties. Advisor analyseert de configuratie van uw resources en de gebruiksgerelateerde telemetrie. Deze oplossingen om u te helpen verbeteren vervolgens raadt de [prestaties](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations), [beveiliging](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations), en [hoge beschikbaarheid](https://docs.microsoft.com/azure/advisor/advisor-high-availability-recommendations) van uw resources tijdens het zoeken naar mogelijkheden om [reduceren uw algehele Azure hoeven te besteden aan](https://docs.microsoft.com/azure/advisor/advisor-cost-recommendations). Azure Advisor bevat aanbevelingen voor beveiliging, aanzienlijk kan verbeteren de algehele beveiligingsstatus voor oplossingen die u in Azure implementeren. Deze aanbevelingen zijn afkomstig van beveiligingsanalyse uitgevoerd door [Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-intro)

### <a name="azure-security-center"></a>Azure Security Center
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) helpt u bij het detecteren, voorkomen van en reageren op bedreigingen dankzij een verhoogde zichtbaarheid van en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

Bovendien Azure Security Center helpt met beveiligingsbewerkingen doordat u een dashboard dat verwerkingsinformatie waarschuwingen en aanbevelingen die kunnen worden gereageerd onmiddellijk. U kunt vaak problemen met een enkele Klik vanuit de Azure Security Center-console herstellen.
## <a name="applications"></a>Toepassingen
De sectie bevat aanvullende informatie over belangrijke functies in de toepassing beveiligings- en samenvatting informatie over deze mogelijkheden.

### <a name="web-application-vulnerability-scanning"></a>Web Application beveiligingslek scannen
Een van de eenvoudigste manieren om aan de slag met het testen op beveiligingsproblemen op uw [App Service-app](https://docs.microsoft.com/azure/app-service/app-service-web-overview) is het gebruik van de [integratie met Tinfoil Security](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) één muisklik beveiligingslek scannen van uw app uitvoeren. U kunt de resultaten weergeven in een rapport gemakkelijk te begrijpen en informatie over het oplossen van elk beveiligingsprobleem met stapsgewijze instructies.

### <a name="penetration-testing"></a>Indringingstests
Als u liever uw eigen tests binnendringen uitvoeren of een andere scanner suite of provider wilt gebruiken, moet u de [Azure binnendringen goedkeuringsproces testen](https://security-forms.azure.com/penetration-testing/terms) en voorafgaande goedkeuring om uit te voeren van de gewenste binnendringen tests verkrijgen.

### <a name="web-application-firewall"></a>Web Application firewall
De web application firewall (WAF) in [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) helpt webtoepassingen te beschermen tegen veelvoorkomende web gebaseerde aanvallen zoals SQL-injectie, cross-site scripting aanvallen en sessiehijacking. Het is vooraf geconfigureerd met beveiliging tegen bedreigingen die zijn geïdentificeerd door de [Open Web Application Security Project (OWASP) als de top 10 veelvoorkomende beveiligingslekken](https://msdn.microsoft.com/library/).

### <a name="authentication-and-authorization-in-azure-app-service"></a>Verificatie en autorisatie in Azure App Service
[App Service-verificatie / autorisatie](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) is een functie die een manier om uw toepassing aan te melden gebruikers biedt zodat u hoeft de code op de back-end voor de app wijzigen. Het biedt een eenvoudige manier voor het beveiligen van uw toepassing en werken met gegevens per gebruiker.

### <a name="layered-security-architecture"></a>Gelaagde beveiligingsarchitectuur
Aangezien [App Service-omgevingen](https://docs.microsoft.com/azure/app-service/environment/app-service-app-service-environment-intro) voorziet in een geïsoleerde runtime-omgeving geïmplementeerd in een [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview), kunnen ontwikkelaars een gelaagd beveiligingsarchitectuur bieden verschillende niveaus van toegang tot het netwerk voor elke toepassingslaag maken. Een algemene streven is verbergen API back-ends van algemene toegang tot Internet en dat alleen API's om te worden aangeroepen door de upstream-web-apps. [Netwerkbeveiligingsgroepen (nsg's)](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/) op Azure Virtual Network subnetten met App Service-omgevingen kunnen worden gebruikt voor het openbare toegang tot API toepassingen beperken.

### <a name="web-server-diagnostics-and-application-diagnostics"></a>Web server diagnostics en application diagnostics
App Service-web-apps bieden diagnostische functionaliteit voor logboekregistratie informatie van de webserver en de webtoepassing. Deze logisch zijn verdeeld in [web server diagnostische](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log) en [application diagnostics](https://technet.microsoft.com/library/hh530058(v=sc.12).aspx). Webserver bevat twee belangrijke ontwikkelingen bij het opsporen en oplossen van sites en toepassingen.

De eerste nieuwe functie is realtime staat informatie over groepen van toepassingen, werkprocessen, sites, toepassingsdomeinen en uitvoeren van aanvragen. De tweede nieuwe voordelen zijn gedetailleerde traceringsgebeurtenissen die een aanvraag tijdens het volledige proces voor aanvragen en antwoorden bijhouden.

Om de verzameling van deze traceringsgebeurtenissen, kan IIS 7 worden geconfigureerd om vast te leggen automatisch volledige traceerlogboeken in XML-indeling voor een bepaalde aanvraag op basis van de verstreken tijd of antwoord-foutcodes.

#### <a name="web-server-diagnostics"></a>Web server diagnostische gegevens
U kunt in- of uitschakelen van de volgende soorten logboeken:

-   Gedetailleerde foutregistratie - gedetailleerde informatie over de fout voor HTTP-statuscodes die wijzen op mislukte (statuscode 400 of hoger). Deze kan informatie om te bepalen waarom de server heeft geretourneerd met de foutcode bevatten.

-   Tracering van mislukte aanvragen - gedetailleerde informatie over de mislukte aanvragen, met inbegrip van een tracering van de IIS-onderdelen gebruikt voor het verwerken van de aanvraag en de tijd die in elk onderdeel. Dit is handig als u probeert te verhogen van de prestaties van de site of isoleren wat de oorzaak van een specifieke HTTP-fout wordt geretourneerd.

-   Web Server-logboekregistratie - informatie over HTTP transacties met behulp van de W3C extended logboekbestandsindeling. Dit is handig bij het bepalen van de algemene site metrische gegevens zoals het aantal aanvragen dat is verwerkt of hoeveel aanvragen van een specifiek IP-adres.

#### <a name="application-diagnostics"></a>Application diagnostics
[Application diagnostics](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log) kunt u informatie die wordt geproduceerd door een webtoepassing vastleggen. ASP.NET-toepassingen kunnen gebruikmaken van de [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) klasse om informatie te registreren in het toepassingslogboek van de diagnostische gegevens. In Application Diagnostics zijn er twee belangrijke gebeurtenistypen gebeurtenissen, die gerelateerd zijn aan toepassingsprestaties en die betrekking hebben op toepassingsfouten en fouten. De storingen en fouten kunnen worden onderverdeeld in connectiviteit, beveiliging en fout-problemen. Systeemfouten worden doorgaans veroorzaakt een probleem met de toepassingscode.

In Application Diagnostics, kunt u gebeurtenissen gegroepeerd op de volgende manieren weergeven:

-   Alle (alle gebeurtenissen weergegeven)
-   Toepassingsfouten (uitzonderingsgebeurtenissen worden weergegeven)
-   Prestaties (prestatiegebeurtenissen worden weergegeven)

## <a name="storage"></a>Storage
De sectie bevat aanvullende informatie over belangrijke functies in Azure storage beveiligings- en samenvatting informatie over deze mogelijkheden.

### <a name="role-based-access-control-rbac"></a>RBAC (op rollen gebaseerd toegangsbeheer)
U kunt uw storage-account met op rollen gebaseerde toegangsbeheer (RBAC) beveiligen. De toegang beperken op basis van de [moet weten](https://en.wikipedia.org/wiki/Need_to_know) en [minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) beveiligingsprincipes is van cruciaal belang voor organisaties die willen beveiligingsbeleid instellen voor toegang tot gegevens. Deze toegangsrechten worden verleend door de juiste RBAC-rol toewijzen aan groepen en toepassingen op een bepaalde scope. U kunt [ingebouwde RBAC-rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles), zoals opslag Account Inzender rechten toewijzen aan gebruikers. Toegang tot de opslag-codes voor een opslag-account met de [Azure Resource Manager](https://docs.microsoft.com/azure/storage/storage-security-guide) model kan worden beheerd via op rollen gebaseerde toegangsbeheer (RBAC).

### <a name="shared-access-signature"></a>Shared Access Signature
Een [SAS (Shared Access Signature; handtekening voor gedeelde toegang)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) biedt gedelegeerde toegang tot bronnen in uw opslagaccount. De SAS betekent dat u een client beperkte machtigingen voor objecten in uw storage-account voor een opgegeven periode en met een opgegeven set machtigingen kunt verlenen. U kunt deze beperkte rechten verlenen zonder te delen van de toegangssleutels van uw account.

### <a name="encryption-in-transit"></a>Codering in Transit
Versleuteling onderweg is een mechanisme om gegevens te beveiligen wanneer deze worden verzonden via netwerken. U kunt beveiligen met behulp van gegevens met Azure Storage:
-   [Versleuteling transportniveau](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-in-transit), zoals HTTPS wanneer u gegevens van of naar Azure Storage overbrengen.

-   [Versleuteling Wire](https://docs.microsoft.com/azure/storage/storage-security-guide#using-encryption-during-transit-with-azure-file-shares), zoals [versleuteling door SMB 3.0](https://docs.microsoft.com/azure/storage/storage-security-guide) voor [Azure-bestandsshares](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-files).

-   Client-side versleuteling, voor het versleutelen van de gegevens voordat deze worden overgedragen naar de opslag en de gegevens te decoderen nadat deze zijn overgebracht buiten-opslag.

### <a name="encryption-at-rest"></a>Versleuteling 'at rest'
Gegevensversleuteling in rust is voor veel organisaties een verplichte stap naar de privacy van gegevens, naleving en onafhankelijkheid van gegevens. Er zijn drie Azure storage-beveiligingsfuncties die versleuteling van gegevens die zich 'in rust' bieden:

-   [Versleuteling van de opslagruimte](https://docs.microsoft.com/azure/storage/storage-service-encryption) Hiermee kunt u aanvragen dat de storage-service gegevens automatisch coderen wanneer het schrijven naar Azure Storage.

-   [Versleuteling aan clientzijde](https://docs.microsoft.com/azure/storage/storage-client-side-encryption) biedt ook de functie van versleuteling in rust.

-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) kunt u voor het versleutelen van de OS-schijven en gegevensschijven die wordt gebruikt door een virtuele machine voor IaaS.

### <a name="storage-analytics"></a>Storage Analytics
[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) voert logboekregistratie en voorziet in metrische gegevens voor een opslagaccount. Deze gegevens kunt u gebruiken om aanvragen te traceren, gebruikstrends te analyseren en een diagnose uit te voeren voor problemen met uw opslagaccount. Storage Analytics wordt gedetailleerde informatie over geslaagde en mislukte aanvragen in een storage-service geregistreerd. Deze informatie kan worden gebruikt voor het bewaken van afzonderlijke aanvragen en voor het vaststellen van problemen met de storage-service. Aanvragen worden geregistreerd op basis van best-effort. De volgende soorten geverifieerde aanvragen worden geregistreerd:
-   Geslaagde aanvragen.

-   Mislukte aanvragen, met inbegrip van de time-outperiode, beperking, netwerk, autorisatie en andere fouten.

-   -Aanvragen via een Shared Access Signature (SAS), met inbegrip van mislukte en geslaagde aanvragen.

-   Aanvragen voor analytische gegevens.

### <a name="enabling-browser-based-clients-using-cors"></a>Browser gebaseerde Clients met behulp van CORS inschakelen
[Cross-Origin-Resource delen (CORS)](https://docs.microsoft.com/rest/api/storageservices/fileservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) is een mechanisme waarmee domeinen elkaar om toestemming te geven voor toegang tot elkaars bronnen. De gebruiker-Agent verzendt extra headers om ervoor te zorgen dat de JavaScript-code geladen vanaf een bepaald domein is toegestaan voor toegang tot resources die zich bevindt op een ander domein. Het laatste domein en antwoorden met extra kopteksten toestaan of weigeren van de oorspronkelijke domeintoegang tot de bronnen.

Azure storage-services bieden nu ondersteuning voor CORS, zodat als u de CORS-regels voor de service hebt ingesteld, een correct geverifieerde aanvraag ten opzichte van de service uit een ander domein wordt beoordeeld om te bepalen of het is toegestaan volgens de regels die u hebt opgegeven.
## <a name="networking"></a>Netwerken
De sectie bevat aanvullende informatie over belangrijke functies in Azure-netwerk beveiligings- en samenvatting informatie over deze mogelijkheden.

### <a name="network-layer-controls"></a>Laag netwerkfuncties
Netwerktoegangsbeheer wordt het beperken van de connectiviteit van en naar specifieke apparaten of subnetten en vertegenwoordigt de kern van netwerkbeveiliging. Het doel van netwerktoegangsbeheer is om ervoor te zorgen dat uw virtuele machines en services toegankelijk is voor alleen gebruikers en apparaten waarnaar ze toegankelijk zijn.

#### <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen
Een [Netwerkbeveiligingsgroep (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) is een eenvoudige filterregels voor pakketten firewall en kunt u toegangsbeheer op basis van een [5-tuple](https://www.techopedia.com/definition/28190/5-tuple). Nsg's bieden geen application layer inspectie of geverifieerde toegangsbeheer. Ze kunnen worden gebruikt om te bepalen van verkeer tussen subnetten binnen een Azure-netwerk en het verkeer tussen een Azure-netwerk en Internet.

#### <a name="route-control-and-forced-tunneling"></a>Route besturingselement en geforceerde Tunneling
De mogelijkheid om te bepalen de werking van de routering op uw virtuele netwerken van Azure is een kritieke beveiliging en toegang besturingselement mogelijkheid via het netwerk. Als u wilt ervoor zorgen dat alle verkeer van en naar uw Azure Virtual Network dat toestel virtuele beveiliging doorloopt, moet u bijvoorbeeld kunnen worden beheerd en aangepast routeringsgedrag. U kunt dit doen door zelfgedefinieerde Routes configureren in Azure.

[De gebruiker gedefinieerde Routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) kunt u om aan te passen van binnenkomende en uitgaande paden voor verkeer naar te verplaatsen en buiten afzonderlijke virtuele machines of subnetten om ervoor te zorgen, de meest beveiligde route mogelijk. [Geforceerde tunneling](https://www.petri.com/azure-forced-tunneling) is een mechanisme die u gebruiken kunt om ervoor te zorgen dat uw services niet zijn toegestaan voor het initiëren van een verbinding met apparaten op Internet.

Dit wijkt af van het accepteren van binnenkomende verbindingen worden en vervolgens reageren op deze. Front-endwebservers moeten reageren op aanvragen van Internet-hosts en dus Internet afkomstig verkeer wordt toegestaan inkomend verkeer met deze webservers en de webservers kunnen reageren.

Geforceerde tunneling wordt meestal gebruikt om af te dwingen uitgaand verkeer naar Internet te doorlopen lokale beveiliging proxy's en firewalls.

#### <a name="virtual-network-security-appliances"></a>Virtueel netwerk beveiligingsapparaten
Terwijl Netwerkbeveiligingsgroepen, door de gebruiker gedefinieerde Routes en geforceerde tunneling u een niveau van beveiliging op de netwerk- en lagen van bieden de [OSI-model](https://en.wikipedia.org/wiki/OSI_model), het kan gebeuren wanneer u beveiliging wilt inschakelen op een hoger niveau van de stack. U kunt toegang tot deze beveiligingsfuncties verbeterde netwerk met behulp van een Azure-partner toestel oplossing voor netwerkbeveiliging. U vindt de meest recente Azure partnernetwerk beveiligingsoplossingen in via de [Azure Marketplace](https://azure.microsoft.com/marketplace/) en zoeken naar 'beveiliging' en "netwerkbeveiliging."

### <a name="azure-virtual-network"></a>Azure Virtual Network

Een virtueel Azure-netwerk (VNet) is een weergave van uw eigen netwerk in de cloud. Het is een logische isolatie van de infrastructuur Azure-netwerk toegewezen aan uw abonnement. U kunt de IP-adresblokken, DNS-instellingen, beveiligingsbeleidsregels en routetabellen binnen dit netwerk volledig beheren. U kunt uw VNet onderverdelen in subnetten en Azure IaaS virtuele machines (VM's) te plaatsen en/of [Cloudservices (PaaS-rolexemplaren)](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) op Azure Virtual Networks.

Hiermee kunt u het virtuele netwerk via een van de beschikbare [verbindingsopties](https://docs.microsoft.com/azure/vpn-gateway/) in Azure verbinden met uw on-premises netwerk. In wezen kunt u uw netwerk uitbreiden naar Azure met behoud van de volledige controle over IP-adresblokken en de schaalvoordelen van Azure voor ondernemingen.

Azure-netwerken ondersteunt verschillende scenario's voor veilige externe toegang. Enkele van de volgende:

-   [Afzonderlijke werkstations verbinden met een Azure Virtual Network](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

-   [Verbinding maken met on-premises netwerk om een virtueel Azure-netwerk met een VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

-   [Verbinding maken met on-premises netwerk om een virtueel Azure-netwerk met een specifieke WAN-verbinding](https://docs.microsoft.com/azure/expressroute/expressroute-introduction)

-   [Virtuele netwerken in Azure met elkaar verbinden](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps)

### <a name="vpn-gateway"></a>VPN Gateway
Voor het verzenden van netwerkverkeer tussen uw Azure Virtual Network en uw on-premises site, moet u een VPN-gateway maken voor uw Azure Virtual Network. Een [VPN-gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) is een soort virtuele netwerkgateway die versleuteld verkeer via een openbare verbinding verzonden. U kunt ook VPN-gateways gebruiken om verkeer tussen virtuele netwerken van Azure via het netwerk van Azure-infrastructuur te verzenden.

### <a name="express-route"></a>ExpressRoute
Microsoft Azure [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) is een speciale WAN-koppeling waarmee u uw on-premises netwerken in de Microsoft cloud uitbreiden via een speciale persoonlijke verbinding die wordt gefaciliteerd door een connectiviteitsprovider.

![ExpressRoute](./media/azure-security/azure-security-fig1.png)

Met ExpressRoute kunt u verbindingen tot stand brengen met Microsoft Cloud-services, zoals Microsoft Azure, Office 365 en CRM Online. Via een connectiviteitsprovider in een co-locatiefaciliteit is connectiviteit mogelijk vanuit een any-to-any (IP VPN) netwerk, een point-to-point Ethernet-netwerk of een virtuele overlappende verbinding.

ExpressRoute-verbindingen gaan niet via het openbare Internet en dus kunnen worden beschouwd als veiliger dan een VPN-oplossingen. Daardoor zijn ExpressRoute-verbindingen betrouwbaarder en sneller en hebben ze lagere latenties en betere beveiliging dan gewone verbindingen via internet.


### <a name="application-gateway"></a>Application Gateway
Microsoft [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) biedt een [toepassing levering Controller (ADC)](https://en.wikipedia.org/wiki/Application_delivery_controller) als een service, biedt verschillende laag 7 taakverdeling mogelijkheden voor uw toepassing.

![Application Gateway](./media/azure-security/azure-security-fig2.png)

Hiermee kunt u web-farm productiviteit optimaliseren door het offloaden van CPU intensief SSL-beëindiging aan de toepassingsgateway (ook wel bekend als 'SSL-offload' of 'SSL-bridging'). Het bevat ook een andere laag 7 routering mogelijkheden, waaronder round-robin distributie van inkomend verkeer, sessie cookies gebaseerde affiniteit URL-pad gebaseerde routering en de mogelijkheid voor het hosten van meerdere websites achter een enkele Gateway van de toepassing. Azure Application Gateway is een load balancer in laag 7.

De gateway biedt opties voor failovers en het routeren van HTTP-aanvragen tussen servers (on-premises en in de cloud).

Toepassing biedt veel toepassing levering Controller (ADC)-functies, zoals HTTP laden taakverdeling, cookies gebaseerde affiniteit van sessie, [Secure Sockets Layer (SSL)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-powershell) -offload, aangepaste statuscontroles, ondersteuning voor meerdere locaties en vele andere.

### <a name="web-application-firewall"></a>Web Application Firewall
Web Application Firewall is een functie van [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) die biedt beveiliging voor webtoepassingen die gebruikmaken van toepassingsgateway voor standaardfuncties toepassing levering besturingselement (ADC). Web Application Firewall doet dit door deze te beschermen tegen het grootste deel van de algemene internetbeveiligingsproblemen uit de OWASP top 10.

![Web Application Firewall](./media/azure-security/azure-security-fig1.png)

-   Beveiliging tegen SQL-injecties

-   Beveiliging tegen veelvoorkomende aanvallen via internet, zoals opdrachtinjectie, het smokkelen van HTTP-aanvragen, het uitsplitsen van HTTP-antwoorden en aanvallen waarbij een extern bestand wordt ingesloten

-   Beveiliging tegen schendingen van het HTTP-protocol

-   Beveiliging tegen afwijkingen van het HTTP-protocol, zoals een gebruikersagent voor de host en Accept-headers die ontbreken

-   Beveiliging tegen bots, crawlers en scanners

-   Detectie van veelvoorkomende onvolkomenheden in toepassing (dat wil zeggen, Apache, IIS, enz.)


Een gecentraliseerde Web Application Firewall ter bescherming tegen aanvallen via internet maakt het beveiligingsbeheer veel eenvoudiger en biedt de toepassing meer veiligheid tegen de bedreigingen van indringers. Een WAF-oplossing kan ook sneller reageren op een beveiligingsrisico door een patch voor een bekend beveiligingsprobleem toe te passen op een centrale locatie in plaats van elke afzonderlijke webtoepassing te beveiligen. Bestaande toepassingsgateways kunnen eenvoudig worden geconverteerd naar een toepassingsgateway met Web Application Firewall.
### <a name="traffic-manager"></a>Traffic Manager
Microsoft [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) kunt u de distributie van gebruikersverkeer voor service-eindpunten in verschillende datacenters te bepalen. Ondersteund door Traffic Manager-service-eindpunten zijn virtuele Azure-machines, Web-Apps en cloudservices. U kunt Traffic Manager ook gebruiken met externe eindpunten die niet van Azure zijn. Traffic Manager maakt gebruik van de Domain Name System (DNS) om te leiden aanvragen van clients naar de meest geschikte eindpunt op basis van een [verkeersroutering methode](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) en de status van de eindpunten.

Traffic Manager biedt een reeks verkeersroutering methoden aan de behoeften van verschillende groepen van toepassingen, eindpunt health [bewaking](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring), en automatische failover. Traffic Manager is mislukt, met inbegrip van het uitvallen van een volledige Azure-regio tegen.
### <a name="azure-load-balancer"></a>Azure Load Balancer
[Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) zorgt dat uw toepassingen een hoge beschikbaarheid hebben en goede netwerkprestaties leveren. Het is een laag 4 (TCP, UDP) load balancer die binnenkomend verkeer over orde exemplaren van services gedefinieerd in een set met gelijke taakverdeling distribueert. Azure Load Balancer kan worden geconfigureerd voor:

-   Binnenkomend internetverkeer saldo aan virtuele machines worden geladen. Deze configuratie wordt ook wel [internetgerichte taakverdeling](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview).

-   Load balance verkeer tussen virtuele machines in een virtueel netwerk, tussen virtuele machines in de cloud-services of tussen virtuele machines in een virtueel netwerk met cross-premises en lokale computers. Deze configuratie wordt ook wel [interne load balancing](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview). 

- Doorsturen van externe verkeer naar een specifieke virtuele machine

### <a name="internal-dns"></a>Interne DNS
U kunt de lijst met DNS-servers die worden gebruikt in een VNet in de beheerportal of in het configuratiebestand van het netwerk beheren. Klanten kan maximaal 12 DNS-servers toevoegen voor elk VNet. Wanneer u DNS-servers opgeeft, is het belangrijk om te controleren dat u een van de klant DNS-servers in de juiste volgorde voor de omgeving van de klant lijst. Een lijst met DNS-server werkt niet round robin. Ze worden gebruikt in de volgorde waarin ze worden opgegeven. Als de eerste DNS-server in de lijst kan worden bereikt, gebruikt de client die DNS-server, ongeacht of de DNS-server goed of niet werkt. De volgorde van de DNS-server voor het virtuele netwerk van de klant wijzigen, de DNS-servers verwijderen uit de lijst en voeg ze terug in de volgorde die klant wil. DNS ondersteunt het aspect van de beschikbaarheid van de drie vakken 'CIA'-beveiliging.

### <a name="azure-dns"></a>Azure DNS
De [Domain Name System](https://technet.microsoft.com/library/bb629410.aspx), of DNS, is verantwoordelijk voor het omzetten van (of het oplossen van) de naam van een website of -service het IP-adres. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) is een hosting service voor DNS-domeinen, omzetten van namen met behulp van Microsoft Azure-infrastructuur. Door uw domeinen in Azure te hosten, kunt u uw DNS-records met dezelfde referenties, API's, hulpprogramma's en facturering beheren als voor uw andere Azure-services. DNS ondersteunt het aspect van de beschikbaarheid van de drie vakken 'CIA'-beveiliging.
### <a name="log-analytics-nsgs"></a>Log Analytics nsg 's
U kunt de volgende categorieën van diagnostische logboeken inschakelen voor het nsg's:
-   Gebeurtenis: Bevat vermeldingen voor welke NSG regels worden toegepast op VM's en rolinstanties op basis van MAC-adres. De status voor deze regels worden verzameld om de 60 seconden.

-   Regels teller: bevat vermeldingen voor het aantal keren dat elke regel van het NSG wordt toegepast om te weigeren of verkeer toestaan.

### <a name="azure-security-center"></a>Azure Security Center
Security Center helpt u bij het detecteren, voorkomen van en reageren op bedreigingen, en biedt dat u grotere zichtbaarheid van en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde Beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt detecteren bedreigingen die anders onopgemerkt, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen. Aanbevelingen Netwerkcentrum rond firewalls, Netwerkbeveiligingsgroepen en regels voor binnenkomend verkeer op configureren.

Aanbevelingen voor netwerken beschikbaar zijn als volgt:

-   [Next Generation Firewall toevoegen](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall) raadt aan dat u de volgende generatie Firewall (NGFW) uit een Microsoft-partner toevoegt te verhogen uw beveiligingsinstellingen

-   [Alleen via NGFW verkeer routeren](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall#route-traffic-through-ngfw-only) raadt die u configureert netwerk groep (NSG) regels voor binnenkomend verkeer naar uw virtuele machine via uw NGFW forceren.

-   [Inschakelen van Netwerkbeveiligingsgroepen op subnetten of virtuele machines](https://docs.microsoft.com/azure/security-center/security-center-enable-network-security-groups) raadt aan om het nsg's op subnetten of VM's in te schakelen.

-   [Beperken van toegang via Internetgericht eindpunt](https://docs.microsoft.com/azure/security-center/security-center-restrict-access-through-internet-facing-endpoints) raadt aan dat u regels voor binnenkomend verkeer voor het nsg's configureren.


## <a name="compute"></a>Compute

De sectie bevat aanvullende informatie over belangrijke functies in dit gedeelte en samenvatting informatie over deze mogelijkheden.

### <a name="antimalware--antivirus"></a>Antimalware & Antivirus
U kunt met Azure IaaS antimalware-software van beveiliging leveranciers zoals Microsoft, Symantec, Trend Micro, McAfee en Kaspersky gebruiken uw virtuele machines beschermen tegen schadelijke bestanden, adware en andere dreigingen. [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) voor Azure Cloud Services en virtuele Machines is een beveiliging-functie waarmee identificeren en virussen, spyware en andere schadelijke software verwijderen. Microsoft Antimalware biedt configureerbare waarschuwingen wanneer bekende schadelijke of ongewenste software probeert zichzelf te installeren of uitvoeren op uw Azure-systemen. Microsoft Antimalware kan ook worden geïmplementeerd met Azure Security Center

### <a name="hardware-security-module"></a>Hardware Security Module
Versleuteling en authenticatie verbetert niet beveiliging tenzij de sleutels zelf worden beveiligd. Kunt u het beheer en beveiliging van uw kritieke geheimen en sleutels vereenvoudigen doordat ze op in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Sleutelkluis biedt de mogelijkheid voor het opslaan van uw sleutels in hardware Security modules (HSM's) die is gecertificeerd voor FIPS 140-2 Level 2-standaarden. De versleutelingssleutels SQL Server voor back-up of [transparante gegevensversleuteling](https://msdn.microsoft.com/library/bb934049.aspx) kunnen alle worden opgeslagen in de Sleutelkluis met alle sleutels of geheimen van uw toepassingen. Machtigingen en toegang tot deze beveiligde items worden beheerd via [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

### <a name="virtual-machine-backup"></a>Back-up van virtuele machine
[Azure Backup](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) is een oplossing die uw toepassingsgegevens met nul investeringen en minimale beschermt bedrijfskosten. Toepassingsfouten kunnen uw gegevens beschadigd raken en menselijke fouten kunnen fouten veroorzaken in uw toepassingen die tot beveiligingsproblemen leiden kunnen. Uw virtuele machines met Windows en Linux zijn beveiligd met Azure Backup.

### <a name="azure-site-recovery"></a>Azure Site Recovery
Een belangrijk onderdeel van uw organisatie [zakelijke continuïteit en herstel in noodgevallen (BCDR)](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) strategie is na te gaan hoe u zakelijke workloads en apps up-to-date te houden bij het geplande en ongeplande uitval optreedt. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) helpt bij het indelen replicatie, failovers en herstel van workloads en apps, zodat ze beschikbaar vanaf een secundaire locatie zijn als uw primaire locatie uitvalt.

### <a name="sql-vm-tde"></a>SQL VM TDE
[Transparante gegevensversleuteling (TDE)](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-keyvault) en kolom: versleuteling op bestandsniveau (wissen) zijn functies van SQL server-versleuteling. Deze vorm van versleuteling is vereist voor klanten om te beheren en opslaan van de cryptografische sleutels die u voor versleuteling gebruikt.

De service Azure Key Vault (Azure Sleutelkluis) is ontworpen voor het verbeteren van de beveiliging en beheer van deze sleutels op een locatie met veilige en maximaal beschikbaar. De SQL Server-Connector kan SQL-Server deze sleutels van Azure Sleutelkluis wilt gebruiken.

Als u SQL Server met lokale virtuele machines uitvoert, zijn er stappen die u kunt volgen om toegang tot Azure Key Vault vanuit uw on-premises SQL Server-machine. Maar voor SQL Server in Azure VM's, kunt u tijd besparen met behulp van de functie Azure Sleutelkluis-integratie. Met een paar Azure PowerShell-cmdlets voor deze functie inschakelt, kunt u de configuratie die nodig zijn voor een SQL-VM voor toegang tot de sleutelkluis automatiseren.

### <a name="vm-disk-encryption"></a>VM-schijfversleuteling
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) is een nieuwe functie waarmee u uw Windows- en Linux IaaS-schijven voor virtuele machine versleutelen. De branche standaard BitLocker-functie van Windows en de functie DM-Crypt van Linux voor volumeversleuteling voor het besturingssysteem en de gegevensschijven toepassing. De oplossing is geïntegreerd met Azure Key Vault om te controleren en beheren van de schijfversleuteling sleutels en geheimen in uw abonnement Sleutelkluis. De oplossing zorgt er ook voor dat alle gegevens op de virtuele machine-schijven zijn versleuteld in rust in uw Azure-opslag.

### <a name="virtual-networking"></a>Virtuele netwerken
Verbinding met het netwerk, moeten virtuele machines. Azure vereist ter ondersteuning van dit vereiste, virtuele machines worden verbonden met een virtueel netwerk van Azure. Een Azure-netwerk is een logische constructie die is gebouwd op de fysieke netwerk van Azure-infrastructuur. Elke logische [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) wordt geïsoleerd van alle andere virtuele netwerken van Azure. Deze isolatie kunt zorgen dat het netwerkverkeer in uw implementaties is niet toegankelijk voor andere Microsoft Azure-klanten.

### <a name="patch-updates"></a>Patchupdates
Patch Updates vormen de basis voor het opsporen en oplossen van problemen en het beheerproces van software-update vereenvoudigen zowel het aantal software-updates die u in uw onderneming implementeren moet te verminderen en door te verhogen van de mogelijkheid om naleving te bewaken.

### <a name="security-policy-management-and-reporting"></a>Beheer van beveiligingsbeleid en rapportage
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) helpt u te voorkomen, detecteren, en reageren op bedreigingen, en biedt u meer inzicht in en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde Beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt detecteren bedreigingen die anders onopgemerkt, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

### <a name="azure-security-center"></a>Azure Security Center
Azure Security Center helpt u bij het detecteren, voorkomen van en reageren op bedreigingen dankzij een verhoogde zichtbaarheid van en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

## <a name="identify-and-access-management"></a>Identificatie en toegang beheren

Systemen, toepassingen en gegevens beveiligen begint met toegangsbeheer op basis van identiteit. De identiteit en toegang beheerfuncties die zijn ingebouwd in zakelijke Microsoft-producten en services te beveiligen van uw organisatie en persoonlijke gegevens tegen onbevoegde toegang tijdens het beschikbaar maken voor legitieme gebruikers telkens wanneer en waar nodig.

### <a name="secure-identity"></a>Beveiligde identiteit
Microsoft gebruikt meerdere procedures voor beveiliging en technologieën op producten en -services om identiteit en toegang te beheren.
-   [Multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/) moeten gebruikers meerdere methoden gebruiken om toegang te krijgen, lokaal en in de cloud. Het biedt geavanceerde verificatie met tal van opties voor eenvoudige verificatie tijdens kunnen voldoen aan gebruikers met een eenvoudig proces aanmelden.

-   [Microsoft Authenticator](https://aka.ms/authenticator) biedt een gebruiksvriendelijke multi-factor Authentication-ervaring die werkt met Microsoft Azure Active Directory en Microsoft-accounts en biedt ondersteuning voor wearables en goedkeuringen op basis van een vingerafdruk.

-   [Wachtwoord-beleidsafdwinging](https://azure.microsoft.com/documentation/articles/active-directory-passwords-policy/) verhoogt de beveiliging van traditionele wachtwoorden door wachtwoordlengte en complexiteitsvereisten vereisten, periodieke rotatie en accountvergrendeling gedwongen na mislukte verificatiepogingen vast te stellen.

-   [Verificatie op basis van tokens](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/) Hiermee schakelt u verificatie via Active Directory Federation Services (AD FS) of secure token systemen van derden.

-   [Op rollen gebaseerde toegangsbeheer (RBAC)](https://azure.microsoft.com/documentation/articles/role-based-access-built-in-roles/) kunt u het verlenen van toegang op basis van de rol van de gebruiker, zodat u gemakkelijk kunt gebruikers alleen de mate van toegang voor het uitvoeren van hun taken te geven. U kunt RBAC per bedrijfsmodel van uw organisatie en risicotolerantie aanpassen.

-   [Geïntegreerd identiteitsbeheer (hybride identiteit)](https://azure.microsoft.com/documentation/articles/active-directory-hybrid-identity-design-considerations-overview/) kunt u beheer van toegang van gebruikers voor interne datacenters en cloudservices platforms, het maken van de identiteit van een enkele gebruiker voor verificatie en autorisatie tot alle bronnen onderhouden.

### <a name="secure-apps-and-data"></a>Apps en gegevens beveiligen
[Azure Active Directory](https://azure.microsoft.com/services/active-directory/), een uitgebreide identiteits- en toegangsbeheer cloud beheeroplossing voor beveiligde toegang tot gegevens in toepassingen op de site en in de cloud helpt en vereenvoudigt u het beheer van gebruikers en groepen. Het combineert core directoryservices, geavanceerde identiteit governance, beveiliging en toegang van Toepassingsbeheer, en maakt het eenvoudig voor ontwikkelaars op beleid gebaseerde identiteitsbeheer in hun apps. Voor het verbeteren van uw Azure Active Directory, kunt u betaald mogelijkheden met de Azure Active Directory Basic, Premium P1 en Premium P2-edities kunt toevoegen.

| Gratis / algemene functies     | Basisfuncties    |Premium P1-functies |Premium P2-functies | Azure Active Directory Join – alleen gerelateerde functies van Windows 10|
| :------------- | :------------- |:------------- |:------------- |:------------- |
|   [Directory-objecten](https://docs.microsoft.com/azure/active-directory/active-directory-editions#directory-objects), [gebruiker of groep Management (toevoegen, bijwerken, verwijderen) / op basis van gebruikers inrichten, apparaatregistratie](https://docs.microsoft.com/azure/active-directory/active-directory-editions#usergroup-management-addupdatedelete-user-based-provisioning-device-registration), [eenmalige aanmelding (SSO)](https://docs.microsoft.com/azure/active-directory/active-directory-editions#single-sign-on-sso), [Self-service Wijzigen van wachtwoorden voor cloudgebruikers](https://docs.microsoft.com/azure/active-directory/active-directory-editions#self-service-password-change-for-cloud-users), [Connect (Sync engine die on-premises adreslijsten naar Azure Active Directory uitbreidt)](https://docs.microsoft.com/azure/active-directory/active-directory-editions#connect-sync-engine-that-extends-on-premises-directories-to-azure-active-directory), [beveiliging / gebruiksrapporten](https://docs.microsoft.com/azure/active-directory/active-directory-editions#securityusage-reports)       |   [Toegangsbeheer op basis van een groep / inrichting](https://docs.microsoft.com/azure/active-directory/active-directory-editions#group-based-access-managementprovisioning), [selfservice voor wachtwoordherstel voor cloudgebruikers](https://docs.microsoft.com/azure/active-directory/active-directory-editions#self-service-password-reset-for-cloud-users), [bedrijf huisstijl (aanmelden pagina's / Toegangsvenster aanpassing)](https://docs.microsoft.com/azure/active-directory/active-directory-editions#company-branding-logon-pagesaccess-panel-customization), [ Toepassingsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-editions#application-proxy), [SLA 99,9%](https://docs.microsoft.com/azure/active-directory/active-directory-editions#sla-999) |  [Self-Service groep en app Management/zelfstandige verkeer toepassing toevoegingen/dynamische groepen](https://docs.microsoft.com/azure/active-directory/active-directory-editions#self-service-group), [Self-Service wachtwoord opnieuw instellen/wijzigen/ontgrendelen met lokale terugschrijven](https://docs.microsoft.com/azure/active-directory/active-directory-editions#self-service-password-resetchangeunlock-with-on-premises-write-back), [multi-factor Verificatie (Cloud en On-premises (MFA-Server))](https://docs.microsoft.com/azure/active-directory/active-directory-editions#multi-factor-authentication-cloud-and-on-premises-mfa-server), [MIM CAL + MIM-Server](https://docs.microsoft.com/azure/active-directory/active-directory-editions#mim-cal-mim-server), [Cloud App Discovery](https://docs.microsoft.com/azure/active-directory/active-directory-editions#cloud-app-discovery), [Connect Health](https://docs.microsoft.com/azure/active-directory/active-directory-editions#connect-health), [ Rollover van de automatische wachtwoord voor groepsaccounts](https://docs.microsoft.com/azure/active-directory/active-directory-editions#automatic-password-rollover-for-group-accounts)|   [Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection), [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure)|  [Een apparaat toevoegen aan Azure AD, bureaublad SSO, Microsoft Passport voor Azure AD, beheerder Bitlocker-herstel](https://docs.microsoft.com/azure/active-directory/active-directory-editions#join-a-device-to-azure-ad-desktop-sso-microsoft-passport-for-azure-ad-administrator-bitlocker-recovery), [MDM automatische inschrijving, Self-Service Bitlocker-herstel, extra lokale beheerders op Windows 10-apparaten via Azure AD Join](https://docs.microsoft.com/azure/active-directory/active-directory-editions#mdm-auto-enrollment)|


- [Cloud App Discovery](https://docs.microsoft.com/azure/active-directory/active-directory-cloudappdiscovery-whatis) is een premium-functie van Azure Active Directory waarmee u kunt het identificeren van cloud-toepassingen die worden gebruikt door de werknemers in uw organisatie.

- [Azure Active Directory: Identity Protection](https://azure.microsoft.com/documentation/articles/active-directory-identityprotection/) is een security-service die gebruikmaakt van Azure Active Directory afwijkingsdetectie detectiemogelijkheden een geconsolideerde weergave risicogebeurtenissen en mogelijke beveiligingsproblemen die invloed kunnen zijn op de identiteiten van uw organisatie.

- [Azure Active Directory Domain Services](https://azure.microsoft.com/services/active-directory-ds/) kunt u virtuele machines in Azure toevoegen aan een domein zonder de noodzaak voor het implementeren van domeincontrollers. Gebruikers zich aanmelden bij deze VM's met hun bedrijfsreferenties van Active Directory en naadloos toegang tot bronnen.

- [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) is een maximaal beschikbare, wereldwijde identity management-service voor consumentgerichte apps die kunnen worden uitgebreid naar honderden miljoenen identiteiten en geïntegreerd in mobiele en web-platforms. Uw klanten kunnen aanmelden bij uw apps via aanpasbare die gebruikmaken van bestaande accounts van sociale media, of u kunt nieuwe zelfstandige referenties maken.

- [Azure Active Directory B2B-samenwerking](https://aka.ms/aad-b2b-collaboration) is een beveiligde integratie partneroplossing die ondersteuning biedt voor uw externe bedrijfsrelaties door in te schakelen partners toegang tot uw gegevens en zakelijke toepassingen selectief met behulp van hun zelfbeheerde identiteiten.

- [Azure Active Directory Join](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/) kunt u cloud uitbreiden naar Windows 10-apparaten voor gecentraliseerd beheer. Dit maakt het mogelijk dat gebruikers verbinding maken met de zakelijke of organisatie-cloud via Azure Active Directory en vereenvoudigt de toegang tot apps en resources.

- [Azure Active Directory-toepassingsproxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/) biedt eenmalige aanmelding en veilige externe toegang voor webtoepassingen die lokaal worden gehost.

## <a name="next-steps"></a>Volgende stappen
- [Aan de slag met Microsoft Azure-beveiliging](https://docs.microsoft.com/azure/security/azure-security-getting-started)

Azure-services en -functies die u kunt gebruiken voor het beveiligen van uw services en gegevens binnen Azure

- [Azure Security Center](https://azure.microsoft.com/services/security-center/)

Voorkom, detecteer en reageer op bedreigingen met verbeterde zichtbaarheid en controle over de beveiliging van uw Azure-resources

- [Beveiligingsstatus bewaken in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-monitoring)

De bewakingsmogelijkheden in Azure Security Center voor het bewaken van naleving van beleid.
