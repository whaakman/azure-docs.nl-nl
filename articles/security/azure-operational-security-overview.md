---
title: Overzicht van Azure operationele beveiliging | Microsoft Docs
description: Dit artikel bevat een overzicht van Azure operationele beveiliging.
services: security
documentationcenter: na
author: unifycloud
manager: mbaldwin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: tomsh
ms.openlocfilehash: 547943b04fdfb03a3eccf12a66772992f7fa6212
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50249357"
---
# <a name="azure-operational-security-overview"></a>Overzicht van Azure operationele beveiliging

[Azure operational security](https://docs.microsoft.com/azure/security/azure-operational-security) verwijst naar de services, besturingselementen en functies die beschikbaar zijn voor gebruikers voor het beveiligen van hun gegevens, toepassingen en andere items in Microsoft Azure. Er is een structuur waarin de kennis opgedaan uit een verscheidenheid aan functies die uniek voor Microsoft zijn. Deze mogelijkheden omvatten de Microsoft Security Development Lifecycle (SDL), het programma Microsoft Security Response Center en diep besef van het landschap van cyberveiligheidsbedreigingen.

## <a name="azure-management-services"></a>Azure-beheerservices

IT operations-teams is verantwoordelijk voor het beheren van datacenter-infrastructuur, toepassingen en gegevens, inclusief de stabiliteit en beveiliging van deze systemen. Verkrijgen van beveiligingsinzichten op in complexe IT-omgevingen vaak verhogen vereist echter organisaties cobble samen gegevens van meerdere systemen voor beveiliging en beheer.

[Microsoft Log Analytics](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) is een cloudgebaseerde IT-beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloudinfrastructuur. De kernfunctionaliteit wordt geleverd door de volgende services die worden uitgevoerd in Azure. Azure bevat meerdere services die u helpen beheren en beveiligen van uw on-premises en cloudinfrastructuur. Elke service biedt een specifieke beheerfunctie. U kunt services voor het bereiken van verschillende beheerscenario combineren. 

### <a name="azure-monitor"></a>Azure Monitor

[Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) verzamelt gegevens van beheerde bronnen in de centrale gegevensopslag. Deze gegevens kunnen bevatten gebeurtenissen, prestatiegegevens of aangepaste gegevens die worden geleverd via de API. Nadat de gegevens zijn verzameld, is het beschikbaar voor waarschuwingen, analyse en export. 

U kunt gegevens uit een groot aantal bronnen consolideren en gegevens uit uw Azure-services combineren met uw bestaande on-premises omgeving. Log Analytics ook duidelijk het verzamelen van de gegevens worden gescheiden van de actie wordt uitgevoerd op die gegevens, zodat alle acties beschikbaar voor alle soorten gegevens zijn.

### <a name="automation"></a>Automation

[Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) is een manier waarop u de handmatige, langlopende, foutgevoelige en regelmatig herhaalde taken die vaak worden uitgevoerd in een cloud- en enterprise-omgeving automatiseren. Dit bespaart tijd en verhoogt de betrouwbaarheid van administratieve taken. Deze taken worden automatisch uitgevoerd met regelmatige tussenpozen worden zelfs gepland. U kunt processen automatiseren met behulp van runbooks of Configuratiebeheer automatiseren met behulp van Desired State Configuration.

### <a name="backup"></a>Backup

[Azure Backup](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) is de Azure-gebaseerde service die u gebruiken kunt voor back-up maken (of Bescherm) en het herstellen van uw gegevens in de Microsoft-Cloud. Azure Backup vervangt uw bestaande on-premises of off-site back-upoplossing door een cloudgebaseerde oplossing die is betrouwbare, veilige en kostenbesparende. 

Azure Backup biedt onderdelen die u downloadt en implementeert op de desbetreffende computer of server of in de cloud. Welk onderdeel, of welke agent, u implementeert, is afhankelijk van wat u wilt beveiligen. Alle onderdelen van Azure Backup kunnen (ongeacht of u gegevens on-premises of in de cloud) worden gebruikt voor back-up van gegevens naar een Azure Recovery Services-kluis in Azure. 

Zie voor meer informatie de [Azure Backup onderdelentabel](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup#which-azure-backup-components-should-i-use).

### <a name="site-recovery"></a>Site Recovery

[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) voorziet in bedrijfscontinuïteit door de replicatie van on-premises virtuele en fysieke machines naar Azure of naar een secundaire site te organiseren. Als uw primaire site niet beschikbaar is, schakelt u over naar de secundaire locatie zodat gebruikers kunnen blijven werken. U failback toepassen wanneer systemen weer normaal te functioneren beginnen. Azure Security Center gebruiken om uit te voeren van intelligente en effectieve bedreigingsdetectie.

## <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-enable-sso-scenario) is een uitgebreide identity-service die:

-   Identiteits- en toegangsbeheer management (IAM) als een cloudservice kunt.
-   Biedt centraal toegangsbeleid, eenmalige aanmelding (SSO), en rapportage.
-   Biedt ondersteuning voor geïntegreerde toegangsbeheer voor [duizenden toepassingen](https://azure.microsoft.com/marketplace/active-directory/) in de Azure Marketplace, waaronder Salesforce, Google Apps, Box en Concur.

Azure AD bevat ook een volledige reeks [mogelijkheden voor identiteitsbeheer](https://docs.microsoft.com/azure/security/security-identity-management-overview#security-monitoring-alerts-and-machine-learning-based-reports), waaronder de volgende:

- [Multi-factor authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Selfservice wachtwoordbeheer](https://azure.microsoft.com/resources/videos/self-service-password-reset-azure-ad/)
- [Self-service groepsbeheer](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password)
- [Bevoegd accountmanagement](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure)
- [Op rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/role-based-access-control/overview)
- [Bewaking van toepassingsgebruik](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health)
- [Uitgebreide controle](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Beveiligingsbewaking en waarschuwingen](https://docs.microsoft.com/azure/operations-management-suite/oms-security-responding-alerts)

Met Azure Active Directory, alle toepassingen die u voor uw partners en klanten (bedrijven of consumenten publiceert) hebben dezelfde identiteit en toegang tot de mogelijkheden voor beheer. Hiermee kunt u uw operationele kosten aanzienlijk verlagen.

## <a name="azure-security-center"></a>Azure Security Center

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) helpt u bij het voorkomen, detecteren, en reageer op bedreigingen met verbeterde zichtbaarheid (en controle over) de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw abonnementen. Het helpt bedreigingen detecteren die anders onopgemerkt en het werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

[Bescherming van gegevens van virtuele machines (VM)](https://docs.microsoft.com/azure/security-center/security-center-linux-virtual-machine) in Azure door te geven inzicht in de beveiligingsinstellingen van de virtuele machine en controleren op bedreigingen. Security Center kan uw virtuele machines controleren op:

- Beveiligingsinstellingen besturingssysteem met de aanbevolen configuratieregels.
- Systeembeveiliging en essentiële updates dat ontbreekt.
- Aanbevelingen voor endpoint protection.
- Validatie voor schijfversleuteling.
- Netwerkgebaseerde aanvallen.

Security Center maakt gebruik van [Role-Based Access Control (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal). RBAC biedt [ingebouwde rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) die kunnen worden toegewezen aan gebruikers, groepen en services in Azure.

Security Center beoordeelt de configuratie van uw resources om beveiligingsproblemen met zich mee en beveiligingsproblemen te identificeren. In Security Center ziet u informatie met betrekking tot een resource, alleen wanneer u de rol van eigenaar, bijdrager of lezer voor het abonnement of resourcegroep waartoe een resource behoort bent toegewezen.

>[!Note]
>Voor meer informatie over rollen en toegestane acties in Security Center, Zie [machtigingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-permissions).

Security Center maakt gebruik van de Microsoft Monitoring Agent. Dit is dezelfde agent die gebruikmaakt van de service Log Analytics. Gegevens die worden verzameld van deze agent worden opgeslagen in een bestaande Log Analytics [werkruimte](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access) die zijn gekoppeld aan uw Azure-abonnement of een nieuwe werkruimte, rekening houdend met de geolocatie van de virtuele machine.

## <a name="azure-monitor"></a>Azure Monitor

Prestatieproblemen in uw cloud-app kunnen invloed hebben op uw bedrijf. Degradations kan met meerdere onderdelen onderling verbonden en regelmatige versies, op elk gewenst moment gebeuren. En als u een app ontwikkelt, uw gebruikers gewoonlijk detecteren van problemen die u niet hebt gevonden in de testfase. U moet weten over deze problemen onmiddellijk en hebt u hulpprogramma's voor het vaststellen en oplossen van de problemen.

[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) is basic hulpprogramma voor het bewaken van services die worden uitgevoerd op Azure. Dit biedt u de gegevens op infrastructuurniveau over de doorvoer van een service en de omringende omgeving. Als u uw apps in Azure en bepalen of schaal omhoog of omlaag resources beheert, is Azure Monitor de plek om te beginnen.

U kunt ook bewakingsgegevens diep om inzicht te krijgen over uw toepassing gebruiken. Deze kennis kan u helpen te verbeteren van de prestaties van de toepassing of onderhoud, of Automatiseer acties die anders handmatig worden opgelost moeten zouden. 

Azure Monitor omvat de volgende onderdelen.

### <a name="azure-activity-log"></a>Azure-activiteitenlogboek

De [Azure Activity Log](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) biedt inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement. Het was voorheen bekend als "Auditlogboek" of 'Operationeel logboek', omdat het besturingselement vlak gebeurtenissen voor uw abonnementen.

### <a name="azure-diagnostic-logs"></a>Diagnostische logboeken in Azure

[Diagnostische logboeken in Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) worden gegenereerd door een resource en biedt uitgebreide, regelmatig gegevens over de werking van die resource. Resourcetype is afhankelijk van de inhoud van deze logboeken.

Windows-gebeurtenislogboeken system zijn één categorie van logboeken met diagnostische gegevens voor virtuele machines. BLOB-, tabel en wachtrijlogboeken zijn categorieën van diagnostische logboeken voor opslagaccounts.

Diagnoselogboeken verschillen van de [activiteitenlogboek](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Het activiteitenlogboek biedt inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement. Diagnoselogboeken bieden inzicht in bewerkingen die de resources zelf zijn uitgevoerd.

### <a name="metrics"></a>Metrische gegevens

Azure Monitor biedt telemetrie die u meer inzicht in de prestaties en status van uw workloads op Azure hebt. Het belangrijkste type Azure telemetrische gegevens is de [metrische gegevens](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) (ook wel prestatiemeteritems) die zijn gegenereerd door de meeste Azure-resources. Azure Monitor biedt verschillende manieren om te configureren en gebruiken van deze metrische gegevens voor controle en probleemoplossing.

### <a name="azure-diagnostics"></a>Azure Diagnostics

Azure Diagnostics kunt het verzamelen van diagnostische gegevens op een geïmplementeerde toepassing. U kunt de extensie voor diagnostische gegevens gebruiken uit verschillende bronnen. Op dit moment ondersteund zijn [Azure cloud service-rollen](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [virtuele Azure-machines](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) met Microsoft Windows, en [Azure Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics).

## <a name="azure-network-watcher"></a>Azure Network Watcher

Klanten een end-to-end-netwerk in Azure kunt maken met het organiseren van en schrijven van afzonderlijke netwerkbronnen zoals Azure ExpressRoute, Azure Application Gateway, virtuele netwerken en load balancers. Bewaking is beschikbaar op elk van de netwerkbronnen.

De end-to-end-netwerk kan hebben complexe configuraties en interacties tussen resources. Het resultaat is een complexe scenario's waarvoor de scenario's gebaseerde controle via [Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).

Network Watcher vereenvoudigt bewaking en diagnose van uw Azure-netwerk. U kunt de hulpprogramma's voor diagnose en visualisatie in Network Watcher te gebruiken:

- Externe pakketopnamen uitvoeren op een virtuele machine van Azure.
- Krijg inzicht in uw netwerkverkeer met behulp van Logboeken van de stroom.
- Azure VPN-Gateway en verbindingen vaststellen.

Netwerk-Watcher heeft momenteel de volgende mogelijkheden:

- [Topologie](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview): biedt een overzicht van de verschillende onderlinge verbindingen en koppelingen tussen netwerkbronnen in een resourcegroep.
- [Variabele pakketopname](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview): pakketgegevens in en uit een virtuele machine worden vastgelegd. Geavanceerde filteropties en verfijnde besturingselementen, zoals de mogelijkheid om tijd- en groottebeperkingen in te stellen, bieden flexibiliteit. De pakketgegevens kunnen worden opgeslagen in een blob-opslag of op de lokale schijf in de CAP-indeling.
- [IP-stroom controleren](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview): controleert of een pakket wordt toegestaan of geweigerd op basis van 5-tuple pakket parameters voor informatie over de gebeurtenisstroom (doel-IP, bron-IP, doelpoort, bronpoort en protocol). Als u een beveiligingsgroep weigert het pakket, de regel en de groep die het pakket geweigerd geretourneerd.
- [Volgende hop](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview): Hiermee bepaalt u de volgende hop voor pakketten worden gerouteerd in de infrastructuur van Azure-netwerk, zodat u kunt een diagnose gebruiker gedefinieerde routes onjuist geconfigureerd.
- [Weergave van de beveiligingsgroep](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview): de kracht en toegepaste beveiligingsregels die worden toegepast op een virtuele machine opgehaald.
- [NSG-stroomlogboeken voor netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview): Hiermee kunt u voor het vastleggen van logboeken met betrekking tot het verkeer dat is toegestaan of geweigerd door de beveiligingsregels in de groep. De stroom wordt gedefinieerd door 5-tuple-informatie: bron-IP, doel-IP, bronpoort, bestemmingspoort en protocol.
- [Virtuele netwerkgateway en verbinding probleemoplossing](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest): biedt de mogelijkheid om op te lossen virtuele netwerkgateways en verbindingen.
- [De abonnementslimieten netwerk](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): Hiermee kunt u gebruik van netwerkbronnen op basis van limieten weergeven.
- [Diagnostische logboeken](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): biedt één of diagnostische logboeken voor netwerkbronnen in een resourcegroep uit te schakelen.

Zie voor meer informatie, [Network Watcher configureren](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="cloud-service-provider-access-transparency"></a>Transparantie van de cloud serviceprovider-toegang

[Klanten-Lockbox voor Microsoft Azure](https://azure.microsoft.com/blog/approve-audit-support-access-requests-to-vms-using-customer-lockbox-for-azure/) is een service geïntegreerd in Azure portal waarmee u kunt expliciete controle in het zeldzame exemplaar als een ondersteuningsmedewerker van Microsoft mogelijk toegang tot uw gegevens om een probleem te verhelpen. Er zijn heel weinig instanties, zoals een probleem met foutopsporing externe toegang, waarbij een ondersteuningsmedewerker van Microsoft vereist verhoogde machtigingen om dit probleem te verhelpen. In dergelijke gevallen gebruikt Microsoft-technici just-in-time-access-service die beperkte, tijdsgebonden autorisatie toegang beperkt tot de service biedt.  
Hoewel Microsoft altijd toestemming van de klant om toegang te krijgen verkregen heeft, kunt klanten-Lockbox nu u de mogelijkheid om te controleren en goedkeuren of weigeren van dergelijke aanvragen vanuit de Azure-Portal. Microsoft support-engineers wordt niet toegang verleend totdat u de aanvraag goedkeuren.

## <a name="standardized-and-compliant-deployments"></a>Gestandaardiseerde en compatibele implementaties

[Azure blauwdrukken](../governance/blueprints/overview.md) inschakelen cloudarchitecten en -groepen van centrale informatie technologie voor het definiëren van een herhaalbare set Azure-resources die implementeren en te voldoen aan de standaarden, patronen en vereisten van een organisatie.  
Dit maakt het mogelijk voor DevOps-teams kunnen snel bouwen en nieuwe omgevingen inrichten en vertrouwen dat ze deze gaat bouwen met een infrastructuur die wordt onderhouden door organisatorische naleving. Blauwdrukken bieden een declaratieve manier voor het indelen van de implementatie van verschillende resource-sjablonen en andere artefacten, zoals: 

- Roltoewijzingen
- Beleidstoewijzingen
- Azure Resource Manager-sjablonen
- Resourcegroepen

## <a name="devops"></a>DevOps

Voordat u [Developer-bewerkingen (DevOps)](https://www.visualstudio.com/learn/what-is-devops/) ontwikkelen van toepassingen, teams zijn verantwoordelijk voor het verzamelen van zakelijke vereisten voor een softwareprogramma en het schrijven van code. Een afzonderlijk QA-team wordt vervolgens het programma getest in een geïsoleerde-ontwikkelomgeving. Als vereisten is voldaan, vrijgegeven de QA-team de code voor bewerkingen om te implementeren. Het implementatieteam zijn meer gefragmenteerd in groepen, zoals netwerk- en database. Telkens wanneer een programma is 'gegenereerd via de wand"voor een onafhankelijke team deze knelpunten toegevoegd.

DevOps kan teams beter te beveiligen, hoogwaardige oplossingen sneller en goedkoper leveren. Klanten verwachten een dynamische en betrouwbare ervaring bij het gebruiken van software en services. Teams moeten snel Voortborduren op software-updates en meten van de impact van de updates. Ze moeten reageer snel met nieuwe ontwikkeling iteraties om problemen te verhelpen of meer waarde bieden.  

Cloudplatformen, zoals Microsoft Azure hebt verwijderd van traditionele knelpunten en geholpen commoditize infrastructuur. Software presteert minder in elk bedrijf als belangrijk verschil tussen en de multi-factor Authentication in de bedrijfsresultaten. Er is geen organisatie, ontwikkelaars of IT, werknemer kunt of het DevOps-verkeer moet worden voorkomen.

Goed ontwikkelde DevOps-adepten vast aantal van de volgende procedures. Deze procedures [betrekking hebben op mensen](https://www.visualstudio.com/learn/what-is-devops-culture/) op formulier-strategieën op basis van het bedrijfsscenario's. Hulpprogramma's kunt de verschillende procedures te automatiseren.

- [Flexibele planning en projectbeheer](https://www.visualstudio.com/learn/what-is-agile/) technieken worden gebruikt voor het plannen en werk in sprints isoleren, team capaciteit beheren en kunnen teams snel aan te passen aan veranderende bedrijfsbehoeften.
- [Versiebeheer, meestal met Git](https://www.visualstudio.com/learn/what-is-git/), kunnen teams overal ter wereld te delen bron en integreren met de software-ontwikkelingsprogramma's voor het automatiseren van de release-pijplijn.
- [Continue integratie](https://www.visualstudio.com/learn/what-is-continuous-integration/) stations de lopende samenvoegen en het testen van code, die leiden tot defecten vroeg zoeken.  Andere voordelen zijn minder tijd op het bestrijden van merge problemen en snelle feedback voor ontwikkelteams verspild.
- [Continue levering](https://www.visualstudio.com/learn/what-is-continuous-delivery/) van softwareoplossingen voor productie en testen omgevingen helpt organisaties snel het corrigeren van fouten en reageren op steeds veranderende bedrijfsbehoeften.
- [Bewaking](https://www.visualstudio.com/learn/what-is-monitoring/) van actieve toepassingen, met inbegrip van productie omgevingen voor de status van de toepassing, evenals de klant gebruik--helpt organisaties bij het vormen een hypothese en snel te valideren of te bewijzen strategieën.  Uitgebreide gegevens worden vastgelegd en opgeslagen in verschillende indelingen voor logboekregistratie.
- [Infrastructuur als Code (IaC)](https://www.visualstudio.com/learn/what-is-infrastructure-as-code/) is een procedure waarmee de automatisering en de validatie van het maken en teardown van netwerken en virtuele machines om u te helpen met het leveren van beveiligde, stabiele toepassing hosten van platforms.
- [Microservices](https://www.visualstudio.com/learn/what-are-microservices/) architectuur wordt gebruikt voor het isoleren van zakelijke gebruiksvoorbeelden in kleine herbruikbare services.  Deze architectuur kunt u schaalbaarheid en efficiëntie.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de oplossing beveiliging en controle, de volgende artikelen:

- [Beveiliging en naleving](https://www.microsoft.com/cloud-platform/security-and-compliance)
- [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
- [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview)
