---
title: Eenheidstests voor stateful services in Azure Service Fabric ontwikkelen | Microsoft Docs
description: Informatie over het ontwikkelen van eenheidstests voor Service Fabric-Stateful Services.
services: service-fabric
documentationcenter: .net
author: charleszipp
manager: timlt
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/04/2018
ms.author: ryanwi
ms.openlocfilehash: 945cdf63a178a09f121f355aaa7635537e46e5ff
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43703706"
---
# <a name="create-unit-tests-for-stateful-services"></a>Eenheidstests voor Stateful Services maken
Eenheidstesten stateful Service Fabric-services kunt verwerven op basis veelvoorkomende fouten die zou niet per se worden opgepikt door conventionele toepassing of domeinspecifieke Eenheidstesten. Tijdens het ontwikkelen van eenheidstests voor stateful services, zijn er enkele speciale overwegingen die moeten worden opgeslagen in gedachten.

1. Elke replica toepassingscode wordt uitgevoerd, maar in verschillende context. Als de service gebruikmaakt van drie replica's, wordt de servicecode wordt uitgevoerd op drie knooppunten parallel onder andere context/rol.
2. Status die zijn opgeslagen in de stateful service moet consistent zijn met alle replica's. De status manager en de betrouwbare verzamelingen biedt deze consistentie out-of-the-box. De status in het geheugen moet echter worden beheerd door de toepassingscode.
3. Elke replica verandert rollen op een bepaald moment tijdens het uitvoeren van op het cluster. Een secundaire replica als primair, in het geval dat het hosten van het primaire knooppunt niet beschikbaar of overbelast raakt. Dit is natuurlijk gedrag voor Service Fabric daarom services voor het uitvoeren van uiteindelijk onder een andere rol moeten plannen.

In dit artikel wordt ervan uitgegaan dat [Eenheidstesten stateful services in Service Fabric](service-fabric-concepts-unit-testing.md) is gelezen.

## <a name="the-servicefabricmocks-library"></a>De bibliotheek ServiceFabric.Mocks
Vanaf versie 3.3.0, [ServiceFabric.Mocks](https://www.nuget.org/packages/ServiceFabric.Mocks/) biedt een API voor het simuleren van zowel de indeling van de replica's en het beheer staat. Dit wordt gebruikt in de voorbeelden.

[Nuget](https://www.nuget.org/packages/ServiceFabric.Mocks/)
[Github](https://github.com/loekd/ServiceFabric.Mocks)

*ServiceFabric.Mocks is niet het eigendom of beheerd door Microsoft. Dit is echter op dat moment de Microsoft-bibliotheek voor stateful services Eenheidstesten aanbevolen.*

## <a name="set-up-the-mock-orchestration-and-state"></a>Instellen van de mock-indeling en de status
Als onderdeel van het gedeelte rangschikken van een test, een mock replicaset en status manager wordt gemaakt. Het maken van een exemplaar van de geteste service voor elke replica wordt vervolgens eigenaar van de replicaset. Levenscyclusgebeurtenissen voor het uitvoeren wordt tevens zoals eigenaar `OnChangeRole` en `RunAsync`. Het model de status manager zorgt ervoor dat alle bewerkingen die worden uitgevoerd op basis van de status manager worden uitgevoerd en bewaard als de huidige status manager.

1. Maak een service factory gemachtigde die wordt een exemplaar van de service worden getest. Dit moet lijken op of hetzelfde zijn als de service factory callback gewoonlijk in `Program.cs` voor een Service Fabric-service of een actor. Dit moet de volgende handtekening volgen:
```csharp
MyStatefulService CreateMyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica2 stateManager)
```
2. Maak een instantie van `MockReliableStateManager` klasse. Hiermee worden alle interacties met de status manager model.
3. Maak een instantie van `MockStatefulServiceReplicaSet<TStatefulService>` waar `TStatefulService` is het type van de service worden getest. Hiervoor moet de gemachtigde die zijn gemaakt in stap 1 # en de status manager geïnstantieerd in #2
4. Replica's toevoegen aan de replicaset. Geef de rol (zoals primair, ActiveSecondary, IdleSecondary) en de ID van de replica
> Houd de replica-id's! Deze zal waarschijnlijk worden gebruikt tijdens de handeling en delen van een moduletest assert.

```csharp
//service factory to instruct how to create the service instance
var serviceFactory = (StatefulServiceContext context, IReliableStateManagerReplica2 stateManager) => new MyStatefulService(context, stateManager);
//instantiate a new mock state manager
var stateManager = new MockReliableStateManager();
//instantiate a new replica set with the service factory and state manager
var replicaSet = new MockStatefulServiceReplicaSet<MyStatefulService>(CreateStatefulService, stateManager);
//add a new Primary replica with id 1
await replicaSet.AddReplicaAsync(ReplicaRole.Primary, 1);
//add a new ActiveSecondary replica with id 2
await replicaSet.AddReplicaAsync(ReplicaRole.ActiveSecondary, 2);
//add a second ActiveSecondary replica with id 3
await replicaSet.AddReplicaAsync(ReplicaRole.ActiveSecondary, 3);
```

## <a name="execute-service-requests"></a>Uitvoeren van serviceaanvragen
Aanvragen van een service kunnen worden uitgevoerd op een specifieke replica met behulp van de eigenschappen voor uw gemak en zoekacties.
```csharp
const string stateName = "test";
var payload = new Payload(StatePayload);

//execute a request on the primary replica using
await replicaSet.Primary.ServiceInstance.InsertAsync(stateName, payload);

//execute a request against replica with id 2
await replicaSet[2].ServiceInstance.InsertAsync(stateName, payload);

//execute a request against one of the active secondary replicas
await replicaSet.FirstActiveSecondary.InsertAsync(stateName, payload);
```

## <a name="execute-a-service-move"></a>Het verplaatsen van een service uitvoeren
De mock replicaset beschrijft de verschillende methoden voor gebruiksgemak voor het activeren van verschillende typen service verplaatsen.
```csharp
//promote the first active secondary to primary
replicaSet.PromoteNewReplicaToPrimaryAsync();
//promote the secondary with replica id 4 to primary
replicaSet.PromoteNewReplicaToPrimaryAsync(4);

//promote the first idle secondary to an active secondary
PromoteIdleSecondaryToActiveSecondaryAsync();
//promote idle secodary with replica id 4 to active secondary 
PromoteIdleSecondaryToActiveSecondaryAsync(4);

//add a new replica with randomly assigned replica id and promote it to primary
PromoteNewReplicaToPrimaryAsync()
//add a new replica with replica id 4 and promote it to primary
PromoteNewReplicaToPrimaryAsync(4)
```

## <a name="putting-it-all-together"></a>Dit alles
De volgende test ziet u een verzameling met drie knooppunten replica instellen en gecontroleerd of de gegevens van een secundaire beschikbaar na een rolwijziging is. Een typisch probleem dit kan variabel is als de gegevens toegevoegd tijdens de `InsertAsync` is opgeslagen op iets in het geheugen of op een betrouwbare verzameling zonder uitgevoerd `CommitAsync`. In beide gevallen zou de secundaire server worden gesynchroniseerd met de primaire. Dit zou leiden tot inconsistent antwoorden nadat service is verplaatst.

```csharp
[TestMethod]
public async Task TestServiceState_InMemoryState_PromoteActiveSecondary()
{
    var stateManager = new MockReliableStateManager();
    var replicaSet = new MockStatefulServiceReplicaSet<MyStatefulService>(CreateStatefulService, stateManager);
    await replicaSet.AddReplicaAsync(ReplicaRole.Primary, 1);
    await replicaSet.AddReplicaAsync(ReplicaRole.ActiveSecondary, 2);
    await replicaSet.AddReplicaAsync(ReplicaRole.ActiveSecondary, 3);

    const string stateName = "test";
    var payload = new Payload(StatePayload);

    //insert data
    await replicaSet.Primary.ServiceInstance.InsertAsync(stateName, payload);
    //promote one of the secondaries to primary
    await replicaSet.PromoteActiveSecondaryToPrimaryAsync(2);
    //get data
    var payloads = (await replicaSet.Primary.ServiceInstance.GetPayloadsAsync()).ToList();

    //data should match what was inserted against the primary
    Assert.IsTrue(payloads.Count == 1);
    Assert.IsTrue(payloads[0].Content == payload.Content);

    //verify the data was saved against the reliable dictionary
    var dictionary = await StateManager.GetOrAddAsync<IReliableDictionary<string, Payload>>(MyStatefulService.StateManagerDictionaryKey);
    using(var tx = StateManager.CreateTransaction())
    {
        var payload = await dictionary.TryGetValue(stateName);
        Assert.IsTrue(payload.HasValue);
        Assert.IsTrue(payload.Value.Content == payload.Content);
    }
}
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het testen [service-naar-servicecommunicatie](service-fabric-testability-scenarios-service-communication.md) en [storingen simuleren met gecontroleerde chaos](service-fabric-controlled-chaos.md).