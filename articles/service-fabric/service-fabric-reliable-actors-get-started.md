---
title: Maken van uw eerste Azure op basis van actor-microservice in C# | Microsoft Docs
description: Deze zelfstudie leert u de stappen voor het maken en implementeren van een eenvoudige actor-service met behulp van Service Fabric Reliable Actors foutopsporing.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d4aebe72-1551-4062-b1eb-54d83297f139
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 3f447e049ccd33c77f422e8aa703ad6646f9ffa2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="getting-started-with-reliable-actors"></a>Aan de slag met Reliable Actors
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-actors-get-started.md)
> * [Java op Linux](service-fabric-reliable-actors-get-started-java.md)
> 
> 

Dit artikel vindt u de basisbeginselen van Azure Service Fabric Reliable Actors en leidt u door het maken en implementeren van een eenvoudige betrouwbare Actor-toepassing in Visual Studio-foutopsporing.

## <a name="installation-and-setup"></a>Installatie en configuratie
Voordat u begint, zorg ervoor dat u de Service Fabric-ontwikkelomgeving instellen op uw computer.
Als u instellen wilt, raadpleegt u gedetailleerde instructies op [het instellen van de ontwikkelomgeving](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Basisbegrippen
Om aan de slag te gaan met Reliable Actors hoeft u slechts enkele basisbegrippen te kennen:

* **Actor-service**. Reliable Actors zijn verpakt in Reliable Services die in de Service Fabric-infrastructuur kunnen worden geïmplementeerd. Actor-exemplaren worden geactiveerd in een benoemd service-exemplaar.
* **Actor-registratie**. Net als Reliable Services moet een Reliable Actor-service worden geregistreerd bij de Service Fabric-runtime. Daarnaast moet het actortype worden geregistreerd bij de Actor-runtime.
* **Actor-interface**. De Actor-interface wordt gebruikt voor het definiëren van een sterk getypeerde openbare interface van een actor. In de terminologie van Reliable Actor-modellen definieert de actorinterface de typen berichten die de actor kan begrijpen en verwerken. De actorinterface wordt door andere actors en clienttoepassingen gebruikt om (asynchroon) berichten naar de actor te 'verzenden'. Reliable Actors kunnen meerdere interfaces implementeren.
* **Klasse ActorProxy**. De klasse ActorProxy wordt door clienttoepassingen gebruikt voor het aanroepen van de methoden die toegankelijk zijn via de actorinterface. De klasse ActorProxy heeft twee belangrijke functies:
  
  * Naamomzetting: de klasse kan de actor in het cluster vinden (het knooppunt van het cluster waar de actor wordt gehost).
  * Foutafhandeling: de klasse kan methodes opnieuw proberen aan te roepen en de locatie van een actor opnieuw omzetten, bijvoorbeeld na een storing waardoor de actor is verplaatst naar een ander knooppunt in het cluster.

De volgende regels met betrekking tot actorinterfaces zijn het noemen waard:

* Actor-interfacemethoden kunnen niet worden overbelast.
* Actor-interfacemethoden mogen geen out-, ref- of optionele parameters hebben.
* Algemene interfaces worden niet ondersteund.

## <a name="create-a-new-project-in-visual-studio"></a>Maak een nieuw project in Visual Studio
Start Visual Studio 2015 of Visual Studio 2017 als beheerder en maak een nieuw project voor Service Fabric-toepassing:

![Service Fabric-tools voor Visual Studio - nieuw project][1]

In het volgende dialoogvenster kunt u het type project dat u wilt maken.

![Service Fabric-projectsjablonen][5]

We gebruiken de Reliable Actors van Service Fabric-service voor het project HelloWorld.

Nadat u de oplossing hebt gemaakt, ziet u de volgende structuur:

![De structuur van de service Fabric-project][2]

## <a name="reliable-actors-basic-building-blocks"></a>Basisbouwstenen van Reliable Actors
Een typische Reliable Actors oplossing bestaat uit drie projecten:

* **Het toepassingsproject (MyActorApplication)**. Dit is het project uit dat alle services samen voor de implementatie van pakketten. Bevat de *ApplicationManifest.xml* en PowerShell-scripts voor het beheren van de toepassing.
* **De interface-project (MyActor.Interfaces)**. Dit is het project met de interfacedefinitie van de voor de actor. U kunt de interfaces die worden gebruikt door de actoren in de oplossing definiëren in het project MyActor.Interfaces. Uw actor-interfaces kunnen worden gedefinieerd in een project met een naam, maar de interface het contract actor die wordt gedeeld door de actor-implementatie en het aanroepen van de actor definieert dus meestal is het verstandig om te definiëren in een assembly die is gescheiden van clients de implementatie van de actor en kunnen worden gedeeld door meerdere andere projecten.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **Het serviceproject actor (MyActor)**. Dit is het project dat wordt gebruikt voor het definiëren van de Service Fabric-service die als host voor de actor gaat. De uitvoering van de actor bevat. Een actor-implementatie is een klasse die is afgeleid van het basistype `Actor` en implementeert de interface (s) die zijn gedefinieerd in het project MyActor.Interfaces. Een actor-klasse moet ook een constructor implementeren een `ActorService` exemplaar en een `ActorId` en geeft deze door aan de base `Actor` klasse. Hierdoor constructor afhankelijkheidsinjectie van platform afhankelijkheden.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

De actorservice moet met een servicetype worden geregistreerd in de Service Fabric-runtime. De Actor-service kan uw actorexemplaren alleen uitvoeren als ook het actortype is geregistreerd bij de Actor-service. De `ActorRuntime`-registratiemethode doet dit voor actors.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Als u vanaf een nieuw project in Visual Studio opstarten en hebt u slechts één definitie van acteur, is de registratie standaard opgenomen in de code die Visual Studio gegenereerd. Als u andere actoren in de service definiëren, moet u de registratie actor toevoegen met behulp van:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [!TIP]
> De Service Fabric actoren runtime verzendt sommige [gebeurtenissen en prestatiemeteritems met betrekking tot actor methoden](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Ze zijn nuttig zijn bij de diagnostische gegevens en prestatiebewaking.
> 
> 

## <a name="debugging"></a>Foutopsporing
De Service Fabric-tools voor Visual Studio ondersteuning voor foutopsporing op uw lokale machine. U kunt een sessie voor foutopsporing starten kunt u door op de toets F5. Visual Studio maakt (indien nodig) pakketten. Ook de toepassing op de lokale Service Fabric-cluster worden geïmplementeerd en wordt het foutopsporingsprogramma.

Tijdens de implementatie, ziet u de voortgang op het **uitvoer** venster.

![Service Fabric foutopsporing uitvoervenster][3]

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [hoe Reliable Actors gebruiken voor het Service Fabric-platform](service-fabric-reliable-actors-platform.md).

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
