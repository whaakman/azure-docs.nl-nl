---
title: Maken van een actor-service in Azure Service Fabric | Microsoft Docs
description: Informatie over het maken, fouten opsporen en implementeren van uw eerste service op basis van een actor in met behulp van Service Fabric Reliable Actors voor C#.
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
ms.openlocfilehash: f92fe2432051b148bf0b35fccc3fa33db9b66a14
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55093142"
---
# <a name="getting-started-with-reliable-actors"></a>Aan de slag met Reliable Actors
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-actors-get-started.md)
> * [Java op Linux](service-fabric-reliable-actors-get-started-java.md)

Dit artikel helpt bij het maken van en opsporen van fouten in een eenvoudige Reliable actors-toepassing in Visual Studio. Zie voor meer informatie over Reliable Actors [Inleiding tot Service Fabric Reliable Actors](service-fabric-reliable-actors-introduction.md).

## <a name="prerequisites"></a>Vereisten

Voordat u begint, controleert u of de Service Fabric-ontwikkelomgeving, met inbegrip van Visual Studio, instellen op uw computer. Zie voor meer informatie, [over het instellen van de ontwikkelomgeving](service-fabric-get-started.md).

## <a name="create-a-new-project-in-visual-studio"></a>Maak een nieuw project in Visual Studio

Start Visual Studio 2015 of hoger als beheerder en maak vervolgens een nieuwe **Service Fabric-toepassing** project:

![Service Fabric tools voor Visual Studio - nieuw project][1]

Kies in het volgende dialoogvenster **Actorservice** onder **.Net Core 2.0** en voer een naam voor de service.

![Service Fabric-projectsjablonen][5]

Het project bevat de volgende structuur:

![De structuur van de service Fabric-project][2]

## <a name="examine-the-solution"></a>Bekijk de oplossing

De oplossing bevat drie projecten:

* **Het toepassingsproject (MyApplication)**. Dit project pakketten alle services samen voor implementatie. Deze bevat de *ApplicationManifest.xml* en PowerShell-scripts voor het beheren van de toepassing.

* **De interfaceproject (HelloWorld.Interfaces)**. Dit project bevat de interfacedefinitie voor de actor. Actor-interfaces kunnen worden gedefinieerd in een project met een willekeurige naam.  De interface definieert het actorcontract dat wordt gedeeld met de actorimplementatie en de clients die de actor aanroepen.  Omdat afhankelijk van het client-projecten zijn, zinvol het doorgaans om te definiëren in een assembly die is gescheiden van de actorimplementatie.

* **De actor-service-project (HelloWorld)**. Dit project definieert de Service Fabric-service die u voor het hosten van de actor. Deze bevat de implementatie van de actor *HelloWorld.cs*. De actorimplementatie van een is een klasse die is afgeleid van het basistype `Actor` en implementeert de interfaces die zijn gedefinieerd in de *MyActor.Interfaces* project. Een actor-klasse moet ook implementeren om een constructor die accepteert een `ActorService` exemplaar en een `ActorId` en geeft deze door aan de base `Actor` klasse.
    
    Dit project bevat ook *Program.cs*, waardoor actor klassen wordt geregistreerd met de Service Fabric-runtime via `ActorRuntime.RegisterActorAsync<T>()`. De `HelloWorld` klasse is al geregistreerd. De aanvullende actor implementaties die zijn toegevoegd aan het project moeten ook worden geregistreerd de `Main()` methode.

## <a name="customize-the-helloworld-actor"></a>De actor HelloWorld aanpassen

De projectsjabloon, maken definieert sommige methoden in de `IHelloWorld` interface en implementeert u deze in de `HelloWorld` actorimplementatie.  Vervang deze methoden, zodat de actor-service een eenvoudige 'Hallo wereld'-tekenreeks retourneert.

In de *HelloWorld.Interfaces* project in de *IHelloWorld.cs* bestand, vervangt u de interfacedefinitie als volgt:

```csharp
public interface IHelloWorld : IActor
{
    Task<string> GetHelloWorldAsync();
}
```

In de **HelloWorld** project in **HelloWorld.cs**, vervangt u de definitie van de gehele klasse als volgt:

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

Druk op **Ctrl-Shift-B** aan het project te bouwen en te controleren of alles wordt gecompileerd.

## <a name="add-a-client"></a>Een client toevoegen

Maak een eenvoudige consoletoepassing voor het aanroepen van de actor-service.

1. Met de rechtermuisknop op de oplossing in Solution Explorer > **toevoegen** > **nieuw Project...** .

2. Onder de **.NET Core** projecttypen, kiest u **Console-App (.NET Core)**.  Noem het project *ActorClient*.
    
    ![Dialoogvenster Nieuw Project toevoegen][6]    
    
    > [!NOTE]
    > Een consoletoepassing is niet het type van de app die u vaak als een client in Service Fabric gebruikt, maar het is een eenvoudig voorbeeld voor foutopsporing en testen met behulp van de lokale Service Fabric-cluster.

3. De consoletoepassing moet een 64-bits-toepassing voor compatibiliteit met de interfaceproject en andere afhankelijkheden.  Klik in Solution Explorer met de rechtermuisknop op de **ActorClient** project en klik vervolgens op **eigenschappen**.  Op de **bouwen** tabblad **Platform doel** naar **x64**.
    
    ![Eigenschappen bouwen][8]

4. Het clientproject vereist het NuGet-pakket voor betrouwbare actoren.  Klik op **Hulpprogramma's** > **NuGet Package Manager** > **Package Manager-console**.  Voer de volgende opdracht in de Package Manager-Console:
    
    ```powershell
    Install-Package Microsoft.ServiceFabric.Actors -IncludePrerelease -ProjectName ActorClient
    ```

    Het NuGet-pakket en alle afhankelijkheden ervan zijn in het project ActorClient geïnstalleerd.

5. Het clientproject vereist ook een verwijzing naar het project interfaces.  Met de rechtermuisknop in het project ActorClient **afhankelijkheden** en klik vervolgens op **verwijzing toevoegen...** .  Selecteer **projecten > oplossing** (indien nog niet is ingeschakeld), en vervolgens het selectievakje in naast voor maatstreepjes **HelloWorld.Interfaces**.  Klik op **OK**.
    
    ![Dialoogvenster toevoegen][7]

6. Vervang de volledige inhoud van in het project ActorClient *Program.cs* door de volgende code:
    
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

Druk op **F5** bouwen, implementeren en de toepassing lokaal uitvoeren in de ontwikkeling van Service Fabric-cluster.  Tijdens de implementatie, ziet u de voortgang in de **uitvoer** venster.

![Service Fabric het uitvoervenster van foutopsporing][3]

Wanneer de uitvoer de tekst bevat, *de toepassing gereed is*, is het mogelijk om de service met behulp van de toepassing ActorClient te testen.  Klik in Solution Explorer met de rechtermuisknop op de **ActorClient** project en klik vervolgens op **Debug** > **nieuwe instantie starten**.  De uitvoer van de actor-service moet worden weergegeven in de toepassing vanaf de opdrachtregel.

![Uitvoer van de toepassing][9]

> [!TIP]
> De runtime Service Fabric-actoren verzendt sommige [gebeurtenissen en prestatiemeteritems die betrekking hebben op de actor methoden](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Ze zijn nuttig in diagnostische gegevens en bewaking van toepassingsprestaties.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [hoe Reliable Actors het Service Fabric-platform gebruiken](service-fabric-reliable-actors-platform.md).


[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
[6]: ./media/service-fabric-reliable-actors-get-started/new-console-app.png
[7]: ./media/service-fabric-reliable-actors-get-started/add-reference.png
[8]: ./media/service-fabric-reliable-actors-get-started/build-props.png
[9]: ./media/service-fabric-reliable-actors-get-started/app-output.png