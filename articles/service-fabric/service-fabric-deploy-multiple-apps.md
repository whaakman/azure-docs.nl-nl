---
title: Een Node.js-toepassing die gebruikmaakt van MongoDB implementeren | Microsoft Docs
description: Overzicht over het pakket meerdere Gast uitvoerbare bestanden naar een Azure Service Fabric-cluster implementeren
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: b76bb756-c1ba-49f9-9666-e9807cf8f92f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: msfussell;mikhegn
ms.openlocfilehash: d7a37d7c04f85e9031cab52fa86026e56315c882
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="deploy-multiple-guest-executables"></a>Meerdere toepassingen implementeren die door gasten kunnen worden uitgevoerd
In dit artikel laat zien hoe verpakken en distribueren van meerdere Gast uitvoerbare bestanden naar de Azure Service Fabric. Voor het maken en implementeren van één Service Fabric-pakket leest u hoe aan [uitvoerbare Gast implementeren op Service Fabric](service-fabric-deploy-existing-app.md).

Terwijl dit overzicht hoe een toepassing met een Node.js-front-end dat u MongoDB als het gegevensarchief gebruikt implementeren toont, kunt u de stappen toepassen op alle toepassingen die afhankelijk van een andere toepassing is.   

U kunt Visual Studio gebruiken voor het produceren van het toepassingspakket dat meerdere Gast uitvoerbare bestanden bevat. Zie [met behulp van Visual Studio aan het pakket van een bestaande toepassing](service-fabric-deploy-existing-app.md). Nadat u het eerste uitvoerbare bestand van het gastbesturingssysteem hebt toegevoegd, klik met de rechtermuisknop op het toepassingsproject en selecteer de **toevoegen -> nieuwe Service Fabric-service** het tweede Gast uitvoerbare project toevoegen aan de oplossing. Opmerking: Als u de bron is in de Visual Studio-project koppelt, de Visual Studio-oplossing bouwen zorgt ervoor dat het toepassingspakket bijgewerkt met wijzigingen in de bron is. 

## <a name="samples"></a>Voorbeelden
* [Voorbeeld voor verpakken en distribueren van een gast uitvoerbaar bestand](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Voorbeeld van twee Gast uitvoerbare bestanden (C# en nodejs) communiceren via de Naming service met behulp van REST](https://github.com/Azure-Samples/service-fabric-containers)

## <a name="manually-package-the-multiple-guest-executable-application"></a>De meerdere Gast uitvoerbare toepassing handmatig van het pakket
U kunt ook kunt u handmatig de Gast uitvoerbare pakket. Voor de handmatige verpakking, het hulpprogramma Service Fabric-pakket, dat beschikbaar is op dit artikel wordt gebruikt [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).

### <a name="packaging-the-nodejs-application"></a>Het verpakken van de Node.js-toepassing
In dit artikel wordt ervan uitgegaan dat Node.js niet is geïnstalleerd op de knooppunten in het Service Fabric-cluster. Als gevolg hiervan moet u Node.exe toevoegen aan de hoofdmap van uw knooppunttoepassing voordat de pakketten. De mapstructuur van de Node.js-toepassing (met behulp van de Express-webframework en Jade sjabloon engine), moet er ongeveer als hieronder:

```
|-- NodeApplication
    |-- bin
        |-- www
    |-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
    |-- public
        |-- images
        |-- etc.
    |-- routes
        |-- index.js
        |-- users.js
    |-- views
        |-- index.jade
        |-- etc.
    |-- app.js
    |-- package.json
    |-- node.exe
```

Een volgende stap moet u een pakket voor de Node.js-toepassing maken. De code hieronder maakt een Service Fabric-toepassingspakket dat u de Node.js-toepassing bevat.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Hieronder volgt een beschrijving van de parameters die worden gebruikt:

* **/ source** verwijst naar de map van de toepassing die moet worden verpakt.
* **/ target** definieert u de map waarin het pakket moet worden gemaakt. Deze map moet afwijken van de bronmap.
* **/ AppName** definieert de naam van de toepassing van de bestaande toepassing. Het is belangrijk om te begrijpen dat dit komt neer op de servicenaam in het manifest en niet op de naam van de Service Fabric-toepassing.
* **exe** definieert het uitvoerbare bestand dat de Service Fabric behoort te starten in dit geval `node.exe`.
* **/ma** het argument dat wordt gebruikt voor het starten van het uitvoerbare bestand definieert. Als Node.js niet is geïnstalleerd, Service Fabric moet de Node.js-webserver start door te voeren `node.exe bin/www`.  `/ma:'bin/www'`vertelt verpakking hulpprogramma gebruikt `bin/www` als het argument voor node.exe.
* **/ AppType** definieert de naam van het Service Fabric-toepassing.

Als u de map die is opgegeven in de parameter/target bladert, ziet u dat het hulpprogramma een volledig werkend Service Fabric-pakket is gemaakt, zoals hieronder wordt weergegeven:

```
|--[yourtargetdirectory]
    |-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
De gegenereerde ServiceManifest.xml heeft nu een sectie waarin wordt beschreven hoe de Node.js-web-server moet worden gestart, zoals wordt weergegeven in het onderstaande codefragment:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
In dit voorbeeld luistert de Node.js-webserver op poort 3000, dus moet u het bijwerken van de endpoint-informatie in het bestand ServiceManifest.xml zoals hieronder wordt weergegeven.   

```xml
<Resources>
      <Endpoints>
         <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>Het verpakken van de MongoDB-toepassing
Nu dat u de Node.js-toepassing ingepakt hebt, kunt u doorgaan en MongoDB-pakket. Zoals al eerder vermeld, zijn de stappen die u hebt nu doorlopen niet specifiek zijn voor Node.js en MongoDB. In feite ze gelden voor alle toepassingen die zijn bedoeld om samen als één Service Fabric-toepassing worden verpakt.  

Als u wilt inpakken MongoDB, die u wilt Zorg ervoor dat u het pakket Mongod.exe en Mongo.exe. Beide binaire bestanden bevinden zich in de `bin` map van de installatiemap van MongoDB. De mapstructuur lijkt op hieronder.

```
|-- MongoDB
    |-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Service Fabric moet MongoDB beginnen met een opdracht vergelijkbaar met die hieronder, zodat u wilt gebruiken, de `/ma` parameter door bij het verpakken van MongoDB.

```
mongod.exe --dbpath [path to data]
```
> [!NOTE]
> De gegevens is niet het geval van storing op een knooppunt wordt behouden als u de MongoDB-gegevensmap op de lokale map van het knooppunt geplaatst. U moet duurzame opslag gebruiken of implementeren van een replicaset MongoDB om gegevensverlies te voorkomen.  
>
>

In PowerShell of de opdrachtshell uitvoeren we het verpakking-hulpprogramma met de volgende parameters:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

U moet MongoDB toevoegen aan uw Service Fabric-toepassingspakket, zorg ervoor dat de parameter/target naar dezelfde directory waarmee al de toepassing bevat verwijst samen met de Node.js-toepassing manifest. U moet ook om ervoor te zorgen dat u van dezelfde ApplicationType naam gebruikmaakt.

Laten we Blader naar de map en onderzoeken van wat het hulpprogramma heeft gemaakt.

```
|--[yourtargetdirectory]
    |-- MyNodeApplication
    |-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
Zoals u ziet, wordt met het hulpprogramma een nieuwe map, MongoDB, toegevoegd aan de map waarin de binaire bestanden voor MongoDB. Als u opent de `ApplicationManifest.xml` -bestand, kunt u zien dat het pakket nu de Node.js-toepassing en de MongoDB bevat. De volgende code toont de inhoud van het toepassingsmanifest.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-the-application"></a>De toepassing publiceren
De laatste stap is het publiceren van de toepassing naar de lokale Service Fabric-cluster met behulp van de onderstaande PowerShell-scripts:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Zodra de toepassing is gepubliceerd naar het lokale cluster, kunt u de Node.js-toepassing op de poort die we hebben ingevoerd in het servicemanifest van de Node.js-toepassing bijvoorbeeld http://localhost: 3000 openen.

In deze zelfstudie hebt u gezien hoe eenvoudig het verpakken van twee bestaande toepassingen als een Service Fabric-toepassing. U hebt ook geleerd implementeren naar Service Fabric, zodat deze van enkele van de Service Fabric-functies, zoals hoge beschikbaarheid en health systeemintegratie profiteren kan.


## <a name="adding-more-guest-executables-to-an-existing-application-using-yeoman-on-linux"></a>Meer Gast uitvoerbare bestanden toe te voegen aan een bestaande toepassing Yeoman met op Linux

Voer de volgende stappen uit als u nog een service wilt toevoegen aan een toepassing die al is gemaakt met `yo`: 
1. Stel de directory in op de hoofdmap van de bestaande toepassing.  Bijvoorbeeld `cd ~/YeomanSamples/MyApplication` als `MyApplication` de toepassing is die is gemaakt door Yeoman.
2. Voer `yo azuresfguest:AddService` en geef de benodigde gegevens.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het implementeren van containers met [overzicht van Service Fabric en containers](service-fabric-containers-overview.md)
* [Voorbeeld voor verpakken en distribueren van een gast uitvoerbaar bestand](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Voorbeeld van twee Gast uitvoerbare bestanden (C# en nodejs) communiceren via de Naming service met behulp van REST](https://github.com/Azure-Samples/service-fabric-containers)
