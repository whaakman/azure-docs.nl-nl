---
title: 'Toepassingsupgrade: gegevensserialisatie | Microsoft Docs'
description: Aanbevolen procedures voor serialisatie van gegevens en hoe dit van invloed op rolling upgrades van toepassingen.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: a5f36366-a2ab-4ae3-bb08-bc2f9533bc5a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 55cbd869e7434469ebddd7af493c91bfedafc594
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58665675"
---
# <a name="how-data-serialization-affects-an-application-upgrade"></a>Hoe serialisatie van gegevens is van invloed op een toepassingsupgrade
In een [rolling upgrade van de toepassing](service-fabric-application-upgrade.md), de upgrade wordt toegepast op een subset van knooppunten, één upgradedomein tegelijk. Tijdens dit proces, aantal upgradedomeinen zijn op de nieuwere versie van uw toepassing en sommige upgradedomeinen zijn op de oudere versie van uw toepassing. Tijdens de implementatie, de nieuwe versie van uw toepassing moet in staat om te lezen van de oude versie van uw gegevens zijn en de oude versie van uw toepassing moet in staat om te lezen van de nieuwe versie van uw gegevens zijn. Als de gegevensindeling niet compatibel met voorwaartse en achterwaartse is, wordt de upgrade mislukken, of nog erger, gegevens mogelijk verloren of beschadigd. In dit artikel wordt beschreven wat de gegevensindeling van uw om te achterhalen en biedt aanbevolen procedures om ervoor te zorgen dat uw gegevens vooruit en achteruit is compatibel.

## <a name="what-makes-up-your-data-format"></a>Waaruit bestaat de gegevensindeling van uw?
In Azure Service Fabric, de gegevens die worden opgeslagen en gerepliceerd afkomstig is van uw C# klassen. Voor toepassingen die gebruikmaken van [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md), dat gegevens de objecten in de betrouwbare woordenboeken en wachtrijen worden. Voor toepassingen die gebruikmaken van [Reliable Actors](service-fabric-reliable-actors-introduction.md), dat wil zeggen de status van back-ups maken voor de actor. Deze C# klassen moeten worden geserialiseerd worden opgeslagen en gerepliceerd. Daarom wordt de gegevensindeling gedefinieerd door de velden en eigenschappen die zijn geserialiseerd, evenals hoe ze worden geserialiseerd. Bijvoorbeeld, in een `IReliableDictionary<int, MyClass>` de gegevens is een geserialiseerde `int` en een geserialiseerde `MyClass`.

### <a name="code-changes-that-result-in-a-data-format-change"></a>Code wijzigt die resulteren in een gegevens-indeling wijzigen
Omdat de gegevensindeling wordt bepaald door C# klassen, wijzigingen in de klassen die kunnen leiden tot een gegevenswijziging-indeling. Wees voorzichtig om ervoor te zorgen dat de wijziging van de indeling gegevens kan worden verwerkt door een rolling upgrade. Voorbeelden en leiden gegevenswijzigingen van de indeling tot kunnen:

* Toevoegen of verwijderen van velden of properties
* Wijzigen van velden of properties
* Het wijzigen van de typen velden of properties
* Wijzigen van de naam van klasse of naamruimte

### <a name="data-contract-as-the-default-serializer"></a>Als de serializer van de standaard-gegevenscontract
De serializer is doorgaans verantwoordelijk voor het lezen van gegevens en bij het deserialiseren van het in de huidige versie, zelfs als de gegevens zich in een oudere of *nieuwere* versie. De serializer van de standaard is de [gegevenscontract serializer](https://msdn.microsoft.com/library/ms733127.aspx), die is voorzien van versiebeheer goed gedefinieerde regels. Betrouwbare verzamelingen kunnen de serializer kunnen worden overschreven, maar Reliable Actors op dit moment niet. De serializer van de gegevens speelt een belangrijke rol bij het inschakelen van rolling upgrades. De serializer van de Data Contract is de serializer die wij voor Service Fabric-toepassingen aanraden.

## <a name="how-the-data-format-affects-a-rolling-upgrade"></a>Hoe de gegevensindeling van invloed is op een rolling upgrade
Tijdens een rolling upgrade, er zijn twee hoofdscenario's waar de serializer van een oudere kan optreden of *nieuwere* versie van uw gegevens:

1. Nadat een knooppunt wordt bijgewerkt en opnieuw is gestart, de serializer van de nieuwe zal worden gebruikt voor het laden van de gegevens dat is aangehouden op schijf op de oude versie.
2. Tijdens de rolling upgrade wordt het cluster een combinatie van de oude en nieuwe versies van uw code bevatten. Omdat de replica's kunnen worden geplaatst in verschillende domeinen van de upgrade en replica's verzenden van gegevens met elkaar, kan de nieuwe en/of de oude versie van uw gegevens worden aangetroffen door de nieuwe en/of de oude versie van de serializer.

> [!NOTE]
> De ' nieuwe ' en 'oude versie' hier verwijzen naar de versie van de code die wordt uitgevoerd. De 'nieuwe serializer"verwijst naar de serializer-code die wordt uitgevoerd in de nieuwe versie van uw toepassing. De 'nieuwe gegevens"verwijst naar de geserialiseerde C# klasse van de nieuwe versie van uw toepassing.
> 
> 

De twee versies van de indeling van code en gegevens moeten vooruit en achteruit compatibel is. Als ze niet compatibel is zijn, de rolling upgrade mislukken of gegevens verloren kunnen gaan. De rolling upgrade mislukken omdat de code of serializer kan throw uitzonderingen of een fout wanneer wordt vastgesteld dat de andere versie. Mogelijk gaan gegevens verloren als u bijvoorbeeld een nieuwe eigenschap is toegevoegd, maar de serializer van de oude verwijdert alle tijdens de deserialisatie.

Data Contract is de aanbevolen oplossing om ervoor te zorgen dat uw gegevens compatibel is. Het bevat versiebeheer van goed gedefinieerde regels voor het toevoegen, verwijderen en wijzigen van velden. Het bevat ook ondersteuning voor het omgaan met onbekende velden en omgaan met klassenovername Inhaken op het proces van serialisatie en deserialisatie. Zie voor meer informatie, [met behulp van Data Contract](https://msdn.microsoft.com/library/ms733127.aspx).

## <a name="next-steps"></a>Volgende stappen
[Een upgrade van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u bij de upgrade van een toepassing met Visual Studio.

[Een upgrade van uw toepassing met behulp van Powershell](service-fabric-application-upgrade-tutorial-powershell.md) begeleidt u bij de upgrade van een toepassing met behulp van PowerShell.

Bepalen hoe uw toepassing met behulp van upgrades [Parameters Upgrade](service-fabric-application-upgrade-parameters.md).

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Oplossen van veelvoorkomende problemen in upgrades van toepassingen door te verwijzen naar de stappen in [toepassingsupgrades oplossen van problemen](service-fabric-application-upgrade-troubleshooting.md).

