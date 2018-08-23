---
title: Azure logboekregistratie en controle | Microsoft Docs
description: Meer informatie over hoe u gegevens voor logboekregistratie diep om inzicht te krijgen over uw toepassing kunt gebruiken.
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
ms.openlocfilehash: a499f609e517d880b3a942ac6dc3eb0dc10b69e7
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42055053"
---
# <a name="azure-logging-and-auditing"></a>Logboekregistratie en bewaking in Azure

Azure biedt een breed scala aan configureerbare beveiliging, controle en logboekregistratie opties waarmee u kunt hiaten in uw beveiligingsbeleid en -mechanismen. Dit artikel worden besproken genereren, verzamelen en analyseren van beveiligingslogboeken van services die worden gehost op Azure.

> [!Note]
> Bepaalde aanbevelingen in dit artikel kunnen leiden tot grotere hoeveelheden gegevens, netwerk- of computerresources en de kosten van uw licentie of abonnement verhogen.

## <a name="types-of-logs-in-azure"></a>Verschillende typen logboeken in Azure
Cloud-Apps zijn complexe, met veel bewegende onderdelen bevatten. -Logboeken bieden gegevens voor bescherming van uw toepassingen operationeel te houden. Logboeken helpen bij het oplossen van problemen met het verleden of te voorkomen dat de mogelijkheden die zijn. En ze kunnen helpen verbeteren de prestaties van toepassingen of onderhoud of Automatiseer acties die anders handmatig worden opgelost moeten zouden.

Logboeken in Azure zijn onderverdeeld in de volgende typen:
* **Beheer/beheer logboeken** bevatten informatie over de werking van Azure Resource Manager maken, bijwerken en verwijderen. Zie voor meer informatie, [Azure-activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).

* **Gegevens vlak logboeken** bevatten informatie over gebeurtenissen die worden gegenereerd als onderdeel Azure Resourcegebruik. Voorbeelden van dit type logboek zijn het Windows-gebeurtenis-systeem, beveiliging, en toepassingslogboeken in een virtuele machine (VM) en de [diagnoselogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) die zijn geconfigureerd via Azure Monitor.

* **Gebeurtenissen verwerkt** bevatten informatie over geanalyseerde gebeurtenissen/waarschuwingen die zijn verwerkt op uw rekening. Voorbeelden van dit type zijn [Azure Security Center-waarschuwingen](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) waar [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) heeft verwerkt en geanalyseerd van uw abonnement en biedt beknopte beveiligingswaarschuwingen.

De volgende tabel bevat de belangrijkste typen logboeken beschikbaar zijn in Azure:

| Logboekcategorie | Logboektype | Gebruik | Integratie |
| ------------ | -------- | ------ | ----------- |
|[Activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|Besturingselement vlak gebeurtenissen op Azure Resource Manager-resources|   Biedt inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement.|    De rest-API, [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|
|[Diagnostische logboeken van Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|Regelmatig gegevens over de werking van Azure Resource Manager-resources in het abonnement|    Biedt inzicht in bewerkingen die uw resource zelf uitgevoerd.| Azure Monitor, [Stream](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|
|[Azure AD-rapportage](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)|Logboeken en rapporten | Rapporten aanmelden gebruikersactiviteiten en informatie over gebruikers- en groepsbeheer systeemactiviteit.|[Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api-quickstart)|
|[Virtuele machines en cloudservices](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-collect-azurevm)|Windows Event Log-service en de Linux Syslog|    Systeemgegevens en logboekregistratie op de virtuele machines vastgelegd en die gegevens overdraagt naar een storage-account van uw keuze.|   Windows (met behulp van Windows Azure Diagnostics [[WAD](https://docs.microsoft.com/azure/azure-diagnostics)] opslag) en Linux in Azure Monitor|
|[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)|Logboekregistratie van opslag, biedt metrische gegevens voor een storage-account|Biedt inzicht in de trace-aanvragen, analyseert trends in gebruik en vaststellen van problemen met uw storage-account.|   REST-API of de [-clientbibliotheek](https://msdn.microsoft.com/library/azure/mt347887.aspx)|
|[Stroomlogboeken van Netwerkbeveiligingsgroep (NSG)](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview)|JSON-indeling, ziet u binnenkomende en uitgaande stromen op basis van per regel|Geeft informatie weer over de inkomende en uitgaande IP-verkeer via een Netwerkbeveiligingsgroep.|[Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)|
|[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview)|Logboeken, uitzonderingen en aangepaste diagnoses|   Biedt een application performance monitoring (APM)-service voor webontwikkelaars op meerdere platforms.| REST-API, [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)|
|Gegevens verwerken / beveiligingswaarschuwingen|    Waarschuwingen in Azure Security Center, Azure Log Analytics-waarschuwingen|   Biedt informatie over beveiliging en waarschuwingen.|  REST-API's, JSON|

### <a name="activity-logs"></a>Activiteitenlogboeken
[Azure-activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement. Activiteitenlogboeken werden voorheen bekend als 'audit logs' of 'operationele logs', omdat ze rapporteren [bedieningsvlak gebeurtenissen](https://driftboatdave.com/2016/10/13/azure-auditing-options-for-your-custom-reporting-needs/) voor uw abonnementen. 

Activiteitenlogboeken help u bepalen de ' wat, wie, en wanneer ' voor schrijfbewerkingen (dat wil zeggen, PUT, POST of te verwijderen). Activiteitenlogboek registreert ook informatie geven over de status van de bewerking en andere relevante eigenschappen. Activiteitenlogboeken bevatten geen lees (GET)-bewerkingen.

In dit artikel verwijzen PUT, POST en DELETE naar alle schrijfbewerkingen die een activiteitenlogboek op de resources bevat. Bijvoorbeeld, kunt u de activiteitenlogboeken vinden van een fout wanneer u bij het oplossen van problemen of om te controleren hoe een gebruiker in uw organisatie een resource heeft gewijzigd.

![Diagram van activiteit-logboek](./media/azure-log-audit/azure-log-audit-fig1.png)

U kunt gebeurtenissen van een activiteitenlogboek ophalen met behulp van Azure portal, [Azure CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli), PowerShell-cmdlets en [Azure Monitor REST API](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough). Activiteitenlogboeken zijn gegevens-bewaarperiode van 90 dagen.

Integratie van scenario's voor het activiteitenlogboek:

* [Maken van een e-mailadres of webhook-waarschuwing die wordt geactiveerd door het activiteitenlogboek](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-auditlog-to-webhook-email).

* [Deze Stream naar een event hub](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs) voor opname van een service van derden of aangepaste analyseoplossing zoals Power BI.

* Analyseren in Power BI met behulp van de [Power BI-inhoudspakket](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).

* [Sla deze op een storage-account voor archivering of handmatige inspectie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-activity-log). Met behulp van logboekprofielen kunt u de bewaartijd (in dagen).

* Opvragen en weergeven in de Azure-portal.

* Deze opvragen via PowerShell-cmdlet, Azure CLI of REST-API.

* Exporteren van het activiteitenlogboek met logboekprofielen naar [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).

U kunt een storage-account gebruiken of [event hub-naamruimte](https://docs.microsoft.com/azure/event-hubs/event-hubs-resource-manager-namespace-event-hub-enable-archive) die zich niet in hetzelfde abonnement bevinden als het account waarmee het logboek wordt verzenden. Degene die de instelling configureert moet de juiste [op rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) toegang tot beide abonnementen.

### <a name="azure-diagnostics-logs"></a>Diagnostische logboeken van Azure
Diagnostische logboeken van Azure worden gegenereerd door een resource die uitgebreide, regelmatig gegevens over de werking van die resource biedt. Resourcetype is afhankelijk van de inhoud van deze logboeken. Bijvoorbeeld, [Windows-gebeurtenislogboeken system](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) zijn van een categorie van logboeken met diagnostische gegevens voor virtuele machines, en [blob, table en wachtrijlogboeken](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) categorieën van diagnostische logboeken voor storage-accounts zijn. Diagnoselogboeken verschillen van activiteitenlogboeken die inzicht geven in de bewerkingen die zijn uitgevoerd op resources in uw abonnement.

![Diagnostische logboeken van Azure diagrammen](./media/azure-log-audit/azure-log-audit-fig2.png)

Diagnostische logboeken van Azure bieden meerdere configuratieopties, zoals de Azure-portal, PowerShell, Azure CLI en de REST-API.

**Integratiescenario 's**

* Opslaan naar een [opslagaccount](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) voor controle of handmatige controle. Met behulp van de diagnostische instellingen kunt u de bewaartijd (in dagen).

* [Deze Stream naar eventhubs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs) voor opname van een service van derden of aangepaste analytics-oplossing, zoals [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/).

* Analyseren met [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).

**Ondersteunde services, schema voor diagnostische logboeken en ondersteunde logboekcategorieën per resourcetype**


| Service | Schema's en documentatie | Resourcetype | Categorie |
| ------- | ------------- | ------------- | -------- |
|Azure Load Balancer| [Log Analytics voor Load Balancer (Preview)](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log)|Microsoft.Network/loadBalancers<br>Microsoft.Network/loadBalancers| LoadBalancerAlertEvent<br>LoadBalancerProbeHealthStatus|
|Netwerkbeveiligingsgroepen|[Logboekanalyses voor Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)|Microsoft.Network/networksecuritygroups<br>Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent<br>NetworkSecurityGroupRuleCounter|
|Azure Application Gateway|[Diagnostische logboekregistratie voor Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)|Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog<br>ApplicationGatewayPerformanceLog<br>ApplicationGatewayFirewallLog|
|Azure Key Vault|[Sleutelkluis-Logboeken](https://docs.microsoft.com/azure/key-vault/key-vault-logging)|Microsoft.KeyVault/vaults|AuditEvent|
|Azure Search|[Inschakelen en gebruiken van Search Traffic Analytics](https://docs.microsoft.com/azure/search/search-traffic-analytics)|Microsoft.Search/searchServices|OperationLogs|
|Azure Data Lake Store|[Diagnostische logboeken openen voor Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-diagnostic-logs)|Microsoft.DataLakeStore/accounts<br>Microsoft.DataLakeStore/accounts|Controleren<br>Aanvragen|
|Azure Data Lake Analytics|[Diagnostische logboeken openen voor Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-diagnostic-logs)|Microsoft.DataLakeAnalytics/accounts<br>Microsoft.DataLakeAnalytics/accounts|Controleren<br>Aanvragen|
|Azure Logic Apps|[Aangepast Logic Apps B2B-volgschema](https://docs.microsoft.com/azure/logic-apps/logic-apps-track-integration-account-custom-tracking-schema)|Microsoft.Logic/workflows<br>Microsoft.Logic/integrationAccounts|WorkflowRuntime<br>IntegrationAccountTrackingEvents|
|Azure Batch|[Diagnostische logboeken van Azure Batch](https://docs.microsoft.com/azure/batch/batch-diagnostics)|Microsoft.Batch/batchAccounts|ServiceLog|
|Azure Automation|[Log Analytics voor Azure Automation](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|Microsoft.Automation/automationAccounts<br>Microsoft.Automation/automationAccounts|JobLogs<br>JobStreams|
|Azure Event Hubs|[Event Hubs diagnostische logboeken](https://docs.microsoft.com/azure/event-hubs/event-hubs-diagnostic-logs)|Microsoft.EventHub/namespaces<br>Microsoft.EventHub/namespaces|ArchiveLogs<br>OperationalLogs|
|Azure Stream Analytics|[Diagnostische logboeken van taak](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)|Microsoft.StreamAnalytics/streamingjobs<br>Microsoft.StreamAnalytics/streamingjobs|Uitvoering<br>Ontwerpen|
|Azure Service Bus|[Diagnostische logboeken van Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-diagnostic-logs)|Microsoft.ServiceBus/namespaces|OperationalLogs|

### <a name="azure-active-directory-reporting"></a>Azure Active Directory-rapportage
Azure Active Directory (Azure AD) bevat beveiligings-, activiteit- en controlerapporten voor de map van een gebruiker. De [Azure AD-controlerapport](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) helpt u bij het identificeren van bevoegde acties die zijn opgetreden in de Azure AD-exemplaar van de gebruiker. Bevoegde acties zijn wijzigingen van de uitbreiding van bevoegdheden (bijvoorbeeld rol maken of opnieuw instellen van wachtwoorden), veranderende beleidsconfiguraties (bijvoorbeeld wachtwoordbeleid) of wijzigingen in de directory-configuratie (bijvoorbeeld: wijzigingen in domein federatie-instellingen).

De rapporten bevatten de controlerecord voor de naam van de gebeurtenis, de gebruiker die de actie, de doelresource beïnvloed door de wijziging en de datum en tijd (in UTC) wordt uitgevoerd. Gebruikers kunnen de lijst met controlegebeurtenissen ophalen voor Azure AD via de [Azure-portal](https://portal.azure.com/), zoals beschreven in [uw auditlogboeken weergeven](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

De opgenomen rapporten worden vermeld in de volgende tabel:

| Beveiligingsrapporten | Activiteitsrapporten | Controlerapporten |
| :--------------- | :--------------- | :------------ |
|Aanmeldingen van onbekende bronnen| Toepassingsgebruik: samenvatting| Directorycontrolerapport|
|Aanmeldingen na meerdere mislukte pogingen|  Toepassingsgebruik: gedetailleerd||
|Aanmeldingen vanuit meerdere locaties|    Toepassingsdashboard||
|Aanmeldingen van IP-adressen met verdachte activiteit|   Fouten bij het inrichten van een account||
|Onregelmatige aanmeldingsactiviteiten|    Afzonderlijke gebruikersapparaten||
|Aanmeldingen vanaf mogelijk geïnfecteerde apparaten|   Afzonderlijke gebruikersactiviteiten||
|Gebruikers met afwijkende aanmeldingsactiviteiten| Rapport met groepsactiviteiten||
||Activiteitenrapport voor registratie voor wachtwoord opnieuw instellen||
||Activiteit voor wachtwoord opnieuw instellen|||

De gegevens in deze rapporten kunnen nuttig zijn voor uw toepassingen, zoals Security Information and Event Management (SIEM)-systemen, audit en hulpprogramma's voor business intelligence. De API's van Azure AD Reporting bieden toegang tot de gegevens op programmeerniveau via een set op REST-gebaseerde API's. U kunt deze aanroepen [API's](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) uit verschillende programmeertalen en hulpprogramma's.

Gebeurtenissen in het controlerapport Azure AD worden gedurende 180 dagen bewaard.

> [!Note]
> Zie voor meer informatie over het bewaren van rapport [bewaarbeleid voor Azure AD-rapporten](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention).

Als u geïnteresseerd bent in uw controlegebeurtenissen langer bewaren, gebruikt u de rapportage-API voor het ophalen van regelmatig [controlegebeurtenissen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) in een afzonderlijk gegevensarchief.

### <a name="virtual-machine-logs-that-use-azure-diagnostics"></a>Virtuele machines, apparaatlogboeken die gebruikmaken van Azure Diagnostics
[Azure Diagnostics](https://docs.microsoft.com/azure/azure-diagnostics) is de mogelijkheid in Azure die kunt u het verzamelen van diagnostische gegevens op een geïmplementeerde toepassing. U kunt de extensie voor diagnostische gegevens gebruiken uit een van verschillende bronnen. Op dit moment ondersteund zijn [Azure cloud service-web- en werkrollen rollen](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me).

![Virtuele machines, apparaatlogboeken die gebruikmaken van Azure Diagnostics](./media/azure-log-audit/azure-log-audit-fig3.png)

### <a name="azure-virtual-machineshttpsazuremicrosoftcomdocumentationlearning-pathsvirtual-machines-that-are-running-microsoft-windows-and-service-fabrichttpsdocsmicrosoftcomazureservice-fabricservice-fabric-overview"></a>[Virtuele machines van Azure](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/) die Microsoft Windows worden uitgevoerd en [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview)

U kunt Azure Diagnostics op een virtuele machine inschakelen door een van de volgende handelingen uit:

* [Visual Studio gebruiken voor het traceren van virtuele machines van Azure](https://docs.microsoft.com/azure/vs-azure-tools-debug-cloud-services-virtual-machines)

* [Instellen van Azure Diagnostics op afstand op een Azure-machine](https://docs.microsoft.com/azure/virtual-machines-dotnet-diagnostics)

* [PowerShell gebruiken voor het instellen van diagnostische gegevens op Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

* [Een Windows-machine met controle en diagnostiek met behulp van een Azure Resource Manager-sjabloon maken](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="storage-analytics"></a>Storage Analytics
[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) logboeken en metrische gegevens biedt voor een opslagaccount. Deze gegevens kunt u gebruiken om aanvragen te traceren, gebruikstrends te analyseren en een diagnose uit te voeren voor problemen met uw opslagaccount. Logboekregistratie van opslag Analytics is beschikbaar voor de [Azure Blob-, Azure Queue- en Azure Table storage-services](https://docs.microsoft.com/azure/storage/storage-introduction). Storage Analytics wordt gedetailleerde informatie over geslaagde en mislukte aanvragen in een storage-service geregistreerd.

U kunt deze informatie gebruiken voor het bewaken van afzonderlijke aanvragen en om problemen met een opslagservice te diagnosticeren. Aanvragen worden geregistreerd op basis van best-effort. Logboekvermeldingen worden alleen als er aanvragen voor het service-eindpunt gemaakt. Bijvoorbeeld, als een storage-account heeft een activiteit in de blobeindpunt, maar niet in de tabel of wachtrij eindpunten, zijn alleen de logboeken die betrekking op de Blob storage-service hebben gemaakt.

Voor het gebruik van Storage Analytics, schakelen afzonderlijk voor elke service die u wilt bewaken. U kunt deze inschakelen in de [Azure-portal](https://portal.azure.com/). Zie voor meer informatie, [een opslagaccount in Azure portal controleren](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). U kunt ook Storage Analytics via een programma via de REST-API of de clientbibliotheek inschakelen. Gebruik de bewerking van de Service-eigenschappen instellen voor het inschakelen van Storage Analytics afzonderlijk voor elke service.

De samengevoegde gegevens worden opgeslagen in een bekende blob (voor logboekregistratie) en in bekende tabellen (voor metrische gegevens), die u openen kunt met behulp van de Blob storage-service en Table storage-service API's.

Storage Analytics heeft een limiet van 20 terabyte (TB) voor de hoeveelheid opgeslagen gegevens die onafhankelijk is van de totale limiet voor uw storage-account. Alle logboeken worden opgeslagen in [blok-blobs](https://docs.microsoft.com/azure/storage/storage-analytics) in een container met de naam $logs, die automatisch wordt gemaakt wanneer u Storage Analytics voor een opslagaccount inschakelen.

> [!Note]
> * Zie voor meer informatie over facturering en bewaarbeleid voor gegevens, [Storage Analytics en facturering](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> * Zie voor meer informatie over limieten voor opslagaccounts [schaalbaarheids- en prestatiedoelen voor Azure Storage](https://docs.microsoft.com/azure/storage/storage-scalability-targets).

Storage Analytics registreert de volgende soorten geverifieerde en anonieme aanvragen:

| Geverifieerd  | Anoniem|
| :------------- | :-------------|
| Geslaagde aanvragen | Geslaagde aanvragen |
|Mislukte aanvragen, met inbegrip van time-out, beperking, netwerk, autorisatie en andere fouten | Aanvragen via een handtekening voor gedeelde toegang, met inbegrip van mislukte en geslaagde aanvragen |
| Aanvragen via een handtekening voor gedeelde toegang, met inbegrip van mislukte en geslaagde aanvragen |Time-outfouten voor zowel client als server |
|   Aanvragen voor het analytics-gegevens |    GET-aanvragen is mislukt met foutcode 304 (niet gewijzigd) |
| Aanvragen van Storage Analytics zelf, zoals logboekbestanden worden gemaakt of verwijderd, worden niet geregistreerd. Een volledige lijst van de gegevens in het logboek wordt gedocumenteerd in [Opslaganalyse vastgelegd operations en statusberichten](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) en [Opslaganalyse logboekindeling](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). | Alle andere mislukte anonieme aanvragen worden niet geregistreerd. Een volledige lijst van de gegevens in het logboek wordt gedocumenteerd in [Opslaganalyse vastgelegd operations en statusberichten](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) en [Opslaganalyse logboekindeling](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |

### <a name="azure-networking-logs"></a>Azure-netwerklogboeken
Netwerk logboekregistratie en bewaking in Azure is uitgebreid en omvat onderverdeeld in twee hoofdcategorieën:

* [Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher): Scenario's gebaseerde netwerkbewaking is opgegeven met de functies van Network Watcher. Deze service omvat pakket vastleggen, volgende hop, IP-stroom controleren, weergave van de beveiligingsgroep, NSG-stroomlogboeken. Scenario niveau bewaking biedt een end-to-weergave van netwerkbronnen in tegenstelling tot afzonderlijke resource netwerkbewaking.

* [Controle van bronnen](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-resource-level-monitoring): resourcecontrole niveau bestaat uit vier functies, diagnostische logboeken, metrische gegevens, probleemoplossing en resource health. Al deze functies zijn gebouwd op het niveau van de netwerk-resource.

![Azure-netwerklogboeken](./media/azure-log-audit/azure-log-audit-fig4.png)

Network Watcher is een regionale service waarmee u kunt bewaken en diagnoses uitvoeren omstandigheden op netwerkscenarioniveau in, en naar Azure. Network Diagnostics Framework en visualisatiehulpmiddelen beschikbaar met Network Watcher kunnen u te begrijpen, vaststellen en inzicht verkrijgen in uw netwerk in Azure.

### <a name="network-security-group-flow-logging"></a>Stroomlogboeken van Netwerkbeveiligingsgroep

[NSG-stroomlogboeken](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) zijn een functie van Network Watcher die u gebruiken kunt om informatie over het inkomende en uitgaande IP-verkeer via een NSG weer te geven. Deze logboeken van de stroom zijn geschreven in JSON-indeling en weergeven:
* Binnenkomende en uitgaande stromen op basis van per regel.
* De NIC die de stroom is van toepassing op.
* 5-tuple-informatie over de stroom: het bron- of -IP-adres, de bron- of -poort en het protocol.
* Of het verkeer is toegestaan of geweigerd.

Hoewel stroomlogboeken voor nsg's doel, worden ze niet op dezelfde manier als de andere logboeken weergegeven. Logboeken van de stroom worden alleen binnen een storage-account opgeslagen.

Dezelfde beleidsregels voor retentie die zichtbaar zijn op andere logboeken van toepassing op Logboeken van de stroom. Logboeken hebben een bewaarbeleid dat u van 1 dag tot 365 dagen instellen kunt. Als geen bewaarbeleid is ingesteld, worden de logboeken voor altijd bewaard.

**Diagnostische logboeken**

Periodieke en spontaan gebeurtenissen zijn gemaakt met netwerkbronnen en vastgelegd in de storage-accounts en verzonden naar een event hub of Log Analytics. De logboeken bieden inzicht in de status van een resource. Ze kunnen worden weergegeven in de hulpprogramma's zoals Power BI en Log Analytics. Zie voor informatie over het weergeven van logboeken met diagnostische gegevens, [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics).

![Logboeken met diagnostische gegevens](./media/azure-log-audit/azure-log-audit-fig5.png)

Logboeken met diagnostische gegevens zijn beschikbaar voor [Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), Routes, en [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).

Network Watcher biedt dat een diagnostische logboeken weergeven. Deze weergave bevat alle netwerkresources die ondersteuning bieden voor logboekregistratie van diagnostische gegevens. In deze weergave kunt u in- en netwerkresources uitschakelen gemakkelijk en snel.


Naast de logboekregistratiemogelijkheden van het eerder genoemde heeft Network Watcher momenteel de volgende mogelijkheden:
- [Topologie](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview): biedt een weergave op netwerkniveau waarin de verschillende onderlinge verbindingen en koppelingen tussen netwerkbronnen in een resourcegroep.

- [Variabele pakketopname](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview): pakketgegevens in en uit een virtuele machine worden vastgelegd. Geavanceerde opties voor het filteren en verder aanpassen besturingselementen, zoals instellingen voor tijd en formaat beperking veelzijdigheid bieden. De pakketgegevens kunnen worden opgeslagen in een blob-opslag of op de lokale schijf in *.cap* bestandsindeling.

* [IP-stroom verificatie](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview): controleert of een pakket wordt toegestaan of geweigerd op basis van flow informatie 5-tuple pakket parameters (dat wil zeggen, doel-IP, bron-IP, doelpoort, bronpoort en protocol). Als het pakket is geweigerd door een beveiligingsgroep, wordt de regel en de groep die het pakket geweigerd geretourneerd.

* [Volgende hop](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview): Hiermee bepaalt u de volgende hop voor pakketten worden gerouteerd in de infrastructuur van Azure-netwerk, zodat u kunt een diagnose gebruiker gedefinieerde routes onjuist geconfigureerd.

* [Weergave van de beveiligingsgroep](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview): de kracht en toegepaste beveiligingsregels die worden toegepast op een virtuele machine opgehaald.

* [Virtuele netwerkgateway en verbinding probleemoplossing](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest): helpt u bij het oplossen van virtuele netwerkgateways en verbindingen.

* [De abonnementslimieten netwerk](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-subscription-limits): Hiermee kunt u gebruik van netwerkbronnen op basis van limieten weergeven.

### <a name="application-insights"></a>Application Insights

[Azure Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) is een uitbreidbare APM-service voor webontwikkelaars op meerdere platforms. Gebruik dit voor het bewaken van live web-apps. Er wordt automatisch gedetecteerd prestatieafwijkingen. De service bevat krachtige analysehulpmiddelen om u te helpen bij het vaststellen van problemen en te begrijpen wat gebruikers daadwerkelijk doen met uw app.

Application Insights is ontworpen om u te helpen u de prestaties en bruikbaarheid continu te verbeteren.

De Tool werkt voor apps op een groot aantal platforms, waaronder .NET, Node.js en J2EE, of ze nu on-premises gehost, of in de cloud. Het kan worden geïntegreerd met uw DevOps-proces en bevat verbindingspunten met verschillende hulpprogramma's voor ontwikkeling.

![Application Insights-diagram](./media/azure-log-audit/azure-log-audit-fig6.png)

Application Insights is bedoeld voor het ontwikkelingsteam en helpt u om te begrijpen hoe de app presteert en op welke manier de app wordt gebruikt. Met deze service kunt u het volgende controleren:

* **Aanvragen-tarieven, reactietijden en foutpercentages**: Ontdek welke pagina's meest populaire, op welke tijdstippen van de dag zijn en waar uw gebruikers zich bevinden. Ontdek welke pagina's het beste presteren. Als uw reactietijden en foutpercentages gaan wanneer er meer aanvragen worden ingediend, wellicht u foutpercentages.

* **Afhankelijkheidsrelaties, reactietijden en foutpercentages**: Ontdek of externe services zorgen voor vertraging.

* **Uitzonderingen**: analyseren van de cumulatieve statistische gegevens, of Kies specifieke gegevens en Zoom in op de stack-trace en verwante aanvragen. Zowel server- als browseruitzonderingen worden gerapporteerd.

* **Paginaweergaven en laadprestaties**: rapporten ophalen uit de browsers van uw gebruikers.

* **AJAX-aanroepen**: ophalen van webpagina-tarieven, reactietijden en foutpercentages.

* **Aantal gebruikers en sessies**.

* **Prestatiemeteritems**: gegevens ophalen uit de Windows- of Linux-servers, zoals CPU, geheugen, en netwerkgebruik.

* **Diagnostische gegevens van hosts**: gegevens ophalen uit Docker of Azure.

* **Diagnostische logboeken met traceringen**: gegevens ophalen uit uw app, zodat u traceringsgebeurtenissen met aanvragen correleren kan.

* **Aangepaste gebeurtenissen en metrische gegevens**: ophalen van gegevens die u zelf in de code van een client of server schrijft u zakelijke gebeurtenissen bijhouden zoals verkochte artikelen of gewonnen spellen.

De volgende tabel geeft een lijst van en beschrijft integratiescenario's:

| Integratiescenario | Beschrijving |
| --------------------- | :---------- |
|[Overzicht van de toepassing](https://docs.microsoft.com/azure/application-insights/app-insights-app-map)|Bekijk de onderdelen van uw app met belangrijke metrische gegevens en waarschuwingen.||
|[Diagnostische gegevens over zoeken bijvoorbeeld gegevens](https://docs.microsoft.com/azure/application-insights/app-insights-diagnostic-search)| U kunt zoeken naar gebeurtenissen, zoals aanvragen, uitzonderingen, afhankelijkheidsaanroepen, logboektraceringen en paginaweergaven en deze gegevens ook filteren.||
|[Metrics Explorer voor cumulatieve gegevens](https://docs.microsoft.com/azure/application-insights/app-insights-metrics-explorer)|Verken, filter en segmenteer cumulatieve gegevens, zoals aantallen aanvragen, fouten en uitzonderingen, reactietijden en paginalaadtijden.||
|[Dashboards](https://docs.microsoft.com/azure/application-insights/app-insights-dashboards#dashboards)|Combineer gegevens van meerdere resources tot een mash-up en deel deze met anderen. Ideaal voor toepassingen met meerdere onderdelen en om continu weer te geven in de teamkamer.||
|[Live Metrics Stream](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream)|Wanneer u een nieuwe build implementeert, kunt u kijken naar deze 'near-realtime' prestatie-indicatoren om te controleren of dat alles naar verwachting werkt.||
|[Analytische gegevens](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)|Beantwoord moeilijke vragen over de prestaties en het gebruik van uw app met behulp van deze krachtige querytaal.||
|[Automatische en handmatige waarschuwingen](https://docs.microsoft.com/azure/application-insights/app-insights-alerts)|Automatische waarschuwingen Anticipeer op de normale patronen van telemetrie van uw app en worden geactiveerd wanneer er iets buiten het normale patroon. U kunt ook waarschuwingen instellen voor bepaalde niveaus van aangepaste functies of standaardfuncties voor het verzamelen van metrische gegevens.||
|[Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)|Prestatiegegevens bekijken in de code. Ga naar de code vanuit stack-traces.||
|[Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi)|Integreer metrische gegevens over het gebruik van de toepassing met andere business intelligence.||
|[REST API](https://dev.applicationinsights.io/)|Schrijf code voor het uitvoeren van query's op uw functies voor het verzamelen van metrische gegevens en op onbewerkte gegevens.||
|[Continue export](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry)|Bulksgewijs exporteren van onbewerkte gegevens naar de opslag wanneer ze worden ontvangen.||

### <a name="azure-security-center-alerts"></a>Azure Security Center-waarschuwingen
Azure Security Center werkt volgens het automatisch verzamelen van gegevens van de beveiliging van uw Azure-resources, het netwerk en verbonden partneroplossingen. Deze informatie wordt door Security Center geanalyseerd, waarbij vaak informatie uit meerdere bronnen wordt samengebracht om bedreigingen te analyseren. Beveiligingswaarschuwingen krijgen in Security Center een prioriteit, evenals aanbevelingen voor het oplossen van de bedreiging. Zie voor meer informatie, [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro).

![Azure Security Center-diagram](./media/azure-log-audit/azure-log-audit-fig7.png)

Security Center maakt gebruik van geavanceerde beveiligingsanalyses die veel verder gaan dan op handtekeningen gebaseerde benaderingen. Toepassing doorbraken in grote hoeveelheden gegevens en [machine learning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) technologieën voor het evalueren van gebeurtenissen in de hele cloudinfrastructuur. Op deze manier kunnen er bedreigingen zijn gedetecteerd die onmogelijk te identificeren met behulp van handmatige benaderingen en het voorspellen van de ontwikkeling van aanvallen. Deze beveiligingsanalyses omvatten:

* **Geïntegreerde beveiligingsinformatie**: gezocht naar aanvallers door toe te passen van informatie over wereldwijde dreigingen van Microsoft-producten en services, de Microsoft Digital Crimes Unit (DCU), de Microsoft Security Response Center (MSRC) en externe feeds.

* **Gedragsanalyse**: bekende patronen voor het detecteren van schadelijk gedrag is van toepassing.

* **Detectie van afwijkingen**: maakt gebruik van statistische profilering om te maken van een historische basislijn. Er wordt een waarschuwing gegeven bij afwijkingen van vastgestelde basislijnen die aan een mogelijke aanvalsvector voldoen.

Veel beveiligingsbewerkingen en incident response teams vertrouwen op een SIEM-oplossing als startpunt voor het sorteren en onderzoeken van beveiligingswaarschuwingen. Met Azure-Logboekintegratie, kunt u waarschuwingen van Beveiligingscentrum en VM-beveiligingsgebeurtenissen, die worden verzameld door de diagnose en controle in Logboeken in Azure, met uw Log Analytics of SIEM-oplossing in bijna realtime synchroniseren.

## <a name="log-analytics"></a>Log Analytics 

Log Analytics is een service in Azure waarmee u verzamelen en analyseren van gegevens die wordt gegenereerd door resources in uw cloud en kunt on-premises omgevingen. Dit biedt u realtime inzichten met behulp van geïntegreerde Zoek- en aangepaste dashboards voor het analyseren van miljoenen records gemakkelijk in uw werkbelastingen en servers, ongeacht hun fysieke locatie.

![Log Analytics-diagram](./media/azure-log-audit/azure-log-audit-fig8.png)

In het centrum van Log Analytics is de Log Analytics-werkruimte, die wordt gehost in Azure. Log Analytics verzamelt gegevens in de werkruimte van verbonden bronnen door te configureren van gegevensbronnen en oplossingen toe te voegen aan uw abonnement. Gegevensbronnen en oplossingen creëren elk verschillende recordtypen, elk met een eigen set eigenschappen. Maar gegevensbronnen en oplossingen kunnen nog steeds worden geanalyseerd samen in query's in de werkruimte. Op deze manier kunt u dezelfde hulpprogramma's en methoden gebruiken om te werken met een verscheidenheid aan gegevens die door een groot aantal bronnen worden verzameld.

Verbonden bronnen zijn de computers en andere resources die de gegevens die worden verzameld door Log Analytics genereren. Bronnen kunnen opnemen agents die zijn geïnstalleerd op [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) en [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) computers die rechtstreeks verbinding maken of agenten in [een verbonden beheergroep van System Center Operations Manager](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents). Log Analytics kan ook gegevens verzamelen uit een [Azure storage-account](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage).

[Gegevensbronnen](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources) worden de verschillende soorten gegevens die worden verzameld van elke verbonden bron. Bronnen omvatten gebeurtenissen en [prestatiegegevens](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters) van [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) en Linux-agenten naast gegevensbronnen zoals [IIS-logboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs) en [aangepaste tekstlogboeken](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-custom-logs). U configureert elke gegevensbron die u wenst te verzamelen en de configuratie wordt automatisch doorgegeven aan elke verbonden bron.

Er zijn vier manieren om [verzamelen van Logboeken en metrische gegevens voor Azure-services](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage):
* Azure Diagnostics rechtstreeks naar Log Analytics (**Diagnostics** in de volgende tabel)

* Azure Diagnostics naar Azure-opslag naar Log Analytics (**opslag** in de volgende tabel)

* Connectors voor Azure-services (**Connector** in de volgende tabel)

* Scripts voor het verzamelen en vervolgens boek gegevens in Log Analytics (lege cellen in de volgende tabel en voor services die niet zijn opgenomen)

| Service | Resourcetype | Logboeken | Metrische gegevens | Oplossing |
| :------ | :------------ | :--- | :------ | :------- |
|Azure Application Gateway| Microsoft.Network/<br>applicationGateways|  Diagnostiek|Diagnostiek|    [Azure-toepassing](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics) [Gateway Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics)|
|Application Insights||     Connector|  Connector|  [Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) [Connector (Preview)](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)|
|Azure Automation-accounts| Microsoft.Automation/<br>AutomationAccounts|    Diagnostiek||       [Meer informatie](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|
|Azure Batch-accounts|  Microsoft.Batch/<br>batchAccounts|  Diagnostiek|    Diagnostiek||
|Klassieke cloudservices||       Storage||       [Meer informatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage-iis-table)|
|Cognitive Services|    Microsoft.CognitiveServices/<br>accounts|       Diagnostiek|||
|Azure Data Lake Analytics| Microsoft.DataLakeAnalytics/<br>accounts|   Diagnostiek|||
|Azure Data Lake Store| Microsoft.DataLakeStore/<br>accounts|   Diagnostiek|||
|Azure Event Hub-naamruimte| Microsoft.EventHub/<br>Naamruimten|  Diagnostiek|    Diagnostiek||
|Azure IoT Hub| Microsoft.Devices/<br>IotHubs||     Diagnostiek||
|Azure Key Vault|   Microsoft.KeyVault/<br>Kluizen|  Diagnostiek  || [Key Vault-analyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-key-vault)|
|Azure Load Balancer|   Microsoft.Network/<br>load balancers|    Diagnostiek|||
|Azure Logic Apps|  Microsoft.Logic/<br>Werkstromen|  Diagnostiek|    Diagnostiek||
||Microsoft.Logic/<br>integrationAccounts||||
|Netwerkbeveiligingsgroepen|   Microsoft.Network/<br>networksecuritygroups|Diagnostiek||   [Azure Network Security Group analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-network-security-group-analytics-solution-in-log-analytics)|
|Recovery-kluizen|   Microsoft.RecoveryServices/<br>Kluizen|||[Azure Recovery Services-analyse (Preview)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
|Services zoeken|   Microsoft.Search/<br>searchServices|    Diagnostiek|    Diagnostiek||
|Service Bus-naamruimte| Microsoft.ServiceBus/<br>Naamruimten|    Diagnostiek|Diagnostiek|    [Service Bus-analyse (Preview)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
|Service Fabric||       Storage||    [Service Fabric-analyse (Preview)](https://docs.microsoft.com/azure/log-analytics/log-analytics-service-fabric)|
|SQL (v12)| Microsoft.Sql/<br>servers /<br>databases||       Diagnostiek||
||Microsoft.Sql/<br>servers /<br>elasticPools||||
|Storage|||         Script| [Azure Storage Analytics (Preview)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution)|
|Azure Virtual Machines|    Microsoft.Compute/<br>virtuele machines|  Toestelnummer|  Toestelnummer||
||||Diagnostiek||
|Virtuele-machineschaalsets|    Microsoft.Compute/<br>virtuele machines    ||Diagnostiek||
||Microsoft.Compute/<br>virtualMachineScaleSets /<br>virtuele machines||||
|Web server-farms|Microsoft.Web/<br>serverfarms||   Diagnostiek
|Websites|  Microsoft.Web/<br>Sites ||      Diagnostiek|    [Meer informatie](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring)|
||Microsoft.Web/<br>sites /<br>sleuven|||||


## <a name="log-integration-with-on-premises-siem-systems"></a>Integratie met on-premises SIEM-systemen
Met [Azure-Logboekintegratie](https://www.microsoft.com/download/details.aspx?id=53324), kunt u de onbewerkte logboeken integreren in uw Azure-resources met uw on-premises SIEM-systeem.

![Meld u integratie-diagram](./media/azure-log-audit/azure-log-audit-fig9.png)

Logboekintegratie verzamelt diagnostische gegevens van Azure van uw virtuele machines van Windows, Azure-activiteitenlogboeken, Azure Security Center-waarschuwingen en logboeken van de Azure-resource provider. Deze integratie biedt via het uniforme dashboard voor al uw activa, of ze nu on-premises of in de cloud, zodat u samenvoegen kunt, correleren en analyseren en waarschuwen voor beveiligingsgebeurtenissen.

Logboekintegratie ondersteunt momenteel de integratie van Azure-activiteitenlogboeken, Windows-gebeurtenislogboeken van Windows-machines met uw Azure-abonnement, Azure Security Center-waarschuwingen, diagnostische logboeken van Azure en Azure AD-auditlogboeken.

| Logboektype | Log Analytics ondersteunt JSON (Splunk, ArcSight en IBM QRadar) |
| :------- | :-------------------------------------------------------- |
|Azure AD-auditlogboeken beschikbaar zijn|   Ja|
|Activiteitenlogboeken| Ja|
|Waarschuwingen van Beveiligingscentrum |Ja|
|Logboeken met diagnostische gegevens (Logboeken resource)|  Ja|
|VM-Logboeken|   Ja, via de doorgestuurde gebeurtenissen en niet via JSON|

[Aan de slag met Azure-Logboekintegratie](https://docs.microsoft.com/azure/security/security-azure-log-integration-get-started): in deze zelfstudie begeleidt u bij het installeren van Azure-Logboekintegratie en integratie van Logboeken van Azure storage, Azure-activiteitenlogboeken, waarschuwingen van Azure Security Center en Azure AD auditlogboeken.

Integratie van scenario's voor SIEM:

* [Stappen voor het configureren van partners](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): dit blogbericht ziet u hoe u Azure-Logboekintegratie om te werken met oplossingen van partners Splunk, HP ArcSight en IBM QRadar configureert.

* [Veelgestelde vragen over van Azure Log integratie](https://docs.microsoft.com/azure/security/security-azure-log-integration-faq): in dit artikel vindt u antwoorden op vragen over Azure-Logboekintegratie.

* [Waarschuwingen van Beveiligingscentrum integreren met Azure-Logboekintegratie](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration): in dit artikel wordt beschreven hoe u om te synchroniseren van waarschuwingen van Beveiligingscentrum, VM-beveiligingsgebeurtenissen verzameld door Azure diagnostics-logboeken en auditlogboeken van Azure beschikbaar zijn met uw Log Analytics of SIEM oplossing.

## <a name="next-steps"></a>Volgende stappen

- [Controle en logboekregistratie](https://docs.microsoft.com/azure/security/security-management-and-monitoring-overview): beveiligen van gegevens met het beheren van inzicht en snel reageren op tijdige beveiligingswaarschuwingen.

- [Beveiliging-logboekregistratie en auditlogboek verzameling in Azure](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/): afdwingen van deze instellingen om ervoor te zorgen dat uw Azure-exemplaren de juiste beveiliging en controle-logboeken verzamelt.

- [Controle-instellingen configureren voor een siteverzameling](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US): als u de beheerder van een site-verzameling, halen de geschiedenis van individuele gebruikers, acties en de geschiedenis van acties die worden uitgevoerd tijdens een bepaald datumbereik. 

- [Zoeken in het auditlogboek in het Office 365-beveiligings- en Nalevingscentrum](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US): het Office 365-beveiligings- en Nalevingscentrum gebruiken om te zoeken naar het geïntegreerde auditlogboek en activiteit van gebruikers en de beheerder in uw organisatie Office 365 weergeven.


