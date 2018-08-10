---
title: Voorbeelden van Azure Event Hubs | Microsoft Docs
description: Voorbeelden van Azure Event Hubs
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2018
ms.author: shvija
ms.openlocfilehash: fbde6e5a5ed053d6c151b3af25535c397a496ef4
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005331"
---
# <a name="event-hubs-samples"></a>Event Hubs-voorbeelden 
U kunt voorbeelden voor Event Hubs vinden op [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples). Deze voorbeelden demonstreren belangrijke functies in [Azure Event Hubs](/azure/event-hubs/). In dit artikel worden gecategoriseerd en een beschrijving van de voorbeelden die beschikbaar zijn, met koppelingen naar elk.

## <a name="net-samples"></a>.NET-voorbeelden

| Voorbeeldnaam | Beschrijving | 
| ----------- | ----------- | 
| [SampleSender](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) | Dit voorbeeld laat zien hoe u een .NET Core-consoletoepassing die een reeks gebeurtenissen naar een event hub verzendt. |
| [SampleEHReceiver](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) | Dit voorbeeld laat zien hoe u een .NET Core-consoletoepassing die een reeks gebeurtenissen vanaf een event hub ontvangt met behulp van de Event Processor Host-bibliotheek.  | 

## <a name="java-samples"></a>Java-voorbeelden

| Voorbeeldnaam | Beschrijving | 
| ----------- | ----------- | 
| [SendBatch](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SendBatch)  | In dit voorbeeld ziet hoe u voor het opnemen van batches van gebeurtenissen in uw event hub. | 
| [SimpleSend](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend) | In dit voorbeeld ziet hoe u voor het opnemen van gebeurtenissen in uw event hub. |
| [AdvanceSendOptions](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/AdvancedSendOptions) | In dit voorbeeld ziet de verschillende opties die beschikbaar zijn met Event Hubs om gebeurtenissen te nemen. |
| [ReceiveByDateTime](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveByDateTime) | In dit voorbeeld ziet hoe u gebeurtenissen ontvangen van een event hub-partitie met behulp van een specifieke datum / tijd offset. |
| [ReceiveUsingOffset](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingOffset) | In dit voorbeeld ziet hoe u gebeurtenissen ontvangen van een event hub-partitie met behulp van een offset van het specifieke gegevens. |  
| [ReceiveUsingSequenceNumber](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingSequenceNumber) | In dit voorbeeld laat zien hoe kunnen ontvangen van een event hub-partities met behulp van een volgnummer. |   
| [EventProcessorSample](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/EventProcessorSample) |In dit voorbeeld ziet hoe u gebeurtenissen ontvangen van een event hub met behulp van de event processor host, waarmee automatische partitie selectie en failover over meerdere gelijktijdige ontvangers. | 
| [AutoScaleOnIngress](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/AutoScaleOnIngress) | In dit voorbeeld ziet hoe een event hub kan automatisch opschalen op hoge belasting. Het voorbeeld verzendt gebeurtenissen met een snelheid die alleen groter zijn dan de geconfigureerde snelheid van een event hub, waardoor de event hub om omhoog te schalen. | 
| [IngressBenchmark](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/IngressBenchmark) | In dit voorbeeld kunt meten van het tarief voor inkomend verkeer. | 

## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs in de volgende artikelen:

- [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
- [Functies van Event Hubs](event-hubs-features.md)
- [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)