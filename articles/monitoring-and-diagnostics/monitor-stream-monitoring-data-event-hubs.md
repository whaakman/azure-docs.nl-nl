---
title: Het bewaken van gegevens naar Event Hubs Azure Stream | Microsoft Docs
description: Informatie over het streamen van alle Azure-bewaking gegevens naar een event hub te halen van de gegevens in een SIEM-partner of -hulpprogramma voor analyse.
author: johnkemnetz
manager: robb
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/11/2018
ms.author: johnkem
ms.openlocfilehash: b2813035b4665a36b475e791965d395b84ddb3f1
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="stream-azure-monitoring-data-to-an-event-hub-for-consumption-by-an-external-tool"></a>Stroom Azure voor het bewaken van gegevens naar een event hub voor verbruik door een extern hulpprogramma

Azure biedt één pijplijn voor het verkrijgen van toegang tot alle bewakingsgegevens van uw Azure-omgeving, zodat u gemakkelijk partner SIEM instellen en controleprogramma's voor het verbruik van die gegevens. Dit artikel begeleidt bij het instellen van de verschillende lagen van gegevens van uw Azure-omgeving worden verzonden naar een enkele Event Hubs naamruimte of event hub, waar deze kan worden verzameld door een extern hulpprogramma.

## <a name="what-data-can-i-send-into-an-event-hub"></a>Welke gegevens kan ik in een event hub verzenden? 

Er zijn verschillende 'lagen' van het bewaken van gegevens binnen uw Azure-omgeving en de methode van de toegang tot gegevens van elke laag verschilt enigszins. Deze lagen kunnen normaal gesproken als worden beschreven:

- **Gegevens voor toepassingsbewaking:** gegevens over de prestaties en functionaliteit van de code die u hebt geschreven en worden uitgevoerd op Azure. Voorbeelden van gegevens voor toepassingsbewaking zijn prestaties traceringen en toepassingslogboeken telemetrie van de gebruiker. Toepassing bewakingsgegevens worden meestal verzameld in een van de volgende manieren:
  - Door het instrumenteren van uw code bij een SDK, zoals de [Application Insights-SDK](../application-insights/app-insights-overview.md).
  - Door het uitvoeren van een bewakingsagent die luistert naar nieuwe op de machine toepassingslogboeken uitvoeren van uw toepassing, zoals de [Windows Azure diagnostische Agent](./azure-diagnostics.md) of [diagnostische Azure-Agent Linux](../virtual-machines/linux/diagnostic-extension.md).
- **Gastbesturingssysteem bewakingsgegevens:** gegevens over het besturingssysteem waarop de toepassing wordt uitgevoerd. Voorbeelden van Gast OS bewakingsgegevens's Linux syslog- of Windows-systeemgebeurtenissen zijn. Voor het verzamelen van dit type gegevens, moet u een agent installeren zoals de [Windows Azure diagnostische Agent](./azure-diagnostics.md) of [diagnostische Azure-Agent Linux](../virtual-machines/linux/diagnostic-extension.md).
- **Azure-resource bewakingsgegevens:** gegevens over de werking van een Azure-resource. Voor sommige Azure brontypen, zoals virtuele machines is een gastbesturingssysteem en de toepassing(en) om te controleren in die Azure-service. De resource bewakingsgegevens is voor andere Azure-resources, zoals Netwerkbeveiligingsgroepen, de hoogste laag van de beschikbare gegevens (omdat er is geen gastbesturingssysteem of de toepassing die wordt uitgevoerd in die bronnen). Deze gegevens kan worden verzameld met behulp van [diagnostische broninstellingen](./monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).
- **Azure-platform bewakingsgegevens:** gegevens over de werking en het beheer van een Azure-abonnement of de tenant, evenals gegevens over de status en de werking van Azure zelf. De [activiteitenlogboek](./monitoring-overview-activity-logs.md), met inbegrip van de health-servicegegevens en Active Directory audits zijn voorbeelden van bewakingsgegevens platform. Deze gegevens kan worden verzameld met behulp van diagnostische instellingen ook.

Gegevens van elke laag kunnen worden verzonden in een event hub, waar deze in een partner-hulpprogramma kan worden opgevraagd. De volgende secties wordt beschreven hoe u gegevens van elke laag kunnen worden gestreamd naar een event hub kunt configureren. De stappen wordt ervan uitgegaan dat u al activa op die laag worden bewaakt.

Voordat u begint, moet u [een Event Hubs-naamruimte en event hub maken](../event-hubs/event-hubs-create.md). Deze naamruimte en event hub is de bestemming voor al uw bewakingsgegevens.

## <a name="how-do-i-set-up-azure-platform-monitoring-data-to-be-streamed-to-an-event-hub"></a>Hoe stel ik Azure-platform bewakingsgegevens in kunnen worden gestreamd naar een event hub?

Er is een Azure-platform bewakingsgegevens afkomstig uit twee belangrijke bronnen:
1. De [Azure activiteitenlogboek](./monitoring-overview-activity-logs.md), waarin de maken, bijwerken en verwijderen van bewerkingen van Resource Manager, de wijzigingen in [Azure servicestatus](../service-health/service-health-overview.md) die mogelijk van invloed op bronnen in uw abonnement, het [resourcestatus](../service-health/resource-health-overview.md) statusovergangen en verschillende andere typen abonnementsniveau gebeurtenissen. [In dit artikel beschrijft alle categorieën van gebeurtenissen die worden weergegeven in het Azure activiteitenlogboek](./monitoring-activity-log-schema.md).
2. [Rapportage van Azure Active Directory](../active-directory/active-directory-reporting-azure-portal.md), die de geschiedenis van aanmelden activiteit en audit audittrail van wijzigingen aangebracht in een bepaalde tenant bevat. Het is nog niet mogelijk om te streamen Azure Active Directory-gegevens in een event hub.

### <a name="stream-azure-activity-log-data-into-an-event-hub"></a>Stroom Azure activiteit logboekgegevens in een event hub

Om gegevens te verzenden van het Azure activiteitenlogboek in een naamruimte Event Hubs, u een logboek profiel instellen voor uw abonnement. [Deze handleiding volgt](./monitoring-stream-activity-logs-event-hubs.md) voor het instellen van een profiel van het logboek op uw abonnement. Doe dit eenmaal per abonnement dat u wilt bewaken.

> [!TIP]
> Een profiel voor een logboek kunt momenteel u alleen selecteren van een naamruimte Event Hubs, waarop een event hub is gemaakt met de naam 'insights-operationele-Logboeken.' Het is nog niet kunt u de naam van uw eigen event hub opgeven in een logboek-profiel.

## <a name="how-do-i-set-up-azure-resource-monitoring-data-to-be-streamed-to-an-event-hub"></a>Hoe stel ik Azure-resource bewakingsgegevens in kunnen worden gestreamd naar een event hub?

Azure-resources verzenden twee soorten bewakingsgegevens:
1. [Diagnostische logboeken van resource](./monitoring-overview-of-diagnostic-logs.md)
2. [Metrische gegevens](monitoring-overview-metrics.md)

Beide typen gegevens worden verzonden naar een event hub met behulp van de diagnostische instelling van een resource. [Deze handleiding volgt](./monitoring-stream-diagnostic-logs-to-event-hubs.md) voor het instellen van een resource diagnostische instellen op een bepaalde bron. Stel een diagnostische instelling voor elke bron van waaruit u wilt verzamelen van Logboeken.

> [!TIP]
> U kunt Azure-beleid gebruiken om ervoor te zorgen dat elke bron binnen een bepaalde scope altijd is ingesteld met een diagnostische instelling [met behulp van het effect DeployIfNotExists in de beleidsregel](../azure-policy/policy-definition.md#policy-rule). Vandaag de dag wordt DeployIfNotExists alleen ondersteund op de ingebouwde beleid.

## <a name="how-do-i-set-up-guest-os-monitoring-data-to-be-streamed-to-an-event-hub"></a>Hoe stel ik Gast OS bewakingsgegevens in kunnen worden gestreamd naar een event hub?

U moet een agent voor het verzenden van Gast OS bewakingsgegevens in een event hub te installeren. Voor Windows of Linux geeft u de gegevens die u wilt dat moet worden verzonden naar de event hub, evenals de event hub waarnaar de gegevens moeten worden verzonden in een configuratiebestand en configuratiebestand doorgeven aan de agent wordt uitgevoerd op de virtuele machine.

### <a name="stream-linux-data-to-an-event-hub"></a>Stream Linux-gegevens naar een event hub

De [Linux Azure diagnoseagent](../virtual-machines/linux/diagnostic-extension.md) kan worden gebruikt voor het verzenden van gegevens van een Linux-machine naar een event hub bewaking. Dit doen door de event hub toe te voegen als een sink in uw LAD beveiligde instellingen voor het configuratiebestand JSON. [Raadpleeg dit artikel voor meer informatie over het toevoegen van de hub gebeurtenisopvanger aan uw Linux Azure diagnoseagent](../virtual-machines/linux/diagnostic-extension.md#protected-settings).

> [!NOTE]
> U instellen streaming van Gast OS bewakingsgegevens naar een event hub in de portal niet. In plaats daarvan moet u handmatig het configuratiebestand bewerken.

### <a name="stream-windows-data-to-an-event-hub"></a>Windows-gegevens naar een event hub Stream

De [Windows Azure-diagnoseagent](./azure-diagnostics.md) kan worden gebruikt voor het verzenden van het bewaken van gegevens van een Windows-machine naar een event hub. Dit doen door de event hub toe te voegen als een sink in de sectie privateConfig van het configuratiebestand af. [Raadpleeg dit artikel voor meer informatie over het toevoegen van de hub gebeurtenisopvanger aan uw Windows Azure-diagnoseagent](./azure-diagnostics-streaming-event-hubs.md).

> [!NOTE]
> U instellen streaming van Gast OS bewakingsgegevens naar een event hub in de portal niet. In plaats daarvan moet u handmatig het configuratiebestand bewerken.

## <a name="how-do-i-set-up-application-monitoring-data-to-be-streamed-to-event-hub"></a>Hoe stel ik toepassingsbewaking gegevens kunnen worden gestreamd naar event hub

Toepassing bewakingsgegevens is vereist dat uw code is uitgerust met een SDK, zodat er geen een algemene oplossing voor routering application monitoring-gegevensset naar een event hub in Azure. Echter, [Azure Application Insights](../application-insights/app-insights-overview.md) is een service die kan worden gebruikt voor het verzamelen van gegevens van Azure op toepassingsniveau. Als u Application Insights gebruikt, kunt u bewakingsgegevens naar een event hub streamen als volgt:

1. [Instellen van continue export](../application-insights/app-insights-export-telemetry.md) van de Application Insights-gegevens naar een opslagaccount.

2. Een timer geactiveerd logische App ingesteld die [haalt gegevens uit blob storage](../connectors/connectors-create-api-azureblobstorage.md#use-an-action) en [duwt deze als een bericht naar de event hub](../connectors/connectors-create-api-azure-event-hubs.md#send-events-to-your-event-hub-from-your-logic-app).

## <a name="what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub"></a>Wat kan ik doen met de bewakingsgegevens naar Mijn event hub worden verzonden?

De controlegegevens routering naar een event hub met Azure-bewaking, kunt u eenvoudig kunt integreren met partners SIEM en hulpmiddelen voor Netwerkcontrole. De meeste hulpprogramma's moeten de verbindingsreeks van de event hub en bepaalde machtigingen voor uw Azure-abonnement om gegevens te lezen van de event hub. Dit is een niet-uitputtende lijst met hulpprogramma's met Azure Monitor-integratie:

* **IBM QRadar** -DSM van de Microsoft Azure en Microsoft Azure Event Hub-Protocol zijn beschikbaar voor downloaden van [de ondersteuningswebsite IBM](http://www.ibm.com/support). U kunt [meer informatie over de integratie met Azure hier](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0).
* **Splunk** - [de Monitor van Azure-invoegtoepassing voor Splunk](https://splunkbase.splunk.com/app/3534/) is beschikbaar in Splunkbase en een open source-project. [Documentatie is hier](https://github.com/Microsoft/AzureMonitorAddonForSplunk/wiki/Azure-Monitor-Addon-For-Splunk).
* **SumoLogic** -instructies voor het instellen van SumoLogic gebruiken voor gegevens van een event hub zijn [beschikbaar hier](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub)

## <a name="next-steps"></a>Volgende stappen
* [Het activiteitenlogboek naar een opslagaccount archiveren](monitoring-archive-activity-log.md)
* [Lees het overzicht van de Azure Activity Log](monitoring-overview-activity-logs.md)
* [Een waarschuwing op basis van een gebeurtenis activiteitenlogboek instellen](insights-auditlog-to-webhook-email.md)

