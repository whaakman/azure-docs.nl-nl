---
title: 'Upgrade van de toepassing: serialisatie van gegevens | Microsoft Docs'
description: Aanbevolen procedures voor serialisatie van gegevens en de mogelijke gevolgen voor rollende toepassingsupgrades.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: a5f36366-a2ab-4ae3-bb08-bc2f9533bc5a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 6aa3ac7842df4657fca7f6b4264e1c6fe52dc0c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-data-serialization-affects-an-application-upgrade"></a>De invloed van serialisatie van gegevens op een upgrade van de toepassing
In een [rolling upgrade van de toepassing](service-fabric-application-upgrade.md), de upgrade wordt toegepast op een deelverzameling met knooppunten, één upgradedomein tegelijk. Tijdens dit proces een aantal upgradedomeinen zijn op de nieuwere versie van uw toepassing en een aantal upgradedomeinen zijn op de oudere versie van uw toepassing. De nieuwe versie van uw toepassing moet kunnen lezen van de oude versie van uw gegevens tijdens de implementatie en de oude versie van uw toepassing moet kunnen lezen van de nieuwe versie van uw gegevens. Als de gegevensindeling niet voorwaarts en achterwaarts compatibel is, wordt de upgrade mislukken of slechter, gegevens mogelijk verloren gegaan of beschadigd. Dit artikel wordt beschreven wat wordt verstaan onder de indeling van uw gegevens en biedt de aanbevolen procedures om ervoor te zorgen dat uw gegevens voorwaarts en achterwaarts zijn compatibel.

## <a name="what-makes-up-your-data-format"></a>Wat is de indeling van uw gegevens?
In de Azure Service Fabric, worden de gegevens die is opgeslagen en gerepliceerd afkomstig van uw C#-klassen. Voor toepassingen die gebruikmaken van [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md), dat gegevens de objecten in de betrouwbare woordenlijsten en wachtrijen worden. Voor toepassingen die gebruikmaken van [Reliable Actors](service-fabric-reliable-actors-introduction.md), die de status van back-ups maken voor de actor. Deze C#-klassen moeten serialiseerbaar zijn opgeslagen en gerepliceerd. Daarom wordt de gegevensindeling gedefinieerd door de velden en eigenschappen die zijn geserialiseerd, evenals hoe ze worden geserialiseerd. Bijvoorbeeld in een `IReliableDictionary<int, MyClass>` de gegevens is een geserialiseerde `int` en een geserialiseerde `MyClass`.

### <a name="code-changes-that-result-in-a-data-format-change"></a>Code wijzigt die resulteren in een wijziging van de indeling gegevens
Aangezien de gegevensindeling wordt bepaald door de C#-klassen, mogelijk wijzigingen aan de klassen indeling gewijzigde gegevens. Moet nauwkeurig om ervoor te zorgen dat de wijziging van de indeling gegevens kan omgaan met een uitrollende upgrade. Voorbeelden kunnen leiden tot gegevenswijzigingen van de indeling:

* Het toevoegen of verwijderen van velden of eigenschappen
* Naamswijzigingen velden of eigenschappen
* De typen van velden of eigenschappen wijzigen
* Wijzigen van de naam van klasse of naamruimte

### <a name="data-contract-as-the-default-serializer"></a>Gegevenscontract als de serialisatiefunctie standaard
De serialisatiefunctie is doorgaans verantwoordelijk voor het lezen van de gegevens en bij het deserialiseren van het in de huidige versie, zelfs als de gegevens in een oudere of *nieuwere* versie. De serialisatiefunctie standaard is de [gegevenscontract serialisatiefunctie](https://msdn.microsoft.com/library/ms733127.aspx), die goed gedefinieerde versieregels heeft. Betrouwbare verzamelingen kunnen de serialisatiefunctie moet worden overschreven, maar Reliable Actors momenteel niet. De serialisatiefunctie gegevens speelt een belangrijke rol bij het inschakelen van rollende upgrades. De serialisatiefunctie gegevenscontract is de serialisatiefunctie die we voor Service Fabric-toepassingen aanbevelen.

## <a name="how-the-data-format-affects-a-rolling-upgrade"></a>De invloed van de indeling van de gegevens op een uitrollende upgrade
Tijdens een rolling upgrade er zijn twee hoofdscenario's waar de serialisatiefunctie een oudere ondervinden of *nieuwere* versie van uw gegevens:

1. Nadat een knooppunt is bijgewerkt en wordt een back-up wordt gestart, de serializer van de nieuwe laden van de gegevens die is opgeslagen op schijf door de oude versie.
2. Tijdens de upgrade rolling bevat het cluster een combinatie van de oude en nieuwe versies van uw code. Omdat replica's kunnen worden geplaatst in verschillende domeinen van de upgrade en replica's verzenden van gegevens met elkaar, kan de nieuwe en/of de oude versie van uw gegevens door de nieuwe en/of de oude versie van de serializer worden aangetroffen.

> [!NOTE]
> De ' nieuwe ' en 'oude versie' hier verwijzen naar de versie van uw code die wordt uitgevoerd. De 'nieuwe serializer"verwijst naar de serialisatiefunctie-code die wordt uitgevoerd in de nieuwe versie van uw toepassing. De 'nieuwe gegevens"verwijst naar de geserialiseerde C#-klasse van de nieuwe versie van uw toepassing.
> 
> 

De twee versies van code en gegevens indeling moet voorwaarts en achterwaarts compatibel. Als ze niet compatibel is, de rolling upgrade mislukken of gegevens zijn mogelijk verloren gegaan. De rolling upgrade mislukken omdat de code of serialisatiefunctie veroorzaken kan uitzonderingen of een storing wanneer de andere versie is aangetroffen. Gegevens zijn mogelijk verloren als er bijvoorbeeld een nieuwe eigenschap is toegevoegd, maar de serializer van de oude verwijdert alle tijdens de deserialisatie.

Gegevenscontract is de aanbevolen oplossing om ervoor te zorgen dat uw gegevens compatibel is. Goed gedefinieerde versieregels voor toevoegen, verwijderen en wijzigen van velden heeft. Er wordt ook ondersteuning voor het omgaan met onbekende velden en omgaan met klassenovername vereist in het proces voor serialisatie en deserialisatie. Zie voor meer informatie [gegevenscontract met behulp van](https://msdn.microsoft.com/library/ms733127.aspx).

## <a name="next-steps"></a>Volgende stappen
[Een upgrade van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u bij een upgrade van de toepassing met Visual Studio.

[Een upgrade van uw toepassing met behulp van Powershell](service-fabric-application-upgrade-tutorial-powershell.md) begeleidt u bij een upgrade van de toepassing met behulp van PowerShell.

Bepalen hoe uw toepassing wordt bijgewerkt met behulp van [Parameters Upgrade](service-fabric-application-upgrade-parameters.md).

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Veelvoorkomende problemen oplossen in toepassingsupgrades door te verwijzen naar de stappen in [toepassingsupgrades probleemoplossing ](service-fabric-application-upgrade-troubleshooting.md).

