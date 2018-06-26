---
title: Azure-logboekregistratie en controle | Microsoft Docs
description: Meer informatie over hoe u logboekgegevens grondige om inzicht te krijgen over uw toepassing kunt gebruiken.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: e4144ca0d87abda3d9f8de47e56af59d0e4af312
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938363"
---
# <a name="azure-logging-and-auditing"></a>Logboekregistratie en bewaking in Azure

Azure biedt een breed scala aan configureerbare beveiliging voor controle en logboekregistratie opties waarmee u hiaten in uw beveiligingsbeleid en -mechanismen. Dit artikel wordt gegenereerd, verzamelen en analyseren van beveiligingslogboeken van services die worden gehost op Azure.

> [!Note]
> Bepaalde aanbevelingen in dit artikel kunnen leiden tot grotere hoeveelheden gegevens, netwerk of compute-Resourcegebruik en uw licentie of abonnement kosten verhoogt.

## <a name="types-of-logs-in-azure"></a>Verschillende typen logboeken in Azure
Cloud-toepassingen zijn complexe met veel bewegende onderdelen. Logboeken kunt u gegevens om te helpen uw toepassingen up-to-date te houden. Logboeken te lossen voorbij problemen of potentiële die voorkomen. En ze kunnen helpen verbeteren de prestaties van toepassingen of onderhoud of acties die anders worden handmatige interventie moeten automatiseren.

Azure logboeken worden onderverdeeld in de volgende typen:
* **Besturingselement/management-logboeken** bevatten informatie over het maken van Azure Resource Manager, UPDATE en DELETE-bewerkingen. Zie voor meer informatie [Azure activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).

* **Gegevens vlak logboeken** bevatten informatie over deze gebeurtenis treedt op als onderdeel Azure Resourcegebruik gebeurtenissen. Voorbeelden van dit type van het logboek zijn de Windows-gebeurtenis-systeem, beveiliging, en de toepassing wordt geregistreerd in een virtuele machine (VM) en de [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) die worden geconfigureerd via de Azure-Monitor.

* **Verwerkte gebeurtenissen** bevatten informatie over geanalyseerde gebeurtenissen /-waarschuwingen die zijn verwerkt namens jou. Voorbeelden van dit type zijn [Azure Security Center waarschuwingen](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) waar [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) heeft verwerkt en uw abonnement geanalyseerd en beknopte beveiligingswaarschuwingen biedt.

De volgende tabel bevat de belangrijkste typen logboeken beschikbaar in Azure:

| Logboek categorie | Logboektype | Gebruik | Integratie |
| ------------ | -------- | ------ | ----------- |
|[Activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|Besturingselement vlak gebeurtenissen op Azure Resource Manager-resources|   Biedt inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement.|    Rest-API, [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|
|[Diagnostische logboeken van Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|Regelmatig gegevens over de werking van Azure Resource Manager-resources in abonnement|    Biedt inzicht in de bewerkingen die de bron zelf wordt uitgevoerd.| Azure Monitor, [Stream](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|
|[Azure AD-rapportage](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)|Logboeken en rapporten | Rapporten aanmelden gebruikersactiviteiten en het systeem activiteit informatie over gebruikers- en groepsbeheer.|[Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api-quickstart)|
|[Virtuele machines en cloudservices](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-collect-azurevm)|Windows Event Log-service- en Linux Syslog|    Systeemgegevens van het en logboekregistratie op de virtuele machines vastgelegd en die gegevens overgebracht naar een opslagaccount van uw keuze.|   Windows (met behulp van Windows Azure Diagnostics [[af](https://docs.microsoft.com/azure/azure-diagnostics)] opslag)- en Linux in de Azure-Monitor|
|[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)|Logboekregistratie van opslag, biedt metrische gegevens voor een opslagaccount|Biedt inzicht in de trace-aanvragen, analyseert trends in gebruik en opsporen van problemen met uw opslagaccount.|   REST-API of de [clientbibliotheek](https://msdn.microsoft.com/library/azure/mt347887.aspx)|
|[Netwerkbeveiligingsgroep (NSG) stroom Logboeken](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview)|Ziet u binnenkomende en uitgaande stromen per regel op basis van een JSON-indeling.|Geeft informatie weer over de inkomende en uitgaande IP-verkeer via een Netwerkbeveiligingsgroep.|[Azure-netwerk-Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)|
|[Toepassing inzicht](https://docs.microsoft.com/azure/application-insights/app-insights-overview)|Logboeken, uitzonderingen en aangepaste diagnostische gegevens|   Biedt een application performance monitoring (APM)-service voor webontwikkelaars op meerdere platforms.| REST-API [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)|
|Gegevens verwerken / beveiligingswaarschuwingen|    Azure Security Center waarschuwingen, meldingen van de Azure Log Analytics|   Biedt informatie over beveiliging en waarschuwingen.|  REST-API's, JSON|

### <a name="activity-logs"></a>Activiteitenlogboeken
[Azure activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement. Activiteitenlogboeken werden voorheen bekend als 'controlelogboeken' of 'operationele logs', omdat ze rapporteren [besturingselement vlak gebeurtenissen](https://driftboatdave.com/2016/10/13/azure-auditing-options-for-your-custom-reporting-needs/) voor uw abonnementen. 

Activiteit logboeken help u bepalen de ' wat, wie, en wanneer ' voor schrijfbewerkingen (dat wil zeggen, PUT, POST of verwijderen). Activiteit registreert ook de status van de bewerking en andere relevante eigenschappen u laten zien. Activiteitenlogboeken bevatten geen leesbewerkingen (GET).

In dit artikel verwijzen PUT, POST en DELETE naar alle schrijfbewerkingen die een activiteitenlogboek op de bronnen bevat. Bijvoorbeeld, kunt u de activiteitenlogboeken van de een fout gevonden bij het oplossen van problemen bent of om te controleren hoe een gebruiker in uw organisatie een resource gewijzigd.

![Diagram van activiteit-logboek](./media/azure-log-audit/azure-log-audit-fig1.png)

U kunt gebeurtenissen ophalen uit een gebeurtenislogboek met behulp van de Azure-portal [Azure CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli), PowerShell-cmdlets en [REST-API van Azure-Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough). Activiteitenlogboeken hebben 19 dag Gegevensretentie periode.

Integratiescenario's voor een activiteit van het gebeurtenislogboek:

* [Maken van een e-mailadres of webhook waarschuwing die wordt geactiveerd door een activiteit van het gebeurtenislogboek](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-auditlog-to-webhook-email).

* [Deze stream naar een event hub](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs) voor opname door een service van derden of aangepaste analytics-oplossing zoals Power BI.

* Analyseren in Power BI met behulp van de [Power BI-inhoudspakket](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).

* [Sla het op een opslagaccount voor archivering of handmatig inspectie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-activity-log). Met behulp van logboek-profielen kunt u de bewaartijd (in dagen).

* Opvragen en weergeven in de Azure-portal.

* Deze query's uitvoeren via PowerShell-cmdlet, Azure CLI of REST-API.

* Exporteren van het activiteitenlogboek met logboek-profielen [logboekanalyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).

U kunt een opslagaccount of [event hub naamruimte](https://docs.microsoft.com/azure/event-hubs/event-hubs-resource-manager-namespace-event-hub-enable-archive) die zich niet in hetzelfde abonnement als degene die het logboek wordt tekensetcodering. Iedereen die Hiermee configureert u de instelling moet beschikken over het juiste [op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) toegang tot beide abonnementen.

### <a name="azure-diagnostics-logs"></a>Azure Diagnostics-logboeken
Azure diagnostics logboeken worden gegenereerd door een resource die uitgebreide, regelmatig gegevens over de werking van de bron biedt. De inhoud van deze logboeken varieert per resourcetype. Bijvoorbeeld, [Windows-gebeurtenislogboeken system](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) een categorie van diagnostische logboeken zijn voor VM's en [blob, table en queue logboeken](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) zijn categorieën van diagnostische logboeken voor storage-accounts. Diagnostische logboeken verschillen van de activiteitenlogboeken bieden inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement.

![Azure diagnostics logboeken diagrammen](./media/azure-log-audit/azure-log-audit-fig2.png)

Logboeken met diagnostische gegevens van Azure bieden meerdere configuratieopties, zoals de Azure-portal, PowerShell, Azure CLI en de REST-API.

**Integratiescenario 's**

* Bewaar ze op een [opslagaccount](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) voor inspectie controle of handmatig. Met behulp van de diagnostische instellingen kunt u de bewaartijd (in dagen).

* [Deze stream naar event hubs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs) voor opname door een service van derden of aangepaste analytics-oplossing, zoals [PowerBI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/).

* Analyseer ze met [logboekanalyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).

**Ondersteunde services, schema voor diagnostische logboeken en ondersteunde logboek categorieën per resourcetype**


| Service | Schema- en documentatie | Resourcetype | Categorie |
| ------- | ------------- | ------------- | -------- |
|Azure Load Balancer| [Log Analytics voor Load Balancer (Preview)](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log)|Microsoft.Network/loadBalancers<br>Microsoft.Network/loadBalancers| LoadBalancerAlertEvent<br>LoadBalancerProbeHealthStatus|
|Netwerkbeveiligingsgroepen|[Log Analytics voor Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)|Microsoft.Network/networksecuritygroups<br>Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent<br>NetworkSecurityGroupRuleCounter|
|Azure Application Gateway|[Diagnostische logboekregistratie voor Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)|Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog<br>ApplicationGatewayPerformanceLog<br>ApplicationGatewayFirewallLog|
|Azure Key Vault|[Sleutelkluis-Logboeken](https://docs.microsoft.com/azure/key-vault/key-vault-logging)|Microsoft.KeyVault/vaults|AuditEvent|
|Azure Search|[Inschakelen en gebruiken van Search Traffic Analytics](https://docs.microsoft.com/azure/search/search-traffic-analytics)|Microsoft.Search/searchServices|OperationLogs|
|Azure Data Lake Store|[Diagnostische logboeken voor Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-diagnostic-logs)|Microsoft.DataLakeStore/accounts<br>Microsoft.DataLakeStore/accounts|Controle<br>Aanvragen|
|Azure Data Lake Analytics|[Diagnostische gegevens toegangslogboeken voor Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-diagnostic-logs)|Microsoft.DataLakeAnalytics/accounts<br>Microsoft.DataLakeAnalytics/accounts|Controle<br>Aanvragen|
|Azure Logic Apps|[Aangepast Logic Apps B2B-volgschema](https://docs.microsoft.com/azure/logic-apps/logic-apps-track-integration-account-custom-tracking-schema)|Microsoft.Logic/workflows<br>Microsoft.Logic/integrationAccounts|WorkflowRuntime<br>IntegrationAccountTrackingEvents|
|Azure Batch|[Azure Batch diagnostische logboeken](https://docs.microsoft.com/azure/batch/batch-diagnostics)|Microsoft.Batch/batchAccounts|ServiceLog|
|Azure Automation|[Log Analytics voor Azure Automation](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|Microsoft.Automation/automationAccounts<br>Microsoft.Automation/automationAccounts|JobLogs<br>JobStreams|
|Azure Event Hubs|[Event Hubs diagnostische logboeken](https://docs.microsoft.com/azure/event-hubs/event-hubs-diagnostic-logs)|Microsoft.EventHub/namespaces<br>Microsoft.EventHub/namespaces|ArchiveLogs<br>OperationalLogs|
|Azure Stream Analytics|[Taak diagnostische logboeken](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)|Microsoft.StreamAnalytics/streamingjobs<br>Microsoft.StreamAnalytics/streamingjobs|Uitvoering<br>Ontwerpen|
|Service Bus van Azure|[Service Bus diagnostische logboeken](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-diagnostic-logs)|Microsoft.ServiceBus/namespaces|OperationalLogs|

### <a name="azure-active-directory-reporting"></a>Azure Active Directory-rapportage
Azure Active Directory (Azure AD) bevat beveiligings-, activiteit en controlerapporten voor de map van een gebruiker. De [Azure AD-controlerapport](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) kunt u bepalen welke bevoorrechte stappen dat is opgetreden in de Azure AD-exemplaar van de gebruiker. Bevoegdheden omvatten wijzigingen van de uitbreiding van bevoegdheden (bijvoorbeeld het maken of opnieuw instellen van wachtwoorden), veranderende beleidsconfiguraties (bijvoorbeeld wachtwoordbeleid) of wijzigingen in de directoryconfiguratie (bijvoorbeeld wijzigingen in de federation-domeininstellingen).

De rapporten bevatten de controlerecord voor de naam van de gebeurtenis, de gebruiker die de actie, de doelresource van invloed op een door de wijziging, en de datum en tijd (in UTC) wordt uitgevoerd. Gebruikers kunnen de lijst van controlegebeurtenissen ophalen voor Azure AD via de [Azure-portal](https://portal.azure.com/), zoals beschreven in [uw controlelogboeken weergeven](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

De opgenomen rapporten worden vermeld in de volgende tabel:

| Beveiligingsrapporten | Activiteitsrapporten | Controlerapporten |
| :--------------- | :--------------- | :------------ |
|Aanmeldingen van onbekende bronnen| Toepassingsgebruik: samenvatting| Directorycontrolerapport|
|Aanmeldingen na meerdere mislukte pogingen|  Toepassingsgebruik: gedetailleerd||
|Aanmeldingen vanuit meerdere locaties|    Toepassingsdashboard||
|Aanmeldingen vanaf IP-adressen met verdachte activiteiten|   Fouten bij het inrichten van een account||
|Onregelmatige aanmeldingsactiviteiten|    Afzonderlijke gebruikersapparaten||
|Aanmeldingen vanaf mogelijk geïnfecteerde apparaten|   Afzonderlijke gebruikersactiviteiten||
|Gebruikers met afwijkende aanmeldingsactiviteiten| Rapport met groepsactiviteiten||
||Activiteitenrapport voor registratie voor wachtwoordherstel||
||Activiteit voor wachtwoord opnieuw instellen|||

De gegevens in deze rapporten kunnen nuttig zijn voor uw toepassingen, zoals Security Information en Event Management SIEM ()-systemen, controle en hulpprogramma's voor bedrijfsinformatie. De API's van Azure AD Reporting bieden toegang tot de gegevens op programmeerniveau via een set op REST-gebaseerde API's. U kunt deze aanroepen [API's](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) van verschillende programmeertalen en hulpprogramma's.

Gebeurtenissen in de Azure AD-controlerapport worden bewaard gedurende 180 dagen.

> [!Note]
> Zie voor meer informatie over het bewaren van rapport [bewaarbeleid voor Azure AD-rapport](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention).

Als u geïnteresseerd bent in uw controlegebeurtenissen langer bewaren, gebruikt u de rapportage-API voor het ophalen van regelmatig [controlegebeurtenissen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) in een afzonderlijke gegevensarchief.

### <a name="virtual-machine-logs-that-use-azure-diagnostics"></a>Virtuele machine-logboeken die gebruikmaken van Azure Diagnostics
[Azure Diagnostics](https://docs.microsoft.com/azure/azure-diagnostics) is de functie binnen Azure waarmee het verzamelen van diagnostische gegevens op een geïmplementeerde toepassing. U kunt de extensie voor diagnostische gegevens uit diverse bronnen gebruiken. Op dit moment ondersteund zijn [Azure cloud service-web- en werkrollen rollen](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me).

![Virtuele machine-logboeken die gebruikmaken van Azure Diagnostics](./media/azure-log-audit/azure-log-audit-fig3.png)

### <a name="azure-virtual-machineshttpsazuremicrosoftcomdocumentationlearning-pathsvirtual-machines-that-are-running-microsoft-windows-and-service-fabrichttpsdocsmicrosoftcomazureservice-fabricservice-fabric-overview"></a>[Virtuele machines in Azure](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/) die Microsoft Windows worden uitgevoerd en [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview)

U kunt Azure Diagnostics op een virtuele machine inschakelen door een van de volgende handelingen uit:

* [Visual Studio gebruiken om te traceren, virtuele machines in Azure](https://docs.microsoft.com/azure/vs-azure-tools-debug-cloud-services-virtual-machines)

* [Instellen van Azure Diagnostics op afstand op een virtuele machine van Azure](https://docs.microsoft.com/azure/virtual-machines-dotnet-diagnostics)

* [PowerShell gebruiken voor het instellen van diagnostische gegevens op virtuele machines in Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

* [Een virtuele Windows-machine maken met controle en diagnostische gegevens met behulp van een Azure Resource Manager-sjabloon](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="storage-analytics"></a>Storage Analytics
[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) registreert en metrische gegevens voor een opslagaccount biedt. Deze gegevens kunt u gebruiken om aanvragen te traceren, gebruikstrends te analyseren en een diagnose uit te voeren voor problemen met uw opslagaccount. Logboekregistratie van Storage Analytics is beschikbaar voor de [Azure Blob-, Azure Queue- en Azure Table storage-services](https://docs.microsoft.com/azure/storage/storage-introduction). Storage Analytics wordt gedetailleerde informatie over geslaagde en mislukte aanvragen in een storage-service geregistreerd.

U kunt deze informatie gebruiken voor het bewaken van afzonderlijke aanvragen en voor het vaststellen van problemen met de storage-service. Aanvragen worden geregistreerd op basis van best-effort. Logboekvermeldingen worden alleen gemaakt als er aanvragen voor het service-eindpunt zijn. Bijvoorbeeld, als een opslagaccount activiteit in het blobeindpunt maar niet in de tabel of een wachtrij eindpunten heeft, worden alleen de logboeken die betrekking op de Blob storage-service hebben gemaakt.

Voor het gebruik van Storage Analytics schakelen afzonderlijk voor elke service die u wilt bewaken. U kunt het inschakelen in de [Azure-portal](https://portal.azure.com/). Zie voor meer informatie [een opslagaccount in de Azure-portal bewaken](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). U kunt ook Opslaganalyse programmatisch via de REST-API of de clientbibliotheek inschakelen. De bewerking Service-eigenschappen instellen voor het inschakelen van Storage Analytics afzonderlijk voor elke service gebruiken.

De samengevoegde gegevens wordt opgeslagen in een bekende blob (voor logboekregistratie) en in een bekende tabellen (voor metrische gegevens), die u openen kunt met behulp van de Blob storage-service en de Table storage-service API's.

Storage Analytics mag maximaal 20-terabyte (TB) van de hoeveelheid opgeslagen gegevens die onafhankelijk is van de totale limiet voor uw opslagaccount. Alle logboeken worden opgeslagen in [blok-blobs](https://docs.microsoft.com/azure/storage/storage-analytics) in een container met de naam $logs, die automatisch wordt gemaakt wanneer u Opslaganalyse voor een opslagaccount inschakelen.

> [!Note]
> * Zie voor meer informatie over facturering en bewaarbeleid voor gegevens [Storage Analytics en facturering](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> * Zie voor meer informatie over opslagaccountlimieten [Azure Storage schaalbaarheids- en prestatiedoelen](https://docs.microsoft.com/azure/storage/storage-scalability-targets).

Storage Analytics registreert de volgende soorten geverifieerde en anonieme aanvragen:

| Geverifieerd  | Anoniem|
| :------------- | :-------------|
| Geslaagde aanvragen | Geslaagde aanvragen |
|Mislukte aanvragen, met inbegrip van de time-outperiode, beperking, netwerk, autorisatie en andere fouten | -Aanvragen via een shared access signature, met inbegrip van mislukte en geslaagde aanvragen |
| -Aanvragen via een shared access signature, met inbegrip van mislukte en geslaagde aanvragen |Time-outfouten voor zowel client als server |
|   Aanvragen voor analytische gegevens |    GET-aanvragen is mislukt met foutcode 304 (niet gewijzigd) |
| Aanvragen door Storage Analytics zelf, zoals logboekbestanden worden gemaakt of verwijderd, worden niet geregistreerd. Een volledige lijst met gegevens in het logboek wordt beschreven in [Opslaganalyse geregistreerde bewerkingen en statusberichten](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) en [Opslaganalyse logboekindeling](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). | Alle andere mislukte anonieme aanvragen worden niet geregistreerd. Een volledige lijst met gegevens in het logboek wordt beschreven in [Opslaganalyse geregistreerde bewerkingen en statusberichten](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) en [Opslaganalyse logboekindeling](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |

### <a name="azure-networking-logs"></a>Azure VPN-Logboeken
Netwerk logboekregistratie en controle in Azure is uitgebreid en bevat informatie over twee hoofdcategorieën:

* [Netwerk-Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher): met de functies in netwerk-Watcher Scenario's gebaseerde netwerkbewaking is opgegeven. Deze service omvat pakketopname, volgende hop, IP-stroom controleren, groep beveiligingsweergave, NSG stroom Logboeken. Scenario niveau bewaking biedt een complete weergave van netwerkbronnen in tegenstelling tot afzonderlijke resource netwerkbewaking.

* [Broncontrole](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-resource-level-monitoring): niveau Broncontrole bestaat uit vier onderdelen, diagnostische logboeken, metrische gegevens, het oplossen van problemen en resourcestatus. Al deze functies zijn gebouwd op het niveau van de resource.

![Azure VPN-Logboeken](./media/azure-log-audit/azure-log-audit-fig4.png)

Netwerk-Watcher is een regionale service waarmee u kunt bewaken en onderzoeken van de voorwaarden op een netwerk scenario niveau in, en naar Azure. Netwerkdiagnose en visualisatie hulpprogramma's beschikbaar met de netwerk-Watcher kunnen u begrijpen, diagnoses stellen en Verkrijg inzicht in uw netwerk in Azure.

### <a name="network-security-group-flow-logging"></a>Netwerkbeveiligingsgroep stroom logboekregistratie

[NSG-logboeken voor stroom](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) zijn een functie van netwerk-Watcher die u gebruiken kunt om informatie over inkomende en uitgaande IP-verkeer via een NSG weer te geven. Deze stroom logboeken zijn geschreven in JSON-indeling en weergeven:
* Binnenkomende en uitgaande stromen per regel op basis van een.
* De NIC die de stroom is van toepassing op.
* informatie over de stroom 5-tuple: het bron- of doelserver IP-adres, de bron- of doelserver poort en het protocol.
* Hiermee wordt aangegeven of het verkeer is toegestaan of geweigerd.

Hoewel stroom doel nsg's registreert, worden ze niet weergegeven op dezelfde manier als de andere logboeken. Stroom logboeken worden opgeslagen alleen binnen een opslagaccount.

Het bewaarbeleid dezelfde die zichtbaar zijn op andere logboeken van toepassing op de logboeken van de stroom. Logboeken hebben een bewaarbeleid die u van 1 dag en 365 dagen instellen kunt. Als geen bewaarbeleid is ingesteld, worden de logboeken voor altijd bewaard.

**Diagnostische logboeken**

Periodieke en eigen initiatief gebeurtenissen zijn gemaakt door netwerkbronnen en vastgelegd in de storage-accounts en verzonden naar een event hub of logboekanalyse. De logboeken bieden inzicht in de status van een resource. Ze kunnen worden weergegeven in de hulpprogramma's, zoals Power BI en Log Analytics. Zie voor meer informatie over het weergeven van diagnostische logboeken, [logboekanalyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics).

![Logboeken met diagnostische gegevens](./media/azure-log-audit/azure-log-audit-fig5.png)

Diagnostische logboeken beschikbaar zijn voor [Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), Routes, en [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).

Netwerk-Watcher biedt dat een diagnostische logboeken weergeven. Deze weergave bevat alle netwerkresources die ondersteuning bieden voor logboekregistratie van diagnostische gegevens. U kunt in deze weergave inschakelen en uitschakelen van netwerkresources snel en gemakkelijk.


Naast de hierboven genoemde logboekregistratiemogelijkheden heeft netwerk-Watcher momenteel de volgende mogelijkheden:
- [Topologie](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview): biedt een weergave op netwerkniveau waarin verschillende verbindingskabels en koppelingen tussen netwerkbronnen in een resourcegroep.

- [Variabele pakketopname](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview): pakketgegevens naar en vanuit een virtuele machine worden vastgelegd. Geavanceerde filteropties en nauwkeuriger standaardtoegangsbeheer, zoals de maximale tijd en grootte-instellingen bieden veelzijdigheid. De pakketgegevens kunnen worden opgeslagen in een blob-opslag of op de lokale schijf in *CAP* bestandsindeling.

* [IP-stroom verificatie](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview): controleert of een pakket wordt toegestaan of geweigerd op basis van stroom informatie 5-tuple pakket parameters (dat wil zeggen, doel-IP, bron-IP, doelpoort, bronpoort en protocol). Als het pakket is geweigerd door een beveiligingsgroep, wordt de regel en de groep die het pakket geweigerd geretourneerd.

* [Volgende hop](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview): Hiermee bepaalt u de volgende hop voor pakketten in de fabric Azure-netwerk worden gerouteerd, zodat u kunt een diagnosticeren gebruiker gedefinieerde routes onjuist geconfigureerd.

* [Groep beveiligingsweergave](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview): de effectieve en toegepaste beveiligingsregels voor verbindingen die worden toegepast op een virtuele machine opgehaald.

* [Virtuele netwerkgateway en verbinding probleemoplossing](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest): helpt u problemen met virtuele netwerkgateways en verbindingen.

* [Netwerk-abonnementen](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-subscription-limits): Hiermee kunt u gebruik van netwerkbronnen op basis van limieten weergeven.

### <a name="application-insights"></a>Application Insights

[Azure Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) is een uitbreidbaar APM-service voor webontwikkelaars op meerdere platforms. Gebruik dit voor live webtoepassingen bewaken. Deze afwijkingen automatisch gedetecteerd. De service bevat krachtige analysehulpmiddelen om u te helpen bij het vaststellen van problemen en te begrijpen wat gebruikers daadwerkelijk doen met uw app.

Application Insights is ontworpen om u te helpen u continu prestaties en bruikbaarheid verbeteren.

De Tool werkt voor apps op een groot aantal verschillende platforms, waaronder .NET, Node.js en J2EE, of ze zijn gehost op lokale of in de cloud. Het kan worden geïntegreerd met uw DevOps-proces en verbindingspunten met verschillende ontwikkelhulpprogramma's heeft.

![Application Insights-diagram](./media/azure-log-audit/azure-log-audit-fig6.png)

Application Insights is bedoeld voor het ontwikkelingsteam en helpt u om te begrijpen hoe de app presteert en op welke manier de app wordt gebruikt. Met deze service kunt u het volgende controleren:

* **Aanvragen tarieven responstijden en uitvalpercentage**: weten welke pagina's van meest populaire op welke momenten van de dag, en waar uw gebruikers zijn. Ontdek welke pagina's het beste presteren. Als uw responstijden en uitvalpercentage hoge gaat wanneer er meer aanvragen, kunt u een resourcing probleem wellicht.

* **Afhankelijkheid tarieven responstijden en uitvalpercentage**: Hiermee wordt aangegeven of externe services u vertragen weten.

* **Uitzonderingen**: de samengevoegde statistieken te analyseren of Kies specifieke exemplaren en inzoomen op de stack-trace en de verwante aanvragen. Zowel server- als browseruitzonderingen worden gerapporteerd.

* **Paginaweergaven en laadprestaties**: ophalen van rapporten uit de browsers van uw gebruikers.

* **AJAX-aanroepen**: webpagina tarieven, responstijden en uitvalpercentage ophalen.

* **Aantal gebruikers en sessies**.

* **Prestatiemeteritems**: gegevens ophalen uit de Windows- of Linux-servers, zoals CPU, geheugen, en netwerkgebruik.

* **Diagnostische gegevens hosten**: gegevens ophalen uit de Docker- of Azure.

* **Diagnostische traceerlogboeken**: gegevens ophalen uit uw app zodat u traceringsgebeurtenissen met aanvragen correleren kunt.

* **Aangepaste gebeurtenissen en metrische gegevens**: ophalen van gegevens die u zelf in de code van een client of server zakelijke gebeurtenissen bijhouden zoals verkochte artikelen of games gewonnen schrijven.

De volgende tabel geeft een lijst van en beschrijft integratiescenario's:

| Integratiescenario | Beschrijving |
| --------------------- | :---------- |
|[De toepassingstoewijzing vast](https://docs.microsoft.com/azure/application-insights/app-insights-app-map)|Bekijk de onderdelen van uw app met belangrijke metrische gegevens en waarschuwingen.||
|[Diagnostische gegevens zoeken bijvoorbeeld gegevens](https://docs.microsoft.com/azure/application-insights/app-insights-diagnostic-search)| U kunt zoeken naar gebeurtenissen, zoals aanvragen, uitzonderingen, afhankelijkheidsaanroepen, logboektraceringen en paginaweergaven en deze gegevens ook filteren.||
|[Metrics Explorer voor geaggregeerde gegevens](https://docs.microsoft.com/azure/application-insights/app-insights-metrics-explorer)|Verken, filter en segmenteer cumulatieve gegevens, zoals aantallen aanvragen, fouten en uitzonderingen, reactietijden en paginalaadtijden.||
|[Dashboards](https://docs.microsoft.com/azure/application-insights/app-insights-dashboards#dashboards)|Combineer gegevens van meerdere resources tot een mash-up en deel deze met anderen. Ideaal voor toepassingen met meerdere onderdelen en om continu weer te geven in de teamkamer.||
|[Metrische gegevens livestream](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream)|Wanneer u een nieuwe build implementeert, kunt u kijken naar deze 'near-realtime' prestatie-indicatoren om te controleren of dat alles naar verwachting werkt.||
|[Analytische gegevens](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)|Beantwoord moeilijke vragen over de prestaties en het gebruik van uw app met behulp van deze krachtige querytaal.||
|[Automatische en handmatige waarschuwingen](https://docs.microsoft.com/azure/application-insights/app-insights-alerts)|Automatische meldingen aanpassen aan uw app normale patronen van Telemetrie en worden geactiveerd wanneer er iets buiten het gebruikelijke patroon. U kunt ook waarschuwingen instellen voor bepaalde niveaus van aangepaste functies of standaardfuncties voor het verzamelen van metrische gegevens.||
|[Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)|Prestatiegegevens bekijken in de code. Ga naar de code vanuit stack-traces.||
|[Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi)|Integreer metrische gegevens over het gebruik van de toepassing met andere business intelligence.||
|[REST API](https://dev.applicationinsights.io/)|Schrijf code voor het uitvoeren van query's op uw functies voor het verzamelen van metrische gegevens en op onbewerkte gegevens.||
|[Continue export](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry)|Het exporteren van de onbewerkte gegevens naar opslag bulksgewijs wanneer het wordt geleverd.||

### <a name="azure-security-center-alerts"></a>Azure Security Center-waarschuwingen
Detectie van dreigingen Azure Security Center werkt door het automatisch verzamelen van gegevens van de beveiliging van uw Azure-resources, het netwerk en verbonden partneroplossingen. Deze informatie wordt door Security Center geanalyseerd, waarbij vaak informatie uit meerdere bronnen wordt samengebracht om bedreigingen te analyseren. Beveiligingswaarschuwingen krijgen in Security Center een prioriteit, evenals aanbevelingen voor het oplossen van de bedreiging. Zie voor meer informatie [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro).

![Azure Security Center-diagram](./media/azure-log-audit/azure-log-audit-fig7.png)

Security Center maakt gebruik van geavanceerde beveiligingsanalyses die veel verder gaan dan op handtekeningen gebaseerde benaderingen. Toepassing doorbraken in grote hoeveelheden gegevens en [machine learning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) technologieën kunt u gebeurtenissen voor de volledige in de cloud-infrastructuur. Op deze manier wordt gedetecteerd bedreigingen die niet mogelijk om u te identificeren met behulp van handmatige benaderingen en het voorspellen van de evolutie van aanvallen. Deze beveiligingsanalyses omvatten:

* **Geïntegreerd dreigingen**: bekende ongewenste actoren zoekt door toe te passen wereldwijde dreigingen van Microsoft-producten en services, de Microsoft Digital Crimes Unit (DCU), de Microsoft Security Response Center (MSRC) en externe feeds.

* **Gedragsanalyse**: bekende patronen voor het detecteren van schadelijke gedrag van toepassing is.

* **Afwijkingsdetectie**: maakt gebruik van statistische samenstellen van profiel voor het bouwen van een historische basislijn. Er wordt een waarschuwing gegeven bij afwijkingen van vastgestelde basislijnen die aan een mogelijke aanvalsvector voldoen.

Veel beveiligingsbewerkingen en respons op incidenten teams afhankelijk van een SIEM-oplossing als uitgangspunt voor gesorteerd en beveiligingswaarschuwingen onderzoeken. U kunt Security Center-waarschuwingen en beveiligingsgebeurtenissen voor virtuele machine, die is verzameld door Azure diagnoses en audit Logboeken, met logboekanalyse of SIEM-oplossing in bijna realtime synchroniseren met Azure Log-integratie.

## <a name="log-analytics"></a>Log Analytics 

Log Analytics is een service in Azure die u bij het verzamelen en analyseren van gegevens die wordt gegenereerd door de bronnen in uw cloud en helpt on-premises omgevingen. Dit biedt u realtime-inzichten verkrijgen via geïntegreerd zoeken en aangepaste dashboards miljoenen records gemakkelijk te analyseren voor alle werkbelastingen en servers, ongeacht de fysieke locatie.

![Log Analytics-diagram](./media/azure-log-audit/azure-log-audit-fig8.png)

Op de center Log Analytics is de werkruimte voor logboekanalyse, die wordt gehost in Azure. Log Analytics verzamelt gegevens in de werkruimte van verbonden bronnen door gegevensbronnen configureren en oplossingen toevoegen aan uw abonnement. Gegevensbronnen en oplossingen die elk verschillende maken recordtypen, elk met een eigen set eigenschappen. Maar bronnen en -oplossingen kunnen nog steeds worden geanalyseerd samen in query's naar de werkruimte. Deze manier kunt u het hulpprogramma's en de methode gebruiken om te werken met een verscheidenheid aan gegevens verzameld door verschillende bronnen.

Verbonden bronnen zijn de computers en andere bronnen die de gegevens die worden verzameld door logboekanalyse genereren. Gegevensbronnen kunnen opnemen agents die zijn geïnstalleerd op [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) en [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) computers die rechtstreeks verbinding te maken of agents in [een verbonden beheergroep van System Center Operations Manager](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents). Log Analytics kan ook verzamelen van gegevens uit een [Azure storage-account](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage).

[Gegevensbronnen](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources) worden de verschillende soorten gegevens die worden verzameld van elke bron verbonden. Gebeurtenissen worden opgenomen die bronnen en [prestatiegegevens](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters) van [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) en Linux-agents, naast gegevensbronnen zoals [IIS-logboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs) en [aangepaste tekstlogboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-custom-logs). U configureert elke gegevensbron die u wenst te verzamelen en de configuratie wordt automatisch doorgegeven aan elke verbonden bron.

Er zijn vier manieren om [verzamelen van Logboeken en metrische gegevens voor Azure-services](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage):
* Azure Diagnostics direct met logboekanalyse (**Diagnostics** in de volgende tabel)

* Azure Diagnostics naar Azure-opslag met logboekanalyse (**opslag** in de volgende tabel)

* Connectors voor Azure-services (**Connector** in de volgende tabel)

* Scripts voor het verzamelen en vervolgens postgegevens in logboekanalyse (lege cellen in de volgende tabel en voor services die niet worden weergegeven)

| Service | Resourcetype | Logboeken | Metrische gegevens | Oplossing |
| :------ | :------------ | :--- | :------ | :------- |
|Azure Application Gateway| Microsoft.Network/<br>applicationGateways|  Diagnostics|Diagnostics|    [Azure-toepassing](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics) [Analytics Gateway](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics)|
|Application Insights||     Connector|  Connector|  [Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) [Connector (evaluatie)](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)|
|Azure Automation-accounts| Microsoft.Automation/<br>AutomationAccounts|    Diagnostics||       [Meer informatie](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|
|Azure Batch-accounts|  Microsoft.Batch/<br>batchAccounts|  Diagnostics|    Diagnostics||
|Klassieke cloudservices||       Storage||       [Meer informatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage-iis-table)|
|Cognitive Services|    Microsoft.CognitiveServices/<br>accounts|       Diagnostics|||
|Azure Data Lake Analytics| Microsoft.DataLakeAnalytics/<br>accounts|   Diagnostics|||
|Azure Data Lake Store| Microsoft.DataLakeStore/<br>accounts|   Diagnostics|||
|Azure Event Hub-naamruimte| Microsoft.EventHub/<br>naamruimten|  Diagnostics|    Diagnostics||
|Azure IoT Hub| Microsoft.Devices/<br>IotHubs||     Diagnostics||
|Azure Key Vault|   Microsoft.KeyVault/<br>kluizen|  Diagnostics  || [Key Vault-analyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-key-vault)|
|Azure Load Balancer|   Microsoft.Network/<br>loadBalancers|    Diagnostics|||
|Azure Logic Apps|  Microsoft.Logic/<br>Werkstromen|  Diagnostics|    Diagnostics||
||Microsoft.Logic/<br>integrationAccounts||||
|Netwerkbeveiligingsgroepen|   Microsoft.Network/<br>networksecuritygroups|Diagnostics||   [Netwerkbeveiligingsgroep Azure analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-network-security-group-analytics-solution-in-log-analytics)|
|Recovery kluizen|   Microsoft.RecoveryServices/<br>kluizen|||[Azure Recovery Services-Analytics (Preview)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
|Services zoeken|   Microsoft.Search/<br>searchServices|    Diagnostics|    Diagnostics||
|Service Bus-naamruimte| Microsoft.ServiceBus/<br>naamruimten|    Diagnostics|Diagnostics|    [Service Bus Analytics (Preview)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
|Service Fabric||       Storage||    [Service Fabric Analytics (Preview)](https://docs.microsoft.com/azure/log-analytics/log-analytics-service-fabric)|
|SQL (v12)| Microsoft.Sql/<br>servers /<br>databases||       Diagnostics||
||Microsoft.Sql/<br>servers /<br>elasticPools||||
|Storage|||         Script| [Azure Storage Analytics (Preview)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution)|
|Azure Virtual Machines|    Microsoft.Compute/<br>virtuele machines|  Extensie|  Extensie||
||||Diagnostics||
|Virtual machine scale sets|    Microsoft.Compute/<br>virtuele machines    ||Diagnostics||
||Microsoft.Compute/<br>virtualMachineScaleSets /<br>virtuele machines||||
|Webserver-farms|Microsoft.Web/<br>serverfarms||   Diagnostics
|Websites|  Microsoft.Web/<br>sites ||      Diagnostics|    [Meer informatie](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring)|
||Microsoft.Web/<br>sites /<br>sleuven|||||


## <a name="log-integration-with-on-premises-siem-systems"></a>Logboek-integratie met on-premises SIEM-systemen
Met [Azure Log integratie](https://www.microsoft.com/download/details.aspx?id=53324), kunt u de onbewerkte logboeken integreren van uw Azure-resources met uw on-premises SIEM-systeem.

![Diagram van logboek-integratie](./media/azure-log-audit/azure-log-audit-fig9.png)

Integratie van logboek verzamelt Azure diagnostics van uw virtuele machines van Windows Azure activiteitenlogboeken, Azure Security Center-waarschuwingen en logboeken van de Azure-resource provider. Deze integratie biedt een uniforme dashboard voor alle activa, of ze nu lokaal of in de cloud, zodat u samenvoegen kunt, correleren, analyseren en waarschuwen voor beveiligingsgebeurtenissen.

Logboek integratie momenteel ondersteunt de integratie van Azure activiteitenlogboeken, controlelogboeken voor Windows-gebeurtenislogboeken van Windows virtuele machines met uw Azure-abonnement, waarschuwingen van Azure Security Center, Azure diagnostische logboeken en Azure AD.

| Logboektype | Log Analytics ondersteuning van JSON (Splunk, ArcSight en IBM QRadar) |
| :------- | :-------------------------------------------------------- |
|Azure AD de auditlogboeken|   Ja|
|Activiteitenlogboeken| Ja|
|Security Center-waarschuwingen |Ja|
|Diagnostische logboeken (resource Logboeken)|  Ja|
|VM-Logboeken|   Ja, via doorgestuurde gebeurtenissen en niet via JSON|

[Aan de slag met Azure Log integratie](https://docs.microsoft.com/azure/security/security-azure-log-integration-get-started): deze zelfstudie leert u Azure Log-integratie installeren en het integreren van Logboeken van Azure storage, Azure activiteitenlogboeken, Azure Security Center-waarschuwingen en Azure AD controlelogboeken.

Integratiescenario's voor SIEM:

* [Partner configuratiestappen](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): dit blogbericht ziet u de integratie van Azure Log werken met partneroplossingen Splunk, HP ArcSight en IBM QRadar configureren.

* [Veelgestelde vragen over de Azure Log integratie](https://docs.microsoft.com/azure/security/security-azure-log-integration-faq): in dit artikel antwoorden op vragen over Azure Log-integratie.

* [Security Center waarschuwingen integreren met Azure Log integratie](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration): dit artikel wordt beschreven hoe u wilt synchroniseren van waarschuwingen in Security Center, beveiligingsgebeurtenissen voor virtuele machine die is verzameld door Azure diagnostics-logboeken en Azure auditlogboeken met uw logboekanalyse of SIEM oplossing.

## <a name="next-steps"></a>Volgende stappen

- [Controle en logboekregistratie](https://docs.microsoft.com/azure/security/security-management-and-monitoring-overview): gegevens beveiligen door het onderhouden van de zichtbaarheid en snel reageren op beveiligingswaarschuwingen tijdige.

- [Beveiliging-logboekregistratie en controlelogboek verzameling in Azure](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/): afdwingen van deze instellingen om ervoor te zorgen dat uw Azure-exemplaren de juiste beveiligings- en audit-logboeken verzamelt.

- [Controle-instellingen voor een siteverzameling configureren](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US): als u een siteverzamelingbeheerder, de geschiedenis van individuele gebruikers, acties en de geschiedenis van acties die worden uitgevoerd tijdens een bepaald datumbereik ophalen. 

- [Het controlelogboek zoeken in de Office 365-beveiliging en naleving Center](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US): gebruik van de Office 365-beveiliging & Compliancecentrum het unified controlelogboek zoeken en gebruikers- en activiteiten in uw Office 365-organisatie bekijken.


