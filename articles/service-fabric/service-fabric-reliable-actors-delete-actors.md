---
title: Azure Service Fabric actoren verwijderen | Microsoft Docs
description: Informatie over het Service Fabric Reliable Actors en hun status handmatig verwijderen.
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
ms.openlocfilehash: fa4fe018a9e6b32158f5bbd13c44ff57069cb1cf
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34208332"
---
# <a name="delete-reliable-actors-and-their-state"></a>Reliable Actors en hun status verwijderen
Het object actor garbagecollection van gedeactiveerde actoren alleen ruimt, maar gegevens die zijn opgeslagen in een actor status Manager wordt niet verwijderd. Wanneer een actor opnieuw wordt geactiveerd, is de gegevens opnieuw beschikbaar gesteld aan via de status Manager. In gevallen waarbij actoren gegevens opslaan in de status Manager en zijn gedeactiveerd maar nooit geactiveerd, is het mogelijk nodig om hun gegevens op te schonen.

De [Actor Service](service-fabric-reliable-actors-platform.md) geeft een functie voor het verwijderen van actoren van een externe aanroeper:

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

Verwijderen van een actor heeft de volgende gevolgen afhankelijk van of de actor momenteel actief is:

* **Actieve Actor**
  * Acteur wordt verwijderd uit active actoren lijst en wordt gedeactiveerd.
  * De status wordt permanent verwijderd.
* **Inactieve Actor**
  * De status wordt permanent verwijderd.

Een actor kan niet worden aangeroepen op zichzelf uit een van de bijbehorende methoden actor verwijderen, omdat de actor kan niet worden verwijderd tijdens het uitvoeren in een context actor-aanroep, waarin de runtime een vergrendeling rond de aanroep van actor afdwingen single thread toegang heeft verkregen.

Voor meer informatie over Reliable Actors Lees het volgende:
* [Acteur timers en herinneringen](service-fabric-reliable-actors-timers-reminders.md)
* [Actor-gebeurtenissen](service-fabric-reliable-actors-events.md)
* [Acteur herintreding](service-fabric-reliable-actors-reentrancy.md)
* [Acteur diagnostische gegevens en prestatiebewaking](service-fabric-reliable-actors-diagnostics.md)
* [Acteur API-naslagdocumentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# voorbeeldcode](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Voorbeeldcode voor Java](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
