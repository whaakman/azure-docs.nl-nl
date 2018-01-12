---
title: Pakket van een Azure-Service Fabric-app | Microsoft Docs
description: Het pakket een Service Fabric-toepassing voordat u implementeert naar een cluster.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: 93c86f4805257aee8e04ef80e33b3cec0fd3c67d
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="package-an-application"></a>Een toepassing van het pakket
In dit artikel wordt beschreven hoe een Service Fabric-toepassing van het pakket en gereed is voor implementatie te maken.

## <a name="package-layout"></a>Lay-out
Het toepassingsmanifest, een of meer service-manifesten en andere benodigde pakket-bestanden moeten worden ingedeeld in een specifieke indeling voor implementatie in een Service Fabric-cluster. De manifesten voorbeeld in dit artikel moet worden ingedeeld in de volgende mapstructuur:

```
PS D:\temp> tree /f .\MyApplicationType

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

De mappen zijn met de naam overeenkomt met de **naam** kenmerken van elk element in de bijbehorende. Als de servicemanifest bevat twee code pakketten met de namen bijvoorbeeld **MyCodeA** en **MyCodeB**, en vervolgens twee mappen met dezelfde namen van de benodigde binaire bestanden voor elk codepakket zou bevatten.

## <a name="use-setupentrypoint"></a>Entrypoint gebruiken
Typische scenario's voor het gebruik van **entrypoint** wanneer moet u een uitvoerbaar bestand uitvoeren voordat de service wordt gestart of moet u een bewerking met verhoogde bevoegdheden uit te voeren. Bijvoorbeeld:

* In te stellen en het initialiseren van omgevingsvariabelen op die moet van het uitvoerbare bestand van de service. Het is niet beperkt tot alleen uitvoerbare bestanden die zijn geschreven via de Service Fabric-programmeermodellen. Npm.exe moet bijvoorbeeld een aantal omgevingsvariabelen die is geconfigureerd voor het implementeren van een node.js-toepassing.
* Instellen van toegangsbeheer door beveiligingscertificaten te installeren.

Voor meer informatie over het configureren van de **entrypoint**, Zie [het beleid voor een toegangspunt voor service-instellingen configureren](service-fabric-application-runas-security.md)

<a id="Package-App"></a>
## <a name="configure"></a>Configureren
### <a name="build-a-package-by-using-visual-studio"></a>Het bouwen van een pakket met behulp van Visual Studio
Als u Visual Studio 2015 gebruikt om uw toepassing te maken, kunt u de pakket-opdracht voor het automatisch maken van een pakket dat overeenkomt met de indeling van de hierboven beschreven.

Voor het maken van een pakket met de rechtermuisknop op de application-project in Solution Explorer en kiest u de opdracht pakket, zoals hieronder wordt weergegeven:

![Een toepassing met Visual Studio-pakket][vs-package-command]

Wanneer het pakket is voltooid, kunt u de locatie van het pakket in vinden de **uitvoer** venster. De stap pakket worden automatisch uitgevoerd wanneer u implementeert of opsporen in uw toepassing in Visual Studio.

### <a name="build-a-package-by-command-line"></a>Het bouwen van een pakket met de opdrachtregel
Het is ook mogelijk via een programma wordt gebruikt door uw toepassing verpakken `msbuild.exe`. Achter de schermen Visual Studio wordt uitgevoerd het zodat de uitvoer hetzelfde is.

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-the-package"></a>Het pakket testen
U kunt de pakketstructuur lokaal via PowerShell controleren met behulp van de [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) opdracht.
Met deze opdracht controleert manifest bij het parseren van problemen en controleer of alle verwijzingen. Met deze opdracht controleert alleen of de structurele juistheid van de mappen en bestanden in het pakket.
Het niet controleren of een van de pakketinhoud code of gegevens buiten de controleren of alle vereiste bestanden aanwezig zijn.

```
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

Deze fout ziet u dat de *MySetup.bat* bestand waarnaar wordt verwezen in het servicemanifest **entrypoint** ontbreekt in het codepakket. Nadat het ontbrekende bestand is toegevoegd, wordt de controle van toepassingen wordt doorgegeven:

```
PS D:\temp> tree /f .\MyApplicationType

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

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
```

Als uw toepassing [toepassingsparameters](service-fabric-manage-multiple-environment-app-configuration.md) gedefinieerd, kunt u ze in doorgeven [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) voor de juiste validatie.

Als u het cluster waarop de toepassing wordt geïmplementeerd, kunt u doorgeeft in de `ImageStoreConnectionString` parameter. Het pakket wordt in dit geval ook gevalideerd met eerdere versies van de toepassing die al in het cluster worden uitgevoerd. Bijvoorbeeld: de validatie kan detecteren of een pakket met dezelfde versie maar verschillende inhoud al is geïmplementeerd.  

Als de toepassing correct wordt verpakt en is gevalideerd, overweeg het comprimeren van het pakket voor snellere implementatiebewerkingen.

## <a name="compress-a-package"></a>Een pakket comprimeren
Wanneer een pakket grote of veel bestanden heeft, kunt u het comprimeren voor een snellere implementatie. Compressie vermindert het aantal bestanden en de grootte van het pakket.
Voor een gecomprimeerde toepassingspakket [uploaden van het toepassingspakket](service-fabric-deploy-remove-applications.md#upload-the-application-package) kan het langer duren vergeleken met het uploaden van de niet-gecomprimeerde pakket, met name als compressie is uitgevoerd als onderdeel van de kopie. Met compressie, [registreren](service-fabric-deploy-remove-applications.md#register-the-application-package) en [afmelden het toepassingstype](service-fabric-deploy-remove-applications.md#unregister-an-application-type) sneller.

Het mechanisme deployment is hetzelfde voor gecomprimeerde en ongecomprimeerde pakketten. Als het pakket is gecomprimeerd, wordt deze als zodanig in het archief van de cluster-installatiekopie is opgeslagen en wordt deze op het knooppunt wordt gedecomprimeerd voordat de toepassing wordt uitgevoerd.
De compressie vervangt het geldig Service Fabric-pakket met de gecomprimeerde versie. De map moet schrijfmachtigingen toestaan. Compressie uitgevoerd op een al gecomprimeerde pakket levert geen wijzigingen aangebracht.

U kunt een pakket met de Powershell-opdracht comprimeren [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) met `CompressPackage` overschakelen. U kunt het pakket met dezelfde decomprimeren opdracht met behulp `UncompressPackage` overschakelen.

De volgende opdracht wordt het pakket gecomprimeerd zonder kopiëren naar de image store. U kunt een gecomprimeerde pakket kopiëren naar een of meer Service Fabric-clusters, indien nodig, met behulp van [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) zonder de `SkipCopy` vlag.
Het pakket bevat nu ZIP-bestanden voor de `code`, `config`, en `data` pakketten. Het toepassingsmanifest en de manifesten voor de service zijn niet ingepakte, omdat ze nodig zijn voor veel interne bewerkingen. Bijvoorbeeld, het delen van het pakket, de toepassing naam en versie extraheren nodig voor bepaalde alle validaties toegang tot de manifesten. Comprimeren van de manifesten zouden deze bewerkingen niet efficiënt.

```
PS D:\temp> tree /f .\MyApplicationType

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
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -CompressPackage -SkipCopy

PS D:\temp> tree /f .\MyApplicationType

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
Als het pakket groot is, geeft u een hoog genoeg time-out zodat de tijd voor de compressie van het pakket en voor het uploaden naar het cluster.
```
PS D:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

Service Fabric berekent intern controlesommen voor de toepassingspakketten voor validatie. Bij gebruik van compressie, worden de controlesommen berekend op de gecomprimeerde versies van elk pakket. Genereren van een nieuwe zip van het toepassingspakket met dezelfde maakt verschillende controlesommen. Om validatiefouten te voorkomen, gebruikt u [diff inrichting](service-fabric-application-upgrade-advanced.md). Met deze optie niet opnemen de ongewijzigde pakketten in de nieuwe versie. In plaats daarvan verwijzing rechtstreeks naar de nieuwe servicemanifest.

Als de diff-inrichting kan niet worden gebruikt en moet u de pakketten, genereren nieuwe versies voor de `code`, `config`, en `data` pakketten om te voorkomen dat checksum komt niet overeen. Genereren van nieuwe versies voor ongewijzigd pakketten is nodig wanneer een gecomprimeerde pakket wordt gebruikt, ongeacht het feit of eerdere versie compressie gebruikt.

Het pakket is nu correct verpakt, gevalideerd en gecomprimeerd (indien nodig), zodat deze gereed is voor [implementatie](service-fabric-deploy-remove-applications.md) op een of meer Service Fabric-clusters.

### <a name="compress-packages-when-deploying-using-visual-studio"></a>Comprimeren van pakketten bij het implementeren met Visual Studio
U kunt opgeven dat Visual Studio te comprimeren van pakketten op implementatie door toe te voegen de `CopyPackageParameters` element dat u wilt uw publicatieprofiel en stel de `CompressPackage` kenmerk `true`.

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="create-an-sfpkg"></a>Een sfpkg maken
Vanaf versie 6.1, staat Service Fabric inrichting van een externe winkel.
Met deze optie geen het toepassingspakket moet worden gekopieerd naar de image store. In plaats daarvan kunt u een `sfpkg` en upload het naar een externe winkel en geef vervolgens de download-URI naar Service Fabric bij het inrichten. Hetzelfde pakket kan worden ingericht met meerdere clusters. Inrichting van de externe winkel, slaat de benodigde tijd voor het pakket kopiëren naar een cluster.

De `sfpkg` bestand is een zip dat het eerste toepassingspakket bevat en de extensie '.sfpkg' heeft.
In het ZIP-bestand, kan het toepassingspakket worden wel of niet gecomprimeerd. De compressie van het toepassingspakket in het ZIP-bestand wordt uitgevoerd op code, configuratie en gegevens pakket niveaus, als [eerder genoemde](service-fabric-package-apps.md#compress-a-package).

Maken van een `sfpkg`, beginnen met een map met het oorspronkelijke toepassingspakket of niet gecomprimeerd. Vervolgens gebruikt u een hulpprogramma naar de map met de extensie '.sfpkg' zip. Bijvoorbeeld: [ZipFile.CreateFromDirectory](https://msdn.microsoft.com/library/hh485721(v=vs.110).aspx).

```csharp
ZipFile.CreateFromDirectory(appPackageDirectoryPath, sfpkgFilePath);
```

De `sfpkg` moet worden geüpload naar de externe store buiten band zijn, buiten de Service Fabric. Het externe archief kan een archief dat toegang biedt tot een REST-eindpunt voor http of https zijn. Tijdens het inrichten, Service Fabric wordt uitgevoerd een GET-bewerking voor het downloaden van de `sfpkg` toepassingspakket, zodat de store leestoegang voor het pakket moet toestaan.

Gebruik externe inrichten, waarvoor de download-URI en informatie over de toepassing-type is vereist voor het inrichten van het pakket.

>[!NOTE]
> Inrichting op basis van het relatieve pad naar afbeelding store ondersteunt momenteel geen `sfpkg` bestanden. Daarom de `sfpkg` niet worden gekopieerd naar de image store.

## <a name="next-steps"></a>Volgende stappen
[Implementeren en toepassingen verwijderen] [ 10] wordt beschreven hoe u PowerShell gebruiken voor het beheren van toepassingsexemplaren

[Het beheren van parameters voor de toepassing voor omgevingen met meerdere] [ 11] wordt beschreven hoe u parameters en variabelen voor exemplaren van een andere toepassing.

[Beveiligingsbeleid voor uw toepassing configureert] [ 12] wordt beschreven hoe u services onder het beveiligingsbeleid om toegang te beperken.

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
