---
title: Service Fabric programming model overzicht | Microsoft Docs
description: 'Service Fabric biedt twee frameworks voor het bouwen van services: de actor-framework en de serviceframework. Ze bieden verschillende verschillen in eenvoud en controle.'
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: vturecek
ms.assetid: 974b2614-014e-4587-a947-28fcef28b382
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 237b8396b56fdec86cc005c121646556825d8e98
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="service-fabric-programming-model-overview"></a>Overzicht van service Fabric programming model
Service Fabric biedt verschillende manieren om te schrijven en beheren van uw services. Services kunt gebruiken van de Service Fabric-API's te kunnen profiteren van het platform functies en toepassingsframeworks. Services kunnen ook worden alle gecompileerde uitvoerbaar programma geschreven in elke taal of de code die wordt uitgevoerd in een container gewoon worden gehost op een Service Fabric-cluster.

## <a name="guest-executables"></a>Gast uitvoerbare bestanden
Een [Gast uitvoerbaar bestand](service-fabric-deploy-existing-app.md) is een bestaande willekeurige uitvoerbaar bestand (geschreven in een andere taal) en dat kan worden uitgevoerd als een service in uw toepassing. Gast uitvoerbare bestanden de Service Fabric SDK-API's niet rechtstreeks aangeroepen. Maar ze nog steeds profiteren van het platform, zoals aangepaste health service onderwerpgebieden biedt functies en laden door het aanroepen van REST-API's die worden weergegeven door de Service Fabric reporting. Ze hebben ook de volledige toepassing lifecycle ondersteuning.

Aan de slag met gast uitvoerbare bestanden door het implementeren van uw eerste [Gast uitvoerbare toepassing](service-fabric-deploy-existing-app.md).

## <a name="containers"></a>Containers
Standaard wordt Service Fabric implementeert en services als processen wordt geactiveerd. Service Fabric kunnen ook services implementeren in [containers](service-fabric-containers-overview.md). Service Fabric ondersteunt de implementatie van containers voor Linux en Windows-containers op Windows Server 2016. Installatiekopieën van de container kunnen worden opgehaald uit de opslagplaats voor elke container en geïmplementeerd op de machine. U kunt bestaande toepassingen implementeren als gast exectuables, Service Fabric stateless of stateful Reliable services of Reliable Actors in containers en kunt u services in processen en -services in containers in dezelfde toepassing elkaar.

[Meer informatie over uw services in Windows of Linux containerizing](service-fabric-deploy-container.md)

## <a name="reliable-services"></a>Reliable Services
Reliable Services is een lichtgewicht framework voor het schrijven van services die integreren met het Service Fabric-platform en profiteren van de volledige set van platformfuncties. Betrouwbare Services bieden een minimale set API's waarmee de Service Fabric-runtime voor het beheren van de levenscyclus van uw services en waarmee uw services om te communiceren met de runtime. Het framework is minimaal, zodat u volledige controle over het ontwerp en de implementatie-opties, en kan worden gebruikt voor het hosten van andere toepassingsframework, zoals ASP.NET Core.

Reliable Services kunnen worden staatloze, vergelijkbaar met de meeste platforms van de service, zoals webservers, waarbij elk exemplaar van de service is gelijk gemaakt en status is opgeslagen in een externe oplossing, zoals Azure DB of Azure Table Storage.

Reliable Services kunnen ook stateful is, worden uitsluitend van toepassing op Service Fabric, waarop de status wordt behouden rechtstreeks in de service zelf betrouwbare verzamelingen gebruiken. Staat het maken van maximaal beschikbare door middel van replicatie en gedistribueerd via partitioneren, alle beheerde automatisch door de Service Fabric.

[Meer informatie over Reliable Services](service-fabric-reliable-services-introduction.md) of aan de slag door [schrijven van uw eerste betrouwbare Service](service-fabric-reliable-services-quick-start.md).

## <a name="aspnet-core"></a>ASP.NET Core
ASP.NET Core is een nieuw open source en platformoverschrijdende framework voor het bouwen van moderne cloud-gebaseerde Internet verbonden toepassingen, zoals web-apps, IoT-apps en mobiele back-ends. Service Fabric worden geïntegreerd met ASP.NET Core, dus u kunt zowel stateless als stateful ASP.NET Core schrijven toepassingen die van betrouwbare verzamelingen en Service-Fabric geavanceerde orchestration mogelijkheden gebruikmaken.

[Meer informatie over ASP.NET Core in Service Fabric](service-fabric-reliable-services-communication-aspnetcore.md) of aan de slag door [uw eerste ASP.NET Core Service Fabric-toepassing schrijft](service-fabric-add-a-web-frontend.md).

## <a name="reliable-actors"></a>Reliable Actors
Het betrouwbare Actor-framework is gebouwd op Reliable Services, is een toepassingsframework dat het virtuele Actor-patroon, op basis van het ontwerppatroon actor implementeert. Onafhankelijke eenheden van de berekenings- en status van het betrouwbare Actor-framework gebruikt met één thread uitvoering actoren aangeroepen. Het betrouwbare Actor-framework biedt ingebouwde communicatie voor actors en vooraf ingestelde status persistentie en scale-out-configuraties.

Als Reliable Actors zelf een toepassingsframework dat is gebaseerd op Reliable Services is, is het volledig geïntegreerd met de Service Fabric-platform en de voordelen van de volledige set met functies die worden aangeboden door het platform.

[Meer informatie over Reliable Actors](service-fabric-reliable-actors-introduction.md) of aan de slag door [schrijven van uw eerste betrouwbare Actor-service](service-fabric-reliable-actors-get-started.md)


[Een front-end-service met behulp van ASP.NET Core bouwen](service-fabric-add-a-web-frontend.md)

## <a name="next-steps"></a>Volgende stappen
[Overzicht van service Fabric en containers](service-fabric-containers-overview.md)

[Overzicht van betrouwbare Services](service-fabric-reliable-services-introduction.md)

[Overzicht van betrouwbare Actors](service-fabric-reliable-actors-introduction.md)

[Service Fabric- en ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)




