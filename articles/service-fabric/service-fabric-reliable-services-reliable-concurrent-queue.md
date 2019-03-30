---
title: ReliableConcurrentQueue in Azure Service Fabric
description: ReliableConcurrentQueue is een hoge doorvoer wachtrij zodat parallelle enqueues en dequeues.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: raja,tyadam,masnider,vturecek
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: aljo
ms.openlocfilehash: dbdfa4686c047fa7cf5d74cd9aca768447f9db93
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663639"
---
# <a name="introduction-to-reliableconcurrentqueue-in-azure-service-fabric"></a>Inleiding tot ReliableConcurrentQueue in Azure Service Fabric
Betrouwbare gelijktijdige wachtrij is een asynchrone, transactionele en gerepliceerde wachtrij welke functies hoog gelijktijdigheid voor in de wachtrij plaatsen en uit de wachtrij verwijderen bewerkingen. Het is ontworpen voor het leveren van hoge doorvoer en lage latentie door versoepeling van de strikte FIFO volgorde geleverd door [betrouwbare wachtrij](https://msdn.microsoft.com/library/azure/dn971527.aspx) en in plaats daarvan biedt een best-effort bestellen.

## <a name="apis"></a>API's

|Gelijktijdige wachtrij                |Betrouwbare gelijktijdige wachtrij                                         |
|--------------------------------|------------------------------------------------------------------|
| void Enqueue(T item)           | Task EnqueueAsync(ITransaction tx, T item)                       |
| BOOL TryDequeue (uit T resultaat)  | Taak < ConditionalValue < T >> TryDequeueAsync (tx ITransaction)  |
| int Count()                    | long Count()                                                     |

## <a name="comparison-with-reliable-queuehttpsmsdnmicrosoftcomlibraryazuredn971527aspx"></a>Vergelijking met [betrouwbare wachtrij](https://msdn.microsoft.com/library/azure/dn971527.aspx)

Betrouwbare gelijktijdige wachtrij wordt aangeboden als een alternatief voor [betrouwbare wachtrij](https://msdn.microsoft.com/library/azure/dn971527.aspx). Het moet worden gebruikt in gevallen waar strikte FIFO-volgorde niet vereist is, als FIFO vereist een negatieve gevolgen voor de met gelijktijdigheid te garanderen.  [Betrouwbare wachtrij](https://msdn.microsoft.com/library/azure/dn971527.aspx) vergrendelingen gebruikt om af te dwingen FIFO-volgorde, met maximaal één transactie toegestaan in de wachtrij plaatsen en maximaal één transactie uit de wachtrij verwijderen tegelijk toegestaan. Ter vergelijking: betrouwbare gelijktijdige wachtrij worden de bestellen beperking en kunt een aantal gelijktijdige transacties interleave hun in de wachtrij plaatsen en uit de wachtrij verwijderen bewerkingen. Best-effort bestellen is opgegeven, wordt echter nooit de relatieve positie van twee waarden in een betrouwbare gelijktijdige wachtrij kan worden gegarandeerd.

Betrouwbare gelijktijdige wachtrij biedt een hogere doorvoer en lagere latentie dan [betrouwbare wachtrij](https://msdn.microsoft.com/library/azure/dn971527.aspx) wanneer er zijn meerdere gelijktijdige transacties enqueues uitvoeren en/of dequeues.

Een voorbeeld van een use-case voor de ReliableConcurrentQueue is de [berichtenwachtrij](https://en.wikipedia.org/wiki/Message_queue) scenario. In dit scenario, een of meer producenten maken en voeg items toe aan de wachtrij, en gebruikers van een of meer berichten ophalen van berichten uit de wachtrij en ze verwerken. Meerdere producenten en consumenten kunnen werken onafhankelijk van elkaar, met behulp van gelijktijdige transacties om te kunnen verwerken van de wachtrij.

## <a name="usage-guidelines"></a>Richtlijnen voor het gebruik
* De wachtrij wordt verwacht dat de items in de wachtrij voor een lage bewaarperiode hebben. Dat wil zeggen, wilt de items blijven niet in de wachtrij gedurende een lange periode.
* De wachtrij is geen garantie strikte FIFO bestellen.
* De wachtrij heeft een eigen schrijfbewerkingen niet lezen. Als een item in de wachtrij geplaatste binnen een transactie is zijn deze niet zichtbaar voor een dequeuer binnen dezelfde transactie.
* Dequeues zijn niet van elkaar geïsoleerd. Als item *A* uit wachtrij wordt geplaatst in transactie *txnA*, ook al *txnA* is item niet doorgevoerd, *A* zou niet zichtbaar zijn voor een gelijktijdige transactie *txnB*.  Als *txnA* wordt afgebroken, *A* wordt zichtbaar voor *txnB* onmiddellijk.
* *TryPeekAsync* gedrag kan worden geïmplementeerd met behulp van een *TryDequeueAsync* en vervolgens de transactie afgebroken. Een voorbeeld hiervan vindt u in de sectie patronen programmeren.
* Aantal is niet-transactionele. Het kan worden gebruikt voor een beter beeld van het aantal elementen in de wachtrij, maar vertegenwoordigt een point-in-time en kan niet worden gebruikt.
* Duur verwerking op de dequeued items moet niet worden uitgevoerd tijdens de transactie actief is, om te voorkomen dat langlopende transacties waarvoor een prestatie-impact op het systeem.

## <a name="code-snippets"></a>Codefragmenten
We bekijken enkele codefragmenten en de verwachte uitvoer. Afhandeling van uitzonderingen wordt in deze sectie genegeerd.

### <a name="enqueueasync"></a>EnqueueAsync
Hier volgen enkele codefragmenten voor het gebruik van EnqueueAsync gevolgd door de verwachte uitvoer.

- *Geval 1: Taak in de wachtrij plaatsen*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}
```

Wordt ervan uitgegaan dat de taak is voltooid en dat er geen gelijktijdige transacties wijzigen van de wachtrij is. De gebruiker kan de wachtrij bevat de items in een van de volgende orders verwachten:

> 10, 20
> 
> 20, 10


- *Geval 2: Parallelle taak in de wachtrij plaatsen*

```
// Parallel Task 1
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}

// Parallel Task 2
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 30, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 40, cancellationToken);

    await txn.CommitAsync();
}
```

Wordt ervan uitgegaan dat de taken is voltooid, of de taken parallel wordt uitgevoerd en dat er geen andere gelijktijdige transacties wijzigen van de wachtrij. Er is geen Deductie kan worden gemaakt over de volgorde van items in de wachtrij. Voor dit codefragment kunnen de items worden weergegeven in een van de 4. mogelijke volgorden.  De wachtrij probeert te houden van de items in de volgorde van de oorspronkelijke (in de wachtrij), maar kan worden gedwongen om deze te rangschikken vanwege gelijktijdige bewerkingen of fouten.


### <a name="dequeueasync"></a>DequeueAsync
Hier volgen enkele codefragmenten voor het gebruik van TryDequeueAsync gevolgd door de verwachte uitvoer. Wordt ervan uitgegaan dat de wachtrij al is gevuld met de volgende items in de wachtrij:
> 10, 20, 30, 40, 50, 60

- *Geval 1: Één taak uit de wachtrij verwijderen*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    await txn.CommitAsync();
}
```

Wordt ervan uitgegaan dat de taak is voltooid en dat er geen gelijktijdige transacties wijzigen van de wachtrij is. Omdat er geen Deductie kan worden gemaakt over de volgorde van de items in de wachtrij, alle drie van de items kunnen worden uit de wachtrij genomen, in een willekeurige volgorde. De wachtrij probeert te houden van de items in de volgorde van de oorspronkelijke (in de wachtrij), maar kan worden gedwongen om deze te rangschikken vanwege gelijktijdige bewerkingen of fouten.  

- *Geval 2: Taken parallel uit de wachtrij verwijderen*

```
// Parallel Task 1
List<int> dequeue1;
using (var txn = this.StateManager.CreateTransaction())
{
    dequeue1.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;
    dequeue1.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;

    await txn.CommitAsync();
}

// Parallel Task 2
List<int> dequeue2;
using (var txn = this.StateManager.CreateTransaction())
{
    dequeue2.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;
    dequeue2.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;

    await txn.CommitAsync();
}
```

Wordt ervan uitgegaan dat de taken is voltooid, of de taken parallel wordt uitgevoerd en dat er geen andere gelijktijdige transacties wijzigen van de wachtrij. Omdat er geen Deductie kan worden gemaakt over de volgorde van de items in de wachtrij en de lijsten *dequeue1* en *dequeue2* wordt elke twee items, in een willekeurige volgorde bevat.

Hetzelfde item wordt *niet* in beide lijsten worden weergegeven. Dus als dequeue1 *10*, *30*, en vervolgens dequeue2 zou hebben *20*, *40*.

- *Voorbeeld 3: Uit de wachtrij verwijderen bestellen met transactie is afgebroken*

Een transactie met die onderweg zijn afgebroken dequeues plaatst die de items back-ups op de kop van de wachtrij maken. De volgorde waarin de items worden teruggeplaatst op de kop van de wachtrij kan niet worden gegarandeerd. We bekijken de volgende code:

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    // Abort the transaction
    await txn.AbortAsync();
}
```
Wordt ervan uitgegaan dat de items uit wachtrij zijn geplaatst in de volgende volgorde:
> 10, 20

Wanneer we de transactie afgebroken, zou de items terug naar de kop van de wachtrij in een van de volgende orders worden toegevoegd:
> 10, 20
> 
> 20, 10

Hetzelfde geldt voor alle gevallen waar de transactie niet met succes is *vastgelegd*.

## <a name="programming-patterns"></a>Patronen voor programmeren
In deze sectie we bekijken enkele programmeertalen patronen die mogelijk nuttig zijn bij het gebruik van ReliableConcurrentQueue.

### <a name="batch-dequeues"></a>Batch Dequeues
Een aanbevolen programming patroon is voor de taak consument batch de dequeues in plaats van een wachtrij op een tijdstip. De gebruiker kan kiezen om te beperken van vertragingen tussen elke batch of de batchgrootte. Het volgende codefragment toont deze programmeermodel.  Houd er rekening mee dat in dit voorbeeld wordt het verwerken is voltooid nadat de transactie doorgevoerd wordt, dus als een storing optreden tijdens het verwerken, zonder dat is verwerkt de niet-verwerkte items gaan verloren.  De verwerking kan ook worden uitgevoerd binnen het bereik van de transactie, maar dit een negatieve invloed op prestaties hebben kan en vereist dat de verwerking van de items die al verwerkt.

```
int batchSize = 5;
long delayMs = 100;

while(!cancellationToken.IsCancellationRequested)
{
    // Buffer for dequeued items
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        ConditionalValue<int> ret;

        for(int i = 0; i < batchSize; ++i)
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if (ret.HasValue)
            {
                // If an item was dequeued, add to the buffer for processing
                processItems.Add(ret.Value);
            }
            else
            {
                // else break the for loop
                break;
            }
        }

        await txn.CommitAsync();
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }

    int delayFactor = batchSize - processItems.Count;
    await Task.Delay(TimeSpan.FromMilliseconds(delayMs * delayFactor), cancellationToken);
}
```

### <a name="best-effort-notification-based-processing"></a>Best-Effort melding gebaseerde verwerking
Een andere interessante programming patroon maakt gebruik van de Count-API. Hier kunnen we best-effort verwerking voor de wachtrij op basis van meldingen implementeren. De wachtrij aantal kan worden gebruikt om te beperken van een in de wachtrij plaatsen of een taak uit de wachtrij verwijderen.  Houd er rekening mee dat het vorige voorbeeld, omdat de verwerking vindt plaats buiten de transactie niet-verwerkte items mogelijk verloren als er een fout optreedt tijdens de verwerking.

```
int threshold = 5;
long delayMs = 1000;

while(!cancellationToken.IsCancellationRequested)
{
    while (this.Queue.Count < threshold)
    {
        cancellationToken.ThrowIfCancellationRequested();

        // If the queue does not have the threshold number of items, delay the task and check again
        await Task.Delay(TimeSpan.FromMilliseconds(delayMs), cancellationToken);
    }

    // If there are approximately threshold number of items, try and process the queue

    // Buffer for dequeued items
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        ConditionalValue<int> ret;

        do
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if (ret.HasValue)
            {
                // If an item was dequeued, add to the buffer for processing
                processItems.Add(ret.Value);
            }
        } while (processItems.Count < threshold && ret.HasValue);

        await txn.CommitAsync();
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
}
```

### <a name="best-effort-drain"></a>Best-Effort Clusterbesturingssysteem
Veel van de wachtrij kan niet worden gegarandeerd door de gelijktijdige aard van de gegevensstructuur.  Het is mogelijk dat, zelfs als er geen gebruikersbewerkingen voor de in de wachtrij die onderweg zijn zijn, een aanroep van bepaalde TryDequeueAsync mogelijk niet retourneren een item was voorheen opgenomen in de wachtrij en doorgevoerd.  Het item in de wachtrij kan worden gegarandeerd *uiteindelijk* zichtbaar te maken uit de wachtrij verwijderen, maar zonder een mechanisme voor out-of-band-communicatie, een onafhankelijke consumer niet weten dat de wachtrij heeft bereikt een stabiele status, zelfs als alle producenten zijn gestopt en nieuwe in de wachtrij plaatsen bewerkingen zijn toegestaan. De bewerking van het clusterbesturingssysteem is dus best-effort zoals hieronder wordt geïmplementeerd.

De gebruiker moet alle verdere producent en consument taken stoppen en wachten op actieve transacties doorvoeren of afbreken, voordat u probeert te corrigeren van de wachtrij.  Als de gebruiker het verwachte aantal items in de wachtrij kent, kunnen ze een melding dat bepaalt dat alle items hebt zijn uit wachtrij verwijderd welke instellen.

```
int numItemsDequeued;
int batchSize = 5;

ConditionalValue ret;

do
{
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        do
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if(ret.HasValue)
            {
                // Buffer the dequeues
                processItems.Add(ret.Value);
            }
        } while (ret.HasValue && processItems.Count < batchSize);

        await txn.CommitAsync();
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
} while (ret.HasValue);
```

### <a name="peek"></a>Piek
ReliableConcurrentQueue beschikt niet over de *TryPeekAsync* api. Gebruikers kunnen krijgen de peek semantische met behulp van een *TryDequeueAsync* en vervolgens de transactie afgebroken. In dit voorbeeld dequeues worden verwerkt alleen als de waarde van het item groter dan is *10*.

```
using (var txn = this.StateManager.CreateTransaction())
{
    ConditionalValue ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);
    bool valueProcessed = false;

    if (ret.HasValue)
    {
        if (ret.Value > 10)
        {
            // Process the item
            Console.WriteLine("Value : " + ret.Value);
            valueProcessed = true;
        }
    }

    if (valueProcessed)
    {
        await txn.CommitAsync();    
    }
    else
    {
        await txn.AbortAsync();
    }
}
```

## <a name="must-read"></a>Moet lezen
* [Snel starten van Reliable Services](service-fabric-reliable-services-quick-start.md)
* [Werken met betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
* [Meldingen van betrouwbare Services](service-fabric-reliable-services-notifications.md)
* [Reliable Services back-up en herstellen (herstel na noodgevallen)](service-fabric-reliable-services-backup-restore.md)
* [Configuratie van betrouwbare status Manager](service-fabric-reliable-services-configuration.md)
* [Aan de slag met Service Fabric-webservices-API](service-fabric-reliable-services-communication-webapi.md)
* [Geavanceerd gebruik van het programmeermodel voor Reliable Services](service-fabric-reliable-services-advanced-usage.md)
* [Referentie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
