---
title: Azure Service Fabric-verschillen tussen Linux en Windows | Microsoft Docs
description: Verschillen tussen Azure Service Fabric in Linux en Azure Service Fabric in Windows.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/19/2017
ms.author: subramar
ms.openlocfilehash: 25976ba919454e26f1dd7965de5db7c4f80b9355
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="differences-between-service-fabric-on-linux-and-windows"></a>Verschillen tussen Service Fabric in Linux en Windows

Er zijn enkele functies die wel worden ondersteund in Windows, maar nog niet in Linux. Uiteindelijk zullen de functiesets gelijk zijn en met elke versie wordt het functiegat kleiner. De nieuwste beschikbare versies (tussen versie 6.0 in Windows en versie 6.0 in Linux) hebben de volgende verschillen: 

* Alle programmeermodellen zijn in preview (Java/C# Reliable Actors, Reliable Stateless Services en Reliable Stateful Services)
* Envoy (ReverseProxy) is in preview in Linux
* Het zelfstandige installatieprogramma voor Linux is nog niet beschikbaar in Linux
* Console-omleiding (niet ondersteund in Linux- en Windows-productieclusters)
* De FAS (Fault Analysis Service) in Linux
* DNS-service voor Service Fabric-services (DNS-service wordt ondersteund voor containers in Linux)
* Equivalenten van CLI-opdrachten voor bepaalde Powershell-opdrachten (zie de onderstaande lijst, waarvan de meeste opdrachten alleen van toepassing zijn op zelfstandige clusters)

De hulpmiddelen voor ontwikkelaars verschillen ook tussen Windows en Linux. Visual Studio, Powershell VSTS en ETW worden gebruikt in Windows. Yeoman, Eclipse, Jenkins en LTTng worden gebruikt in Linux.

## <a name="powershell-cmdlets-that-do-not-work-against-a-linux-service-fabric-cluster"></a>PowerShell-cmdlets die niet werken voor een Service Fabric-cluster in Linux

* Invoke-ServiceFabricChaosTestScenario
* Invoke-ServiceFabricFailoverTestScenario
* Invoke-ServiceFabricPartitionDataLoss
* Invoke-ServiceFabricPartitionQuorumLoss
* Restart-ServiceFabricPartition
* Start-ServiceFabricNode
* Stop-ServiceFabricNode
* Get-ServiceFabricImageStoreContent
* Get-ServiceFabricChaosReport
* Get-ServiceFabricNodeTransitionProgress
* Get-ServiceFabricPartitionDataLossProgress
* Get-ServiceFabricPartitionQuorumLossProgress
* Get-ServiceFabricPartitionRestartProgress
* Get-ServiceFabricTestCommandStatusList
* Remove-ServiceFabricTestState
* Start-ServiceFabricChaos
* Start-ServiceFabricNodeTransition
* Start-ServiceFabricPartitionDataLoss
* Start-ServiceFabricPartitionQuorumLoss
* Start-ServiceFabricPartitionRestart
* Stop-ServiceFabricChaos
* Stop-ServiceFabricTestCommand
* Get-ServiceFabricNodeConfiguration
* Get-ServiceFabricClusterConfiguration
* Get-ServiceFabricClusterConfigurationUpgradeStatus
* Get-ServiceFabricPackageDebugParameters
* New-ServiceFabricPackageDebugParameter
* New-ServiceFabricPackageSharingPolicy
* Add-ServiceFabricNode
* Copy-ServiceFabricClusterPackage
* Get-ServiceFabricRuntimeSupportedVersion
* Get-ServiceFabricRuntimeUpgradeVersion
* New-ServiceFabricCluster
* New-ServiceFabricNodeConfiguration
* Remove-ServiceFabricCluster
* Remove-ServiceFabricClusterPackage
* Remove-ServiceFabricNodeConfiguration
* Test-ServiceFabricClusterManifest
* Test-ServiceFabricConfiguration
* Update-ServiceFabricNodeConfiguration
* Approve-ServiceFabricRepairTask
* Complete-ServiceFabricRepairTask
* Get-ServiceFabricRepairTask
* Invoke-ServiceFabricDecryptText
* Invoke-ServiceFabricEncryptSecret
* Invoke-ServiceFabricEncryptText
* Invoke-ServiceFabricInfrastructureCommand
* Invoke-ServiceFabricInfrastructureQuery
* Remove-ServiceFabricRepairTask
* Start-ServiceFabricRepairTask
* Stop-ServiceFabricRepairTask
* Update-ServiceFabricRepairTaskHealthPolicy



## <a name="next-steps"></a>Volgende stappen
* [Uw ontwikkelomgeving voorbereiden in Linux](service-fabric-get-started-linux.md)
* [Uw ontwikkelomgeving voorbereiden in OSX](service-fabric-get-started-mac.md)
* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Uw eerste Service Fabric Java-toepassing in Linux maken en implementeren met behulp van de Service Fabric-invoegtoepassing voor Eclipse](service-fabric-get-started-eclipse.md)
* [Uw eerste CSharp-toepassing in Linux maken](service-fabric-create-your-first-linux-application-with-csharp.md)
* [De Service Fabric-CLI gebruiken voor het beheren van uw toepassingen](service-fabric-application-lifecycle-sfctl.md)
