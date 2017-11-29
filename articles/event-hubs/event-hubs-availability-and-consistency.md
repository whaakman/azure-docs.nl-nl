---
title: Beschikbaarheid en consistentie in Azure Event Hubs | Microsoft Docs
description: Klik hier voor meer informatie over het opgeven van de maximale hoeveelheid beschikbaarheid en consistentie met Azure Event Hubs met behulp van de partities.
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 8f3637a1-bbd7-481e-be49-b3adf9510ba1
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2017
ms.author: sethm
ms.openlocfilehash: be1398e9b0a10efcd694e46d6322d5d7b9e7a843
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="availability-and-consistency-in-event-hubs"></a>Beschikbaarheid en consistentie in Event Hubs

## <a name="overview"></a>Overzicht
Maakt gebruik van Azure Event Hubs een [partitioneren model](event-hubs-features.md#partitions) voor betere beschikbaarheid en garandeert binnen een enkele event hub. Bijvoorbeeld, als een event hub vier partities heeft en een van deze partities van de ene server naar de andere in een bewerking voor de taakverdeling wordt verplaatst, kunt u nog steeds verzenden en ontvangen van de drie andere partities. Bovendien kunt u meer gelijktijdige lezers verwerken van uw gegevens te verbeteren de geaggregeerde doorvoer hebben meer partities hebben. Inzicht in de gevolgen van het partitioneren en rangschikken in een gedistribueerde systeem is een kritieke aspect van het ontwerp van de oplossing.

Om te helpen de verhouding tussen ordening en beschikbaarheid uitgelegd, Zie de [CAP stelling van](https://en.wikipedia.org/wiki/CAP_theorem), ook wel van Brewer stelling van. Deze stelling van wordt de keuze tussen de consistentie, beschikbaarheid en partitie tolerantie beschreven.

De Brewer stelling van consistentie en beschikbaarheid wordt als volgt gedefinieerd:
* Partitie-tolerantie: de mogelijkheid van een systeem voor gegevensverwerking om door te gaan verwerken van gegevens, zelfs als er een partitie-fout optreedt.
* Beschikbaarheid: een knooppunt niet mislukken retourneert een redelijke antwoord binnen een redelijk tijdsbestek (met geen fouten of time-outs).
* Consistentiecontrole: Lees is gegarandeerd de meest recente schrijven voor een bepaalde client retourneren.

## <a name="partition-tolerance"></a>Partitie tolerantie
Event Hubs is gebaseerd op een gepartitioneerde gegevensmodel. U kunt het aantal partities in de event hub configureren tijdens de installatie, maar u kunt deze waarde later wijzigen. Omdat u partities met Event Hubs gebruiken moet, hebt u een beslissing over de beschikbaarheid en de consistentie van uw toepassing.

## <a name="availability"></a>Beschikbaarheid
De eenvoudigste manier om aan de slag met Event Hubs wordt het standaardgedrag te gebruiken. Als u een nieuwe maakt  **[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient)**  object ingesteld en de  **[verzenden](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.sendasync?view=azure-dotnet#Microsoft_Azure_EventHubs_EventHubClient_SendAsync_Microsoft_Azure_EventHubs_EventData_)**  methode, uw gebeurtenissen worden automatisch gedistribueerd tussen partities in de event hub. Dit gedrag kunt u de grootste hoeveelheid tijd.

Dit model is voor gebruiksvoorbeelden waarvoor de maximale tijd voorkeur.

## <a name="consistency"></a>Consistentie
In sommige gevallen kan kan de ordening van gebeurtenissen belangrijk zijn. U kunt bijvoorbeeld uw back-end-systeem voor het verwerken van een bijwerkopdracht voordat een verwijderopdracht. In dat geval kunt u kunt de partitiesleutel ingesteld op een gebeurtenis, of gebruik een `PartitionSender` object alleen gebeurtenissen verzenden naar een bepaalde partitie. Hiermee zorgt u ervoor dat wanneer deze gebeurtenissen worden gelezen uit de partitie, worden gelezen in volgorde.

Houd er rekening mee dat als de bepaalde partitie die u verzendt niet beschikbaar is, u een foutmelding ontvangt met deze configuratie. Als een punt voor vergelijking, als u nog geen een affiniteit met één partitie, verzendt de Event Hubs-service uw gebeurtenis naar het volgende beschikbare partitie.

Een mogelijke oplossing om ervoor te zorgen ordening tegelijkertijd ook tijd, kan verzamelen van gegevens worden als onderdeel van uw toepassing voor gebeurtenisverwerking. De eenvoudigste manier om dit te bereiken is voor uw gebeurtenis met een aantal aangepaste reeks-eigenschap. De volgende code toont een voorbeeld:

```csharp
// Get the latest sequence number from your application
var sequenceNumber = GetNextSequenceNumber();
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set a custom sequence number property
data.Properties.Add("SequenceNumber", sequenceNumber);
// Send single message async
await eventHubClient.SendAsync(data);
```

In dit voorbeeld uw gebeurtenis verzendt naar een van de beschikbare partities in de event hub en het bijbehorende volgnummer ingesteld van uw toepassing. Deze oplossing status wordt bijgehouden door uw toepassing verwerking vereist, maar uw afzenders biedt een eindpunt dat is het waarschijnlijker beschikbaar.

## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Overzicht van Event Hubs-service](event-hubs-what-is-event-hubs.md)
* [Een Event Hub maken](event-hubs-create.md)
