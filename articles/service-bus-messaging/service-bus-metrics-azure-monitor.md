---
title: Metrische gegevens van Azure Service Bus in Azure Monitor (preview) | Microsoft Docs
description: Bewaking van Azure gebruiken voor het bewaken van Service Bus-entiteiten
services: service-bus-messaging
documentationcenter: .NET
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 05/31/2018
ms.author: spelluru
ms.openlocfilehash: d141ac284b1ed8e128588e1a49a0594e0c61707f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46974048"
---
# <a name="azure-service-bus-metrics-in-azure-monitor-preview"></a>Metrische gegevens van Azure Service Bus in Azure Monitor (preview)

Metrische gegevens over Service Bus biedt u de status van resources in uw Azure-abonnement. Met een uitgebreide set metrische gegevens, kunt u de algemene status van uw Service Bus-resources, niet alleen op het niveau van de naamruimte, maar ook op het entiteitsniveau van de beoordelen. Deze statistische gegevens is belangrijk, omdat ze u houden op de status van Service Bus. Metrische gegevens kunnen ook helpen problemen hoofdoorzaak zonder contact opnemen met ondersteuning van Azure.

Azure Monitor biedt een uniforme gebruikersinterfaces voor bewaking over de verschillende Azure-services. Zie voor meer informatie, [bewaken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md) en de [ophalen van Azure Monitor metrics met .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) op GitHub.

> [!IMPORTANT]
> Wanneer er interactie met een entiteit gedurende 2 uur niet is, kan de metrische gegevens wordt gestart "0" als een waarde weergegeven tot de entiteit niet meer actief is.

## <a name="access-metrics"></a>Toegang tot metrische gegevens

Azure Monitor biedt meerdere manieren voor toegang tot metrische gegevens. U kunt de toegang tot metrische gegevens via de [Azure-portal](https://portal.azure.com), of gebruik de Azure Monitor API's (REST en .NET) en oplossingen voor gegevensanalyse, zoals Log Analytics en Event Hubs. Zie voor meer informatie, [door gegevens te controleren die worden verzameld door Azure Monitor](../monitoring/monitoring-data-collection.md).

Metrische gegevens zijn standaard ingeschakeld en u hebt toegang tot gegevens van de meest recente 30 dagen. Als u behouden van gegevens voor een langere periode wilt, kunt u metrische gegevens om een Azure Storage-account te archiveren. Dit is geconfigureerd in [diagnostische instellingen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) in Azure Monitor.

## <a name="access-metrics-in-the-portal"></a>Toegang tot metrische gegevens in de portal

U kunt metrische gegevens controleren na verloop van tijd in de [Azure-portal](https://portal.azure.com). Het volgende voorbeeld laat zien hoe om binnenkomende aanvragen op accountniveau en geslaagde aanvragen weer te geven:

![][1]

U kunt ook toegang tot metrische gegevens rechtstreeks via de naamruimte. Om dit te doen, selecteert u uw naamruimte en klik vervolgens op **metrische gegevens (Peview)**. Als u wilt weergeven van metrische gegevens die zijn gefilterd op het bereik van de entiteit, selecteer de entiteit en klik vervolgens op **metrische gegevens (preview)**.

![][2]

Voor metrische gegevens voor ondersteuning van dimensies, moet u filteren met de gewenste dimensiewaarde.

## <a name="billing"></a>Billing

Met metrische gegevens in Azure Monitor is gratis tijdens preview. Echter, als u aanvullende oplossingen die metrische gegevens opnemen, u mogelijk worden kosten in rekening gebracht door deze oplossingen. U wordt bijvoorbeeld gefactureerd door Azure Storage als u metrische gegevens om een Azure Storage-account te archiveren. U wordt ook gefactureerd door Log Analytics, als u metrische gegevens naar Log Analytics voor geavanceerde analyse streamen.

De volgende metrische gegevens geven u een overzicht van de status van uw service. 

> [!NOTE]
> Er zijn verschillende metrische gegevens niet meer ondersteund als ze worden verplaatst onder een andere naam. Dit moet u mogelijk uw referenties bijwerken. Metrische gegevens die zijn gemarkeerd met het sleutelwoord 'afgeschaft' wordt niet ondersteund voortaan.

Alle metrische waarden worden verzonden naar Azure Monitor elke minuut. De tijdgranulatie definieert het tijdsinterval waarvoor metrische waarden worden weergegeven. Het ondersteunde tijdsinterval voor alle Service Bus-metrische gegevens is 1 minuut.

## <a name="request-metrics"></a>Aanvraag voor metrische gegevens

Telt het aantal aanvragen voor beheer van gegevens en bewerkingen.

| Naam van meetwaarde | Beschrijving |
| ------------------- | ----------------- |
| Inkomende aanvragen (preview) | Het aantal aanvragen voor de Service Bus-service gedurende een bepaalde periode. <br/><br/> Eenheid: aantal <br/> Aggregatietype: totaal <br/> Dimensie: EntityName|
|Geslaagde aanvragen (preview)|Het aantal geslaagde aanvragen voor de Service Bus-service gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Aggregatietype: totaal <br/> Dimensie: EntityName|
|Server-fouten (preview)|Het aantal aanvragen die niet worden verwerkt vanwege een fout in de Service Bus-service gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Aggregatietype: totaal <br/> Dimensie: EntityName|
|Gebruikersfouten (preview - Zie de volgende subsectie)|Het aantal aanvragen die niet worden verwerkt wegens gebruikersfouten gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Aggregatietype: totaal <br/> Dimensie: EntityName|
|Beperkte aanvragen (preview)|Het aantal aanvragen die zijn beperkt omdat het gebruik is overschreden.<br/><br/> Eenheid: aantal <br/> Aggregatietype: totaal <br/> Dimensie: EntityName|

### <a name="user-errors"></a>Gebruikersfouten

De volgende twee typen fouten zijn geclassificeerd als gebruikersfouten:

1. Client side-fouten (In HTTP die 400 fouten).
2. Fouten die tijdens het verwerken van berichten, zoals optreden [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception).


## <a name="message-metrics"></a>Bericht metrische gegevens

| Naam van meetwaarde | Beschrijving |
| ------------------- | ----------------- |
|Binnenkomende berichten (preview)|Het aantal gebeurtenissen of berichten die naar Service Bus wordt verzonden in een opgegeven periode.<br/><br/> Eenheid: aantal <br/> Aggregatietype: totaal <br/> Dimensie: EntityName|
|Uitgaande berichten (preview)|Het aantal gebeurtenissen of berichten ontvangen van Service Bus gedurende een bepaalde periode.<br/><br/> Eenheid: aantal <br/> Aggregatietype: totaal <br/> Dimensie: EntityName|

## <a name="connection-metrics"></a>Metrische verbindingsgegevens

| Naam van meetwaarde | Beschrijving |
| ------------------- | ----------------- |
|ActiveConnections (preview)|Het aantal actieve verbindingen voor een naamruimte, maar ook op een entiteit.<br/><br/> Eenheid: aantal <br/> Aggregatietype: totaal <br/> Dimensie: EntityName|
|Verbindingen geopend (preview)|Het aantal open verbindingen.<br/><br/> Eenheid: aantal <br/> Aggregatietype: totaal <br/> Dimensie: EntityName|
|Gesloten verbindingen (preview)|Het aantal gesloten verbindingen.<br/><br/> Eenheid: aantal <br/> Aggregatietype: totaal <br/> Dimensie: EntityName |

## <a name="resource-usage-metrics"></a>Metrische gegevens voor het gebruik van resources

| Naam van meetwaarde | Beschrijving |
| ------------------- | ----------------- |
|CPU-gebruik per naamruimte (preview)|Het percentage CPU-gebruik van de naamruimte.<br/><br/> Eenheid: procent <br/> Aggregatietype: Maximum <br/> Dimensie: EntityName|
|Geheugengebruik per naamruimte (preview)|Het geheugengebruik van het percentage van de naamruimte.<br/><br/> Eenheid: procent <br/> Aggregatietype: Maximum <br/> Dimensie: EntityName|

## <a name="metrics-dimensions"></a>Metrische gegevens over dimensies

Azure Service Bus ondersteunt de volgende dimensies voor metrische gegevens in Azure Monitor. Dimensies toevoegen aan uw metrische gegevens is optioneel. Als u dimensies niet toevoegt, worden de metrische gegevens opgegeven op het niveau van de naamruimte. 

|Dimensienaam|Beschrijving|
| ------------------- | ----------------- |
|EntityName| Service Bus ondersteunt berichtentiteiten onder de naamruimte.|

## <a name="next-steps"></a>Volgende stappen

Zie de [overzicht Azure Monitoring](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor1.png
[2]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor2.png


