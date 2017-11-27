---
title: Betrouwbare actoren timers en herinneringen | Microsoft Docs
description: Inleiding tot timers en herinneringen voor Service Fabric Reliable Actors.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 00c48716-569e-4a64-bd6c-25234c85ff4f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: f61743a7925c26767118dcb2d18799c26f880156
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="actor-timers-and-reminders"></a>Acteur timers en herinneringen
Actoren kunnen periodieke werk op zichzelf registreert timers of herinneringen plannen. Dit artikel laat zien hoe u timers en herinneringen en worden de verschillen uitgelegd.

## <a name="actor-timers"></a>Acteur timers
Acteur timers bieden een eenvoudige wrapper rond een timer .NET of Java om ervoor te zorgen dat de callback-methoden de concurrency Schakel gebaseerde naleven zorgt ervoor dat de runtime actoren biedt.

Actoren kunnen gebruiken de `RegisterTimer`(C#) of `registerTimer`(Java) en `UnregisterTimer`(C#) of `unregisterTimer`methoden op hun basisklasse registreren en ongedaan maken van hun timers (Java). Het volgende voorbeeld ziet het gebruik van de timer-API's. De API's zijn vergelijkbaar met de .NET-timer of Java-timer. In dit voorbeeld wanneer de timer vervalt, de runtime actoren roept de `MoveObject`(C#) of `moveObject`(Java)-methode. De methode is gegarandeerd gelijktijdigheid inschakelen op basis van de naleving. Dit betekent dat er geen andere actor-methoden of timer/herinnering retouraanroepen wordt uitgevoerd totdat deze retouraanroep uitvoering is voltooid.

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

De volgende periode van de timer wordt gestart nadat de callback uitvoering is voltooid. Dit betekent dat de timer wordt gestopt terwijl de retouraanroep wordt uitgevoerd en wordt gestart wanneer de callback is voltooid.

De runtime actoren Hiermee slaat u wijzigingen in de actor statusbeheer wanneer de callback is voltooid. Als een fout optreedt in de status wordt opgeslagen, dat actor-object wordt gedeactiveerd en een nieuw exemplaar wordt geactiveerd.

Alle timers zijn gestopt wanneer de actor als onderdeel van de garbagecollection wordt gedeactiveerd. Er is geen retouraanroepen timer worden na die aangeroepen. Ook behoudt de runtime actoren geen informatie over de timers die werden uitgevoerd voordat deactiveren. Het is tot de actor registreren van de vernieuwingstimer die nodig is wanneer deze in de toekomst opnieuw wordt geactiveerd. Zie voor meer informatie het gedeelte over [actor garbagecollection](service-fabric-reliable-actors-lifecycle.md).

## <a name="actor-reminders"></a>Acteur herinneringen
Herinneringen zijn een mechanisme voor het activeren van permanente retouraanroepen op een actor tijdstippen. De functionaliteit is vergelijkbaar met timers. Maar in tegenstelling tot timers, herinneringen onder alle omstandigheden worden geactiveerd totdat de actor expliciet deregistreren of de actor expliciet worden verwijderd. In het bijzonder worden herinneringen geactiveerd via actor deactivations en failovers, omdat de runtime actoren zich blijft voordoen informatie over de actor herinneringen met acteur state-provider. Houd er rekening mee dat de betrouwbaarheid van herinneringen is gekoppeld aan de status betrouwbaarheid garanties geleverd door de actor state-provider. Dit betekent dat voor actoren waarvan persistentie van de status is ingesteld op None, de herinneringen niet na een failover gestart wordt. 

Voor het registreren van een herinnering een actor roept de `RegisterReminderAsync` methode opgegeven op de basisklasse, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
protected override async Task OnActivateAsync()
{
    string reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    IActorReminder reminderRegistration = await this.RegisterReminderAsync(
        reminderName,
        BitConverter.GetBytes(amountInDollars),
        TimeSpan.FromDays(3),
        TimeSpan.FromDays(1));
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

In dit voorbeeld `"Pay cell phone bill"` is de naam herinnering. Dit is een tekenreeks die de actor wordt gebruikt om een herinnering uniek te identificeren. `BitConverter.GetBytes(amountInDollars)`(C#) is de context die is gekoppeld aan de herinnering. Deze wordt doorgegeven naar de actor als een argument voor de callback herinnering dat wil zeggen `IRemindable.ReceiveReminderAsync`(C#) of `Remindable.receiveReminderAsync`(Java).

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

Wanneer een herinnering wordt geactiveerd, de runtime Reliable Actors roept de `ReceiveReminderAsync`(C#) of `receiveReminderAsync`(Java)-methode op de Actor. Een actor meerdere herinneringen kunt registreren en de `ReceiveReminderAsync`(C#) of `receiveReminderAsync`(Java)-methode wordt aangeroepen wanneer een van deze herinneringen is geactiveerd. De actor kunt de naam van de herinnering die is doorgegeven aan de `ReceiveReminderAsync`(C#) of `receiveReminderAsync`(Java)-methode om te achterhalen welke herinnering is geactiveerd.

De runtime de actor slaat actoren status wanneer de `ReceiveReminderAsync`(C#) of `receiveReminderAsync`(Java)-aanroep is voltooid. Als een fout optreedt in de status wordt opgeslagen, dat actor-object wordt gedeactiveerd en een nieuw exemplaar wordt geactiveerd.

Als u wilt een herinnering registratie, een actor roept de `UnregisterReminderAsync`(C#) of `unregisterReminderAsync`(Java)-methode, zoals wordt weergegeven in de volgende voorbeelden.

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```
```Java
ActorReminder reminder = getReminder("Pay cell phone bill");
CompletableFuture reminderUnregistration = unregisterReminderAsync(reminder);
```

Zoals hierboven, de `UnregisterReminderAsync`(C#) of `unregisterReminderAsync`(Java) methode accepteert een `IActorReminder`(C#) of `ActorReminder`(Java)-interface. De basisklasse actor ondersteunt een `GetReminder`(C#) of `getReminder`(Java)-methode die kan worden gebruikt voor het ophalen van de `IActorReminder`(C#) of `ActorReminder`interface door door te geven in de naam van de herinnering (Java). Dit is handig zijn, omdat de actor niet hoeft in stand houden de `IActorReminder`(C#) of `ActorReminder`(Java)-interface die is geretourneerd van de `RegisterReminder`(C#) of `registerReminder`methodeaanroep (Java).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over betrouwbare Actor gebeurtenissen en herintreding:
* [Actor-gebeurtenissen](service-fabric-reliable-actors-events.md)
* [Acteur herintreding](service-fabric-reliable-actors-reentrancy.md)
