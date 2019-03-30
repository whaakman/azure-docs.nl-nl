---
title: De configuratie van een zelfstandig Azure Service Fabric-cluster upgraden | Microsoft Docs
description: Informatie over het bijwerken van de configuratie die wordt uitgevoerd een zelfstandige Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: f99c1ebb64bf881bcd42f15e13bb81b96ccfa064
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58665599"
---
# <a name="upgrade-the-configuration-of-a-standalone-cluster"></a>De configuratie van een zelfstandige cluster upgraden 

Voor elk modern systeem is de mogelijkheid om te upgraden essentieel voor het succes op lange termijn van het product. Een Azure Service Fabric-cluster is een resource waarvan u eigenaar bent. In dit artikel wordt beschreven hoe u de configuratie-instellingen van uw zelfstandige Service Fabric-cluster een upgrade uitvoert.

## <a name="customize-cluster-settings-in-the-clusterconfigjson-file"></a>Clusterinstellingen in het bestand ClusterConfig.json aanpassen
Zelfstandige clusters worden geconfigureerd via de *ClusterConfig.json* bestand. Zie voor meer informatie over de verschillende instellingen, [configuratie-instellingen voor een zelfstandige Windows-cluster](service-fabric-cluster-manifest.md).

U kunt toevoegen, bijwerken of verwijderen van de instellingen in de `fabricSettings` sectie onder de [eigenschappen van het Cluster](./service-fabric-cluster-manifest.md#cluster-properties) in sectie *ClusterConfig.json*. 

Bijvoorbeeld, een nieuwe instelling wordt toegevoegd in de volgende JSON *MaxDiskQuotaInMB* naar de *Diagnostics* sectie onder `fabricSettings`:

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

Nadat u de instellingen hebt gewijzigd in het bestand ClusterConfig.json [testen van de clusterconfiguratie](#test-the-cluster-configuration) en vervolgens [upgrade van de clusterconfiguratie](#upgrade-the-cluster-configuration) de instellingen toepassen op uw cluster. 

## <a name="test-the-cluster-configuration"></a>Test de configuratie van het cluster
Voordat u de upgrade van de configuratie hebt gestart, kunt u uw nieuwe clusterconfiguratie JSON testen door te voeren van de volgende PowerShell-script in het zelfstandige pakket:

```powershell
TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>
```

Of gebruik dit script:

```powershell
TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>
```

Sommige configuraties kunnen niet worden bijgewerkt, zoals eindpunten, de naam van het cluster, het IP-adres, enzovoort. Configuratie van het nieuwe cluster JSON is getest op basis van de oude heeft en genereert fouten in de PowerShell-venster als er een probleem is.

## <a name="upgrade-the-cluster-configuration"></a>De configuratie van het cluster een upgrade uitvoert
Uitvoeren als u de upgrade van het cluster-configuratie bijwerken, [Start ServiceFabricClusterConfigurationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade). De upgrade van de configuratie is verwerkt upgradedomein per upgradedomein.

```powershell
Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
```

## <a name="upgrade-cluster-certificate-configuration"></a>Configuratie van het cluster een upgrade uitvoert
Een clustercertificaat wordt gebruikt voor verificatie tussen de clusterknooppunten. De overschakeling van certificaat moet worden uitgevoerd met extra voorzichtig omdat de communicatie tussen knooppunten van het cluster wordt geblokkeerd als mislukt.

Vier opties worden ondersteund:  

* Upgrade van één certificaat: De upgrade-pad is een (primair) -> certificaat B (primair)-certificaat -> certificaat C (primair) ->...

* Dubbele certificaat bijwerken: De upgrade-pad is een (primair) -> certificaat het certificaat een (primair) en B (secundair) -> certificaat B (primair) > certificaat B (primair) en C (secundair) -> certificaat C (primair) ->...

* Certificaat type upgrade: Op basis van de vingerafdruk van certificaat configuratie <>--CommonName op basis van certificaat-configuratie. Bijvoorbeeld, de vingerafdruk van het certificaat een (primair) en de vingerafdruk van-B (secundair) -> certificaat CommonName C.

* Upgrade van de verlener vingerafdruk van certificaat: Het upgradepad wordt certificaat-CN = A, IssuerThumbprint IT1 = (primaire) -> certificaat-CN = A, IssuerThumbprint IT1, IT2 = (primaire) certificaat-CN -> = A, IssuerThumbprint IT2 = (primaire).


## <a name="next-steps"></a>Volgende stappen
* Informatie over het aanpassen van sommige [instellingen voor Service Fabric-cluster](service-fabric-cluster-fabric-settings.md).
* Meer informatie over het [in en uit het cluster wordt geschaald](service-fabric-cluster-scale-up-down.md).
* Meer informatie over [toepassingsupgrades](service-fabric-application-upgrade.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
