---
title: Maken van uw eerste Azure op basis van actor-microservice in C# | Microsoft Docs
description: Deze zelfstudie leert u de stappen voor het maken en implementeren van een eenvoudige actor-service met behulp van Service Fabric Reliable Actors foutopsporing.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: d4aebe72-1551-4062-b1eb-54d83297f139
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/16/2018
ms.author: vturecek
ms.openlocfilehash: 32d3fa09c863c47753267e97e7c4730dff869887
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="getting-started-with-reliable-actors"></a>Aan de slag met Reliable Actors
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-actors-get-started.md)
> * [Java op Linux](service-fabric-reliable-actors-get-started-java.md)

Dit artikel begeleidt bij het maken en een eenvoudige betrouwbare Actor-toepassing in Visual Studio-foutopsporing. Zie voor meer informatie over Reliable Actors [Inleiding tot Service Fabric Reliable Actors](service-fabric-reliable-actors-introduction.md).

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat u de Service Fabric-ontwikkelomgeving, met inbegrip van Visual Studio op uw computer instellen. Zie voor meer informatie [het instellen van de ontwikkelomgeving](service-fabric-get-started.md).

## <a name="create-a-new-project-in-visual-studio"></a>Maak een nieuw project in Visual Studio

Start Visual Studio 2015 of hoger als beheerder en maak vervolgens een nieuwe **Service Fabric-toepassing** project:

![Service Fabric-tools voor Visual Studio - nieuw project][1]

Kies in het volgende dialoogvenster **Actor Service** onder **.Net Core 2.0** en voer een naam voor de service.

![Service Fabric-projectsjablonen][5]

Het gemaakte project ziet u de volgende structuur:

![De structuur van de service Fabric-project][2]

## <a name="examine-the-solution"></a>Bekijk de oplossing

De oplossing bevat drie projecten:

* **Het toepassingsproject (Mijntoepassing)**. Dit project pakketten alle services samen voor implementatie. Bevat de *ApplicationManifest.xml* en PowerShell-scripts voor het beheren van de toepassing.

* **De interface-project (HelloWorld.Interfaces)**. Dit project bevat de interfacedefinitie voor de actor. Actor-interfaces kunnen worden gedefinieerd in een project met een willekeurige naam.  De interface definieert het contract actor die wordt gedeeld door de actor-implementatie en de clients de actor aanroepen.  Omdat klantprojecten mogelijk afhankelijk, zinvol het doorgaans om te definiëren in een assembly die is gescheiden van de actor-implementatie.

* **Het serviceproject actor (HelloWorld)**. Dit project definieert de Service Fabric-service die als host voor de actor gaat. Bevat de implementatie van de actor *HellowWorld.cs*. Een actor-implementatie is een klasse die is afgeleid van het basistype `Actor` en implementeert de interfaces die zijn gedefinieerd in de *MyActor.Interfaces* project. Een actor-klasse moet ook een constructor implementeren een `ActorService` exemplaar en een `ActorId` en geeft deze door aan de base `Actor` klasse.
    
    Dit project bevat ook *Program.cs*, die klassen actor registreert bij de Service Fabric-runtime gebruiken `ActorRuntime.RegisterActorAsync<T>()`. De `HelloWorld` klasse is al geregistreerd. De aanvullende actor implementaties die zijn toegevoegd aan het project moeten ook zijn geregistreerd in de `Main()` methode.

## <a name="customize-the-helloworld-actor"></a>De HelloWorld actor aanpassen

De projectsjabloon definieert een aantal methoden in de `IHelloWorld` interface en implementeert u ze op in de `HelloWorld` actor-implementatie.  Vervang deze methoden, zodat de actor-service een eenvoudige 'Hallo wereld'-tekenreeks retourneert.

In de *HelloWorld.Interfaces* project in de *IHelloWorld.cs* bestand, vervangt de interfacedefinitie als volgt:

```csharp
public interface IHelloWorld : IActor
{
    Task<string> GetHelloWorldAsync();
}
```

In de **HelloWorld** project in **HelloWorld.cs**, vervang de volledige klassendefinitie als volgt:

```csharp
[StatePersistence(StatePersistence.Persisted)]
internal class HelloWorld : Actor, IHelloWorld
{
    public HelloWorld(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> GetHelloWorldAsync()
    {
        return Task.FromResult("Hello from my reliable actor!");
    }
}
```

Druk op **Ctrl-Shift-B** bouw het project en zorg ervoor dat alles wordt gecompileerd.

## <a name="add-a-client"></a>Toevoegen van een client

Maak een eenvoudige consoletoepassing de actor-service aanroepen.

1. Met de rechtermuisknop op de oplossing in Solution Explorer > **toevoegen** > **nieuw Project...** .

2. Onder de **.NET Core** projecttypen, kiest u **Console-App (.NET Core)**.  Noem het project *ActorClient*.
    
    ![Dialoogvenster Nieuw Project toevoegen][6]    
    
    > [!NOTE]
    > Een consoletoepassing is niet het type app die u vaak als een Service Fabric-client gebruikt, maar het voorbeeld van een handige voor foutopsporing en testen met behulp van de lokale Service Fabric-cluster maakt.

3. De consoletoepassing moet een 64-bits toepassing voor compatibiliteit met de interface-project en andere afhankelijkheden.  Klik in Solution Explorer met de rechtermuisknop op de **ActorClient** project en klik vervolgens op **eigenschappen**.  Op de **bouwen** tabblad, stelt u **Platform doel** naar **x64**.
    
    ![Eigenschappen bouwen][8]

4. Het clientproject vereist het betrouwbare actoren NuGet-pakket.  Klik op **Hulpprogramma's** > **NuGet Package Manager** > **Package Manager-console**.  Voer de volgende opdracht in de Package Manager-Console:
    
    ```powershell
    Install-Package Microsoft.ServiceFabric.Actors -IncludePrerelease -ProjectName ActorClient
    ```

    Het NuGet-pakket en de afhankelijkheden ervan zijn in het project ActorClient geïnstalleerd.

5. Het clientproject is ook vereist een verwijzing naar het project interfaces.  Met de rechtermuisknop in het project ActorClient **afhankelijkheden** en klik vervolgens op **verwijzing toevoegen...** .  Selecteer **projecten > oplossing** (indien niet geselecteerd), en vervolgens het selectievakje in naast voor maatstreepjes **HelloWorld.Interfaces**.  Klik op **OK**.
    
    ![Dialoogvenster verwijzing toevoegen][7]

6. Vervang de volledige inhoud van in het project ActorClient *Program.cs* met de volgende code:
    
    ```csharp
    using System;
    using System.Threading.Tasks;
    using Microsoft.ServiceFabric.Actors;
    using Microsoft.ServiceFabric.Actors.Client;
    using HelloWorld.Interfaces;
    
    namespace ActorClient
    {
        class Program
        {
            static void Main(string[] args)
            {
                IHelloWorld actor = ActorProxy.Create<IHelloWorld>(ActorId.CreateRandom(), new Uri("fabric:/MyApplication/HelloWorldActorService"));
                Task<string> retval = actor.GetHelloWorldAsync();
                Console.Write(retval.Result);
                Console.ReadLine();
            }
        }
    }
    ```

## <a name="running-and-debugging"></a>Uitvoeren en foutopsporing

Druk op **F5** wilt maken, implementeren en de toepassing lokaal uitvoeren in het cluster Service Fabric-ontwikkeling.  Tijdens de implementatie, ziet u de voortgang op het **uitvoer** venster.

![Service Fabric foutopsporing uitvoervenster][3]

Wanneer de uitvoer de tekst bevat, *de toepassing gereed is*, is het mogelijk voor het testen van de service met behulp van de toepassing ActorClient.  Klik in Solution Explorer met de rechtermuisknop op de **ActorClient** project en klik vervolgens op **Debug** > **nieuw exemplaar gestart**.  De uitvoer van de actor-service moet worden weergegeven in de toepassing vanaf de opdrachtregel.

![uitvoer van de toepassing][9]

> [!TIP]
> De Service Fabric actoren runtime verzendt sommige [gebeurtenissen en prestatiemeteritems met betrekking tot actor methoden](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Ze zijn nuttig zijn bij de diagnostische gegevens en prestatiebewaking.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [hoe Reliable Actors gebruiken voor het Service Fabric-platform](service-fabric-reliable-actors-platform.md).


[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
[6]: ./media/service-fabric-reliable-actors-get-started/new-console-app.png
[7]: ./media/service-fabric-reliable-actors-get-started/add-reference.png
[8]: ./media/service-fabric-reliable-actors-get-started/build-props.png
[9]: ./media/service-fabric-reliable-actors-get-started/app-output.png