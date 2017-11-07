---
title: Implementeren van een bestaand uitvoerbaar bestand op Azure Service Fabric | Microsoft Docs
description: "Overzicht over het inpakken van een bestaande toepassing als gast uitvoerbare, zodat het kan worden geïmplementeerd naar een Service Fabric-cluster"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/02/2017
ms.author: mfussell;mikhegn
ms.openlocfilehash: c851e1f756957d58d5f7372098620e4b7129b089
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="deploy-a-guest-executable-to-service-fabric"></a>Uitvoerbare Gast implementeren op Service Fabric
Als een service kunt u verschillende typen code, zoals Node.js, Java of C++ uitvoeren in Azure Service Fabric. Service Fabric verwijst naar deze soorten services als gast uitvoerbare bestanden.

Gast uitvoerbare bestanden worden behandeld door het Service Fabric zoals stateless services. Als gevolg hiervan worden deze geplaatst op knooppunten in een cluster, op basis van beschikbaarheid en andere metrische gegevens. In dit artikel wordt beschreven hoe het pakket en een gast uitvoerbare aan een Service Fabric-cluster implementeren met behulp van Visual Studio of een opdrachtregelprogramma.

In dit artikel komen we de stappen voor het pakket uitvoerbare Gast en deze implementeren in Service Fabric.  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Voordelen van het uitvoeren van een gast uitvoerbare in Service Fabric
Er zijn enkele voordelen voor het uitvoeren van een gast uitvoerbare in een Service Fabric-cluster:

* Hoge beschikbaarheid. Toepassingen die worden uitgevoerd in Service Fabric zijn maximaal beschikbaar is gemaakt. Service Fabric zorgt ervoor dat de exemplaren van een toepassing worden uitgevoerd.
* Statuscontrole. Service Fabric-statuscontrole detecteert als een toepassing wordt uitgevoerd, en bevat de diagnostische gegevens als er een storing optreedt.   
* Levenscyclus van Toepassingsbeheer. Service Fabric biedt automatisch herstel naar de vorige versie naast het bieden van upgrades zonder uitvaltijd, als er een ongeldige statusgebeurtenis gerapporteerd tijdens een upgrade.    
* Dichtheid. U kunt meerdere toepassingen uitvoeren in een cluster die niet meer nodig voor elke toepassing uitvoeren op een eigen hardware.
* U kunt de naamgeving van Service Fabric-service om andere services in het cluster zichtbaarheid: Met behulp van REST aanroepen. 

## <a name="samples"></a>Voorbeelden
* [Voorbeeld voor verpakken en distribueren van een gast uitvoerbaar bestand](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Voorbeeld van twee Gast uitvoerbare bestanden (C# en nodejs) communiceren via de Naming service met behulp van REST](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a>Overzicht van de toepassing en service manifest-bestanden
Als onderdeel van de implementatie van een gast uitvoerbaar bestand is het nuttig om het Service Fabric-model voor pakketten en implementatie begrijpen, zoals beschreven in [toepassingsmodel](service-fabric-application-model.md). Het model van Service Fabric-pakket is afhankelijk van de twee XML-bestanden: de toepassing en service manifesten. De schemadefinitie voor de bestanden ApplicationManifest.xml en ServiceManifest.xml is geïnstalleerd met de Service Fabric SDK in *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

* **Het toepassingsmanifest** het toepassingsmanifest wordt gebruikt om de toepassing te beschrijven. Geeft een lijst van de services die het opstellen en andere parameters die worden gebruikt om te definiëren hoe een of meer services moeten worden geïmplementeerd, zoals het aantal exemplaren.

  Een toepassing is in Service Fabric eenheid van de implementatie en upgrade. Een toepassing kan worden bijgewerkt als één eenheid waar de potentiële fouten en mogelijke Rollback worden beheerd. Service Fabric zorgt ervoor dat het upgradeproces een is geslaagd, of als de upgrade mislukt, laat de toepassing in een onbekende of onstabiele status heeft.
* **Servicemanifest** de servicemanifest beschrijving van de onderdelen van een service. Dit omvat gegevens, zoals de naam en type van service, en de code en configuratie. Het servicemanifest bevat ook een aantal extra parameters die kunnen worden gebruikt voor het configureren van de service nadat deze is geïmplementeerd.

## <a name="application-package-file-structure"></a>Bestandsstructuur voor Application-pakket
De toepassing moet een vooraf gedefinieerde mapstructuur Volg voor het implementeren van een Service Fabric-toepassing. Hier volgt een voorbeeld van die structuur.

```
|-- ApplicationPackageRoot
    |-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```

De ApplicationPackageRoot bevat de ApplicationManifest.xml-bestand waarin de toepassing. Een submap voor elke service die is opgenomen in de toepassing wordt gebruikt voor het bevatten van alle artefacten die de service vereist. Deze submappen zijn de ServiceManifest.xml en gewoonlijk het volgende:

* *Code*. Deze map bevat de servicecode.
* *Config*. Deze map bevat een Settings.xml-bestand (en andere bestanden indien nodig) dat de service toegang heeft tot tijdens runtime specifieke configuratie-instellingen op te halen.
* *Gegevens*. Dit is een extra map voor het opslaan van aanvullende lokale gegevens die de service moet mogelijk. Gegevens moet worden gebruikt voor het opslaan van alleen kortstondige gegevens. Service Fabric niet gekopieerd of wijzigingen repliceren naar de map data als de service moet worden verplaatst (bijvoorbeeld tijdens failover).

> [!NOTE]
> U hoeft te maken van de `config` en `data` mappen als u deze niet nodig hebt.
>
>

## <a name="package-an-existing-executable"></a>Een bestaand uitvoerbaar bestand van het pakket
Wanneer een gast uitvoerbaar bestand verpakking, kunt u ofwel een Visual Studio-projectsjabloon gebruiken of [handmatig maken van het toepassingspakket](#manually). Met Visual Studio, worden de toepassing pakketstructuur en manifest-bestanden door het nieuwe projectsjabloon voor u gemaakt.

> [!TIP]
> De eenvoudigste manier om een bestaande Windows uitvoerbare bij een service van het pakket is met Visual Studio en op Linux Yeoman gebruiken
>

## <a name="use-visual-studio-to-package-and-deploy-an-existing-executable"></a>Visual Studio gebruiken om te verpakken en distribueren van een bestaand uitvoerbaar bestand
Visual Studio biedt een Service Fabric-servicesjabloon om te helpen bij het implementeren van een gast uitvoerbare naar een Service Fabric-cluster.

1. Kies **bestand** > **nieuw Project**, en maak een Service Fabric-toepassing.
2. Kies **Gast uitvoerbaar bestand** als de servicesjabloon.
3. Klik op **Bladeren** om te selecteren van de map met het uitvoerbare bestand en voer de rest van de parameters voor het maken van de service.
   * *Pakketgedrag code*. De inhoud van de map naar de Visual Studio-Project kopiëren dit is handig als het uitvoerbare bestand wordt niet gewijzigd kan worden ingesteld. Als u verwacht het uitvoerbare bestand dat te wijzigen en de mogelijkheid om op te halen nieuwe builds dynamisch wilt, kunt u de koppeling naar de map maken in plaats daarvan. U kunt de gekoppelde mappen kunt gebruiken bij het maken van het project voor een toepassing in Visual Studio. Deze koppelingen naar de bronlocatie van binnen het project, waardoor het mogelijk dat u wilt bijwerken van de Gast uitvoerbare in de doel-bron. Deze updates uitmaken deel van het toepassingspakket met build.
   * *Programma* het uitvoerbare bestand dat moet worden uitgevoerd om de service te starten.
   * *Argumenten* Hiermee geeft u de argumenten die moeten worden doorgegeven aan het uitvoerbare bestand. Het kan zijn dat een lijst met parameters met argumenten.
   * *WorkingFolder* Hiermee geeft u de werkmap voor het proces dat zal worden gestart. U kunt drie waarden opgeven:
     * `CodeBase`Hiermee geeft u aan dat de werkmap moet worden ingesteld op de codemap in het toepassingspakket gaat (`Code` directory die wordt weergegeven in de voorgaande bestandsstructuur).
     * `CodePackage`Hiermee geeft u aan dat de werkmap moet worden ingesteld op de hoofdmap van het toepassingspakket gaat (`GuestService1Pkg` weergegeven in de voorgaande bestandsstructuur).
     * `Work`Hiermee geeft u op dat de bestanden in de submap werk zijn geplaatst.
4. Geef de service een naam en klik op **OK**.
5. Als uw service een eindpunt voor communicatie moet, kunt u nu de protocol, de poort en het type toevoegen aan het bestand ServiceManifest.xml. Bijvoorbeeld: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. U kunt nu gebruik van het pakket en publiceren van actie tegen uw lokale cluster door de oplossing in Visual Studio-foutopsporing. Wanneer u klaar bent, kunt u de toepassing publiceren in een cluster met externe of Controleer in de oplossing met resourcebeheer.
7. Ga naar het einde van dit artikel voor informatie over het weergeven van de Gast uitvoerbare service wordt uitgevoerd op de Service Fabric Explorer.

## <a name="use-yeoman-to-package-and-deploy-an-existing-executable-on-linux"></a>Yeoman gebruiken voor het pakket en implementeren van een bestaand uitvoerbaar bestand op Linux

De procedure voor het maken en implementeren van een uitvoerbaar zijn op Linux Gast is hetzelfde als het implementeren van een csharp of java-toepassing.

1. Typ in een terminal `yo azuresfguest`.
2. Geef uw toepassing een naam.
3. Naam van uw service en geef de details, zoals het pad van het uitvoerbare bestand en de parameters die moeten worden aangeroepen met.

Yeoman een toepassingspakket met de juiste toepassing maakt en manifest-bestanden samen met het installeren en verwijderen van scripts.

<a id="manually"></a>

## <a name="manually-package-and-deploy-an-existing-executable"></a>Handmatig verpakken en distribueren van een bestaand uitvoerbaar bestand
Het proces van het verpakken van handmatig een gast uitvoerbaar bestand is gebaseerd op de volgende algemene stappen:

1. De mapstructuur pakket maken.
2. De bestanden en de configuratie van de toepassing toevoegen.
3. Bewerk het manifestbestand van de service.
4. Bewerk het manifestbestand van de toepassing.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>De mapstructuur pakket maken
U kunt starten door het maken van de mapstructuur zoals beschreven in de vorige sectie, "Toepassing pakket bestandsstructuur."

### <a name="add-the-applications-code-and-configuration-files"></a>De bestanden code en configuratie van de toepassing toevoegen
Nadat u de mapstructuur hebt gemaakt, kunt u de toepassingscode en configuratiebestanden onder de code en config-mappen toevoegen. U kunt ook extra mappen of submappen onder de code of configuratieversie mappen maken.

Service Fabric biedt een `xcopy` van de inhoud van de hoofdmap van toepassing, zodat er geen vooraf gedefinieerde structuur dan het maken van de twee bovenste mappen, code en instellingen. (U kunt kiezen verschillende namen als u wilt. Meer informatie vindt u in de volgende sectie.)

> [!NOTE]
> Zorg ervoor dat u alle bestanden en afhankelijkheden die behoeften van de toepassing opneemt. Service Fabric kopieert de inhoud van het toepassingspakket op alle knooppunten in het cluster waar de toepassing services gaat worden geïmplementeerd. Het pakket bevat de code die de toepassing moet worden uitgevoerd. Niet wordt ervan uitgegaan dat de afhankelijkheden al zijn geïnstalleerd.
>
>

### <a name="edit-the-service-manifest-file"></a>Het manifestbestand van de service bewerken
De volgende stap is het bewerken van het manifestbestand van de service met de volgende informatie:

* De naam van het type van de service. Dit is een ID die Service Fabric gebruikt om een service te identificeren.
* De opdracht die moet worden gebruikt om de toepassing (ExeHost) te starten.
* Elk script dat moet worden uitgevoerd om de toepassing (entrypoint) in te stellen.

Hieronder volgt een voorbeeld van een `ServiceManifest.xml` bestand:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

De volgende secties gaat over de verschillende onderdelen van het bestand dat u wilt bijwerken.

#### <a name="update-servicetypes"></a>ServiceTypes bijwerken
```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

* U kunt elke willekeurige naam die u wilt verzamelen `ServiceTypeName`. De waarde wordt gebruikt in de `ApplicationManifest.xml` bestand om de service te identificeren.
* Geef `UseImplicitHost="true"`. Dit kenmerk instrueert Service Fabric dat de service is gebaseerd op een zelfstandige app, zodat alle Service Fabric-behoeften te doen als een proces te starten en de status controleren.

#### <a name="update-codepackage"></a>CodePackage bijwerken
Het element CodePackage geeft de locatie (en versie) van de code van de service.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

De `Name` element wordt gebruikt om op te geven van de naam van de map in het toepassingspakket met de code van de service. `CodePackage`heeft ook de `version` kenmerk. Dit kan worden gebruikt om op te geven van de versie van de code en mogelijk ook van de service om code te upgraden met behulp van de beheerinfrastructuur van de toepassing lifecycle in Service Fabric kan worden gebruikt.

#### <a name="optional-update-setupentrypoint"></a>Optioneel: Update entrypoint
```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
Het element entrypoint wordt gebruikt om op te geven van een uitvoerbaar bestand of batch-bestand dat moet worden uitgevoerd voordat de code van de service wordt gestart. Het is een optionele stap zodat deze niet hoeft te worden opgenomen als er geen initialisatie vereist. De entrypoint wordt uitgevoerd telkens wanneer de service is gestart.

Er is slechts één entrypoint zodat setup-scripts worden gegroepeerd in een enkel batchbestand moeten als de toepassing setup meerdere scripts vereist. Elk type bestand kan worden uitgevoerd door de entrypoint: uitvoerbare bestanden, batchbestanden en PowerShell-cmdlets. Zie voor meer informatie [entrypoint configureren](service-fabric-application-runas-security.md).

In het voorgaande voorbeeld de entrypoint wordt uitgevoerd een batchbestand aangeroepen `LaunchConfig.cmd` die zich bevindt de `scripts` submap van de codemap (uitgaande van het element WorkingFolder is ingesteld op CodeBase).

#### <a name="update-entrypoint"></a>EntryPoint bijwerken
```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

De `EntryPoint` element in het manifestbestand van de service wordt gebruikt om op te geven hoe u kunt de service te starten. De `ExeHost` element geeft de uitvoerbare bestand (en de argumenten) die moet worden gebruikt voor het starten van de service.

* `Program`Hiermee geeft u de naam van het uitvoerbare bestand dat de service moet worden gestart.
* `Arguments`Hiermee geeft u de argumenten die moeten worden doorgegeven aan het uitvoerbare bestand. Het kan zijn dat een lijst met parameters met argumenten.
* `WorkingFolder`Hiermee geeft u de werkmap voor het proces dat zal worden gestart. U kunt drie waarden opgeven:
  * `CodeBase`Hiermee geeft u aan dat de werkmap moet worden ingesteld op de codemap in het toepassingspakket gaat (`Code` map in de voorgaande bestandsstructuur).
  * `CodePackage`Hiermee geeft u aan dat de werkmap moet worden ingesteld op de hoofdmap van het toepassingspakket gaat (`GuestService1Pkg` in de voorgaande bestandsstructuur).
    * `Work`Hiermee geeft u op dat de bestanden in de submap werk zijn geplaatst.

De WorkingFolder is handig voor het instellen van de juiste werkmap zodat relatieve paden kunnen worden gebruikt door de toepassing of de initialisatie van de scripts.

#### <a name="update-endpoints-and-register-with-naming-service-for-communication"></a>Bijwerken van eindpunten en registreren met Naming Service voor communicatie
```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
In het voorgaande voorbeeld de `Endpoint` element geeft de eindpunten die de toepassing kunt luisteren op. In dit voorbeeld wordt de Node.js-toepassing luistert naar http op 3000-poort.

Bovendien kunt u vragen over Service Fabric dit eindpunt publiceren naar de Naming Service, zodat het adres van het eindpunt naar deze service kunnen worden gedetecteerd door andere services. Hiermee kunt u communiceren tussen services die gast uitvoerbare bestanden.
Adres van het gepubliceerde eindpunt heeft de vorm `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme`en `PathSuffix` optionele kenmerken zijn. `IPAddressOrFQDN`de IP-adres of FQDN-naam van het knooppunt op dit uitvoerbare bestand wordt geplaatst en wordt automatisch voor u.

In het volgende voorbeeld zodra de service is geïmplementeerd, in Service Fabric Explorer ziet u een eindpunt die vergelijkbaar is met `http://10.1.4.92:3000/myapp/` gepubliceerd voor service-exemplaar. Of als dit een lokale computer is, ziet u `http://localhost:3000/myapp/`.

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
U kunt deze adressen met [omgekeerde proxy](service-fabric-reverseproxy.md) voor de communicatie tussen services.

### <a name="edit-the-application-manifest-file"></a>Het manifestbestand van de toepassing bewerken
Nadat u hebt geconfigureerd de `Servicemanifest.xml` -bestand, moet u enkele wijzigingen in de `ApplicationManifest.xml` bestand om ervoor te zorgen dat de juiste servicetype en de naam worden gebruikt.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport
In de `ServiceManifestImport` element, kunt u een of meer services die u wilt opnemen in de app opgeven. Services wordt verwezen met `ServiceManifestName`, die aangeeft dat de naam van de map waar de `ServiceManifest.xml` bestand zich bevindt.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Logboekregistratie instellen
Voor uitvoerbare bestanden Gast is het handig om te kunnen zien van de logboeken van de console om erachter te komen als de toepassing en configuratie-scripts eventuele fouten weergegeven.
Console-omleiding kan worden geconfigureerd in de `ServiceManifest.xml` bestand met de `ConsoleRedirection` element.

> [!WARNING]
> Gebruik het beleid voor omleiding console nooit in een toepassing die wordt geïmplementeerd in productie, omdat dit de failover van de toepassing kan beïnvloeden. *Alleen* Gebruik deze optie voor lokale ontwikkeling en foutopsporing.  
>
>

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

`ConsoleRedirection`kan worden gebruikt voor het omleiden van de console-uitvoer (stdout en stderr) in een werkmap. Dit biedt de mogelijkheid om te controleren of er zijn geen fouten tijdens de installatie of de uitvoering van de toepassing in de Service Fabric-cluster.

`FileRetentionCount`Hiermee wordt bepaald hoeveel bestanden worden opgeslagen in de werkmap. Een waarde van 5, betekent bijvoorbeeld dat de logboekbestanden voor de vorige vijf uitvoeringen zijn opgeslagen in de werkmap.

`FileMaxSizeInKb`Hiermee geeft u de maximale grootte van de logboekbestanden.

Logboekbestanden worden opgeslagen in een van de service werken mappen. Service Fabric Explorer om te bepalen welk knooppunt de service gebruiken om te bepalen waar de bestanden zich bevinden op wordt uitgevoerd en welke werkmap wordt gebruikt. Dit proces wordt verderop in dit artikel beschreven.

## <a name="deployment"></a>Implementatie
De laatste stap is het [implementeren van uw toepassing](service-fabric-deploy-remove-applications.md). De volgende PowerShell-script laat zien hoe uw toepassing implementeert op de lokaal ontwikkelcluster, en een nieuwe Service Fabric-service starten.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```

>[!TIP]
> [Comprimeren van het pakket](service-fabric-package-apps.md#compress-a-package) vóór het kopiëren van de image store als het pakket grote is of veel bestanden. Lees meer [hier](service-fabric-deploy-remove-applications.md#upload-the-application-package).
>

Een Service Fabric-service kan worden geïmplementeerd in verschillende 'configuraties." Bijvoorbeeld, het kan worden geïmplementeerd als één of meerdere exemplaren of deze zodanig dat er een exemplaar van de service op elk knooppunt van het Service Fabric-cluster wordt kan worden geïmplementeerd.

De `InstanceCount` parameter van de `New-ServiceFabricService` cmdlet wordt gebruikt om op te geven hoeveel exemplaren van de service moeten worden gestart in de Service Fabric-cluster. U kunt instellen de `InstanceCount` waarde, afhankelijk van het type van de toepassing die u implementeert. De twee meest voorkomende scenario's:

* `InstanceCount = "1"`. In dit geval wordt wordt slechts één exemplaar van de service geïmplementeerd in het cluster. Service-Fabric scheduler bepaalt welk knooppunt de service gaat wordt geïmplementeerd op.
* `InstanceCount ="-1"`. In dit geval wordt wordt één exemplaar van de service geïmplementeerd op elk knooppunt in het Service Fabric-cluster. Het resultaat heeft één (en slechts één) exemplaar van de service voor elk knooppunt in het cluster.

Dit is een nuttig configuratie voor de front-end-toepassingen (bijvoorbeeld een REST-eindpunt), omdat clienttoepassingen verbinding moeten maken "" een van de knooppunten in het cluster het eindpunt te gebruiken. Deze configuratie kan ook worden gebruikt wanneer u bijvoorbeeld alle knooppunten van het Service Fabric-cluster zijn verbonden met een load balancer. Clientverkeer kan vervolgens worden gedistribueerd via de service die wordt uitgevoerd op alle knooppunten in het cluster.

## <a name="check-your-running-application"></a>Controleer de actieve toepassing
Identificeer in Service Fabric Explorer het knooppunt waar de service wordt uitgevoerd. In dit voorbeeld op wordt uitgevoerd knooppunt1:

![Knooppunt waarop de service wordt uitgevoerd](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Als u naar het knooppunt navigeren en blader naar de toepassing, ziet u de essentiële knooppunt informatie, waaronder de locatie op schijf.

![Locatie op schijf](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Als u naar de map bladeren met behulp van de Server Explorer, vindt u de werkmap en de logboekmap voor de service, zoals wordt weergegeven in de volgende schermafbeelding: 

![Locatie van logboek](./media/service-fabric-deploy-existing-app/loglocation.png)

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe een gast uitvoerbaar bestand van het pakket en deze implementeren in Service Fabric. Zie de volgende artikelen voor meer informatie en taken.

* [Voorbeeld voor verpakken en distribueren van een gast uitvoerbaar bestand](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), inclusief een koppeling naar de voorlopige versie van het hulpprogramma verpakking
* [Voorbeeld van twee Gast uitvoerbare bestanden (C# en nodejs) communiceren via de Naming service met behulp van REST](https://github.com/Azure-Samples/service-fabric-containers)
* [Meerdere toepassingen implementeren die door gasten kunnen worden uitgevoerd](service-fabric-deploy-multiple-apps.md)
* [Uw eerste Service Fabric-toepassing met Visual Studio maken](service-fabric-create-your-first-application-in-visual-studio.md)
