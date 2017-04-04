---
title: Azure Service Fabric-verschillen tussen Linux en Windows | Microsoft Docs
description: Verschillen tussen de preview-versie van Azure Service Fabric in Linux en Azure Service Fabric in Windows.
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
ms.date: 03/23/2017
ms.author: subramar
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 5faf7dc0b544f6fe2f83565cc368e218c6df35af
ms.lasthandoff: 03/28/2017


---
# <a name="differences-between-service-fabric-on-linux-preview-and-windows-generally-available"></a>Verschillen tussen Service Fabric in Linux (preview) en Service Fabric in Windows (algemeen beschikbaar)

Omdat Service Fabric in Linux een preview-versie is, zijn er enkele functies die wel worden ondersteund in Windows, maar niet in Linux. Wanneer Service Fabric in Linux algemeen beschikbaar komt, zullen de functiesets gelijkwaardig zijn.

* Betrouwbare verzamelingen (en betrouwbare stateful services) worden niet ondersteund op Linux.
* ReverseProxy is niet beschikbaar in Linux.
* Zelfstandig installatieprogramma is niet beschikbaar in Linux.
* In Linux wordt geen XML-schemavalidatie voor manifestbestanden uitgevoerd. 
* Console-omleiding wordt niet ondersteund in Linux. 
* De Fault Analysis Service (FAS) is niet beschikbaar in Linux.
* Azure Active Directory wordt niet ondersteund in Linux.
* Sommige CLI-opdrachtequivalenten van Powershell-opdrachten zijn niet beschikbaar.
* Voor een Linux-cluster kan maar een deel van de Powershell-opdrachten worden uitgevoerd (zoals in de volgende sectie wordt uitgelegd).

>[!NOTE]
>Console-omleiding wordt niet ondersteund in productieclusters, ook niet in Windows.

De hulpmiddelen voor ontwikkelaars verschillen. In Windows wordt gebruikgemaakt van VisualStudio, Powershell, VSTS en ETW, en in Linux van Yeoman, Eclipse, Jenkins en LTTng.

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
* Cmd
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
* [De Azure CLI gebruiken voor het beheren van uw Service Fabric-toepassingen](service-fabric-azure-cli.md)

