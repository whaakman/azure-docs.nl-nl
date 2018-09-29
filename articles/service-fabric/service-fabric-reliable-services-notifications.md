---
title: Meldingen van betrouwbare Services | Microsoft Docs
description: Algemene documentatie voor Service Fabric Reliable Services-meldingen
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,vturecek
ms.assetid: cdc918dd-5e81-49c8-a03d-7ddcd12a9a76
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/29/2017
ms.author: mcoskun
ms.openlocfilehash: a13e5d74390b82888f51cfd225c54e29550354e9
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47433511"
---
# <a name="reliable-services-notifications"></a>Meldingen van betrouwbare Services
Kennisgevingen kunnen clients de wijzigingen bijhouden die worden aangebracht in een object waarin ze geïnteresseerd bent. Meldingen voor ondersteunt twee typen objecten: *betrouwbare status Manager* en *betrouwbare Dictionary*.

Veelvoorkomende redenen voor het gebruik van meldingen zijn:

* Het bouwen van gerealiseerde weergaven, zoals secundaire indexen of geaggregeerde gefilterde weergaven van de status van de replica. Een voorbeeld is een gesorteerde index van alle sleutels in een betrouwbare Dictionary.
* Verzonden gegevens, zoals het aantal gebruikers die zijn toegevoegd in het afgelopen uur.

Meldingen worden gestart als onderdeel van het toepassen van bewerkingen. Vanwege die worden meldingen zo snel mogelijk en synchrone gebeurtenissen mag niet bevatten dure bewerkingen behandeld.

## <a name="reliable-state-manager-notifications"></a>Betrouwbare status Manager-meldingen
Betrouwbare status Manager biedt meldingen voor de volgende gebeurtenissen:

* Transactie
  * Doorvoeren
* Status manager
  * Opnieuw samenstellen
  * Toevoeging van een betrouwbare status
  * Het verwijderen van een betrouwbare status

Betrouwbare status Manager houdt bij of de huidige actieve transacties. De enige wijziging in de status van de transactie die ervoor zorgt dat een melding om te worden geactiveerd is een transactie wordt doorgevoerd.

Betrouwbare status Manager onderhoudt een verzameling van betrouwbare statussen, zoals betrouwbare Dictionary en betrouwbare wachtrij. Meldingen van betrouwbare status Manager wordt geactiveerd wanneer deze verzameling wordt gewijzigd: een betrouwbare status is toegevoegd of verwijderd, of de hele verzameling is opnieuw opgebouwd.
De verzameling van betrouwbare status Manager is opnieuw opgebouwd in drie gevallen:

* Herstel: Wanneer een replica wordt gestart, het herstelt de vorige status van de schijf. Aan het einde van het herstel wordt **NotifyStateManagerChangedEventArgs** een gebeurtenis die de set van herstelde betrouwbare statussen bevat.
* Volledige kopie: voordat u een replica kan deelnemen aan de configuratieset, heeft kunnen worden gebouwd. Soms moet een volledige kopie van betrouwbare status Manager de status van de primaire replica moet worden toegepast op de niet-actieve secundaire replica. Maakt gebruik van betrouwbare status Manager op de secundaire replica **NotifyStateManagerChangedEventArgs** een gebeurtenis die de set van betrouwbare statussen die wordt verkregen via de primaire replica bevat.
* Herstellen: In herstel na noodgevallen, de status van de replica kan worden hersteld vanuit een back-up via **RestoreAsync**. In dergelijke gevallen betrouwbare status Manager op de primaire replica gebruikt **NotifyStateManagerChangedEventArgs** een gebeurtenis die de set van betrouwbare statussen die wordt hersteld op basis van de back-up bevat.

Als u wilt registreren voor meldingen en/of de status manager-meldingen, moet u registreren bij de **TransactionChanged** of **StateManagerChanged** gebeurtenissen op betrouwbare status Manager. Een algemene plaats om u te registreren bij deze gebeurtenis-handlers is de constructor van de stateful service. Wanneer u zich bij de constructor registreren, kunt u een melding die wordt veroorzaakt door een wijziging tijdens de levensduur van wordt niet missen **IReliableStateManager**.

```csharp
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

De **TransactionChanged** maakt gebruik van gebeurtenis-handler **NotifyTransactionChangedEventArgs** voor meer informatie over de gebeurtenis. Deze bevat de eigenschap action (bijvoorbeeld **NotifyTransactionChangedAction.Commit**) die aangeeft dat het type wijziging. Het bevat ook de transactie-eigenschap waarmee een verwijzing naar de transactie die gewijzigd.

> [!NOTE]
> Vandaag de dag **TransactionChanged** gebeurtenissen worden gegenereerd, alleen als de transactie doorgevoerd wordt. De actie is gelijk aan **NotifyTransactionChangedAction.Commit**. Maar in de toekomst gebeurtenissen voor andere soorten statuswijzigingen transactie kunnen worden verhoogd. U wordt aangeraden de actie controleren en verwerken van de gebeurtenis alleen als het is een App die u verwacht.
> 
> 

Hieronder volgt een voorbeeld **TransactionChanged** gebeurtenis-handler.

```csharp
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

De **StateManagerChanged** maakt gebruik van gebeurtenis-handler **NotifyStateManagerChangedEventArgs** voor meer informatie over de gebeurtenis.
**NotifyStateManagerChangedEventArgs** heeft twee subklassen: **NotifyStateManagerRebuildEventArgs** en **NotifyStateManagerSingleEntityChangedEventArgs**.
Gebruikmaken van de eigenschap actie **NotifyStateManagerChangedEventArgs** gecast **NotifyStateManagerChangedEventArgs** voor de juiste subklasse:

* **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
* **NotifyStateManagerChangedAction.Add** en **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Hieronder volgt een voorbeeld **StateManagerChanged** wijzigingsmelding.

```csharp
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStateManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Betrouwbare woordenlijst meldingen
Betrouwbare Dictionary biedt meldingen voor de volgende gebeurtenissen:

* Opnieuw opbouwen: Aangeroepen wanneer **ReliableDictionary** de status van een herstelde of gekopieerd lokale status- of back-up is hersteld.
* Wissen: Aangeroepen wanneer de status van **ReliableDictionary** is uitgeschakeld via de **ClearAsync** methode.
* Toevoegen: Wordt aangeroepen wanneer een item is toegevoegd aan **ReliableDictionary**.
* Update: Wordt aangeroepen wanneer een item in **IReliableDictionary** is bijgewerkt.
* Verwijderen: Wordt aangeroepen wanneer een item in **IReliableDictionary** is verwijderd.

Als u betrouwbare Dictionary-meldingen, moet u registreren bij de **DictionaryChanged** gebeurtenis-handler op **IReliableDictionary**. Een algemene plaats om u te registreren bij deze gebeurtenis-handlers is in de **ReliableStateManager.StateManagerChanged** melding toevoegen.
Registreren bij **IReliableDictionary** wordt toegevoegd aan **IReliableStateManager** zorgt ervoor dat u meldingen wordt niet mist.

```csharp
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
        }
    }
}
```

> [!NOTE]
> **ProcessStateManagerSingleEntityNotification** is de voorbeeldmethode die de voorgaande **OnStateManagerChangedHandler** voorbeeld aanroepen.
> 
> 

De bovenstaande code wordt de **IReliableNotificationAsyncCallback** interface, samen met **DictionaryChanged**. Omdat **NotifyDictionaryRebuildEventArgs** bevat een **iasyncenumerable geretourneerd** interface--die moet worden geïnventariseerd asynchroon--meldingen opnieuw worden gestart via  **RebuildNotificationAsyncCallback** in plaats van **OnDictionaryChangedHandler**.

```csharp
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

> [!NOTE]
> In de bovenstaande code, als onderdeel van de verwerking van de melding opnieuw maken eerst de onderhouden geaggregeerde status gewist. Omdat de betrouwbare verzameling wordt opnieuw opgebouwd met een nieuwe status, zijn alle eerdere meldingen niet relevant.
> 
> 

De **DictionaryChanged** maakt gebruik van gebeurtenis-handler **NotifyDictionaryChangedEventArgs** voor meer informatie over de gebeurtenis.
**NotifyDictionaryChangedEventArgs** vijf subklassen heeft. Gebruik de actie-eigenschap in **NotifyDictionaryChangedEventArgs** gecast **NotifyDictionaryChangedEventArgs** voor de juiste subklasse:

* **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
* **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
* **NotifyDictionaryChangedAction.Add** en **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
* **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
* **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```csharp
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Aanbevelingen
* *Voer* meldingsgebeurtenissen zo snel mogelijk te voltooien.
* *Geen* dure bewerkingen (bijvoorbeeld: i/o-bewerkingen) worden uitgevoerd als onderdeel van de synchrone gebeurtenissen.
* *Voer* controleert u het actietype voordat u de gebeurtenis verwerkt. Nieuwe actietypen mogelijk in de toekomst worden toegevoegd.

Hier volgen een aantal zaken waarmee u rekening moet houden:

* Meldingen worden gestart als onderdeel van de uitvoering van een bewerking. Bijvoorbeeld, wordt een melding terugzetten geactiveerd als de laatste stap van een herstelbewerking uit. Een herstelpunt wordt niet voltooien totdat de meldingsgebeurtenis is verwerkt.
* Omdat de meldingen worden geactiveerd als onderdeel van de toepassing bewerkingen, Zie clients alleen meldingen voor lokaal toegezegde bewerkingen. En omdat bewerkingen worden gegarandeerd alleen te worden lokaal toegepast (met andere woorden, vastgelegd), ze kunnen wel of kan niet ongedaan worden gemaakt in de toekomst.
* Op het pad opnieuw wordt één melding geactiveerd voor elke bewerking toegepast. Dit betekent dat als transactie T1 Create(X) Delete(X) en Create(X) bevat, u een melding voor het maken van de X-en één voor de verwijdering, en één voor het maken, in die volgorde ontvangt.
* Voor transacties die meerdere bewerkingen bevatten, worden de bewerkingen in de volgorde waarin ze zijn ontvangen op de primaire replica van de gebruiker toegepast.
* Als onderdeel van de verwerking van de waarde false wordt uitgevoerd, bepaalde bewerkingen mogelijk ongedaan worden gemaakt. Meldingen worden gegenereerd voor deze bewerkingen ongedaan maken, de status van de replica worden teruggedraaid naar een stabiel punt. Een belangrijk verschil van meldingen voor ongedaan maken is dat de gebeurtenissen die dubbele sleutels hebben worden samengevoegd. Bijvoorbeeld, als transactie T1 is ongedaan wordt gemaakt, ziet u een eenmalige kennisgeving aan Delete(X).

## <a name="next-steps"></a>Volgende stappen
* [Betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
* [Snel starten met betrouwbare Services](service-fabric-reliable-services-quick-start.md)
* [Reliable Services back-up en herstellen (herstel na noodgevallen)](service-fabric-reliable-services-backup-restore.md)
* [Referentie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

