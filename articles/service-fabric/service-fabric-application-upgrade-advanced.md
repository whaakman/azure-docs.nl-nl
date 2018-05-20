---
title: Geavanceerd toepassing Upgrade onderwerpen | Microsoft Docs
description: In dit artikel bevat informatie over sommige geavanceerde onderwerpen over het upgraden van een Service Fabric-toepassing.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 168d944f72c1409b5b69c9ab7c07f7fcfa04c7c8
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a>Upgrade van de service Fabric-toepassing: geavanceerde onderwerpen
## <a name="adding-or-removing-service-types-during-an-application-upgrade"></a>Het toevoegen of verwijderen van servicetypen tijdens een upgrade van de toepassing
Als een nieuwe servicetype is toegevoegd aan een gepubliceerde toepassing als onderdeel van een upgrade, vervolgens het type van de nieuwe service toegevoegd aan de gedistribueerde toepassing. Een dergelijke upgrade heeft geen invloed op een van de service-exemplaren die al deel van de toepassing uitmaakten, maar een exemplaar van het type van de service die is toegevoegd, moet worden gemaakt voor het type van de nieuwe service actief zijn (Zie [New-ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice?view=azureservicefabricps)).

Op deze manier kunnen servicetypen worden verwijderd uit een toepassing als onderdeel van een upgrade. Echter, alle exemplaren van de service van het servicetype naar-worden verwijderd voordat u doorgaat met de upgrade moeten worden verwijderd (Zie [verwijderen ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricservice?view=azureservicefabricps)).

## <a name="manual-upgrade-mode"></a>Handmatige upgrademodus
> [!NOTE]
> De *bewaakte* upgrademodus wordt aanbevolen voor alle Service Fabric-upgrades.
> De *UnmonitoredManual* upgrademodus moet alleen worden beschouwd voor upgrades van de mislukte of onderbroken. 
>
>

In *bewaakte* Service Fabric-modus is van toepassing statusbeleid om ervoor te zorgen dat de toepassing tijdens de voortgang van de upgrade in orde is. Als statusbeleid zijn geschonden, wordt de upgrade is onderbroken of is automatisch teruggedraaid, afhankelijk van de opgegeven *FailureAction*.

In *UnmonitoredManual* modus kan de beheerder van de toepassing heeft volledige controle over de voortgang van de upgrade. Deze modus is nuttig wanneer aangepaste evaluatie van het statusbeleid toe te passen of uitvoeren van niet-conventionele upgrades om over te slaan voor health monitoring volledig (bijvoorbeeld de toepassing is al in het verlies van gegevens). Een upgrade die in deze modus wordt opgeschort na het voltooien van elke UD en expliciet moet hervat met behulp van [hervatten ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/resume-servicefabricapplicationupgrade?view=azureservicefabricps). Wanneer een upgrade is onderbroken en gereed is voor het door de gebruiker worden hervat, de upgradestatus weergeven *RollforwardPending* (Zie [UpgradeState](https://docs.microsoft.com/dotnet/api/system.fabric.applicationupgradestate?view=azure-dotnet)).

Ten slotte de *UnmonitoredAuto* modus is nuttig voor het uitvoeren van snel upgrade iteraties tijdens serviceontwikkeling of tests, omdat er geen gebruikersinvoer is vereist en er is geen toepassingsbeleid health worden geëvalueerd.

## <a name="upgrade-with-a-diff-package"></a>Een upgrade uitvoert op een diff-pakket
In plaats van een volledige toepassing inrichtingspakket worden upgrades ook uitgevoerd door provisioning diff-pakketten die bevatten alleen de bijgewerkte configuratie-code-gegevens pakketten samen met het manifest van de volledige toepassing en service manifesten te voltooien. Volledige toepassingspakketten zijn alleen vereist voor de initiële installatie van een toepassing in het cluster. Toekomstige upgrades kunnen zijn van de volledige toepassingspakketten of diff-pakketten.  

Elke vermelding in het manifest van de toepassing of service manifesten van een diff-pakket dat niet is gevonden in het toepassingspakket automatisch wordt vervangen door de versie die momenteel wordt ingericht.

Scenario's voor het gebruik van een pakket diff zijn:

* Wanneer u een groot pakket hebt dat verwijst naar verschillende service manifest-bestanden en/of verschillende pakketten code, config pakketten of gegevenspakketten.
* Wanneer u een implementatiesysteem dat de indeling van de build rechtstreeks vanuit uw toepassing genereert buildproces. Hoewel de code nog niet is gewijzigd, ophalen nieuw gebouwde assembly's in dit geval een andere controlesom. Met behulp van een volledige toepassingspakket, moet u de versie op alle code pakketten bijwerken. Met een diff-pakket bieden u alleen de bestanden die gewijzigd en de manifest-bestanden waarbij de versie is gewijzigd.

Wanneer een toepassing wordt bijgewerkt met behulp van Visual Studio, wordt een diff-pakket wordt automatisch gepubliceerd. Een pakket diff als handmatig wilt maken, het toepassingsmanifest en de service-manifesten moeten worden bijgewerkt, maar alleen de gewijzigde pakketten moeten worden opgenomen in het laatste toepassingspakket.

Bijvoorbeeld, laten we beginnen met de volgende toepassing (versienummers opgegeven voor het gemak van wat):

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Stel de gewenste updatepakket alleen de code van service1 met een diff-pakket. Uw bijgewerkte toepassing heeft wijzigingen in de volgende versie:

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

In dit geval werkt u het toepassingsmanifest naar 2.0.0 en de servicemanifest voor service1 in overeenstemming met de code-pakketupdate. De map voor het toepassingspakket moet de volgende structuur:

```text
app1/
  service1/
    code/
```

Met andere woorden, een volledige toepassingspakket normaal maken en verwijder vervolgens de config-code-gegevens pakketmappen waarvoor de versie niet is gewijzigd.

## <a name="rolling-back-application-upgrades"></a>Toepassingsupgrades terugdraaien

Tijdens upgrades kunnen worden doorgevoerd in een van drie beschikbare modi (*bewaakte*, *UnmonitoredAuto*, of *UnmonitoredManual*), ze kunnen alleen worden teruggezet in beide *UnmonitoredAuto* of *UnmonitoredManual* modus. Rolling terug in de *UnmonitoredAuto* modus werkt op dezelfde manier als rolling doorsturen met de uitzondering die de standaardwaarde van *UpgradeReplicaSetCheckTimeout* verschilt - Zie [toepassing Upgrade van de Parameters](service-fabric-application-upgrade-parameters.md). Rolling terug in de *UnmonitoredManual* modus werkt op dezelfde manier als doorsturen rolling - de terugdraaiactie wordt opgeschort na het voltooien van elke UD en expliciet moet hervat met behulp van [ Resume-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/resume-servicefabricapplicationupgrade?view=azureservicefabricps) om door te gaan met het terugdraaien.

Rollback kunnen automatisch worden geactiveerd wanneer het statusbeleid van een upgrade in *bewaakte* modus met een *FailureAction* van *terugdraaien* zijn geschonden (Zie [Upgrade toepassingsparameters](service-fabric-application-upgrade-parameters.md)) of expliciet gebruik [Start ServiceFabricApplicationRollback](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationrollback?view=azureservicefabricps).

Tijdens het terugdraaien is de waarde van *UpgradeReplicaSetCheckTimeout* en de modus kan nog worden gewijzigd op elke keer die [Update ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/update-servicefabricapplicationupgrade?view=azureservicefabricps).

## <a name="next-steps"></a>Volgende stappen
[Een upgrade van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u bij een upgrade van de toepassing met Visual Studio.

[Een upgrade van uw toepassing met behulp van Powershell](service-fabric-application-upgrade-tutorial-powershell.md) begeleidt u bij een upgrade van de toepassing met behulp van PowerShell.

Bepalen hoe uw toepassing wordt bijgewerkt met behulp van [Parameters Upgrade](service-fabric-application-upgrade-parameters.md).

Uw toepassingsupgrades compatibel maken door te leren hoe u [serialisatie van gegevens](service-fabric-application-upgrade-data-serialization.md).

Veelvoorkomende problemen oplossen in toepassingsupgrades door te verwijzen naar de stappen in [toepassingsupgrades probleemoplossing](service-fabric-application-upgrade-troubleshooting.md).
