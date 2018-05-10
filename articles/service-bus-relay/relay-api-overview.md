---
title: Overzicht van Azure Relay-API | Microsoft Docs
description: Overzicht van de beschikbare Azure Relay-API 's
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/02/2018
ms.author: sethm
ms.openlocfilehash: 00496ca6c0138a840322c053d7d20944df228e9f
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="available-relay-apis"></a>Beschikbare Relay-API 's

## <a name="runtime-apis"></a>Runtime-API 's

De volgende tabel geeft een lijst met alle beschikbare Relay runtime-clients.

De [aanvullende informatie](#additional-information) sectie bevat meer informatie over de status van elke runtime-bibliotheek.

| Taal/Platform | Beschikbare functie | Clientpakket | Opslagplaats |
| --- | --- | --- | --- |
| .NET Standard | Hybride verbindingen | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| .NET framework | WCF-relay | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | N/A |
| Knooppunt | Hybride verbindingen | [Websockets: `hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[Websockets: `hyco-websocket`](https://www.npmjs.com/package/hyco-websocket)<br/>[HTTP-aanvragen: `hyco-https`](https://www.npmjs.com/package/hyco-https) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Aanvullende informatie

#### <a name="net"></a>.NET

Het .NET-ecosysteem heeft meerdere runtimes, dus er zijn meerdere .NET-bibliotheken voor de Relay. De standaard .NET-bibliotheek kan worden uitgevoerd met .NET Core of de .NET Framework, terwijl de .NET Framework-bibliotheek kan alleen worden uitgevoerd in een omgeving met .NET Framework. Zie voor meer informatie over .NET Frameworks [framework-versies](/dotnet/articles/standard/frameworks#framework-versions).

De .NET Framework-bibliotheek alleen ondersteuning biedt voor de WCF-programmeermodel en is afhankelijk van een eigen binaire protocol op basis van de WCF `net.tcp` transport. Dit protocol en de bibliotheek behouden voor achterwaartse compatibiliteit met bestaande toepassingen.

De standaard .NET-bibliotheek is gebaseerd op de protocoldefinitie van de open-voor het doorsturen van hybride verbindingen die is gebaseerd op http- en WebSockets. De bibliotheek ondersteunt een stroom abstractie via Websockets en een gebaar van de API eenvoudige aanvragen en antwoorden naar beantwoorden HTTP-aanvragen. De [Web API](https://github.com/Azure/azure-relay-dotnet) voorbeeld toont het integreren van hybride verbindingen met ASP.NET Core voor webservices.

#### <a name="nodejs"></a>Node.js

De hybride verbindingen modules die worden vermeld in de bovenstaande tabel vervangen of wijzigen van bestaande Node.js-modules met alternatieve implementaties die op de Azure-Relay-service in plaats van de lokale-netwerkstack luisteren.

De `hyco-https` module wijzigt en gedeeltelijk overschrijft de core Node.js-modules `http` en `https`, mits de implementatie van een HTTPS-listener die compatibel is met veel bestaande Node.js-modules en toepassingen die afhankelijk van zijn deze core modules.

De `hyco-ws` en `hyco-websocket` modules wijzigen de populaire `ws` en `websocket` modules voor Node.js, bieden alternatieve listener-implementaties die modules en toepassingen die afhankelijk zijn van de module werken achter de hybride inschakelen Verbindingen Relay.

Meer informatie over deze modules vindt u in de [azure-relay-knooppunt](https://github.com/Azure/azure-relay-node) GitHub-opslagplaats.

## <a name="next-steps"></a>Volgende stappen

Volg deze koppelingen voor meer informatie over Azure Relay:
* [Wat is Azure Relay?](relay-what-is-it.md)
* [Veelgestelde vragen over Relay](relay-faq.md)