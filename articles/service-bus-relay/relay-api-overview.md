---
title: Overzicht van Azure Relay-API | Microsoft Docs
description: Overzicht van de beschikbare Azure Relay-API 's
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: 60413bd110fd4021b070f94d6338a50e1b321dea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="available-relay-apis"></a>Beschikbare Relay-API 's

## <a name="runtime-apis"></a>Runtime-API 's

De volgende tabel geeft een lijst met alle beschikbare Relay runtime-clients.

De [aanvullende informatie](#additional-information) sectie bevat meer informatie over de status van elke runtime-bibliotheek.

| Taal/Platform | Beschikbare functie | Clientpakket | Opslagplaats |
| --- | --- | --- | --- |
| .NET-standaard | Hybride verbindingen | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| .NET framework | WCF-relay | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | N.v.t. |
| Knooppunt | Hybride verbindingen | [`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Aanvullende informatie

#### <a name="net"></a>.NET
Het .NET-ecosysteem heeft meerdere runtimes, dus er zijn meerdere .NET-bibliotheken voor Event Hubs. De standaard .NET-bibliotheek kan worden uitgevoerd met .NET Core of de .NET Framework, terwijl de .NET Framework-bibliotheek kan alleen worden uitgevoerd in een omgeving met .NET Framework. Zie voor meer informatie over .NET Frameworks [framework-versies](/dotnet/articles/standard/frameworks#framework-versions).

## <a name="next-steps"></a>Volgende stappen
Volg deze koppelingen voor meer informatie over Azure Relay:
* [Wat is Azure Relay?](relay-what-is-it.md)
* [Veelgestelde vragen over Relay](relay-faq.md)