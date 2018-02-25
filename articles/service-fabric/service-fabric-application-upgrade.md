---
title: Upgrade van de service Fabric-toepassing | Microsoft Docs
description: Dit artikel bevat een inleiding tot het upgraden van een Service Fabric-toepassing, waaronder kiezen upgrade modi en het uitvoeren van statuscontroles.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 803c9c63-373a-4d6a-8ef2-ea97e16e88dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 765931d8a888432e0cc77ff86d597b6e2a029a2a
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2018
---
# <a name="service-fabric-application-upgrade"></a>Upgrade van Service Fabric-toepassing uitvoeren
Een Azure Service Fabric-toepassing is een verzameling van services. Tijdens een upgrade Service Fabric vergelijkt de nieuwe [toepassingsmanifest](service-fabric-application-and-service-manifests.md) met de vorige versie en bepaalt welke services in de toepassing-updates vereisen. Service Fabric vergelijkt de getallen in de service zich voordoet wanneer het versienummer in de vorige versie versie. Als een service niet is gewijzigd, wordt deze service is niet bijgewerkt.

## <a name="rolling-upgrades-overview"></a>Rolling upgrades-overzicht
De upgrade wordt in een uitrollende upgrade van de toepassing in verschillende fasen uitgevoerd. De upgrade wordt in elk stadium toegepast op een subset van de knooppunten in het cluster, een updatedomein genoemd. Hierdoor blijft de toepassing beschikbaar zijn tijdens de upgrade. Tijdens de upgrade kan het cluster een combinatie van de oude en nieuwe versies bevatten.

Daarom moet de twee versies moet voorwaarts en achterwaarts compatibel. Als ze niet compatibel is, is de beheerder van de toepassing verantwoordelijk voor de upgrade van een meervoudige fasen voor het onderhouden van beschikbaarheid voor fasering. De eerste stap in de upgrade van een meervoudige fasen, is van een upgrade naar een tussenliggende versie van de toepassing die compatibel is met de vorige versie. De tweede stap bestaat uit de definitieve versie die compatibel is met de updateversie die vooraf wordt verbroken, maar is compatibel met de versie van de tussenliggende bijwerken.

Update domeinen zijn opgegeven in het clustermanifest bij het configureren van het cluster. Update domeinen ontvangt geen updates in een bepaalde volgorde. Een updatedomein is een logische eenheid van de implementatie voor een toepassing. Update-domeinen kunnen de services voor hoge beschikbaarheid behouden tijdens een upgrade.

Upgrades niet rolling zijn mogelijk als de upgrade voor alle knooppunten in het cluster, het geval is wanneer de toepassing slechts één updatedomein heeft wordt toegepast. Deze methode wordt niet aanbevolen, omdat de service uitgeschakeld wordt en is niet beschikbaar op het moment van de upgrade. Azure biedt bovendien garanties wanneer een cluster met slechts één updatedomein is ingesteld.

Nadat de upgrade is voltooid, alle services en replicas(instances) in dezelfde versie-dat wil zeggen, blijven zou als de upgrade is geslaagd, worden ze bijgewerkt naar de nieuwe versie; Als de upgrade mislukt en wordt teruggedraaid, ze zouden worden teruggezet naar de oude versie.

## <a name="health-checks-during-upgrades"></a>Statuscontroles tijdens upgrades
Voor een upgrade statusbeleid moeten worden ingesteld (of standaardwaarden kunnen worden gebruikt). Een upgrade wordt aangeduid als geslaagd wanneer alle domeinen van de update zijn bijgewerkt binnen de opgegeven time-outs en wanneer alle domeinen van de update als in orde worden beschouwd.  Een updatedomein in orde betekent dat het updatedomein alle statuscontroles die zijn opgegeven in het statusbeleid doorgegeven. Bijvoorbeeld, een statusbeleid dat alle services in een toepassingsexemplaar moet mogelijk verplichten *orde*, zoals status wordt gedefinieerd door de Service Fabric.

Zijn de service en toepassing networkdirect statusbeleid en controles tijdens de upgrade door de Service Fabric. Dat wil zeggen, zijn geen servicespecifieke tests uitgevoerd.  Bijvoorbeeld: de service kan een vereiste doorvoer hebben, maar Service Fabric heeft niet de informatie om te controleren van de doorvoer. Raadpleeg de [health artikelen](service-fabric-health-introduction.md) voor de controles die worden uitgevoerd. De controles die zich voordoen tijdens een upgrade include-tests voor of het toepassingspakket correct is gekopieerd of het exemplaar is gestart, enzovoort.

De toepassingsstatus is een samenvoeging van de onderliggende entiteiten van de toepassing. Service Fabric evalueert Kortom, de status van de toepassing via de status die is gerapporteerd voor de toepassing. Het ook onderzoekt de status van de services voor de toepassing op deze manier. Service Fabric verdere evalueert de status van de toepassingsservices die door het samenvoegen van de status van onderliggende items, zoals de replica van de service. Zodra het statusbeleid voor de toepassing wordt voldaan, kan de upgrade kan worden voortgezet. Als het statusbeleid wordt geschonden, mislukt de upgrade van de toepassing.

## <a name="upgrade-modes"></a>Upgrade-modi
De modus die wij voor de upgrade van de toepassing aanraden is de bewaakte modus, de meest gebruikte modus is. Bewaakte modus voert de upgrade op één updatedomein en als alle health controleert op te geven (op basis van het beleid opgegeven) verplaatst op de volgende update domein automatisch.  Als statuscontroles mislukken en/of time-outs zijn bereikt, de upgrade wordt ofwel teruggedraaid voor de updatedomein of de modus is gewijzigd in niet-bewaakte handmatig. U kunt de upgrade Kies een van deze twee modi voor upgrades van de mislukte configureren. 

Niet-bewaakte handmatige modus moet handmatige interventie na elke upgrade op een updatedomein, ere van de upgrade op de volgende updatedomein. Er is geen health Service Fabric controles worden uitgevoerd. De beheerder voert de status of health controles voordat u de upgrade start in de volgende updatedomein.

## <a name="upgrade-default-services"></a>Standaard-services upgraden
Sommige standaard service gedefinieerde parameters in de [toepassingsmanifest](service-fabric-application-and-service-manifests.md) kan ook worden bijgewerkt als onderdeel van een upgrade van de toepassing. De serviceparameters die ondersteuning bieden voor wordt gewijzigd via [Update ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/update-servicefabricservice?view=azureservicefabricps) als onderdeel van een upgrade kan worden gewijzigd. Het gedrag van het standaard-services wijzigen tijdens de upgrade van de toepassing is als volgt:

1. Standaard-services in het manifest van de nieuwe toepassing die niet al bestaan in het cluster worden gemaakt.
2. Standaard-services die zijn opgenomen in de vorige en nieuwe Toepassingsmanifesten worden bijgewerkt. De parameters van de standaardservice in het manifest van de nieuwe toepassing overschrijven de parameters van de bestaande service. De upgrade van de toepassing wordt automatisch terugdraaien als een standaardservice bijwerken mislukt.
3. Standaardservices die niet zijn opgenomen in het manifest van de nieuwe toepassing worden verwijderd als deze bestaan in het cluster. **Opmerking dat het verwijderen van een standaardservice in het verwijderen van alle service resulteren zal de status en kan niet ongedaan worden gemaakt.**

Wanneer een upgrade van de toepassing wordt teruggedraaid, worden de standaardparameters voor service teruggezet naar hun oude waarden voordat de upgrade gestart maar verwijderde services kan niet opnieuw gemaakt met hun oude status worden.

> [!TIP]
> De [EnableDefaultServicesUpgrade](service-fabric-cluster-fabric-settings.md) cluster configuratie-instelling moet *true* regels 2 inschakelen) en 3) hierboven (standaard service-update en verwijderen). Deze functie wordt ondersteund in Service Fabric versie 5.5 wordt gestart.

## <a name="application-upgrade-flowchart"></a>Toepassing bijwerken stroomdiagram
Het stroomdiagram hieronder vindt u informatie over het upgradeproces van een Service Fabric-toepassing. In het bijzonder de stroom wordt beschreven hoe de time-outs, met inbegrip van *HealthCheckStableDuration*, *HealthCheckRetryTimeout*, en *UpgradeHealthCheckInterval*, help bepalen wanneer de upgrade in één updatedomein wordt beschouwd als een geslaagd of mislukt.

![Het upgradeproces voor een Service Fabric-toepassing][image]

## <a name="next-steps"></a>Volgende stappen
[Een upgrade van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u bij een upgrade van de toepassing met Visual Studio.

[Een upgrade van uw toepassing met behulp van Powershell](service-fabric-application-upgrade-tutorial-powershell.md) begeleidt u bij een upgrade van de toepassing met behulp van PowerShell.

Bepalen hoe uw toepassing wordt bijgewerkt met behulp van [Parameters Upgrade](service-fabric-application-upgrade-parameters.md).

Uw toepassingsupgrades compatibel maken door te leren hoe u [serialisatie van gegevens](service-fabric-application-upgrade-data-serialization.md).

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Veelvoorkomende problemen oplossen in toepassingsupgrades door te verwijzen naar de stappen in [toepassingsupgrades probleemoplossing](service-fabric-application-upgrade-troubleshooting.md).

[image]: media/service-fabric-application-upgrade/service-fabric-application-upgrade-flowchart.png
