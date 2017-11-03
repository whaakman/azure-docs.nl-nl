---
title: Azure Event Hubs metrische gegevens in de Azure-Monitor (preview) | Microsoft Docs
description: Azure-bewaking gebruiken voor het bewaken van Event Hubs
services: event-hubs
documentationcenter: .NET
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/19/2017
ms.author: sethm
ms.openlocfilehash: c3ee6a87592145801167b35ff281c04062e525da
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2017
---
# <a name="azure-event-hubs-metrics-in-azure-monitor-preview"></a>Azure Event Hubs metrische gegevens in de Azure-Monitor (preview)

Metrische gegevens van Event Hubs biedt u de status van Event Hubs-resources in uw Azure-abonnement. Met een uitgebreide set met metrische gegevens, kunt u de algemene status van de event hubs niet alleen op het niveau van de naamruimte, maar ook op het entiteitsniveau van de beoordelen. Deze statistische gegevens is belangrijk, omdat ze u helpen bij het controleren van de status van de event hubs. Metrische gegevens kunnen ook helpen problemen hoofdoorzaak zonder contact opnemen met ondersteuning van Azure.

Azure biedt een uniforme gebruikersinterfaces voor het bewaken van alle verschillende Azure-services. Zie voor meer informatie [bewaken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md) en de [metrische gegevens ophalen van Azure-Monitor met .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) op GitHub.

## <a name="access-metrics"></a>Toegang tot metrische gegevens

Azure biedt verschillende manieren met toegang tot metrische gegevens. U kunt de toegang tot metrische gegevens via de [Azure-portal](https://portal.azure.com), of gebruik de Azure-Monitor API's (REST en .NET) en analyse-oplossingen zoals bewerking Management Suite en Event Hubs. Zie voor meer informatie [Azure Monitor metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md#access-metrics-via-the-rest-api).

Metrische gegevens zijn standaard ingeschakeld en u toegang hebt tot de meest recente 30 dagen aan gegevens. Als u gegevens wilt behouden voor een langere periode nodig hebt, kunt u metrische gegevens om een Azure Storage-account te archiveren. Dit is geconfigureerd in [diagnostische instellingen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) in de Azure-Monitor.

## <a name="access-metrics-in-the-portal"></a>Toegang tot metrische gegevens in de portal

U kunt metrische gegevens controleren na verloop van tijd in de [Azure-portal](https://portal.azure.com). Het volgende voorbeeld ziet u hoe geslaagde aanvragen en inkomende aanvragen op het niveau van het account weergeven:

![][1]

U kunt ook toegang tot metrische gegevens rechtstreeks via de naamruimte. Om dit te doen, selecteer de naamruimte en klik vervolgens op **metrische gegevens (Peview)**. Om weer te geven de metrische gegevens die zijn gefilterd op het bereik van de event hub, selecteer de event hub en klik vervolgens op **metrische gegevens (preview)**.

Voor de metrische gegevens voor ondersteuning van dimensies, moet u filteren op met de waarde van de gewenste dimensie zoals weergegeven in het volgende voorbeeld:

![][2]

## <a name="billing"></a>Facturering

Met metrische gegevens in de Azure-Monitor is die momenteel beschikbaar is tijdens het Preview-versie. Echter, als u aanvullende oplossingen die metrische gegevens voor opnemen gebruikt, u mogelijk worden gefactureerd door deze oplossingen. U wordt bijvoorbeeld gefactureerd door Azure Storage als archiveren van metrische gegevens aan een Azure Storage-account. U wordt ook door opnieuw Management Suite (OMS) gefactureerd als stream van metrische gegevens aan OMS voor geavanceerde analyse.

De volgende metrische gegevens geven een overzicht van de status van uw service. 

> [!NOTE]
> Er zijn verschillende metrische gegevens bestandstypen terwijl ze worden verplaatst onder een andere naam. Dit moet u mogelijk uw referenties bijwerken. Metrische gegevens die zijn gemarkeerd met het sleutelwoord 'afgeschaft' wordt niet ondersteund voortaan.

Alle waarden van de metrische gegevens worden verzonden naar Azure Monitor elke minuut. De tijdgranulatie definieert het tijdsinterval waarvoor metrische waarden worden weergegeven. De ondersteunde tijdsinterval voor alle Event Hubs metrische gegevens is 1 minuut.

## <a name="request-metrics"></a>Aanvraag metrische gegevens

Telt het aantal aanvragen voor gegevens en beheer van bewerkingen.

| Metrische naam | Beschrijving |
| ------------------- | ----------------- |
| Inkomende aanvragen (preview) | Het aantal aanvragen voor de service Azure Event Hubs gedurende een bepaalde periode. <br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName |
| Geslaagde aanvragen (preview)   | Het aantal geslaagde aanvragen voor de service Azure Event Hubs gedurende een bepaalde periode. <br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName |
| Server-fouten (preview) | Het aantal aanvragen dat is niet verwerkt vanwege een fout in de Azure Event Hubs-service gedurende een bepaalde periode. <br/><br/>Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName |
|Gebruikersfouten (preview)|Het aantal aanvragen dat is niet verwerkt als gevolg van gebruikersfouten gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName|
|Beperkte aanvragen (preview)|Het aantal aanvragen die zijn beperkt omdat het gebruik van de eenheid doorvoer is overschreden.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName|
|Quota overschreden fouten (preview)|Het aantal aanvragen overschrijdt de beschikbare quota. Zie [in dit artikel](event-hubs-quotas.md) voor meer informatie over Event Hubs-quota.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName|

## <a name="throughput-metrics"></a>Doorvoermetrieken

| Metrische naam | Beschrijving |
| ------------------- | ----------------- |
|Beperkte aanvragen (preview)|Het aantal aanvragen die zijn beperkt omdat het gebruik van de eenheid doorvoer is overschreden.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName|

## <a name="message-metrics"></a>Bericht metrische gegevens

| Metrische naam | Beschrijving |
| ------------------- | ----------------- |
|Binnenkomende berichten (preview)|Het aantal gebeurtenissen of berichten die worden verzonden naar Event Hubs gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName|
|Uitgaande berichten (preview)|Het aantal gebeurtenissen of berichten die zijn opgehaald uit Event Hubs gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName|
|Binnenkomende Bytes (preview)|Het aantal bytes dat is verzonden naar de Azure Event Hubs-service gedurende een bepaalde periode.<br/><br/> Eenheid: Bytes <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName|
|Uitgaande Bytes (preview)|Het aantal bytes opgehaald uit de service Azure Event Hubs gedurende een bepaalde periode.<br/><br/> Eenheid: Bytes <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName|

## <a name="connection-metrics"></a>Metrische gegevens voor verbinding

| Metrische naam | Beschrijving |
| ------------------- | ----------------- |
|ActiveConnections (preview)|Het aantal actieve verbindingen voor een naamruimte, evenals op een entiteit.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName|
|Verbindingen Opened (preview)|Het aantal geopende verbindingen.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName|
|Verbindingen gesloten (preview)|Het aantal verbindingen afgesloten.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName|

## <a name="event-hubs-capture-metrics"></a>Event Hubs vastleggen metrische gegevens

U kunt Event Hubs vastleggen metrische gegevens controleren wanneer u de functie vastleggen voor uw event hubs inschakelen. De volgende metrische gegevens wordt beschreven wat u kunt bewaken met vastleggen is ingeschakeld.

| Metrische naam | Beschrijving |
| ------------------- | ----------------- |
|Achterstand (Preview) vastleggen|Het aantal bytes die nog moeten worden vastgelegd met de gekozen bestemming.<br/><br/> Eenheid: Bytes <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName|
|Vastgelegde berichten (Preview)|Het aantal berichten of gebeurtenissen die worden vastgelegd met de gekozen bestemming gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName|
|Vastgelegde Bytes (Preview)|Het aantal bytes die worden vastgelegd met de gekozen bestemming gedurende een bepaalde periode.<br/><br/> Eenheid: Bytes <br/> Samenvoegingstype: totaal <br/> Dimensie: EntityName|

## <a name="metrics-dimensions"></a>Metrische gegevens dimensies

Azure Event Hubs ondersteunt de volgende dimensies voor metrische gegevens in Azure-Monitor. Dimensies toevoegen aan uw metrische gegevens is optioneel. Als u dimensies niet toevoegt, worden de metrische gegevens opgegeven op het niveau van de naamruimte. 

| Metrische naam | Beschrijving |
| ------------------- | ----------------- |
|entityName| Event Hubs biedt ondersteuning voor de event hub-entiteiten onder de naamruimte.|

## <a name="next-steps"></a>Volgende stappen

* Zie de [Azure bewakingsoverzicht](../monitoring-and-diagnostics/monitoring-overview.md).
* [Ophalen van de metrische gegevens voor Azure-Monitor met .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) op GitHub. 

Voor meer informatie over Event Hubs gaat u naar de volgende koppelingen:

* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor1.png
[2]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor2.png



