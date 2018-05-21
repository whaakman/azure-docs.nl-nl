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
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: c0413678aad16105f732ef23fb60c61fddcdad45
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="azure-operational-security-overview"></a>Overzicht van Azure operationele beveiliging
[Azure bedrijfsbeveiliging](https://docs.microsoft.com/azure/security/azure-operational-security) verwijst naar de services, besturingselementen en functies die beschikbaar zijn voor gebruikers voor het beveiligen van hun gegevens, toepassingen en andere elementen in Microsoft Azure. Er is een raamwerk waarin de kennis die is opgedaan met tal van mogelijkheden die uniek voor Microsoft zijn. Deze mogelijkheden zijn Microsoft Security Development Lifecycle (SDL), de Microsoft Security Response Center-programma en grondige kennis van de threat cybersecurity Liggend.

## <a name="operations-management-suite"></a>Operations Management Suite
Een IT-team bewerkingen is verantwoordelijk voor het beheren van datacenter-infrastructuur, toepassingen en gegevens, inclusief de stabiliteit en beveiliging van deze systemen. Security insights krijgen over de vaak complexe IT-omgevingen verhogen vereist echter organisaties cobble samen gegevens uit meerdere systemen voor beveiliging en beheer.

[Microsoft Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) is een cloud-gebaseerde IT beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud-infrastructuur. De kernfunctionaliteit wordt verstrekt door de volgende services die worden uitgevoerd in Azure. Elke service biedt een functie van de specifieke beheerserver. U kunt combineren services om te zorgen voor verschillende scenario's. 

### <a name="log-analytics"></a>Log Analytics
[Azure Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) bewakingsservices voor Operations Management Suite biedt door het verzamelen van gegevens van beheerde bronnen in een centrale opslagplaats. Deze gegevens kunnen gebeurtenissen, prestatiegegevens of aangepaste worden geleverd via de API bevatten. Nadat de gegevens worden verzameld, is het beschikbaar voor waarschuwingen, analyse en exporteren. 

U kunt gegevens uit diverse bronnen consolideren en gegevens van uw Azure-services worden gecombineerd met uw bestaande on-premises omgeving. Het verzamelen van de gegevens logboekanalyse ook duidelijk gescheiden van de actie op die van die gegevens, zodat alle acties beschikbaar voor alle soorten gegevens zijn.

### <a name="automation"></a>Automation
[Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) biedt een manier om de handmatige, langlopende, foutgevoelige en regelmatig herhaalde taken die vaak worden uitgevoerd in een cloud en enterprise-omgeving te automatiseren. Dit bespaart tijd en verhoogt de betrouwbaarheid van administratieve taken. Deze taken worden automatisch uitgevoerd met regelmatige tussenpozen worden zelfs gepland. U kunt processen automatiseren met behulp van runbooks of Configuratiebeheer automatiseren met behulp van Desired State Configuration.

### <a name="backup"></a>Backup
[Azure Backup](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) is de service op basis van Azure kunt u back-up (of beveiligen) en het herstellen van uw gegevens in de Microsoft Cloud. Azure Backup vervangt uw bestaande on-premises of off-site back-upoplossing door een cloud-gebaseerde oplossing die betrouwbare, veilige en kostenbesparende cloudoplossing. 

Azure Backup biedt onderdelen die u downloaden en implementeren op de gewenste computer of server of in de cloud. Welk onderdeel, of welke agent, u implementeert, is afhankelijk van wat u wilt beveiligen. Alle onderdelen van de Azure Backup kunnen (of u bij het beveiligen van gegevens die lokaal of in de cloud) worden gebruikt voor het back-ups naar een Azure Recovery Services-kluis in Azure. 

Zie voor meer informatie de [Azure Backup-onderdelen tabel](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup#which-azure-backup-components-should-i-use).

### <a name="site-recovery"></a>Site Recovery
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) biedt bedrijfscontinuïteit door de replicatie van de lokale virtuele en fysieke machines in Azure of naar een secundaire site te organiseren. Als uw primaire site niet beschikbaar is, schakelt u over naar de secundaire locatie zodat gebruikers kunnen blijven werken. U mislukken wanneer systemen te werk om retourneren. Azure Security Center gebruiken om uit te voeren intelligente en effectieve bedreigingsdetectie.

## <a name="azure-active-directory"></a>Azure Active Directory
[Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-enable-sso-scenario) is een uitgebreide identity-service die:

-   Hiermee identiteits- en toegangsbeheer beheer (IAM) als een cloudservice.
-   Biedt centraal toegangsbeleid, eenmalige aanmelding (SSO) en rapportage.
-   Ondersteunt de geïntegreerde access management voor [duizenden toepassingen](https://azure.microsoft.com/marketplace/active-directory/) in Azure Marketplace, met inbegrip van Salesforce, Google Apps, Box en Concur.

Azure AD bevat ook een volledige reeks [mogelijkheden voor identiteitsbeheer](https://docs.microsoft.com/azure/security/security-identity-management-overview#security-monitoring-alerts-and-machine-learning-based-reports), waaronder de volgende:

- [Multi-factor authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Apparaatregistratie]( https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-overview)
- [Selfservice voor wachtwoordherstel management](https://azure.microsoft.com/resources/videos/self-service-password-reset-azure-ad/)
- [Self-service groepsbeheer](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password)
- [Bevoegd accountmanagement](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure)
- [Toegangsbeheer op basis van rollen](https://docs.microsoft.com/azure/role-based-access-control/overview)
- [Gebruik toepassingsbewaking](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health)
- [Uitgebreide controle](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Beveiligingsbewaking en waarschuwingen](https://docs.microsoft.com/azure/operations-management-suite/oms-security-responding-alerts)

Met Azure Active Directory, alle toepassingen die u voor uw partners en klanten (werk of consumer) publiceren hebben dezelfde identiteit en toegang tot beheermogelijkheden. Hiermee kunt u de operationele kosten aanzienlijk te verlagen.

## <a name="azure-security-center"></a>Azure Security Center
[Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro) helpt u te voorkomen, detecteren, en reageren op bedreigingen dankzij een verhoogde zichtbaarheid van (en meer controle over) de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw abonnementen. Het helpt bedreigingen die anders onopgemerkt detecteren en dit proces werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

[Bescherming van gegevens van de virtuele machine (VM)](https://docs.microsoft.com/azure/security-center/security-center-linux-virtual-machine) in Azure door te geven inzicht in de beveiligingsinstellingen van uw virtuele machine en bewaking voor bedreigingen. Security Center kan uw virtuele machines controleren op:

-   Beveiligingsinstellingen besturingssysteem aan de regels van de aanbevolen configuratie.
-   Systeem beveiligingsupdates en essentiële updates ontbreken.
-   Aanbevelingen voor endpoint protection.
-   Validatie van de schijf-versleuteling.
-   Aanvallen op het netwerk.

Security Center gebruikt [op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal). RBAC biedt [ingebouwde rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) die kunnen worden toegewezen aan gebruikers, groepen en -services in Azure.

Security Center beoordeelt de configuratie van uw resources om beveiligingsproblemen en beveiligingslekken te identificeren. In Security Center ziet u informatie met betrekking tot een resource alleen als u krijgt u de rol van eigenaar, bijdrager of lezer voor het abonnement of resourcegroep die een bron deel uitmaakt.

>[!Note]
>Voor meer informatie over rollen en toegestane acties in Security Center, Zie [machtigingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-permissions).

Security Center maakt gebruik van Microsoft Monitoring Agent. Dit is dezelfde agent die gebruikmaken van Operations Management Suite en de Log Analytics-service. Gegevens die worden verzameld van deze agent wordt opgeslagen in een bestaande logboekanalyse [werkruimte](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access) die zijn gekoppeld aan uw Azure-abonnement of een nieuwe werkruimte, rekening houdend met de geolocatie van de virtuele machine.

## <a name="azure-monitor"></a>Azure Monitor
Prestatieproblemen in uw cloud-app kunnen invloed hebben op uw bedrijf. Degradations kan met meerdere onderdelen onderling verbonden en releases vaak gebeuren op elk gewenst moment. En als u een app ontwikkelt, uw gebruikers problemen die u hebt niet vinden in het testen van meestal te detecteren. U moet weten over deze problemen onmiddellijk en moet u hulpprogramma's voor het opsporen en oplossen van de problemen hebben.

[Monitor voor Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) is basic hulpprogramma voor het bewaken van services die worden uitgevoerd op Azure. Dit biedt u niveau van de infrastructuur gegevens over de doorvoer van een service en de omgeving. Als u uw apps in Azure en bepalen of schaal omhoog of omlaag resources beheert, is Azure Monitor de plaats waar u begint.

U kunt ook bewakingsgegevens grondige om inzicht te krijgen over uw toepassing. Deze kennis kan u helpen bij het verbeteren van de prestaties van toepassingen of onderhoud of acties die anders worden handmatige interventie moeten automatiseren. 

Monitor voor Azure omvat de volgende onderdelen.

### <a name="azure-activity-log"></a>Azure Activity Log
De [Azure Activity Log](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) biedt inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement. Deze heette vroeger 'Controlelogboek' of 'Operationele logboek', omdat het besturingselement vlak gebeurtenissen voor uw abonnementen rapporteert.

### <a name="azure-diagnostic-logs"></a>Diagnostische logboeken in Azure
[Azure diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) worden gegenereerd door een resource en leveren van uitgebreide, regelmatig gegevens over de werking van de bron. De inhoud van deze logboeken varieert per resourcetype.

Windows-gebeurtenislogboeken system zijn één categorie van diagnostische logboeken voor virtuele machines. BLOB, table en queue logboeken zijn categorieën van diagnostische logboeken voor storage-accounts.

Diagnostische logboeken afwijken van de [activiteitenlogboek](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Het activiteitenlogboek verschaft inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement. Diagnostische logboeken bieden inzicht in bewerkingen dat de bron zelf uitgevoerd.

### <a name="metrics"></a>Metrische gegevens
Azure biedt een telemetrie waarmee u inzicht in de prestaties en de status van uw workloads in Azure. Het belangrijkste Azure telemetriegegevens type is de [metrische gegevens](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) (ook wel prestatiemeteritems) die door de meeste Azure-resources. Monitor voor Azure biedt verschillende manieren configureren en gebruiken van deze metrische gegevens voor bewaking en probleemoplossing.

### <a name="azure-diagnostics"></a>Azure Diagnostics
Azure Diagnostics kan het verzamelen van diagnostische gegevens op een geïmplementeerde toepassing. U kunt de extensie voor diagnostische gegevens uit diverse bronnen gebruiken. Op dit moment ondersteund zijn [Azure cloud service-rollen](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [virtuele Azure-machines](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) met Microsoft Windows, en [Azure Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics).


## <a name="azure-network-watcher"></a>Azure-netwerk-Watcher
Klanten een end-to-end-netwerk in Azure bouwen door te organiseren en samenstellen van individuele netwerkresources zoals Azure ExpressRoute, Azure Application Gateway, virtuele netwerken en taakverdelers. Bewaking is beschikbaar op elk van de netwerkbronnen.

Het end-to-end-netwerk kunt complexe configuraties en interacties tussen resources hebben. Het resultaat is een complexe scenario's waarvoor bewaking op basis van een scenario via [Azure-netwerk-Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).

Netwerk-Watcher vereenvoudigt bewaking en het onderzoeken van uw Azure-netwerk. U kunt de hulpprogramma's voor diagnose en visualisatie in netwerk-Watcher te gebruiken:
- Externe pakketten opnamen ondernemen in Azure een virtuele machine.
- Beter inzicht in het netwerkverkeer met behulp van Logboeken van de stroom.
- Onderzoeken Azure VPN-Gateway en verbindingen.

Netwerk-Watcher heeft momenteel de volgende mogelijkheden:

- [Topologie](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview): biedt een overzicht van de verschillende verbindingskabels en koppelingen tussen netwerkbronnen in een resourcegroep.
-   [Variabele pakketopname](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview): pakketgegevens naar en vanuit een virtuele machine worden vastgelegd. Geef veelzijdigheid Geavanceerde filteropties en verfijnd besturingselementen, zoals de mogelijkheid voor het instellen van tijd en het formaat van beperkingen. De pakketgegevens kunnen worden opgeslagen in een blob-opslag of op de lokale schijf in de CAP-indeling.
-   [IP-stroom controleren](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview): controleert of een pakket wordt toegestaan of geweigerd op basis van 5-tuple pakket parameters voor informatie over de gebeurtenisstroom (doel-IP, bron-IP, doelpoort, bronpoort en protocol). Als u een beveiligingsgroep weigert het pakket, de regel en de groep die het pakket geweigerd geretourneerd.
-   [Volgende hop](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview): Hiermee bepaalt u de volgende hop voor pakketten in de fabric Azure-netwerk worden gerouteerd, zodat u kunt een diagnosticeren gebruiker gedefinieerde routes onjuist geconfigureerd.
-   [Groep beveiligingsweergave](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview): de effectieve en toegepaste beveiligingsregels voor verbindingen die worden toegepast op een virtuele machine opgehaald.
-   [NSG-logboeken stroom voor netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview): Hiermee kunt u voor het vastleggen van logboeken die betrekking hebben op het verkeer dat is toegestaan of geweigerd door de beveiligingsregels voor verbindingen in de groep. De stroom wordt gedefinieerd door de 5-tuple informatie: bron-IP, doel-IP, bronpoort, doelpoort en -protocol.
-   [Virtuele netwerkgateway en verbinding probleemoplossing](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest): biedt de mogelijkheid om op te lossen virtuele netwerkgateways en verbindingen.
-   [Netwerk-abonnementen](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): Hiermee kunt u gebruik van netwerkbronnen op basis van limieten weergeven.
-   [Diagnostische logboeken](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): biedt één of diagnostische logboeken voor netwerkbronnen in een resourcegroep uit te schakelen.

Zie voor meer informatie [configureren netwerk-Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="devops"></a>DevOps
Voordat u [Developer-bewerkingen (DevOps)](https://www.visualstudio.com/learn/what-is-devops/) toepassingsontwikkeling, teams zijn verantwoordelijk voor het verzamelen van zakelijke vereisten voor een softwareprogramma en schrijven van code. Een afzonderlijk QA-team getest vervolgens het programma in een geïsoleerde ontwikkelingsomgeving. Als vereisten zijn voldaan, wordt er door het team QA de code voor bewerkingen voor het implementeren van uitgebracht. Het implementatieteam zijn meer gefragmenteerd in groepen zoals netwerk- en database. Telkens wanneer een softwareprogramma is 'geretourneerd via de wanden' naar een onafhankelijke team deze knelpunten toegevoegd.

DevOps kan teams leveren veiliger, hogere kwaliteit oplossingen sneller en goedkoper. Klanten verwachten een dynamische en betrouwbare ervaring wanneer software en -services worden verbruikt. Teams moeten snel herhalen software-updates en de invloed van de updates gemeten. Ze moeten snel reageren met nieuwe ontwikkeling iteraties om problemen te verhelpen of meer waarde bieden.  

Cloud platformen, zoals Microsoft Azure hebt verwijderd van de traditionele knelpunten en geholpen commoditize infrastructuur. Software presteert minder in elk bedrijf als de belangrijkste onderscheid en factor in bedrijfsresultaten. Geen organisatie, developer of IT-worker kan of het DevOps-verkeer moet worden voorkomen.

Professionals in volwassen DevOps vast aantal van de volgende procedures. Deze procedures [personen omvatten](https://www.visualstudio.com/learn/what-is-devops-culture/) naar strategieën van formulier op basis van het bedrijfsscenario's. Tooling kunt de verschillende procedures te automatiseren.

-   [Flexibele planning en projectbeheer](https://www.visualstudio.com/learn/what-is-agile/) technieken worden gebruikt om te plannen en werk in sprints isoleren, team capaciteit beheren en kunnen teams snel aan te passen aan veranderende bedrijfsbehoeften.
-   [Versiebeheer, meestal met Git](https://www.visualstudio.com/learn/what-is-git/), kunnen zich overal ter wereld bron delen en integreren met software ontwikkelingsprogramma's voor het automatiseren van de pijplijn release teams.
-   [Continue integratie](https://www.visualstudio.com/learn/what-is-continuous-integration/) lopende samenvoegen en het testen van code, wat leidt tot het vinden van defecte producten vroeg stations.  Andere voordelen zijn minder tijd verspild op bestrijden samenvoegen problemen en snelle feedback voor ontwikkelteams.
-   [Continue levering](https://www.visualstudio.com/learn/what-is-continuous-delivery/) van softwareoplossingen met productie en testen omgevingen helpt organisaties snel fouten corrigeren en reageren op steeds veranderende bedrijfsvereisten.
-   [Bewaking](https://www.visualstudio.com/learn/what-is-monitoring/) van actieve toepassingen--met inbegrip van productie-omgeving voor de toepassingsstatus, evenals het gebruik van de klant--helpt organisaties vormen een hypothese snel valideren of strategieën bewijzen.  Uitgebreide gegevens worden vastgelegd en opgeslagen in verschillende indelingen voor logboekregistratie.
-   [Infrastructuur als de Code (IaC)](https://www.visualstudio.com/learn/what-is-infrastructure-as-code/) is een procedure waarmee de automatisering en de validatie van het maken en teardown van netwerken en virtuele machines om te helpen bij het leveren van beveiligde, stabiele toepassing hosting-platforms.
-   [Microservices](https://www.visualstudio.com/learn/what-are-microservices/) architectuur wordt gebruikt voor het isoleren van zakelijke gebruiksvoorbeelden in kleine herbruikbare services.  Deze architectuur kunt u schaalbaarheid en efficiëntie.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de beveiliging van Operations Management Suite en Audit-oplossing, de volgende artikelen:

- [Beveiliging en naleving](https://www.microsoft.com/cloud-platform/security-and-compliance)
- [Controleren en reageren op beveiligingswaarschuwingen in de beveiliging van Operations Management Suite en Audit-oplossing](https://docs.microsoft.com/azure/operations-management-suite/oms-security-responding-alerts)
- [Bewaking van resources in de beveiliging van Operations Management Suite en Audit-oplossing](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources)
