---
title: Azure Security gegevens exporteren naar SIEM - Pipeline Configuration [Preview] | Microsoft Docs
description: In dit artikel wordt het produceren van het ophalen van Azure security center-Logboeken in een SIEM
services: security-center
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: barclayn
ms.openlocfilehash: aef623f047bd7e14cb5bd17fb2a2c18e3c5d42b9
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="azure-security-data-export-to-siem--pipeline-configuration-preview"></a>Azure Security gegevens exporteren naar SIEM - Pipeline Configuration [Preview]

In dit document worden de procedure voor het Azure Beveiligingscentrum beveiligingsgegevens exporteren naar een SIEM.

Verwerkte gebeurtenissen die zijn geproduceerd door Azure Security Center worden gepubliceerd naar de Azure [activiteitenlogboek](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), een van het logboek bestandstypen die beschikbaar is via de Azure-Monitor. Azure biedt een geconsolideerde pijplijn voor het doorsturen van een van de controlegegevens in een SIEM-hulpprogramma. Dit wordt gedaan door het streamen van die gegevens naar een Event Hub waarin deze kan vervolgens worden opgevraagd in een partner-hulpprogramma.

Deze pipe gebruikt de [één pijplijn Azure Monitoring](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) voor het verkrijgen van toegang aan de bewakingsgegevens van uw Azure-omgeving. Hiermee kunt u eenvoudig instellen van siem's en hulpmiddelen voor Netwerkcontrole gebruiken voor de gegevens.

De volgende secties wordt beschreven hoe u gegevens kunnen worden gestreamd naar een event hub kunt configureren. De stappen wordt ervan uitgegaan dat u al geconfigureerd in uw Azure-abonnement van Azure Security Center hebt.

Overzicht

![Overzicht op hoog niveau](media/security-center-export-data-to-siem/overview.png)

## <a name="what-is-the-azure-security-data-exposed-to-siem"></a>Wat zijn de gegevens van de Azure-beveiliging blootgesteld aan SIEM?

In deze preview-versie geven we de [beveiligingswaarschuwingen.](../security-center/security-center-managing-and-responding-alerts.md) In toekomstige versies zullen we de gegevensset met aanbevelingen voor beveiliging aanvullen.

## <a name="how-to-setup-the-pipeline"></a>Het instellen van de pijplijn? 

### <a name="create-an-event-hub"></a>Een Event Hub maken 

Voordat u begint, moet u [maken van een naamruimte Event Hubs](../event-hubs/event-hubs-create.md). Deze naamruimte en Event Hub is de bestemming voor de controlegegevens.

### <a name="stream-the-azure-activity-log-to-event-hubs"></a>Stream de Azure Activity Log naar Event Hubs

Raadpleeg het volgende artikel [activiteitenlogboek stream naar Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md)

### <a name="install-a-partner-siem-connector"></a>Een partner SIEM-connector installeren 

De controlegegevens routering naar een Event Hub met Azure-bewaking, kunt u eenvoudig kunt integreren met partners SIEM en hulpmiddelen voor Netwerkcontrole.

Raadpleeg de volgende koppeling voor een overzicht van [siem's ondersteund](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)

## <a name="example-for-querying-data"></a>Voorbeeld voor het opvragen van gegevens 

Hier volgt een aantal Splunk-query's die u kunt gebruiken voor het ophalen van waarschuwingsgegevens:

| **Beschrijving van de Query**                                | **Query**                                                                                                                              |
|---------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Alle waarschuwingen                                              | index belangrijkste Microsoft.Security/locations/alerts =                                                                                         |
| Het aantal bewerkingen door hun naam samenvatten             | **Waarschuwingen** index belangrijkste sourcetype = = "amal: beveiliging ' \| tabel operationName \| operationName stat tellen                                |
| Waarschuwingen info ophalen: tijd, naam, status, -ID en -abonnement | index belangrijkste Microsoft.Security/locations/alerts = \| tabel \_time, properties.eventName, status, properties.operationId, am_subscriptionId |


## <a name="next-steps"></a>Volgende stappen

- [Ondersteunde siem 's](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)
- [Activiteitenlogboek streamen naar Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md)
- [Beveiligingswaarschuwingen.](../security-center/security-center-managing-and-responding-alerts.md)