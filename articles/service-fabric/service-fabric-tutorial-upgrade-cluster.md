---
title: Upgrade van Azure Service Fabric-runtime | Microsoft Docs
description: Informatie over het upgraden van de runtime van een Azure gehoste Service Fabric-cluster met PowerShell.
services: service-fabric
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/28/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: faf134bc0952da913e90a93bc872a53f5f2369ff
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="upgrade-the-runtime-of-a-service-fabric-cluster"></a>Upgrade de runtime van een Service Fabric-cluster

Deze zelfstudie maakt deel uit drie van een reeks en ziet u hoe u een upgrade van de Service Fabric-runtime in een Azure Service Fabric-cluster. Deze zelfstudie is geschreven voor Service Fabric-clusters die zijn uitgevoerd op Azure en niet van toepassing op zelfstandige Service Fabric clusters.

> [!WARNING]
> Dit deel van de zelfstudie is PowerShell vereist. Ondersteuning voor het upgraden van de cluster-runtime is nog niet ondersteund door de Azure CLI-hulpprogramma's. U kunt ook kan een cluster worden bijgewerkt in de portal. Zie voor meer informatie [upgraden van een Azure Service Fabric-cluster](service-fabric-cluster-upgrade.md).

Als uw cluster wordt al uitgevoerd voor de meest recente Service Fabric-runtime, hoeft u geen Voer deze stap. In dit artikel kan echter worden gebruikt voor het installeren van een ondersteunde runtime in een Azure Service Fabric-cluster.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Lezen van de cluster-versie
> * De versie van het cluster instellen

In deze zelfstudie reeks leert u hoe:
> [!div class="checklist"]
> * Maken van een veilige [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) of [Linux-cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) op Azure met behulp van een sjabloon
> * [Een in- of -cluster schalen](service-fabric-tutorial-scale-cluster.md)
> * Upgrade de runtime van een cluster
> * [API Management met Service Fabric implementeren](service-fabric-tutorial-deploy-api-management.md)

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint:
- Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Installeer de [Azure Powershell-moduleversie 4.1 of hoger](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) of [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Maken van een veilige [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) of [Linux-cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) op Azure
- Als u een Windows-cluster implementeert, kunt u een Windows-ontwikkelomgeving instellen. Installeer [Visual Studio 2017](http://www.visualstudio.com) en de **ontwikkelen van Azure**, **ASP.NET en web ontwikkeling**, en **.NET Core platformoverschrijdende ontwikkeling**werkbelastingen.  Stel een [.NET ontwikkelomgeving](service-fabric-get-started.md).
- Als u een Linux-cluster implementeert, stelt u een Java-ontwikkelomgeving op [Linux](service-fabric-get-started-linux.md) of [Mac OS](service-fabric-get-started-mac.md).  Installeer de [Service Fabric CLI](service-fabric-cli.md). 

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Aanmelden bij uw Azure-account, selecteer uw abonnement voordat u Azure-opdrachten uitvoeren.

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="get-the-runtime-version"></a>Ophalen van de runtimeversie

Nadat u verbinding hebt gemaakt in Azure, geselecteerd het abonnement met het Service Fabric-cluster, kunt u de runtimeversie van het cluster krijgen.

```powershell
Get-AzureRmServiceFabricCluster -ResourceGroupName SFCLUSTERTUTORIALGROUP -Name aztestcluster `
    | Select-Object ClusterCodeVersion
```

Of alleen een lijst met alle clusters ophalen in uw abonnement met de volgende opties:

```powershell
Get-AzureRmServiceFabricCluster | Select-Object Name, ClusterCodeVersion
```

Opmerking de **ClusterCodeVersion** waarde. Deze waarde wordt gebruikt in de volgende sectie.

## <a name="upgrade-the-runtime"></a>Upgrade de runtime

Gebruik de waarde van **ClusterCodeVersion** uit de vorige sectie met de `Get-ServiceFabricRuntimeUpgradeVersion` cmdlet om te ontdekken welke versies zijn beschikbaar om te werken naar. Deze cmdlet kan alleen worden uitgevoerd vanaf een computer die is verbonden met internet. Bijvoorbeeld, als u wilt zien welke runtime-versies die u naar versie upgraden kunt `5.7.198.9494`, gebruik de volgende opdracht:

```powershell
Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion "5.7.198.9494"
```

Een lijst met versies ziet u de Azure Service Fabric-cluster om te werken naar een runtime die nieuwer is. Bijvoorbeeld, als versie `6.0.219.9494` beschikbaar is voor de upgrade naar de volgende opdracht gebruiken om te werken van uw cluster.

```powershell
Set-AzureRmServiceFabricUpgradeType -ResourceGroupName SFCLUSTERTUTORIALGROUP `
                                    -Name aztestcluster `
                                    -UpgradeMode Manual `
                                    -Version "6.0.219.9494"
```

> [!IMPORTANT]
> Het upgraden van cluster runtime kan lang duren om te voltooien. PowerShell wordt geblokkeerd terwijl de upgrade wordt uitgevoerd. U kunt een andere PowerShell-sessie gebruiken om de status van de upgrade te controleren.

De status van de upgrade kan worden bewaakt met beide PowerShell of de `sfctl` CLI.

De eerste verbinding met het cluster met de SSL-certificaat in het eerste deel van de zelfstudie hebt gemaakt. Gebruik de `Connect-ServiceFabricCluster` cmdlet of `sfctl cluster upgrade-status`.

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

Gebruik vervolgens `Get-ServiceFabricClusterUpgrade` of `sfctl cluster upgrade-status` om de status weer te geven. Iets soortgelijks als in het volgende resultaat wordt weergegeven.

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

## <a name="conclusion"></a>Conclusie
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * De versie van de cluster-runtime ophalen
> * Upgrade van de cluster-runtime
> * Monitor voor de upgrade

Ga vervolgens naar de volgende zelfstudie voor meer informatie over het implementeren van API Management met een Service Fabric-cluster.
> [!div class="nextstepaction"]
> [API Management met Service Fabric implementeren](service-fabric-tutorial-deploy-api-management.md)
