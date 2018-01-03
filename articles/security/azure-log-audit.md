---
title: Azure-logboekregistratie en controle | Microsoft Docs
description: Meer informatie over hoe u logboekgegevens grondige om inzicht te krijgen over uw toepassing kunt gebruiken.
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
ms.openlocfilehash: 032aa4a6cedd49ff9c3b4803561b8b187e8f9af5
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="azure-logging-and-auditing"></a>Azure logboekregistratie en controle
## <a name="introduction"></a>Inleiding
### <a name="overview"></a>Overzicht
Er is een reeks whitepapers, overzichten van de beveiliging, aanbevolen procedures en controlelijsten ontwikkeld om te helpen de huidige en toekomstige Azure-klanten meer informatie over en de verschillende betrekking hebben op beveiliging mogelijkheden beschikbaar zijn in gebruik en rondom de Azure-Platform. De onderwerpen in termen van breedte en diepte liggen en regelmatig worden bijgewerkt. Dit document is onderdeel van de reeks zoals samengevat in de volgende sectie Abstract.
### <a name="azure-platform"></a>Azure-Platform
Azure is een open en flexibele cloud service-platform die ondersteuning biedt voor de breedste selectie van besturingssystemen, programmeertalen, frameworks, hulpprogramma's, databases, en apparaten.

U kunt bijvoorbeeld:
-   Linux-containers worden uitgevoerd met Docker-integratie.

-   Met JavaScript, Python, .NET, PHP, Java en Node.js-apps bouwen

-   Build back-ends voor iOS, Android en Windows apparaten.

Openbare Azure-cloud-services ondersteunen dezelfde technologieën miljoenen ontwikkelaars en IT-professionals die al zijn afhankelijk van en vertrouwt.

Wanneer u bouwen op of IT-activa te migreren, een cloudprovider, u zijn afhankelijk van die organisatie mogelijkheden voor uw toepassingen en gegevens beschermen met de services en de besturingselementen die ze bieden voor het beheren van de beveiliging van uw assets cloud-gebaseerde.

De infrastructuur van Azure is zo ontworpen dat toepassingen miljoenen klanten tegelijkertijd kunnen hosten en er daarnaast een betrouwbare basis wordt geboden waarop bedrijven kunnen voldoen aan hun beveiligingsbehoeften. Bovendien biedt Azure u een scala aan configureerbare beveiligingsopties en de mogelijkheid om deze te beheren, zodat u de beveiliging kunt afstemmen op de unieke vereisten van uw implementaties. Dit document wordt helpt u aan deze vereisten voldoet.

### <a name="abstract"></a>Abstracte
Controle en logboekregistratie van gebeurtenissen die betrekking hebben op beveiliging en gerelateerde waarschuwingen, zijn belangrijke onderdelen van een effectieve strategie voor gegevensbescherming. Beveiligingslogboeken en rapporten bieden u een elektronisch record van verdachte activiteiten en helpen u de patronen die geprobeerde of geslaagde externe binnendringen van het netwerk, evenals de interne aanvallen aangeven mogelijk detecteren. U kunt controle gebruikersactiviteit regelgeving document controleren, voert u forensische analyse en meer. Waarschuwingen bevatten een onmiddellijke melding wanneer beveiligingsgebeurtenissen plaatsvinden.

Microsoft Azure-services en producten bieden u configureerbare beveiliging voor controle en logboekregistratie opties waarmee u hiaten in uw beveiligingsbeleid en de mechanismen en los deze lacunes ter voorkoming van inbreuk. Microsoft-services bieden een deel (en in sommige gevallen alle) van de volgende opties: gecentraliseerde bewaking, logboekregistratie en analyse-systemen om continue inzicht; tijdige waarschuwingen; en rapporten voor het beheren van de grote hoeveelheid gegevens die worden gegenereerd door apparaten en services.

Microsoft Azure-logboekgegevens kunnen worden geëxporteerd naar Security Incident en Event Management SIEM ()-systemen voor analyse en integreert met controle oplossingen van derden.

Dit technisch document bevat een inleiding voor het genereren, te verzamelen en analyseren van beveiligingslogboeken van services die worden gehost op Azure en kunt u beveiliging voor inzicht in uw Azure-implementaties. Het bereik van dit technisch document is beperkt tot toepassingen en services gebouwd en geïmplementeerd in Azure.

> [!Note]
> Bepaalde aanbevelingen in dit document kunnen leiden tot hogere-, netwerk- of compute-Resourcegebruik en de kosten van uw licentie of abonnement te verhogen.

## <a name="types-of-logs-in-azure"></a>Verschillende typen logboeken in Azure
Cloud-toepassingen zijn complexe met veel bewegende onderdelen. Logboeken kunt u gegevens om ervoor te zorgen dat uw toepassing up blijft en wordt uitgevoerd in een foutloze toestand bevindt. Ook kunt u potentiële problemen voorkomen of oplossen uit het verleden zijn. Bovendien kunt u logboekgegevens grondige om inzicht te krijgen over uw toepassing. Deze kennis kan u helpen bij het verbeteren van de prestaties van toepassingen of onderhoud of acties die anders worden handmatige interventie moeten automatiseren.

Azure produceert uitgebreide logboekregistratie in voor elke Azure-service. Deze logboeken worden gecategoriseerd door deze belangrijkste typen:
-   **Besturingselement/management-logboeken** geven inzicht in de Azure Resource Manager maken, bijwerken en DELETE-bewerkingen. [Azure activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) is een voorbeeld van dit type van het logboek.

-   **Gegevens vlak logboeken** geven inzicht in de gebeurtenissen die deze gebeurtenis treedt op als onderdeel van het gebruik van een Azure-resource. Voorbeelden van dit type van het logboek zijn de Windows-gebeurtenis systeem, beveiliging, en de toepassing wordt geregistreerd in een virtuele machine en de [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) geconfigureerd via de Azure-Monitor


-   **Verwerkte gebeurtenissen** geven informatie over geanalyseerde gebeurtenissen /-waarschuwingen die zijn verwerkt namens jou. Voorbeelden van dit type zijn [Azure Security Center Alerts](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) waar [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) heeft verwerkt en uw abonnement geanalyseerd en biedt beknopte beveiligingswaarschuwingen

De volgende tabel lijsttype belangrijkste logboeken beschikbaar in Azure.

| Logboek categorie | Logboektype | Sleutelgebruik | Integratie |
| ------------ | -------- | ------ | ----------- |
|[Activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|Besturingselement vlak gebeurtenissen op Azure Resource Manager-resources|   Bieden inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement.| Rest-API & [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|
|[Azure diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|regelmatig gegevens over de werking van Azure Resource Manager-resources in abonnement| Inzicht bieden in bewerkingen dat de bron zelf uitgevoerd| Azure Monitor [stroom](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|
|[AAD-rapportage](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)|Logboeken en rapporten|Gebruiker aanmelden activiteiten & systeem activiteit informatie over gebruikers- en groepsbeheer|[Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api-quickstart)|
|[Virtuele Machine & Cloudservices](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics-storage)|Windows-gebeurtenislogboek & Linux Syslog|    Systeemgegevens van het en logboekregistratie op de virtuele machines vastgelegd en die gegevens overgebracht naar een opslagaccount van uw keuze.|   Met behulp van Windows [af](https://docs.microsoft.com/azure/azure-diagnostics) (Windows Azure Diagnostics opslag)- en Linux in Azure monitor|
|[Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)|Logboekregistratie voor opslag en metrische gegevens voor een opslagaccount biedt|Biedt inzicht in de trace-aanvragen trends in gebruik analyseren en onderzoeken van problemen met uw opslagaccount.|    REST-API of de [clientbibliotheek](https://msdn.microsoft.com/library/azure/mt347887.aspx)|
|[NSG (Netwerkbeveiligingsgroep) stroom-Logboeken](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview)|JSON-indeling en ziet u per regel op basis van een binnenkomende en uitgaande stromen|Informatie weergeven over inkomende en uitgaande IP-verkeer via een Netwerkbeveiligingsgroep|[Netwerk-Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)|
|[Toepassing inzicht](https://docs.microsoft.com/azure/application-insights/app-insights-overview)|Logboeken, uitzonderingen en aangepaste diagnostische gegevens|    Application Performance (APM)-service voor webontwikkelaars op meerdere platforms.| REST-API [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)|
|Gegevens verwerken / beveiligingswaarschuwing| Azure Security Center-waarschuwing, OMS-waarschuwing| Informatie over beveiliging en waarschuwingen.|   REST-API's, JSON|

### <a name="activity-log"></a>Activiteitenlogboek
De [Azure Activity Log](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), biedt inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement. Het activiteitenlogboek heette vroeger 'Controlelogboeken' of 'Operationele Logs', omdat deze rapporten [besturingselement vlak gebeurtenissen](https://driftboatdave.com/2016/10/13/azure-auditing-options-for-your-custom-reporting-needs/) voor uw abonnementen. Met het activiteitenlogboek, kunt u bepalen de ' wat, wie, en wanneer ' voor een (PUT, POST, verwijderen schrijfbewerkingen) die zijn gemaakt op de resources in uw abonnement. U kunt ook de status van de bewerking en andere relevante eigenschappen begrijpen. Het activiteitenlogboek omvat geen leesbewerkingen (GET).

Hier verwijst verwijderen PUT, POST, naar alle het activiteitenlogboek schrijven-bewerkingen op de bronnen bevat. Bijvoorbeeld, kunt u de activiteitenlogboeken van de een fout gevonden bij het oplossen van of om te controleren hoe een gebruiker in uw organisatie een resource gewijzigd.

![Activiteitenlogboek](./media/azure-log-audit/azure-log-audit-fig1.png)


U kunt gebeurtenissen ophalen uit uw activiteitenlogboek met de Azure portal [CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli), PowerShell-cmdlets en [REST-API van Azure-Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough). Activiteitenlogboeken hebben Gegevensretentieperiode 19 dagen.

Integratiescenario 's
-   [Maak een e-mailadres of webhook waarschuwing die uit een Activity Log-gebeurtenis wordt geactiveerd.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-auditlog-to-webhook-email)

-   [Deze stream naar een Event Hub](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs) voor opname door een service van derden of aangepaste analytics-oplossing zoals Power BI.

-   Analyseren in Power BI met behulp van de [Power BI-inhoudspakket.](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/)

-   [Sla het op een Opslagaccount voor archivering of handmatige controle.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-activity-log) U kunt de retentietijd (in dagen) met behulp van logboek profielen opgeven.

-   Opvragen en weergeven in de Azure-portal.

-   Deze query's uitvoeren via PowerShell-Cmdlet, CLI of REST-API.

-   Exporteren van het activiteitenlogboek met logboek-profielen [Meld Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).

U kunt een opslagaccount of [event hub naamruimte](https://docs.microsoft.com/azure/event-hubs/event-hubs-resource-manager-namespace-event-hub-enable-archive) die zich niet in hetzelfde abonnement als het een importeerbereik logboek. De gebruiker die de instelling configureert moet beschikken over de juiste [RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) toegang tot beide abonnementen
### <a name="azure-diagnostic-logs"></a>Azure diagnostische logboeken
Azure logboeken met diagnostische gegevens worden gegenereerd door een resource die uitgebreide, regelmatig gegevens over de werking van de bron. De inhoud van deze logboeken varieert per resourcetype (bijvoorbeeld [Windows-gebeurtenislogboeken system](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events)één categorie van diagnostische logboeken zijn voor virtuele machines en [blob, table en queue logboeken](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) categorieën van diagnostische logboeken voor storage-accounts zijn) en verschillen van het activiteitenlogboek, waarmee u inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement.

![Azure diagnostische logboeken](./media/azure-log-audit/azure-log-audit-fig2.png)

Azure Diagnostics-logboeken bieden verschillende configuratieopties die, Azure portal, met behulp van PowerShell-opdrachtregelinterface (CLI) en REST-API.

**Integratiescenario 's**
-   Bewaar ze op een [Opslagaccount](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) voor inspectie controle of handmatig. U kunt de retentietijd (in dagen) met behulp van de diagnostische instellingen opgeven.

-   [Deze stream naar Event Hubs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs) voor opname door een service van derden of aangepaste analytics-oplossing zoals [Power BI.](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

-   Analyseer ze met [OMS-logboekanalyse.](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)

**Services, ondersteund schema voor diagnostische logboeken en ondersteunde logboek categorieën per resourcetype**


| Service | Schema & Docs | Resourcetype | Category |
| ------- | ------------- | ------------- | -------- |
|Load Balancer| [Log analytics voor Azure Load Balancer (Preview)](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log)|Microsoft.Network/loadBalancers|    LoadBalancerAlertEvent|
|||Microsoft.Network/loadBalancers| LoadBalancerProbeHealthStatus
|Netwerkbeveiligingsgroepen|[Logboekanalyses voor netwerkbeveiligingsgroepen (NSG's)](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|
|||Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|
|Toepassingsgateways|[Logboekregistratie van diagnostische gegevens voor de toepassingsgateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|
|||Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|
|||Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|
|Key Vault|[Logboekregistratie van Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-logging)|Microsoft.KeyVault/vaults|AuditEvent|
|Azure Search|[Inschakelen en gebruiken van Search Traffic Analytics](https://docs.microsoft.com/azure/search/search-traffic-analytics)|Microsoft.Search/searchServices|OperationLogs|
|Data Lake Store|[Toegang tot diagnoselogboeken voor Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-diagnostic-logs)|Microsoft.DataLakeStore/accounts|Controleren|
|Data Lake Analytics|[Diagnostische logboeken openen voor Azure Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-diagnostic-logs)|Microsoft.DataLakeAnalytics/accounts|Controleren|
|||Microsoft.DataLakeAnalytics/accounts|Aanvragen|
|||Microsoft.DataLakeStore/accounts|Aanvragen|
|Logic Apps|[Aangepast Logic Apps B2B-volgschema](https://docs.microsoft.com/azure/logic-apps/logic-apps-track-integration-account-custom-tracking-schema)|Microsoft.Logic/workflows|WorkflowRuntime|
|||Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|
|Azure Batch|[Diagnostische logboekregistratie van Azure Batch](https://docs.microsoft.com/azure/batch/batch-diagnostics)|Microsoft.Batch/batchAccounts|ServiceLog|
|Azure Automation|[Log analytics voor Azure Automation](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|Microsoft.Automation/automationAccounts|JobLogs|
|||Microsoft.Automation/automationAccounts|JobStreams|
|Event Hubs|[Diagnostische logboeken van Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-diagnostic-logs)|Microsoft.EventHub/namespaces|ArchiveLogs|
|||Microsoft.EventHub/namespaces|OperationalLogs|
|Stream Analytics|[Diagnostische logboeken van taak](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)|Microsoft.StreamAnalytics/streamingjobs|Uitvoering|
|||Microsoft.StreamAnalytics/streamingjobs|Ontwerpen|
|Service Bus|[Diagnostische logboeken van Azure Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-diagnostic-logs)|Microsoft.ServiceBus/namespaces|OperationalLogs|

### <a name="azure-active-directory-reporting"></a>Azure Active Directory-rapportage
Azure Active Directory (Azure AD) bevat beveiligings-, activiteits- en controlerapporten voor uw directory. De [Azure Active Directory-controlerapport](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) klanten helpt bij het identificeren van bevoegde acties die is opgetreden in Azure Active Directory. Bevoegdheden omvatten wijzigingen van de uitbreiding van bevoegdheden (bijvoorbeeld het maken of opnieuw instellen van wachtwoorden), veranderende beleidsconfiguraties (bijvoorbeeld wachtwoordbeleid) of wijzigingen in de directoryconfiguratie (bijvoorbeeld wijzigingen in de federation-domeininstellingen).

De rapporten bevatten de controlerecord voor de naam van de gebeurtenis, de actor die de actie, de doelresource van invloed op een door de wijziging, en de datum en tijd (in UTC) wordt uitgevoerd. Klanten kunnen ophalen van de lijst van controlegebeurtenissen voor Azure Active Directory via de [Azure-portal](https://portal.azure.com/), zoals beschreven in [uw logboeken Audit](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). Hierna volgt een lijst met de beschikbare rapporten:

| Beveiligingsrapporten | Activiteitsrapporten | Controlerapporten |
| :--------------- | :--------------- | :------------ |
|Aanmeldingen van onbekende bronnen| Toepassingsgebruik: samenvatting| Directorycontrolerapport|
|Aanmeldingen na meerdere mislukte pogingen|  Toepassingsgebruik: gedetailleerd||
|Aanmeldingen vanuit meerdere locaties|    Toepassingsdashboard||
|Aanmeldingen van IP-adressen met verdachte activiteit|   Fouten bij het inrichten van een account||
|Onregelmatige aanmeldingsactiviteiten|    Afzonderlijke gebruikersapparaten||
|Aanmeldingen vanaf mogelijk geïnfecteerde apparaten|   Afzonderlijke gebruikersactiviteiten||
|Gebruikers met afwijkende aanmeldingsactiviteiten| Rapport met groepsactiviteiten||
||Rapport voor de registratie van opnieuw ingestelde wachtwoorden||
||Activiteit voor wachtwoord opnieuw instellen|||

De gegevens van deze rapporten kunnen nuttig zijn voor uw toepassingen, zoals de SIEM-systemen, controle en hulpprogramma's voor bedrijfsinformatie. De API's van Azure AD Reporting bieden toegang tot de gegevens op programmeerniveau via een set op REST-gebaseerde API's. U kunt deze aanroepen [API's](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) van verschillende programmeertalen en hulpprogramma's.

Gebeurtenissen in de Azure AD-controlerapport worden bewaard gedurende 180 dagen.

> [!Note]
> Zie voor meer informatie over de bewaarperiode op rapporten [bewaarbeleid voor Azure Active Directory rapport.](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)

Voor klanten die de bij het opslaan van de controlegebeurtenissen voor langere bewaartermijn, de rapportage-API kan worden gebruikt voor het ophalen van regelmatig [controlegebeurtenissen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) in een afzonderlijke gegevensarchief.

### <a name="virtual-machine-logs-using-azure-diagnostics"></a>Virtuele Machine logboeken met behulp van Azure Diagnostics
[Azure Diagnostics](https://docs.microsoft.com/azure/azure-diagnostics) is de functie binnen Azure waarmee het verzamelen van diagnostische gegevens op een geïmplementeerde toepassing. U kunt de extensie voor diagnostische gegevens uit verschillende bronnen. Op dit moment ondersteund zijn [Azure Cloud Service-Web- en werkrollen](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me),

![Virtuele Machine logboeken met behulp van Azure Diagnostics](./media/azure-log-audit/azure-log-audit-fig3.png)

[Virtuele Machines in Azure](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/) met Microsoft Windows en [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

U diagnostische Azure kunt inschakelen op een virtuele machine met volgende:

-   Met Visual Studio, Zie [Gebruik Visual Studio te traceren Azure Virtual Machines](https://docs.microsoft.com/azure/vs-azure-tools-debug-cloud-services-virtual-machines)

-   [Instellen van Azure Diagnostics op een Azure virtuele Machine op afstand](https://docs.microsoft.com/azure/virtual-machines-dotnet-diagnostics)

-   [PowerShell gebruiken voor het instellen van diagnostische gegevens op Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

-   [Een Windows virtuele machine met de controle en diagnostische gegevens met Azure Resource Manager-sjabloon maken](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="storage-analytics"></a>Storage Analytics
[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) voert logboekregistratie en voorziet in metrische gegevens voor een opslagaccount. Deze gegevens kunt u gebruiken om aanvragen te traceren, gebruikstrends te analyseren en een diagnose uit te voeren voor problemen met uw opslagaccount. Logboekregistratie van Storage Analytics is beschikbaar voor de [services Blob, wachtrijen en tabellen.](https://docs.microsoft.com/azure/storage/storage-introduction) Storage Analytics wordt gedetailleerde informatie over geslaagde en mislukte aanvragen in een storage-service geregistreerd.

Deze informatie kan worden gebruikt voor het bewaken van afzonderlijke aanvragen en voor het vaststellen van problemen met de storage-service. Aanvragen worden geregistreerd op basis van best-effort. Logboekvermeldingen worden alleen gemaakt als er aanvragen voor het service-eindpunt zijn. Als u een opslagaccount heeft een activiteit in de Blob-eindpunt maar niet in de tabel of een wachtrij-eindpunten alleen logboeken met betrekking tot wordt bijvoorbeeld de Blob-service gemaakt.

Voor het gebruik van Storage Analytics, moet u deze inschakelen afzonderlijk voor elke service die u wilt bewaken. U kunt het inschakelen in de [Azure-portal](https://portal.azure.com/); voor meer informatie Zie [bewaken van een opslagaccount in de Azure-portal.](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) U kunt ook Opslaganalyse programmatisch via de REST-API of de clientbibliotheek inschakelen. De bewerking Service-eigenschappen instellen voor het inschakelen van Storage Analytics afzonderlijk voor elke service gebruiken.

De samengevoegde gegevens wordt opgeslagen in een bekende blob (voor logboekregistratie) en in een bekende tabellen (voor metrische gegevens) waartoe toegang kunnen worden verkregen met behulp van de Blob-service en de API's van de tabel-service.

Storage Analytics mag maximaal 20 TB van de hoeveelheid opgeslagen gegevens die onafhankelijk is van de totale limiet voor uw opslagaccount. Alle logboeken worden opgeslagen in [blok-blobs](https://docs.microsoft.com/azure/storage/storage-analytics) in een container met de naam $logs, die automatisch worden gemaakt wanneer Opslaganalyse is ingeschakeld voor een opslagaccount.

> [!Note]
> Zie voor meer informatie over facturering en bewaarbeleid voor gegevens [Storage Analytics en facturering.](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing)
>
> [!Note]
> Zie voor meer informatie over opslagaccountlimieten [Azure Storage Scalability and Performance Targets.](https://docs.microsoft.com/azure/storage/storage-scalability-targets)

De volgende soorten geverifieerde en anonieme aanvragen worden geregistreerd.



| Geverifieerd  | Anoniem|
| :------------- | :-------------|
| Geslaagde aanvragen | Geslaagde aanvragen |
|Mislukte aanvragen, met inbegrip van de time-outperiode, beperking, netwerk, autorisatie en andere fouten | -Aanvragen via een Shared Access Signature (SAS), met inbegrip van mislukte en geslaagde aanvragen |
| -Aanvragen via een Shared Access Signature (SAS), met inbegrip van mislukte en geslaagde aanvragen |Time-out fouten voor zowel client als server |
|   Aanvragen voor analytische gegevens |    GET-aanvragen is mislukt met foutcode 304 (niet gewijzigd) |
| Aanvragen door Storage Analytics zelf, zoals logboekbestanden worden gemaakt of verwijderd, worden niet geregistreerd. Een volledige lijst met gegevens in het logboek wordt beschreven in de [Storage Analytics geregistreerde bewerkingen en statusberichten](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) en [Storage Analytics logboekindeling](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) onderwerpen. | Alle andere mislukte anonieme aanvragen worden niet geregistreerd. Een volledige lijst met gegevens in het logboek wordt beschreven in de [Storage Analytics geregistreerde bewerkingen en statusberichten](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) en [Storage Analytics logboekindeling](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |

### <a name="azure-networking-logs"></a>Azure VPN-Logboeken
Netwerk logboekregistratie en controle in Azure is uitgebreid en bevat informatie over twee hoofdcategorieën:

-   [Netwerk-Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) -Scenario's gebaseerde netwerkbewaking is opgegeven met de functies in netwerk-Watcher. Deze service omvat pakketopname, volgende hop, IP-stroom controleren, groep beveiligingsweergave, NSG stroom Logboeken. Scenario niveau bewaking biedt een complete weergave van netwerkbronnen in tegenstelling tot afzonderlijke resource netwerkbewaking.

-   [Broncontrole](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-resource-level-monitoring) -niveau Broncontrole bestaat uit vier onderdelen, diagnostische logboeken, metrische gegevens, het oplossen van problemen en resourcestatus. Al deze functies zijn gebouwd op het niveau van de resource.

![Azure VPN-Logboeken](./media/azure-log-audit/azure-log-audit-fig4.png)

Netwerk-Watcher is een regionale service waarmee u kunt bewaken en onderzoeken van de voorwaarden op een netwerk scenario niveau in, en naar Azure. Netwerkcontrole en visualisatie hulpprogramma's beschikbaar met de netwerk-Watcher kunnen u begrijpen, diagnoses stellen en Verkrijg inzicht in uw netwerk in Azure.

**Logboekregistratie NSG Flow** -stroom in de logboeken voor Netwerkbeveiligingsgroepen kunt u de logboeken die betrekking hebben op het verkeer dat wordt toegestaan of geweigerd door de beveiligingsregels voor verbindingen in de groep opnemen. Deze stroom logboeken zijn geschreven in JSON-indeling en binnenkomende en uitgaande stromen weergeven op basis van een per regel, de NIC die de stroom van toepassing, 5-tuple informatie over de stroom (bron/het doel-IP, bron/het doel-poort, het Protocol) en als het verkeer is toegestaan of geweigerd.

### <a name="network-security-group-flow-logging"></a>Netwerkbeveiligingsgroep stroom logboekregistratie

[Netwerkbeveiligingsgroep stroom logboeken](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) zijn een functie van netwerk-Watcher waarmee u informatie bekijken over inkomende en uitgaande IP-verkeer via een Netwerkbeveiligingsgroep. Deze stroom logboeken zijn geschreven in JSON-indeling en binnenkomende en uitgaande stromen weergeven op basis van een per regel, de NIC die de stroom van toepassing, 5-tuple informatie over de stroom (bron/het doel-IP, bron/het doel-poort, het Protocol) en als het verkeer is toegestaan of geweigerd.

Stroom registreert Netwerkbeveiligingsgroepen doel, zijn ze niet dezelfde als de andere logboeken worden weergegeven. Stroom logboeken worden opgeslagen alleen binnen een opslagaccount.

Het bewaarbeleid dezelfde Denk op andere logboeken van toepassing op de logboeken van de stroom. Logboeken hebben een bewaarbeleid die kan worden ingesteld van 1 dag en 365 dagen. Als geen bewaarbeleid is ingesteld, worden de logboeken voor altijd bewaard.

**Diagnostische logboeken**

Periodieke en eigen initiatief gebeurtenissen zijn gemaakt door netwerkbronnen en storage-accounts, verzonden naar een Event Hub of Log Analytics aangemeld. Deze logboeken bieden inzicht in de status van een resource. Deze logboeken kunnen worden weergegeven in de hulpprogramma's, zoals Power BI en Log Analytics. Als u wilt weten hoe u logboeken met diagnostische gegevens bekijken, gaat u naar [logboekanalyse.](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics)

![Diagnostische logboeken](./media/azure-log-audit/azure-log-audit-fig5.png)

Diagnostische logboeken beschikbaar zijn voor [Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), Routes, en [Application Gateway.](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)

Netwerk-Watcher biedt dat een diagnostische logboeken weergeven. Deze weergave bevat alle netwerkresources die ondersteuning bieden voor diagnostische gegevens vastleggen. U kunt in deze weergave inschakelen en uitschakelen van netwerkresources snel en gemakkelijk.


Naast de voorgaande mogelijkheden voor logboekregistratie en heeft netwerk-Watcher momenteel de volgende mogelijkheden:
- [Topologie](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) -biedt een netwerk niveau weergave voor de verschillende verbindingskabels en koppelingen tussen netwerkbronnen in een resourcegroep.

- [Variabele pakketopname](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) -pakketgegevens naar en vanuit een virtuele machine worden vastgelegd. Geavanceerde filteropties en verfijnd besturingselementen zoals kunnen tijd instellen en de grootte van beperkingen bieden veelzijdigheid. De pakketgegevens kunnen worden opgeslagen in een blob-opslag of op de lokale schijf in de CAP-indeling.

-   [IP-stroom controleert of](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) -controleert of een pakket wordt toegestaan of geweigerd op basis van stroom informatie 5-tuple pakket parameters (doel-IP, bron-IP, doelpoort, bronpoort en Protocol). Als het pakket is geweigerd door een beveiligingsgroep, wordt de regel en de groep die het pakket geweigerd geretourneerd.

-   [Volgende hop](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) -bepaalt de volgende hop voor pakketten worden gerouteerd in de Azure-netwerk Fabric, zodat u voor het vaststellen van een gebruiker gedefinieerde routes onjuist geconfigureerd.

-   [Groep beveiligingsweergave](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) -opgehaald van de effectieve en toegepaste beveiligingsregels voor verbindingen die worden toegepast op een virtuele machine.

-   [Virtuele netwerkgateway en verbinding probleemoplossing](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) -biedt de mogelijkheid om op te lossen virtuele netwerkgateways en verbindingen.

-   [Netwerk-abonnementen](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-subscription-limits) -Hiermee kunt u gebruik van netwerkbronnen op basis van limieten weergeven.

### <a name="application-insight"></a>Toepassing inzicht

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) is een uitbreidbaar Management APM (Application Performance)-service voor webontwikkelaars op meerdere platforms. Hiermee kunt u uw livewebtoepassing controleren. Is het automatisch detecteren afwijkingen. De service bevat krachtige analysehulpmiddelen om u te helpen bij het vaststellen van problemen en te begrijpen wat gebruikers daadwerkelijk doen met uw app.

 Het is bedoeld om u te helpen de prestaties en bruikbaarheid continu te verbeteren.

 Het werkt voor apps op uiteenlopende platforms, waaronder .NET, Node.js en J2EE, on-premises of in de cloud gehost. Het kan worden geïntegreerd met uw devOps-proces en verwijst naar verschillende ontwikkelingsprogramma's heeft.

![Toepassing inzicht](./media/azure-log-audit/azure-log-audit-fig6.png)

Application Insights is bedoeld voor het ontwikkelingsteam en helpt u om te begrijpen hoe de app presteert en op welke manier de app wordt gebruikt. Met deze service kunt u het volgende controleren:

-   **Aantal aanvragen, reactietijden en foutpercentages** - ga na welke pagina's het populairst zijn op welke tijdstippen van de dag en waar uw gebruikers zich bevinden. Ontdek welke pagina's het beste presteren. Als uw reactietijden en foutpercentages omhoog gaan wanneer er meer aanvragen binnenkomen, hebt u mogelijk te weinig resources.

-   **Aantal afhankelijkheidsrelaties, reactietijden en foutpercentages** - controleer of externe services zorgen voor vertraging.

-   **Uitzonderingen** - analyseren van de samengevoegde statistieken of Kies specifieke exemplaren en inzoomen op de stack-trace en de verwante aanvragen. Zowel server- als browseruitzonderingen worden gerapporteerd.

-   **Paginaweergaven en de prestaties bij het laden van pagina’s** - deze gegevens worden gerapporteerd door de browsers van uw gebruikers.

-   **AJAX-aanroepen** van webpagina's - ga na wat het aantal aanroepen, de reactietijden en de foutpercentages zijn.

-   **Aantal gebruikers en sessies**.

-   **Prestatiemeteritems** van uw Windows- of Linux-servers, zoals die voor CPU-, geheugen- en netwerkgebruik.

-   **Diagnostische gegevens van hosts** van Docker of Azure.

-   **Diagnostische traceerlogboeken** van uw app - met behulp hiervan kunt u de samenhang vaststellen tussen traceergebeurtenissen en aanvragen.

-   **Aangepaste gebeurtenissen en functies voor het verzamelen van metrische gegevens** die u zelf schrijft in de client- of servercode - hiermee kunt u zakelijke gebeurtenissen bijhouden, zoals het aantal verkochte artikelen of gewonnen spellen.

**Lijst met integratiescenario's en -beschrijving:**

| Integratiescenario 's | Beschrijving |
| --------------------- | :---------- |
|[De toepassingstoewijzing vast](https://docs.microsoft.com/azure/application-insights/app-insights-app-map)|Bekijk de onderdelen van uw app met belangrijke metrische gegevens en waarschuwingen.||
|[Diagnostische gegevens doorzoeken bijvoorbeeld gegevens](https://docs.microsoft.com/azure/application-insights/app-insights-diagnostic-search)| U kunt zoeken naar gebeurtenissen, zoals aanvragen, uitzonderingen, afhankelijkheidsaanroepen, logboektraceringen en paginaweergaven en deze gegevens ook filteren.||
|[Metrics Explorer voor geaggregeerde gegevens](https://docs.microsoft.com/azure/application-insights/app-insights-metrics-explorer)|Verken, filter en segmenteer cumulatieve gegevens, zoals aantallen aanvragen, fouten en uitzonderingen, reactietijden en paginalaadtijden.||
|[Dashboards](https://docs.microsoft.com/azure/application-insights/app-insights-dashboards#dashboards)|Combineer gegevens van meerdere resources tot een mash-up en deel deze met anderen. Ideaal voor toepassingen met meerdere onderdelen en om continu weer te geven in de teamkamer.||
|[Metrische gegevens livestream](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream)|Wanneer u een nieuwe build implementeert, kunt u kijken naar deze 'near-realtime' prestatie-indicatoren om te controleren of dat alles naar verwachting werkt.||
|[Analytische gegevens](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)|Beantwoord moeilijke vragen over de prestaties en het gebruik van uw app met behulp van deze krachtige querytaal.||
|[Automatische en handmatige waarschuwingen](https://docs.microsoft.com/azure/application-insights/app-insights-alerts)|De functie die automatische waarschuwingen genereert, kan normale telemetriepatronen van uw app herkennen en wordt geactiveerd wanneer er zich iets voordoet dat buiten het normale patroon valt. U kunt ook waarschuwingen instellen voor bepaalde niveaus van aangepaste functies of standaardfuncties voor het verzamelen van metrische gegevens.||
|[Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)|Bekijk prestatiegegevens in de code. Ga naar de code vanuit stack-traces.||
|[Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi)|Integreer metrische gegevens over het gebruik van de toepassing met andere business intelligence.||
|[REST API](https://dev.applicationinsights.io/)|Schrijf code voor het uitvoeren van query's op uw functies voor het verzamelen van metrische gegevens en op onbewerkte gegevens.||
|[Continue export](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry)|Het exporteren van de onbewerkte gegevens naar opslag bulksgewijs wanneer het wordt geleverd.||

### <a name="azure-security-center-alerts"></a>Azure Security Center-waarschuwingen
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) automatisch verzamelt, analyseert en integreert logboekgegevens van uw Azure-resources, het netwerk- en verbonden partneroplossingen, zoals een firewall en endpoint protection-oplossingen, om werkelijke dreigingen te detecteren en fout-positieven te verminderen. In Security Center wordt een lijst met beveiligingswaarschuwingen met prioriteiten weergegeven samen met de informatie die u nodig hebt om snel onderzoek te doen naar het probleem en aanbevelingen voor het herstellen van een aanval.

Het detecteren van bedreigingen van Security Center werkt volgens het automatisch verzamelen van beveiligingsgegevens van uw Azure-resources, het netwerk en verbonden partneroplossingen. Deze informatie wordt door Security Center geanalyseerd, waarbij vaak informatie uit meerdere bronnen wordt samengebracht om bedreigingen te analyseren. Beveiligingswaarschuwingen krijgen in Security Center een prioriteit, evenals aanbevelingen voor het oplossen van de bedreiging.

![Azure Security Center](./media/azure-log-audit/azure-log-audit-fig7.png)

Security Center maakt gebruik van geavanceerde beveiligingsanalyses die veel verder gaan dan op handtekeningen gebaseerde benaderingen. Doorbraken in grote hoeveelheden gegevens en [machine learning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) technologieën worden toegepast om te evalueren gebeurtenissen over de hele cloudfabric – bedreigingen die niet mogelijk om u te identificeren met behulp van handmatige benaderingen en het voorspellen van de evolutie van aanvallen te detecteren. Deze beveiligingsanalyses omvatten:

-   **Geïntegreerd dreigingen:** bekende ongewenste actoren zoekt door toe te passen wereldwijde dreigingen van Microsoft-producten en services, de Microsoft Digital Crimes Unit (DCU), de Microsoft Security Response Center (MSRC) en externe feeds.

-   **Gedragsanalyse:** bekende patronen voor het detecteren van schadelijke gedrag van toepassing is.

-   **Afwijkingsdetectie:** gebruikt statistische samenstellen van profiel voor het maken van een historische basislijn. Er wordt een waarschuwing gegeven bij afwijkingen van vastgestelde basislijnen die aan een mogelijke aanvalsvector voldoen.


Veel beveiligingsbewerkingen en respons op incidenten teams afhankelijk van een oplossing Security Information en Event Management (SIEM) als uitgangspunt voor gesorteerd en beveiligingswaarschuwingen onderzoeken. Klanten kunnen met Azure-logboekanalyse-integratie synchroniseren Security Center-waarschuwingen en beveiligingsgebeurtenissen voor virtuele machine, die zijn verzameld door Azure Diagnostics- en Azure controlelogboeken met hun logboekanalyse of SIEM-oplossing in bijna realtime.


## <a name="log-analytics"></a>Log Analytics

Log Analytics is een service in [Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) die helpt u bij het verzamelen en analyseren van gegevens die zijn gegenereerd voor resources in uw cloud en on-premises omgevingen. Dit biedt u realtime-inzichten geïntegreerd zoeken en aangepaste dashboards met miljoenen records gemakkelijk te analyseren voor alle werkbelastingen en servers, ongeacht de fysieke locatie.

![Log Analytics](./media/azure-log-audit/azure-log-audit-fig8.png)

Op de center Log Analytics is de OMS-opslagplaats, die wordt gehost in de Azure-cloud. Gegevens worden vanuit verbonden bronnen verzameld in de opslagplaats door gegevensbronnen te configureren en oplossingen toe te voegen aan uw abonnement. Gegevensbronnen en oplossingen creëren elk verschillende recordtypen die hun eigen set eigenschappen hebben, maar nog steeds samen kunnen worden geanalyseerd in query's in de opslagplaats. Hiermee kunt u dezelfde hulpprogramma's en methoden gebruiken om te werken met verschillende soorten gegevens die door verschillende bronnen zijn verzameld.

Verbonden bronnen zijn de computers en andere bronnen die gegevens genereren die worden verzameld door Log Analytics. Het kan hierbij gaan agents zijn geïnstalleerd op [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) en [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) computers die rechtstreeks verbinding te maken of agents in [een verbonden beheergroep van System Center Operations Manager.](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents) Log Analytics kunnen ook worden verzameld van [Azure-opslag.](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage)

[Gegevensbronnen](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources) zijn de verschillende soorten gegevens die uit elke verbonden bron worden verzameld. Dit omvat gebeurtenissen en [prestatiegegevens](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters) van [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) en Linux-agents naast bronnen zoals [IIS-logboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs), en [aangepaste tekstlogboeken.](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-custom-logs) U configureert elke gegevensbron die u wenst te verzamelen en de configuratie wordt automatisch doorgegeven aan elke verbonden bron.

Er zijn vier verschillende manieren van [verzamelen van Logboeken en metrische gegevens voor Azure-services:](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage)
1.  Azure diagnostics direct met logboekanalyse (diagnostische gegevens in de volgende tabel)

2.  Azure diagnostics naar Azure-opslag met logboekanalyse (opslag in de volgende tabel)

3.  Connectors voor Azure-services (Connectors in de volgende tabel)

4.  Scripts voor het verzamelen en vervolgens postgegevens in logboekanalyse (lege cellen in de volgende tabel en voor services die niet worden weergegeven)

| Service | Resourcetype | Logboeken | Metrische gegevens | Oplossing |
| :------ | :------------ | :--- | :------ | :------- |
|Toepassingsgateways|  Microsoft.Network/<br>applicationGateways|  Diagnostiek|Diagnostiek|    [Azure-toepassing](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics) [Analytics Gateway](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics)|
|Application insights||     Connector|  Connector|  [Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) [Connector (evaluatie)](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)|
|Automation-accounts|   Microsoft.Automation/<br>AutomationAccounts|    Diagnostiek||       [Meer informatie](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|
|Batch-accounts|    Microsoft.Batch/<br>batchAccounts|  Diagnostiek|    Diagnostiek||
|Klassieke cloudservices||       Storage||       [Meer informatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage-iis-table)|
|Cognitive Services|    Microsoft.CognitiveServices/<br>accounts|       Diagnostiek|||
|Data Lake analytics|   Microsoft.DataLakeAnalytics/<br>accounts|   Diagnostiek|||
|Data Lake store|   Microsoft.DataLakeStore/<br>accounts|   Diagnostiek|||
|Event Hub-naamruimte|   Microsoft.EventHub/<br>Naamruimten|  Diagnostiek|    Diagnostiek||
|IoT Hubs|  Microsoft.Devices/<br>IotHubs||     Diagnostiek||
|Key Vault| Microsoft.KeyVault/<br>Kluizen|  Diagnostiek  || [KeyVault Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-key-vault)|
|Load Balancers|    Microsoft.Network/<br>loadBalancers|    Diagnostiek|||
|Logic Apps|    Microsoft.Logic/<br>Werkstromen|  Diagnostiek|    Diagnostiek||
||Microsoft.Logic/<br>integrationAccounts||||
|Netwerkbeveiligingsgroepen|   Microsoft.Network/<br>networksecuritygroups|Diagnostiek||   [Netwerkbeveiligingsgroep Azure Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-network-security-group-analytics-solution-in-log-analytics)|
|Recovery kluizen|   Microsoft.RecoveryServices/<br>Kluizen|||[Azure Recovery Services-Analytics (Preview)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
|Services zoeken|   Microsoft.Search/<br>searchServices|    Diagnostiek|    Diagnostiek||
|Service Bus-naamruimte| Microsoft.ServiceBus/<br>Naamruimten|    Diagnostiek|Diagnostiek|    [Service Bus Analytics (Preview)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
|Service Fabric||       Storage||    [Service Fabric Analytics (Preview)](https://docs.microsoft.com/azure/log-analytics/log-analytics-service-fabric)|
|SQL (v12)| Microsoft.Sql/<br>servers /<br>databases||       Diagnostiek||
||Microsoft.Sql/<br>servers /<br>elasticPools||||
|Storage|||         Script| [Azure Storage Analytics (Preview)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution)|
|Virtuele machines|  Microsoft.Compute/<br>Virtuele machines|  Toestelnummer|  Toestelnummer||
||||Diagnostiek||
|Virtuele Machines-schaalsets|   Microsoft.Compute/<br>Virtuele machines    ||Diagnostiek||
||Microsoft.Compute/<br>virtualMachineScaleSets /<br>Virtuele machines||||
|Webserver-farms|Microsoft.Web/<br>serverfarms||   Diagnostiek
|Web Sites| Microsoft.Web/<br>sites ||      Diagnostiek|    [Meer informatie](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring)|
||Microsoft.Web/<br>sites /<br>sleuven|||||


## <a name="log-integration-with-on-premises-siem-systems"></a>Logboek-integratie met on-premises SIEM-systemen
[Integratie van Azure-logboekanalyse](https://www.microsoft.com/download/details.aspx?id=53324) kunt u voor het integreren van onbewerkte logboeken van uw Azure-resources in uw on-premises **Security Information en Event Management SIEM ()-systemen**.

![Logboek-integratie](./media/azure-log-audit/azure-log-audit-fig9.png)

Integratie van Azure-logboekanalyse verzamelt diagnostische Azure-gegevens van uw Windows (af) virtuele machines, activiteitenlogboeken Azure, Azure Security Center-waarschuwingen en Azure Resource Provider registreert. Deze integratie biedt een uniforme dashboard voor alle activa, lokaal of in de cloud zodat aggregeren, correleren, analyseren en voor beveiligingsgebeurtenissen waarschuwen.



Azure-logboekanalyse-integratie ondersteunt momenteel integratie van Azure activiteitenlogboeken, Windows-gebeurtenislogboek van Windows virtuele machine in uw Azure-abonnement, de auditlogboeken van Azure Security Center waarschuwingen, Azure diagnostische logboeken en Azure Active Directory.

| Logboektype | Log analytics ondersteuning van JSON (Splunk, ArcSight, Qradar) |
| :------- | :-------------------------------------------------------- |
|AAD-auditlogboeken|    ja|
|Activiteitenlogboeken| Ja|
|ASC waarschuwingen |Ja|
|Diagnostische logboeken (resource Logboeken)|  Ja|
|VM-Logboeken|   Ja via doorgestuurde gebeurtenissen en niet via JSON|


De volgende tabel beschrijft de logboek-categorie en de details van de SIEM-integratie.

[Aan de slag met Azure-logboekanalyse integratie](https://docs.microsoft.com/azure/security/security-azure-log-integration-get-started) - zelfstudie leert u de installatie van de integratie van Azure-logboekanalyse en integratie van Logboeken vanuit Azure af opslag activiteitenlogboeken Azure, Azure Security Center-waarschuwingen en Azure Active Directory controlelogboeken.

Integratiescenario 's

-   [Partner configuratiestappen](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – dit blogbericht ziet u de integratie van Azure-logboekanalyse werken met partneroplossingen Splunk, HP ArcSight en IBM QRadar configureren.

-   [Azure-logboekanalyse integratie Veelgestelde vragen (FAQ)](https://docs.microsoft.com/azure/security/security-azure-log-integration-faq) -deze Veelgestelde vragen over de antwoorden op vragen over Azure-logboekanalyse-integratie.

-   [Waarschuwingen met Azure Security Center integreren Meld integratie](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration) : dit document ziet u hoe Security Center-waarschuwingen, samen met de virtuele machine beveiligingsgebeurtenissen die zijn verzameld door Azure Diagnostics- en Azure controlelogboeken met uw SIEM-oplossingen of logboekanalyse synchroniseren.

## <a name="next-steps"></a>Volgende stappen

- [Controle en logboekregistratie](https://www.microsoft.com/trustcenter/security/auditingandlogging)

Gegevens beveiligen door het onderhouden van de zichtbaarheid en snel reageren op tijdige beveiligingswaarschuwingen

- [Opties voor logboekregistratie en Audit Collection Services-logboek in Azure](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/)

Welke instellingen u afdwingen wilt, zodat u uw Azure-exemplaren de juiste beveiliging verzamelt en de auditlogboeken.

- [Controle-instellingen voor een siteverzameling configureren](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US)

Als beheerder van een site een de geschiedenis van acties die door een bepaalde gebruiker kunt ophalen en kunt ook de geschiedenis van acties die worden uitgevoerd tijdens een bepaald datumbereik ophalen. 

- [Het controlelogboek zoeken in de Office 365-beveiliging en naleving Center](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US)

Een kan het Office 365-beveiliging & naleving Center gebruiken voor het zoeken van het geïntegreerde controlelogboek om weer te geven van gebruikers- en activiteiten in uw organisatie Office 365.


