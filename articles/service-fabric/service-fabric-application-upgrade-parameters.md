---
title: 'Toepassingsupgrade: Upgradeparameters | Microsoft Docs'
description: Hierin wordt beschreven parameters met betrekking tot het upgraden van een Service Fabric-toepassing, met inbegrip van statuscontroles uit te voeren en het beleid om de upgrade automatisch ongedaan te maken.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: a4170ac6-192e-44a8-b93d-7e39c92a347e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/17/2018
ms.author: subramar
ms.openlocfilehash: f3f381fddee9c1830202854f02556f73b5aeed23
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47055574"
---
# <a name="application-upgrade-parameters"></a>Parameters toepassingsupgrade
Dit artikel beschrijft de verschillende parameters die van toepassing tijdens de upgrade van een Azure Service Fabric-toepassing zijn. Parameters toepassingsupgrade bepaalt de time-outs en statuscontroles die worden toegepast tijdens de upgrade en u deze opgeven met de beleidsregels die moeten worden toegepast wanneer een upgrade mislukt.

Parameters voor de toepassing van toepassing op upgrades met behulp van PowerShell of Visual Studio. De vereiste en optionele parameters van toepassing op PowerShell en/of Visual Studio worden beschreven in de tabellen vereiste Parameters en optionele Parameters als volgt.

Upgrades van toepassingen worden gestart via een van drie modi voor de gebruiker instelbare upgrade. Beide modi heeft een eigen set parameters voor de toepassing:
- Gecontroleerd
- Niet-bewaakte automatisch
- Niet-bewaakte handmatig

Upgrades van de service Fabric-toepassingen met behulp van PowerShell via de [Start ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade) opdracht. De upgrademodus is geselecteerd door ofwel de **bewaakte**, **UnmonitoredAuto**, of **UnmonitoredManual** parameter [ Start-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade).

Visual Studio Service Fabric application upgrade parameters zijn ingesteld via het dialoogvenster Visual Studio Upgrade-instellingen. De upgrademodus voor Visual Studio is ingeschakeld met behulp van de **Upgrade modus** vervolgkeuzelijst tot **bewaakte**, **UnmonitoredAuto**, of **UnmonitoredManual** . Zie voor meer informatie, [configureren van de upgrade van een Service Fabric-toepassing in Visual Studio](service-fabric-visualstudio-configure-upgrade.md).

## <a name="required-parameters"></a>Vereiste Parameters
(PS = PowerShell, VS = Visual Studio)

| Parameter | Van toepassing op | Beschrijving |
| --- | --- | --- |
ApplicationName |PS| De naam van de toepassing die wordt bijgewerkt. Voorbeelden: fabric: / VisualObjects, fabric: / ClusterMonitor. |
ApplicationTypeVersion|PS|Typt u de versie van de toepassing die de upgrade doelen. |
FailureAction |PS, VS|Toegestane waarden zijn **ongeldig**, **terugdraaien**, en **handmatig**. De actie op die door Service Fabric bij de upgrade mislukt. De toepassing kan worden teruggedraaid naar de versie van vóór het bijwerken (Rollback) of de upgrade op het huidige upgradedomein kan worden gestopt. In dat laatste geval, de upgrademodus ook wordt gewijzigd naar **handmatig**.|
Gecontroleerd |PS|Geeft aan dat de upgrademodus wordt bewaakt. Nadat de cmdlet is een upgrade voor een upgradedomein voltooid als de status van het upgradedomein en de cluster voldoen aan de health-beleidsregels die u definieert, wordt het volgende upgradedomein bijgewerkt in Service Fabric. Als de upgradedomein of het cluster is mislukt om te voldoen aan statusbeleid, mislukt de upgrade en Service Fabric de upgrade voor het upgradedomein teruggedraaid of ingesteld op handmatige modus per het beleid dat is opgegeven. Dit is de aanbevolen modus voor upgrades van toepassingen in een productieomgeving. |
UpgradeMode | VISUAL STUDIO | Toegestane waarden zijn **bewaakte** (standaard), **UnmonitoredAuto**, of **UnmonitoredManual**. Zie de PowerShell-parameters voor beide modi in dit artikel voor meer informatie. |
UnmonitoredAuto | PS | Geeft aan dat de upgrademodus voor niet-bewaakte automatisch. Nadat de Service Fabric een upgradedomein wordt bijgewerkt, wordt het volgende upgradedomein, ongeacht de status van de toepassing in Service Fabric bijgewerkt. In deze modus wordt niet aanbevolen voor productie, en is alleen nuttig tijdens de ontwikkeling van een toepassing. |
UnmonitoredManual | PS | Geeft aan dat de upgrademodus voor niet-bewaakte handmatig. Nadat de Service Fabric voert u een upgrade van een upgradedomein, wacht u het volgende upgradedomein bijwerken met behulp van de *hervatten ServiceFabricApplicationUpgrade* cmdlet. |

## <a name="optional-parameters"></a>Optionele Parameters

De status evaluatie-parameters zijn optioneel. Als de status evaluatie van criteria zijn opgegeven wanneer een upgrade wordt gestart, wordt in Service Fabric de toepassing statusbeleid opgegeven in de ApplicationManifest.xml van exemplaar van de toepassing gebruikt.

Gebruik de horizontale schuifbalk aan de onderkant van de tabel om het veld volledige beschrijving weer te geven.

(PS = PowerShell, VS = Visual Studio)

| Parameter | Van toepassing op | Beschrijving |
| --- | --- | --- |
| ApplicationParameter |PS, VS| Hiermee geeft u de onderdrukkingen voor de parameters voor de toepassing.<br>PowerShell applcation-parameters worden opgegeven als hashtabel naam/waarde-paren. Bijvoorbeeld: @{"VotingData_MinReplicaSetSize" = "3"; "VotingData_PartitionCount" = "1"}.<br>Parameters voor Visual Studio de toepassing kunnen worden opgegeven in het dialoogvenster voor Service Fabric-toepassing publiceren in de **Toepassingsparametersbestand** veld.
| Bevestigen |PS| Toegestane waarden zijn **waar** en **False**. Als u wordt gevraagd om bevestiging voordat u de cmdlet uitvoert. |
| ConsiderWarningAsError |PS, VS |Toegestane waarden zijn **waar** en **False**. De standaardwaarde is **False**. De health-gebeurtenissen van de waarschuwing voor de toepassing behandelen als fouten bij het evalueren van de status van de toepassing tijdens de upgrade. Service Fabric beoordeelt standaard niet statusgebeurtenissen van de waarschuwing om fouten (fouten), zodat de upgrade kan worden voortgezet zelfs als er waarschuwingsgebeurtenissen. |
| DefaultServiceTypeHealthPolicy | PS, VS |Hiermee geeft u het statusbeleid voor het type van de standaard-service moet worden gebruikt voor de bewaakte upgrade in de indeling MaxPercentUnhealthyPartitionsPerService, MaxPercentUnhealthyReplicasPerPartition, MaxPercentUnhealthyServices. 5,10,15 geeft bijvoorbeeld de volgende waarden: MaxPercentUnhealthyPartitionsPerService = 5, MaxPercentUnhealthyReplicasPerPartition = 10, MaxPercentUnhealthyServices = 15. |
| Force | PS, VS | Toegestane waarden zijn **waar** en **False**. Geeft aan dat het upgradeproces het waarschuwingsbericht staan aangegeven slaat en zorgt ervoor dat de upgrade, zelfs wanneer het versienummer niet gewijzigd. Dit is handig voor het lokale testen, maar wordt niet aanbevolen voor gebruik in een productieomgeving omdat hiervoor verwijderen van de bestaande implementatie van dit leidt ertoe dat de uitval en potentieel gegevensverlies. |
| ForceRestart |PS, VS |Als u een configuratie of een gegevenspakket bijwerken zonder de code bijwerken, de service wordt opnieuw gestart alleen als de eigenschap ForceRestart is ingesteld op **waar**. Wanneer de update voltooid is, meldt Service Fabric de service dat een nieuwe configuratie of gegevenspakket beschikbaar is. De service is verantwoordelijk voor het toepassen van de wijzigingen. Indien nodig, kan de service zelf opnieuw. |
| HealthCheckRetryTimeoutSec |PS, VS |De duur (in seconden) doorgaat die Service Fabric met het uitvoeren van evalueren voordat u de upgrade declareren als mislukt. De standaardwaarde is 600 seconden. Deze duur wordt gestart nadat de *HealthCheckWaitDurationSec* is bereikt. In deze *HealthCheckRetryTimeout*, Service Fabric kan meerdere statuscontroles van de status van de toepassing uitvoeren. De standaardwaarde is 10 minuten en moet op de juiste wijze worden aangepast voor uw toepassing. |
| HealthCheckStableDurationSec |PS, VS |De duur (in seconden) om te controleren of de toepassing stabiel voordat u verplaatsen naar het volgende upgradedomein of de upgrade te voltooien. Deze wachttijd wordt gebruikt om te voorkomen dat ongedetecteerde wijzigingen van de status direct nadat de statuscontrole is uitgevoerd. De standaardwaarde is 120 seconden en moet op de juiste wijze worden aangepast aan uw toepassing. |
| HealthCheckWaitDurationSec |PS, VS | Moet worden gewacht (in seconden) nadat de upgrade is voltooid in het upgradedomein voor Service Fabric evalueert de status van de toepassing. Deze duur kan ook worden beschouwd als de tijd die een toepassing moet worden uitgevoerd voordat deze kan worden beschouwd als in orde. Als de statuscontrole is geslaagd, wordt het upgradeproces gaat deze naar het volgende upgradedomein.  Als de controle van gatewayservicestatus is mislukt, Service Fabric wacht voor [UpgradeHealthCheckInterval](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-fabric-settings#clustermanager) voordat opnieuw wordt geprobeerd de status controleren pas weer de *HealthCheckRetryTimeoutSec* is bereikt. De standaard- en aanbevolen waarde is 0 seconden. |
| MaxPercentUnhealthyDeployedApplications|PS, VS |Standaard- en aanbevolen waarde is 0. Geef het maximum aantal geïmplementeerde toepassingen (Zie de [Health sectie](service-fabric-health-introduction.md)) die kunnen worden niet in orde voordat de toepassing als slecht beschouwd en de upgrade is mislukt. Deze parameter bepaalt de status van de toepassing op het knooppunt en kunt u problemen detecteren tijdens de upgrade. Normaal gesproken ophalen de replica's van de toepassing Netwerktaakverdeling naar het andere knooppunt, waardoor de toepassing wordt weergegeven in orde, zodat de upgrade om door te gaan. Door op te geven een strikte *MaxPercentUnhealthyDeployedApplications* status van de Service Fabric kan een probleem met het toepassingspakket snel te detecteren en te produceren van een snelle upgrade mislukken. |
| MaxPercentUnhealthyServices |PS, VS |Een parameter voor *DefaultServiceTypeHealthPolicy* en *ServiceTypeHealthPolicyMap*. Standaard- en aanbevolen waarde is 0. Geef het maximum aantal services in een exemplaar van de toepassing die niet in orde zijn mag voordat de toepassing als slecht beschouwd en de upgrade is mislukt. |
| MaxPercentUnhealthyPartitionsPerService|PS, VS |Standaard- en aanbevolen waarde is 0. Geef het maximum aantal partities in een service die niet in orde zijn mag voordat de service wordt als slecht beschouwd. |
| MaxPercentUnhealthyReplicasPerPartition|PS, VS |Standaard- en aanbevolen waarde is 0. Geef het maximum aantal replica's in de partitie die niet in orde zijn mag voordat de partitie als slecht beschouwd. |
| ServiceTypeHealthPolicyMap | PS, VS | Hiermee geeft u het statusbeleid dat wordt gebruikt voor het evalueren van de status van services die behoren tot een servicetype. Neemt de invoer van een hash-tabel in de volgende notatie: @ {"ServiceTypeName": "MaxPercentUnhealthyPartitionsPerService, MaxPercentUnhealthyReplicasPerPartition, MaxPercentUnhealthyServices"} bijvoorbeeld: @{"ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5"} |
| TimeoutSec | PS, VS | Hiermee geeft u de time-outperiode in seconden voor het opnieuw. |
| UpgradeDomainTimeoutSec |PS, VS |Maximale tijd (in seconden) voor het upgraden van een één upgradedomein. Als de time-outwaarde is bereikt, de upgrade gestopt en wordt voortgezet op basis van de instelling voor *FailureAction*. De standaardwaarde is nooit (oneindig) en moet op de juiste wijze worden aangepast voor uw toepassing. |
| UpgradeReplicaSetCheckTimeoutSec |PS, VS |**Stateless service**--binnen één upgradedomein, Service Fabric probeert om ervoor te zorgen dat er extra exemplaren van de service beschikbaar zijn. Als de exemplaren van het doel meer dan één is, wordt Service Fabric wacht gedurende meer dan één exemplaar beschikbaar, tot een maximale time-outwaarde. De time-outwaarde is opgegeven met behulp van de *UpgradeReplicaSetCheckTimeoutSec* eigenschap. Als de time-outperiode is verstreken, gaat de Service Fabric met de upgrade, ongeacht het aantal exemplaren van de service. Als de doel-exemplaren een is Service Fabric niet wacht en onmiddellijk wordt voortgezet met de upgrade.<br><br>**Stateful service**--binnen één upgradedomein, Service Fabric probeert om ervoor te zorgen dat de replicaset een quorum heeft. Service Fabric wacht voor een quorum beschikbaar, tot een maximale time-outwaarde (opgegeven door de *UpgradeReplicaSetCheckTimeoutSec* eigenschap). Als de time-outperiode is verlopen, wordt de Service Fabric met de upgrade, ongeacht het quorum voortgezet. Deze instelling is ingesteld als nooit (oneindig) bij het implementeren van doorsturen en 1200 seconden wanneer terugdraaien. |
| UpgradeTimeoutSec |PS, VS |Er is een time-out (in seconden) die van toepassing voor de volledige upgrade is. Als de time-outwaarde is bereikt, wordt de upgrade gestopt en *FailureAction* wordt geactiveerd. De standaardwaarde is nooit (oneindig) en moet op de juiste wijze worden aangepast voor uw toepassing. |
| WhatIf | PS | Toegestane waarden zijn **waar** en **False**. Geeft aan wat er gebeuren zou als de cmdlet wordt uitgevoerd. De cmdlet wordt niet uitgevoerd. |

De *MaxPercentUnhealthyServices*, *MaxPercentUnhealthyPartitionsPerService*, en *MaxPercentUnhealthyReplicasPerPartition* criteria per kunnen worden opgegeven het type van de service voor het exemplaar van een toepassing. Deze parameters per-service in te stellen kunt voor een toepassing naar verschillende services bevatten met verschillende evaluatie-beleid. Bijvoorbeeld, een stateless service Gatewaytype kan hebben een *MaxPercentUnhealthyPartitionsPerService* verschillend is van een servicetype stateful-engine voor het exemplaar van een bepaalde toepassing.

## <a name="next-steps"></a>Volgende stappen
[Een upgrade van uw toepassing met behulp van Visual Studio](service-fabric-application-upgrade-tutorial.md) begeleidt u bij de upgrade van een toepassing met Visual Studio.

[Een upgrade van uw toepassing met behulp van Powershell](service-fabric-application-upgrade-tutorial-powershell.md) begeleidt u bij de upgrade van een toepassing met behulp van PowerShell.

[Bijwerken van uw toepassing met behulp van Service Fabric-CLI op Linux](service-fabric-application-lifecycle-sfctl.md#upgrade-application) begeleidt u bij de upgrade van een toepassing met behulp van Service Fabric-CLI.

[Bijwerken van uw toepassing met behulp van Service Fabric Eclipse-invoegtoepassing](service-fabric-get-started-eclipse.md#upgrade-your-service-fabric-java-application)

Upgrades van uw toepassingen compatibel maken door te leren hoe u [Gegevensserialisatie](service-fabric-application-upgrade-data-serialization.md).

Informatie over het gebruik van geavanceerde functionaliteit tijdens het bijwerken van uw toepassing door te verwijzen naar [geavanceerde onderwerpen](service-fabric-application-upgrade-advanced.md).

Oplossen van veelvoorkomende problemen in upgrades van toepassingen door te verwijzen naar de stappen in [toepassingsupgrades oplossen van problemen](service-fabric-application-upgrade-troubleshooting.md).
