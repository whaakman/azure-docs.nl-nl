---
title: ReliableConcurrentQueue in Azure Service Fabric
description: ReliableConcurrentQueue is een hoge gegevensdoorvoer wachtrij die u kunt parallelle enqueues en dequeues.
services: service-fabric
documentationcenter: .net
author: sangarg
manager: timlt
editor: raja,tyadam,masnider,vturecek
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: sangarg
ms.openlocfilehash: 122cb48149477f295a65b8ee623c647b6db10a86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-reliableconcurrentqueue-in-azure-service-fabric"></a>Inleiding tot ReliableConcurrentQueue in Azure Service Fabric
Betrouwbare gelijktijdige wachtrij is een asynchrone transactionele en gerepliceerde wachtrij welke functies hoge gelijktijdigheid van taken voor de wachtrij plaatsen en bewerkingen in wachtrij. Het is ontworpen voor het leveren van hoge doorvoer en lage latentie door doet de strikte FIFO-principe ordening geleverd door [betrouwbare wachtrij](https://msdn.microsoft.com/library/azure/dn971527.aspx) en stelt u in plaats daarvan een rangschikking van best-effort.

## <a name="apis"></a>API's

|Gelijktijdige wachtrij                |Betrouwbare gelijktijdige wachtrij                                         |
|--------------------------------|------------------------------------------------------------------|
| VOID Enqueue(T item)           | Taak EnqueueAsync (tx ITransaction, T-item)                       |
| BOOL TryDequeue (uit resultaat T)  | Taak < ConditionalValue < T >> TryDequeueAsync (tx ITransaction)  |
| int Count()                    | lange Count()                                                     |

## <a name="comparison-with-reliable-queuehttpsmsdnmicrosoftcomlibraryazuredn971527aspx"></a>Vergelijking met [betrouwbare wachtrij](https://msdn.microsoft.com/library/azure/dn971527.aspx)

Betrouwbare gelijktijdige wachtrij wordt aangeboden als een alternatief voor [betrouwbare wachtrij](https://msdn.microsoft.com/library/azure/dn971527.aspx). Het moet worden gebruikt in gevallen waarbij strikte FIFO-principe ordening niet vereist is als het FIFO-principe vereist een afweging met gelijktijdigheid van taken te garanderen.  [Betrouwbare wachtrij](https://msdn.microsoft.com/library/azure/dn971527.aspx) vergrendelingen af te dwingen FIFO-principe ordening met maximaal één transactie toegestaan in de wachtrij plaatsen en maximaal één transactie tegelijk in wachtrij mag gebruikt. Ter vergelijking, betrouwbare gelijktijdige wachtrij met deze sjabloon worden de beperking ordenen en aantal gelijktijdige transacties interleave hun in de wachtrij plaatsen en uit de wachtrij halen bewerkingen kunt. Best-effort ordening is opgegeven, wordt echter nooit de relatieve volgorde van de twee waarden in een betrouwbare gelijktijdige wachtrij kan worden gegarandeerd.

Betrouwbare gelijktijdige wachtrij biedt hogere doorvoer en lagere latentie mogelijk dan [betrouwbare wachtrij](https://msdn.microsoft.com/library/azure/dn971527.aspx) wanneer er meerdere gelijktijdige transacties enqueues uitvoeren en/of dequeues.

Een voorbeeld van een gebruiksvoorbeeld voor de ReliableConcurrentQueue is de [Message Queue](https://en.wikipedia.org/wiki/Message_queue) scenario. In dit scenario kan een of meer bericht producenten maken en items toevoegen aan de wachtrij en een of meer bericht consumenten berichten uit de wachtrij ophalen en deze te verwerken. Meerdere producenten en consumenten kunnen onafhankelijk werken, gelijktijdige transacties met de verwerking van de wachtrij.

## <a name="usage-guidelines"></a>Richtlijnen voor het gebruik
* De wachtrij wordt verwacht dat de artikelen in de wachtrij een lage bewaarperiode moeten. Dat wil zeggen, zou de items blijven niet in de wachtrij gedurende een lange periode.
* Strikte FIFO-volgorde wordt niet gegarandeerd dat de wachtrij.
* De wachtrij heeft een eigen schrijfbewerkingen niet lezen. Als een item in de wachtrij binnen een transactie is, wordt deze niet zichtbaar is voor een dequeuer binnen dezelfde transactie worden.
* Dequeues zijn niet van elkaar geïsoleerd. Als het item *A* uit wachtrij is geplaatst in transactie *txnA*, ook al *txnA* is geen item worden toegewezen, *A* niet zichtbaar is voor een gelijktijdige zou worden transactie *txnB*.  Als *txnA* wordt afgebroken, *A* wordt zichtbaar voor *txnB* onmiddellijk.
* *TryPeekAsync* gedrag kan worden geïmplementeerd met behulp van een *TryDequeueAsync* en vervolgens de transactie afgebroken. Een voorbeeld hiervan vindt in de sectie Programming patronen.
* Aantal is niet-transactionele. Het kan worden gebruikt om een idee krijgen van het aantal elementen in de wachtrij, maar vertegenwoordigt een punt in tijd en kan niet worden aangehaald.
* Dure verwerking op de dequeued items moet niet worden uitgevoerd tijdens de transactie actief en is om te voorkomen dat langlopende transacties die mogelijk invloed op de prestaties op het systeem.

## <a name="code-snippets"></a>Codefragmenten
Laat het ons Bekijk enkele codefragmenten en de verwachte uitvoer. Afhandeling van uitzonderingen wordt in deze sectie genegeerd.

### <a name="enqueueasync"></a>EnqueueAsync
Hier volgen enkele codefragmenten voor het gebruik van EnqueueAsync gevolgd door de verwachte uitvoer.

- *Voorbeeld 1: Één taak in de wachtrij plaatsen*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}
```

Wordt ervan uitgegaan dat de taak is voltooid en dat er geen gelijktijdige transacties wijzigen van de wachtrij is. De gebruiker kan de wachtrij bevat de items in een van de volgende bestellingen verwachten:

> 10, 20

> 20, 10


- *Voorbeeld 2: Parallelle taak in de wachtrij plaatsen*

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

Wordt ervan uitgegaan dat de taken is voltooid, of de taken parallel uitgevoerd en er zijn geen gelijktijdige transacties van dat de wachtrij wijzigen. Er is geen Deductie kan worden gemaakt over de volgorde van items in de wachtrij. Voor dit codefragment kunnen de items in een van de 4 voorkomen! mogelijke volgorden.  De wachtrij probeert te houden van de items in de volgorde van de oorspronkelijke (in wachtrij gezet), maar kan geforceerd worden gedragsitems vanwege gelijktijdige bewerkingen of fouten.


### <a name="dequeueasync"></a>DequeueAsync
Hier volgen enkele codefragmenten voor het gebruik van TryDequeueAsync gevolgd door de verwachte uitvoer. Wordt ervan uitgegaan dat de wachtrij al is gevuld met de volgende items in de wachtrij:
> 10, 20, 30, 40, 50, 60

- *Voorbeeld 1: Één taak in wachtrij*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    await txn.CommitAsync();
}
```

Wordt ervan uitgegaan dat de taak is voltooid en dat er geen gelijktijdige transacties wijzigen van de wachtrij is. Omdat er geen Deductie kan worden gemaakt over de volgorde van de items in de wachtrij, alle drie van de items kunnen worden uit wachtrij geplaatst, in willekeurige volgorde. De wachtrij probeert te houden van de items in de volgorde van de oorspronkelijke (in wachtrij gezet), maar kan geforceerd worden gedragsitems vanwege gelijktijdige bewerkingen of fouten.  

- *Voorbeeld 2: Taak Parallel uit de wachtrij halen*

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

Wordt ervan uitgegaan dat de taken is voltooid, of de taken parallel uitgevoerd en er zijn geen gelijktijdige transacties van dat de wachtrij wijzigen. Omdat er geen Deductie kan worden gemaakt over de volgorde van de items in de wachtrij, de lijsten *dequeue1* en *dequeue2* wordt elke twee items, in willekeurige volgorde bevatten.

Hetzelfde item wordt *niet* in beide lijsten worden weergegeven. Daarom als dequeue1 *10*, *30*, en vervolgens dequeue2 zou hebben *20*, *40*.

- *Voorbeeld 3: Wachtrij ordening met transactie afgebroken*

Een transactie met onderweg wordt afgebroken dequeues puts die de items terug op de kop van de wachtrij. De volgorde waarin de items worden aangeboden terug op de kop van de wachtrij kan niet worden gegarandeerd. De volgende code laat het ons raadplegen:

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    // Abort the transaction
    await txn.AbortAsync();
}
```
Wordt ervan uitgegaan dat de items uit de wachtrij in de volgende volgorde zijn geplaatst:
> 10, 20

Wanneer we de transactie afgebroken, zou de items terug naar de kop van de wachtrij in een van de volgende bestellingen worden toegevoegd:
> 10, 20

> 20, 10

Hetzelfde geldt voor alle gevallen waarin de transactie niet met succes is *doorgevoerd*.

## <a name="programming-patterns"></a>Patronen programmering
In deze sectie laat het ons kijken enkele programmering patronen die mogelijk nuttig zijn bij het gebruik van ReliableConcurrentQueue.

### <a name="batch-dequeues"></a>Batch Dequeues
Een aanbevolen programming patroon is voor de taak consumer batch de dequeues in plaats van een wachtrij op een tijdstip. De gebruiker kan kiezen om te beperken vertragingen tussen elke batch of de batchgrootte van de. Het volgende codefragment bevat dit programmeermodel.  Houd er rekening mee dat in dit voorbeeld wordt het verwerken is voltooid nadat de transactie doorgevoerd is, dus als een fout optreden tijdens de verwerking van, de niet-verwerkte items zullen verloren zonder dat is verwerkt.  U kunt ook kan de verwerking worden uitgevoerd binnen het transactiebereik, maar dit een negatieve invloed op prestaties hebben kan en vereist dat de verwerking van de items die al is verwerkt.

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

### <a name="best-effort-notification-based-processing"></a>Verwerking van Best-Effort op basis van meldingen
Een ander interessant programming patroon gebruikt de Count-API. Hier kunnen we best-effort verwerking van de wachtrij op basis van meldingen implementeren. Aantal van de wachtrij kan worden gebruikt om te beperken van een wachtrij plaatsen of een taak wachtrij halen.  Houd er rekening mee dat zoals in het vorige voorbeeld omdat de verwerking buiten de transactie vindt niet-verwerkte items mogelijk verloren als er een fout optreedt tijdens de verwerking.

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

### <a name="best-effort-drain"></a>Best-Effort leegmaken
Leegmaken van de wachtrij kan niet worden gegarandeerd vanwege de gelijktijdige aard van de structuur van de gegevens.  Het is mogelijk dat, zelfs als er geen bewerkingen van de gebruiker in de wachtrij onderweg zijn, een aanroep van bepaalde TryDequeueAsync kan niet als resultaat een item die eerder in de wachtrij en doorgevoerd.  Het item in de wachtrij is gegarandeerd *uiteindelijk* zichtbaar te maken in wachtrij, maar zonder een out-of-band-communicatiemechanisme een onafhankelijke consumer kan niet weet dat de wachtrij heeft bereikt een stabiele status zelfs als alle producenten is gestopt en nieuwe wachtrij plaatsen bewerkingen zijn toegestaan. De bewerking leegmaken is dus best-effort zoals hieronder wordt geïmplementeerd.

De gebruiker moet alle verdere producenten en consumenten taken stoppen en wachten op onderweg transacties die zijn doorgevoerd of afgebroken voordat u probeert te verwijderen uit de wachtrij.  Als de gebruiker het verwachte aantal items in de wachtrij kent, kunnen ze een melding die geeft aan dat alle items uit wachtrij hebben is geplaatst instellen.

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
ReliableConcurrentQueue zorgt niet voor de *TryPeekAsync* api. Gebruikers kunnen krijgen de peek semantische met behulp van een *TryDequeueAsync* en vervolgens de transactie afgebroken. In dit voorbeeld dequeues worden verwerkt alleen als de waarde van het item groter dan is *10*.

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
* [Snel starten Reliable Services](service-fabric-reliable-services-quick-start.md)
* [Werken met betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
* [Betrouwbare Services meldingen](service-fabric-reliable-services-notifications.md)
* [Reliable Services Backup and Restore (herstel na noodgevallen)](service-fabric-reliable-services-backup-restore.md)
* [Configuratie van betrouwbare status Manager](service-fabric-reliable-services-configuration.md)
* [Aan de slag met Service Fabric Web API-Services](service-fabric-reliable-services-communication-webapi.md)
* [Geavanceerd gebruik van het programmeermodel betrouwbare voor Services](service-fabric-reliable-services-advanced-usage.md)
* [Referentie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
