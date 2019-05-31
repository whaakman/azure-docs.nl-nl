---
title: Bronnen van de gegevens in Azure Monitor | Microsoft Docs
description: Beschrijving van de gegevens beschikbaar voor het bewaken van de status en prestaties van uw Azure-resources en de toepassingen die daarop worden uitgevoerd.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2019
ms.author: bwren
ms.openlocfilehash: b77fb3ab5651147c59b9f0afd22a2d6d0159c90e
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357529"
---
# <a name="sources-of-monitoring-data-for-azure-monitor"></a>Bronnen van bewakingsgegevens voor Azure Monitor
Azure Monitor is gebaseerd op een [algemene gegevens bewakingsplatform](data-platform.md) die bevat [logboeken](data-platform-logs.md) en [metrische gegevens](data-platform-metrics.md). Dit platform verzamelen van gegevens, kunt gegevens uit meerdere bronnen om te worden geanalyseerd samen met een gemeenschappelijke set hulpprogramma's in Azure Monitor. Bewakingsgegevens kan ook worden verzonden naar andere locaties voor de ondersteuning van bepaalde scenario's en bronnen naar andere locaties kunnen schrijven voordat ze verzameld in Logboeken of metrische gegevens worden kunnen.

Dit artikel beschrijft de verschillende bronnen van bewakingsgegevens die worden verzameld door Azure Monitor naast de gegevens die zijn gemaakt door Azure-resources. Vindt u koppelingen naar gedetailleerde informatie over de configuratie is vereist voor het verzamelen van deze gegevens naar verschillende locaties.

## <a name="application-tiers"></a>Toepassingslagen

Bronnen van bewakingsgegevens van Azure-toepassingen kunnen worden ingedeeld in lagen, de hoogste niveaus wordt uw toepassing zelf en de lagere lagen worden onderdelen van Azure-platform. De methode van de toegang tot gegevens van elke laag verschilt. Toepassingslagen worden samengevat in de onderstaande tabel en de bronnen van bewakingsgegevens in elke laag worden weergegeven in de volgende secties. Zie [gegevenslocaties in Azure Monitoring](data-locations.md) voor een beschrijving van elke locatie en hoe u toegang hebt tot de gegevens.


![Bewaking van lagen](../media/overview/overview.png)


### <a name="azure"></a>Azure
De volgende tabel wordt kort beschreven toepassingslagen die specifiek voor Azure zijn. Na de koppeling voor meer informatie over elk in de volgende secties.

| Laag | Description | Methode voor gebruikersstatusverzameling |
|:---|:---|:---|
| [Azure Tenant](#azure-tenant) | Gegevens over de werking van op tenantniveau-Azure-services, zoals Azure Active Directory. | AAD-gegevens weergeven in de portal of verzamelen van Azure Monitor met behulp van de diagnostische instelling van een tenant configureren. |
| [Azure-abonnement](#azure-subscription) | Gegevens met betrekking tot de status en het beheer van meerdere bronnen services in uw Azure-abonnement, zoals Resource Manager en de status van de Service. | In de portal weergeven of configureren van verzameling naar Azure Monitor met een logboekprofiel voor een. |
| [Azure-resources](#azure-resources) |  Gegevens over de werking en prestaties van elke Azure-resource. | Verzamelde metrische gegevens worden automatisch in Metrics Explorer weergeven.<br>Diagnostische instellingen voor het verzamelen van Logboeken in Azure Monitor configureren.<br>Bewaking van oplossingen en inzichten die beschikbaar zijn voor meer gedetailleerde bewaking voor specifieke brontypen. |

### <a name="azure-other-cloud-or-on-premises"></a>Azure, andere cloud of on-premises 
De volgende tabel wordt kort beschreven toepassingslagen die mogelijk in Azure, een andere cloud of on-premises. Na de koppeling voor meer informatie over elk in de volgende secties.

| Laag | Description | Methode voor gebruikersstatusverzameling |
|:---|:---|:---|
| [Besturingssysteem (Gast)](#operating-system-guest) | Gegevens over het besturingssysteem op de compute-resources. | Log Analytics-agent voor het verzamelen van gegevensbronnen van de client in Azure Monitor- en Afhankelijkheidsmonitors agent voor het verzamelen van afhankelijkheden ondersteuning van Azure Monitor voor virtuele machines installeren.<br>Voor virtuele machines van Azure, installeert u diagnostische Azure-extensie voor het verzamelen van Logboeken en metrische gegevens in Azure Monitor. |
| [Toepassingscode](#application-code) | Gegevens over de prestaties en functionaliteit van de werkelijke toepassing en de code, inclusief prestatietraces, toepassingslogboeken en telemetrie van de gebruiker. | Instrumenteer uw code voor het verzamelen van gegevens naar Application Insights. |
| [Aangepaste bronnen](#custom-sources) | Gegevens van externe services of andere onderdelen of apparaten. | Logboek of de metrische gegevens verzamelen in Azure Monitor vanaf een willekeurige REST-client. |

## <a name="azure-tenant"></a>Azure-tenant
Telemetrie met betrekking tot uw Azure-tenant worden verzameld van de tenant-brede services zoals Azure Active Directory.

![Verzameling van de Azure-tenant](media/data-sources/tenant.png)

### <a name="azure-active-directory-audit-logs"></a>Auditlogboeken van Azure Active Directory
[Azure Active Directory-rapportage](../../active-directory/reports-monitoring/overview-reports.md) bevat de geschiedenis van aanmelding activiteit en audit audittrail van wijzigingen in een bepaalde tenant. 

| Bestemming | Description | Referentie |
|:---|:---|:---|
| Logboeken van Azure Monitor | Azure AD-logboeken te verzamelen in Azure Monitor voor het analyseren van deze met andere bewakingsgegevens configureren. | [Azure AD-logboeken integreren met Azure Monitor-Logboeken (preview)](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) |
| Azure Storage | Azure AD-logboeken exporteren naar Azure Storage voor het archiveren. | [Zelfstudie: Logboeken van Azure AD naar Azure storage-account (preview) archiveren](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md) |
| Event Hub | Azure AD-Stream logboeken naar andere locaties met behulp van Event Hubs. | [Zelfstudie: Azure Active Directory-logboeken naar een Azure event hub (preview) Stream](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md). |



## <a name="azure-subscription"></a>Azure-abonnement
Telemetrie met betrekking tot de status en de werking van uw Azure-abonnement.

![Azure-abonnement](media/data-sources/azure-subscription.png)

### <a name="azure-activity-log"></a>Azure-activiteitenlogboek 
De [Azure-activiteitenlogboek](activity-logs-overview.md) service medische dossiers, samen met records in de van configuratiewijzigingen aan de resources in uw Azure-abonnement bevat. Het activiteitenlogboek is beschikbaar voor alle Azure-resources en vertegenwoordigt de _externe_ weergeven.

| Bestemming | Description | Referentie |
|:---|:---|
| Activiteitenlogboek | Het activiteitenlogboek zijn verzameld in een eigen gegevensarchief die u kunt bekijken in het menu Azure Monitor of gebruiken voor het maken van waarschuwingen voor activiteitenlogboeken. | [Query uitvoeren op het activiteitenlogboek in Azure portal](activity-log-view.md#azure-portal) |
| Logboeken van Azure Monitor | Configureer Azure Monitor-logboeken voor het verzamelen van het activiteitenlogboek voor het analyseren van deze met andere gegevens. | [Verzamelen en analyseren van Azure-activiteitenlogboeken in Log Analytics-werkruimte in Azure Monitor](activity-log-collect.md) |
| Azure Storage | Het activiteitenlogboek exporteren naar Azure Storage voor het archiveren. | [Het activiteitenlogboek archiveren](activity-log-export.md#archive-activity-log)  |
| Event Hubs | Stream het activiteitenlogboek naar andere locaties met behulp van Event Hubs | [Stream-activiteitenlogboek naar Event Hub](activity-log-export.md#stream-activity-log-to-event-hub). |

### <a name="azure-service-health"></a>Azure Service Health
[Azure Service Health](../../service-health/service-health-overview.md) bevat informatie over de status van de Azure-services in uw abonnement die uw toepassing en de resources afhankelijk van zijn.

| Bestemming | Description | Referentie |
|:---|:---|:---|
| Activiteitenlogboek<br>Logboeken van Azure Monitor | Servicestatus records worden opgeslagen in de Azure-activiteitenlogboek, zodat u kunt ze in Azure portal weergeven of uitvoeren van alle andere activiteiten die u met het activiteitenlogboek uitvoeren kunt. | [Servicestatusmeldingen bekijken met Azure Portal](service-notifications.md) |


## <a name="azure-resources"></a>Azure-resources
Metrische gegevens en resources niveau diagnostische logboeken bieden informatie over de _interne_ werking van Azure-resources. Deze zijn beschikbaar voor de meeste Azure-services en oplossingen en inzichten verzamelen van aanvullende gegevens voor bepaalde services.

![Azure-resource-verzameling](media/data-sources/azure-resources.png)


### <a name="platform-metrics"></a>Platform metrische gegevens 
De meeste Azure-services wordt verzonden [platform metrische gegevens](data-platform-metrics.md) die overeenkomen de prestaties en de bewerking rechtstreeks naar de database van de metrische gegevens. De specifieke [metrische gegevens varieert voor elk type resource](metrics-supported.md). 

| Bestemming | Description | Referentie |
|:---|:---|:---|
| Azure Monitor Metrics | Platform metrische gegevens worden geschreven naar de database van Azure Monitor metrische gegevens waarvoor geen configuratie. Platform metrische gegevens op de toegang van Metrics Explorer.  | [Aan de slag met Azure Metrics Explorer](metrics-getting-started.md)<br>[Ondersteunde metrische gegevens met Azure Monitor](metrics-supported.md) |
| Logboeken van Azure Monitor | Kopieer platform metrische gegevens naar de logboeken voor trending en andere analyse met behulp van Log Analytics. | [Azure diagnostics rechtstreeks naar Log Analytics](collect-azure-metrics-logs.md#azure-diagnostics-direct-to-log-analytics) |
| Event Hubs | Stream metrische gegevens naar andere locaties met behulp van Event Hubs. |[Azure-Stream bewakingsgegevens naar een event hub voor gebruik door een extern hulpprogramma](stream-monitoring-data-event-hubs.md) |

### <a name="diagnostic-logs"></a>Diagnostische logboeken
[Diagnostische logboeken](diagnostic-logs-overview.md) inzicht geven in de _interne_ werking van een Azure-resource.  Logboeken met diagnostische gegevens worden niet standaard ingeschakeld. U moet ze inschakelen en een doel voor elke resource opgeven. 

De configuratievereisten en de inhoud van diagnostische logboeken variëren per resourcetype, en niet alle services nog maken diagnostische logboeken. Zie [ondersteunde services, schema's en categorieën voor diagnostische logboeken van Azure](diagnostic-logs-schema.md) voor meer informatie over elke service en koppelingen naar gedetailleerde procedures. Als de service niet wordt weergegeven in dit artikel, wordt deze service wordt momenteel niet schrijven naar Logboeken met diagnostische gegevens.

| Bestemming | Description | Referentie |
|:---|:---|:---|
| Logboeken van Azure Monitor | Logboeken met diagnostische gegevens verzenden naar Azure Monitor-logboeken voor analyse met andere verzamelde gegevens. Sommige resources kunnen schrijven rechtstreeks naar Azure Monitor, terwijl anderen naar een opslagaccount schrijven voordat het wordt geïmporteerd in een Log Analytics-werkruimte. | [Stream Azure diagnostische logboeken naar Log Analytics-werkruimte in Azure Monitor](diagnostic-logs-stream-log-store.md)<br>[De Azure portal gebruiken voor het verzamelen van Logboeken van Azure Storage](azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage)  |
| Opslag | Verzenden van diagnostische logboeken naar Azure Storage voor het archiveren. | [Archiveren van diagnostische logboeken in Azure](archive-diagnostic-logs.md) |
| Event Hubs | Stream diagnostische logboeken naar andere locaties met behulp van Event Hubs. |[Stream Azure diagnostische logboeken naar een event hub](diagnostic-logs-stream-event-hubs.md) |

## <a name="operating-system-guest"></a>Besturingssysteem (Gast)
COMPUTE-resources in Azure, in andere clouds en on-premises hebben een gast-besturingssysteem om te controleren. Met de installatie van een of meer agents, kunt u telemetrie van de Gast verzamelen in Azure Monitor om te analyseren met de dezelfde controleprogramma's als de Azure-services zelf.

![Verzameling van Azure compute-resource](media/data-sources/compute-resources.png)

### <a name="azure-diagnostic-extension"></a>Diagnostische Azure-extensie
Inschakelen van de Azure Diagnostics-extensie voor Azure Virtual machines, kunt u voor het verzamelen van Logboeken en metrische gegevens van het gastbesturingssysteem van de Azure-rekenresources met inbegrip van Azure Cloud Service (klassiek) Web- en werkrollen, virtuele Machines, virtuele machine schaalsets, en Service Fabric.

| Bestemming | Description | Referentie |
|:---|:---|:---|
| Opslag | Wanneer u de extensie voor diagnostische gegevens inschakelt, wordt het schrijven naar een opslagaccount standaard. | [Diagnostische gegevens opslaan en weergeven in Azure Storage](diagnostics-extension-to-storage.md) |
| Azure Monitor Metrics | Wanneer u de extensie voor diagnostische gegevens voor het verzamelen van prestatiemeteritems configureert, worden ze naar de database van Azure Monitor metrics geschreven. | [Verzenden van Guest OS metrische gegevens in de Azure Monitor-metriek opslaan met behulp van Resource Manager-sjabloon voor een virtuele machine van Windows](collect-custom-metrics-guestos-resource-manager-vm.md) |
| Application Insights-logboekbestanden | Logboeken en prestatiemeteritems verzamelen van de rekenresource ondersteuning van uw toepassing kunnen worden geanalyseerd met andere toepassingsgegevens. | [Cloud Service, virtuele Machine of Service Fabric diagnostische gegevens verzenden naar Application Insights](diagnostics-extension-to-application-insights.md) |
| Event Hubs | Configureer de extensie voor diagnostische gegevens als u wilt de gegevens naar andere locaties met behulp van Event Hubs streamen.  | [Streaming van Azure Diagnostics-gegevens in het dynamische pad met behulp van Event Hubs](diagnostics-extension-stream-event-hubs.md) |

### <a name="log-analytics-agent"></a>Log Analytics-agent 
Installeer de Log Analytics-agent voor uitgebreide bewaking en beheer van uw Windows- of Linux-machines. De virtuele machine kan worden uitgevoerd in Azure, een andere cloud of on-premises.

| Bestemming | Description | Referentie |
|:---|:---|:---|
| Logboeken van Azure Monitor | De Log Analytics-agent maakt verbinding met Azure controleren rechtstreeks of via System Center Operations Manager en kunt u het verzamelen van gegevens van gegevensbronnen die u configureert of bewakingsoplossingen zodat u meer inzicht in toepassingen uitgevoerd op de virtuele machine. | [Agent-gegevensbronnen in Azure Monitor](agent-data-sources.md)<br>[Operations Manager verbinden met Azure Monitor](om-agents.md) |


### <a name="azure-monitor-for-vms"></a>Azure Monitor voor virtuele machines 
[Azure Monitor voor virtuele machines](../insights/vminsights-overview.md) biedt u een aangepaste bewakingservaring voor virtuele machines biedt functies dan kernfunctionaliteit van Azure Monitor, met inbegrip van de status van service- en VM-status. Een Agent voor afhankelijkheden op Windows en Linux-machines die in combinatie met de Log Analytics-agent voor het verzamelen van gedetecteerde gegevens over de processen die worden uitgevoerd op de virtuele machine en een extern Procesafhankelijkheden is vereist.

| Bestemming | Description | Referentie |
|:---|:---|:---|
| Logboeken van Azure Monitor | Gegevens over processen en afhankelijkheden worden opgeslagen op de agent. | [Met behulp van Azure Monitor voor virtuele machines (preview) de kaart om te begrijpen van toepassingsonderdelen](../insights/vminsights-maps.md) |
| VM-opslag | Azure Monitor voor virtuele machines worden opgeslagen informatie over de status van de Health in een aangepaste locatie. Dit is alleen beschikbaar voor Azure Monitor voor virtuele machines in Azure portal in aanvulling op de [REST-API van Azure Resource health](/rest/api/resourcehealth/). | [De status van uw virtuele Azure-machines begrijpen](../insights/vminsights-health.md)<br>[Azure Resource health REST-API](https://docs.microsoft.com/rest/api/resourcehealth/) |



## <a name="application-code"></a>Toepassingscode
Gedetailleerde toepassing bewaken in Azure Monitor is klaar met [Application Insights](https://docs.microsoft.com/azure/application-insights/) verzamelt gegevens van toepassingen die op een aantal verschillende platformen. De toepassing kan worden uitgevoerd in Azure, een andere cloud of on-premises.

![Verzamelen van gegevens van toepassing](media/data-sources/applications.png)


### <a name="application-data"></a>Toepassingsgegevens
Wanneer u Application Insights voor een toepassing inschakelt door een instrumentatiepakket installeren, verzamelt deze metrische gegevens en logboeken met betrekking tot de prestaties en het gebruik van de toepassing. Application Insights slaat de gegevens worden verzameld in de dezelfde Azure Monitor gegevensplatform dat wordt gebruikt door andere gegevensbronnen. Het uitgebreide hulpmiddelen voor het analyseren van deze gegevens bevat, maar u kunt deze ook analyseren met gegevens uit andere bronnen met behulp van hulpprogramma's zoals Metrics Explorer en Log Analytics.

| Bestemming | Description | Referentie |
|:---|:---|:---|
| Logboeken van Azure Monitor | Operationele gegevens over uw toepassing met inbegrip van paginaweergaven, aanvragen, uitzonderingen en traceringen. | [Analyseren van logboekgegevens in Azure Monitor](../log-query/log-query-overview.md) |
|                    | De afhankelijkheidsinformatie tussen toepassingsonderdelen ter ondersteuning van Toepassingsoverzicht en telemetriecorrelatie. | [Telemetriecorrelatie in Application Insights](../app/correlation.md) <br> [Toepassingskaart](../app/app-map.md) |
|            | Resultaten van de beschikbaarheidstests die testen van de beschikbaarheid en reactiesnelheid van uw toepassing vanaf verschillende locaties op het openbare Internet. | [De beschikbaarheid en reactiesnelheid van een website bewaken](../app/monitor-web-app-availability.md) |
| Azure Monitor Metrics | Application Insights verzamelt metrische gegevens met een beschrijving van de prestaties en het gebruik van de toepassing naast aangepaste metrische gegevens die u in uw toepassing in de database van Azure Monitor metrics definieert. | [Op basis van een logboek en vooraf samengevoegde metrische gegevens in Application Insights](../app/pre-aggregated-metrics-log-metrics.md)<br>[Application Insights-API voor aangepaste gebeurtenissen en metrische gegevens](../app/api-custom-events-metrics.md) |
| Azure Storage | Toepassingsgegevens verzenden naar Azure Storage voor het archiveren. | [Telemetrie exporteren vanuit Application Insights](../app/export-telemetry.md) |
|            | Details van de beschikbaarheidstests worden opgeslagen in Azure Storage. Application Insights in Azure portal gebruiken om te downloaden voor lokale analyse. Resultaten van de beschikbaarheidstests worden opgeslagen in Azure Monitor-Logboeken. | [De beschikbaarheid en reactiesnelheid van een website bewaken](../app/monitor-web-app-availability.md) |
|            | Profiler-traceringsgegevens worden opgeslagen in Azure Storage. Application Insights in Azure portal gebruiken om te downloaden voor lokale analyse.  | [Profiel productie-apps in Azure met Application Insights](../app/profiler-overview.md) 
|            | Fouten opsporen in momentopname van gegevens die is opgenomen voor een subset van uitzonderingen worden opgeslagen in Azure Storage. Application Insights in Azure portal gebruiken om te downloaden voor lokale analyse.  | [De werking van momentopnamen](../app/snapshot-debugger.md#how-snapshots-work) |

## <a name="monitoring-solutions-and-insights"></a>Bewakingsoplossingen en inzichten
[Bewakingsoplossingen](../insights/solutions.md) en [Insights](../insights/insights-overview.md) verzamelen van gegevens voor meer inzicht in de werking van een bepaalde service of toepassing. Ze kunnen resources in andere toepassingslagen en zelfs meerdere lagen adres.

### <a name="monitoring-solutions"></a>Bewakingsoplossingen

| Bestemming | Description | Referentie
|:---|:---|:---|
| Logboeken van Azure Monitor | Bewakingsoplossingen verzamelen van gegevens in Logboeken van Azure Monitor waarbij het wellicht niet geanalyseerd op basis van de querytaal of [weergaven](view-designer.md) die gewoonlijk zijn opgenomen in de oplossing. | [Details van de verzameling gegevens voor het bewaken van oplossingen in Azure](../insights/solutions-inventory.md) |


### <a name="azure-monitor-for-containers"></a>Azure Monitor voor Containers
[Azure Monitor voor Containers](../insights/container-insights-overview.md) vindt u een aangepaste controlemogelijkheden voor [Azure Kubernetes Service (AKS)](/azure/aks/). Het verzamelt gegevens over deze resources in de volgende tabel beschreven.

| Bestemming | Description | Referentie |
|:---|:---|:---|
| Logboeken van Azure Monitor | Slaat gegevens voor AKS inclusief voorraad, logboeken en gebeurtenissen te controleren. Metrische gegevens wordt ook opgeslagen in Logboeken om te kunnen gebruikmaken van de analyse-functionaliteit in de portal. | [Inzicht in prestaties van een AKS-cluster met Azure Monitor voor containers](../insights/container-insights-analyze.md) |
| Azure Monitor Metrics | Metrische gegevens is opgeslagen in de metrische database station visualisatie en waarschuwingen. | [De container metrische gegevens weergeven in metrics explorer](../insights/container-insights-analyze.md#view-container-metrics-in-metrics-explorer) |
| Azure Kubernetes Service | In volgorde naar een bijna realtime-ervaring geeft Azure-Monitor voor Containers gegevens rechtstreeks vanuit de service Azure Kubernetes in Azure portal. | [Containerlogboeken in realtime weergeven met Azure Monitor voor containers (preview)](../insights/container-insights-live-logs.md) |

### <a name="azure-monitor-for-vms"></a>Azure Monitor voor virtuele machines
[Azure Monitor voor virtuele machines](../insights/vminsights-overview.md) biedt een uiterst persoonlijke ervaring voor het bewaken van virtuele machines. Een beschrijving van de gegevens die door Azure Monitor worden verzameld voor virtuele machines is opgenomen in de [besturingssysteem (Gast)](#operating-system-guest) hierboven.

## <a name="custom-sources"></a>Aangepaste bronnen
Naast de standaard-laag van een toepassing moet u mogelijk andere bronnen die telemetrie die niet kan worden verzameld met de andere gegevensbronnen hebben controleren. Voor deze resources, deze gegevens naar metrische gegevens of logboeken met behulp van een API van Azure Monitor te schrijven.

![Aangepaste verzameling](media/data-sources/custom.png)

| Bestemming | Methode | Description | Referentie |
|:---|:---|:---|:---|
| Logboeken van Azure Monitor | Gegevensverzamelaar-API | Verzamel logboekgegevens uit alle REST-client en op te slaan in Log Analytics-werkruimte. | [Logboekgegevens verzenden naar Azure Monitor met de HTTP Data Collector-API (preview-versie)](data-collector-api.md) |
| Azure Monitor Metrics | API voor aangepaste metrische gegevens | Metrische gegevens verzamelen vanuit elke REST-client en opslaan in Azure Monitor metrics-database. | [Aangepaste metrische gegevens voor een Azure-resource verzenden naar de Azure Monitor metrische store met behulp van een REST-API](metrics-store-custom-rest-api.md) |


## <a name="other-services"></a>Andere services
Andere services in Azure schrijven gegevens naar de Azure Monitor-gegevensplatform. Hiermee kunt u voor het analyseren van gegevens die door deze services zijn verzameld met gegevens die zijn verzameld door Azure Monitor en gebruikmaken van de dezelfde analyse en visualisatie-hulpprogramma's.

| Service | Bestemming | Description | Referentie |
|:---|:---|:---|:---|
| [Azure Security Center](/azure/security-center/) | Logboeken van Azure Monitor | Azure Security Center slaat de beveiliging die worden verzameld in een Log Analytics-werkruimte, zodat ze kunnen worden geanalyseerd met andere logboekgegevens die door Azure Monitor worden verzameld.  | [Gegevensverzameling in Azure Security Center](../../security-center/security-center-enable-data-collection.md) |
| [Azure Sentinel](/azure/sentinel/) | Logboeken van Azure Monitor | Azure Sentinel slaat de gegevens die worden verzameld uit verschillende gegevensbronnen in een Log Analytics-werkruimte, zodat ze kunnen worden geanalyseerd met andere logboekgegevens die door Azure Monitor worden verzameld.  | [Verbinding maken met gegevensbronnen](/azure/sentinel/quickstart-onboard) |


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [typen gegevens te controleren die worden verzameld door Azure Monitor](data-platform.md) en hoe u kunt weergeven en analyseren van deze gegevens.
- Lijst met de [verschillende locaties waar de gegevens voor het opslaan van Azure-resources](data-locations.md) en hoe u het kunt openen. 
