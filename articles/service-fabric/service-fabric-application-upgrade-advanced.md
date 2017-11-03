---
title: Geavanceerd toepassing Upgrade onderwerpen | Microsoft Docs
description: In dit artikel bevat informatie over sommige geavanceerde onderwerpen over het upgraden van een Service Fabric-toepassing.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar;chackdan
ms.openlocfilehash: 8d3b922f3d50b645ac9db2cc879a319df1262e0a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a>Upgrade van de service Fabric-toepassing: geavanceerde onderwerpen
## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Het toevoegen of verwijderen van services tijdens een upgrade van de toepassing
Als een nieuwe service wordt toegevoegd aan een toepassing die wordt al geïmplementeerd en gepubliceerd als een upgrade, wordt de nieuwe service toegevoegd aan de gedistribueerde toepassing.  Een dergelijke upgrade heeft geen invloed op een van de services die al deel van de toepassing uitmaakten. Een exemplaar van de service die is toegevoegd moet echter worden gestart voor de nieuwe service actief zijn (met behulp van de `New-ServiceFabricService` cmdlet).

Services kunnen ook worden verwijderd uit een toepassing als onderdeel van een upgrade. Echter alle huidige exemplaren van de service naar-worden verwijderd voordat u doorgaat met de upgrade moeten worden gestopt (met behulp van de `Remove-ServiceFabricService` cmdlet).

## <a name="manual-upgrade-mode"></a>Handmatige upgrademodus
> [!NOTE]
> De niet-bewaakte handmatige modus gelden alleen voor de upgrade van een mislukte of onderbroken. De bewaakte modus is de aanbevolen upgrademodus voor Service Fabric-toepassingen.
>
>

Azure Service Fabric bevat meerdere upgrade modi ter ondersteuning van ontwikkeling en productie-clusters. Implementatieopties gekozen kunnen afwijken voor verschillende omgevingen.

De bewaakte rolling upgrade van de toepassing is de meest voorkomende upgrade moet worden gebruikt in de productieomgeving. Wanneer het Upgradebeleid is opgegeven, Service Fabric zorgt ervoor dat de toepassing goed is voordat de upgrade wordt uitgevoerd.

 De beheerder van de toepassing kunt de handmatige rolling upgrade toepassingsmodus volledige controle over de voortgang van de upgrade via de verschillende upgradedomeinen hebt. Deze modus is handig wanneer een aangepaste of complexe evaluatie statusbeleid vereist is, of een upgrade van een ongebruikelijke activiteiten (bijvoorbeeld de toepassing is al in het verlies van gegevens).

Ten slotte is de geautomatiseerde rolling upgrade van de toepassing nuttig voor ontwikkeling of testomgevingen om een cyclus snelle herhaling tijdens het ontwikkelen van de service.

## <a name="change-to-manual-upgrade-mode"></a>Wijzig in de handmatige upgrademodus
**Handmatige**--stoppen van de upgrade van de toepassing op de huidige UD en de upgrademodus op niet-bewaakte handmatig wijzigen. De beheerder moet handmatig aanroepen **MoveNextApplicationUpgradeDomainAsync** te gaan met de upgrade terugdraaien activeren door een nieuwe upgrade initiëren. Zodra de upgrade in de handmatige modus invoert, blijft in de handmatige modus totdat een nieuwe upgrade wordt gestart. De **GetApplicationUpgradeProgressAsync** opdracht retourneert FABRIC\_toepassing\_UPGRADE\_status\_rollend\_doorsturen\_in behandeling.

## <a name="upgrade-with-a-diff-package"></a>Een upgrade uitvoert op een diff-pakket
Een Service Fabric-toepassing kan worden bijgewerkt door de inrichting met een volledige en zelfstandige toepassingspakket. Een toepassing kan ook worden bijgewerkt met behulp van een diff-pakket met de bijgewerkte toepassingsbestanden, het bijgewerkte toepassingsmanifest en de service manifest-bestanden.

Een volledige toepassingspakket bevat alle bestanden die nodig zijn om te starten en uitvoeren van een Service Fabric-toepassing. Een diff-pakket bevat alleen de bestanden die tussen de laatste inrichten en de huidige upgrade gewijzigd, plus manifest voor de volledige toepassing en de service manifest bestanden. Elke vermelding in het manifest van de toepassing of servicemanifest dat niet is gevonden in de indeling van de build wordt gezocht in de image store.

Volledige toepassingspakketten zijn vereist voor de eerste installatie van een toepassing in het cluster. Daaropvolgende updates kunnen zijn voor een volledige toepassingspakket of een diff-pakket.

Gevallen terwijl een goede keuze met behulp van een pakket diff zou zijn:

* Een diff-pakket verdient de voorkeur wanneer u een groot pakket die verwijst naar verschillende service manifest-bestanden en/of verschillende pakketten code, config pakketten of gegevenspakketten hebt.
* Een diff-pakket verdient de voorkeur wanneer u een implementatiesysteem dat de indeling van de build rechtstreeks vanuit uw buildproces toepassing genereert. Hoewel de code nog niet is gewijzigd, ophalen nieuw gebouwde assembly's in dit geval een andere controlesom. Met behulp van een volledige toepassingspakket, moet u de versie op alle code pakketten bijwerken. Met een diff-pakket bieden u alleen de bestanden die gewijzigd en de manifest-bestanden waarbij de versie is gewijzigd.

Wanneer een toepassing wordt bijgewerkt met behulp van Visual Studio, wordt het diff-pakket wordt automatisch gepubliceerd. Een pakket diff als handmatig wilt maken, het toepassingsmanifest en de service-manifesten moeten worden bijgewerkt, maar alleen de gewijzigde pakketten moeten worden opgenomen in het laatste toepassingspakket.

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

Nu gaan we wordt ervan uitgegaan dat u wilt bijwerken, alleen het codepakket van service1 met een diff-pakket met behulp van PowerShell. Uw bijgewerkte toepassing heeft nu de volgende mapstructuur:

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

## <a name="next-steps"></a>Volgende stappen
[Een upgrade van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u bij een upgrade van de toepassing met Visual Studio.

[Een upgrade van uw toepassing met behulp van Powershell](service-fabric-application-upgrade-tutorial-powershell.md) begeleidt u bij een upgrade van de toepassing met behulp van PowerShell.

Bepalen hoe uw toepassing wordt bijgewerkt met behulp van [Parameters Upgrade](service-fabric-application-upgrade-parameters.md).

Uw toepassingsupgrades compatibel maken door te leren hoe u [serialisatie van gegevens](service-fabric-application-upgrade-data-serialization.md).

Veelvoorkomende problemen oplossen in toepassingsupgrades door te verwijzen naar de stappen in [toepassingsupgrades probleemoplossing](service-fabric-application-upgrade-troubleshooting.md).
