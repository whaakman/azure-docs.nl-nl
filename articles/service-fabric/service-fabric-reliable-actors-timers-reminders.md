---
title: Betrouwbare actoren timers en herinneringen | Microsoft Docs
description: Inleiding tot timers en herinneringen voor Service Fabric Reliable Actors.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: amanbha
ms.assetid: 00c48716-569e-4a64-bd6c-25234c85ff4f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 323de842645cced3c6f490e98112fcbcd184aa64
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58667426"
---
# <a name="actor-timers-and-reminders"></a>Actor-timers en herinneringen
Actoren kunnen periodieke werk op zichzelf door het registreren van timers of herinneringen plannen. In dit artikel ziet u hoe u timers en herinneringen en worden de verschillen tussen deze uitgelegd.

## <a name="actor-timers"></a>Actor-timers
Actor-timers bieden een eenvoudige wrapper rond een timer .NET of Java om ervoor te zorgen dat de callback-methoden aansluiten bij de gelijktijdigheid van de beurt zorgt ervoor dat de runtime actoren biedt.

Actoren kunnen gebruiken de `RegisterTimer`(C#) of `registerTimer`(Java) en `UnregisterTimer`(C#) of `unregisterTimer`methoden op hun basisklasse te registreren en de registratie van hun timers (Java). In het volgende voorbeeld ziet u het gebruik van de timer-API's. De API's zijn vergelijkbaar met de timer voor .NET of Java-timer. In dit voorbeeld, wanneer de timer loopt, de runtime actoren roept de `MoveObject`(C#) of `moveObject`(Java)-methode. De methode kan worden gegarandeerd rekening houden met de gelijktijdigheid van de beurt. Dit betekent dat er geen andere actor methoden of timer/herinnering callbacks uitgevoerd zijn totdat deze aanroep is voltooid.

```csharp
class VisualObjectActor : Actor, IVisualObject
{
    private IActorTimer _updateTimer;

    public VisualObjectActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    protected override Task OnActivateAsync()
    {
        ...

        _updateTimer = RegisterTimer(
            MoveObject,                     // Callback method
            null,                           // Parameter to pass to the callback method
            TimeSpan.FromMilliseconds(15),  // Amount of time to delay before the callback is invoked
            TimeSpan.FromMilliseconds(15)); // Time interval between invocations of the callback method

        return base.OnActivateAsync();
    }

    protected override Task OnDeactivateAsync()
    {
        if (_updateTimer != null)
        {
            UnregisterTimer(_updateTimer);
        }

        return base.OnDeactivateAsync();
    }

    private Task MoveObject(object state)
    {
        ...
        return Task.FromResult(true);
    }
}
```
```Java
public class VisualObjectActorImpl extends FabricActor implements VisualObjectActor
{
    private ActorTimer updateTimer;

    public VisualObjectActorImpl(FabricActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    @Override
    protected CompletableFuture onActivateAsync()
    {
        ...

        return this.stateManager()
                .getOrAddStateAsync(
                        stateName,
                        VisualObject.createRandom(
                                this.getId().toString(),
                                new Random(this.getId().toString().hashCode())))
                .thenApply((r) -> {
                    this.registerTimer(
                            (o) -> this.moveObject(o),                        // Callback method
                            "moveObject",
                            null,                                             // Parameter to pass to the callback method
                            Duration.ofMillis(10),                            // Amount of time to delay before the callback is invoked
                            Duration.ofMillis(timerIntervalInMilliSeconds));  // Time interval between invocations of the callback method
                    return null;
                });
    }

    @Override
    protected CompletableFuture onDeactivateAsync()
    {
        if (updateTimer != null)
        {
            unregisterTimer(updateTimer);
        }

        return super.onDeactivateAsync();
    }

    private CompletableFuture moveObject(Object state)
    {
        ...
        return this.stateManager().getStateAsync(this.stateName).thenCompose(v -> {
            VisualObject v1 = (VisualObject)v;
            v1.move();
            return (CompletableFuture<?>)this.stateManager().setStateAsync(stateName, v1).
                    thenApply(r -> {
                      ...
                      return null;});
        });
    }
}
```

De volgende periode van de timer wordt gestart nadat de callback uitvoering is voltooid. Dit betekent dat de timer wordt gestopt terwijl de callback die wordt uitgevoerd en wordt gestart wanneer de retouraanroep is voltooid.

De runtime actoren slaat wijzigingen aangebracht in van de actor status Manager wanneer de retouraanroep is voltooid. Als er een fout optreedt bij het opslaan van de status, dat actorobject wordt gedeactiveerd en een nieuw exemplaar wordt geactiveerd.

Timers worden beëindigd wanneer de actor als onderdeel van de garbagecollection is gedeactiveerd. Geen callbacks timer worden die aangeroepen. Ook behoudt de runtime actoren geen informatie over de timers die werden uitgevoerd voordat de deactiveren. Het is aan de actor voor het registreren van elke timers die nodig is wanneer deze in de toekomst opnieuw wordt geactiveerd. Zie voor meer informatie de sectie over [actor garbagecollection](service-fabric-reliable-actors-lifecycle.md).

## <a name="actor-reminders"></a>Actor-herinneringen
Herinneringen zijn een mechanisme voor het activeren van permanente callbacks van een actor tijdstippen. De functionaliteit is vergelijkbaar met timers. Maar in tegenstelling tot timers, herinneringen onder alle omstandigheden worden geactiveerd totdat de actor expliciet registratie of de actor is expliciet worden verwijderd. Herinneringen zijn met name in actor deactivations en failovers geactiveerd omdat de runtime actoren zich blijft voordoen informatie over het actor state-provider met behulp van de actor-herinneringen. Houd er rekening mee dat de betrouwbaarheid van herinneringen is gebonden aan de status betrouwbaarheid garanties geboden door de actor state-provider. Dit betekent dat dit soort waarvan persistentie ingeschakeld is ingesteld op None, de herinneringen niet na een failover gestart worden. 

Voor het registreren van een herinnering, een actor roept de `RegisterReminderAsync` methode opgegeven op de basisklasse, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
protected override async Task OnActivateAsync()
{
    string reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    IActorReminder reminderRegistration = await this.RegisterReminderAsync(
        reminderName,
        BitConverter.GetBytes(amountInDollars),
        TimeSpan.FromDays(3),    //The amount of time to delay before firing the reminder
        TimeSpan.FromDays(1));    //The time interval between firing of reminders
}
```

```Java
@Override
protected CompletableFuture onActivateAsync()
{
    String reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    ActorReminder reminderRegistration = this.registerReminderAsync(
            reminderName,
            state,
            dueTime,    //The amount of time to delay before firing the reminder
            period);    //The time interval between firing of reminders
}
```

In dit voorbeeld `"Pay cell phone bill"` is de naam van de herinnering. Dit is een tekenreeks die de actor wordt gebruikt voor het aanduiden van een herinnering. `BitConverter.GetBytes(amountInDollars)`(C#) is de context die is gekoppeld aan de herinnering. Het zal worden teruggestuurd naar de actor als een argument voor de herinnering callback, dat wil zeggen `IRemindable.ReceiveReminderAsync`(C#) of `Remindable.receiveReminderAsync`(Java).

Actoren die gebruikmaken van herinneringen moeten implementeren de `IRemindable` interface, zoals wordt weergegeven in het onderstaande voorbeeld.

```csharp
public class ToDoListActor : Actor, IToDoListActor, IRemindable
{
    public ToDoListActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task ReceiveReminderAsync(string reminderName, byte[] context, TimeSpan dueTime, TimeSpan period)
    {
        if (reminderName.Equals("Pay cell phone bill"))
        {
            int amountToPay = BitConverter.ToInt32(context, 0);
            System.Console.WriteLine("Please pay your cell phone bill of ${0}!", amountToPay);
        }
        return Task.FromResult(true);
    }
}
```
```Java
public class ToDoListActorImpl extends FabricActor implements ToDoListActor, Remindable
{
    public ToDoListActor(FabricActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture receiveReminderAsync(String reminderName, byte[] context, Duration dueTime, Duration period)
    {
        if (reminderName.equals("Pay cell phone bill"))
        {
            int amountToPay = ByteBuffer.wrap(context).getInt();
            System.out.println("Please pay your cell phone bill of " + amountToPay);
        }
        return CompletableFuture.completedFuture(true);
    }

```

Wanneer een herinnering wordt geactiveerd, de Reliable Actors-runtime roept de `ReceiveReminderAsync`(C#) of `receiveReminderAsync`(Java)-methode op de actor aangeroepen. Een actor meerdere herinneringen kunt registreren en de `ReceiveReminderAsync`(C#) of `receiveReminderAsync`(Java)-methode wordt aangeroepen wanneer een van deze herinneringen wordt geactiveerd. De actor kunt de naam van de herinnering die is doorgegeven aan de `ReceiveReminderAsync`(C#) of `receiveReminderAsync`(Java)-methode om te achterhalen welke herinnering is geactiveerd.

De runtime van de actor slaat actoren status wanneer de `ReceiveReminderAsync`(C#) of `receiveReminderAsync`(Java)-aanroep is voltooid. Als er een fout optreedt bij het opslaan van de status, dat actorobject wordt gedeactiveerd en een nieuw exemplaar wordt geactiveerd.

Als u wilt een herinnering de registratie ongedaan maken, een actor roept de `UnregisterReminderAsync`(C#) of `unregisterReminderAsync`(Java)-methode, zoals wordt weergegeven in de onderstaande voorbeelden.

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```
```Java
ActorReminder reminder = getReminder("Pay cell phone bill");
CompletableFuture reminderUnregistration = unregisterReminderAsync(reminder);
```

Zoals hierboven, de `UnregisterReminderAsync`(C#) of `unregisterReminderAsync`(Java)-methode accepteert een `IActorReminder`(C#) of `ActorReminder`(Java)-interface. De actor basisklasse ondersteunt een `GetReminder`(C#) of `getReminder`(Java)-methode die kan worden gebruikt om op te halen de `IActorReminder`(C#) of `ActorReminder`(Java)-interface door te geven in de naam van de herinnering. Dit is handig omdat de actor niet hoeft om vast te leggen de `IActorReminder`(C#) of `ActorReminder`(Java)-interface die is geretourneerd van de `RegisterReminder`(C#) of `registerReminder`methodeaanroep (Java).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over Reliable Actor-gebeurtenissen en herbetreedbaarheid van:
* [Actor-gebeurtenissen](service-fabric-reliable-actors-events.md)
* [Herbetreedbaarheid actor](service-fabric-reliable-actors-reentrancy.md)
