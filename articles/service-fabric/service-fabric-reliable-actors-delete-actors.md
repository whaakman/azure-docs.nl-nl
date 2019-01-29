---
title: Azure Service Fabric-actoren verwijderen | Microsoft Docs
description: Leer hoe u Service Fabric Reliable Actors en hun status handmatig verwijderen.
services: service-fabric
documentationcenter: .net
author: amanbha
manager: timlt
editor: vturecek
ms.assetid: b91384cc-804c-49d6-a6cb-f3f3d7d65a8e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2018
ms.author: amanbha
ms.openlocfilehash: d6ac5ea79ed1eb47bc71a520761050889fe6edd8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55178168"
---
# <a name="delete-reliable-actors-and-their-state"></a>Betrouwbare actoren en hun status verwijderen
Garbagecollection van gedeactiveerde actoren alleen opschonen van de actorobject, maar deze gegevens die zijn opgeslagen in van een actor status Manager worden niet verwijderd. Wanneer een actor opnieuw wordt geactiveerd, wordt de gegevens opnieuw beschikbaar gesteld aan via de status Manager. In gevallen waar actoren gegevens opslaan in de status Manager en zijn gedeactiveerd maar nooit geactiveerd, is het mogelijk dat het nodig zijn om hun gegevens op te schonen.

De [Actorservice](service-fabric-reliable-actors-platform.md) biedt een functie voor het verwijderen van actoren vanaf een externe aanroeper:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

Verwijderen van een actor heeft de volgende gevolgen, afhankelijk van of de actor momenteel actief is:

* **Actieve Actor**
  * Actor wordt verwijderd uit de lijst met actieve actoren en is gedeactiveerd.
  * De status wordt permanent verwijderd.
* **Inactieve Actor**
  * De status wordt permanent verwijderd.

Een actor kan niet worden aangeroepen op zichzelf uit een van de actor-methoden niet verwijderen omdat de actor kan niet worden verwijderd tijdens het uitvoeren van binnen een actor aanroepcontext, waarbij de runtime een vergrendeling rond de actor-aanroep om af te dwingen single-threaded toegang heeft verkregen.

Voor meer informatie over Reliable Actors, lees het volgende:
* [Actor-timers en herinneringen](service-fabric-reliable-actors-timers-reminders.md)
* [Actor-gebeurtenissen](service-fabric-reliable-actors-events.md)
* [Herbetreedbaarheid actor](service-fabric-reliable-actors-reentrancy.md)
* [Actor-diagnose en bewaking van toepassingsprestaties](service-fabric-reliable-actors-diagnostics.md)
* [Actor-API-referentiedocumentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Voorbeeld van C#-code](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java-voorbeeldcode](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
