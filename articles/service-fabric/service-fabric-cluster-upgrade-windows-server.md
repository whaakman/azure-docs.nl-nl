---
title: De versie van een zelfstandig Azure Service Fabric cluster bijwerken | Microsoft Docs
description: Werk de Azure Service Fabric-code die wordt uitgevoerd een zelfstandige Service Fabric-cluster.
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
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: cac0b620ae0361ba6903599799f80adb9a917fc7
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2018
ms.locfileid: "51853112"
---
# <a name="upgrade-the-service-fabric-version-that-runs-on-your-cluster"></a>De Service Fabric-versie die wordt uitgevoerd op uw cluster bijwerken 

Voor elk modern systeem is de mogelijkheid om te upgraden essentieel voor het succes op lange termijn van het product. Een Azure Service Fabric-cluster is een resource waarvan u eigenaar bent. In dit artikel wordt beschreven hoe u de versie van Service Fabric die worden uitgevoerd op uw zelfstandige cluster bijwerkt.

> [!NOTE]
> Zorg ervoor dat uw cluster altijd wordt uitgevoerd een ondersteunde versie van Service Fabric. Wanneer Microsoft de release van een nieuwe versie van Service Fabric kondigt, betekent dit dat de vorige versie zich na een minimum van 60 dagen vanaf de datum van de aankondiging is gemarkeerd voor einde van ondersteuning. Nieuwe releases worden aangekondigd [op de blog van het Service Fabric-team](https://blogs.msdn.microsoft.com/azureservicefabric/). De nieuwe versie is beschikbaar om te kiezen op dat moment.
>
>

U kunt uw cluster upgraden naar de nieuwe versie, alleen als u een productie-achtige knooppuntconfiguratie, waarbij elke Service Fabric-knooppunt op een afzonderlijke fysieke of virtuele machine is toegewezen. Hebt u een ontwikkelingscluster, waarbij meer dan één Service Fabric-knooppunt op een enkele fysieke of virtuele machine is, moet u het cluster met de nieuwe versie opnieuw maken.

Twee afzonderlijke werkstromen kunnen u uw cluster bijwerken naar de meest recente versie of een ondersteunde versie van de Service Fabric. Er is een werkstroom voor clusters die verbonden zijn met de nieuwste versie automatisch downloaden. De andere werkstroom is voor clusters die niet verbonden zijn met het downloaden van de meest recente versie van Service Fabric.

## <a name="enable-auto-upgrade-of-the-service-fabric-version-of-your-cluster"></a>Auto-upgrade van de Service Fabric-versie van het cluster inschakelen
Om in te stellen uw cluster om updates te downloaden van Service Fabric wanneer Microsoft een nieuwe versie brengt, de `fabricClusterAutoupgradeEnabled` clusterconfiguratie naar *waar*. Instellen om handmatig een ondersteunde versie van Service Fabric, waarmee u uw cluster op wilt selecteren, de `fabricClusterAutoupgradeEnabled` clusterconfiguratie naar *false*.

## <a name="upgrade-clusters-that-have-connectivity-to-download-the-latest-code-and-configuration"></a>Bijwerken van clusters die verbonden zijn met het downloaden van de meest recente code en configuratie
Volg deze stappen om uw cluster naar een ondersteunde versie upgraden als de clusterknooppunten over een internetverbinding beschikt op de [Microsoft Download Center](https://download.microsoft.com).

Voor clusters die verbonden met zijn de [Microsoft Download Center](https://download.microsoft.com), Microsoft controleert periodiek op de beschikbaarheid van de nieuwe Service Fabric-versies.

Wanneer een nieuwe Service Fabric-versie beschikbaar is, wordt het pakket gedownload naar het cluster en ingericht voor de upgrade. Daarnaast bevat het systeem om te informeren over de klant van deze nieuwe versie, een expliciete cluster status waarschuwing die vergelijkbaar is met het volgende:

"De huidige clusterversie [versie #] [datum] ondersteuning wordt beëindigd."

Nadat het cluster de meest recente versie wordt uitgevoerd, verdwijnt de waarschuwing.

Wanneer u het cluster: waarschuwing ziet, moet u het cluster upgraden:

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

    ```powershell
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

## <a name="upgrade-clusters-that-have-no-connectivity-to-download-the-latest-code-and-configuration"></a>Bijwerken van clusters met *afwezig* voor het downloaden van de meest recente code en configuratie
Volg deze stappen om uw cluster naar een ondersteunde versie upgraden als de clusterknooppunten geen internetverbinding met de [Microsoft Download Center](https://download.microsoft.com).

> [!NOTE]
> Als u een cluster dat is niet met internet verbonden uitvoert, hebt u voor het bewaken van de [Service Fabric-teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/) voor meer informatie over nieuwe releases. Het systeem kan een cluster: waarschuwing u nieuwe releases wordt niet weergegeven.  
>
>

### <a name="auto-provisioning-vs-manual-provisioning"></a>Automatische inrichting versus handmatige inrichting
Instellen om in te schakelen automatisch downloaden en de registratie voor de meest recente codeversie, de Service Fabric-Update-Service. Zie voor instructies *Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt* in de [zelfstandige pakket](service-fabric-cluster-standalone-package-contents.md).

Volg deze instructies voor het handmatige proces.

Wijzigen van de configuratie van het cluster de volgende eigenschap wilt instellen op *false* voordat u een upgrade van een configuratie:

```json
"fabricClusterAutoupgradeEnabled": false,
```

Zie voor informatie over het gebruik, de [Start ServiceFabricClusterConfigurationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade) PowerShell-opdracht. Zorg ervoor dat u 'clusterConfigurationVersion' in het JSON bijwerken voordat u de configuratie-upgrade start.

```powershell
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
```

### <a name="cluster-upgrade-workflow"></a>Upgradewerkstroom voor cluster

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

    ```powershell
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

## <a name="next-steps"></a>Volgende stappen
* [De configuratie van een zelfstandige cluster upgraden](service-fabric-cluster-config-upgrade-windows-server.md)
* Sommige aanpassen [instellingen voor Service Fabric-cluster](service-fabric-cluster-fabric-settings.md).
* [Uw cluster inschalen en uitschalen](service-fabric-cluster-scale-up-down.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
