---
title: Voorbeelden van Azure Event Hubs | Microsoft Docs
description: Azure Event Hubs-voorbeelden
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: sethm
ms.openlocfilehash: a581b7039a3631b7f1dc35816175242f892bd7dd
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="event-hubs-samples"></a>Voorbeelden van Event Hubs 

De set van Azure Event Hubs-voorbeelden worden de belangrijkste functies in [Azure Event Hubs](/azure/event-hubs/). In dit artikel, ingedeeld en beschrijft de voorbeelden die beschikbaar is, met koppelingen naar elk.

Op het moment van schrijven van dit zijn voorbeelden van Event Hubs bevinden zich op verschillende plaatsen:

- [MSDN developer-codevoorbeelden](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples)

Zie voor meer informatie over de verschillende versies van .NET Framework [Frameworks en doelen](/dotnet/articles/standard/frameworks).

Meer voorbeelden worden toegevoegd na verloop van tijd, dus Kijk regelmatig op updates.

## <a name="net-standard"></a>.NET Standard

De volgende voorbeelden laten zien hoe u verzenden en ontvangen van gebeurtenissen met de [Event Hubs-client](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) voor de [standaard .NET-bibliotheek](/dotnet/articles/standard/library).

### <a name="send-events"></a>Gebeurtenissen verzenden 

De [verzenden aan de slag](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) voorbeeld laat zien hoe u schrijft een .NET Core-consoletoepassing die gebeurtenissen naar een event hub verzendt.

### <a name="receive-events"></a>Gebeurtenissen ontvangen 

De [aan de slag met het Event Processor Host ontvangen](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) voorbeeld is een .NET Core-consoletoepassing die berichten ontvangt van een event hub met behulp van de Event Processor Host.

## <a name="net-framework"></a>.NET Framework   

Deze voorbeelden demonstreren verschillende andere functies van Azure Event Hubs, die gericht is op de [.NET Framework-bibliotheek](/dotnet/framework/index).
 
### <a name="notify-users-of-events-received"></a>Waarschuw gebruikers van gebeurtenissen ontvangen

De [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) voorbeeld waarschuwt gebruikers gegevens ontvangen van sensoren of andere systemen.

### <a name="get-started-with-event-hubs"></a>Aan de slag met Event Hubs 

De [Event Hubs aan de slag](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) voorbeeld demonstreert essentiële mogelijkheden van Event Hubs, zoals het maken van een event hub, gebeurtenissen verzenden naar een event hub en gebruiken van gebeurtenissen met de [Gebeurtenisprocessorhost](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) .

### <a name="scale-out-event-processing"></a>Verwerking van de gebeurtenis uitschalen 

De [Scale-out gebeurtenisverwerking](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) voorbeeld laat zien hoe u de [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) voor het distribueren van de werkbelasting van Event Hubs stroom verbruik. Er wordt weergegeven hoe voor het implementeren van de **EventProcessor** en **EventProcessorFactory** objecten voor het beheren van de gebeurtenisstroom. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de versies van .NET Framework via de volgende koppelingen:

- [Frameworks en doelen](/dotnet/articles/standard/frameworks)
- [.NET framework 4.6 en 4.5](/dotnet/framework/index)

U kunt meer informatie over Event Hubs in de volgende artikelen:

- [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
- [Functies van Event Hubs](event-hubs-features.md)
- [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)