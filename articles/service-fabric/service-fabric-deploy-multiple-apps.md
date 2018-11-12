---
title: Een Node.js-toepassing die gebruikmaakt van MongoDB naar Azure Service Fabric implementeren | Microsoft Docs
description: Overzicht van hoe u meerdere gastbestanden pakket implementeren op een Azure Service Fabric-cluster
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: ''
ms.assetid: b76bb756-c1ba-49f9-9666-e9807cf8f92f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: ea2f27069ca445a4d74ddc634f5c396ab13564a1
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51248993"
---
# <a name="deploy-multiple-guest-executables"></a>Meerdere toepassingen implementeren die door gasten kunnen worden uitgevoerd
In dit artikel laat zien hoe verpakken en implementeren van meerdere gastbestanden Azure Service fabric. Lees hoe voor het bouwen en implementeren van een enkele Service Fabric-pakket naar [een door gasten uitvoerbare bestanden implementeren naar Service Fabric](service-fabric-deploy-existing-app.md).

Tijdens deze procedure ziet hoe u een toepassing implementeren met een Node.js-front-end die MongoDB als gegevensopslag gebruikt, kunt u de stappen toepassen op alle toepassingen die afhankelijk van een andere toepassing is.   

U kunt Visual Studio gebruiken voor het produceren van het toepassingspakket met meerdere gastbestanden. Zie [met behulp van Visual Studio op een bestaande toepassing verpakken](service-fabric-deploy-existing-app.md). Nadat u de eerste Gast kan worden uitgevoerd hebt toegevoegd, klik met de rechtermuisknop op het toepassingsproject en selecteer de **Add -> Nieuw Service Fabric-service** het tweede Gast uitvoerbare project toevoegen aan de oplossing. Opmerking: Als u ervoor kiest om te koppelen van de bron in het Visual Studio-project, het bouwen van de Visual Studio-oplossing, zorgt u ervoor dat het toepassingspakket bijgewerkt met wijzigingen in de bron is. 

## <a name="samples"></a>Voorbeelden
* [Voorbeeld voor het verpakken en implementeren van een Gast kan worden uitgevoerd](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Voorbeeld van twee Gast uitvoerbare bestanden (C# en nodejs) communiceren via de Naming-service met behulp van REST](https://github.com/Azure-Samples/service-fabric-containers)

## <a name="manually-package-the-multiple-guest-executable-application"></a>Handmatig de meerdere door Gast uitvoerbare toepassing verpakken
U kunt ook handmatig de Gast uitvoerbare verpakken. Voor de handmatige verpakking, in dit artikel wordt het hulpprogramma voor het verpakken van Service Fabric, dat beschikbaar is op [ http://aka.ms/servicefabricpacktool ](https://aka.ms/servicefabricpacktool).

### <a name="packaging-the-nodejs-application"></a>De Node.js-toepassing Inpakken
In dit artikel wordt ervan uitgegaan dat Node.js niet is geïnstalleerd op de knooppunten in het Service Fabric-cluster. Als gevolg hiervan moet u Node.exe naar de hoofdmap van uw knooppunttoepassing voorafgaand aan pakketten toevoegen. De mapstructuur van de Node.js-toepassing (met behulp van de Express-webstructuur en Jade sjabloon-engine) ziet er ongeveer zo uit:

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

Als de volgende stap maakt u een toepassingspakket voor de Node.js-toepassing. De code hieronder maakt u een Service Fabric-toepassingspakket met de Node.js-toepassing.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Hieronder volgt een beschrijving van de parameters die worden gebruikt:

* **/ source** verwijst naar de map van de toepassing die moet worden verpakt.
* **/ doel** definieert de map waarin het pakket moet worden gemaakt. Deze map moet afwijken van de bronmap.
* **/ AppName** definieert de naam van de toepassing van de bestaande toepassing. Het is belangrijk om te begrijpen dat dit wordt omgezet naar de naam van de service in het manifest, en niet naar de naam van de Service Fabric-toepassing.
* **exe** definieert het uitvoerbare bestand dat Service Fabric om te starten in dit geval `node.exe`.
* **/ma** definieert het argument dat wordt gebruikt voor het starten van het uitvoerbare bestand. Als u Node.js niet is geïnstalleerd, Service Fabric moet de Node.js-web-server starten door het uitvoeren van `node.exe bin/www`.  `/ma:'bin/www'` Geeft het hulpprogramma verpakking gebruiken `bin/www` als het argument voor node.exe.
* **/ AppType** definieert de naam van het Service Fabric-toepassing.

Als u naar de map die is opgegeven in de parameter/target bladert, ziet u dat het hulpprogramma een volledig functionerende Service Fabric-pakket heeft gemaakt, zoals hieronder weergegeven:

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
In dit voorbeeld luistert de Node.js-web-server op poort 3000, dus u bijwerken van de informatie over het eindpunt in het bestand ServiceManifest.xml moet zoals hieronder wordt weergegeven.   

```xml
<Resources>
      <Endpoints>
         <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>Verpakking van de MongoDB-toepassing
Nu dat u de Node.js-toepassing verpakt hebt, kunt u direct verder gaan en MongoDB pakket. Zoals al eerder vermeld, zijn de stappen die u hebt nu doorlopen niet specifiek zijn voor Node.js en MongoDB. In feite ze van toepassing op alle toepassingen die zijn bedoeld om te samen zijn verpakt als een Service Fabric-toepassing.  

Voor het pakket MongoDB, die u wilt controleren of dat u een pakket Mongod.exe en Mongo.exe. Beide binaire bestanden bevinden zich in de `bin` map van uw MongoDB-installatiemap. De mapstructuur lijkt op hieronder wordt weergegeven.

```
|-- MongoDB
    |-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Service Fabric moet MongoDB beginnen met een vergelijkbaar met de opdracht hieronder, dus u moet gebruiken de `/ma` parameter bij het verpakken van MongoDB.

```
mongod.exe --dbpath [path to data]
```
> [!NOTE]
> De gegevens is niet in het geval van storing op een knooppunt wordt behouden als u de map van de MongoDB-gegevens op de lokale map van het knooppunt plaatst. U moet gebruiken duurzame opslag of implementeren van een MongoDB-replica instellen om te voorkomen dat gegevens verloren gaan.  
>
>

In PowerShell of de opdrachtshell we de verpakking-hulpprogramma's uitvoeren met de volgende parameters:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

Als u wilt toevoegen van MongoDB naar uw Service Fabric-toepassingspakket, moet u om ervoor te zorgen dat de punten van de parameter/target naar dezelfde map dat al de toepassing bevat het manifest, samen met de Node.js-toepassing. U moet ook om ervoor te zorgen dat u van dezelfde ApplicationType naam gebruikmaakt.

We gaan bladeren naar de map en bekijk wat het hulpprogramma heeft gemaakt.

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
Zoals u ziet, wordt er door het hulpprogramma een nieuwe map, MongoDB, toegevoegd aan de map waarin de binaire bestanden voor MongoDB. Als u opent de `ApplicationManifest.xml` -bestand, kunt u zien dat het pakket nu de Node.js-toepassing en de MongoDB bevat. De onderstaande code toont de inhoud van het toepassingsmanifest.

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

Zodra de toepassing is op het lokale cluster wordt gepubliceerd, kunt u toegang tot de Node.js-toepassing op de poort die we hebben opgegeven in het servicemanifest van de Node.js-toepassing--bijvoorbeeld http://localhost:3000.

In deze zelfstudie hebt u gezien hoe u gemakkelijk pakket twee bestaande toepassingen als een Service Fabric-toepassing. U hebt ook geleerd hoe u in Service Fabric implementeert, zodat deze van enkele van de Service Fabric-functies, zoals hoge beschikbaarheid en de gezondheid van systeemintegratie profiteren kan.


## <a name="adding-more-guest-executables-to-an-existing-application-using-yeoman-on-linux"></a>Meer gasten uitvoerbare bestanden toe te voegen aan een bestaande toepassing in Linux met behulp van Yeoman

Voer de volgende stappen uit als u nog een service wilt toevoegen aan een toepassing die al is gemaakt met `yo`: 
1. Stel de directory in op de hoofdmap van de bestaande toepassing.  Bijvoorbeeld `cd ~/YeomanSamples/MyApplication` als `MyApplication` de toepassing is die is gemaakt door Yeoman.
2. Voer `yo azuresfguest:AddService` en geef de details op die nodig zijn.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het implementeren van containers met [overzicht van Service Fabric en containers](service-fabric-containers-overview.md)
* [Voorbeeld voor het verpakken en implementeren van een Gast kan worden uitgevoerd](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Voorbeeld van twee Gast uitvoerbare bestanden (C# en nodejs) communiceren via de Naming-service met behulp van REST](https://github.com/Azure-Samples/service-fabric-containers)
