---
title: De upgrade van de service Fabric-toepassing | Microsoft Docs
description: In dit artikel bevat een inleiding tot het upgraden van een Service Fabric-toepassing, met inbegrip van kiezen upgrade modi en het uitvoeren van statuscontroles.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: 803c9c63-373a-4d6a-8ef2-ea97e16e88dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 79408c9936000aa18dba9347b8a10fa7dcd8e8ee
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35759555"
---
# <a name="service-fabric-application-upgrade"></a>Upgrade van Service Fabric-toepassing uitvoeren
Een Azure Service Fabric-toepassing is een verzameling van services. Tijdens een upgrade, Service Fabric vergelijkt de nieuwe [toepassingsmanifest](service-fabric-application-and-service-manifests.md) met de vorige versie en bepaalt welke services in de updates voor het vereisen van toepassing. Service Fabric vergelijkt de versie van getallen in de service zich voordoet wanneer de versienummers in de vorige versie. Als een service is niet gewijzigd, wordt deze service is niet bijgewerkt.

## <a name="rolling-upgrades-overview"></a>Rolling upgrades-overzicht
In een rolling upgrade van de toepassing, wordt de upgrade in fasen uitgevoerd. De upgrade wordt in elk stadium toegepast op een subset van de knooppunten in het cluster, met de naam een updatedomein. Als gevolg hiervan blijft de toepassing beschikbaar zijn tijdens de upgrade. Tijdens de upgrade kan het cluster een combinatie van de oude en nieuwe versies bevatten.

Om die reden, de twee versies moeten vooruit en achteruit compatibel is. Als ze niet compatibel zijn, is de beheerder van de toepassing is verantwoordelijk voor het Faseren van de upgrade van een meervoudige fasen voor het onderhouden van beschikbaarheid. Bij een upgrade van een meervoudige-fase, wordt de eerste stap bijgewerkt naar een versie van de toepassing die compatibel is met de vorige versie. De tweede stap is het de laatste versie van de compatibiliteit met de versie van vóór het bijwerken, maar is compatibel met de tussenliggende versie bijwerken.

Update-domeinen zijn opgegeven in het clustermanifest bij het configureren van het cluster. Update-domeinen ontvangen geen updates in een bepaalde volgorde. Een updatedomein is een logische eenheid van de implementatie voor een toepassing. Update-domeinen kunnen de services te blijven in hoge mate van beschikbaarheid tijdens een upgrade.

Niet-rolling upgrades zijn mogelijk als de upgrade wordt toegepast op alle knooppunten in het cluster, het geval is wanneer de toepassing slechts één updatedomein heeft. Deze methode wordt niet aanbevolen, omdat de service uitvalt en is niet beschikbaar op het moment van de upgrade. Bovendien biedt Azure geen garanties wanneer een cluster met slechts één updatedomein is ingesteld.

Nadat de upgrade is voltooid, alle services en replicas(instances) in dezelfde versie-dat wil zeggen, blijven zou als de upgrade is geslaagd, wordt deze bijgewerkt naar de nieuwe versie; Als de upgrade mislukt en wordt teruggedraaid, ze zouden worden teruggedraaid naar de oude versie.

## <a name="health-checks-during-upgrades"></a>Statuscontroles tijdens upgrades
Voor een upgrade statusbeleid moeten worden ingesteld (of standaardwaarden kunnen worden gebruikt). Een upgrade wordt aangeduid als geslaagde wanneer alle updatedomeinen zijn bijgewerkt binnen de opgegeven time-outs, en wanneer alle updatedomeinen als in orde worden beschouwd.  Een updatedomein in orde betekent dat het updatedomein alle statuscontroles die zijn opgegeven in het beleid van de status geslaagd. Bijvoorbeeld, een statusbeleid dat alle services in een toepassingsexemplaar moeten zijn mogelijk verplichten *in orde*, zoals de status wordt gedefinieerd door Service Fabric.

Zijn de service en toepassing neutraal statusbeleid en controles tijdens de upgrade van Service Fabric. Dat wil zeggen, worden er zijn geen service-specifieke tests uitgevoerd.  Bijvoorbeeld, wordt uw service kan een vereiste doorvoer hebt, maar Service Fabric heeft geen informatie om te controleren of de doorvoer. Raadpleeg de [health artikelen](service-fabric-health-introduction.md) voor de controles die worden uitgevoerd. De controles die zich voordoen tijdens een upgrade opnemen prestatietests voor of het toepassingspakket correct is gekopieerd of het exemplaar is gestart, enzovoort.

De status van de toepassing is een samenvoeging van de onderliggende entiteiten van de toepassing. Kort gezegd, evalueert de Service Fabric de status van de toepassing via de status die wordt gerapporteerd over de toepassing. Deze ook evalueert de status van de services voor de toepassing op deze manier. Service Fabric verder evalueert de status van de toepassingsservices door samenvoeging van de status van hun kinderen, zoals de service-replica. Zodra het statusbeleid voor de toepassing is voldaan, wordt de upgrade kan worden voortgezet. Als het statusbeleid wordt geschonden, mislukt de upgrade van de toepassing.

## <a name="upgrade-modes"></a>Upgrade-modi
De modus die wij voor de upgrade van toepassing aanraden is de bewaakte modus, de meest gebruikte modus is. Bewaakte modus de upgrade uitvoert op een updatedomein en als alle statuscontroles pass (op basis van het beleid opgegeven) gaat verder met het volgende updatedomein automatisch.  Als u statuscontroles mislukken en/of time-outs zijn bereikt, de upgrade is ofwel teruggedraaid voor de updatedomein of de modus is gewijzigd in niet-bewaakte handmatig. U kunt de upgrade als u een van deze twee modi voor upgrades van de mislukte wilt configureren. 

Niet-bewaakte handmatige modus moet handmatige interventie na elke upgrade voor een updatedomein, een vliegende start de upgrade op het volgende updatedomein. Er is geen statuscontroles van Service Fabric worden uitgevoerd. De beheerder voert de status of de status controles vóór de upgrade in het volgende updatedomein wordt gestart.

## <a name="upgrade-default-services"></a>Standaard-services upgraden
Sommige standaardparameters van de service gedefinieerd in de [toepassingsmanifest](service-fabric-application-and-service-manifests.md) kan ook worden bijgewerkt als onderdeel van een upgrade van de toepassing. De serviceparameters die ondersteuning bieden voor wordt gewijzigd via [Update ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/update-servicefabricservice?view=azureservicefabricps) als onderdeel van een upgrade kan worden gewijzigd. Het gedrag van het standaard-services wijzigen tijdens de upgrade van de toepassing is als volgt:

1. Services die standaard in het manifest van de nieuwe toepassing die niet al bestaan in het cluster worden gemaakt.
2. Standaard-services die zijn opgenomen in de vorige en nieuwe Toepassingsmanifesten worden bijgewerkt. De parameters van de standaard-service in het manifest van de nieuwe toepassing overschrijven de parameters van de bestaande service. Upgrade van de toepassing wordt automatisch ongedaan maken als het bijwerken van een standaardservice is mislukt.
3. Standaardservices die niet zijn opgenomen in het manifest van de nieuwe toepassing worden verwijderd als deze bestaan in het cluster. **Houd er rekening mee dat het verwijderen van een standaardservice in het verwijderen van alle service resulteren zal de status en kan niet ongedaan worden gemaakt.**

Wanneer u een upgrade van de toepassing wordt teruggedraaid, worden de standaardparameters van de service voordat de upgrade is gestart maar verwijderde services kunnen niet opnieuw worden gemaakt met de oude status terug naar de oude waarden hersteld.

> [!TIP]
> De [EnableDefaultServicesUpgrade](service-fabric-cluster-fabric-settings.md) cluster configuratie-instelling moet *waar* om in te schakelen regels 2) en 3) hierboven (standaard service bijwerken en verwijderen). Deze functie wordt ondersteund vanaf in Service Fabric versie 5.5.

## <a name="upgrading-multiple-applications-with-https-endpoints"></a>Upgrade van meerdere toepassingen met HTTPS-eindpunten
Moet u ervoor dat u geen gebruik van de **dezelfde poort** voor verschillende exemplaren van dezelfde toepassing bij het gebruik van HTTP**S**. De reden is dat Service Fabric niet mogelijk om te upgraden van het certificaat voor een van de exemplaren van de toepassing. Bijvoorbeeld, als toepassing 1 of toepassing 2 beide wilt upgraden van hun certificaat 1 naar 2 certificaat. Als de upgrade gebeurt, kan Service Fabric hebben opgeschoond de certificaat-1-registratie bij http.sys zelfs als deze nog steeds door de andere toepassing gebruikt wordt. Om dit te voorkomen, detecteert Service Fabric dat er al een ander exemplaar van de toepassing geregistreerd op de poort met het certificaat (als gevolg van http.sys), en de bewerking is mislukt.

Daarom Service Fabric biedt geen ondersteuning voor een upgrade van twee verschillende services met behulp van **dezelfde poort** in instanties van een andere toepassing. U niet met andere woorden, hetzelfde certificaat gebruiken op verschillende services op dezelfde poort. Als u een gedeelde-certificaat hebben op dezelfde poort wilt, moet u om ervoor te zorgen dat de services op verschillende computers met plaatsingsbeperkingen zijn geplaatst. Of Overweeg het gebruik van Service Fabric dynamische poorten indien mogelijk voor elke service in elk exemplaar. 

Als er een upgrade is mislukt met https, een foutbericht verschijnt waarin wordt gemeld "De Windows HTTP-Server API ondersteunt geen meerdere certificaten voor toepassingen die een poort delen."

## <a name="application-upgrade-flowchart"></a>Toepassing bijwerken stroomdiagram
De stroomdiagram hieronder krijgt u inzicht in het upgradeproces van een Service Fabric-toepassing. In het bijzonder de stroom wordt beschreven hoe de time-outs, met inbegrip van *HealthCheckStableDuration*, *HealthCheckRetryTimeout*, en *UpgradeHealthCheckInterval*, help besturingselement wanneer de upgrade in één updatedomein wordt beschouwd als een geslaagd of mislukt.

![Het upgradeproces voor een Service Fabric-toepassing][image]

## <a name="next-steps"></a>Volgende stappen
[Een upgrade van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u bij de upgrade van een toepassing met Visual Studio.

[Een upgrade van uw toepassing met behulp van PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) begeleidt u bij de upgrade van een toepassing met behulp van PowerShell.

Bepalen hoe uw toepassing met behulp van upgrades [Parameters Upgrade](service-fabric-application-upgrade-parameters.md).

Upgrades van uw toepassingen compatibel maken door te leren hoe u [Gegevensserialisatie](service-fabric-application-upgrade-data-serialization.md).

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Oplossen van veelvoorkomende problemen in upgrades van toepassingen door te verwijzen naar de stappen in [toepassingsupgrades oplossen van problemen](service-fabric-application-upgrade-troubleshooting.md).

[image]: media/service-fabric-application-upgrade/service-fabric-application-upgrade-flowchart.png
