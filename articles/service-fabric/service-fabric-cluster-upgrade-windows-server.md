---
title: Upgraden van een zelfstandige Azure Service Fabric-cluster op Windows Server | Microsoft Docs
description: Upgrade de Azure Service Fabric-code en/of de configuratie die wordt uitgevoerd een zelfstandige Service Fabric-cluster, inclusief het instellen van de cluster-bijwerkmodus.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: c95c1827d0433dcb61eace34e7a905a5610c7781
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="upgrade-your-standalone-azure-service-fabric-cluster-on-windows-server"></a>Upgrade van uw zelfstandige Azure Service Fabric-cluster op Windows Server 
> [!div class="op_single_selector"]
> * [Azure-cluster](service-fabric-cluster-upgrade.md)
> * [Zelfstandige cluster](service-fabric-cluster-upgrade-windows-server.md)
>
>

Voor elk moderne systeem is de mogelijkheid om bij te werken sleutel voor het succes op lange termijn van uw product. Een Azure Service Fabric-cluster is een resource waarvan u eigenaar. Dit artikel wordt beschreven hoe u ervoor kunt zorgen dat het cluster altijd wordt uitgevoerd ondersteunde versies van Service Fabric-code en configuraties.

## <a name="control-the-service-fabric-version-that-runs-on-your-cluster"></a>De Service Fabric-versie die wordt uitgevoerd op uw cluster beheren
Om in te stellen uw cluster om updates te downloaden van Service Fabric wanneer Microsoft een nieuwe versie is vrijgegeven, zodanig dat de clusterconfiguratie fabricClusterAutoupgradeEnabled *true*. Selecteer een ondersteunde versie van Service Fabric die u wilt dat uw cluster op, stel de fabricClusterAutoupgradeEnabled cluster op *false*.

> [!NOTE]
> Zorg ervoor dat uw cluster altijd wordt uitgevoerd een ondersteunde versie van Service Fabric. Als Microsoft de release van een nieuwe versie van Service Fabric introduceert, wordt de vorige versie gemarkeerd voor einde van ondersteuning na een minimum van 60 dagen na de datum van de aankondiging. Nieuwe releases worden aangekondigd [op het Service Fabric-teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/). De nieuwe versie is beschikbaar op dat moment kiezen.
>
>

Alleen als u een productie-stijl knooppuntconfiguratie, waarbij elk Service Fabric-knooppunt is toegewezen aan een afzonderlijke fysieke of virtuele machine, kunt u uw cluster bijwerken naar de nieuwe versie. Als u een ontwikkelcluster waarbij meer dan één Service Fabric-knooppunt op een enkele fysieke of virtuele machine hebt is, moet u het cluster met de nieuwe versie opnieuw maken.

Twee afzonderlijke werkstromen kunnen uw cluster upgraden naar de nieuwste versie of een ondersteunde versie van de Service Fabric. Een werkstroom is voor clusters die verbonden zijn met de meest recente versie automatisch downloaden. De andere workflow is voor clusters die niet verbonden zijn met het downloaden van de meest recente versie van Service Fabric.

### <a name="upgrade-clusters-that-have-connectivity-to-download-the-latest-code-and-configuration"></a>Bijwerken van clusters die verbonden zijn met het downloaden van de meest recente code en configuratie
Volg deze stappen om uw cluster naar een ondersteunde versie upgraden als de clusterknooppunten beschikt over een internetverbinding naar de [Microsoft Download Center](http://download.microsoft.com).

Voor clusters die verbonden met zijn de [Microsoft Download Center](http://download.microsoft.com), Microsoft controleert periodiek op de beschikbaarheid van de nieuwe Service Fabric-versies.

Wanneer een nieuwe Service Fabric-versie beschikbaar is, wordt het pakket lokaal wordt gedownload naar het cluster en ingericht voor de upgrade. Daarnaast om te informeren over de klant van deze nieuwe versie, worden er een waarschuwing expliciete cluster die vergelijkbaar is met het volgende:

"De huidige versie van de cluster [versie #] [datum] ondersteuning wordt beëindigd."

Nadat het cluster wordt uitgevoerd op de nieuwste versie, verdwijnt de waarschuwing.

#### <a name="cluster-upgrade-workflow"></a>Werkstroom voor serverupgrades cluster
Wanneer u het cluster: waarschuwing ziet, het volgende doen:

1. Verbinding maken met het cluster vanaf een willekeurige computer met beheerdersrechten op alle machines die worden weergegeven als knooppunten in het cluster. Dit script wordt uitgevoerd op de machine geen deel uitmaken van het cluster.

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

    Deze krijgt u uitvoer ongeveer als volgt:

    ![Service Fabric-versies ophalen][getfabversions]
3. Start de upgrade van een cluster naar een versie met de [Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) Windows PowerShell-opdracht.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   U kunt gebruiken van Service Fabric Explorer of Voer de volgende PowerShell-opdracht om de voortgang van de upgrade:

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Als de cluster-statusbeleid niet worden voldaan, is de upgrade teruggedraaid. Zie de documentatie voor het opgeven van aangepaste statusbeleid voor de opdracht Start-ServiceFabricClusterUpgrade [Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

Nadat u de problemen die heeft uitgewezen dat het terugdraaien opgelost, start u de upgrade opnieuw de dezelfde stappen als eerder beschreven.

### <a name="upgrade-clusters-that-have-no-connectivity-to-download-the-latest-code-and-configuration"></a>Bijwerken van clusters met *geen verbinding* voor het downloaden van de meest recente code en configuratie
Volg deze stappen om uw cluster naar een ondersteunde versie upgraden als de clusterknooppunten geen internetverbinding naar de [Microsoft Download Center](http://download.microsoft.com).

> [!NOTE]
> Als u een cluster die niet is verbonden met internet uitvoert, hebt u voor het bewaken van de Service Fabric-team-blog voor meer informatie over nieuwe releases. Het systeem weergegeven niet een cluster: waarschuwing u nieuwe releases een waarschuwing.  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a>Automatische inrichting versus handmatige inrichting
Instellen van de Service Fabric-Update-Service zodat automatisch downloaden en registratie voor de meest recente codeversie. Zie voor instructies Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt binnen de [zelfstandig pakket](service-fabric-cluster-standalone-package-contents.md).
Volg deze instructies voor het handmatige proces.

Wijzigen van uw clusterconfiguratie de volgende eigenschap wilt instellen op *false* voordat u een upgrade van een configuratie:

        "fabricClusterAutoupgradeEnabled": false,

Zie voor meer informatie over het gebruik, de [Start ServiceFabricClusterConfigurationUpgrade PowerShell-opdracht](https://msdn.microsoft.com/en-us/library/mt788302.aspx). Zorg ervoor dat 'clusterConfigurationVersion' in de JSON bijwerken voordat u de configuratie-upgrade start.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

#### <a name="cluster-upgrade-workflow"></a>Werkstroom voor serverupgrades cluster

1. Get-ServiceFabricClusterUpgrade uitvoeren vanaf een van de knooppunten in het cluster en noteer de TargetCodeVersion.

2. Het volgende uitvoeren via een internetverbinding machine lijst met alle versies van de upgrade-compatibel is met de huidige versie en de bijbehorende downloaden uit de bijbehorende downloadkoppelingen:

    ```powershell

    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. Verbinding maken met het cluster vanaf een willekeurige computer met beheerdersrechten op alle machines die worden weergegeven als knooppunten in het cluster. Dit script wordt uitgevoerd op de machine geen deel uitmaken van het cluster.

    ```powershell

   ###### Get the list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

   ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```
4. Kopieer het gedownloade pakket in het archief van de cluster-installatiekopie.

5. De gekopieerde pakket registreren.

    ```powershell

    ###### Get the list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
6. Start een upgrade van het cluster naar een beschikbare versie.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   U kunt de voortgang van de upgrade in Service Fabric Explorer of u kunt de volgende PowerShell-opdracht uitvoeren:

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Als de cluster-statusbeleid niet worden voldaan, is de upgrade teruggedraaid. Zie de documentatie voor het opgeven van aangepaste statusbeleid voor de opdracht Start-ServiceFabricClusterUpgrade [Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).

Nadat u de problemen die heeft uitgewezen dat het terugdraaien opgelost, start u de upgrade opnieuw de dezelfde stappen als eerder beschreven.


## <a name="upgrade-the-cluster-configuration"></a>Upgrade de clusterconfiguratie
Voordat u de upgrade van de configuratie hebt gestart, kunt u uw nieuwe clusterconfiguratie JSON testen door het volgende PowerShell-script uitgevoerd in het zelfstandige pakket:

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>

```
Of dit script gebruiken:

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>

```

Sommige configuraties kunnen niet worden bijgewerkt, zoals eindpunten, de naam van het cluster, knooppunt IP-adres enzovoort. Configuratie van het nieuwe cluster JSON is getest met de oude heeft en er fouten in de PowerShell-venster als er een probleem is.

Als u de upgrade van de configuratie van cluster upgraden, Start-ServiceFabricClusterConfigurationUpgrade worden uitgevoerd. De upgrade van de configuratie is verwerkt upgradedomein door upgradedomein.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

### <a name="cluster-certificate-config-upgrade"></a>Upgraden van cluster certificaat-configuratie  
Een certificaat van het cluster wordt gebruikt voor authenticatie tussen clusterknooppunten. De overschakeling van het certificaat moet worden uitgevoerd met extra voorzichtig omdat fout de communicatie tussen clusterknooppunten blokkeert.

Technisch gezien is worden vier opties ondersteund:  

* Er wordt één certificaat upgrade: het upgradepad is een (primaire) -> certificaat B (primair)-certificaat -> certificaat C (primair) ->...

* Dubbele certificaat upgrade: het upgradepad is een (primaire) -> certificaat van het certificaat een (primaire) en B (secundair) -> certificaat B (primair) -> certificaat B (primair) en C (secundair) -> certificaat C (primair) ->...

* Type upgrade van het certificaat: op basis van de vingerafdruk van certificaat configuratie <>--certificaat CommonName gebaseerde configuratie. Bijvoorbeeld, de vingerafdruk van het certificaat een (primaire) en vingerafdruk B (secundair) -> certificaat CommonName C.

* Certificaatverlener vingerafdruk upgrade van het certificaat: het upgradepad is certificaat CN = A, IssuerThumbprint IT1 = (primaire) certificaat CN -> = A, IssuerThumbprint IT1, IT2 = (primaire) -> certificaat CN = A, IssuerThumbprint IT2 = (primaire).


## <a name="next-steps"></a>Volgende stappen
* Informatie over het aanpassen van sommige [Service Fabric-clusterinstellingen](service-fabric-cluster-fabric-settings.md).
* Meer informatie over hoe [in en uit het cluster schalen](service-fabric-cluster-scale-up-down.md).
* Meer informatie over [toepassingsupgrades](service-fabric-application-upgrade.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
