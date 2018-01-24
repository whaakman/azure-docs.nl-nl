---
title: Azure Relay metrische gegevens in de Azure-Monitor (preview) | Microsoft Docs
description: Azure-bewaking gebruiken voor het bewaken van Azure Relay
services: service-bus-relay
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: sethm
ms.openlocfilehash: dd5bc0dd0088856954e06d880f2c03f0a74ed9d7
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-relay-metrics-in-azure-monitor-preview"></a>Azure Relay metrische gegevens in de Azure-Monitor (preview)

Azure metrische gegevens van Relay biedt u de status van resources in uw Azure-abonnement. Met een uitgebreide set met metrische gegevens, kunt u de algemene status van uw resources Relay, niet alleen op het niveau van de naamruimte, maar ook op het entiteitsniveau van de beoordelen. Deze statistische gegevens is belangrijk, omdat ze u helpen bij het controleren van de status van Azure Relay. Metrische gegevens kunnen ook helpen problemen hoofdoorzaak zonder contact opnemen met ondersteuning van Azure.

Azure biedt een uniforme gebruikersinterfaces voor het bewaken van alle verschillende Azure-services. Zie voor meer informatie [bewaken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md) en de [metrische gegevens ophalen van Azure-Monitor met .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) op GitHub.

## <a name="access-metrics"></a>Toegang tot metrische gegevens

Azure biedt verschillende manieren met toegang tot metrische gegevens. U kunt de toegang tot metrische gegevens via de [Azure-portal](https://portal.azure.com), of gebruik de Azure-Monitor API's (REST en .NET) en analyse-oplossingen zoals bewerking Management Suite en Event Hubs. Zie voor meer informatie [Azure Monitor metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md#access-metrics-via-the-rest-api).

Metrische gegevens zijn standaard ingeschakeld en u toegang hebt tot de meest recente 30 dagen aan gegevens. Als u gegevens wilt behouden voor een langere periode nodig hebt, kunt u metrische gegevens om een Azure Storage-account te archiveren. Dit is geconfigureerd in [diagnostische instellingen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) in de Azure-Monitor.

## <a name="access-metrics-in-the-portal"></a>Toegang tot metrische gegevens in de portal

U kunt metrische gegevens controleren na verloop van tijd in de [Azure-portal](https://portal.azure.com). Het volgende voorbeeld ziet u hoe geslaagde aanvragen en inkomende aanvragen op het niveau van het account weergeven:

![][1]

U kunt ook toegang tot metrische gegevens rechtstreeks via de naamruimte. Om dit te doen, selecteer de naamruimte en klik vervolgens op **metrische gegevens (Peview)**. 

Voor de metrische gegevens voor ondersteuning van dimensies, moet u met de waarde van de gewenste dimensie filteren.

## <a name="billing"></a>Facturering

Met metrische gegevens in de Azure-Monitor is die momenteel beschikbaar is tijdens het Preview-versie. Echter, als u aanvullende oplossingen die metrische gegevens voor opnemen gebruikt, u mogelijk worden gefactureerd door deze oplossingen. U wordt bijvoorbeeld gefactureerd door Azure Storage als archiveren van metrische gegevens aan een Azure Storage-account. U wordt ook door opnieuw Management Suite (OMS) gefactureerd als stream van metrische gegevens aan OMS voor geavanceerde analyse.

De volgende metrische gegevens geven een overzicht van de status van uw service. 

> [!NOTE]
> Er zijn verschillende metrische gegevens bestandstypen terwijl ze worden verplaatst onder een andere naam. Dit moet u mogelijk uw referenties bijwerken. Metrische gegevens die zijn gemarkeerd met het sleutelwoord 'afgeschaft' wordt niet ondersteund voortaan.

Alle waarden van de metrische gegevens worden verzonden naar Azure Monitor elke minuut. De tijdgranulatie definieert het tijdsinterval waarvoor metrische waarden worden weergegeven. De ondersteunde tijdsinterval voor alle Azure-Relay metrische gegevens is 1 minuut.

## <a name="connection-metrics"></a>Metrische gegevens voor verbinding

| Naam van meetwaarde | Beschrijving |
| ------------------- | ----------------- |
| ListenerConnections-geslaagd (preview) | Het aantal geslaagde listener verbindingen met Azure Relay gedurende een bepaalde periode. <br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimension: EntityName|
|ListenerConnections-ClientError (preview)|Het aantal fouten van de client op de listener-verbindingen gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimension: EntityName|
|ListenerConnections-ServerError (preview)|Het nummer van de server-fouten van de listener-verbindingen gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimension: EntityName|
|SenderConnections-geslaagd (preview)|Het aantal geslaagde afzender verbindingen gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimension: EntityName|
|SenderConnections-ClientError (preview)|Het aantal fouten van de client op de afzender-verbindingen gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimension: EntityName|
|SenderConnections-ServerError (preview)|Het aantal serverfouten op de afzender-verbindingen gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimension: EntityName|
|ListenerConnections-TotalRequests (preview)|Het totale aantal listener-verbindingen gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimension: EntityName|
|SenderConnections-TotalRequests (preview)|De verbindingsaanvragen door de afzenders gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimension: EntityName|
|ActiveConnections (preview)|Het aantal actieve verbindingen gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimension: EntityName|
|ActiveListeners (preview)|Het aantal actieve listeners gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimension: EntityName|
|ListenerDisconnects (preview)|Het aantal niet-verbonden listeners gedurende een bepaalde periode.<br/><br/> Eenheid: Bytes <br/> Samenvoegingstype: totaal <br/> Dimension: EntityName|
|SenderDisconnects (preview)|Het aantal niet-verbonden afzenders gedurende een bepaalde periode.<br/><br/> Eenheid: Bytes <br/> Samenvoegingstype: totaal <br/> Dimension: EntityName|

## <a name="memory-usage-metrics"></a>Meetgegevens voor softwaregebruik geheugen

| Naam van meetwaarde | Beschrijving |
| ------------------- | ----------------- |
|BytesTransferred (preview)|Het aantal bytes overgedragen gedurende een bepaalde periode.<br/><br/> Eenheid: Bytes <br/> Samenvoegingstype: totaal <br/> Dimension: EntityName|

## <a name="metrics-dimensions"></a>Metrische gegevens dimensies

Azure Relay ondersteunt de volgende dimensies voor metrische gegevens in Azure-Monitor. Dimensies toevoegen aan uw metrische gegevens is optioneel. Als u dimensies niet toevoegt, worden de metrische gegevens opgegeven op het niveau van de naamruimte. 

|De dimensienaam van de|Beschrijving|
| ------------------- | ----------------- |
|EntityName| Azure Relay ondersteunt berichtentiteiten onder de naamruimte.|

## <a name="next-steps"></a>Volgende stappen

Zie de [Azure bewakingsoverzicht](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/relay-metrics-azure-monitor/relay-monitor1.png




