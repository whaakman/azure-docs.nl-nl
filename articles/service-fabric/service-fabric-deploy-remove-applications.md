---
title: Implementatie van Azure Service Fabric-toepassing | Microsoft Docs
description: Het implementeren en toepassingen in Service Fabric met behulp van PowerShell verwijderen.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/19/2018
ms.author: ryanwi
ms.openlocfilehash: cd3c97bc95fb7ccfaa0dbf2d7ca76ae598a4b320
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="deploy-and-remove-applications-using-powershell"></a>Implementeren en verwijderen van toepassingen met behulp van PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager](service-fabric-application-arm-resource.md)
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Service Fabric-CLI](service-fabric-application-lifecycle-sfctl.md)
> * [FabricClient-API's](service-fabric-deploy-remove-applications-fabricclient.md)

<br/>

Eenmaal een [toepassingstype zijn ingepakt][10], deze gereed is voor implementatie in een Azure Service Fabric-cluster. Implementatie omvat de volgende drie stappen:

1. Het toepassingspakket uploaden naar de image store.
2. Het toepassingstype registreren met het relatieve pad naar afbeelding store.
3. Exemplaar van de toepassing maken.

Zodra de geïmplementeerde toepassing niet langer vereist is, kunt u het toepassingsexemplaar van de en het toepassingstype verwijderen. Omvat de volgende stappen om een toepassing volledig te verwijderen uit het cluster:

1. De uitgevoerd verwijderen (of verwijderen) toepassingsexemplaar.
2. Hef de registratie van het toepassingstype als u deze niet langer nodig hebt.
3. Het toepassingspakket verwijderen uit de image store.

Als u Visual Studio gebruikt voor het implementeren en foutopsporing van toepassingen op uw lokale ontwikkeling-cluster, worden automatisch alle voorgaande stappen afgehandeld via een PowerShell-script.  Dit script is gevonden in de *Scripts* map van het toepassingsproject. Dit artikel vindt achtergrond op script doet zodat u dezelfde bewerkingen buiten Visual Studio kunt uitvoeren. 

Er is een andere manier om een toepassing te implementeren met behulp van externe inrichten. Het toepassingspakket kan worden [verpakte als `sfpkg` ](service-fabric-package-apps.md#create-an-sfpkg) en geüpload naar een externe winkel. Uploaden naar de image store is niet nodig. Implementatie moet de volgende stappen uit:

1. Upload de `sfpkg` naar een externe store. Het externe archief kan een archief dat toegang biedt tot een REST-eindpunt voor http of https zijn.
2. Registreren van het type van de toepassing met het downloaden van de externe URI en de type-informatie van toepassing.
2. Exemplaar van de toepassing maken.

Voor opschonen van de exemplaren van een toepassing verwijderen en registratie van het toepassingstype. Omdat het pakket is niet gekopieerd naar de image store, is er geen tijdelijke locatie op te schonen. Inrichting via externe store is beschikbaar vanaf Service Fabric versie 6.1.

>[!NOTE]
> Visual Studio biedt momenteel geen ondersteuning voor externe inrichten.

 
## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster
Voordat u een PowerShell-opdrachten in dit artikel uitvoert, wordt altijd gestart via [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) verbinding maken met de Service Fabric-cluster. Voor verbinding met het lokaal ontwikkelcluster, voert u de volgende:

```powershell
PS C:\>Connect-ServiceFabricCluster
```

Voor voorbeelden van verbinding maken met een externe cluster of cluster die zijn beveiligd met Azure Active Directory, X509 certificaten of Windows Active Directory-Zie [verbinding maken met een beveiligde cluster](service-fabric-connect-to-secure-cluster.md).

## <a name="upload-the-application-package"></a>Het toepassingspakket uploaden
Uploaden van het toepassingspakket plaatst het in een locatie die toegankelijk is voor interne Service Fabric-onderdelen.
Als u controleren of het toepassingspakket lokaal wilt, gebruikt u de [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.

De [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) opdracht het toepassingspakket uploadt naar het archief van de cluster-installatiekopie.

Stel dat u maakt en een toepassing met de naam van het pakket *Mijntoepassing* in Visual Studio 2015. Standaard is de naam van het toepassingstype die worden vermeld in de ApplicationManifest.xml 'MyApplicationType'.  Het toepassingspakket dat de benodigde toepassingsmanifest, manifesten service en config-code-gegevens pakketten bevat, bevindt zich in *C:\Users\<gebruikersnaam\>\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug*. 

De volgende opdracht worden de inhoud van het toepassingspakket:

```powershell
PS C:\> $path = 'C:\Users\<user\>\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug'
PS C:\> tree /f $path
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
│   ApplicationManifest.xml
│
└───Stateless1Pkg
    │   ServiceManifest.xml
    │
    ├───Code
    │       Microsoft.ServiceFabric.Data.dll
    │       Microsoft.ServiceFabric.Data.Interfaces.dll
    │       Microsoft.ServiceFabric.Internal.dll
    │       Microsoft.ServiceFabric.Internal.Strings.dll
    │       Microsoft.ServiceFabric.Services.dll
    │       ServiceFabricServiceModel.dll
    │       Stateless1.exe
    │       Stateless1.exe.config
    │       Stateless1.pdb
    │       System.Fabric.dll
    │       System.Fabric.Strings.dll
    │
    └───Config
            Settings.xml
```

Als het toepassingspakket groot is en/of veel bestanden, kunt u [comprimeert](service-fabric-package-apps.md#compress-a-package). De compressie vermindert de grootte en het aantal bestanden.
Een neveneffect is dat registreren en afmelden het toepassingstype zijn sneller. Uploadtijd trager worden momenteel, met name als u de tijd voor het comprimeren van het pakket opnemen. 

Voor het comprimeren van een pakket, gebruikt u dezelfde [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) opdracht. Compressie kan worden gedaan afzonderlijke van uploaden, door met de `SkipCopy` vlag of samen met de uploadbewerking. Compressie toepassen op een gecomprimeerde pakket is geen.
Als u wilt een gecomprimeerde pakket niet decomprimeren, gebruik van dezelfde [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) opdracht met de `UncompressPackage` overschakelen.

Het pakket, de volgende cmdlet comprimeren zonder kopiëren naar de image store. Het pakket bevat nu ZIP-bestanden voor de `Code` en `Config` pakketten. De toepassing en de manifesten voor de service zijn niet ingepakte, omdat ze nodig zijn voor veel interne bewerkingen (zoals pakket delen, een toepassing de naam en versie extraheren voor bepaalde validaties). Comprimeren van de manifesten zouden deze bewerkingen niet efficiënt.

```
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -CompressPackage -SkipCopy
PS C:\> tree /f $path
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
|   ApplicationManifest.xml
|
└───Stateless1Pkg
       Code.zip
       Config.zip
       ServiceManifest.xml
```

Voor grote toepassingpakketten duurt de compressie. Gebruik een snel SSD-station voor de beste resultaten. De tijden compressie en de grootte van het gecomprimeerde pakket ook u verschillen op basis van de inhoud van het pakket.
Dit is bijvoorbeeld compressie statistieken voor sommige pakketten, die de eerste en de grootte van het gecomprimeerde pakket, met de tijd compressie weergeven.

|Aanvankelijke grootte (MB)|Aantal bestanden|Compressie tijd|Gecomprimeerde pakketgrootte (MB)|
|----------------:|---------:|---------------:|---------------------------:|
|100|100|00:00:03.3547592|60|
|512|100|00:00:16.3850303|307|
|1024|500|00:00:32.5907950|615|
|2048|1000|00:01:04.3775554|1231|
|5012|100|00:02:45.2951288|3074|

Nadat u een pakket is gecomprimeerd, kan deze worden geüpload naar een of meerdere Service Fabric-clusters naar behoefte. Het mechanisme deployment is hetzelfde voor gecomprimeerde en ongecomprimeerde pakketten. Gecomprimeerde pakketten worden als zodanig opgeslagen in het archief van de cluster-installatiekopie. De pakketten zijn gedecomprimeerd op het knooppunt voordat de toepassing wordt uitgevoerd.


Het volgende voorbeeld wordt het pakket geüpload naar het archief van de installatiekopie naar een map met de naam 'MyApplicationV1':

```powershell
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -TimeoutSec 1800
```

Als u niet geeft de *- ApplicationPackagePathInImageStore* parameter, het toepassingspakket is gekopieerd naar de map 'Debug' in de image store.

>[!NOTE]
>**Kopiëren ServiceFabricApplicationPackage** detecteert automatisch de juiste installatiekopie store-verbindingsreeks als de PowerShell-sessie is verbonden met een Service Fabric-cluster. Voor Service Fabric-versies ouder dan 5.6, de **- ImageStoreConnectionString** argument moet expliciet worden opgegeven.
>
>```powershell
>PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)) -TimeoutSec 1800
>```
>
>De **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet die deel uitmaakt van de Service Fabric SDK PowerShell-module, waarmee de image store-verbindingsreeks ophalen.  Voor het importeren van de SDK-module worden uitgevoerd:
>
>```powershell
>Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
>```
>
>Zie [inzicht in de verbindingsreeks van de image store](service-fabric-image-store-connection-string.md) verbindingsreeks voor aanvullende informatie over de image store en de installatiekopie te slaan.
>
>
>

De tijd die nodig zijn om een pakket te uploaden, is afhankelijk van meerdere factoren. Sommige van deze factoren zijn het aantal bestanden in het pakket, de grootte van het pakket en de bestandsgrootte. De netwerksnelheid tussen de bronmachine en het Service Fabric-cluster ook van invloed is op de tijd voor uploaden. De standaardtime-out voor [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) is 30 minuten.
Afhankelijk van de factoren beschreven, is het wellicht Verhoog de time-out. Als u het pakket in de aanroep van de kopie comprimeert, moet u ook rekening houden met de compressie-tijd.



## <a name="register-the-application-package"></a>Registreren van het toepassingspakket
Het toepassingstype en de versie is gedeclareerd in het toepassingsmanifest worden beschikbaar voor gebruik wanneer het toepassingspakket is geregistreerd. Het systeem leest het pakket dat is geüpload in de vorige stap, controleert of het pakket, de inhoud van het pakket verwerkt en de verwerkte pakket gekopieerd naar de locatie van een intern systeemprobleem.  

Voer de [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) cmdlet het toepassingstype registreren in het cluster en het beschikbaar voor implementatie:

### <a name="register-the-application-package-copied-to-image-store"></a>Het toepassingspakket gekopieerd naar de installatiekopieopslag registreren
Wanneer een pakket is eerder hebt gekopieerd naar de image store, geeft de registerbewerking het relatieve pad in de image store.

```powershell
PS C:\> Register-ServiceFabricApplicationType -ApplicationPackagePathInImageStore MyApplicationV1
Register application type succeeded
```

'MyApplicationV1' is de map in de image store waar het toepassingspakket zich bevindt. Het toepassingstype met de naam 'MyApplicationType' en '1.0.0' (zowel gevonden in het toepassingsmanifest) versie is nu geregistreerd in het cluster.

### <a name="register-the-application-package-copied-to-an-external-store"></a>Het toepassingspakket gekopieerd naar een externe winkel registreren
Beginnen met het Service Fabric versie 6.1, inrichten ondersteunt het pakket downloaden vanaf een externe winkel. Het downloaden van de URI het pad naar vertegenwoordigt de [ `sfpkg` toepassingspakket](service-fabric-package-apps.md#create-an-sfpkg) waar het toepassingspakket kan worden gedownload met behulp van HTTP of HTTPS-protocollen. Het pakket moet zijn eerder hebt geüpload naar deze externe locatie. De URI moet leestoegang toestaan om Service Fabric kan het bestand downloaden. De `sfpkg` bestand moet de extensie '.sfpkg' hebben. De inrichtingsbewerking moet de informatie van het type toepassing, zoals gevonden in het toepassingsmanifest bevatten.

```
PS C:\> Register-ServiceFabricApplicationType -ApplicationPackageDownloadUri "https://sftestresources.blob.core.windows.net:443/sfpkgholder/MyAppPackage.sfpkg" -ApplicationTypeName MyApp -ApplicationTypeVersion V1 -Async
```

De [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) opdracht retourneert alleen nadat het systeem heeft het toepassingspakket geregistreerd. Hoe lang duurt registratie afhankelijk van de grootte en de inhoud van het toepassingspakket. Indien nodig, de **- TimeoutSec** parameter kan worden gebruikt om op te geven van een langere time-out (de standaardtime-out is 60 seconden).

Als u een grote toepassing van het pakket of als u time-outs ondervindt, gebruikt u de **Async -** parameter. De opdracht retourneert wanneer het cluster de opdracht register accepteert. De registerbewerking wordt voortgezet zoals nodig.
De [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) opdracht geeft de versies van het type toepassingen en hun registratiestatus. U kunt deze opdracht gebruiken om te bepalen wanneer de registratie is uitgevoerd.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="remove-an-application-package-from-the-image-store"></a>Een toepassingspakket verwijderen uit de image store
Als een pakket is gekopieerd naar de image store, moet u deze verwijderen vanuit de tijdelijke locatie nadat de toepassing is geregistreerd. Toepassingspakketten verwijderen uit de image store vrijgemaakt systeembronnen. Schijfopslag verbruikt en leidt tot een toepassing prestatieproblemen in ongebruikte toepassingspakketten houden.

```powershell
PS C:\>Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore MyApplicationV1
```

## <a name="create-the-application"></a>De toepassing maken
U kunt een toepassing met een versie van het toepassingstype dat is geregistreerd met behulp van instantiëren de [nieuw ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet. De naam van elke toepassing moet beginnen met de *"fabric: '* schema en moet uniek zijn voor elk toepassingsexemplaar. Standaardservices die zijn gedefinieerd in het toepassingsmanifest van het type doeltoepassing worden ook gemaakt.

```powershell
PS C:\> New-ServiceFabricApplication fabric:/MyApp MyApplicationType 1.0.0

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationParameters  : {}
```
Meerdere exemplaren van een toepassing kunnen worden gemaakt voor een bepaalde versie van een geregistreerde toepassingstype. Elk toepassingsexemplaar wordt uitgevoerd in een geïsoleerde omgeving met een eigen werk directory en het proces.

Om te zien die met de naam apps en services worden uitgevoerd in het cluster, voer de [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) en [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) cmdlets:

```powershell
PS C:\> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Ok
ApplicationParameters  : {}

PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/Stateless1
ServiceKind            : Stateless
ServiceTypeName        : Stateless1Type
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
ServiceStatus          : Active
HealthState            : Ok
```

## <a name="remove-an-application"></a>Een toepassing verwijderen
Wanneer een toepassingsexemplaar niet meer nodig is, u kunt definitief wordt verwijderd met behulp van de naam van de [verwijderen ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) cmdlet. [Verwijder ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) alle services die deel uitmaken van de toepassing ook, permanent verwijderen van de status van alle service wordt automatisch verwijderd. 

> [!WARNING]
> Deze bewerking kan niet ongedaan worden gemaakt en de status van toepassing kan niet worden hersteld.

```powershell
PS C:\> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS C:\> Get-ServiceFabricApplication
```

## <a name="unregister-an-application-type"></a>Registratie van een toepassingstype
Wanneer een bepaalde versie van het type van een toepassing niet meer nodig is, moet u de toepassing gebruikt registratie de [Unregister ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) cmdlet. De registratie van ongebruikte toepassingstypen versies opslagruimte die wordt gebruikt door de image store door het type toepassingsbestanden te verwijderen. De registratie van een toepassingstype verwijdert niet het toepassingspakket gekopieerd naar de image store tijdelijke locatie, als kopiëren naar de image store is gebruikt. Een toepassingstype kan ongedaan worden zolang geen toepassingen worden geïnstantieerd tegen en niet in afwachting van toepassing upgrades hiernaar wordt verwezen.

Voer [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) om te zien van de toepassingstypen die zijn geregistreerd in het cluster:

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

Voer [Unregister ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) registratie van het type van een specifieke toepassing:

```powershell
PS C:\> Unregister-ServiceFabricApplicationType MyApplicationType 1.0.0
```

## <a name="troubleshooting"></a>Problemen oplossen
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopiëren ServiceFabricApplicationPackage vraagt om een ImageStoreConnectionString
De Service Fabric SDK-omgeving moet al de juiste standaardwaarden instellen. Maar indien nodig, de ImageStoreConnectionString voor alle opdrachten moet overeenkomen met de waarde die het Service Fabric-cluster wordt gebruikt. U kunt de ImageStoreConnectionString vinden in het clustermanifest opgehaald met de [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) en Get-ImageStoreConnectionStringFromClusterManifest-opdrachten:

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

De **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet die deel uitmaakt van de Service Fabric SDK PowerShell-module, waarmee de image store-verbindingsreeks ophalen.  Voor het importeren van de SDK-module worden uitgevoerd:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

De ImageStoreConnectionString is gevonden in het clustermanifest:

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

Zie [inzicht in de verbindingsreeks van de image store](service-fabric-image-store-connection-string.md) verbindingsreeks voor aanvullende informatie over de image store en de installatiekopie te slaan.

### <a name="deploy-large-application-package"></a>Grote toepassingspakket implementeren
Probleem: [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) een time-out voor een groot pakket (in volgorde van GB).
Probeer een van:
- Geef een hogere time-out voor [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) opdracht met `TimeoutSec` parameter. Standaard is de time-out voor 30 minuten.
- Controleer de netwerkverbinding tussen de bron- en cluster. Als de verbinding langzaam is, kunt u overwegen een machine met een betere netwerkverbinding.
Als de clientcomputer zich in een andere regio dan het cluster, overweeg het gebruik van een client-computer in een dichter of dezelfde regio als het cluster.
- Controleer als u externe beperking zijn raken. Wanneer de image store is geconfigureerd voor gebruik van azure-opslag, kunnen uploaden bijvoorbeeld kan worden beperkt.

Probleem: Upload-pakket is voltooid, maar [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) een time-out optreedt. Probeer een van:
- [Comprimeren van het pakket](service-fabric-package-apps.md#compress-a-package) voordat u naar de image store kopieert.
De compressie vermindert de grootte en het aantal bestanden dat op zijn beurt reduceert de hoeveelheid verkeer van en werken die Service Fabric moet uitvoeren. De uploadbewerking mogelijk langzamer (met name als u de tijd compressie opnemen), maar het register en het toepassingstype ongedaan maken van de registratie zijn sneller.
- Geef een hogere time-out voor [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) met `TimeoutSec` parameter.
- Geef `Async` overschakelen voor [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps). De opdracht retourneert wanneer het cluster de opdracht accepteert en de registratie van het toepassingstype asynchroon blijft. Daarom hoeft niet in dit geval een hogere time-out opgeven. De [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) opdracht geeft een lijst van alle versies van het type toepassing is geregistreerd en hun registratiestatus. U kunt deze opdracht gebruiken om te bepalen wanneer de registratie is uitgevoerd.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

### <a name="deploy-application-package-with-many-files"></a>Toepassingspakket implementeren met veel bestanden
Probleem: [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) time-out voor een toepassingspakket met veel bestanden (volgorde van duizendtallen).
Probeer een van:
- [Comprimeren van het pakket](service-fabric-package-apps.md#compress-a-package) voordat u naar de image store kopieert. De compressie vermindert het aantal bestanden.
- Geef een hogere time-out voor [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) met `TimeoutSec` parameter.
- Geef `Async` overschakelen voor [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps). De opdracht retourneert wanneer het cluster de opdracht accepteert en de registratie van het toepassingstype asynchroon blijft.
Daarom hoeft niet in dit geval een hogere time-out opgeven. De [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) opdracht geeft een lijst van alle versies van het type toepassing is geregistreerd en hun registratiestatus. U kunt deze opdracht gebruiken om te bepalen wanneer de registratie is uitgevoerd.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="next-steps"></a>Volgende stappen
[Toepassingspakket maken](service-fabric-package-apps.md)

[Upgrade van de service Fabric-toepassing](service-fabric-application-upgrade.md)

[Service Fabric health Inleiding](service-fabric-health-introduction.md)

[Vaststellen en oplossen van een Service Fabric-service](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Model van een toepassing in Service Fabric](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-package-apps.md
[11]: service-fabric-application-upgrade.md
