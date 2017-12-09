---
title: Overzicht van Azure operationele beveiliging | Microsoft Docs
description: Dit artikel bevat een overzicht van de Azure-operationele beveiliging.
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: dba643f5c8e926bee1c5d13e71f785e5cc72a2dc
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-operational-security-overview"></a>Overzicht van Azure operationele beveiliging
Azure bedrijfsbeveiliging verwijst naar de services, besturingselementen en functies die beschikbaar zijn voor gebruikers voor het beveiligen van hun gegevens, toepassingen en andere elementen in Microsoft Azure. [Azure bedrijfsbeveiliging](https://docs.microsoft.com/azure/security/azure-operational-security) is een framework waarin de kennis die is opgedaan met tal van mogelijkheden die uniek voor Microsoft zijn, met inbegrip van de Microsoft Security Development Lifecycle (SDL), de Microsoft Security Response Programma Center en grondige kennis van de cyberbeveiliging beveiliging threat Liggend.

In dit artikel operationele beveiligingsoverzicht van Azure is gericht op de volgende gebieden:

- Azure Operations Management Suite
-   Azure Security Center
-   Azure Monitor
-   Azure-netwerk-watcher
-   Azure Storage analytics
-   Azure Active directory

## <a name="azure-operations-management-suite"></a>Azure Operations Management Suite
IT Operations is verantwoordelijk voor het beheren van datacenter-infrastructuur, toepassingen en gegevens, inclusief de stabiliteit en beveiliging van deze systemen. Security insights krijgen over de vaak complexe IT-omgevingen verhogen vereist echter organisaties cobble samen gegevens uit meerdere systemen voor beveiliging en beheer.

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) van Microsoft cloud-gebaseerde IT beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud-infrastructuur.

OMS is een beheeroplossing cloud-gebaseerde IT met veel aanbiedingen, zoals de automatisering van IT, beveiliging en naleving, Log Analytics en back-up en herstel. Als zodanig is een perfecte hulp te beheren en beveiligen van uw IT-infrastructuur, on-premises en in de cloud.

De kernfunctionaliteit van OMS wordt geleverd door een reeks services die in Azure worden uitgevoerd. Elke service biedt een specifieke beheerfunctie. U kunt services combineren om verschillende beheerscenario's te bewerkstelligen. Dit omvat:

-   Log Analytics
-   Automatisering
-   Back-up maken
-   Site Recovery

### <a name="log-analytics"></a>Log Analytics
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) biedt bewakingsservices voor OMS door in een centrale opslagplaats gegevens te verzamelen van beheerde resources. Deze gegevens kunnen gebeurtenissen, prestatiegegevens en aangepaste gegevens omvatten die via de API worden geleverd. Na verzameling zijn de gegevens beschikbaar voor waarschuwingen, analyse en export. Met deze methode kunt u gegevens uit een groot aantal bronnen consolideren, zodat u gegevens uit uw Azure-services met uw bestaande on-premises omgeving kunt combineren. De methode maakt ook een duidelijk onderscheid tussen het verzamelen van gegevens en het bewerken hiervan. Zo zijn alle bewerkingen beschikbaar voor alle soorten gegevens.

### <a name="automation"></a>Automatisering
Microsoft [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) biedt een manier voor gebruikers om de handmatige, langlopende, foutgevoelige en regelmatig herhaalde taken die vaak worden uitgevoerd in een cloud en enterprise-omgeving te automatiseren. Het bespaart tijd en verhoogt de betrouwbaarheid van regelmatige beheertaken. Deze worden zelfs gepland om met regelmatige intervallen automatisch te worden uitgevoerd. U kunt processen automatiseren met behulp van runbooks of configuratiebeheer automatiseren met behulp van Desired State Configuration.

### <a name="backup"></a>Back-up
[Azure Backup](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) is de service op basis van Azure kunt u back-up (of beveiligen) en het herstellen van uw gegevens in de Microsoft cloud. Met Azure Backup vervangt u uw bestaande on-premises of off-site back-upoplossing door een betrouwbare, veilige en kostenbesparende cloudoplossing. Azure Backup biedt meerdere onderdelen die u kunt downloaden en implementeren op de desbetreffende computer, server, of in de cloud. Welk onderdeel, of welke agent, u implementeert, is afhankelijk van wat u wilt beveiligen. Alle onderdelen van Azure Backup (ongeacht of u gegevens on-premises of in de cloud wilt beveiligen) kunnen worden gebruikt om back-ups te maken naar een Recovery Services-kluis in Azure. Zie de [Azure Backup-onderdelen tabel](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup#which-azure-backup-components-should-i-use).

### <a name="site-recovery"></a>Siteherstel
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) biedt bedrijfscontinuïteit bij het orkestreren van replicatie van on-premises virtuele en fysieke machines naar Azure of een secundaire site. Als uw primaire site niet beschikbaar is, kunt u een failover toepassen naar de secundaire locatie zodat gebruikers kunnen doorwerken en failback toepassen wanneer systemen weer normaal beginnen te functioneren. intelligente en effectieve bedreigingsdetectie.

## <a name="azure-active-directory"></a>Azure Active Directory
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-enable-sso-scenario) is uitgebreide identiteit als een Service (IDaaS) van Microsoft die:

-   Hiermee kunt u IAM als een cloudservice
-   Voorziet in centraal toegangsbeleid beheer, eenmalige aanmelding (SSO) en rapportage
-   Ondersteunt de geïntegreerde access management voor [duizenden toepassingen](https://azure.microsoft.com/marketplace/active-directory/) in de galerie met toepassingen, inclusief Salesforce, Google Apps, Box, Concur en meer.

Azure AD bevat ook een volledige reeks [mogelijkheden voor identiteitsbeheer](https://docs.microsoft.com/azure/security/security-identity-management-overview#security-monitoring-alerts-and-machine-learning-based-reports) inclusief [multi-factorauthenticatie](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), [apparaatregistratie]( https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-overview), [ Selfservice voor wachtwoordherstel management](https://azure.microsoft.com/resources/videos/self-service-password-reset-azure-ad/), [selfservicegroepsbeheer](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password), [bevoorrecht accountbeheer](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure), [op rollen gebaseerde toegangsbeheer](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is), [gebruik toepassingsbewaking](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health), [uitgebreide controle](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), en [beveiligingsbewaking en waarschuwingen](https://docs.microsoft.com/azure/operations-management-suite/oms-security-responding-alerts).

Met Azure Active Directory, alle toepassingen die u publiceert voor uw partners en klanten (werk of consumer) hebben dezelfde identiteit en toegang tot beheermogelijkheden. Hiermee kunt u de operationele kosten aanzienlijk te verlagen.

## <a name="azure-security-center"></a>Azure Security Center
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-get-started) helpt u bij het detecteren, voorkomen van en reageren op bedreigingen dankzij een verhoogde zichtbaarheid van en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

[Security Center](https://docs.microsoft.com/azure/security-center/security-center-linux-virtual-machine) helpt u gegevens van de virtuele machine in Azure beveiligt door te geven inzicht in de beveiligingsinstellingen van uw virtuele machine en bewaking voor bedreigingen. Security Center kan uw virtuele machines controleren op:

-   Beveiligingsinstellingen voor het besturingssysteem met de aanbevolen configuratieregels
-   Updates voor systeembeveiliging en essentiële updates die ontbreken
-   Aanbevelingen voor eindpuntbeveiliging
-   Validatie voor schijfversleuteling
-   Aanvallen op het netwerk

Maakt gebruik van Azure Security Center [op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure), waarmee u [ingebouwde rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) die kunnen worden toegewezen aan gebruikers, groepen en -services in Azure.

Security Center beoordeelt de configuratie van uw resources om beveiligingsproblemen en beveiligingslekken te identificeren. In Security Center ziet u alleen informatie met betrekking tot een bron wanneer u de rol van eigenaar, bijdrager of lezer voor het abonnement of resourcegroep die deel uitmaakt van een resource te worden toegewezen.

>[!Note]
>Zie [machtigingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-permissions) voor meer informatie over de functies en de toegestane acties in Security Center.

Security Center maakt gebruik van Microsoft Monitoring Agent – dit is dezelfde agent gebruikt door de Operations Management Suite en Log Analytics-service. Gegevens die worden verzameld van deze agent wordt opgeslagen in een bestaande logboekanalyse [werkruimte](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access) die zijn gekoppeld aan uw Azure-abonnement of een nieuwe workspace(s), rekening houdend met de geolocatie van de virtuele machine.

## <a name="azure-monitor"></a>Azure Monitor
Prestatieproblemen in uw cloud-app kunnen invloed hebben op uw bedrijf. Degradations kan met meerdere onderdelen onderling verbonden en releases vaak gebeuren op elk gewenst moment. En als u een app ontwikkelt, uw gebruikers problemen die u hebt niet vinden in het testen van meestal te detecteren. U moet weten over deze problemen direct en hulpprogramma's voor het opsporen en oplossen van de problemen hebben.

[Monitor voor Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) is basic hulpprogramma voor het bewaken van services die worden uitgevoerd op Azure. Dit biedt u niveau van de infrastructuur gegevens over de doorvoer van een service en de omgeving. Als u uw apps in Azure beheert, krijgt bepalen of schaal omhoog of omlaag resources, Azure Monitor u dan gebruik om te starten.

Bovendien kunt u bewakingsgegevens grondige om inzicht te krijgen over uw toepassing. Deze kennis kan u helpen bij het verbeteren van de prestaties van toepassingen of onderhoud of acties die anders worden handmatige interventie moeten automatiseren. Het bevat:

-   Azure Activity Log
-   Azure diagnostische logboeken
-   Metrische gegevens
-   Azure Diagnostics

### <a name="azure-activity-log"></a>Azure Activity Log
Het is een logboek biedt inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement. De [activiteitenlogboek](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) heette vroeger 'Controlelogboeken' of 'Operationele Logs', omdat het besturingselement vlak gebeurtenissen voor uw abonnementen rapporten.

### <a name="azure-diagnostic-logs"></a>Azure diagnostische logboeken
[Azure diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) worden gegenereerd door een resource en leveren van uitgebreide, regelmatig gegevens over de werking van de bron. De inhoud van deze logboeken varieert per resourcetype.

Bijvoorbeeld Windows-gebeurtenislogboeken system zijn één categorie van diagnostische logboeken voor virtuele machines en blob, table en queue logboeken zijn categorieën van diagnostische logboeken voor opslagaccounts.

Diagnostische logboeken afwijken van de [activiteitenlogboek (voorheen bekend als het auditlogboek of operationeel logboek)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Het activiteitenlogboek verschaft inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement. Diagnostische logboeken bieden inzicht in bewerkingen dat de bron zelf uitgevoerd.

### <a name="metrics"></a>Metrische gegevens
Monitor voor Azure kunt u telemetrie om meer inzicht verkrijgen in de prestaties en de status van uw workloads in Azure gebruiken. De belangrijkste type Azure telemetriegegevens is de metrische gegevens die (ook wel prestatiemeteritems) die door de meeste Azure-resources. Azure biedt verschillende manieren configureren en gebruiken deze [metrische gegevens](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) voor bewaking en probleemoplossing.

### <a name="azure-diagnostics"></a>Azure Diagnostics
De mogelijkheid in Azure waarmee het verzamelen van diagnostische gegevens op een geïmplementeerde toepassing is. U kunt de extensie voor diagnostische gegevens uit diverse verschillende bronnen. Op dit moment ondersteund zijn [Azure Cloud Service-Web- en werkrollen](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [Azure Virtual Machines](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) met Microsoft Windows, en [Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics).


## <a name="network-watcher"></a>Network Watcher
Een end-to-end-netwerk in Azure opbouwen klanten door te organiseren en samenstellen van verschillende afzonderlijke netwerkbronnen zoals VNet, ExpressRoute, Application Gateway, Load balancers en meer. Bewaking is beschikbaar op elk van de netwerkbronnen.

Het end-to-end-netwerk kunt complexe configuraties en interacties tussen resources, het maken van complexe scenario's die bewaking op basis van een scenario via netwerk-Watcher nodig hebben.

[Netwerk-Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) wordt vereenvoudigt bewaking en het onderzoeken van uw Azure-netwerk. Diagnostische en visualisatie hulpprogramma's beschikbaar met de netwerk-Watcher inschakelen deel te nemen van externe pakket vastgelegd op een virtuele Machine van Azure, beter inzicht in uw netwerk met behulp van Logboeken van de stroom van verkeer en onderzoeken van VPN-Gateway en verbindingen.

Netwerk-Watcher heeft momenteel de volgende mogelijkheden:

- [Topologie](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) -biedt een netwerk niveau weergave voor de verschillende verbindingskabels en koppelingen tussen netwerkbronnen in een resourcegroep.
-   [Variabele pakketopname](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) -pakketgegevens naar en vanuit een virtuele machine worden vastgelegd. Geavanceerde filteropties en verfijnd besturingselementen zoals kunnen tijd instellen en de grootte van beperkingen bieden veelzijdigheid. De pakketgegevens kunnen worden opgeslagen in een blob-opslag of op de lokale schijf in de CAP-indeling.
-   [IP-stromen controleren](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) -controleert of een pakket wordt toegestaan of geweigerd op basis van stroom informatie 5-tuple pakket parameters (doel-IP, bron-IP, doelpoort, bronpoort en Protocol). Als het pakket is geweigerd door een beveiligingsgroep, wordt de regel en de groep die het pakket geweigerd geretourneerd.
-   [Volgende hop](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) -bepaalt de volgende hop voor pakketten worden gerouteerd in de Azure-netwerk Fabric, zodat u voor het vaststellen van een gebruiker gedefinieerde routes onjuist geconfigureerd.
-   [Groep beveiligingsweergave](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) -opgehaald van de effectieve en toegepaste beveiligingsregels voor verbindingen die worden toegepast op een virtuele machine.
-   [Logboekregistratie NSG Flow](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) -stroom in de logboeken voor Netwerkbeveiligingsgroepen kunt u de logboeken die betrekking hebben op het verkeer dat wordt toegestaan of geweigerd door de beveiligingsregels voor verbindingen in de groep opnemen. De stroom wordt gedefinieerd door de gegevens van een 5-tuple: bron-IP, doel-IP, bronpoort, doelpoort en -Protocol.
-   [Virtuele netwerkgateway en verbinding probleemoplossing](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) -biedt de mogelijkheid om op te lossen virtuele netwerkgateways en verbindingen.
-   [Netwerk-abonnementen](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) -Hiermee kunt u gebruik van netwerkbronnen op basis van limieten weergeven.
-   [Configureren van diagnostische gegevens logboek](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) – biedt één of diagnostische logboeken voor netwerkbronnen in een resourcegroep uit te schakelen.

Voor meer informatie over het configureren van netwerk-watcher Zie [configureren van netwerk-watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="developer-operations-devops"></a>Developer-bewerkingen (DevOps)
Voorafgaand aan DevOps toepassingsontwikkeling zijn teams verantwoordelijk voor het verzamelen van zakelijke vereisten voor een softwareprogramma en schrijven van code. Een afzonderlijk QA-team test vervolgens het programma in een geïsoleerde ontwikkelingsomgeving als vereisten zijn voldaan en versies van de code voor bewerkingen voor het implementeren van. Het implementatieteam meer gefragmenteerd zijn in siloed groepen zoals netwerk- en database. Telkens wanneer een softwareprogramma 'gegenereerd via de wanden' naar een onafhankelijke team het voegt knelpunten.

[DevOps](https://www.visualstudio.com/learn/what-is-devops/) kunnen teams beter beveiligde, hogere kwaliteit oplossingen sneller en goedkoper. Klanten verwachten een dynamische en betrouwbare ervaring wanneer software en -services worden verbruikt.  Software-updates snel teams moeten herhalen meten van de invloed van de updates en snel reageren met nieuwe ontwikkeling iteraties om problemen te verhelpen, of geef meer waarde.  Cloud platformen, zoals Microsoft Azure hebt verwijderd van de traditionele knelpunten en geholpen commoditize infrastructuur. Software presteert minder in elk bedrijf als de belangrijkste onderscheid en factor in bedrijfsresultaten. Geen organisatie, developer of IT-worker kan of het DevOps-verkeer moet worden voorkomen.

Professionals in volwassen DevOps vast aantal van de volgende procedures. Deze procedures [personen omvatten](https://www.visualstudio.com/learn/what-is-devops-culture/) naar strategieën van formulier op basis van het bedrijfsscenario's.  Tooling kunt automatiseren van de verschillende manieren:

-   [Flexibele planning en projectbeheer](https://www.visualstudio.com/learn/what-is-agile/) technieken worden gebruikt om te plannen en werk in sprints isoleren, team capaciteit beheren en kunnen teams snel aan te passen aan veranderende bedrijfsbehoeften.
-   [Versiebeheer, meestal met Git](https://www.visualstudio.com/learn/what-is-git/), kunnen zich overal ter wereld bron delen en integreren met software ontwikkelingsprogramma's voor het automatiseren van de pijplijn release teams.
-   [Continue integratie](https://www.visualstudio.com/learn/what-is-continuous-integration/) lopende samenvoegen en het testen van code, wat leidt tot het vinden van defecte producten vroeg stations.  Andere voordelen zijn minder tijd verspild op bestrijden samenvoegen problemen en snelle feedback voor ontwikkelteams.
-   [Continue levering](https://www.visualstudio.com/learn/what-is-continuous-delivery/) van softwareoplossingen voor productie en testen omgevingen helpen organisaties snel fouten corrigeren en reageren op steeds veranderende bedrijfsvereisten.
-   [Bewaking](https://www.visualstudio.com/learn/what-is-monitoring/) van actieve toepassingen, waaronder productieomgevingen voor toepassingsstatus als klant gebruik help organisaties formulier evenals met een hypothese en snel valideren of strategieën bewijzen.  Uitgebreide gegevens worden vastgelegd en opgeslagen in verschillende indelingen voor logboekregistratie.
-   [Infrastructuur als de Code (IaC)](https://www.visualstudio.com/learn/what-is-infrastructure-as-code/) aanbevolen, waardoor de automatisering en de validatie van het maken en teardown van netwerken en virtuele machines om te helpen bij het leveren van beveiligde, stabiele toepassing hosting-platforms.
-   [Microservices](https://www.visualstudio.com/learn/what-are-microservices/) architectuur wordt gebruikt om te isoleren van zakelijke gebruiksvoorbeelden in kleine herbruikbare services.  Deze architectuur kunt u schaalbaarheid en efficiëntie.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over OMS beveiligings- en Audit-oplossing, de volgende artikelen:

- [Operations Management Suite | Beveiliging en naleving](https://www.microsoft.com/cloud-platform/security-and-compliance).
- [Bewaking en Responding to Security Alerts in de beveiliging van Operations Management Suite en Audit oplossing](https://docs.microsoft.com/azure/operations-management-suite/oms-security-responding-alerts).
- [Bewaking van Resources in de beveiliging van Operations Management Suite en Audit oplossing](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources).
