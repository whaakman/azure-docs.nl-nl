---
title: Service Fabric programming model overview | Microsoft Docs
description: 'Service Fabric biedt twee frameworks voor het bouwen van services: de actorframework en de serviceframework. Deze verbindingen bieden een unieke wisselwerking in eenvoud en controle.'
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: vturecek
ms.assetid: 974b2614-014e-4587-a947-28fcef28b382
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: d764cbe2df78cb9029a4109caa2998ddded5d6ff
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58665709"
---
# <a name="service-fabric-programming-model-overview"></a>Overzicht van service Fabric programming model
Service Fabric biedt verschillende manieren om te schrijven en beheren van uw services. Services kunnen kiezen voor de Service Fabric-API's te kunnen profiteren van de functies van het platform en de App-frameworks. Services kunnen ook worden alle gecompileerde uitvoerbaar programma geschreven in elke taal of de code die wordt uitgevoerd in een container die wordt gehost op een Service Fabric-cluster.

## <a name="guest-executables"></a>Uitvoerbare gastbestanden
Een [Gast kan worden uitgevoerd](service-fabric-guest-executables-introduction.md) is een bestaande, willekeurige uitvoerbaar bestand (geschreven in elke taal) en die kan worden uitgevoerd als een service in uw toepassing. Gastbestanden aanroepen de Service Fabric SDK API's niet rechtstreeks. Maar ze nog steeds profiteren van functies die het platform, zoals service detectie, aangepaste status biedt en laden door het aanroepen van REST-API's die worden weergegeven door de Service Fabric reporting. Ze hebben ook de volledige levenscyclus van ondersteuning.

Aan de slag met uitvoerbare gastbestanden door het implementeren van uw eerste [door Gast uitvoerbare toepassing](service-fabric-deploy-existing-app.md).

## <a name="containers"></a>Containers
Standaard wordt Service Fabric implementeert en services als processen wordt geactiveerd. Service Fabric kunnen ook implementeren van services in [containers](service-fabric-containers-overview.md). Service Fabric biedt ondersteuning voor implementatie van Linux-containers en Windows-containers op Windows Server 2016. Containerinstallatiekopieën kunnen worden opgehaald uit een containeropslagplaats en geïmplementeerd op de machine. Kunt u bestaande toepassingen implementeren als uitvoerbare gastbestanden, Service Fabric stateless of stateful Reliable services en Reliable Actors in containers, en u kunt combineren met services in processen en services in containers binnen dezelfde toepassing.

[Meer informatie over het beperken van de services in Windows of Linux](service-fabric-deploy-container.md)

## <a name="reliable-services"></a>Reliable Services
Reliable Services is een lichte structuur voor het schrijven van services die integreren met het Service Fabric-platform en profiteren van de volledige set functies van het platform. Reliable Services bieden een minimale set API's waarmee de Service Fabric-runtime voor het beheren van de levenscyclus van uw services en dat uw services om te communiceren met de runtime toe te staan. Application framework is minimaal, waardoor u volledige controle over het ontwerp en implementatie-opties, en kan worden gebruikt voor het hosten van andere toepassingsframework, zoals ASP.NET Core.

Betrouwbare Services kunnen worden stateless, vergelijkbaar met de meeste service-platformen, zoals webservers, waarin elk exemplaar van de service is gelijk gemaakt en status is opgeslagen in een externe oplossing, zoals Azure DB of Azure-tabelopslag.

Reliable Services kunnen ook worden stateful, exclusief voor Service Fabric, waar status is opgeslagen rechtstreeks in de service zelf met behulp van betrouwbare verzamelingen. Status is hoge beschikbaarheid door middel van replicatie gemaakt en gedistribueerd via partitionering, alle beheerde automatisch door Service Fabric.

[Meer informatie over Reliable Services](service-fabric-reliable-services-introduction.md) of aan de slag met [schrijven van uw eerste betrouwbare Service](service-fabric-reliable-services-quick-start.md).

## <a name="aspnet-core"></a>ASP.NET Core
ASP.NET Core is een nieuw open-source en platformoverschrijdende framework voor het bouwen van moderne cloud-gebaseerde Internet verbonden toepassingen, zoals web-apps, IoT-apps en mobiele back-ends. Service Fabric is geïntegreerd met ASP.NET Core, zodat u toepassingen die van betrouwbare verzamelingen en de geavanceerde planningsmogelijkheden van Service Fabric profiteren kunt schrijven voor staatloze en stateful ASP.NET Core.

[Meer informatie over ASP.NET Core in Service Fabric](service-fabric-reliable-services-communication-aspnetcore.md) of aan de slag met [schrijven van uw eerste ASP.NET Core-Service Fabric-toepassing](service-fabric-tutorial-create-dotnet-app.md).

## <a name="reliable-actors"></a>Reliable Actors
Gebaseerd op Reliable Services, is het Reliable Actor-framework een toepassingsframework dat het virtuele Actor-patroon, op basis van het ontwerppatroon actor implementeert. Onafhankelijke eenheden van reken- en staat het Reliable Actor-framework gebruikt met één thread worden uitgevoerd met de naam actoren. Het framework Reliable Actor biedt ingebouwde communicatie voor actors en vooraf ingestelde status persistentie en scale-out-configuraties.

Omdat Reliable Actors een toepassingsframework dat is gebaseerd op betrouwbare Services is, is het volledig geïntegreerd met de Service Fabric-platform en de voordelen van de volledige set functies die worden aangeboden door het platform.

[Meer informatie over Reliable Actors](service-fabric-reliable-actors-introduction.md) of aan de slag met [schrijven van uw eerste Reliable Actor-service](service-fabric-reliable-actors-get-started.md)


[Bouw een front-end-service met behulp van ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)

## <a name="next-steps"></a>Volgende stappen
[Overzicht van service Fabric en containers](service-fabric-containers-overview.md)

[Overzicht van Reliable Services](service-fabric-reliable-services-introduction.md)

[Overzicht van Reliable Actors](service-fabric-reliable-actors-introduction.md)

[Service Fabric en ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)




