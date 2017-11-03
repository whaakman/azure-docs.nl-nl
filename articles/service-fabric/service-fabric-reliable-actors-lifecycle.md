---
title: Overzicht van Azure microservices op basis van actor-levenscyclus | Microsoft Docs
description: Legt uit betrouwbare Service Fabric-Actor levenscyclus, garbagecollection en het handmatig verwijderen actors en hun status
services: service-fabric
documentationcenter: .net
author: amanbha
manager: timlt
editor: vturecek
ms.assetid: b91384cc-804c-49d6-a6cb-f3f3d7d65a8e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/06/2017
ms.author: amanbha
ms.openlocfilehash: d49afd9e5cfe80ddc2d919c76eaa0cb168280c15
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2017
---
# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a>Actor levenscyclus, garbagecollection automatische en handmatige verwijderen
Een actor is de eerste keer die wordt een aanroep naar een van de bijbehorende methoden geactiveerd. Een actor is gedeactiveerd (garbage collector zijn verzameld door de runtime actoren) als deze niet wordt gebruikt voor een configureerbare periode. Een actor en de status kunnen ook handmatig worden verwijderd op elk gewenst moment.

## <a name="actor-activation"></a>Actor-activering
Wanneer een actor is geactiveerd, gebeurt het volgende:

* Wanneer een oproep voor een actor binnenkomt en een nog niet actief, wordt een nieuwe actor gemaakt.
* Status van de actor is geladen als deze status wordt onderhouden.
* De `OnActivateAsync` (C#) of `onActivateAsync` (Java)-methode (die kan worden overschreven in de implementatie van de actor) wordt aangeroepen.
* De actor is nu beschouwd als actief.

## <a name="actor-deactivation"></a>Deactivering van actor
Wanneer een actor is gedeactiveerd, gebeurt het volgende:

* Wanneer een actor niet voor een bepaalde tijd gebruikt wordt, wordt dit verwijderd uit de tabel Active actoren.
* De `OnDeactivateAsync` (C#) of `onDeactivateAsync` (Java)-methode (die kan worden overschreven in de implementatie van de actor) wordt aangeroepen. Hiermee worden de timers voor de actor. Actor-bewerkingen zoals status wijzigingen moeten niet worden aangeroepen vanuit deze methode.

> [!TIP]
> De runtime Fabric actoren verzendt sommige [gebeurtenissen met betrekking tot actor activering en deactivering](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters). Ze zijn nuttig zijn bij de diagnostische gegevens en prestatiebewaking.
>
>

### <a name="actor-garbage-collection"></a>Garbagecollection actor
Wanneer een actor is gedeactiveerd, verwijzingen naar het object actor worden vrijgegeven en wordt deze de garbage collector zijn verzameld normaal door de common language runtime (CLR) of de garbagecollector van de virtuele machine (JVM) java kan worden. Het object actor; ruimt garbagecollection alleen Dit gebeurt **niet** status opgeslagen in de actor status Manager verwijderen. De volgende keer dat de actor is geactiveerd, een nieuw actor-object wordt gemaakt en de status is hersteld.

Wat wordt geteld als 'wordt gebruikt' voor het deactiveren en garbagecollection?

* Ontvangt een oproep
* `IRemindable.ReceiveReminderAsync`methode wordt aangeroepen (alleen van toepassing als de actor herinneringen gebruikt)

> [!NOTE]
> Als de actor timers gebruikt en de timer-retouraanroep wordt aangeroepen, wordt **niet** aantal als 'wordt gebruikt'.
>
>

Voordat we gaan in de details van deactivering, is het belangrijk om te definiëren van de volgende voorwaarden:

* *Controle-interval*. Dit is het interval waarmee de runtime actoren scant de tabel Active actoren voor actoren die kunnen worden gedeactiveerd en garbage collector zijn verzameld. De standaardwaarde voor deze is 1 minuut.
* *Time-out voor inactiviteit*. Dit is de hoeveelheid tijd die een actor moet niet opnieuw gebruikt (niet-actief) voordat deze worden gedeactiveerd en garbage collector zijn verzameld. De standaardwaarde voor deze is 60 minuten.

Normaal gesproken hoeft u niet te deze standaardinstellingen wijzigen. Indien nodig, deze intervallen kunnen echter worden gewijzigd via `ActorServiceSettings` bij het registreren van uw [Actor Service](service-fabric-reliable-actors-platform.md):

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        ActorRuntime.RegisterActorAsync<MyActor>((context, actorType) =>
                new ActorService(context, actorType,
                    settings:
                        new ActorServiceSettings()
                        {
                            ActorGarbageCollectionSettings =
                                new ActorGarbageCollectionSettings(10, 2)
                        }))
            .GetAwaiter()
            .GetResult();
    }
}
```

```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
    }
}
```
Voor elke actieve actor de runtime actor houdt van de hoeveelheid tijd die inactief is geweest (d.w.z. niet gebruikt). Elk van de actoren wordt gecontroleerd door de runtime actor elke `ScanIntervalInSeconds` om te zien als deze worden kan garbage collector zijn verzameld en verzamelt deze als gebruikt gedurende is `IdleTimeoutInSeconds`.

Telkens wanneer een actor wordt gebruikt, wordt de niet-actieve tijd ingesteld op 0. Hierna, mag de actor de garbage collector zijn verzameld alleen als het opnieuw inactief blijft voor `IdleTimeoutInSeconds`. Intrekken dat een actor wordt beschouwd als zijn gebruikt als een interfacemethode actor of een herinnering actor retouraanroep wordt uitgevoerd. Een actor is **niet** beschouwd als zijn gebruikt als de timer-retouraanroep wordt uitgevoerd.

Het volgende diagram toont de levenscyclus van een enkele actor ter illustratie van deze concepten.

![Voorbeeld van een niet-actieve tijd][1]

Het voorbeeld ziet de impact van de methode actoraanroepen, herinneringen en timers van de levensduur van deze acteur. De volgende punten over in het voorbeeld zijn opgemerkt:

* ScanInterval en IdleTimeout zijn ingesteld op 5 en 10 respectievelijk. (Eenheden niet terzake hier, omdat het doel is alleen ter illustratie van het concept.)
* De scan voor actoren garbage collector zijn verzameld worden gebeurt op T = 0, 5, 10, 15, 20, 25, zoals gedefinieerd door het controle-interval van 5.
* Een periodieke timer wordt geactiveerd op T = 4, 8, 12, 16, 20, 24, en de retouraanroep wordt uitgevoerd. De niet-actieve tijd van de actor geen gevolgen.
* Een methodeaanroep actor op T = 7 stelt de niet-actieve tijd in op 0, en de garbagecollection van de actor uitstelt.
* Een herinnering actor retouraanroep wordt uitgevoerd op T = 14 en verdere uitstelt de garbagecollection van de actor.
* Tijdens het scannen van de verzameling afval T = 25 niet-actieve tijd van de actor ten slotte de time-out voor inactiviteit van 10 overschrijdt en is de actor garbage collector zijn verzameld.

Een actor kan garbage collector zijn verzameld terwijl deze wordt uitgevoerd een van de bijbehorende methoden, ongeacht hoeveel tijd is besteed aan het uitvoeren van deze methode niet. Zoals eerder vermeld, wordt de uitvoering van actor interfacemethoden en herinnering retouraanroepen voorkomen dat garbagecollection door niet-actieve tijd van de actor instellen op 0. De uitvoering van retouraanroepen timer niet opnieuw instellen niet-actieve tijd op 0. De garbagecollection van de actor wordt echter worden uitgesteld totdat de timercallback uitvoering is voltooid.

## <a name="deleting-actors-and-their-state"></a>Actors en hun status verwijderen
Het object actor garbagecollection van gedeactiveerde actoren alleen ruimt, maar gegevens die zijn opgeslagen in een actor status Manager wordt niet verwijderd. Wanneer een actor opnieuw worden geactiveerd, is de gegevens opnieuw beschikbaar gesteld aan via de status Manager. In gevallen waarbij actoren gegevens opslaan in de status Manager en zijn gedeactiveerd maar nooit opnieuw worden geactiveerd, is het mogelijk nodig om hun gegevens op te schonen.

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

Let op: een actor kan niet worden aangeroepen op zichzelf uit een van de bijbehorende methoden actor niet verwijderen omdat de actor kan niet worden verwijderd tijdens het uitvoeren in een context actor-aanroep, waarin de runtime een vergrendeling rond de aanroep van actor afdwingen single thread toegang heeft verkregen.

## <a name="next-steps"></a>Volgende stappen
* [Acteur timers en herinneringen](service-fabric-reliable-actors-timers-reminders.md)
* [Actor-gebeurtenissen](service-fabric-reliable-actors-events.md)
* [Acteur herintreding](service-fabric-reliable-actors-reentrancy.md)
* [Acteur diagnostische gegevens en prestatiebewaking](service-fabric-reliable-actors-diagnostics.md)
* [Acteur API-naslagdocumentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# voorbeeldcode](https://github.com/Azure/servicefabric-samples)
* [Voorbeeldcode voor Java](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
