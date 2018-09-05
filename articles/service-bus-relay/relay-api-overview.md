---
title: Overzicht van Azure Relay-API | Microsoft Docs
description: Overzicht van beschikbare Azure Relay-API 's
services: event-hubs
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/02/2018
ms.author: spelluru
ms.openlocfilehash: 2c7aec700a1077bc1a1b56afb5d8d07f47f4c6e8
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700369"
---
# <a name="available-relay-apis"></a>Beschikbare Relay-API 's

## <a name="runtime-apis"></a>Runtime-API 's

De volgende tabel bevat alle beschikbare Relay runtime-clients.

De [aanvullende informatie](#additional-information) sectie vindt u meer informatie over de status van elke wisselaar runtime.

| Taal/Platform | Beschikbare functie | Client-pakket | Opslagplaats |
| --- | --- | --- | --- |
| .NET Standard | Hybride verbindingen | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| .NET Framework | WCF-relay | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | N/A |
| Knooppunt | Hybride verbindingen | [Websockets: `hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[Websockets: `hyco-websocket`](https://www.npmjs.com/package/hyco-websocket)<br/>[HTTP-aanvragen: `hyco-https`](https://www.npmjs.com/package/hyco-https) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Aanvullende informatie

#### <a name="net"></a>.NET

Het .NET-ecosysteem heeft meerdere runtimes, dus er zijn meerdere .NET-bibliotheken voor de Relay. De standaard .NET-bibliotheek kan worden uitgevoerd met behulp van .NET Core of .NET Framework, terwijl de .NET Framework-bibliotheek kan alleen worden uitgevoerd in een omgeving met .NET Framework. Zie voor meer informatie over .NET Frameworks [frameworkversies](/dotnet/articles/standard/frameworks#framework-versions).

De .NET Framework-bibliotheek alleen ondersteuning biedt voor de WCF-programmeermodel en maakt gebruik van een eigen binaire protocol op basis van de WCF `net.tcp` transport. Dit protocol en de bibliotheek wordt bijgehouden voor achterwaartse compatibiliteit met bestaande toepassingen.

De standaard .NET-bibliotheek is gebaseerd op de definitie van de open-protocol voor de hybride Relay-verbindingen die is gebaseerd op HTTP en WebSockets. De bibliotheek ondersteunt een abstractie stream via Websockets en een beweging van eenvoudige request response-API voor beantwoording HTTP-aanvragen. De [Web-API](https://github.com/Azure/azure-relay-dotnet) voorbeeld laat zien hoe u hybride verbindingen met ASP.NET Core integreert voor webservices.

#### <a name="nodejs"></a>Node.js

De Hybrid Connections-modules die worden vermeld in de bovenstaande tabel vervangen of wijzigen van bestaande Node.js-modules met alternatieve implementaties die luisteren naar de Azure Relay-service in plaats van de lokale netwerken stack.

De `hyco-https` module wijzigt en gedeeltelijk overschrijft de Node.js-kernmodules `http` en `https`, mits een HTTPS-listener-toepassing die compatibel is met veel bestaande Node.js-modules en toepassingen die afhankelijk van zijn deze core modules.

De `hyco-ws` en `hyco-websocket` modules wijziging van de populaire `ws` en `websocket` modules voor Node.js, bieden alternatieve listener-implementaties die modules en toepassingen die afhankelijk zijn van een module om te werken achter de hybride inschakelen Verbindingen Relay.

Meer informatie over deze modules kunnen u vinden in de [azure relay-knooppunten](https://github.com/Azure/azure-relay-node) GitHub-opslagplaats.

## <a name="next-steps"></a>Volgende stappen

Volg deze koppelingen voor meer informatie over Azure Relay:
* [Wat is Azure Relay?](relay-what-is-it.md)
* [Veelgestelde vragen over Relay](relay-faq.md)