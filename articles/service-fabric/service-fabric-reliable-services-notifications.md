---
title: Meldingen voor betrouwbare Services | Microsoft Docs
description: Conceptuele documentatie voor Service Fabric Reliable Services meldingen
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,vturecek
ms.assetid: cdc918dd-5e81-49c8-a03d-7ddcd12a9a76
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/29/2017
ms.author: mcoskun
ms.openlocfilehash: 8b8a0aad23c6c4ceaf23dd3fbde5daef3519fdcf
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="reliable-services-notifications"></a>Betrouwbare Services meldingen
Kennisgevingen kunnen clients de wijzigingen die worden uitgevoerd op een object dat ze geïnteresseerd bent in bijhouden. Twee soorten objecten ondersteunen meldingen: *betrouwbare statusbeheer* en *betrouwbare woordenlijst*.

Er zijn veelvoorkomende redenen voor het gebruik van meldingen:

* Gebouw gematerialiseerd weergaven, zoals secundaire indexen of geaggregeerd gefilterde weergaven van de status van de replica. Een voorbeeld is een gesorteerde index van alle sleutels in betrouwbare woordenlijst.
* Verzenden bewakingsgegevens, zoals het aantal gebruikers die zijn toegevoegd in het afgelopen uur.

Meldingen worden gestart als onderdeel van het toepassen van bewerkingen. Vanwege die worden meldingen zo snel mogelijk en synchrone gebeurtenissen mag niet zijn dure bewerkingen behandeld.

## <a name="reliable-state-manager-notifications"></a>Betrouwbaar status Manager-meldingen
Betrouwbaar status Manager biedt meldingen voor de volgende gebeurtenissen:

* Transactie
  * Doorvoeren
* Status manager
  * Opnieuw samenstellen
  * Toevoeging van een betrouwbare status
  * Verwijderen van een betrouwbare status

Statusbeheer voor betrouwbaar houdt de huidige inflight transacties. De enige wijziging in transactiestatus dat ervoor zorgt dat een melding om te worden gestart, is een transactie wordt doorgevoerd.

Statusbeheer voor betrouwbaar onderhoudt een verzameling van betrouwbare statussen zoals betrouwbare woordenlijst en betrouwbare wachtrij. Statusbeheer voor betrouwbaar meldingen wordt geactiveerd wanneer deze verzameling wordt gewijzigd: een betrouwbare status wordt toegevoegd of verwijderd of de volledige verzameling worden opnieuw wordt opgebouwd.
De verzameling betrouwbare status Manager is opnieuw opgebouwd in drie gevallen:

* Herstel: Wanneer een replica wordt gestart, het herstelt de vorige status van de schijf. Aan het einde van het herstel van de hierbij **NotifyStateManagerChangedEventArgs** een gebeurtenis die de set van herstelde betrouwbare statussen bevat.
* Volledige kopiëren: voordat u een replica kunt deelnemen aan de configuratieset, heeft kunnen worden gebouwd. Soms moet hiervoor een volledige kopie van betrouwbare status Manager de status van de primaire replica moet worden toegepast op de niet-actieve secundaire replica. Betrouwbaar status Manager op de secundaire replica gebruikt **NotifyStateManagerChangedEventArgs** een gebeurtenis met het aantal betrouwbare statussen die die is verkregen van de primaire replica.
* Terugzetten: In scenario's voor herstel na noodgevallen, de status van de replica kan worden hersteld vanuit een back-up via **RestoreAsync**. In dergelijke gevallen betrouwbare status Manager op de primaire replica gebruikt **NotifyStateManagerChangedEventArgs** een gebeurtenis met de set van betrouwbare statussen die deze uit de back-up hersteld.

Als u wilt registreren voor meldingen en/of de status manager-meldingen, moet u registreren bij de **TransactionChanged** of **StateManagerChanged** gebeurtenissen op betrouwbare status Manager. Een algemene plaats om te registreren bij deze gebeurtenis-handlers is de constructor van uw stateful service. Wanneer u zich bij de constructor registreren, u een melding die wordt veroorzaakt door een wijziging tijdens de levensduur van niet naar hun **IReliableStateManager**.

```csharp
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

De **TransactionChanged** maakt gebruik van gebeurtenis-handler **NotifyTransactionChangedEventArgs** om details over de gebeurtenis te verstrekken. Deze bevat de eigenschap action (bijvoorbeeld **NotifyTransactionChangedAction.Commit**) die aangeeft welk type wijziging. Het bevat ook de eigenschap transaction waarmee een verwijzing naar de transactie die gewijzigd.

> [!NOTE]
> Vandaag de dag **TransactionChanged** gebeurtenissen worden gegenereerd, alleen als de transactie doorgevoerd is. De actie is gelijk aan **NotifyTransactionChangedAction.Commit**. Maar in de toekomst gebeurtenissen voor andere soorten statuswijzigingen transactie kunnen worden verhoogd. Het is raadzaam om de actie controleren en verwerken van de gebeurtenis alleen als het is een die u verwacht.
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

De **StateManagerChanged** maakt gebruik van gebeurtenis-handler **NotifyStateManagerChangedEventArgs** om details over de gebeurtenis te verstrekken.
**NotifyStateManagerChangedEventArgs** twee subklassen: **NotifyStateManagerRebuildEventArgs** en **NotifyStateManagerSingleEntityChangedEventArgs**.
Gebruikmaken van de eigenschap actie **NotifyStateManagerChangedEventArgs** voor het casten van **NotifyStateManagerChangedEventArgs** voor de juiste subklasse:

* **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
* **NotifyStateManagerChangedAction.Add** en **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Hieronder volgt een voorbeeld **StateManagerChanged** meldings-handler.

```csharp
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Betrouwbare woordenlijst meldingen
Betrouwbare woordenlijst biedt meldingen voor de volgende gebeurtenissen:

* Opnieuw maken: Aangeroepen wanneer **ReliableDictionary** heeft de status is hersteld van een herstelde of gekopieerde lokale staat of de back-up.
* Wissen: Aangeroepen wanneer de status van **ReliableDictionary** is uitgeschakeld via de **ClearAsync** methode.
* Toevoegen: Wordt aangeroepen wanneer een item is toegevoegd aan **ReliableDictionary**.
* Update: Wordt aangeroepen wanneer een item in **IReliableDictionary** is bijgewerkt.
* Verwijderen: Wordt aangeroepen wanneer een item in **IReliableDictionary** is verwijderd.

Als u meldingen betrouwbare woordenlijst, moet u registreren bij de **DictionaryChanged** gebeurtenis-handler op **IReliableDictionary**. Een algemene plaats om te registreren bij deze gebeurtenis-handlers bevindt zich in de **ReliableStateManager.StateManagerChanged** melding toevoegen.
Wanneer u registreert **IReliableDictionary** wordt toegevoegd aan **IReliableStateManager** zorgt ervoor dat u meldingen niet naar hun.

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
}
```

> [!NOTE]
> **ProcessStateManagerSingleEntityNotification** is de voorbeeldmethode die de voorgaande **OnStateManagerChangedHandler** voorbeeld aanroepen.
> 
> 

De voorgaande codesets de **IReliableNotificationAsyncCallback** interface, samen met **DictionaryChanged**. Omdat **NotifyDictionaryRebuildEventArgs** bevat een **IAsyncEnumerable** interface--die moet worden geïnventariseerd asynchroon--meldingen opnieuw worden gestart via **RebuildNotificationAsyncCallback** in plaats van **OnDictionaryChangedHandler**.

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
> In de voorgaande code, als onderdeel van de verwerking van de melding opnieuw opbouwen eerst de handhaven geaggregeerde status gewist. Omdat de verzameling betrouwbare wordt opnieuw opgebouwd met een nieuwe status, zijn alle eerdere meldingen niet relevant.
> 
> 

De **DictionaryChanged** maakt gebruik van gebeurtenis-handler **NotifyDictionaryChangedEventArgs** om details over de gebeurtenis te verstrekken.
**NotifyDictionaryChangedEventArgs** vijf subklassen. Gebruik de eigenschap action in **NotifyDictionaryChangedEventArgs** voor het casten van **NotifyDictionaryChangedEventArgs** voor de juiste subklasse:

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
* *Geen* dure bewerkingen (bijvoorbeeld i/o-bewerkingen) worden uitgevoerd als onderdeel van synchrone gebeurtenissen.
* *Voer* controleren van het actietype voordat u de gebeurtenis verwerkt. Nieuwe actietypen mogelijk in de toekomst worden toegevoegd.

Hier volgen een aantal zaken rekening moet houden:

* Meldingen worden gestart als onderdeel van de uitvoering van een bewerking. Bijvoorbeeld, wordt een melding herstellen gestart als de laatste stap van een herstelbewerking. Een herstelpunt wordt niet voltooien totdat de meldingsgebeurtenis is verwerkt.
* Omdat de meldingen worden gestart als onderdeel van de bewerkingen toepassen, Zie clients alleen meldingen voor lokaal doorgevoerd bewerkingen. En omdat bewerkingen zijn gegarandeerd alleen lokaal toegewezen (met andere woorden, vastgelegd), ze kunnen wel of kan niet ongedaan worden gemaakt in de toekomst.
* Op het pad opnieuw wordt één melding geactiveerd voor elke toegepaste bewerking. Dit betekent dat als transactie T1 Create(X) Delete(X) en Create(X) bevat, u een melding voor het maken van de X-en één voor de verwijdering en één voor het maken van de opnieuw in die volgorde krijgt.
* Voor transacties die meerdere bewerkingen bevatten, worden bewerkingen in de volgorde waarin ze zijn ontvangen op de primaire replica van de gebruiker toegepast.
* Als onderdeel van de verwerking van false voortgang bepaalde bewerkingen mogelijk ongedaan worden gemaakt. Meldingen worden gegenereerd voor dergelijke bewerkingen ongedaan maken, de status van de replica terugdraaien naar een stabiele punt. Een belangrijk verschil van meldingen voor ongedaan maken is dat de gebeurtenissen die dubbele sleutels hebben worden geaggregeerd. Bijvoorbeeld, als T1 transactie wordt ongedaan wordt gemaakt, ziet u een eenmalige melding Delete(X).

## <a name="next-steps"></a>Volgende stappen
* [Betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
* [Betrouwbare Services snel starten](service-fabric-reliable-services-quick-start.md)
* [Betrouwbare Services back-up en herstel (herstel na noodgevallen)](service-fabric-reliable-services-backup-restore.md)
* [Referentie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

