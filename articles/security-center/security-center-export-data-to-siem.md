---
title: Azure Security-gegevens exporteren naar SIEM - Pipeline Configuration [Preview] | Microsoft Docs
description: In dit artikel wordt het produceren van het ophalen van Azure security center-logboeken naar een SIEM
services: security-center
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2018
ms.author: barclayn
ms.openlocfilehash: aede60a729fe9c0594ded485e189c0b467e34271
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44298230"
---
# <a name="azure-security-data-export-to-siem--pipeline-configuration-preview"></a>Azure Security-gegevens exporteren naar SIEM - Pipeline Configuration [Preview]

In dit document worden de procedure voor het exporteren van Azure Security Center security gegevens naar een SIEM.

Verwerkte gebeurtenissen die worden geproduceerd door Azure Security Center worden gepubliceerd naar de Azure [activiteitenlogboek](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), een van het logboek voor bestandstypen die beschikbaar zijn via Azure Monitor. Azure Monitor biedt een geconsolideerde pijplijn voor de routering van uw bewakingsgegevens naar een SIEM-hulpprogramma. Dit wordt gedaan door het streamen van die gegevens naar een Event Hub waar deze kan vervolgens worden opgehaald naar een partner-hulpprogramma.

Maakt gebruik van deze pipe de [één pijplijn Azure Monitoring](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) voor het verkrijgen van toegang aan de bewakingsgegevens uit uw Azure-omgeving. Hiermee kunt u eenvoudig instellen van siem's en controleprogramma's gebruiken voor de gegevens.

De volgende secties wordt beschreven hoe u gegevens kunnen worden gestreamd naar een event hub kunt configureren. De stappen wordt ervan uitgegaan dat u al geconfigureerd in uw Azure-abonnement van Azure Security Center hebt.

Overzicht

![Overzicht op hoog niveau](media/security-center-export-data-to-siem/overview.png)

## <a name="what-is-the-azure-security-data-exposed-to-siem"></a>Wat zijn de gegevens van de Azure-beveiliging blootgesteld aan SIEM?

In deze preview-versie stellen we de [beveiligingswaarschuwingen.](../security-center/security-center-managing-and-responding-alerts.md) In toekomstige versies zullen we de gegevensset met aanbevelingen voor beveiliging verrijken.

## <a name="how-to-setup-the-pipeline"></a>Het instellen van de pijplijn? 

### <a name="create-an-event-hub"></a>Een Event Hub maken 

Voordat u begint, moet u [maken van een Event Hubs-naamruimte](../event-hubs/event-hubs-create.md). Deze naamruimte en Event Hub is de bestemming voor al uw bewakingsgegevens.

### <a name="stream-the-azure-activity-log-to-event-hubs"></a>Stream het Azure-activiteitenlogboek naar Eventhubs

Raadpleeg het volgende artikel [activiteitenlogboek streamen naar Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md)

### <a name="install-a-partner-siem-connector"></a>Een partner SIEM-connector installeren 

Routering van uw bewakingsgegevens naar een Event Hub met Azure Monitor kunt u eenvoudig kunt integreren met SIEM-partner- en controlehulpprogramma's.

Raadpleeg de volgende koppeling om te zien van de lijst met [ondersteunde siem's](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)

## <a name="example-for-querying-data"></a>Voorbeeld voor het opvragen van gegevens 

Hier volgt een aantal Splunk-query's die u kunt gebruiken voor het ophalen van waarschuwingsgegevens:

| **Beschrijving van Query**                                | **Query**                                                                                                                              |
|---------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Alle waarschuwingen                                              | index = belangrijkste Microsoft.Security/locations/alerts                                                                                         |
| Aantal bewerkingen met hun naam samenvatten             | index = belangrijkste sourcetype = "amal: security" \| tabel operationName \| statistieken operationName tellen                                |
| Informatie over waarschuwingen: tijd, naam, status, -ID en -abonnement | index = belangrijkste Microsoft.Security/locations/alerts \| tabel \_tijd, properties.eventName, status, properties.operationId, am_subscriptionId |


## <a name="next-steps"></a>Volgende stappen

- [Ondersteunde siem 's](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)
- [Activiteitenlogboek streamen naar Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md)
- [Beveiligingswaarschuwingen.](../security-center/security-center-managing-and-responding-alerts.md)