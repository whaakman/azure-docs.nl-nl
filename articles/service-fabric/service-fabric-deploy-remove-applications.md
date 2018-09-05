---
title: Implementatie van Azure Service Fabric-toepassing | Microsoft Docs
description: Over het implementeren en verwijderen van toepassingen in Service Fabric met behulp van PowerShell.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/19/2018
ms.author: ryanwi
ms.openlocfilehash: d38ec87fb634e1809959b85f0382935e8a78bf3b
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43697161"
---
# <a name="deploy-and-remove-applications-using-powershell"></a>Implementeren en verwijderen van toepassingen met behulp van PowerShell
> [!div class="op_single_selector"]
> * [Resource Manager](service-fabric-application-arm-resource.md)
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Service Fabric-CLI](service-fabric-application-lifecycle-sfctl.md)
> * [FabricClient-API's](service-fabric-deploy-remove-applications-fabricclient.md)

<br/>

Zodra een [toepassingstype is ingepakt][10], deze gereed is voor implementatie in een Azure Service Fabric-cluster. Implementatie omvat de volgende drie stappen:

1. Het toepassingspakket uploaden naar de installatiekopieopslag.
2. Het toepassingstype te registreren met het relatieve pad naar afbeelding store.
3. Exemplaar van de toepassing maken.

Wanneer de geïmplementeerde toepassing niet langer vereist is, kunt u een exemplaar van de toepassing en het bijbehorende toepassingstype verwijderen. Als u wilt een toepassing volledig te verwijderen uit het cluster omvat de volgende stappen uit:

1. Verwijderen (of verwijderen) op de actieve exemplaar van toepassing.
2. De registratie van het toepassingstype ongedaan maken als u deze niet meer nodig hebt.
3. Het toepassingspakket verwijderen uit de installatiekopieopslag.

Als u Visual Studio gebruikt voor het implementeren en opsporen van fouten in toepassingen op uw lokale ontwikkelcluster, worden automatisch de voorgaande stappen verwerkt via een PowerShell-script.  Met dit script wordt gevonden de *Scripts* map van het project voor een toepassing. In dit artikel bevat achtergrondinformatie over wat dit script doet zodat u dezelfde bewerkingen buiten Visual Studio kunt uitvoeren. 

Er is een andere manier om een toepassing te implementeren met behulp van externe inrichten. Het toepassingspakket kan worden [verpakte als `sfpkg` ](service-fabric-package-apps.md#create-an-sfpkg) en geüpload naar een externe opslag. In dit geval is uploaden naar de installatiekopieopslag niet nodig. Implementatie moet de volgende stappen uit:

1. Upload de `sfpkg` naar een externe opslag. De externe opslag kan een archief dat wordt aangegeven dat een REST-eindpunt voor http of https zijn.
2. Registreren van het toepassingstype met behulp van het downloaden van de externe URI en de informatie van het type toepassing.
2. Exemplaar van de toepassing maken.

Om op te schonen, de exemplaren van een toepassing verwijderen en registratie van het toepassingstype. Omdat het pakket is niet gekopieerd naar de installatiekopieopslag, is er geen tijdelijke locatie worden opgeschoond. Inrichten vanuit externe opslag is beschikbaar vanaf met Service Fabric versie 6.1.

>[!NOTE]
> Visual Studio biedt momenteel geen ondersteuning voor externe inrichten.

 
## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster
Voordat u een PowerShell-opdrachten in dit artikel uitvoert, begin altijd met behulp van [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) verbinding maken met de Service Fabric-cluster. Voor verbinding met het lokale ontwikkelcluster, voert u het volgende uit:

```powershell
PS C:\>Connect-ServiceFabricCluster
```

Voor voorbeelden van het verbinden met een extern cluster of het cluster is beveiligd met behulp van Azure Active Directory, X509 certificaten of raadpleegt u de Active Directory Windows [verbinding maken met een beveiligd cluster](service-fabric-connect-to-secure-cluster.md).

## <a name="upload-the-application-package"></a>Het toepassingspakket uploaden
Uploaden van het toepassingspakket is ondergebracht in een locatie die toegankelijk is voor interne Service Fabric-onderdelen.
Als u controleren of het toepassingspakket op een lokaal wilt, gebruikt u de [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet.

De [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) opdracht wordt het toepassingspakket geüpload naar de installatiekopieopslag van het cluster.

Stel dat u maken en een toepassing met de naam *MyApplication* in Visual Studio 2015. Standaard is de naam van het toepassingen die worden vermeld in de ApplicationManifest.xml 'MyApplicationType'.  De, dit toepassingspakket de benodigde toepassingsmanifest, servicemanifesten en code/config/gegevenspakketten bevat, bevindt zich in *C:\Users\<gebruikersnaam\>\Documents\Visual Studio 2015\Projects\ MyApplication\MyApplication\pkg\Debug*. 

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
De neveneffect is dat registreren en niet registreren het toepassingstype zijn sneller. Uploadtijd trager worden op dit moment, met name als u de tijd voor het comprimeren van het pakket bevatten. 

Voor het comprimeren van een pakket, gebruikt u dezelfde [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) opdracht. Compressie kan worden gedaan afzonderlijke van het uploaden, door met behulp van de `SkipCopy` vlag of samen met de uploadbewerking. Compressie toepassen op een gecomprimeerde pakket is geen.
Als u wilt een gecomprimeerde pakket decomprimeren, gebruikt u dezelfde [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) opdracht met de `UncompressPackage` overschakelen.

De volgende cmdlet wordt het pakket zonder te kopiëren naar de installatiekopieopslag gecomprimeerd. Het pakket bevat nu ZIP-bestanden voor de `Code` en `Config` pakketten. De toepassing en de servicemanifesten zijn niet ingepakte, omdat ze nodig voor veel interne bewerkingen zijn wel (zoals delen, toepassing de naam en versie extraheren voor bepaalde validaties pakket). Het inpakken van de manifesten zouden deze bewerkingen inefficiënt.

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

Voor grote toepassingpakketten, is de compressie duurt. Voor de beste resultaten gebruikt u een snelle SSD-station. De tijden compressie en de grootte van het gecomprimeerde pakket ook u verschillen op basis van de inhoud van het pakket.
Bijvoorbeeld, als volgt compressie-statistieken voor sommige pakketten die worden weergegeven met de eerste en de grootte van het gecomprimeerde pakket, met de compressie-tijd.

|Oorspronkelijke grootte (MB)|Aantal bestanden|Compressie-tijd|Gecomprimeerde pakketgrootte (MB)|
|----------------:|---------:|---------------:|---------------------------:|
|100|100|00:00:03.3547592|60|
|512|100|00:00:16.3850303|307|
|1024|500|00:00:32.5907950|615|
|2048|1000|00:01:04.3775554|1231|
|5012|100|00:02:45.2951288|3074|

Nadat u een pakket is gecomprimeerd, kan deze worden geüpload naar een of meer Service Fabric-clusters naar behoefte. Het implementatiemechanisme voor is hetzelfde voor gecomprimeerde en niet-gecomprimeerde pakketten. Gecomprimeerde pakketten worden als zodanig opgeslagen in de installatiekopieopslag van het cluster. De pakketten zijn niet-gecomprimeerd op het knooppunt voordat de toepassing wordt uitgevoerd.


In het volgende voorbeeld wordt het pakket naar de installatiekopieopslag geüpload naar een map met de naam 'MyApplicationV1':

```powershell
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -TimeoutSec 1800
```

Als u geen opgeeft de *- ApplicationPackagePathInImageStore* parameter, het toepassingspakket is gekopieerd naar de map "Debug" in de installatiekopieopslag.

>[!NOTE]
>**Kopiëren-ServiceFabricApplicationPackage** detecteert automatisch de juiste installatiekopie store-verbindingsreeks als de PowerShell-sessie is verbonden met een Service Fabric-cluster. Voor Service Fabric-versies ouder dan 5.6, de **- ImageStoreConnectionString** argument moet expliciet worden opgegeven.
>
>```powershell
>PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)) -TimeoutSec 1800
>```
>
>De **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet die deel van de Service Fabric SDK PowerShell-module uitmaakt, wordt gebruikt om op te halen van de verbindingsreeks van de installatiekopie-store.  Voor het importeren van de SDK-module, voert u de volgende uit:
>
>```powershell
>Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
>```
>
>Zie [inzicht in de verbindingsreeks van de installatiekopie-store](service-fabric-image-store-connection-string.md) verbindingsreeks voor aanvullende informatie over de installatiekopieopslag en de installatiekopie te slaan.
>
>
>

De tijd die nodig is voor het uploaden van een pakket, is afhankelijk van meerdere factoren. Sommige van deze factoren zijn het aantal bestanden in het pakket, de grootte van het pakket en de grootte. De snelheid van het netwerk tussen de broncomputer en de Service Fabric-cluster zijn ook van invloed op de tijd. De standaardtime-out voor [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) is 30 minuten.
Afhankelijk van de factoren beschreven, moet u wellicht Verhoog de time-out. Als u het pakket in de aanroep van de kopie comprimeert, moet u ook rekening houden met de compressie-tijd.



## <a name="register-the-application-package"></a>Registreren van het toepassingspakket
Het toepassingstype en de versie is gedeclareerd in het manifest van de toepassing beschikbaar voor gebruik wanneer het toepassingspakket is geregistreerd. Het systeem leest het pakket in de vorige stap hebt geüpload, controleert of het pakket, de inhoud van het pakket verwerkt en de verwerkte pakket worden gekopieerd naar de locatie van een intern systeem.  

Voer de [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) cmdlet het toepassingstype te registreren in het cluster en het beschikbaar voor implementatie:

### <a name="register-the-application-package-copied-to-image-store"></a>Het toepassingspakket gekopieerd naar de installatiekopieopslag registreren
Wanneer een pakket is eerder hebt gekopieerd naar de installatiekopieopslag, wordt in de register-bewerking wordt het relatieve pad opgegeven in de installatiekopieopslag.

```powershell
PS C:\> Register-ServiceFabricApplicationType -ApplicationPathInImageStore MyApplicationV1
Register application type succeeded
```

"MyApplicationV1" is de map in de installatiekopieopslag waar het toepassingspakket zich bevindt. Het toepassingstype met de naam 'MyApplicationType' en '1.0.0' (beide zijn gevonden in het toepassingsmanifest)-versie is nu geregistreerd in het cluster.

### <a name="register-the-application-package-copied-to-an-external-store"></a>Registreren van het toepassingspakket gekopieerd naar een externe opslag
Beginnen met Service Fabric versie 6.1, ondersteunt het pakket downloaden vanaf een externe opslag inrichten. Het downloaden van de URI het pad naar vertegenwoordigt de [ `sfpkg` toepassingspakket](service-fabric-package-apps.md#create-an-sfpkg) waar het toepassingspakket kan worden gedownload met behulp van HTTP of HTTPS-protocol. Het pakket moet zijn eerder hebt geüpload naar deze externe locatie. De URI moet leestoegang toestaan om Service Fabric kan het bestand downloaden. De `sfpkg` bestand moet de extensie '.sfpkg' hebben. De bewerking voor het inrichten, moet de informatie van het type toepassing, zoals gevonden in het toepassingsmanifest bevatten.

```
PS C:\> Register-ServiceFabricApplicationType -ApplicationPackageDownloadUri "https://sftestresources.blob.core.windows.net:443/sfpkgholder/MyAppPackage.sfpkg" -ApplicationTypeName MyApp -ApplicationTypeVersion V1 -Async
```

De [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) opdracht retourneert alleen nadat het systeem heeft het toepassingspakket geregistreerd. Hoe lang inschrijving duurt is afhankelijk van de grootte en de inhoud van het toepassingspakket. Indien nodig, de **- TimeoutSec** parameter kan worden gebruikt om op te geven van een langere time-out (de standaardtime-out is 60 seconden).

Als u een grote toepassing verpakken of als u time-outs ondervindt, gebruikt u de **- asynchrone** parameter. De opdracht retourneert wanneer het cluster de opdracht register accepteert. De register-bewerking blijft indien nodig.
De [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) opdracht worden de versies van een toepassingstype en de status van de registratie. U kunt deze opdracht gebruiken om te bepalen wanneer de registratie is voltooid.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="remove-an-application-package-from-the-image-store"></a>Een toepassingspakket verwijderen uit de installatiekopieopslag
Als een pakket is gekopieerd naar de installatiekopieopslag, moet u het verwijderen van de tijdelijke locatie nadat de registratie van de toepassing is geslaagd. Toepassingspakketten verwijderen uit de installatiekopieopslag vrijgemaakt systeemresources. Niet-gebruikte toepassingspakketten disk-opslag verbruikt en leidt tot prestatieproblemen van toepassing.

```powershell
PS C:\>Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore MyApplicationV1
```

## <a name="create-the-application"></a>De toepassing maken
U kunt instantiëren van de toepassing in een versie van een toepassing die is geregistreerd met behulp van de [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet. De naam van elke toepassing moet beginnen met de *' fabric: "* -schema en moet uniek zijn voor elk exemplaar. Standaardservices die zijn gedefinieerd in het toepassingsmanifest van het type doeltoepassing worden ook gemaakt.

```powershell
PS C:\> New-ServiceFabricApplication fabric:/MyApp MyApplicationType 1.0.0

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationParameters  : {}
```
Meerdere exemplaren van een toepassing kunnen worden gemaakt voor een bepaalde versie van het type van een geregistreerde toepassing. Elk exemplaar geïsoleerd, met een eigen werk directory en het proces wordt uitgevoerd.

Om te zien die met de naam apps en services worden uitgevoerd in het cluster, voert u de [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication) en [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) cmdlets:

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
Wanneer een exemplaar van de toepassing niet meer nodig hebt is, kunt u permanent verwijderen deze met behulp van de naam van de [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) cmdlet. [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) worden alle services die deel uitmaken van de toepassing ook, permanent verwijderen van de status van alle service automatisch verwijderd. 

> [!WARNING]
> Met deze bewerking kan niet ongedaan worden gemaakt en de status van toepassing kan niet worden hersteld.

```powershell
PS C:\> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS C:\> Get-ServiceFabricApplication
```

## <a name="unregister-an-application-type"></a>Registratie van een toepassingstype
Wanneer een bepaalde versie van een toepassingstype niet meer nodig hebt is, moet u de toepassing met behulp van registratie de [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) cmdlet. Registratie van niet-gebruikte toepassingstypen versies opslagruimte die wordt gebruikt door de installatiekopieopslag door het verwijderen van de bestanden van het type toepassing. Registratie van een toepassingstype wordt het toepassingspakket gekopieerd naar de installatiekopie store tijdelijke locatie, niet verwijderd als kopiëren naar de installatiekopieopslag is gebruikt. Een toepassingstype kan ongedaan worden, zolang er zijn geen toepassingen worden geïnstantieerd tegen en niet in afwachting van toepassing upgrades verwijzen naar deze.

Voer [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) om te zien van de toepassingstypen die zijn geregistreerd in het cluster:

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

Voer [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) registratie van het type van een specifieke toepassing:

```powershell
PS C:\> Unregister-ServiceFabricApplicationType MyApplicationType 1.0.0
```

## <a name="troubleshooting"></a>Problemen oplossen
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopiëren-ServiceFabricApplicationPackage vraagt om een ImageStoreConnectionString
De Service Fabric-SDK-omgeving moet de juiste standaardwaarden instellen al hebben. Maar indien nodig, de ImageStoreConnectionString voor alle opdrachten moet overeenkomen met de waarde die door de Service Fabric-cluster wordt gebruikt. U kunt de ImageStoreConnectionString vinden in het clustermanifest opgehaald met de [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) en Get-ImageStoreConnectionStringFromClusterManifest opdrachten:

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

De **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet die deel van de Service Fabric SDK PowerShell-module uitmaakt, wordt gebruikt om op te halen van de verbindingsreeks van de installatiekopie-store.  Voor het importeren van de SDK-module, voert u de volgende uit:

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

Zie [inzicht in de verbindingsreeks van de installatiekopie-store](service-fabric-image-store-connection-string.md) verbindingsreeks voor aanvullende informatie over de installatiekopieopslag en de installatiekopie te slaan.

### <a name="deploy-large-application-package"></a>Grote toepassingspakket implementeren
Probleem: [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) er is een time-out opgetreden voor een grote toepassing-pakket (volgorde van GB).
Probeer:
- Geef een hogere time-out voor [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) opdracht met `TimeoutSec` parameter. De time-out is standaard 30 minuten.
- Controleer de netwerkverbinding tussen de broncomputer en de cluster. Als de verbinding langzaam is, kunt u overwegen een machine met een betere netwerkverbinding.
Als de client-computer in een andere regio dan het cluster, kunt u met behulp van een client-computer in een dichter of dezelfde regio als het cluster.
- Controleer als u externe beperking worden bereikt. Wanneer de installatiekopieopslag is geconfigureerd voor gebruik van azure-opslag, kunnen uploaden bijvoorbeeld kan worden beperkt.

Probleem: Upload-pakket is voltooid, maar [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) een time-out optreedt. Probeer:
- [Comprimeren van het pakket](service-fabric-package-apps.md#compress-a-package) voordat u kopieert naar de installatiekopieopslag.
De compressie vermindert de grootte en het aantal bestanden, die op zijn beurt wordt de hoeveelheid netwerkverkeer en werken die Service Fabric moet uitvoeren. De uploadbewerking mogelijk langzamer (met name als u de tijd compressie opnemen), maar het register en het toepassingstype opheffen van de registratie sneller zijn.
- Geef een hogere time-out voor [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) met `TimeoutSec` parameter.
- Geef `Async` overschakelen voor [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps). De opdracht retourneert wanneer het cluster de opdracht accepteert en de registratie van het toepassingstype asynchroon blijft. Daarom is er niet nodig om op te geven een hogere time-out in dit geval. De [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) opdracht geeft een lijst van alle versies van een toepassingstype is geregistreerd en hun registratiestatus. U kunt deze opdracht gebruiken om te bepalen wanneer de registratie is voltooid.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

### <a name="deploy-application-package-with-many-files"></a>Toepassingspakket implementeren met veel bestanden
Probleem: [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) time-out optreedt voor een toepassingspakket met veel bestanden (volgorde van duizenden).
Probeer:
- [Comprimeren van het pakket](service-fabric-package-apps.md#compress-a-package) voordat u kopieert naar de installatiekopieopslag. De compressie vermindert het aantal bestanden.
- Geef een hogere time-out voor [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) met `TimeoutSec` parameter.
- Geef `Async` overschakelen voor [registreren ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps). De opdracht retourneert wanneer het cluster de opdracht accepteert en de registratie van het toepassingstype asynchroon blijft.
Daarom is er niet nodig om op te geven een hogere time-out in dit geval. De [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) opdracht geeft een lijst van alle versies van een toepassingstype is geregistreerd en hun registratiestatus. U kunt deze opdracht gebruiken om te bepalen wanneer de registratie is voltooid.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="next-steps"></a>Volgende stappen
[Toepassingspakket maken](service-fabric-package-apps.md)

[Service Fabric-toepassingsupgrade](service-fabric-application-upgrade.md)

[Introductie van service Fabric-status](service-fabric-health-introduction.md)

[Problemen vaststellen en oplossen van een Service Fabric-service](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Een toepassing modelleren in Service Fabric](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-package-apps.md
[11]: service-fabric-application-upgrade.md
