---
title: Pakket van een Azure-Service Fabric-app | Microsoft Docs
description: Klik hier voor meer informatie over het inpakken van een Service Fabric-toepassing voorafgaand aan implementatie naar een cluster.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: mani-ramaswamy
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: atsenthi
ms.openlocfilehash: b8e66a9d5bba0c48f15b1ccd3f2d47e5405db792
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58791589"
---
# <a name="package-an-application"></a>Toepassingspakket maken

In dit artikel wordt beschreven hoe u een Service Fabric-toepassing verpakken en geef deze als gereed voor implementatie.

## <a name="package-layout"></a>Lay-out

Het toepassingsmanifest, een of meer servicemanifesten en andere benodigde pakket-bestanden moeten worden ingedeeld in een specifieke indeling voor implementatie in een Service Fabric-cluster. De manifesten voorbeeld in dit artikel moet worden ingedeeld in de volgende mapstructuur:

```
tree /f .\MyApplicationType
```

```Output
D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

De mappen zijn met de naam overeenkomt met de **naam** kenmerken van elke overeenkomstige element. Bijvoorbeeld, als het servicemanifest bevat twee pakketten met de namen van de **MyCodeA** en **MyCodeB**, en vervolgens twee mappen met dezelfde namen zou de vereiste binaire bestanden voor ieder codepakket bevatten.

## <a name="use-setupentrypoint"></a>Gebruik SetupEntryPoint

Typische scenario's voor het gebruik van **SetupEntryPoint** als u wilt uitvoeren van een uitvoerbaar bestand voordat u de service wordt gestart of moet u een bewerking met verhoogde bevoegdheden uit te voeren. Bijvoorbeeld:

* Instellen van en het initialiseren van omgevingsvariabelen die het uitvoerbare bestand van de service nodig heeft. Het is niet beperkt tot alleen uitvoerbare bestanden die zijn geschreven via de Service Fabric-programmeermodellen. Npm.exe moet bijvoorbeeld enkele omgevingsvariabelen die is geconfigureerd voor het implementeren van een node.js-toepassing.
* Toegangsbeheer instellen door het installeren van beveiligingscertificaten.

Voor meer informatie over het configureren van de **SetupEntryPoint**, Zie [het beleid voor een toegangspunt voor service-instellingen configureren](service-fabric-application-runas-security.md)

<a id="Package-App"></a>

## <a name="configure"></a>Configureren

### <a name="build-a-package-by-using-visual-studio"></a>Een pakket bouwen met behulp van Visual Studio

Als u Visual Studio 2015 gebruikt om uw toepassing te maken, kunt u de pakket-opdracht voor het automatisch maken van een pakket dat overeenkomt met de indeling die hierboven worden beschreven.

Voor het maken van een pakket met de rechtermuisknop op het toepassingsproject in Solution Explorer en kies de pakket-opdracht, zoals hieronder wordt weergegeven:

![Verpakking van een toepassing met Visual Studio][vs-package-command]

Wanneer het verpakken is voltooid, vindt u de locatie van het pakket in de **uitvoer** venster. De stap verpakking gebeurt automatisch wanneer u implementeert of fouten opsporen in uw toepassing in Visual Studio.

### <a name="build-a-package-by-command-line"></a>Het bouwen van een pakket met de opdrachtregel

Het is ook mogelijk via een programma verpakken uw toepassing met `msbuild.exe`. Achter de schermen, Visual Studio wordt uitgevoerd wordt, zodat de uitvoer hetzelfde is.

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-the-package"></a>Het pakket testen

U kunt de pakketstructuur lokaal via PowerShell controleren met behulp van de [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) opdracht.
Met deze opdracht wordt gecontroleerd op manifest parseren problemen en controleer of alle verwijzingen. Met deze opdracht wordt alleen gecontroleerd of de mappen en bestanden in het pakket structurele juist.
Het niet controleren of een van de inhoud van een pakket code of gegevens buiten de controleren of alle vereiste bestanden aanwezig zijn.

```powershell
Test-ServiceFabricApplicationPackage .\MyApplicationType
```

```Output
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

Deze fout ziet u dat de *MySetup.bat* bestand waarnaar wordt verwezen in het servicemanifest **SetupEntryPoint** ontbreekt in het codepakket. Nadat het ontbrekende bestand is toegevoegd, wordt de verificatie van de App wordt doorgegeven:

```
tree /f .\MyApplicationType
```

```Output
D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

```powershell
Test-ServiceFabricApplicationPackage .\MyApplicationType
```

```Output
True
```

Als uw toepassing heeft [toepassingsparameters](service-fabric-manage-multiple-environment-app-configuration.md) gedefinieerd, kunt u ze in doorgeven [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) voor validatie.

Als u bekend bent met het cluster waarop de toepassing wordt geïmplementeerd, is het raadzaam u doorgeeft in de `ImageStoreConnectionString` parameter. In dit geval wordt ook het pakket gevalideerd op basis van eerdere versies van de toepassing die al in het cluster worden uitgevoerd. Bijvoorbeeld: de validatie kan detecteren of een pakket met dezelfde versie, maar andere inhoud al is geïmplementeerd.  

Zodra de toepassing correct wordt geleverd en de validatietests doorstaat, houd rekening met het pakket voor snellere implementatiebewerkingen comprimeren.

## <a name="compress-a-package"></a>Een pakket comprimeren

Wanneer een pakket groot is of veel bestanden bevat, kunt u het comprimeren voor een snellere implementatie. Compressie vermindert het aantal bestanden en de grootte van het pakket.
Voor een gecomprimeerde toepassingspakket [uploaden van het toepassingspakket](service-fabric-deploy-remove-applications.md#upload-the-application-package) kan langer duren in vergelijking tot het uploaden van het niet-gecomprimeerde pakket, met name als compressie wordt gedaan als onderdeel van de kopie. Met compressie wordt [registreren](service-fabric-deploy-remove-applications.md#register-the-application-package) en [niet registreren het toepassingstype](service-fabric-deploy-remove-applications.md#unregister-an-application-type) sneller.

Het implementatiemechanisme voor is hetzelfde voor gecomprimeerde en niet-gecomprimeerde pakketten. Als het pakket is gecomprimeerd, wordt deze als zodanig worden opgeslagen in de installatiekopieopslag van het cluster en deze op het knooppunt wordt gecomprimeerd voordat de toepassing wordt uitgevoerd.
De compressie vervangt de geldig Service Fabric-pakket met de gecomprimeerde versie. De map moet schrijfmachtigingen toestaan. Compressie die wordt uitgevoerd op een reeds gecomprimeerde pakket levert geen wijzigingen.

U kunt een pakket met de Powershell-opdracht comprimeren [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) met `CompressPackage` overschakelen. U kunt het pakket met dezelfde decomprimeren opdracht, met behulp van `UncompressPackage` overschakelen.

De volgende opdracht wordt het pakket zonder te kopiëren naar de installatiekopieopslag gecomprimeerd. U kunt een gecomprimeerde pakket kopiëren naar een of meer Service Fabric-clusters, indien nodig, met behulp van [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) zonder de `SkipCopy` vlag.
Het pakket bevat nu ZIP-bestanden voor de `code`, `config`, en `data` pakketten. Manifest van de toepassing en de servicemanifesten zijn niet ingepakte, omdat ze nodig voor veel interne bewerkingen zijn wel. Bijvoorbeeld: package delen, toepassing de naam en versie extraheren voor bepaalde alle validaties nodig voor toegang tot de manifesten. Het inpakken van de manifesten zouden deze bewerkingen inefficiënt.

```
tree /f .\MyApplicationType
```

```Output
D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

```powershell
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -CompressPackage -SkipCopy
tree /f .\MyApplicationType
```

```Output
D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
       ServiceManifest.xml
       MyCode.zip
       MyConfig.zip
       MyData.zip

```

U kunt ook comprimeren en kopieer het pakket met [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) in één stap.
Als het pakket groot is, geeft u de time-out van een hoog genoeg zijn zodat er voldoende tijd voor het ondersteuningspakket niet comprimeren en voor het uploaden naar het cluster.

```powershell
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

Service Fabric berekent intern controlesommen voor de toepassingspakketten voor validatie. Bij het gebruik van compressie, worden de controlesommen worden berekend op de gecomprimeerde versies van elk pakket. Genereren van een nieuw zip van de dezelfde toepassingspakket maakt verschillende controlesommen. Gebruik ter voorkoming van validatiefouten [diff inrichting](service-fabric-application-upgrade-advanced.md). Met deze optie moet u niet de ongewijzigd pakketten opnemen in de nieuwe versie. In plaats daarvan ernaar te verwijzen rechtstreeks vanuit de nieuwe servicemanifest.

Als de diff-inrichting kan niet worden gebruikt en moet u de pakketten opnemen, genereert u nieuwe versies voor de `code`, `config`, en `data` pakketten om te voorkomen dat controlesom komt niet overeen. Genereren van nieuwe versies voor ongewijzigd pakketten is noodzakelijk wanneer een gecomprimeerde pakket wordt gebruikt, ongeacht of vorige versie compressie gebruikt.

Het pakket is nu correct worden verpakt, gevalideerd en gecomprimeerd (indien nodig), zodat deze gereed is voor [implementatie](service-fabric-deploy-remove-applications.md) naar een of meer Service Fabric-clusters.

### <a name="compress-packages-when-deploying-using-visual-studio"></a>Comprimeren van pakketten bij het implementeren met Visual Studio

Kunt u Visual Studio voor het comprimeren van pakketten op implementatie door toe te voegen opdracht geven de `CopyPackageParameters` element naar uw publicatieprofiel, en stel de `CompressPackage` kenmerk `true`.

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="create-an-sfpkg"></a>Een sfpkg maken

Vanaf versie 6.1, kunt Service Fabric inrichten vanuit een externe opslag.
Met deze optie geen het toepassingspakket moet worden gekopieerd naar de installatiekopieopslag. In plaats daarvan kunt u een `sfpkg` en dit uploaden naar een externe opslag en geef vervolgens de download-URI in Service Fabric bij het inrichten. Hetzelfde pakket kan worden ingericht met meerdere clusters. De tijd die nodig is om te kopiëren van het pakket naar elk cluster inrichten vanuit externe opslag worden opgeslagen.

De `sfpkg` bestand is een zip die de eerste toepassingspakket bevat en de extensie '.sfpkg' heeft.
In het ZIP-bestand, kan het toepassingspakket worden wel of niet gecomprimeerd. De compressie van het toepassingspakket in het ZIP-bestand wordt uitgevoerd op de code, configuratie en gegevens pakket niveaus, als [eerder genoemde](service-fabric-package-apps.md#compress-a-package).

Maakt een `sfpkg`, beginnen met een map met het oorspronkelijke toepassingspakket, gecomprimeerd of niet. Vervolgens gebruikt u een hulpprogramma naar het zip-de map met de extensie '.sfpkg'. Gebruik bijvoorbeeld [ZipFile.CreateFromDirectory](https://msdn.microsoft.com/library/hh485721(v=vs.110).aspx).

```csharp
ZipFile.CreateFromDirectory(appPackageDirectoryPath, sfpkgFilePath);
```

De `sfpkg` moet worden geüpload naar de externe opslag buiten-band buiten een Service Fabric. De externe opslag kan een archief dat wordt aangegeven dat een REST-eindpunt voor http of https zijn. Tijdens het inrichten, Service Fabric voert een GET-bewerking voor het downloaden van de `sfpkg` toepassingspakket, zodat de store leestoegang voor het pakket moet toestaan.

Gebruik voor het inrichten van het pakket, externe inrichten, waarvoor de URI voor downloaden en informatie over de toepassing-type is vereist.

>[!NOTE]
> Inrichting op basis van het relatieve pad naar afbeelding store ondersteunt momenteel geen `sfpkg` bestanden. Daarom de `sfpkg` moet niet worden gekopieerd naar de installatiekopieopslag.

## <a name="next-steps"></a>Volgende stappen

[Implementeren en verwijderen van toepassingen] [ 10] wordt beschreven hoe u PowerShell gebruiken voor het beheren van instanties van een toepassing

[Parameters voor de toepassing voor meerdere omgevingen beheren] [ 11] wordt beschreven hoe u parameters en variabelen voor exemplaren van een andere toepassing.

[Beveiligingsbeleid configureren voor uw toepassing] [ 12] wordt beschreven hoe u services onder het beveiligingsbeleid om toegang te beperken uit te voeren.

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md