---
title: Geavanceerde toepassing Upgrade onderwerpen | Microsoft Docs
description: In dit artikel bevat informatie over paar geavanceerde onderwerpen over het upgraden van een Service Fabric-toepassing.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 3cdddac74552b56dfe3567adf30f1a05b6eb8e24
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663788"
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a>Service Fabric-toepassingsupgrade: geavanceerde onderwerpen
## <a name="adding-or-removing-service-types-during-an-application-upgrade"></a>Toevoegen of verwijderen van servicetypen tijdens een upgrade van de toepassing
Als een nieuw servicetype wordt toegevoegd aan een gepubliceerde toepassing als onderdeel van een upgrade, wordt het type van de nieuwe service toegevoegd aan de geïmplementeerde toepassing. Dergelijke een upgrade heeft geen invloed op een van de service-exemplaren die al deel van de toepassing uitmaken, maar een exemplaar van het type van de service die is toegevoegd voor het type van de nieuwe service actief moet worden gemaakt (Zie [New-ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice?view=azureservicefabricps)).

Servicetypen, kunnen ook vanuit een toepassing als onderdeel van een upgrade worden verwijderd. Echter, alle exemplaren van de service van het servicetype naar-worden verwijderd voordat u doorgaat met de upgrade moeten worden verwijderd (Zie [Remove-ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricservice?view=azureservicefabricps)).

## <a name="manual-upgrade-mode"></a>Handmatige upgrademodus
> [!NOTE]
> De *bewaakte* upgrademodus wordt aanbevolen voor alle Service Fabric-upgrades.
> De *UnmonitoredManual* upgrademodus moet alleen worden overwogen voor upgrades van de mislukte of onderbroken. 
>
>

In *bewaakte* Service Fabric-modus is van toepassing statusbeleid om ervoor te zorgen dat de toepassing in orde als de upgrade worden uitgevoerd is. Als statusbeleid zijn geschonden, wordt de upgrade is onderbroken of automatisch teruggedraaid, afhankelijk van de opgegeven *FailureAction*.

In *UnmonitoredManual* modus, de beheerder van de toepassing heeft volledige controle over de voortgang van de upgrade. In deze modus is handig bij het toepassen van beleid voor aangepaste status evaluatie of uitvoeren van niet-conventionele upgrades om over te slaan voor health monitoring volledig (bijvoorbeeld de toepassing is al in het verlies van gegevens). Een upgrade uitvoert in deze modus wordt de slaapstand na het voltooien van elke UD en expliciet moet hervat met behulp van [hervatten ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/resume-servicefabricapplicationupgrade?view=azureservicefabricps). Wanneer een upgrade is onderbroken en gereed is voor het door de gebruiker worden hervat, de upgradestatus wordt weergegeven *RollforwardPending* (Zie [UpgradeState](https://docs.microsoft.com/dotnet/api/system.fabric.applicationupgradestate?view=azure-dotnet)).

Ten slotte de *UnmonitoredAuto* modus is handig voor het uitvoeren van snelle upgrade iteraties tijdens service ontwikkelen of testen omdat er is geen gebruikersinvoer vereist is en er is geen toepassingsbeleid voor de status worden geëvalueerd.

## <a name="upgrade-with-a-diff-package"></a>Een upgrade uitvoert op een diff-pakket
In plaats van inrichtingspakket van een volledige toepassing kunnen ook upgrades worden uitgevoerd door in te richten diff-pakketten die alleen de bijgewerkte code/config/gegevens pakketten samen met de volledige toepassingsmanifest bevatten en servicemanifesten te voltooien. Volledige toepassingspakketten zijn alleen vereist voor de eerste installatie van een toepassing in het cluster. Volgende upgrades kunnen zijn van de volledige toepassingspakketten of diff-pakketten.  

Alle verwijzingen in het manifest van de toepassing of servicemanifesten van een diff-pakket dat niet kan worden gevonden in het toepassingspakket automatisch wordt vervangen door de momenteel ingerichte versie.

Er zijn scenario's voor het gebruik van een diff-pakket:

* Wanneer u een grote toepassingspakket dat verwijst naar verschillende service manifest-bestanden en/of verschillende pakketten, config pakketten of gegevenspakketten hebt.
* Wanneer u een implementatiesysteem dat de build-indeling rechtstreeks vanuit uw toepassing genereert bouwproces. Zelfs als de code nog niet is gewijzigd, krijg pasgebouwde assembly's in dit geval een andere controlesom. Met behulp van een pakket van de volledige toepassing, moet u de versie op voor alle pakketten bij te werken. Met een diff-pakket kunt bieden u alleen de bestanden die gewijzigd en de manifest-bestanden waarbij de versie is gewijzigd.

Wanneer een toepassing wordt bijgewerkt met behulp van Visual Studio, wordt een diff-pakket wordt automatisch gepubliceerd. Als u wilt een diff-pakket handmatig hebt gemaakt, het manifest van de toepassing en de servicemanifesten moeten worden bijgewerkt, maar alleen de gewijzigde pakketten moeten worden opgenomen in het laatste toepassingspakket.

Bijvoorbeeld, laten we beginnen met de volgende toepassing (versienummers opgegeven voor het gebruiksgemak understanding):

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Stel dat u wilt bijwerken alleen het codepakket van service1 met behulp van een diff-pakket. Uw bijgewerkte toepassing heeft wijzigingen in de volgende versie:

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

In dit geval bijwerken u het toepassingsmanifest 2.0.0 en het servicemanifest voor service1 in overeenstemming met de pakketupdate code. De map voor uw toepassingspakket zou hebben de volgende structuur:

```text
app1/
  service1/
    code/
```

Met andere woorden, een volledige toepassingspakket normaal maken en verwijder vervolgens de code/config/gegevenspakket mappen waarvoor de versie niet is gewijzigd.

## <a name="rolling-back-application-upgrades"></a>Het terugdraaien van een upgrades van toepassingen

Tijdens upgrades kunnen worden doorgevoerd in een van drie modi (*bewaakte*, *UnmonitoredAuto*, of *UnmonitoredManual*), ze kunnen alleen worden teruggedraaid in een van beide *UnmonitoredAuto* of *UnmonitoredManual* modus. Rolling terug in *UnmonitoredAuto* modus werkt op dezelfde manier als rolling doorsturen met de uitzondering die de standaardwaarde van *UpgradeReplicaSetCheckTimeout* verschilt - Zie [toepassing Upgradeparameters](service-fabric-application-upgrade-parameters.md). Rolling terug in *UnmonitoredManual* modus werkt op dezelfde manier als doorsturen rolling - het terugdraaien wordt opgeschort na het voltooien van elke UD en expliciet moet hervat met behulp van [ Resume-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/resume-servicefabricapplicationupgrade?view=azureservicefabricps) om door te gaan met het ongedaan maken.

Terugdraaiacties kunnen automatisch worden geactiveerd wanneer het statusbeleid van een upgrade in *bewaakte* modus met een *FailureAction* van *terugdraaien* zijn geschonden (Zie [Upgrade toepassingsparameters](service-fabric-application-upgrade-parameters.md)) of expliciet [Start ServiceFabricApplicationRollback](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationrollback?view=azureservicefabricps).

Tijdens het terugdraaien van de waarde van *UpgradeReplicaSetCheckTimeout* en de modus kan nog steeds worden gewijzigd op elk gewenst moment [Update ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/update-servicefabricapplicationupgrade?view=azureservicefabricps).

## <a name="next-steps"></a>Volgende stappen
[Een upgrade van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u bij de upgrade van een toepassing met Visual Studio.

[Een upgrade van uw toepassing met behulp van Powershell](service-fabric-application-upgrade-tutorial-powershell.md) begeleidt u bij de upgrade van een toepassing met behulp van PowerShell.

Bepalen hoe uw toepassing met behulp van upgrades [Parameters Upgrade](service-fabric-application-upgrade-parameters.md).

Upgrades van uw toepassingen compatibel maken door te leren hoe u [Gegevensserialisatie](service-fabric-application-upgrade-data-serialization.md).

Oplossen van veelvoorkomende problemen in upgrades van toepassingen door te verwijzen naar de stappen in [toepassingsupgrades oplossen van problemen](service-fabric-application-upgrade-troubleshooting.md).
