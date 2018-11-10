---
title: Een zelfstandig Azure Service Fabric-cluster op Windows Server upgraden | Microsoft Docs
description: De Azure Service Fabric-code en/of de configuratie die wordt uitgevoerd een zelfstandige Service Fabric-cluster, inclusief het instellen van de modus voor het bijwerken van cluster een upgrade uitvoert.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 2190978b2583b2f5d8a1b10431c65fd24fe6842d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228144"
---
# <a name="upgrade-your-standalone-azure-service-fabric-cluster-on-windows-server"></a>Uw zelfstandige Azure Service Fabric-cluster op Windows Server upgraden 
> [!div class="op_single_selector"]
> * [Azure-cluster](service-fabric-cluster-upgrade.md)
> * [Zelfstandige cluster](service-fabric-cluster-upgrade-windows-server.md)
>
>

Voor elk modern systeem is de mogelijkheid om te upgraden essentieel voor het succes op lange termijn van het product. Een Azure Service Fabric-cluster is een resource waarvan u eigenaar bent. Dit artikel wordt beschreven hoe u kunt ervoor zorgen dat het cluster altijd wordt uitgevoerd ondersteunde versies van Service Fabric-code en configuraties.

## <a name="control-the-service-fabric-version-that-runs-on-your-cluster"></a>De Service Fabric-versie die wordt uitgevoerd op uw cluster beheren
Om in te stellen uw cluster om updates te downloaden van Service Fabric wanneer een nieuwe versie door Microsoft worden uitgegeven, stelt u de clusterconfiguratie fabricClusterAutoupgradeEnabled naar *waar*. Als u wilt een ondersteunde versie van Service Fabric, waarmee u uw cluster op wilt selecteren, kunt u de configuratie van de cluster fabricClusterAutoupgradeEnabled instellen op *false*.

> [!NOTE]
> Zorg ervoor dat uw cluster altijd wordt uitgevoerd een ondersteunde versie van Service Fabric. Wanneer Microsoft de release van een nieuwe versie van Service Fabric kondigt, betekent dit dat de vorige versie zich na een minimum van 60 dagen vanaf de datum van de aankondiging is gemarkeerd voor einde van ondersteuning. Nieuwe releases worden aangekondigd [op de blog van het Service Fabric-team](https://blogs.msdn.microsoft.com/azureservicefabric/). De nieuwe versie is beschikbaar om te kiezen op dat moment.
>
>

U kunt uw cluster upgraden naar de nieuwe versie, alleen als u een productie-achtige knooppuntconfiguratie, waarbij elke Service Fabric-knooppunt op een afzonderlijke fysieke of virtuele machine is toegewezen. Hebt u een ontwikkelingscluster, waarbij meer dan één Service Fabric-knooppunt op een enkele fysieke of virtuele machine is, moet u het cluster met de nieuwe versie opnieuw maken.

Twee afzonderlijke werkstromen kunnen u uw cluster bijwerken naar de meest recente versie of een ondersteunde versie van de Service Fabric. Er is een werkstroom voor clusters die verbonden zijn met de nieuwste versie automatisch downloaden. De andere werkstroom is voor clusters die niet verbonden zijn met het downloaden van de meest recente versie van Service Fabric.

### <a name="upgrade-clusters-that-have-connectivity-to-download-the-latest-code-and-configuration"></a>Bijwerken van clusters die verbonden zijn met het downloaden van de meest recente code en configuratie
Volg deze stappen om uw cluster naar een ondersteunde versie upgraden als de clusterknooppunten over een internetverbinding beschikt op de [Microsoft Download Center](https://download.microsoft.com).

Voor clusters die verbonden met zijn de [Microsoft Download Center](https://download.microsoft.com), Microsoft controleert periodiek op de beschikbaarheid van de nieuwe Service Fabric-versies.

Wanneer een nieuwe Service Fabric-versie beschikbaar is, wordt het pakket gedownload naar het cluster en ingericht voor de upgrade. Daarnaast bevat het systeem om te informeren over de klant van deze nieuwe versie, een expliciete cluster status waarschuwing die vergelijkbaar is met het volgende:

"De huidige clusterversie [versie #] [datum] ondersteuning wordt beëindigd."

Nadat het cluster de meest recente versie wordt uitgevoerd, verdwijnt de waarschuwing.

#### <a name="cluster-upgrade-workflow"></a>Upgradewerkstroom voor cluster
Wanneer u het cluster: waarschuwing ziet, het volgende doen:

1. Verbinding maken met het cluster van elke computer die de beheerderstoegang heeft tot alle machines die worden weergegeven als knooppunten in het cluster. De machine die met dit script wordt uitgevoerd op beschikt niet over moet deel uitmaken van het cluster.

    ```powershell

    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3"
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. De lijst met Service Fabric-versies die u naar upgraden kunt ophalen.

    ```powershell

    ###### Get the list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    U moet een uitvoer die vergelijkbaar is met dit krijgen:

    ![Service Fabric-versies ophalen][getfabversions]
3. Start de clusterupgrade van een naar een beschikbare versie met behulp van de [Start ServiceFabricClusterUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterupgrade) Windows PowerShell-opdracht.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   U kunt voor het controleren van de voortgang van de upgrade, Service Fabric Explorer gebruiken of Voer de volgende PowerShell-opdracht:

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Als de cluster-statusbeleid niet worden voldaan, is de upgrade teruggedraaid. Zie de documentatie voor opgeven van aangepaste statusbeleid voor de opdracht Start-ServiceFabricClusterUpgrade [Start ServiceFabricClusterUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterupgrade).

    Nadat u de betreffende leidde tot het terugdraaien van de problemen hebt opgelost, start u de upgrade opnieuw met de volgende dezelfde stappen als eerder beschreven.

### <a name="upgrade-clusters-that-have-no-connectivity-to-download-the-latest-code-and-configuration"></a>Bijwerken van clusters met *afwezig* voor het downloaden van de meest recente code en configuratie
Volg deze stappen om uw cluster naar een ondersteunde versie upgraden als de clusterknooppunten geen internetverbinding met de [Microsoft Download Center](https://download.microsoft.com).

> [!NOTE]
> Als u een cluster dat is niet met internet verbonden uitvoert, hebt u voor het bewaken van de Service Fabric-team-blog voor meer informatie over nieuwe releases. Het systeem kan een cluster: waarschuwing u nieuwe releases wordt niet weergegeven.  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a>Automatische inrichting versus handmatige inrichting
Instellen om in te schakelen automatisch downloaden en de registratie voor de meest recente codeversie, de Service Fabric-Update-Service. Zie voor instructies Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt binnen de [zelfstandige pakket](service-fabric-cluster-standalone-package-contents.md).
Volg deze instructies voor het handmatige proces.

Wijzigen van de configuratie van het cluster de volgende eigenschap wilt instellen op *false* voordat u een upgrade van een configuratie:

        "fabricClusterAutoupgradeEnabled": false,

Zie voor informatie over het gebruik, de [Start ServiceFabricClusterConfigurationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade) PowerShell-opdracht. Zorg ervoor dat u 'clusterConfigurationVersion' in het JSON bijwerken voordat u de configuratie-upgrade start.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

#### <a name="cluster-upgrade-workflow"></a>Upgradewerkstroom voor cluster

1. Voer [Get-ServiceFabricClusterUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterupgrade) uit een van de knooppunten in het cluster en de opmerking de *TargetCodeVersion*.

2. Voer de volgende van een machine internetverbinding lijst met alle versies van de upgrade-compatibel is met de huidige versie en de bijbehorende downloaden via de downloadkoppelingen gekoppeld:

    ```powershell

    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. Verbinding maken met het cluster van elke computer die de beheerderstoegang heeft tot alle machines die worden weergegeven als knooppunten in het cluster. De machine die met dit script wordt uitgevoerd op beschikt niet over moet deel uitmaken van het cluster.

    ```powershell

   ###### Get the list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

   ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```
4. Kopieer het gedownloade pakket naar de installatiekopieopslag van het cluster.

5. Registreer de gekopieerde pakket.

    ```powershell

    ###### Get the list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
6. Start de clusterupgrade van een naar een beschikbare versie.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   U kunt de voortgang van de upgrade van Service Fabric Explorer of u kunt de volgende PowerShell-opdracht uitvoeren:

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Als de cluster-statusbeleid niet worden voldaan, is de upgrade teruggedraaid. Zie de documentatie voor opgeven van aangepaste statusbeleid voor de opdracht Start-ServiceFabricClusterUpgrade [Start ServiceFabricClusterUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterupgrade).

    Nadat u de betreffende leidde tot het terugdraaien van de problemen hebt opgelost, start u de upgrade opnieuw met de volgende dezelfde stappen als eerder beschreven.


## <a name="upgrade-the-cluster-configuration"></a>De configuratie van het cluster een upgrade uitvoert
Voordat u de upgrade van de configuratie hebt gestart, kunt u uw nieuwe clusterconfiguratie JSON testen door te voeren van de volgende PowerShell-script in het zelfstandige pakket:

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>

```
Of gebruik dit script:

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>

```

Sommige configuraties kunnen niet worden bijgewerkt, zoals eindpunten, de naam van het cluster, het IP-adres, enzovoort. Configuratie van het nieuwe cluster JSON is getest op basis van de oude heeft en genereert fouten in de PowerShell-venster als er een probleem is.

Uitvoeren als u de upgrade van het cluster-configuratie bijwerken, [Start ServiceFabricClusterConfigurationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade). De upgrade van de configuratie is verwerkt upgradedomein per upgradedomein.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

### <a name="cluster-certificate-config-upgrade"></a>Upgrade voor certificaat-configuratie  
Een clustercertificaat wordt gebruikt voor verificatie tussen de clusterknooppunten. De overschakeling van certificaat moet worden uitgevoerd met extra voorzichtig omdat de communicatie tussen knooppunten van het cluster wordt geblokkeerd als mislukt.

In technisch opzicht kunnen worden vier opties ondersteund:  

* Upgrade van één certificaat: het upgradepad is een (primair) -> certificaat B (primair)-certificaat -> certificaat C (primair) ->...

* Dubbele certificaat upgrade: de upgrade-pad is een (primair) -> certificaat het certificaat een (primair) en B (secundair) -> certificaat B (primair) > certificaat B (primair) en C (secundair) -> certificaat C (primair) ->...

* Type upgrade van het certificaat: op basis van de vingerafdruk van certificaat configuratie <>--CommonName op basis van certificaat-configuratie. Bijvoorbeeld, de vingerafdruk van het certificaat een (primair) en de vingerafdruk van-B (secundair) -> certificaat CommonName C.

* Upgrade van de verlener vingerafdruk van certificaat: het upgradepad wordt certificaat-CN = A, IssuerThumbprint IT1 = (primaire) certificaat-CN -> = A, IssuerThumbprint IT1, IT2 = (primaire) certificaat-CN -> = A, IssuerThumbprint IT2 = (primaire).


## <a name="next-steps"></a>Volgende stappen
* Informatie over het aanpassen van sommige [instellingen voor Service Fabric-cluster](service-fabric-cluster-fabric-settings.md).
* Meer informatie over het [in en uit het cluster wordt geschaald](service-fabric-cluster-scale-up-down.md).
* Meer informatie over [toepassingsupgrades](service-fabric-application-upgrade.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
