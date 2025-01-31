---
title: Azure Service Fabric-runtime upgraden | Microsoft Docs
description: In deze zelfstudie leert u hoe u PowerShell gebruikt om de runtime te upgraden van een Service Fabric-cluster dat wordt gehost in Azure.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/28/2017
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 8bb8a635c3699828376390c489697b6315030937
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306679"
---
# <a name="tutorial-upgrade-the-runtime-of-a-service-fabric-cluster-in-azure"></a>Zelfstudie: De runtime van een Service Fabric-cluster upgraden in Azure

Deze zelfstudie is deel vier van een reeks en ziet u hoe u de Service Fabric-runtime upgraden op een Azure Service Fabric-cluster. In dit deel van de zelfstudie is geschreven voor Service Fabric-clusters die worden uitgevoerd op Azure en niet van toepassing op zelfstandige Service Fabric clusters.

> [!WARNING]
> Voor dit deel van de zelfstudie is PowerShell vereist. De Azure CLI-hulpprogramma's bieden nog geen ondersteuning voor het upgraden van de clusterruntime. Een cluster kan ook worden geüpgraded in de portal. Zie [Een Azure Service Fabric-cluster upgraden](service-fabric-cluster-upgrade.md) voor meer informatie.

Als uw cluster wordt al uitgevoerd voor de meest recente Service Fabric-runtime, bevindt u zich hoeft te doen in deze stap. Dit artikel kan echter worden gebruikt om elke willekeurige ondersteunde runtime te installeren op een Azure Service Fabric-cluster.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De clusterversie lezen
> * De clusterversie instellen

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * Een beveiligd [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) maken in Azure met behulp van een sjabloon
> * [Een cluster bewaken](service-fabric-tutorial-monitor-cluster.md)
> * [Een cluster in- of uitschalen](service-fabric-tutorial-scale-cluster.md)
> * De runtime van een cluster upgraden
> * [Een cluster verwijderen](service-fabric-tutorial-delete-cluster.md)


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Installeer [Azure Powershell](https://docs.microsoft.com/powershell/azure/install-Az-ps) of [Azure CLI](/cli/azure/install-azure-cli).
* Een beveiligd [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) maken in Azure
* Stel een Windows-ontwikkelomgeving in. Installeer [Visual Studio 2019](https://www.visualstudio.com) en de **Azure-ontwikkeling**, **ASP.NET en webontwikkeling**, en **.NET Core platformoverschrijdende ontwikkeling**werkbelastingen.  Richt vervolgens een [.NET-ontwikkelomgeving in](service-fabric-get-started.md).

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-account en selecteer uw abonnement voordat u Azure-opdrachten gaat uitvoeren.

```powershell
Connect-AzAccount
Get-AzSubscription
Set-AzContext -SubscriptionId <guid>
```

## <a name="get-the-runtime-version"></a>De runtimeversie ophalen

Wanneer u verbinding hebt gemaakt naar Azure, het abonnement met de Service Fabric-cluster, krijgt u de runtimeversie van het cluster.

```powershell
Get-AzServiceFabricCluster -ResourceGroupName SFCLUSTERTUTORIALGROUP -Name aztestcluster `
    | Select-Object ClusterCodeVersion
```

Of haal alleen een lijst met alle clusters in uw abonnement met het volgende voorbeeld:

```powershell
Get-AzServiceFabricCluster | Select-Object Name, ClusterCodeVersion
```

Noteer de waarde voor **ClusterCodeVersion**. Deze waarde wordt gebruikt in de volgende sectie.

## <a name="upgrade-the-runtime"></a>De runtime upgraden

Gebruik de waarde voor **ClusterCodeVersion** uit de vorige sectie met de cmdlet `Get-ServiceFabricRuntimeUpgradeVersion`om te ontdekken naar welke versies u kunt upgraden. Deze cmdlet kan alleen worden uitgevoerd vanaf een computer die is verbonden met internet. Als u bijvoorbeeld wilt zien naar welke runtimeversies u kunt upgraden vanaf versie `5.7.198.9494`, gebruikt u de volgende opdracht:

```powershell
Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion "5.7.198.9494"
```

Met een lijst met versies kunt u het Azure Service Fabric-cluster de opdracht geven om te upgraden naar een nieuwere runtime. Als bijvoorbeeld versie `6.0.219.9494` beschikbaar is als upgrade, gebruikt u de volgende opdracht om het cluster te upgraden.

```powershell
Set-AzServiceFabricUpgradeType -ResourceGroupName SFCLUSTERTUTORIALGROUP `
                                    -Name aztestcluster `
                                    -UpgradeMode Manual `
                                    -Version "6.0.219.9494"
```

> [!IMPORTANT]
> Het kan lang duren voordat de upgrade voor de clusterruntime is voltooid. Tijdens het uitvoeren van de upgrade is PowerShell geblokkeerd. U kunt een andere PowerShell-sessie gebruiken om de status van de upgrade te controleren.

De status van de upgrade kan worden gecontroleerd met PowerShell of met de Azure Service Fabric CLI (sfctl).

Maak eerst verbinding met het cluster met het SSL-certificaat dat is gemaakt in het eerste deel van de zelfstudie. Gebruik de cmdlet `Connect-ServiceFabricCluster` of `sfctl cluster upgrade-status`.

```powershell
$endpoint = "<mycluster>.southcentralus.cloudapp.azure.com:19000"
$thumbprint = "63EB5BA4BC2A3BADC42CA6F93D6F45E5AD98A1E4"

Connect-ServiceFabricCluster -ConnectionEndpoint $endpoint `
                             -KeepAliveIntervalInSec 10 `
                             -X509Credential -ServerCertThumbprint $thumbprint `
                             -FindType FindByThumbprint -FindValue $thumbprint `
                             -StoreLocation CurrentUser -StoreName My
```

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Gebruik vervolgens `Get-ServiceFabricClusterUpgrade` of `sfctl cluster upgrade-status` om de status weer te geven. Het resultaat zier er ongeveer uit zoals hieronder wordt weergegeven.

```powershell
Get-ServiceFabricClusterUpgrade

TargetCodeVersion                          : 6.0.219.9494
TargetConfigVersion                        : 3
StartTimestampUtc                          : 11/28/2017 3:09:48 AM
UpgradeState                               : RollingForwardPending
UpgradeDuration                            : 00:09:00
CurrentUpgradeDomainDuration               : 00:09:00
NextUpgradeDomain                          : 1
UpgradeDomainsStatus                       : { "0" = "Completed";
                                             "1" = "Pending";
                                             "2" = "Pending";
                                             "3" = "Pending";
                                             "4" = "Pending" }
UpgradeKind                                : Rolling
RollingUpgradeMode                         : Monitored
FailureAction                              : Rollback
ForceRestart                               : False
UpgradeReplicaSetCheckTimeout              : 37201.09:59:01
HealthCheckWaitDuration                    : 00:05:00
HealthCheckStableDuration                  : 00:05:00
HealthCheckRetryTimeout                    : 00:45:00
UpgradeDomainTimeout                       : 02:00:00
UpgradeTimeout                             : 12:00:00
ConsiderWarningAsError                     : False
MaxPercentUnhealthyApplications            : 0
MaxPercentUnhealthyNodes                   : 100
ApplicationTypeHealthPolicyMap             : {}
EnableDeltaHealthEvaluation                : True
MaxPercentDeltaUnhealthyNodes              : 0
MaxPercentUpgradeDomainDeltaUnhealthyNodes : 0
ApplicationHealthPolicyMap                 : {}
```

```azurecli
sfctl cluster upgrade-status

{
  "codeVersion": "6.0.219.9494",
  "configVersion": "3",

... item cut to save space ...

  },
  "upgradeDomains": [
    {
      "name": "0",
      "state": "Completed"
    },
    {
      "name": "1",
      "state": "Pending"
    },
    {
      "name": "2",
      "state": "Pending"
    },
    {
      "name": "3",
      "state": "Pending"
    },
    {
      "name": "4",
      "state": "Pending"
    }
  ],
  "upgradeDurationInMilliseconds": "PT1H2M4.63889S",
  "upgradeState": "RollingForwardPending"
}
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * De versie van de clusterruntime ophalen
> * De clusterruntime upgraden
> * De upgrade controleren

Ga door naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Een cluster verwijderen](service-fabric-tutorial-delete-cluster.md)
