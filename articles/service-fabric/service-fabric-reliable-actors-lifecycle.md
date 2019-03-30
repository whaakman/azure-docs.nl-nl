---
title: Overzicht van de levenscyclus van de Azure Service Fabric-actor | Microsoft Docs
description: Service Fabric Reliable Actor levenscyclus, garbagecollection en handmatig verwijderen van actoren en hun status uitgelegd
services: service-fabric
documentationcenter: .net
author: amanbha
manager: chackdan
editor: vturecek
ms.assetid: b91384cc-804c-49d6-a6cb-f3f3d7d65a8e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/06/2017
ms.author: amanbha
ms.openlocfilehash: f81fde441a2f0dc2504601f82e5b890eb6e216de
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58660983"
---
# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a>Actor-levenscyclus, automatische garbagecollection en handmatig verwijderen
Een actor is de eerste keer die wordt een aanroep uitgevoerd naar een van de bijbehorende methoden geactiveerd. Een actor is gedeactiveerd (garbagecollection die worden verzameld door de runtime actoren) als deze niet is gebruikt gedurende een configureerbare tijd. Een actor en de status kunnen ook handmatig worden verwijderd op elk gewenst moment.

## <a name="actor-activation"></a>Actor-activering
Als een actor is geactiveerd, gebeurt het volgende:

* Wanneer een oproep afkomstig van een actor is en deze nog niet is actief, wordt een nieuwe actor gemaakt.
* Status van de actor wordt geladen als deze status wordt onderhouden.
* De `OnActivateAsync` (C#) of `onActivateAsync` (Java)-methode (die kan worden overschreven in de actorimplementatie) wordt genoemd.
* De actor wordt nu als actief beschouwd.

## <a name="actor-deactivation"></a>Actor-deactiveren
Wanneer een actor is gedeactiveerd, gebeurt het volgende:

* Wanneer een actor niet voor een bepaalde tijd gebruikt wordt, wordt deze verwijderd uit de tabel Active actoren.
* De `OnDeactivateAsync` (C#) of `onDeactivateAsync` (Java)-methode (die kan worden overschreven in de actorimplementatie) wordt genoemd. Hiermee worden alle timers voor de actor. Actor-bewerkingen, zoals status wijzigingen mag niet worden aangeroepen vanuit deze methode.

> [!TIP]
> De Fabric-actoren-runtime verzendt sommige [gebeurtenissen met betrekking tot de actor-activering en deactivering](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters). Ze zijn nuttig in diagnostische gegevens en bewaking van toepassingsprestaties.
>
>

### <a name="actor-garbage-collection"></a>Actor-garbagecollection
Wanneer een actor is gedeactiveerd, verwijzingen naar de actorobject worden vrijgegeven en garbagecollection normaal gesproken worden verzameld door de common language runtime (CLR) of de java virtual machine (JVM ook) garbagecollector kan zijn. Garbagecollection alleen opschonen van de actorobject. Dit gebeurt **niet** staat die zijn opgeslagen in de actor van status Manager verwijderen. De volgende keer dat de actor is geactiveerd, een nieuw actorobject wordt gemaakt en de status is hersteld.

Wat telt als 'wordt gebruikt' voor het deactiveren en garbagecollection?

* Een gesprek ontvangen
* `IRemindable.ReceiveReminderAsync` methode wordt aangeroepen (alleen van toepassing als herinneringen maakt gebruik van de actor)

> [!NOTE]
> Als de actor timers gebruikt en de retouraanroep timer wordt aangeroepen, wordt **niet** tellen als 'gebruikte'.
>
>

Voordat we de details van de deactivering, is het belangrijk om te definiëren van de volgende voorwaarden:

* *Scan interval*. Dit is het interval waarmee de runtime actoren scant de tabel Active actoren op actoren die kunnen worden gedeactiveerd en garbagecollection die worden verzameld. De standaardwaarde voor deze is 1 minuut.
* *Time-out voor inactiviteit*. Dit is de hoeveelheid tijd die een actor moet blijven ongebruikte (inactief) voordat deze kan worden gedeactiveerd en garbagecollection die worden verzameld. De standaardwaarde voor deze is 60 minuten.

Normaal gesproken hoeft u niet te deze standaardinstellingen wijzigen. Indien nodig, deze intervallen kunnen echter worden gewijzigd via `ActorServiceSettings` bij het registreren van uw [Actorservice](service-fabric-reliable-actors-platform.md):

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
Voor elke actieve acteur, de actor-runtime houdt van de hoeveelheid tijd die inactief is geweest (dat wil zeggen niet gebruikt). De actor-runtime wordt elk van de actoren elke `ScanIntervalInSeconds` om te zien als mag garbagecollection die worden verzameld en verzamelt deze als u een niet-actieve voor `IdleTimeoutInSeconds`.

Telkens wanneer een actor wordt gebruikt, wordt de niet-actieve tijd ingesteld op 0. Hierna de actor kan worden verwijderd als het opnieuw inactief blijft voor `IdleTimeoutInSeconds`. Intrekken dat een actor wordt beschouwd als zijn gebruikt als een actormethode-interface of een herinnering actor terugbellen wordt uitgevoerd. Is van een actor **niet** beschouwd als zijn gebruikt als de retouraanroep timer wordt uitgevoerd.

Het volgende diagram toont de levenscyclus van een enkele actor om deze concepten te illustreren.

![Voorbeeld van een niet-actieve tijd][1]

Het voorbeeld ziet de invloed van de methodeaanroepen actor, herinneringen en timers van de levensduur van deze actor. De volgende punten over het voorbeeld worden opgemerkt:

* ScanInterval en IdleTimeout zijn ingesteld op 5 en 10 respectievelijk. (Eenheden niet terzake hier, omdat ons doel is uitsluitend ter illustratie van het concept.)
* De scan voor actors moet worden verwijderd vindt plaats op T = 0, 5, 10, 15, 20, 25, zoals gedefinieerd door het controle-interval van 5.
* Een periodieke timer wordt geactiveerd op T = 4, 8, 12, 16, 20, 24 uur per dag, en de retouraanroep wordt uitgevoerd. Dit heeft geen invloed op de niet-actieve tijd van de actor.
* De aanroep van een actor-methode op T = 7 wordt de niet-actieve tijd teruggezet naar de 0 en uitstelt de garbagecollection van de actor.
* Een herinnering actor terugbellen wordt uitgevoerd om T = 14 en verder uitstelt de garbagecollection van de actor.
* Niet-actieve tijd van de actor ten slotte is groter dan de time-out voor inactiviteit van 10 en de actor wordt onleesbare die worden verzameld tijdens het scannen van de verzameling garbagecollection T = 25.

Een actor wordt nooit worden verwijderd terwijl deze een van de bijbehorende methoden, ongeacht hoeveel tijd wordt besteed aan het bij het uitvoeren van deze methode wordt uitgevoerd. Zoals eerder vermeld, wordt de uitvoering van de actor-interfacemethoden en herinnering callbacks garbagecollection voorkomen dat door de fabrieksinstellingen van niet-actieve tijd van de actor op 0. De uitvoering van de timer callbacks heeft niet de niet-actieve tijd ingesteld op 0. De garbagecollection van de actor wordt echter worden uitgesteld totdat de timercallback uitvoering is voltooid.

## <a name="manually-deleting-actors-and-their-state"></a>Handmatig verwijderen van actoren en hun status
Garbagecollection van gedeactiveerde actoren alleen opschonen van de actorobject, maar deze gegevens die zijn opgeslagen in van een actor status Manager worden niet verwijderd. Wanneer een actor opnieuw worden geactiveerd, wordt de gegevens opnieuw beschikbaar gesteld aan via de status Manager. In gevallen waar actoren gegevens opslaan in de status Manager en zijn gedeactiveerd maar nooit opnieuw worden geactiveerd, is het mogelijk dat het nodig zijn om hun gegevens op te schonen.  Lees voor voorbeelden van het verwijderen van actoren, [actoren en hun status verwijderen](service-fabric-reliable-actors-delete-actors.md).

## <a name="next-steps"></a>Volgende stappen
* [Actor-timers en herinneringen](service-fabric-reliable-actors-timers-reminders.md)
* [Actor-gebeurtenissen](service-fabric-reliable-actors-events.md)
* [Herbetreedbaarheid actor](service-fabric-reliable-actors-reentrancy.md)
* [Actor-diagnose en bewaking van toepassingsprestaties](service-fabric-reliable-actors-diagnostics.md)
* [Actor-API-referentiedocumentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Voorbeeld van C#-code](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java-voorbeeldcode](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
