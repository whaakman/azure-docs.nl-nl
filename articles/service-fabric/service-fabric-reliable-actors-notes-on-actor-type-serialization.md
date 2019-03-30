---
title: Notities van betrouwbare actoren op actor serialisatie van actortypen | Microsoft Docs
description: Basisvereisten voor het definiëren van serialiseerbare klassen die kunnen worden gebruikt voor het definiëren van Service Fabric Reliable Actors Staten en interfaces worden beschreven
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 6e50e4dc-969a-4a1c-b36c-b292d964c7e3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: c8eeeb0ade6ca002adf3211cbf49127be9b76edb
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58667504"
---
# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Opmerkingen bij de Service Fabric Reliable Actors serialisatie van actortypen
De argumenten van alle methoden, resultaattypen van de taken die zijn geretourneerd door elke methode in een actorinterface en objecten die zijn opgeslagen in beheer van de status van een actor moet [data contract serialiseerbare](/dotnet/framework/wcf/feature-details/types-supported-by-the-data-contract-serializer). Dit geldt ook voor de argumenten van de methoden die zijn gedefinieerd [actor gebeurtenisinterfaces](service-fabric-reliable-actors-events.md). (Actor-interfacemethoden gebeurtenis altijd void retourneren.)

## <a name="custom-data-types"></a>Aangepaste gegevenstypen
In dit voorbeeld definieert de volgende actorinterface een methode die wordt geretourneerd van een aangepast gegevenstype met de naam `VoicemailBox`:

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

```Java
public interface VoiceMailBoxActor extends Actor
{
    CompletableFuture<VoicemailBox> getMailBoxAsync();
}
```

De interface is geïmplementeerd door een actor die gebruikmaakt van de status manager voor het opslaan van een `VoicemailBox` object:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class VoiceMailBoxActorImpl extends FabricActor implements VoicemailBoxActor
{
    public VoiceMailBoxActorImpl(ActorService actorService, ActorId actorId)
    {
         super(actorService, actorId);
    }

    public CompletableFuture<VoicemailBox> getMailBoxAsync()
    {
         return this.stateManager().getStateAsync("Mailbox");
    }
}

```

In dit voorbeeld wordt de `VoicemailBox` object is geserialiseerd als:

* Het object wordt tussen een actor-exemplaar en een aanroeper verzonden.
* Het object is opgeslagen in de staat waarin deze is opgeslagen op schijf en gerepliceerd naar andere knooppunten.

Het Reliable Actor-framework gebruikt DataContract serialisatie. Daarom de aangepaste objecten en hun leden moeten worden voorzien van aantekeningen met de **DataContract** en **DataMember** kenmerken, respectievelijk.

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```
```Java
public class Voicemail implements Serializable
{
    private static final long serialVersionUID = 42L;

    private UUID id;                    //getUUID() and setUUID()

    private String message;             //getMessage() and setMessage()

    private GregorianCalendar receivedAt; //getReceivedAt() and setReceivedAt()
}
```


```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```
```Java
public class VoicemailBox implements Serializable
{
    static final long serialVersionUID = 42L;
    
    public VoicemailBox()
    {
        this.messageList = new ArrayList<Voicemail>();
    }

    private List<Voicemail> messageList;   //getMessageList() and setMessageList()

    private String greeting;               //getGreeting() and setGreeting()
}
```


## <a name="next-steps"></a>Volgende stappen
* [Actor-levenscyclus en garbagecollection verzameling](service-fabric-reliable-actors-lifecycle.md)
* [Actor-timers en herinneringen](service-fabric-reliable-actors-timers-reminders.md)
* [Actor-gebeurtenissen](service-fabric-reliable-actors-events.md)
* [Herbetreedbaarheid actor](service-fabric-reliable-actors-reentrancy.md)
* [Actor-polymorfisme en objectgeoriënteerde ontwerppatronen](service-fabric-reliable-actors-polymorphism.md)
* [Actor-diagnose en bewaking van toepassingsprestaties](service-fabric-reliable-actors-diagnostics.md)
