---
title: Azure-Stream bewakingsgegevens naar Event Hubs
description: Meer informatie over het streamen van uw Azure-bewakingsgegevens naar een event hub om op te halen van de gegevens in een SIEM-partner of analyseprogramma.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: johnkem
ms.subservice: ''
ms.openlocfilehash: 549ec74514ff03e06ff25893d3fa865f179470e9
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56870683"
---
# <a name="stream-azure-monitoring-data-to-an-event-hub-for-consumption-by-an-external-tool"></a>Azure-Stream bewakingsgegevens naar een event hub voor gebruik door een extern hulpprogramma

Dit artikel helpt bij het instellen van de verschillende lagen van de gegevens van uw Azure-omgeving moet worden verzonden naar een enkele Event Hubs-naamruimte of event hub, waar deze kan worden verzameld door een extern hulpprogramma.

> [!VIDEO https://www.youtube.com/embed/SPHxCgbcvSw]

## <a name="what-data-can-i-send-into-an-event-hub"></a>Welke gegevens kan ik in een event hub verzenden?

Er zijn verschillende 'categorieën' van de gegevens te controleren binnen uw Azure-omgeving, en de methode van de toegang tot gegevens van elke laag verschilt enigszins. Deze lagen kunnen normaal gesproken worden omschreven als:

- **Bewakingsgegevens van de toepassing:** Gegevens over de prestaties en functionaliteit van de code die u hebt geschreven en worden uitgevoerd op Azure. Voorbeelden van gegevens voor toepassingsbewaking zijn prestatietraces, toepassingslogboeken en telemetrie van de gebruiker. Toepassing bewakingsgegevens worden meestal verzameld in een van de volgende manieren:
  - Door het instrumenteren van uw code met een SDK, zoals de [Application Insights-SDK](../../azure-monitor/app/app-insights-overview.md).
  - Door het uitvoeren van een bewakingsagent die luistert naar nieuwe op de machine toepassingslogboeken uitvoeren van uw toepassing, zoals de [Windows Azure Diagnoseagent](./../../azure-monitor/platform/diagnostics-extension-overview.md) of [Linux Azure Diagnoseagent](../../virtual-machines/extensions/diagnostics-linux.md).
- **Bewakingsgegevens van Guest OS:** Gegevens over het besturingssysteem waarop uw toepassing wordt uitgevoerd. Voorbeelden van Gast OS bewakingsgegevens zou zijn Linux syslog- of Windows-systeemgebeurtenissen. Voor het verzamelen van dit soort gegevens, moet u een agent wilt installeren, zoals de [Windows Azure Diagnoseagent](./../../azure-monitor/platform/diagnostics-extension-overview.md) of [Linux Azure Diagnoseagent](../../virtual-machines/extensions/diagnostics-linux.md).
- **Azure-resource door gegevens te controleren:** Gegevens over de werking van een Azure-resource. Voor bepaalde typen Azure-resource, zoals virtuele machines, moet u er een gastbesturingssysteem en toepassingen om te controleren binnen die Azure-service is. De resource door gegevens te controleren is het hoogste niveau van de gegevens die beschikbaar zijn voor andere Azure-resources, zoals Network Security Groups, (omdat er is geen gastbesturingssysteem of de toepassing die wordt uitgevoerd in die bronnen). Deze gegevens kan worden verzameld met behulp van [instellingen voor resourcediagnose](./../../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings).
- **Azure-abonnement door gegevens te controleren:** Gegevens over de werking en het beheer van een Azure-abonnement, evenals gegevens over de status en de werking van Azure zelf. De [activiteitenlogboek](./../../azure-monitor/platform/activity-logs-overview.md) bevat de meeste abonnement, zoals service health incidenten en Azure Resource Manager-controle door gegevens te controleren. U kunt deze gegevens met behulp van een Logboekprofiel verzamelen.
- **Azure-tenant door gegevens te controleren:** Gegevens over de werking van op tenantniveau-Azure-services, zoals Azure Active Directory. Controles van de Azure Active Directory en aanmeldingen zijn voorbeelden van de tenant door gegevens te controleren. Deze gegevens kan worden verzameld met behulp van de diagnostische instelling van een tenant.

Gegevens van elke laag kunnen worden verzonden naar een event hub, waar deze kan worden opgehaald in een partner-hulpprogramma. Sommige gegevensbronnen kunnen worden geconfigureerd voor het verzenden van gegevens rechtstreeks naar een event hub, terwijl andere verwerken, zoals een logische App zijn vereist om de vereiste gegevens op te halen. De volgende secties wordt beschreven hoe u gegevens uit elke laag kunnen worden gestreamd naar een event hub kunt configureren. De stappen wordt ervan uitgegaan dat u de activa in die laag moet worden bewaakt al hebt.

## <a name="set-up-an-event-hubs-namespace"></a>Instellen van een Event Hubs-naamruimte

Voordat u begint, moet u [maken van een Event Hubs-naamruimte en event hub](../../event-hubs/event-hubs-create.md). Deze naamruimte en event hub is de bestemming voor al uw bewakingsgegevens. Een Event Hubs-naamruimte is een logische groepering van eventhubs die toegang tot hetzelfde beleid delen, zoals een opslag veel heeft account afzonderlijke blobs in het storage-account. Houd er rekening mee enkele gegevens over de event hubs-naamruimte en eventhubs die u hebt gemaakt:
* We raden u aan met behulp van een Standard Event Hubs-naamruimte.
* Normaal gesproken is slechts één doorvoereenheid nodig. Als u nodig hebt om omhoog te schalen als uw logboek gebruik toeneemt, kunt u altijd handmatig het aantal doorvoereenheden voor de naamruimte later verhogen of automatisch inflatie inschakelen.
* Het aantal doorvoereenheden kunt u schaalt de doorvoer voor uw eventhubs. Het aantal partities kunt u parallel verbruik in veel consumenten. Één partitie kan maximaal 20MBps of ongeveer 20.000 berichten per seconde. Afhankelijk van het hulpprogramma gebruiken van de gegevens, mogelijk of die uit meerdere partities mogelijk niet ondersteund. Als u niet zeker weet over het aantal partities om in te stellen, het beste beginnen met vier partities.
* U wordt aangeraden dat u de bewaartermijn voor berichten ingesteld op uw event hub tot zeven dagen duren. Als de verbruikende tool uitgeschakeld voor meer dan een dag wordt, dit zorgt ervoor dat het hulpprogramma verder kan gaan waar deze is gestopt (voor gebeurtenissen tot 7 dagen).
* Wordt u aangeraden de standaardgroep voor consumenten voor uw event hub. Er is niet nodig om te maken van andere consumentengroepen of een afzonderlijke consumergroep wilt gebruiken, tenzij u van plan bent twee verschillende hulpprogramma's gebruiken dezelfde gegevens uit de dezelfde event hub.
* Voor de Azure-activiteitenlogboek, kiest u een Event Hubs-naamruimte en Azure Monitor maakt u een event hub in die naamruimte met de naam 'insights-logs-operationallogs'. Voor andere typen logboeken, kunt u een bestaande event hub (zodat u kunt het gebruiken van de dezelfde insights-logs-operationallogs event hub) kiezen of Azure Monitor een event hub per logboekcategorie maken.
* Poort 5671 en 5672 moet normaal gesproken worden geopend op de computer gebruiken van gegevens uit de event hub.

Ook raadpleegt u de [Veelgestelde vragen over Azure Event Hubs](../../event-hubs/event-hubs-faq.md).

## <a name="azure-tenant-monitoring-data"></a>Azure-tenant door gegevens te controleren

Azure-tenant door gegevens te controleren is momenteel alleen beschikbaar voor Azure Active Directory. U kunt de gegevens van [Azure Active Directory-rapportage](../../active-directory/reports-monitoring/overview-reports.md), die de geschiedenis van aanmelding activiteit en audit audittrail van wijzigingen in een bepaalde tenant bevat.

### <a name="azure-active-directory-data"></a>Azure Active Directory-gegevens

Voor het verzenden van gegevens uit de Azure Active Directory-logboek in een Event Hubs-naamruimte, instellen van de diagnostische instelling van een tenant van uw AAD-tenant. [Deze handleiding volgt](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md) voor het instellen van de diagnostische instelling van een tenant.

## <a name="azure-subscription-monitoring-data"></a>Azure-abonnement door gegevens te controleren

Azure-abonnement door gegevens te controleren is beschikbaar in de [Azure-activiteitenlogboek](./../../azure-monitor/platform/activity-logs-overview.md). Hierin zijn de maken, bijwerken en verwijderen van bewerkingen van Resource Manager, de wijzigingen in [Azure-servicestatus](../../service-health/service-health-overview.md) die mogelijk van invloed op bronnen in uw abonnement, de [resourcestatus](../../service-health/resource-health-overview.md) status overgangen en diverse andere soorten gebeurtenissen op abonnementsniveau. [Dit artikel worden alle categorieën van gebeurtenissen die worden weergegeven in de Azure-activiteitenlogboek](./../../azure-monitor/platform/activity-log-schema.md).

### <a name="activity-log-data"></a>Gegevens van een activiteitenlogboek

Voor het verzenden van gegevens uit de Azure-activiteitenlogboek in een Event Hubs-naamruimte, instellen van een Logboekprofiel voor uw abonnement. [Deze handleiding volgt](./activity-logs-stream-event-hubs.md) voor het instellen van een Logboekprofiel voor uw abonnement. Doe dit eenmaal per abonnement dat u wilt bewaken.

> [!TIP]
> Een Logboekprofiel kunt op dit moment u alleen te selecteren van een Event Hubs-naamruimte waarin een event hub is gemaakt met de naam 'insights-operational-logs.' Het is nog niet mogelijk om op te geven van uw eigen naam event hub in een logboek-profiel.

## <a name="azure-resource-metrics-and-diagnostics-logs"></a>Logboeken voor het metrische en diagnostische gegevens van Azure-resource

Azure-resources verzenden twee soorten gegevens te controleren:
1. [Diagnostische logboeken van resource](./../../azure-monitor/platform/diagnostic-logs-overview.md)
2. [Metrische gegevens](../../azure-monitor/platform/data-collection.md)

Beide typen gegevens worden verzonden naar een event hub met behulp van de diagnostische instelling van een resource. [Deze handleiding volgt](./../../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md) voor het instellen van de diagnostische instelling van een resource op een bepaalde resource. Een resource diagnostische instelling voor elke bron van waaruit u wenst te verzamelen van logboeken instellen.

> [!TIP]
> U kunt Azure Policy gebruiken om ervoor te zorgen dat elke resource binnen een bepaald bereik altijd is ingesteld met een diagnostische instelling [met behulp van het DeployIfNotExists-effect in de beleidsregel](../../governance/policy/concepts/definition-structure.md#policy-rule).

## <a name="guest-os-data"></a>Gast-OS-gegevens

U moet een agent voor het verzenden van Gast OS bewakingsgegevens naar een event hub te installeren. Voor Windows of Linux geeft u de gegevens die u wilt dat moet worden verzonden naar de event hub, evenals de event hub waarnaar de gegevens moeten worden verzonden in een configuratiebestand en het configuratiebestand doorgeven aan de agent wordt uitgevoerd op de virtuele machine.

### <a name="linux-data"></a>Linux-gegevens

De [Linux Azure Diagnostics-agent](../../virtual-machines/extensions/diagnostics-linux.md) kan worden gebruikt voor het verzenden van gegevens van een Linux-machine naar een event hub te controleren. Dit doen door toe te voegen van de event hub als een sink in uw LAD beveiligde instellingen van configuratiebestand JSON. [Raadpleeg dit artikel voor meer informatie over het toevoegen van de event hub-sink aan uw Linux Azure Diagnostics-agent](../../virtual-machines/extensions/diagnostics-linux.md#protected-settings).

> [!NOTE]
> U kunt voor streaming van Gast-OS bewakingsgegevens naar een event hub in de portal instellen. In plaats daarvan moet u handmatig het configuratiebestand bewerken.

### <a name="windows-data"></a>Windows-gegevens

De [Windows Azure Diagnostics-agent](./../../azure-monitor/platform/diagnostics-extension-overview.md) kan worden gebruikt voor het verzenden van gegevens van een Windows-machine naar een event hub te controleren. Dit doen door de event hub als een sink in de sectie privateConfig van het configuratiebestand WAD toe te voegen. [Raadpleeg dit artikel voor meer informatie over het toevoegen van de event hub-sink aan uw Windows Azure Diagnostics-agent](./../../azure-monitor/platform/diagnostics-extension-stream-event-hubs.md).

> [!NOTE]
> U kunt voor streaming van Gast-OS bewakingsgegevens naar een event hub in de portal instellen. In plaats daarvan moet u handmatig het configuratiebestand bewerken.

## <a name="application-monitoring-data"></a>Bewakingsgegevens van de toepassing

Toepassing door gegevens te controleren is vereist dat uw code is uitgerust met een SDK, dus er is niet een algemene oplossing om routering toepassing bewakingsgegevens naar een event hub in Azure. Echter, [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) is een service die kan worden gebruikt voor het verzamelen van gegevens van Azure op toepassingsniveau. Als u Application Insights gebruikt, kunt u bewakingsgegevens naar een event hub streamen door het volgende te doen:

1. [Instellen van continue export](../../azure-monitor/app/export-telemetry.md) van de Application Insights-gegevens naar een opslagaccount.

2. Een timer wordt geactiveerd logische App instelt die [worden gegevens opgehaald uit de blob-opslag](../../connectors/connectors-create-api-azureblobstorage.md#add-action) en [wordt als een bericht naar de event hub gepusht](../../connectors/connectors-create-api-azure-event-hubs.md#add-action).

## <a name="what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub"></a>Wat kan ik doen met de gegevens worden verzonden naar de event hub?

Routering van uw bewakingsgegevens naar een event hub met Azure Monitor kunt u eenvoudig kunt integreren met SIEM-partner- en controlehulpprogramma's. De meeste hulpprogramma's moeten de event hub-verbindingsreeks en bepaalde machtigingen aan uw Azure-abonnement om gegevens te lezen uit de event hub. Hier ziet u een onvolledige lijst met Azure Monitor-integratie:

* **IBM QRadar** -DSM van de Microsoft Azure en Microsoft Azure Event Hub-Protocol zijn beschikbaar voor downloaden van [de ondersteuningswebsite van IBM](https://www.ibm.com/support). Informatie over [de integratie mat Azure vindt u hier](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0).
* **Splunk** -afhankelijk van de instellingen Splunk, er zijn twee manieren:
    1. [De Azure Monitor-invoegtoepassing voor Splunk](https://splunkbase.splunk.com/app/3534/) is beschikbaar in Splunkbase en een open-source-project. [Documentatie is hier](https://github.com/Microsoft/AzureMonitorAddonForSplunk/wiki/Azure-Monitor-Addon-For-Splunk).
    2. Als u niet een invoegtoepassing in uw exemplaar van Splunk (bv installeren. Als een proxy of die worden uitgevoerd op Splunk Cloud), kunt u deze gebeurtenissen voor het gebruik van de Splunk HTTP Event Collector doorsturen [deze functie die wordt geactiveerd door nieuwe berichten in de event hub](https://github.com/Microsoft/AzureFunctionforSplunkVS).
* **SumoLogic** -instructies voor het instellen van SumoLogic gebruiken voor gegevens van een event hub zijn [beschikbaar hier](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub)
* **ArcSight** -slimme de ArcSight Azure Event Hub-connector is beschikbaar als onderdeel van [de ArcSight slimme connector verzameling hier](https://community.softwaregrp.com/t5/Discussions/Announcing-General-Availability-of-ArcSight-Smart-Connectors-7/m-p/1671852).
* **Syslog-server** : als u wilt dat Azure Monitor om gegevens te streamen rechtstreeks naar een syslog-server, kunt u uitchecken [deze GitHub-opslagplaats](https://github.com/miguelangelopereira/azuremonitor2syslog/).

## <a name="next-steps"></a>Volgende stappen
* [Het activiteitenlogboek naar een opslagaccount archiveren](../../azure-monitor/platform/archive-activity-log.md)
* [Lees het overzicht van de Azure-activiteitenlogboek](../../azure-monitor/platform/activity-logs-overview.md)
* [Instellen van een waarschuwing op basis van een activiteitenlogboek-gebeurtenis](../../azure-monitor/platform/alerts-log-webhook.md)


