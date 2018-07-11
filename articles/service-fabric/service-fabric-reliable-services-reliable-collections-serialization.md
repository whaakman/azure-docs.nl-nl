---
title: Betrouwbare verzamelingserialisatie van het type object in Azure Service Fabric | Microsoft Docs
description: Azure Service Fabric Reliable Collections object serialisatie
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 9d35374c-2d75-4856-b776-e59284641956
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/8/2017
ms.author: mcoskun
ms.openlocfilehash: 8fb6f1767741e950b300fd297250a6b64656191c
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952423"
---
# <a name="reliable-collection-object-serialization-in-azure-service-fabric"></a>Betrouwbare verzamelingserialisatie van het type object in Azure Service Fabric
Betrouwbare verzamelingen repliceren en hun items om ervoor te zorgen dat ze duurzame zijn voor de machine fouten en stroomstoringen behouden.
Om te repliceren en om vast te leggen items moeten betrouwbare verzamelingen te serialiseren.

Betrouwbare verzamelingen voor het ophalen van de serializer van de juiste voor een bepaald type van betrouwbare status Manager.
Betrouwbare status Manager bevat ingebouwde objectserializers en kunt aangepaste objectserializers worden geregistreerd voor een bepaald type.

## <a name="built-in-serializers"></a>Ingebouwde objectserializers

Betrouwbare status Manager bevat ingebouwde serialisatiefunctie voor enkele algemene typen, zodat ze efficiënt kunnen worden geserialiseerd standaard. Voor andere typen betrouwbare status Manager terugvalt voor het gebruik van de [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).
Ingebouwde objectserializers zijn efficiënter omdat ze weten dat de typen niet wijzigen en ze hoeven niet informatie over het type, zoals de typenaam op te nemen.

Betrouwbare status Manager beschikt over ingebouwde serialisatiefunctie voor het volgende typen: 
- GUID
- BOOL
- byte
- sbyte
- byte[]
- CHAR
- tekenreeks
- decimaal
- double
- drijvend
- int
- uint
- lengte
- ULONG
- korte
- USHORT

## <a name="custom-serialization"></a>Aangepaste serialisatie

Aangepaste objectserializers worden vaak gebruikt om prestaties te verhogen of voor het versleutelen van de gegevens via de kabel en op schijf. Aangepaste objectserializers zijn onder andere redenen, meestal efficiënter dan algemene serializer omdat ze niet nodig hebben om te serialiseren van informatie over het type. 

[IReliableStateManager.TryAddStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer) wordt gebruikt voor het registreren van een aangepaste serializer voor het opgegeven type T. Deze registratie moet gebeuren in de constructie van de StatefulServiceBase om ervoor te zorgen dat voordat herstel wordt gestart, alle betrouwbare verzamelingen toegang hebben tot de relevante serializer om hun persistente gegevens te lezen.

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
> Aangepaste objectserializers krijgen voorrang op ingebouwde objectserializers. Bijvoorbeeld, als een aangepaste serializer voor int is geregistreerd, wordt deze gebruikt om te serialiseren van gehele getallen in plaats van de ingebouwde serialisatiefunctie voor int.

### <a name="how-to-implement-a-custom-serializer"></a>Het implementeren van een aangepaste serializer

Een aangepaste serializer nodig heeft voor het implementeren van de [IStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) interface.

> [!NOTE]
> IStateSerializer<T> bevat een overbelasting voor schrijven en lezen die in een extra T basiswaarde genoemd. Deze API is voor de differentiële serialisatie. Differentiële serialisatie-functie is momenteel niet beschikbaar gemaakt. Daarom kan deze twee overloads niet worden aangeroepen totdat differentiële serialisatie wordt beschikbaar gemaakt en ingeschakeld.

Hieronder volgt een voorbeeld van een aangepaste type OrderKey met vier eigenschappen aangeroepen

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
Merk op dat lezen en schrijven overloads die nemen in baseValue, hun respectieve overload voor compatibiliteit met doorsturen aan te roepen.

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
In een [rolling upgrade van de toepassing](service-fabric-application-upgrade.md), de upgrade wordt toegepast op een subset van knooppunten, één upgradedomein tegelijk. Tijdens dit proces, aantal upgradedomeinen worden uitgevoerd in de nieuwere versie van uw toepassing en aantal upgradedomeinen zullen zijn op de oudere versie van uw toepassing. Tijdens de implementatie, de nieuwe versie van uw toepassing moet in staat om te lezen van de oude versie van uw gegevens zijn en de oude versie van uw toepassing moet in staat om te lezen van de nieuwe versie van uw gegevens zijn. Als de gegevensindeling niet compatibel met voorwaartse en achterwaartse is, wordt de upgrade mislukken, of nog erger, gegevens mogelijk verloren of beschadigd.

Als u met behulp van ingebouwde serializer, hoeft u geen zorgen te hoeven maken over de compatibiliteit.
Als u van een aangepaste serializer of de DataContractSerializer gebruikmaakt, hebben de gegevens voor oneindig doorstuurt en achterwaartse compatibiliteit.
Met andere woorden, moet elke versie van de serializer kan serialiseren en deserialiseren op een willekeurige versie van het type.

Data Contract gebruikers moeten volgen de goed gedefinieerde versiebeheer regels voor het toevoegen, verwijderen en wijzigen van velden. Gegevenscontract biedt ook ondersteuning voor het omgaan met onbekende velden en omgaan met klassenovername Inhaken op het proces van serialisatie en deserialisatie. Zie voor meer informatie, [met behulp van Data Contract](https://msdn.microsoft.com/library/ms733127.aspx).

Aangepaste serializer-gebruikers moeten voldoen aan de richtlijnen van de serializer die ze gebruiken of deze achterwaartse en verzendt om compatibel te maken.
Veelgebruikte manier om ondersteunen alle versies is informatie over de grootte toe te voegen aan het begin en alleen optioneel eigenschappen toe te voegen.
Op deze manier elke versie kan lezen veel kunt en gaan via het resterende deel van de stroom.

## <a name="next-steps"></a>Volgende stappen
  * [Serialisatie en upgrade](service-fabric-application-upgrade-data-serialization.md)
  * [Referentie voor ontwikkelaars voor betrouwbare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  * [Een upgrade van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u bij de upgrade van een toepassing met Visual Studio.
  * [Een upgrade van uw toepassing met behulp van Powershell](service-fabric-application-upgrade-tutorial-powershell.md) begeleidt u bij de upgrade van een toepassing met behulp van PowerShell.
  * Bepalen hoe uw toepassing met behulp van upgrades [Parameters Upgrade](service-fabric-application-upgrade-parameters.md).
  * Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).
  * Oplossen van veelvoorkomende problemen in upgrades van toepassingen door te verwijzen naar de stappen in [toepassingsupgrades oplossen van problemen](service-fabric-application-upgrade-troubleshooting.md).
