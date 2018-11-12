---
title: Een bestaand uitvoerbaar bestand implementeren naar Azure Service Fabric | Microsoft Docs
description: Informatie over het verpakken van een bestaande toepassing als gast uitvoerbare bestanden, zodat deze kan worden geïmplementeerd in een Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/02/2017
ms.author: mfussell
ms.openlocfilehash: 0f4bb3f32b264bd894341a8776d48eb9f8b061a2
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258728"
---
# <a name="package-and-deploy-an-existing-executable-to-service-fabric"></a>Pakket maken en implementeren van een bestaand uitvoerbaar bestand naar Service Fabric
Bij het verpakken van een bestaand uitvoerbaar bestand als een [Gast kan worden uitgevoerd](service-fabric-guest-executables-introduction.md), kunt u ofwel een Visual Studio-projectsjabloon gebruiken of [handmatig maken van het toepassingspakket](#manually). Met behulp van Visual Studio, worden de toepassing pakketstructuur en de manifest-bestanden door de nieuwe projectsjabloon, maken voor u gemaakt.

> [!TIP]
> De eenvoudigste manier om het inpakken van een bestaande Windows uitvoerbare bestand in een service is het gebruik van Visual Studio en op Linux gebruiken Yeoman
>

## <a name="use-visual-studio-to-package-and-deploy-an-existing-executable"></a>Visual Studio gebruiken om te verpakken en implementeren van een bestaand uitvoerbaar bestand
Visual Studio biedt een Service Fabric-servicesjabloon om u te helpen bij het implementeren van een door gasten uitvoerbare bestanden in een Service Fabric-cluster.

1. Kies **bestand** > **nieuw Project**, en maak een Service Fabric-toepassing.
2. Kies **Gast kan worden uitgevoerd** als de servicesjabloon.
3. Klik op **Bladeren** voor het selecteren van de map met het uitvoerbare bestand en vul in de rest van de parameters voor het maken van de service.
   * *Gedrag Codepakket code*. Kan worden ingesteld om te kopiëren van de inhoud van de map aan de Visual Studio-Project, dit is handig als het uitvoerbare bestand niet verandert. Als u verwacht het uitvoerbare bestand dat te wijzigen en de mogelijkheid om op te halen nieuwe builds dynamisch wilt, kunt u in plaats daarvan een koppeling naar de map. U kunt de gekoppelde mappen gebruiken bij het maken van het project voor een toepassing in Visual Studio. Deze koppelingen naar de bronlocatie van binnen het project, waardoor het mogelijk dat u bij het bijwerken van de door gasten uitvoerbare bestanden in de bron-doel. Deze updates worden onderdeel van het toepassingspakket op build.
   * *Programma* Hiermee geeft u het uitvoerbare bestand dat moet worden uitgevoerd om de service te starten.
   * *Argumenten* Hiermee geeft u de argumenten die moeten worden doorgegeven aan het uitvoerbare bestand. Een lijst met parameters met argumenten kan zijn.
   * *WorkingFolder* Hiermee geeft u de werkmap voor het proces dat moet worden gestart. U kunt drie waarden opgeven:
     * `CodeBase` Hiermee geeft u aan dat de werkmap moet worden ingesteld op de codemap in het toepassingspakket is gaan (`Code` map die wordt weergegeven in de structuur van het vorige bestand).
     * `CodePackage` Hiermee geeft u aan dat de werkmap moet worden ingesteld op de hoofdmap van het toepassingspakket is gaan (`GuestService1Pkg` wordt weergegeven in de structuur van het vorige bestand).
     * `Work` Hiermee geeft u op dat de bestanden worden geplaatst in een submap met de naam werk.
4. Geef de service een naam en klik op **OK**.
5. Als uw service heeft een eindpunt voor communicatie nodig, kunt u nu het protocol, de poort en het type toevoegen aan het bestand ServiceManifest.xml. Bijvoorbeeld: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. U kunt nu het pakket en publiceren van actie op basis van uw lokale cluster door het opsporen van fouten in de oplossing in Visual Studio. Wanneer u klaar bent, kunt u de toepassing in een extern cluster publiceren of Controleer in de oplossing voor broncodebeheer.
7. Lezen [controleren van de toepassing wordt uitgevoerd](#check-your-running-application) voor informatie over het weergeven van de Gast uitvoerbare service die wordt uitgevoerd in Service Fabric Explorer.

Zie voor een overzicht voorbeeld [uw eerste Gast uitvoerbare toepassing maken met Visual Studio](quickstart-guest-app.md).

## <a name="use-yeoman-to-package-and-deploy-an-existing-executable-on-linux"></a>Gebruik van Yeoman pakket en de implementatie van een bestaand uitvoerbaar bestand op Linux

De procedure voor het maken en implementeren van een door gasten uitvoerbare bestanden in Linux is hetzelfde als bij het implementeren van een csharp of java-toepassing.

1. Typ in een terminal `yo azuresfguest`.
2. Geef uw toepassing een naam.
3. Noem uw service en geef de details op zoals pad van het uitvoerbare bestand en de toepassing moet worden aangeroepen met parameters.

Yeoman maakt een toepassingspakket met de juiste toepassing en -manifestbestanden samen met het installeren en verwijderen van scripts.

<a id="manually"></a>

## <a name="manually-package-and-deploy-an-existing-executable"></a>Handmatig verpakken en implementeren van een bestaand uitvoerbaar bestand
Het proces van het inpakken van handmatig een Gast kan worden uitgevoerd, is gebaseerd op de volgende algemene stappen:

1. De mapstructuur pakket maken.
2. De code en configuratiebestanden van de toepassing toevoegen.
3. Bewerk het manifestbestand van de service.
4. Bewerk het manifestbestand van de toepassing.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](https://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>De mapstructuur pakket maken
U kunt beginnen met het maken van de directory-structuur, zoals beschreven in de vorige sectie, "Toepassing pakket bestandsstructuur."

### <a name="add-the-applications-code-and-configuration-files"></a>De code en configuratiebestanden van de toepassing toevoegen
Nadat u de mapstructuur hebt gemaakt, kunt u de code van de toepassing en configuratiebestanden onder de code en config-mappen toevoegen. U kunt ook extra mappen of submappen onder de code of configuratieversie mappen maken.

Service Fabric biedt een `xcopy` van de inhoud van de hoofdmap van toepassing, zodat er geen vooraf gedefinieerde structuur te gebruiken dan het maken van de twee bovenste directory's, code en -instellingen. (U kunt kiezen verschillende namen als u wilt. Meer informatie vindt u in de volgende sectie.)

> [!NOTE]
> Zorg ervoor dat u alle bestanden en afhankelijkheden die de toepassing nodig heeft. Service Fabric kopieert de inhoud van het toepassingspakket op alle knooppunten in het cluster waar services van de toepassing gaat worden geïmplementeerd. Het pakket bevat de code die de toepassing nodig heeft om uit te voeren. Niet wordt ervan uitgegaan dat er al de afhankelijkheden zijn geïnstalleerd.
>
>

### <a name="edit-the-service-manifest-file"></a>Het service manifest-bestand bewerken
De volgende stap is het bewerken van het manifestbestand van de service met de volgende informatie:

* De naam van het type van de service. Dit is een ID die gebruikmaakt van Service Fabric om een service te identificeren.
* De opdracht gebruiken om te starten van de toepassing (ExeHost).
* Elk script dat worden uitgevoerd moet voor het instellen van de toepassing (SetupEntrypoint).

Hier volgt een voorbeeld van een `ServiceManifest.xml` bestand:

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

De volgende secties gaat over de verschillende onderdelen van het bestand dat u nodig hebt om bij te werken.

#### <a name="update-servicetypes"></a>ServiceTypes bijwerken
```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

* U kunt een willekeurige naam die u wilt gebruiken voor kiezen `ServiceTypeName`. De waarde wordt gebruikt de `ApplicationManifest.xml` bestand om de service te identificeren.
* Geef `UseImplicitHost="true"`. Dit kenmerk wordt Service Fabric aan dat de service is gebaseerd op een zelfstandige app, zodat alle Service Fabric-behoeften te doen als een proces te starten en de status controleren.

#### <a name="update-codepackage"></a>CodePackage bijwerken
Het element CodePackage Hiermee geeft u de locatie (en -versie) van de code van de service.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

De `Name` element wordt gebruikt om op te geven van de naam van de map in het toepassingspakket met de code van de service. `CodePackage` heeft ook de `version` kenmerk. Dit kan worden gebruikt om op te geven van de versie van de code en kan ook worden gebruikt om upgradecode van de service met behulp van de application lifecycle management-infrastructuur in Service Fabric.

#### <a name="optional-update-setupentrypoint"></a>Optioneel: Update SetupEntrypoint
```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
Het element SetupEntryPoint wordt gebruikt om op te geven van een uitvoerbaar bestand of batch-bestand dat moet worden uitgevoerd voordat de code van de service wordt gestart. Dit is een optionele stap zodat deze niet hoeft te worden opgenomen als er geen initialisatie vereist. De SetupEntryPoint wordt uitgevoerd telkens wanneer de service opnieuw is gestart.

Er is slechts één SetupEntryPoint zodat installatiescripts worden gegroepeerd in een één batch-bestand moeten als de installatie van de toepassing meerdere scripts vereist. Elk type bestand kan worden uitgevoerd door de SetupEntryPoint: uitvoerbare bestanden, batch-bestanden en PowerShell-cmdlets. Zie voor meer informatie, [SetupEntryPoint configureren](service-fabric-application-runas-security.md).

In het voorgaande voorbeeld de wordt uitgevoerd een batchbestand met de naam van de SetupEntryPoint `LaunchConfig.cmd` die bevindt zich in de `scripts` submap van de codemap (ervan uitgaande dat het element WorkingFolder is ingesteld op CodeBase).

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

De `EntryPoint` element in het manifestbestand van de service wordt gebruikt om op te geven over het starten van de service.

De `ExeHost` element Hiermee geeft u het uitvoerbare bestand (en de argumenten) die moet worden gebruikt voor het starten van de service. U kunt eventueel toevoegen de `IsExternalExecutable="true"` kenmerk te `ExeHost` om aan te geven dat het programma een externe uitvoerbaar bestand buiten het codepakket. Bijvoorbeeld `<ExeHost IsExternalExecutable="true">`.

* `Program` Hiermee geeft u de naam van het uitvoerbare bestand dat de service moet worden gestart.
* `Arguments` Hiermee geeft u de argumenten die moeten worden doorgegeven aan het uitvoerbare bestand. Een lijst met parameters met argumenten kan zijn.
* `WorkingFolder` Hiermee geeft u de werkmap voor het proces dat moet worden gestart. U kunt drie waarden opgeven:
  * `CodeBase` Hiermee geeft u aan dat de werkmap moet worden ingesteld op de codemap in het toepassingspakket is gaan (`Code` Active directory in de structuur van het vorige bestand).
  * `CodePackage` Hiermee geeft u aan dat de werkmap moet worden ingesteld op de hoofdmap van het toepassingspakket is gaan (`GuestService1Pkg` in de structuur van het vorige bestand).
    * `Work` Hiermee geeft u op dat de bestanden worden geplaatst in een submap met de naam werk.

De WorkingFolder is handig om de juiste werkmap zo instellen dat relatieve paden kunnen worden gebruikt door de toepassing of de initialisatie van scripts.

#### <a name="update-endpoints-and-register-with-naming-service-for-communication"></a>Bijwerken van eindpunten en registreren bij Naming-Service voor communicatie
```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
In het voorgaande voorbeeld wordt de `Endpoint` element Hiermee geeft u de eindpunten die de toepassing kan luisteren. In dit voorbeeld wordt de Node.js-toepassing luistert naar http op poort 3000.

Bovendien kunt u vragen over Service Fabric dit eindpunt publiceren naar de Naming-Service, zodat het adres van het eindpunt bij deze service kunnen u andere services detecteren. Hiermee kunt u communiceren tussen services die uitvoerbare gastbestanden.
Adres van het gepubliceerde eindpunt heeft de vorm `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme` en `PathSuffix` optionele kenmerken zijn. `IPAddressOrFQDN` de IP-adres of de volledig gekwalificeerde domeinnaam van het knooppunt op dit uitvoerbare bestand wordt geplaatst en wordt berekend voor u.

In het volgende voorbeeld, wanneer de service is geïmplementeerd, in Service Fabric Explorer ziet u een eindpunt die vergelijkbaar is met `http://10.1.4.92:3000/myapp/` gepubliceerd voor de service-exemplaar. Of als dit een lokale computer is, ziet u `http://localhost:3000/myapp/`.

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
U kunt deze adressen met [omgekeerde proxy](service-fabric-reverseproxy.md) voor de communicatie tussen services.

### <a name="edit-the-application-manifest-file"></a>Het manifestbestand van de toepassing bewerken
Nadat u hebt geconfigureerd de `Servicemanifest.xml` -bestand, moet u de wijzigingen aanbrengen aan de `ApplicationManifest.xml` bestand om ervoor te zorgen dat de juiste servicetype en de naam worden gebruikt.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport
In de `ServiceManifestImport` -element, kunt u een of meer services die u wilt opnemen in de app. Services zijn waarnaar wordt verwezen met `ServiceManifestName`, dat aangeeft dat de naam van de map waar de `ServiceManifest.xml` -bestand zich bevindt.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Logboekregistratie instellen
Voor gasten uitvoerbare bestanden is het handig om te kunnen zien consolelogboeken om erachter te komen als de toepassing en configuratie van scripts voor eventuele fouten weergegeven.
Console-omleiding kan worden geconfigureerd de `ServiceManifest.xml` bestand met behulp van de `ConsoleRedirection` element.

> [!WARNING]
> Gebruik nooit het beleid voor console-omleiding in een toepassing die is geïmplementeerd in een productieomgeving omdat dit kan invloed hebben op de failover van de toepassing. *Alleen* Gebruik deze optie voor lokale ontwikkeling en foutopsporing.  
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

`ConsoleRedirection` kan worden gebruikt om te leiden van de console-uitvoer (stdout en stderr) naar een werkmap. Dit biedt de mogelijkheid om te controleren of dat er geen fouten zijn opgetreden tijdens de installatie of de uitvoering van de toepassing in de Service Fabric-cluster.

`FileRetentionCount` bepaalt hoeveel bestanden worden opgeslagen in de werkmap. Een waarde van 5, betekent bijvoorbeeld dat de logboekbestanden voor de vorige vijf uitvoeringen zijn opgeslagen in de werkmap.

`FileMaxSizeInKb` Hiermee geeft u de maximale grootte van de logboekbestanden.

Logboekbestanden worden opgeslagen in een van de service werken mappen. Service Fabric Explorer om te bepalen welk knooppunt de service gebruiken om te bepalen waar de bestanden zich bevinden op wordt uitgevoerd en welke werkmap wordt gebruikt. Dit proces wordt verderop in dit artikel beschreven.

## <a name="deployment"></a>Implementatie
De laatste stap is het [Implementeer uw toepassing](service-fabric-deploy-remove-applications.md). De volgende PowerShell-script laat zien hoe het implementeren van uw toepassing op het lokale ontwikkelingscluster en start een nieuwe Service Fabric-service.

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
> [Comprimeren van het pakket](service-fabric-package-apps.md#compress-a-package) voordat u kopieert naar de installatiekopieopslag als het pakket grote is of veel bestanden. Lees meer [hier](service-fabric-deploy-remove-applications.md#upload-the-application-package).
>

Een Service Fabric-service kan worden geïmplementeerd in verschillende 'configuraties." Bijvoorbeeld, deze kan worden geïmplementeerd als één of meerdere instanties, of deze zodanig dat er een exemplaar van de service op elk knooppunt van de Service Fabric-cluster wordt kan worden geïmplementeerd.

De `InstanceCount` parameter van de `New-ServiceFabricService` cmdlet wordt gebruikt om op te geven hoeveel exemplaren van de service moeten worden gestart in de Service Fabric-cluster. U kunt instellen dat de `InstanceCount` waarde, afhankelijk van het type van de toepassing die u implementeert. De twee meest voorkomende scenario's zijn:

* `InstanceCount = "1"`. In dit geval wordt slechts één exemplaar van de service geïmplementeerd in het cluster. Service-Fabric scheduler bepaalt welk knooppunt de service zal worden geïmplementeerd op.
* `InstanceCount ="-1"`. In dit geval is één exemplaar van de service op elk knooppunt in het Service Fabric-cluster geïmplementeerd. Het resultaat heeft één (en slechts één) exemplaar van de service voor elk knooppunt in het cluster.

Dit is een nuttig configuratie voor de front-end-toepassingen (bijvoorbeeld, een REST-eindpunt), omdat clienttoepassingen moeten 'connect' met een van de knooppunten in het cluster om het eindpunt te gebruiken. Deze configuratie kan ook worden gebruikt wanneer u bijvoorbeeld alle knooppunten van de Service Fabric-cluster zijn verbonden met een load balancer. -Clientverkeer kan vervolgens worden verdeeld over de service die wordt uitgevoerd op alle knooppunten in het cluster.

## <a name="check-your-running-application"></a>Controleer de toepassing wordt uitgevoerd
Identificeer in Service Fabric Explorer het knooppunt waar de service wordt uitgevoerd. In dit voorbeeld wordt deze uitgevoerd op knooppunt1:

![Knooppunt waarop de service wordt uitgevoerd](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Als u naar het knooppunt en blader naar de toepassing, ziet u de essentiële knooppunt-informatie, met inbegrip van de locatie op schijf.

![Locatie op schijf](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Als u naar de map via Server Explorer bladert, kunt u de werkmap en van de service logboekmap vinden, zoals wordt weergegeven in de volgende schermafbeelding: 

![Locatie van logboekbestand](./media/service-fabric-deploy-existing-app/loglocation.png)

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe kunnen worden verpakt een Gast kan worden uitgevoerd en deze implementeren in Service Fabric. Zie de volgende artikelen voor gerelateerde informatie en taken.

* [Voorbeeld voor het verpakken en implementeren van een Gast kan worden uitgevoerd](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), met inbegrip van een koppeling naar de voorlopige versie van het hulpprogramma verpakking
* [Voorbeeld van twee Gast uitvoerbare bestanden (C# en nodejs) communiceren via de Naming-service met behulp van REST](https://github.com/Azure-Samples/service-fabric-containers)
* [Meerdere toepassingen implementeren die door gasten kunnen worden uitgevoerd](service-fabric-deploy-multiple-apps.md)
* [Uw eerste Service Fabric-toepassing met Visual Studio maken](service-fabric-create-your-first-application-in-visual-studio.md)
