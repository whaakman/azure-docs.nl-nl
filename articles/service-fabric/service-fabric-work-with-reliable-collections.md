---
title: Werken met betrouwbare collecties | Microsoft Docs
description: Meer informatie over de aanbevolen procedures voor het werken met betrouwbare verzamelingen.
services: service-fabric
documentationcenter: .net
author: rajak
manager: timlt
editor: 
ms.assetid: 39e0cd6b-32c4-4b97-bbcf-33dad93dcad1
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/19/2017
ms.author: rajak
ms.openlocfilehash: f53f13e4fb83b1cd370ec673e86e5311cd93055f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="working-with-reliable-collections"></a>Werken met betrouwbare verzamelingen
Service Fabric biedt een stateful programmeermodel beschikbaar voor .NET-ontwikkelaars via betrouwbare verzamelingen. In het bijzonder biedt Service Fabric betrouwbare woordenlijst en betrouwbare wachtrij klassen. Wanneer u deze klassen gebruikt, wordt uw status gepartitioneerd (voor schaalbaarheid), gerepliceerd (voor beschikbaarheid) en transactionele binnen een partitie (voor ACID-semantiek). Laten we kijken naar het standaardgebruik van een betrouwbare dictionary-object en zien welke ervan daadwerkelijk doen.

```csharp

///retry:

try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) {
   await Task.Delay(100, cancellationToken); goto retry;
}
```

Alle bewerkingen op betrouwbare dictionary-objecten (met uitzondering van ClearAsync die niet ongedaan maken), vereisen een ITransaction-object. Dit object is gekoppeld aan een en alle wijzigingen die u probeert te maken aan de woordenlijst van een betrouwbare en/of betrouwbare objecten binnen één partitie in een wachtrij. Verkrijgen van een ITransaction object door het aanroepen van de partitie de StateManager van CreateTransaction methode.

In de bovenstaande code wordt de ITransaction-object doorgegeven aan een betrouwbare woordenlijst AddAsync methode. Intern woordenlijst methoden die een sleutel accepteert maken gebruik van een lezer/schrijver vergrendeling die zijn gekoppeld aan de sleutel. Als de methode van de sleutel waarde wijzigt, gaat de methode een schrijfvergrendeling voor de sleutel en als de methode is alleen wordt gelezen van de waarde van de sleutel, wordt een leesvergrendeling genomen voor de sleutel. Aangezien AddAsync waarde van de sleutel op de nieuwe, doorgegeven waarde wijzigt, wordt de sleutel-/ schrijfvergrendeling genomen. Dus 2 (of hoger) threads probeert toe te voegen van waarden met dezelfde sleutel op hetzelfde moment, één thread wordt de schrijfvergrendeling verkrijgen als de andere threads wordt geblokkeerd. Standaard methoden blokkeren voor maximaal 4 seconden de vergrendeling; de methoden genereren na 4 seconden een TimeoutException. Methode overloads bestaan zodat u een expliciete time-outwaarde doorgeven als u liever.

Meestal kunt u uw code schrijven om te reageren op een TimeoutException door het afvangen en u de hele bewerking (zoals weergegeven in de bovenstaande code). In mijn eenvoudige code alleen gebeld Task.Delay 100 milliseconden elke keer dat wordt doorgegeven. Maar in werkelijkheid is het mogelijk dat beter met een soort exponentiële back-uit vertraging in plaats daarvan.

Zodra de vergrendeling wordt verkregen, voegt AddAsync de sleutel en waarde objectverwijzingen naar een interne tijdelijke woordenlijst is gekoppeld aan de ITransaction-object. Dit wordt gedaan om te lezen-your-eigenaar-schrijven semantiek bieden. Dat wil zeggen, na het aanroepen van AddAsync een aanroep van TryGetValueAsync (met hetzelfde object ITransaction) hoger de waarde als resultaat zelfs als u zich nog niet de transactie verplicht hebt. Vervolgens AddAsync serialiseert uw sleutel en waarde objecten naar byte matrices en voegt deze byte-matrices met een logboekbestand op het lokale knooppunt. AddAsync verzendt ten slotte de byte-matrices naar de secundaire replica's zodat ze dezelfde sleutel/waarde-gegevens hebben. Hoewel de sleutel/waarde-informatie is geschreven naar een logboekbestand, de informatie wordt niet beschouwd als onderdeel van het woordenboek totdat de transactie die ze zijn gekoppeld aan is toegewezen.

In de code voert de aanroep van CommitAsync alle bewerkingen van de transactie. In het bijzonder commit informatie toegevoegd aan het logboekbestand op het lokale knooppunt en verstuurt ook de commit-record op de secundaire replica's. Zodra een quorum (meerderheid) van de replica's al heeft geantwoord, worden alle gegevenswijzigingen als permanente beschouwd en alle sleutels die zijn gemanipuleerd via de ITransaction-object gekoppeld vergrendelingen zijn vrijgegeven zodat andere threads/transacties dezelfde sleutels kunt bewerken en hun waarden.

Als CommitAsync (meestal als gevolg van een uitzondering wordt veroorzaakt) niet wordt aangeroepen, haalt de ITransaction-object verwijderd. Bij de buitengebruikstelling van een niet-doorgevoerde ITransaction-object, Service Fabric afbreken informatie toegevoegd aan het lokale knooppunt logboekbestand en hoeft u niets worden verzonden naar een van de secundaire replica's. En vervolgens alle vergrendelingen die zijn gekoppeld aan de sleutels die zijn gemanipuleerd via de transactie zijn vrijgegeven.

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Veelvoorkomende valkuilen en hoe te vermijden
Nu dat u hoe de betrouwbare verzamelingen intern werken begrijpt, gaan we verdiepen in enkele algemene misbruik van deze. Zie de volgende code:

```csharp
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes,
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
```

Als u werkt met een normale .NET-woordenlijst, kunt u een sleutel/waarde toevoegen aan de woordenlijst en wijzig de waarde van een eigenschap (zoals LastLogin). Echter, deze code werkt niet correct met een betrouwbare woordenlijst. Onthouden van de eerdere discussie de aanroep van AddAsync serialiseert van de sleutel/waarde-objecten voor matrices byte en slaat vervolgens de matrices met een lokaal bestand en verzendt ze ook naar de secundaire replica's. Als u later een eigenschap wijzigt, Hiermee kunt u de waarde van de eigenschap in het geheugen. Dit heeft geen gevolgen voor het lokale bestand of de gegevens die worden verzonden met de replica's. Als het proces vastloopt, is wat is er in het geheugen weggegooid. Wanneer een nieuw proces wordt gestart of als een andere replica primaire wordt, is de waarde van de oude eigenschap wat beschikbaar is.

Ik kan geen stress genoeg hoe eenvoudig dat het is om te maken van het soort fout hierboven weergegeven. En u alleen leert over de fout als of wanneer dat het proces wordt afgesloten. De juiste manier om de code te schrijven is gewoon omkeren van de twee regels:


```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync();
}
```

Hier volgt een voorbeeld weergegeven van een algemene fout:

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user =
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync();
   }
}
```

Opnieuw met normale .NET woordenboeken, de bovenstaande code werkt goed samen en is een algemene patroon: de ontwikkelaar een sleutel gebruikt om een waarde te zoeken. Als de waarde bestaat, verandert de ontwikkelaar van de waarde van de eigenschap. Met betrouwbare verzamelingen echter vertoont deze code hetzelfde probleem al besproken: **moet u een object niet wijzigen nadat u deze aan een betrouwbare verzameling hebt gegeven.**

De juiste manier om een waarde in een betrouwbare verzameling bijwerken is het een verwijzing naar de bestaande waarde ophalen en houd rekening met het object waarnaar wordt verwezen door deze verwijzing onveranderbaar. Vervolgens maakt u een nieuw object dat een exacte kopie van het originele object. U kunt nu de status van dit nieuwe object wijzigen en schrijven van het nieuwe object in de verzameling zodat deze wordt geserialiseerd naar byte-matrices toegevoegd aan het lokale bestand en verzonden naar de replica's. Nadat de wijzigingen zijn doorgevoerd, hebben de objecten in het geheugen, het lokale bestand en alle replica's dezelfde exact overeen. Goede is.

De onderstaande code ziet u de juiste manier om een waarde in een betrouwbare verzameling te werken:

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser =
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync();
   }
}
```

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a>Niet-wijzigbaar gegevenstypen om te voorkomen dat programmeurs fout definiëren
Willen we graag de compiler fouten rapporteren in het ideale geval wanneer u per ongeluk code die de status van een object dat u moet rekening houden met niet-wijzigbaar mutates produceren. Maar de C# compiler heeft niet de mogelijkheid om dit te doen. Dus Voorkom mogelijke programmeurs fouten ten zeerste aanbevolen dat u de typen definieert u met betrouwbare verzamelingen om te worden niet-wijzigbaar typen. Dit houdt in dat u Houd u aan de waardetypen core (zoals getallen [Int32, UInt64, enz.], DateTime, Guid, TimeSpan en dergelijke). En natuurlijk kunt u ook tekenreeks gebruiken. Het is raadzaam om te voorkomen dat de eigenschappen van de verzameling als serialiseren en een deserialiseren ze kunt vaak kunt lagere prestaties. Echter, als u gebruiken van eigenschappen van verzameling wilt, we raden het gebruik van. NET van niet-wijzigbaar verzamelingen bibliotheek ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)). Deze bibliotheek is beschikbaar voor downloaden van http://nuget.org. Ook wordt aangeraden uw klassen verzegelen en het aanbrengen van velden alleen-lezen indien mogelijk.

Het type gebruikersgegevens hieronder laat zien hoe een onveranderbare type profiteren van de hiervoor genoemde aanbevelingen definiëren.

```csharp

[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;

   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }

   [DataMember]
   public readonly String Email;

   // Ideally, this would be a readonly field but it can't be because OnDeserialized
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
```

Het type ItemId is ook een niet-wijzigbaar type, zoals hier wordt weergegeven:

```csharp

[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
```

## <a name="schema-versioning-upgrades"></a>Schemaversies (upgrades)
Intern serialiseren betrouwbare verzamelingen uw objecten gebruikt. De NET DataContractSerializer. De geserialiseerde objecten zijn opgeslagen op lokale schijf van de primaire replica en de secundaire replica's ook worden verzonden. Naarmate uw service meer vormt krijgt, is het waarschijnlijk wilt u het type gegevens (schema), die uw service nodig wijzigen. U moet versiebeheer van uw gegevens met uiterst voorzichtig benaderen. Allereerst omdat kunt u moet altijd oude gegevens worden gedeserialiseerd. Dit houdt in uw code deserialisatie oneindig achterwaarts compatibel moet zijn: 333 versie van uw servicecode moet kunnen worden uitgevoerd voor de gegevens worden geplaatst in een betrouwbare verzameling met versie 1 van uw servicecode 5 jaar geleden.

Bovendien is de servicecode bijgewerkte één upgradedomein tegelijk. Tijdens een upgrade hebt u dus twee verschillende versies van uw servicecode die tegelijkertijd wordt uitgevoerd. U moet voorkomen met de nieuwe versie van uw servicecode gebruik van het nieuwe schema oude versies van uw servicecode wordt mogelijk niet kunnen verwerken van het nieuwe schema. Indien mogelijk moet u elke versie van de service worden voorwaarts compatibel met versie 1 ontwerpen. Dit betekent in het bijzonder V1 van uw servicecode moet kunnen schema-elementen die dit niet expliciet verwerkt gewoon negeren. Maar moet deze kunnen gegevens weet niet expliciet over en gewoon schrijven weer uit bij het bijwerken van een woordenboeksleutel of een waarde op te slaan.

> [!WARNING]
> Terwijl u het schema van een sleutel wijzigen kunt, moet u ervoor zorgen dat uw sleutel hash-code en is gelijk aan algoritmen stabiel zijn. Als u hoe een van deze algoritmen werken wijzigen, kunt u zich niet opzoeken van de sleutel in het woordenboek betrouwbare ooit opnieuw.
>
>

U kunt ook uitvoeren wat wordt meestal aangeduid als een upgrade van een 2-fase. Met de upgrade van een 2-fase, upgrade uw service van V1 naar V2: V2 bevat de code die het omgaan met de nieuwe schemawijziging kent, maar deze code wordt niet uitgevoerd. Wanneer de code V2 V1 gegevens leest, is van invloed op deze en V1-gegevens worden geschreven. Vervolgens, nadat de upgrade voltooid voor alle upgrade-domeinen is, u kunt enigszins signaal aan de actieve V2-exemplaren dat de upgrade voltooid is. (Eén manier om signaal is dit Implementeer de upgrade van een configuratie; Dit is wat is dit een upgrade van een 2-fase.) Nu kunnen de V2-exemplaren V1-gegevens lezen, converteren naar V2-gegevens, op worden uitgevoerd en uitschrijven als V2-gegevens. Wanneer andere exemplaren V2-gegevens lezen, ze hoeven niet te converteren, worden ze alleen worden uitgevoerd en V2-gegevens schrijven.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het maken van voorwaarts compatibel gegevenscontracten, [voorwaarts compatibel gegevenscontracten](https://msdn.microsoft.com/library/ms731083.aspx).

Zie voor meer informatie over aanbevolen procedures op versiebeheer gegevenscontracten, [Data Contract Versioning](https://msdn.microsoft.com/library/ms731138.aspx).

Zie voor meer informatie over het implementeren van versie fouttolerante gegevenscontracten, [versie fouttolerante serialisatie retouraanroepen](https://msdn.microsoft.com/library/ms733734.aspx).

Zie voor informatie over het opgeven van een gegevensstructuur die voor de verschillende versies kan samenwerken, [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
