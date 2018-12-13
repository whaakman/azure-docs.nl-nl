---
title: Werken met betrouwbare verzamelingen | Microsoft Docs
description: Informatie over de aanbevolen procedures voor het werken met betrouwbare verzamelingen.
services: service-fabric
documentationcenter: .net
author: tylermsft
manager: timlt
editor: ''
ms.assetid: 39e0cd6b-32c4-4b97-bbcf-33dad93dcad1
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/19/2017
ms.author: twhitney
ms.openlocfilehash: 86e1370bb5241dbe14b34cebe2f2ee6d71a0a323
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53193532"
---
# <a name="working-with-reliable-collections"></a>Werken met betrouwbare verzamelingen
Service Fabric biedt een stateful programmeermodel beschikbaar voor .NET-ontwikkelaars via betrouwbare verzamelingen. Specifiek, biedt Service Fabric betrouwbare dictionary en betrouwbare wachtrij klassen. Wanneer u deze klassen, uw status is gepartitioneerd (voor schaalbaarheid), gerepliceerd (voor beschikbaarheid) en transactionele binnen een partitie (voor ACID-semantiek). Laten we kijken naar een typische gebruik van een betrouwbare dictionary-object en zien wat de daadwerkelijk aan het doen.

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

Alle bewerkingen op een betrouwbare dictionary-objecten (met uitzondering van ClearAsync die niet ongedaan maken), vereisen een ITransaction-object. Dit object is gekoppeld aan deze een en alle wijzigingen die u probeert te maken op een betrouwbare dictionary en/of betrouwbare objecten in één partitie in de wachtrij. U hebt aanschaft, een ITransaction object door het aanroepen van de partitie de StateManager van CreateTransaction methode.

In de bovenstaande code wordt de ITransaction-object doorgegeven aan een betrouwbare dictionary AddAsync methode. Intern, maken woordenlijst methoden die accepteert van een sleutel gebruik van een vergrendeling reader/writer is gekoppeld aan de sleutel. Als de methode van de sleutel waarde wijzigt, gaat de methode een schrijfvergrendeling voor de sleutel en als de methode alleen uit van de sleutel-waarde leest, wordt een leesvergrendeling is genomen van de sleutel. Aangezien AddAsync Hiermee wijzigt u de waarde van de sleutel op de nieuwe, doorgegeven waarde, wordt de sleutel-/ schrijfvergrendeling genomen. Dus als 2 (of meer) threads probeert het toevoegen van waarden met dezelfde sleutel op hetzelfde moment, één thread wordt de schrijfvergrendeling verkrijgen en de andere threads worden geblokkeerd. Standaard methoden voor maximaal 4 seconden om te verkrijgen van de vergrendeling; blokkeren de methoden genereren na 4 seconden een TimeoutException. Methode overloads bestaan zodat u een expliciete time-outwaarde doorgeven als u liever wilt.

Meestal kunt u uw code schrijven om te reageren op een TimeoutException door het afvangen en opnieuw te proberen de gehele bewerking (zoals weergegeven in de bovenstaande code). In mijn eenvoudig code, ben ik net Task.Delay doorgeven van 100 milliseconden telkens wanneer aanroepen. Maar in werkelijkheid wellicht beter met behulp van een soort exponentieel uitstel vertraging in plaats daarvan.

Zodra de vergrendeling is aangeschaft, wordt de sleutel en waarde objectverwijzingen naar een interne tijdelijke woordenlijst die is gekoppeld aan de ITransaction-object door AddAsync toegevoegd. Dit wordt gedaan om te voorzien van read-your-own-writes semantiek. Dat wil zeggen, nadat u AddAsync aanroepen, retourneert een hoger aanroep TryGetValueAsync (met behulp van de dezelfde ITransaction-object) de waarde, zelfs als u de transactie nog niet zijn doorgevoerd. Vervolgens AddAsync serialiseert uw sleutel en waarde om bytematrices objecten en deze bytematrices toegevoegd aan een bestand op het lokale knooppunt. Ten slotte verzendt AddAsync de bytematrices naar de secundaire replica's, zodat ze dezelfde sleutel/waarde-gegevens hebben. Hoewel de sleutel/waarde-gegevens is geschreven naar een logboekbestand, de informatie wordt niet beschouwd als onderdeel van de woordenlijst totdat de transactie die ze zijn gekoppeld aan doorgevoerd zijn.

In de bovenstaande code voert de aanroep van CommitAsync alle bewerkingen van de transactie. Met name het doorvoeren informatie toegevoegd aan het bestand op het lokale knooppunt en verzendt ook de commit-record naar de secundaire replica's. Zodra een quorum (meerderheid) van de replica's heeft gereageerd, alle gegevenswijzigingen worden beschouwd als permanente en eventuele vergrendelingen die zijn gekoppeld aan de sleutels die zijn bewerkt via de ITransaction-object worden vrijgegeven, zodat andere threads/transacties dezelfde sleutels kunt bewerken en de bijbehorende de waarden.

Als CommitAsync is niet aangeroepen (meestal als gevolg van een uitzondering), haalt de ITransaction-object verwijderd. Wanneer verwijdering van een niet-doorgevoerde ITransaction-object, Service Fabric afbreken informatie toegevoegd aan het lokale knooppunt bestand en er niets hoeft te worden verzonden naar een van de secundaire replica's. En vervolgens een vergrendeling die is gekoppeld aan de sleutels die zijn bewerkt via de transactie worden uitgebracht.

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Veel voorkomende valkuilen en hoe u deze kunt vermijden
Nu dat u hoe de betrouwbare verzamelingen werken intern begrijpt, laten we eens enkele algemene bovendien hiervan. Zie de onderstaande code:

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

Als u werkt met een reguliere .NET-woordenlijst, kunt u een sleutel/waarde toevoegen aan de woordenlijst en wijzig de waarde van een eigenschap (zoals LastLogin). Maar werkt deze code niet goed met een betrouwbare dictionary. Houd er rekening mee uit de eerdere discussie de aanroep van AddAsync serialiseert van de sleutel/waarde-objecten voor bytematrices en vervolgens naar een lokaal bestand opslaat de matrices en verzendt ze ook naar de secundaire replica's. Als u later een eigenschap wijzigt, verandert deze de waarde van de eigenschap in het geheugen. Dit heeft geen invloed op het lokale bestand of de gegevens die worden verzonden naar de replica's. Als het proces vastloopt, is wat is er in het geheugen weggegooid. Wanneer een nieuw proces wordt gestart of als een andere replica primaire wordt, is de waarde van de oude eigenschap wat er beschikbaar is.

Ik kan geen stress genoeg hoe eenvoudig dat het is om te maken van het soort fout dat hierboven wordt weergegeven. En u alleen informatie over de fout als/wanneer dat het proces uitvalt. De juiste manier om de code te schrijven is gewoon omkeren van de twee regels:


```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync();
}
```

Hier volgt een voorbeeld van een algemene fout:

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

Nogmaals, met reguliere .NET woordenboeken, de bovenstaande code werkt prima en is een algemeen patroon: de ontwikkelaar een sleutel wordt gebruikt om te controleren of een waarde. Als de waarde bestaat, verandert de ontwikkelaar van de waarde van de eigenschap. Met betrouwbare verzamelingen, zorgt deze code hetzelfde probleem zoals eerder besproken: **moet u een object niet wijzigen nadat u deze op een betrouwbare verzameling hebt gegeven.**

De juiste manier om een waarde in een betrouwbare verzameling te werken is geen verwijzing ophalen naar de bestaande waarde en houd rekening met het object waarnaar wordt verwezen door deze verwijzing onveranderbaar. Vervolgens maakt u een nieuw object dat een exacte kopie van het oorspronkelijke object. U kunt nu de status van deze nieuwe object wijzigen en het nieuwe object in de verzameling zodat deze wordt geserialiseerd naar bytematrices, toegevoegd aan het lokale bestand en verzonden naar de replica's schrijven. Nadat de wijzigingen zijn doorgevoerd, hebben de objecten in het geheugen, het lokale bestand en alle replica's dezelfde exacte staat. Alles goed is!

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

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a>Onveranderbare gegevenstypen om te voorkomen dat programmeurs fout definiëren
In het ideale geval wij willen graag de compiler fouten rapporteren wanneer u per ongeluk code die de status van een object dat u moet overwegen onveranderbare mutates produceren. Maar, de C# compiler beschikt niet over de mogelijkheid om dit te doen. Dus, om te voorkomen van potentiële programmeur bugs, is het raadzaam dat u de typen definieert u met betrouwbare verzamelingen zijn onveranderd typen. Dit houdt in dat u aan de regels Houd waardetypen core (zoals cijfers [Int32, UInt64, enz.], DateTime, Guid, TimeSpan en dergelijke). En natuurlijk kunt u ook tekenreeks gebruiken. Het is raadzaam om te voorkomen dat de eigenschappen van de verzameling als serialiseren en deserialiseren van ze kan vaak kunnen de prestaties nadelig beïnvloeden. Echter, als u gebruiken van de eigenschappen van verzameling wilt, wij raden het gebruik van. De NET onveranderbare verzamelingen bibliotheek ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)). Deze bibliotheek is beschikbaar voor downloaden van http://nuget.org. Het beste ook verzegelen van de klassen en het aanbrengen van velden alleen-lezen indien mogelijk.

Het type gebruikersgegevens hieronder ziet u hoe u een onveranderbare type profiteren van de hiervoor genoemde aanbevelingen definiëren.

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

Het type ItemId is ook een onveranderbare type, zoals hier wordt weergegeven:

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
Betrouwbare verzamelingen serialiseren intern, uw objecten gebruiken. De NET DataContractSerializer. De geserialiseerde objecten worden opgeslagen op de lokale schijf van de primaire replica en worden ook verzonden naar de secundaire replica's. Als uw service zich verder ontwikkelt, is het waarschijnlijk dat moet u het soort gegevens (schema) is vereist voor uw service te wijzigen. U moet het versiebeheer van uw gegevens met uiterst voorzichtig benaderen. Allereerst omdat, moet u altijd deserialiseren van oude gegevens zijn. Dit houdt in dat uw code deserialisatie oneindig achterwaarts compatibel moet zijn: Versie 333 van de servicecode van uw moet kunnen worden uitgevoerd op gegevens die worden geplaatst in een betrouwbare verzameling met versie 1 van de servicecode van uw 5 jaar geleden.

Bovendien is de servicecode bijgewerkte één upgradedomein tegelijk. Daarom, tijdens een upgrade uitvoert moet u twee verschillende versies van uw servicecode die gelijktijdig wordt uitgevoerd. U moet voorkomen wanneer u de nieuwe versie van de servicecode die het nieuwe schema gebruiken zoals oude versies van de servicecode van uw mogelijk niet voor het afhandelen van het nieuwe schema. Indien mogelijk moet u elke versie van uw service zijn voorwaarts compatibel met versie 1 ontwerpen. Dit betekent met name V1-versie van de servicecode van uw moet kunnen gewoon negeren alle schema-elementen die dit expliciet niet verwerkt. Echter, moet het mogelijk zijn alle gegevens die deze expliciet niet kent en gewoon schrijven het weer bij het bijwerken van een woordenlijst sleutel of een waarde op te slaan.

> [!WARNING]
> Terwijl u het schema van een sleutel wijzigen kunt, moet u ervoor zorgen dat de hash-code van uw sleutel en is gelijk aan-algoritmen stabiel zijn. Als u hoe een van deze algoritmen werken wijzigen, wordt het niet mogelijk om te controleren of de sleutel in de betrouwbare dictionary ooit opnieuw.
>
>

U kunt ook uitvoeren wat wordt meestal aangeduid als een upgrade van een 2-fase. Met de upgrade van een 2-fase, u uw service upgraden van V1 in V2: V2 bevat de code die weet hoe u omgaat met de nieuwe schemawijziging, maar deze code wordt uitgevoerd. Wanneer de code V2 V1 gegevens leest, is van invloed op deze en V1-gegevens worden geschreven. Vervolgens, nadat de upgrade voltooid voor alle upgrade-domeinen is, u kunt op een signaal verzenden naar de V2-exemplaren die worden uitgevoerd dat de upgrade voltooid is. (Eén manier om het signaal is dit een upgrade van een configuratie worden uitgerold; dit is wat is dit een upgrade van een 2-fase.) Nu kunnen de V2-exemplaren V1-gegevens lezen, converteren naar een V2-gegevens worden uitgevoerd en schrijf deze als V2-gegevens. Wanneer andere exemplaren V2 gegevens leest, ze hoeven niet te converteren, ze alleen worden uitgevoerd en V2-gegevens schrijven.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het maken van voorwaarts compatibel gegevenscontracten, [voorwaarts compatibel gegevenscontracten](https://msdn.microsoft.com/library/ms731083.aspx).

Zie voor meer best practices voor versiebeheer gegevenscontracten [Data Contract versiebeheer](https://msdn.microsoft.com/library/ms731138.aspx).

Zie voor informatie over het implementeren van versie fouttolerante gegevenscontracten, [versie fouttolerante serialisatie Callbacks](https://msdn.microsoft.com/library/ms733734.aspx).

Zie voor informatie over het bieden van een gegevensstructuur die voor de verschillende versies kunt samenwerken, [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
