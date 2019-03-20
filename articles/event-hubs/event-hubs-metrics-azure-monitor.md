---
title: Metrische gegevens in Azure Monitor - Azure Eventhubs | Microsoft Docs
description: Dit artikel bevat informatie over het gebruik van de Azure Monitoring voor het bewaken van Azure Event Hubs
services: event-hubs
documentationcenter: .NET
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 268b94cb77c238c0d6e0d3fe51a059e3a6602c80
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58003159"
---
# <a name="azure-event-hubs-metrics-in-azure-monitor"></a>Metrische gegevens van Azure Event Hubs in Azure Monitor

Metrische gegevens van Event Hubs biedt u de status van Event Hubs-bronnen in uw Azure-abonnement. Met een uitgebreide set metrische gegevens, kunt u de algemene status van uw eventhubs niet alleen op het niveau van de naamruimte, maar ook op het entiteitsniveau van de beoordelen. Deze statistische gegevens is belangrijk, omdat ze u houden op de status van uw eventhubs. Metrische gegevens kunnen ook helpen problemen hoofdoorzaak zonder contact opnemen met ondersteuning van Azure.

Azure Monitor biedt een uniforme gebruikersinterfaces voor bewaking over de verschillende Azure-services. Zie voor meer informatie, [bewaken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md) en de [ophalen van Azure Monitor metrics met .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) op GitHub.

## <a name="access-metrics"></a>Toegang tot metrische gegevens

Azure Monitor biedt meerdere manieren voor toegang tot metrische gegevens. U kunt de toegang tot metrische gegevens via de [Azure-portal](https://portal.azure.com), of gebruik de Azure Monitor API's (REST en .NET) en oplossingen voor gegevensanalyse, zoals operations Management Suite en Event Hubs. Zie voor meer informatie, [door gegevens te controleren die worden verzameld door Azure Monitor](../azure-monitor/platform/data-collection.md).

Metrische gegevens zijn standaard ingeschakeld en u hebt toegang tot gegevens van de meest recente 30 dagen. Als u behouden van gegevens voor een langere periode wilt, kunt u metrische gegevens om een Azure Storage-account te archiveren. Dit is geconfigureerd in [diagnostische instellingen](../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings) in Azure Monitor.

## <a name="access-metrics-in-the-portal"></a>Toegang tot metrische gegevens in de portal

U kunt metrische gegevens controleren na verloop van tijd in de [Azure-portal](https://portal.azure.com). Het volgende voorbeeld laat zien hoe om binnenkomende aanvragen op accountniveau en geslaagde aanvragen weer te geven:

![Geslaagde metrische gegevens weergeven][1]

U kunt ook toegang tot metrische gegevens rechtstreeks via de naamruimte. Om dit te doen, selecteert u uw naamruimte en klik vervolgens op **metrische gegevens**. Als u wilt weergeven van metrische gegevens die zijn gefilterd op het bereik van de event hub, selecteer de event hub en klik vervolgens op **metrische gegevens**.

Voor metrische gegevens voor ondersteuning van dimensies, moet u filteren met de waarde van de gewenste dimensie zoals wordt weergegeven in het volgende voorbeeld:

![Filteren met de dimensiewaarde][2]

## <a name="billing"></a>Billing

Met metrische gegevens in Azure Monitor is momenteel gratis. Echter, als u aanvullende oplossingen die metrische gegevens opnemen, u mogelijk worden kosten in rekening gebracht door deze oplossingen. U wordt bijvoorbeeld gefactureerd door Azure Storage als u metrische gegevens om een Azure Storage-account te archiveren. U wordt ook gefactureerd door Azure, als u metrische gegevens naar Azure Monitor-logboeken voor geavanceerde analyse streamen.

De volgende metrische gegevens geven u een overzicht van de status van uw service. 

> [!NOTE]
> Er zijn verschillende metrische gegevens niet meer ondersteund als ze worden verplaatst onder een andere naam. Dit moet u mogelijk uw referenties bijwerken. Metrische gegevens die zijn gemarkeerd met het sleutelwoord 'afgeschaft' wordt niet ondersteund voortaan.

Alle metrische waarden worden verzonden naar Azure Monitor elke minuut. De tijdgranulatie definieert het tijdsinterval waarvoor metrische waarden worden weergegeven. Het ondersteunde tijdsinterval voor alle Event Hubs-metrische gegevens is 1 minuut.

## <a name="request-metrics"></a>Aanvraag voor metrische gegevens

Telt het aantal aanvragen voor beheer van gegevens en bewerkingen.

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
| Inkomende aanvragen (preview) | Het aantal aanvragen voor de Azure Event Hubs-service gedurende een bepaalde periode. <br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName |
| Geslaagde aanvragen (preview)   | Het aantal geslaagde aanvragen voor de Azure Event Hubs-service gedurende een bepaalde periode. <br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName |
| Server-fouten (preview) | Het aantal aanvragen die niet worden verwerkt vanwege een fout in de Azure Event Hubs-service gedurende een bepaalde periode. <br/><br/>Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName |
|Gebruikersfouten (preview)|Het aantal aanvragen die niet worden verwerkt wegens gebruikersfouten gedurende een bepaalde periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|Quotum overschreden fouten (preview)|Het aantal aanvragen van overschreden het beschikbare quotum. Zie [in dit artikel](event-hubs-quotas.md) voor meer informatie over Event Hubs-quota.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|

## <a name="throughput-metrics"></a>Metrische gegevens over doorvoer

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
|Beperkte aanvragen (preview)|Het aantal aanvragen die zijn beperkt omdat het gebruik van de eenheid doorvoer is overschreden.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|

## <a name="message-metrics"></a>Bericht metrische gegevens

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
|Binnenkomende berichten (preview)|Het aantal gebeurtenissen of berichten die worden verzonden naar Event Hubs in een opgegeven periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|Uitgaande berichten (preview)|Het aantal gebeurtenissen of berichten ophalen uit Event Hubs in een opgegeven periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|Binnenkomende Bytes (preview)|Het aantal bytes dat is verzonden naar de Azure Event Hubs-service gedurende een bepaalde periode.<br/><br/> Eenheid: Bytes <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|Uitgaande Bytes (preview)|Het aantal bytes opgehaald vanuit de Azure Event Hubs-service gedurende een bepaalde periode.<br/><br/> Eenheid: Bytes <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|

## <a name="connection-metrics"></a>Metrische verbindingsgegevens

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
|ActiveConnections (preview)|Het aantal actieve verbindingen voor een naamruimte, maar ook op een entiteit.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|Verbindingen geopend (preview)|Het aantal open verbindingen.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|Gesloten verbindingen (preview)|Het aantal gesloten verbindingen.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|

## <a name="event-hubs-capture-metrics"></a>Event Hubs Capture metrische gegevens

U kunt Event Hubs Capture metrische gegevens controleren wanneer u de Capture-functie voor uw eventhubs inschakelt. De volgende metrische gegevens wordt beschreven wat u kunt controleren met vastleggen is ingeschakeld.

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
|Achterstand (Preview) vastleggen|Het aantal bytes die nog moeten worden vastgelegd in het geselecteerde doel.<br/><br/> Eenheid: Bytes <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|Vastgelegde berichten (Preview)|Het aantal berichten of gebeurtenissen die zijn vastgelegd op de gekozen bestemming gedurende een bepaalde periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|Vastgelegde Bytes (Preview)|Het aantal bytes dat voor de gekozen bestemming gedurende een bepaalde periode worden vastgelegd.<br/><br/> Eenheid: Bytes <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|

## <a name="metrics-dimensions"></a>Metrische gegevens over dimensies

Azure Event Hubs ondersteunt de volgende dimensies voor metrische gegevens in Azure Monitor. Dimensies toevoegen aan uw metrische gegevens is optioneel. Als u dimensies niet toevoegt, worden de metrische gegevens opgegeven op het niveau van de naamruimte. 

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
|EntityName| Eventhubs biedt ondersteuning voor de event hub-entiteiten in de naamruimte.|

## <a name="next-steps"></a>Volgende stappen

* Zie de [overzicht Azure Monitoring](../monitoring-and-diagnostics/monitoring-overview.md).
* [Ophalen van Azure Monitor metrics met .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) op GitHub. 

Voor meer informatie over Event Hubs gaat u naar de volgende koppelingen:

* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor1.png
[2]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor2.png



