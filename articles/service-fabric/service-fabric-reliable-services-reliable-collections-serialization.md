---
title: Betrouwbare verzameling object serialisatie in Azure Service Fabric | Microsoft Docs
description: Azure Service Fabric betrouwbare verzamelingen object serialisatie
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 9d35374c-2d75-4856-b776-e59284641956
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/8/2017
ms.author: mcoskun
ms.openlocfilehash: 0687baf12a48788d86467b1f1a822b5d9050e5d5
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="reliable-collection-object-serialization-in-azure-service-fabric"></a>Betrouwbare verzameling object serialisatie in Azure Service Fabric
Betrouwbare verzamelingen repliceren en behouden hun objecten om te controleren of dat ze via machine fouten en stroomstoringen duurzaam zijn.
Om te repliceren en om vast te leggen items moeten betrouwbare verzamelingen te serialiseren.

Betrouwbare verzamelingen voor het ophalen van de juiste serialisatiefunctie voor een bepaald type van betrouwbare status Manager.
Statusbeheer voor betrouwbaar bevat ingebouwde objectserializers en kan aangepaste objectserializers te registreren voor een bepaald type.

## <a name="built-in-serializers"></a>Ingebouwde objectserializers

Statusbeheer voor betrouwbaar bevat ingebouwde serialisatiefunctie voor enkele algemene typen, zodat kan efficiënt standaard worden geserialiseerd. Voor andere typen betrouwbare statusbeheer terugvalt voor het gebruik van de [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).
Ingebouwde objectserializers zijn efficiënter omdat ze weten dat hun typen kunnen niet worden gewijzigd en hoeven ze niet met informatie over het type, zoals de typenaam.

Betrouwbaar status Manager beschikt over ingebouwde serialisatiefunctie voor het volgende typen: 
- GUID
- BOOL
- byte
- sbyte
- byte[]
- CHAR
- tekenreeks
- Decimale
- dubbel
- drijvend
- int
- uint
- lang
- ulong
- korte
- USHORT

## <a name="custom-serialization"></a>Aangepaste serialisatie

Aangepaste objectserializers wordt vaak gebruikt om prestaties te verhogen of voor het versleutelen van de gegevens via de kabel en op schijf. Aangepaste objectserializers zijn onder andere redenen, meestal efficiënter dan algemene serialisatiefunctie omdat ze niet nodig voor het serialiseren van informatie over het type. 

[IReliableStateManager.TryAddStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer--1?Microsoft_ServiceFabric_Data_IReliableStateManager_TryAddStateSerializer__1_Microsoft_ServiceFabric_Data_IStateSerializer___0__) wordt gebruikt voor het registreren van een aangepaste serialisatiefunctie voor het opgegeven type T. Deze registratie vindt plaats in de constructie van de StatefulServiceBase om ervoor te zorgen dat voordat het herstel wordt gestart, alle betrouwbare verzamelingen toegang hebben tot de serializer van de relevante hun persistente gegevens lezen.

```csharp
public StatefulBackendService(StatefulServiceContext context)
  : base(context)
  {
    if (!this.StateManager.TryAddStateSerializer(new OrderKeySerializer()))
    {
      throw new InvalidOperationException("Failed to set OrderKey custom serializer");
    }
  }
```

> [!NOTE]
> Aangepaste objectserializers krijgen voorrang boven ingebouwde objectserializers. Bijvoorbeeld, wanneer een aangepaste serialisatiefunctie voor int is geregistreerd, wordt deze gebruikt voor het serialiseren van gehele getallen in plaats van de ingebouwde serialisatiefunctie voor int.

### <a name="how-to-implement-a-custom-serializer"></a>Het implementeren van een aangepaste serialisatiefunctie

Geen aangepaste serialisatiefunctie moet voor het implementeren van de [IStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) interface.

> [!NOTE]
> IStateSerializer<T> bevat een overbelasting voor schrijven en lezen die in een extra T basiswaarde aangeroepen. Deze API is voor de differentiële serialisatie. Differentiële serialisatie-functie is momenteel niet beschikbaar gemaakt. Daarom zijn deze twee overloads niet aangeroepen totdat differentiële serialisatie is beschikbaar gemaakt en ingeschakeld.

Hieronder volgt een voorbeeld van een aangepast type aangeroepen OrderKey met vier eigenschappen

```csharp
public class OrderKey : IComparable<OrderKey>, IEquatable<OrderKey>
{
    public byte Warehouse { get; set; }

    public short District { get; set; }

    public int Customer { get; set; }

    public long Order { get; set; }

    #region Object Overrides for GetHashCode, CompareTo and Equals
    #endregion
}
```

Hieronder volgt een voorbeeld van de implementatie van IStateSerializer<OrderKey>.
Merk op dat in lezen en schrijven overloads die baseValue, hun respectieve overload-aanroep voor doorsturen compatibiliteit.

```csharp
public class OrderKeySerializer : IStateSerializer<OrderKey>
{
  OrderKey IStateSerializer<OrderKey>.Read(BinaryReader reader)
  {
      var value = new OrderKey();
      value.Warehouse = reader.ReadByte();
      value.District = reader.ReadInt16();
      value.Customer = reader.ReadInt32();
      value.Order = reader.ReadInt64();

      return value;
  }

  void IStateSerializer<OrderKey>.Write(OrderKey value, BinaryWriter writer)
  {
      writer.Write(value.Warehouse);
      writer.Write(value.District);
      writer.Write(value.Customer);
      writer.Write(value.Order);
  }
  
  // Read overload for differential de-serialization
  OrderKey IStateSerializer<OrderKey>.Read(OrderKey baseValue, BinaryReader reader)
  {
      return ((IStateSerializer<OrderKey>)this).Read(reader);
  }

  // Write overload for differential serialization
  void IStateSerializer<OrderKey>.Write(OrderKey baseValue, OrderKey newValue, BinaryWriter writer)
  {
      ((IStateSerializer<OrderKey>)this).Write(newValue, writer);
  }
}
```

## <a name="upgradability"></a>Kunnen
In een [rolling upgrade van de toepassing](service-fabric-application-upgrade.md), de upgrade wordt toegepast op een deelverzameling met knooppunten, één upgradedomein tegelijk. Tijdens dit proces wordt een aantal upgradedomeinen worden uitgevoerd in de nieuwere versie van uw toepassing en een aantal upgradedomeinen bevindt zich op de oudere versie van uw toepassing. De nieuwe versie van uw toepassing moet kunnen lezen van de oude versie van uw gegevens tijdens de implementatie en de oude versie van uw toepassing moet kunnen lezen van de nieuwe versie van uw gegevens. Als de gegevensindeling niet voorwaarts en achterwaarts compatibel is, wordt de upgrade mislukken of slechter, gegevens mogelijk verloren gegaan of beschadigd.

Als u ingebouwde serialisatiefunctie gebruikt, hoeft u geen zorgen te hoeven maken over de compatibiliteit.
Als u geen aangepaste serialisatiefunctie of de DataContractSerializer gebruikt, hebben de gegevens voor oneindig doorsturen en achterwaartse compatibiliteit.
Elke versie van de serialisatiefunctie moet met andere woorden, kunnen voor het serialiseren en deserialiseren van een willekeurige versie van het type.

Data Contract-gebruikers dienen de goed gedefinieerde versieregels voor toevoegen, verwijderen en wijzigen van de velden te volgen. Gegevenscontract biedt ook ondersteuning voor het omgaan met onbekende velden en omgaan met klassenovername vereist in het proces voor serialisatie en deserialisatie. Zie voor meer informatie [gegevenscontract met behulp van](https://msdn.microsoft.com/library/ms733127.aspx).

Aangepaste serialisatiefunctie gebruikers moeten voldoen aan de richtlijnen van de serializer die ze gebruiken achterwaartse is en stuurt om compatibel te maken.
Algemene manier voor het ondersteunen van alle versies is de grootte van gegevens toe te voegen aan het begin en alleen optionele eigenschappen toe te voegen.
Op deze manier elke versie kan lezen als veel kunt en springen via het resterende deel van de stroom.

## <a name="next-steps"></a>Volgende stappen
  * [Serialisatie en upgrade](service-fabric-application-upgrade-data-serialization.md)
  * [Referentie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  * [Een upgrade van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u bij een upgrade van de toepassing met Visual Studio.
  * [Een upgrade van uw toepassing met behulp van Powershell](service-fabric-application-upgrade-tutorial-powershell.md) begeleidt u bij een upgrade van de toepassing met behulp van PowerShell.
  * Bepalen hoe uw toepassing wordt bijgewerkt met behulp van [Parameters Upgrade](service-fabric-application-upgrade-parameters.md).
  * Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).
  * Veelvoorkomende problemen oplossen in toepassingsupgrades door te verwijzen naar de stappen in [toepassingsupgrades probleemoplossing](service-fabric-application-upgrade-troubleshooting.md).
