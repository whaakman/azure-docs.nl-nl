---
title: Duurzame functies preview-functies - Azure Functions
description: Meer informatie over preview-functies voor duurzame functies.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: article
ms.date: 07/08/2019
ms.author: azfuncdf
ms.openlocfilehash: 7101519aa4a87995dac3a7f11046eed84a2c09b6
ms.sourcegitcommit: af31deded9b5836057e29b688b994b6c2890aa79
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812765"
---
# <a name="durable-functions-20-preview-azure-functions"></a>Duurzame functies 2.0 preview (Azure Functions)

*Duurzame functies* is een uitbreiding van [Azure Functions](../functions-overview.md) en [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md) waarmee u het schrijven van stateful functies in een serverloze omgeving. Met de extensie worden status, controlepunten en het opnieuw opstarten voor u beheerd. Als u niet al bekend met duurzame functies bent, raadpleegt u de [overzicht documentatie](durable-functions-overview.md).

Duurzame functies 1.x is een algemene beschikbaarheid (algemeen beschikbaar)-functie van Azure Functions, maar bevat ook enkele subfuncties die zich momenteel in openbare preview. In dit artikel wordt beschreven nieuw uitgebrachte preview-functies en gaat in op details op hoe ze werken en hoe u met hen kunt gaan.

> [!NOTE]
> Deze preview-functies deel uitmaken van een release duurzame functies 2.0, dat zich momenteel een **preview-versie van de kwaliteit** met verschillende belangrijke wijzigingen. De duurzame Azure-functies is-uitbreidingspakket is gebaseerd kan worden gevonden op nuget.org met versies in de vorm van **2.0.0-betaX**. Deze versies zijn niet bedoeld voor werkbelastingen voor productie en latere versies mag de aanvullende belangrijke wijzigingen.

## <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken

Enkele belangrijke wijzigingen zijn geïntroduceerd in duurzame functies 2.0. Bestaande toepassingen wordt niet verwacht voor compatibiliteit met duurzame functies 2.0 zonder codewijzigingen. Deze sectie vindt u enkele wijzigingen:

### <a name="hostjson-schema"></a>Schema voor host.JSON

Het volgende fragment toont het nieuwe schema voor host.json. De belangrijkste wijzigingen te worden op de hoogte van zijn de nieuwe subsecties:

* `"storageProvider"` (en de `"azureStorage"` subsectie) voor opslag-specifieke configuratie
* `"tracking"` voor het bijhouden en configuratie voor logboekregistratie
* `"notifications"` (en de `"eventGrid"` subsectie) voor de Meldingsconfiguratie voor event grid

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "azureStorage": {
          "connectionStringName": <string>,
          "controlQueueBatchSize": <int?>,
          "partitionCount": <int?>,
          "controlQueueVisibilityTimeout": <hh:mm:ss?>,
          "workItemQueueVisibilityTimeout": <hh:mm:ss?>,
          "trackingStoreConnectionStringName": <string?>,
          "trackingStoreNamePrefix": <string?>,
          "maxQueuePollingInterval": <hh:mm:ss?>
        }
      },
      "tracking": {
        "traceInputsAndOutputs": <bool?>,
        "traceReplayEvents": <bool?>,
      },
      "notifications": {
        "eventGrid": {
          "topicEndpoint": <string?>,
          "keySettingName": <string?>,
          "publishRetryCount": <string?>,
          "publishRetryInterval": <hh:mm:ss?>,
          "publishRetryHttpStatus": <int[]?>,
          "publishEventTypes": <string[]?>,
        }
      },
      "maxConcurrentActivityFunctions": <int?>,
      "maxConcurrentOrchestratorFunctions": <int?>,
      "extendedSessionsEnabled": <bool?>,
      "extendedSessionIdleTimeoutInSeconds": <int?>,
      "customLifeCycleNotificationHelperType": <string?>
  }
}
```

Naarmate duurzame functies 2.0 te laten stabiliseren, meer wijzigingen worden geïntroduceerd aan de `durableTask` host.json sectie. Zie voor meer informatie over deze wijzigingen [deze GitHub-probleem](https://github.com/Azure/azure-functions-durable-extension/issues/641).

### <a name="public-interface-changes"></a>Wijzigingen van de openbare interface

De verschillende 'context'-objecten worden ondersteund door duurzame functies had abstracte basisklassen bedoeld voor gebruik in Eenheidstesten. Als onderdeel van duurzame functies 2.0, zijn deze abstract basisklassen vervangen door interfaces. Functiecode aan te geven die gebruikmaakt van de concrete typen rechtstreeks worden niet beïnvloed.

De volgende tabel geeft de belangrijkste wijzigingen:

| Oude type | Nieuw type |
|----------|----------|
| DurableOrchestrationClientBase | IDurableOrchestrationClient |
| DurableOrchestrationContextBase | IDurableOrchestrationContext |
| DurableActivityContextBase | IDurableActivityContext |

In het geval waarbij een abstracte basisklasse virtuele methoden opgenomen, deze virtuele methoden zijn vervangen door uitbreidingsmethoden die zijn gedefinieerd in `DurableContextExtensions`.

## <a name="entity-functions"></a>Functies van de entiteit

Entiteit functies definiëren bewerkingen voor lezen en bijwerken van kleine stukjes staat bekend als *duurzame entiteiten*. Zoals orchestrator-functies, functies van de entiteit zijn functies met een speciale triggertype *entiteit trigger*. In tegenstelling tot de orchestrator-functies hebt functies van de entiteit geen specifieke beperkingen. Entiteit functies ook beheren staat expliciet in plaats van dat status via Controlestroom impliciet vertegenwoordigt.

### <a name="net-programing-models"></a>.NET programmeren modellen

Er zijn twee optionele programmeermodellen voor het ontwerpen van duurzame entiteiten. De volgende code is een voorbeeld van een eenvoudige *teller* entiteit geïmplementeerd als een standaard-functie. Deze functie definieert drie *operations*, `add`, `reset`, en `get`, elk van die werkzaam zijn in een status integerwaarde `currentValue`.

```csharp
[FunctionName("Counter")]
public static void Counter([EntityTrigger] IDurableEntityContext ctx)
{
    int currentValue = ctx.GetState<int>();

    switch (ctx.OperationName.ToLowerInvariant())
    {
        case "add":
            int amount = ctx.GetInput<int>();
            currentValue += operand;
            break;
        case "reset":
            currentValue = 0;
            break;
        case "get":
            ctx.Return(currentValue);
            break;
    }

    ctx.SetState(currentValue);
}
```

Dit model geschikt is voor implementaties van eenvoudige entiteit of implementaties waarvoor een dynamische set bewerkingen. Er is echter ook een op basis van een klasse programmeermodel dat geschikt is voor entiteiten die statisch zijn, maar bij complexere implementaties hebben. Het volgende voorbeeld is een gelijkwaardige implementatie van de `Counter` entiteit met behulp van .NET-klassen en methoden.

```csharp
public class Counter
{
    [JsonProperty("value")]
    public int CurrentValue { get; set; }

    public void Add(int amount) => this.CurrentValue += amount;
    
    public void Reset() => this.CurrentValue = 0;
    
    public int Get() => this.CurrentValue;

    [FunctionName(nameof(Counter))]
    public static Task Run([EntityTrigger] IDurableEntityContext ctx)
        => ctx.DispatchAsync<Counter>();
}
```

Het model op basis van een klasse is vergelijkbaar met het programmeermodel gepopulariseerd door [Orleans](https://www.microsoft.com/research/project/orleans-virtual-actors/). In dit model, wordt een entiteitstype gedefinieerd als een .NET-klasse. Elke methode van de klasse is een bewerking die kan worden aangeroepen door een externe client. In tegenstelling tot Orleans, echter zijn .NET interfaces optioneel. De vorige *teller* voorbeeld van een interface niet hebt gebruikt, maar kan nog steeds worden aangeroepen via andere functies of via HTTP API-aanroepen.

Entiteit *exemplaren* zijn toegankelijk via een unieke id, de *entiteit-ID*. Een entiteit-ID is gewoon een paar van tekenreeksen die een entiteitexemplaar wordt aangeduid. Deze bestaat uit:

* Een **entiteitnaam**: een naam ter identificatie van het type van de entiteit (bijvoorbeeld 'Item').
* Een **entiteitssleutel**: een tekenreeks is die de entiteit tussen alle andere entiteiten met dezelfde naam (bijvoorbeeld een GUID) wordt aangeduid.

Bijvoorbeeld, een *teller* entiteit functie kan worden gebruikt voor het bewaren van score in een online game. Elk exemplaar van het spel hebben een unieke entiteit-ID, zoals `@Counter@Game1`, `@Counter@Game2`, enzovoort.

### <a name="comparison-with-virtual-actors"></a>Vergelijking met virtuele actoren

Het ontwerp van duurzame entiteiten sterk wordt beïnvloed door de [actormodel](https://en.wikipedia.org/wiki/Actor_model). Als u al bekend met actors bent, moeten de concepten achter duurzame entiteiten bekend aan u zijn. In het bijzonder duurzame entiteiten zijn vergelijkbaar met [virtuele actoren](https://research.microsoft.com/projects/orleans/) op tal van manieren:

* Duurzame entiteiten kunnen worden opgevraagd via een *entiteit-ID*.
* Duurzame entiteit bewerkingen uitvoeren opeenvolgend, één voor één, om te voorkomen dat racevoorwaarden.
* Duurzame entiteiten worden automatisch gemaakt wanneer ze worden aangeroepen of een signaal ontvangen.
* Wanneer bewerkingen niet wordt uitgevoerd, zijn duurzame entiteiten op de achtergrond uit het geheugen verwijderd.

Er zijn enkele belangrijke verschillen, die zijn echter vermelden waard:

* Duurzame entiteiten prioriteren *duurzaamheid* via *latentie*, en is dus mogelijk niet geschikt voor toepassingen met strikte latentievereisten.
* Berichten die worden verzonden tussen entiteiten worden geleverd, betrouwbaar en in de volgorde.
* Duurzame entiteiten kunnen worden gebruikt in combinatie met duurzame indelingen en kunnen fungeren als gedistribueerde wordt vergrendeld, die verderop in dit artikel worden beschreven.
* Aanvraag/antwoord-patronen in entiteiten zijn beperkt tot indelingen. Voor communicatie met de entiteit naar entiteit zijn slechts één richting berichten (ook wel bekend als '-signalering') toegestaan, zoals in het oorspronkelijke actormodel. Dit gedrag wordt voorkomen dat gedistribueerde impassen.

### <a name="durable-entity-net-apis"></a>Duurzame entiteit .NET-API 's

Entiteit-ondersteuning omvat verschillende API's. Voor een is er een nieuwe API voor het definiëren van de entiteit-functies, zoals hierboven, die opgeven wat er moet gebeuren wanneer een bewerking wordt aangeroepen voor een entiteit. Bestaande API's voor clients en indelingen zijn bovendien bijgewerkt met nieuwe functionaliteit voor interactie met entiteiten.

#### <a name="implementing-entity-operations"></a>Uitvoering van bewerkingen van de entiteit

De uitvoering van een bewerking op een entiteit deze leden van het contextobject kan aanroepen (`IDurableEntityContext` in .NET):

* **OperationName**: haalt u de naam van de bewerking.
* **GetInput\<tUITGANGSMATERIALEN >** : Hiermee haalt u de invoer voor de bewerking.
* **GetState\<TState >** : Hiermee wordt de huidige status van de entiteit.
* **SetState**: updates van de status van de entiteit.
* **SignalEntity**: verzendt een eenzijdige bericht naar een entiteit.
* **Selfservice**: de ID van de entiteit opgehaald.
* **Retourneren**: een waarde retourneert met de client of de indeling die de bewerking genoemd.
* **IsNewlyConstructed**: retourneert `true` als de entiteit niet vóór de bewerking bestaat.
* **DestructOnExit**: Hiermee verwijdert u de entiteit nadat de bewerking is voltooid.

Bewerkingen zijn minder dan indelingen met beperkte toegang:

* Bewerkingen kunnen externe i/o, met behulp van synchrone of asynchrone API's (wordt u aangeraden asynchrone die alleen) aanroepen.
* Bewerkingen kunnen worden niet-deterministisch. Bijvoorbeeld, het is veilig om aan te roepen `DateTime.UtcNow`, `Guid.NewGuid()` of `new Random()`.

#### <a name="accessing-entities-from-clients"></a>Toegang tot entiteiten van clients

Duurzame entiteiten kunnen worden aangeroepen vanuit de gewone functies via het `orchestrationClient` binding (`IDurableOrchestrationClient` in .NET). De volgende methoden worden ondersteund:

* **ReadEntityStateAsync\<T >** : de status van een entiteit leest.
* **SignalEntityAsync**: verzendt een eenzijdige bericht naar een entiteit en wacht deze naar de wachtrij worden gezet.
* **SignalEntityAsync\<T >** : hetzelfde als `SignalEntityAsync` maar maakt gebruik van een gegenereerde proxy-object van het type `T`.

De vorige `SignalEntityAsync` aanroep is vereist op te geven de naam van de entiteit-bewerking als een `string` en de nettolading van de bewerking als een `object`. De volgende voorbeeldcode wordt een voorbeeld van dit patroon:

```csharp
EntityId id = // ...
object amount = 5;
context.SignalEntityAsync(id, "Add", amount);
```

Het is ook mogelijk voor het genereren van een proxyobject voor type veilige toegang. Voor het genereren van een proxy type kluis, moet het entiteitstype een interface implementeren. Stel bijvoorbeeld dat de `Counter` entiteit eerder hebt geïmplementeerd een `ICounter` -interface, als volgt gedefinieerd:

```csharp
public interface ICounter
{
    void Add(int amount);
    void Reset();
    int Get();
}

public class Counter : ICounter
{
    // ...
}
```

Clientcode kan vervolgens gebruiken `SignalEntityAsync<T>` en geef de `ICounter` interface als de typeparameter voor het genereren van een proxy-type-safe. Dit gebruik van het type kluis proxy's wordt geïllustreerd in het volgende codevoorbeeld:

```csharp
[FunctionName("UserDeleteAvailable")]
public static async Task AddValueClient(
    [QueueTrigger("my-queue")] string message,
    [OrchestrationClient] IDurableOrchestrationClient client)
{
    int amount = int.Parse(message);
    var target = new EntityId(nameof(Counter), "MyCounter");
    await client.SignalEntityAsync<ICounter>(target, proxy => proxy.Add(amount));
}
```

In het vorige voorbeeld de `proxy` parameter is een dynamisch gegenereerd exemplaar van `ICounter`, die de aanroep van intern vertaalt `Add` aanroepen naar de overeenkomstige (getypeerde) `SignalEntityAsync`.

> [!NOTE]
> Het is belangrijk te weten dat de `ReadEntityStateAsync` en `SignalEntityAsync` methoden van `IDurableOrchestrationClient` prestaties via consistentiecontrole prioriteren. `ReadEntityStateAsync` kan een verouderde waarde retourneren en `SignalEntityAsync` kunt terugkeren voordat de bewerking is voltooid.

#### <a name="accessing-entities-from-orchestrations"></a>Toegang tot entiteiten van indelingen

Indelingen hebben toegang tot entiteiten met behulp van de `IDurableOrchestrationContext` object. Ze kunnen kiezen tussen communicatie in één richting (worden geactiveerd en vergeten) en communicatie in twee richtingen (aanvraag en antwoord). De bijbehorende methoden zijn:

* **SignalEntity**: verzendt een eenzijdige bericht naar een entiteit.
* **CallEntityAsync**: verzendt een bericht naar een entiteit en wacht op een reactie waarmee wordt aangegeven dat de bewerking is voltooid.
* **CallEntityAsync\<T >** : verzendt een bericht naar een entiteit en wachten op antwoord met een resultaat van het type T.

Wanneer u communicatie in twee richtingen, worden eventuele uitzonderingen tijdens het uitvoeren van de bewerking ook verzonden naar de aanroepende orchestration en rethrown. Daarentegen, wanneer u fire-and-forgetstromen, worden uitzonderingen niet waargenomen.

Voor toegang tot het type kluis, kunnen de orchestration-functies proxy's op basis van een interface genereren. De `CreateEntityProxy` uitbreidingsmethode kan worden gebruikt voor dit doel:

```csharp
public interface IAsyncCounter
{
    Task AddAsync(int amount);
    Task ResetAsync();
    Task<int> GetAsync();
}

[FunctionName("CounterOrchestration)]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    // ...
    IAsyncCounter proxy = context.CreateEntityProxy<IAsyncCounter>("MyCounter");
    await proxy.AddAsync(5);
    int newValue = await proxy.GetAsync();
    // ...
}
```

In het vorige voorbeeld wordt een 'item' entiteit is wordt ervan uitgegaan dat die implementeert de `IAsyncCounter` interface. De indeling is vervolgens kunnen gebruiken de `IAsyncCounter` definitie voor het genereren van een proxytype voor synchroon interactie met de entiteit van het type.

### <a name="locking-entities-from-orchestrations"></a>Vergrendeling entiteiten van indelingen

Indelingen kunnen vergrendelen entiteiten. Deze mogelijkheid biedt een eenvoudige manier om te voorkomen dat ongewenste stappen met behulp van *kritieke secties*.

Het context-object bevat de volgende methoden:

* **LockAsync**: verkrijgt vergrendelingen op een of meer entiteiten.
* **IsLocked**: retourneert ' True ' als u momenteel in een kritieke sectie false anders.

De essentiële sectie wordt beëindigd en alle vergrendelingen zijn vrijgegeven, wanneer de indeling is beëindigd. In .NET, `LockAsync` retourneert een `IDisposable` die eindigt op de kritieke gedeelte als u buiten gebruik gesteld, die kan worden gebruikt samen met een `using` component naar een syntactische weergave van de kritieke sectie.

Bijvoorbeeld, kunt u een indeling die nodig heeft om te testen of twee spelers beschikbaar zijn en deze vervolgens toewijzen aan een spel. Deze taak kan worden geïmplementeerd met behulp van een kritieke sectie als volgt:

```csharp
[FunctionName("Orchestrator")]
public static async Task RunOrchestrator(
    [OrchestrationTrigger] IDurableOrchestrationContext ctx)
{
    EntityId player1 = /* ... */;
    EntityId player2 = /* ... */;

    using (await ctx.LockAsync(player1, player2))
    {
        bool available1 = await ctx.CallEntityAsync<bool>(player1, "is-available");
        bool available2 = await ctx.CallEntityAsync<bool>(player2, "is-available");

        if (available1 && available2)
        {
            Guid gameId = ctx.NewGuid();

            await ctx.CallEntityAsync(player1, "assign-game", gameId);
            await ctx.CallEntityAsync(player2, "assign-game", gameId);
        }
    }
}
```

In de sectie kritieke zijn beide player-entiteiten vergrendeld, wat betekent dat ze worden niet alle bewerkingen dan degene die worden aangeroepen vanuit de kritieke sectie uitgevoerd). Dit gedrag wordt voorkomen dat de stappen met conflicterende bewerkingen worden uitgevoerd, zoals die wordt toegewezen aan een andere spelers spel of ondertekenen uitgeschakeld.

We enkele beperkingen opleggen aan hoe essentieel onderdelen kunnen worden gebruikt. Deze beperkingen fungeren impassen en herbetreedbaarheid te voorkomen.

* Kritieke secties kunnen niet worden genest.
* Kritieke secties kunnen suborchestrations niet maken.
* Kritieke secties kunnen alleen entiteiten die ze hebben een vergrendeld aanroepen.
* Kritieke secties kunnen dezelfde entiteit met behulp van meerdere parallelle aanroepen niet aanroepen.
* Kritieke secties kunnen alleen entiteiten die ze hebben niet vergrendeld een signaal verzenden.

## <a name="alternate-storage-providers"></a>Alternatieve opslagproviders

Het duurzame taak Framework ondersteunt meerdere opslagproviders vandaag, met inbegrip van [Azure Storage](https://github.com/Azure/durabletask/tree/master/src/DurableTask.AzureStorage), [Azure Service Bus](https://github.com/Azure/durabletask/tree/master/src/DurableTask.ServiceBus), een [in-memory-emulator](https://github.com/Azure/durabletask/tree/master/src/DurableTask.Emulator), en een experimentele [Redis](https://github.com/Azure/durabletask/tree/redis/src/DurableTask.Redis) provider. Echter tot op heden ondersteund de extensie duurzame taak voor Azure Functions alleen de Azure Storage-provider. Beginnen met duurzame functies 2.0, ondersteuning voor alternatieve opslagproviders wordt toegevoegd, beginnend met de Redis-provider.

> [!NOTE]
> Duurzame functies 2.0 biedt alleen ondersteuning voor .NET Standard 2.0-compatibele providers. Op het moment van schrijven, de Azure Service Bus-provider biedt geen ondersteuning voor .NET Standard 2.0 en kan daarom niet beschikbaar als een alternatieve opslagprovider.

### <a name="emulator"></a>Emulator

De [DurableTask.Emulator](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Emulator/) -provider is een lokaal geheugen, een niet-duurzame opslagprovider geschikt is voor lokale Testscenario's. Deze kan worden geconfigureerd met behulp van de volgende minimale **host.json** schema:

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "emulator": { }
      }
    }
  }
}
```

### <a name="redis-experimental"></a>Redis (experimenteel)

De [DurableTask.Redis](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Redis/) provider zich blijft voordoen alle orchestration staat een geconfigureerde Redis-cluster.

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "redis": {
          "connectionStringName": <string>,
        }
      }
    }
  }
}
```

De `connectionStringName` moet verwijzen naar de naam van een app-instelling of de omgeving variabele. Die app-instelling of de omgeving variabele mag een Redis connection string-waarde in de vorm van *server: poort*. Bijvoorbeeld, `localhost:6379` voor het verbinden met een lokale Redis-cluster.

> [!NOTE]
> De Redis-provider is op dit moment experimenteel en biedt alleen ondersteuning voor functie-apps die worden uitgevoerd op een enkel knooppunt. Is er geen garantie dat de Redis-provider wordt ooit algemeen beschikbaar gesteld, en deze kan worden verwijderd in een toekomstige release.
