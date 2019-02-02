---
title: Overzicht van Azure Event Hubs-API | Microsoft Docs
description: Overzicht van beschikbare Azure Event Hubs-API 's
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: 3f221a0c-182d-4e39-9f3d-3a3c16c5c6ed
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/02/2018
ms.author: shvija
ms.openlocfilehash: 432f2495dd1b4c8aeb761d12904adf26f3c24c0b
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55660694"
---
# <a name="available-event-hubs-apis"></a>Beschikbare Event Hubs-API 's

Dit artikel beschrijft de set beschikbare API-clients die u gebruiken kunt voor het beheren van Event Hubs-bronnen.

## <a name="runtime-apis"></a>Runtime-API 's

Het volgende gedeelte bevat alle beschikbare Azure Event Hubs runtime-clients. Sommige van deze bibliotheken ook beperkte beheerfunctionaliteit bevatten, maar er zijn ook [specifieke bibliotheken](#management-apis) toegewezen aan bewerkingen. De focus core van deze bibliotheken is voor het verzenden en ontvangen van berichten van een event hub.

Zie voor meer informatie over de huidige status van elke wisselaar runtime [aanvullende informatie](#additional-information).

| Language/Platform | Client-pakket | EventProcessorHost-pakket | Opslagplaats |
| --- | --- | --- | --- |
| .NET Standard | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) | [GitHub](https://github.com/azure/azure-event-hubs-dotnet) |
| .NET Framework | [NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) | N/A |
| Java | [Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) | [Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22) | [GitHub](https://github.com/Azure/azure-event-hubs-java) |
| Knooppunt | [NPM](https://www.npmjs.com/package/azure-event-hubs) | N/A | [GitHub](https://github.com/Azure/azure-event-hubs-node) |
| C | N/A | N/A | [GitHub](https://github.com/Azure/azure-event-hubs-c) |

### <a name="additional-information"></a>Aanvullende informatie

#### <a name="net"></a>.NET

Het .NET-ecosysteem heeft meerdere runtimes, dus er meerdere .NET-bibliotheken voor Event Hubs zijn. De standaard .NET-bibliotheek kan worden uitgevoerd met behulp van .NET Core of .NET Framework, terwijl de .NET Framework-bibliotheek kan alleen worden uitgevoerd in een omgeving met .NET Framework. Zie voor meer informatie over de versies van .NET Framework, [frameworkversies](https://docs.microsoft.com/dotnet/articles/standard/frameworks).

#### <a name="node"></a>Knooppunt

De [Node.js-bibliotheek](https://github.com/Azure/azure-event-hubs-node) is momenteel in preview en als een project aan clientzijde wordt onderhouden door Microsoft-werknemers en externe inzenders. Alle bijdragen, met inbegrip van broncode vrij en wordt gecontroleerd.

## <a name="management-apis"></a>Beheer API’s

De volgende tabel bevat alle beschikbare management-specifieke-bibliotheken. Geen van deze bibliotheken runtime-bewerkingen bevatten, en zijn voor het enige doel van het beheren van Event Hubs-entiteiten.

| Language/Platform | Management Pack | Opslagplaats |
| --- | --- | --- | --- |
| .NET Standard | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.EventHub) | [GitHub](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/ResourceManagement/EventHub) |

## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Een Event Hub maken](event-hubs-create.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
