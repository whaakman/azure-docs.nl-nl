---
title: Azure Service Fabric-toepassingsmodel | Microsoft Docs
description: Het model en beschrijven toepassingen en services in Service Fabric.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: e30482427b88eb3e58d39075c7f0734664b79aa2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="model-an-application-in-service-fabric"></a>Model van een toepassing in Service Fabric
Dit artikel bevat een overzicht van het model van Azure Service Fabric-toepassing en het definiëren van een toepassing en service via de manifest-bestanden.

## <a name="understand-the-application-model"></a>Inzicht in het toepassingsmodel
Een toepassing is een verzameling van de services die een bepaalde functie of functies uitvoeren. Een service kunt vervult een functie die volledig en zelfstandig starten en onafhankelijk van andere services worden uitgevoerd.  Een service bestaat uit code, configuratie en gegevens. Voor elke service code bestaat uit de uitvoerbare binaire bestanden, configuratie bestaat uit de service-instellingen die kunnen worden geladen tijdens runtime en gegevens bestaan uit willekeurige statische gegevens kunnen worden gebruikt door de service. Elk onderdeel in dit toepassingsmodel hiërarchische kan worden samengesteld en bijgewerkte onafhankelijk.

![Het Service Fabric-toepassing-model][appmodel-diagram]

Een toepassingstype een categorisatie van een toepassing is en bestaat uit een bundel van servicetypen. Een service is een categorisatie van een service. De categorisatie kan hebben verschillende instellingen en configuraties, maar de kernfunctionaliteit hetzelfde is gebleven. De exemplaren van een service zijn de andere service configuration variaties van hetzelfde servicetype.  

Klassen (of 'typen') van toepassingen en services via XML-bestanden (Toepassingsmanifesten en service manifesten) worden beschreven.  De manifesten zijn de sjablonen op basis waarvan toepassingen uit het archief van de installatiekopie van het cluster kunnen worden gemaakt. De schemadefinitie voor het bestand ServiceManifest.xml en ApplicationManifest.xml is geïnstalleerd met de Service Fabric SDK en hulpprogramma's voor *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

De code voor exemplaren van een andere toepassing wordt uitgevoerd als afzonderlijke processen, zelfs wanneer deze wordt gehost door de dezelfde Service Fabric-knooppunt. Bovendien de levenscyclus van elk toepassingsexemplaar kan worden beheerd (bijvoorbeeld een bijgewerkt) onafhankelijk. Het volgende diagram toont hoe toepassingstypen servicetype die op zijn beurt bestaan uit code, configuratie en gegevenspakketten zijn samengesteld. Om te vereenvoudigen in het diagram, alleen de code/config/gegevens pakketten voor `ServiceType4` worden weergegeven, hoewel elk servicetype u enkele vindt zou of alle pakkettypen.

![Service Fabric-toepassing waardetypen en servicetypen][cluster-imagestore-apptypes]

Twee verschillende manifest-bestanden worden gebruikt om toepassingen en services te beschrijven: het servicemanifest en het toepassingsmanifest. Manifesten worden in detail in de volgende secties besproken.

Er kan een of meer exemplaren van een servicetype actief zijn in het cluster. Bijvoorbeeld, bereiken stateful service-exemplaren of replica's hoge betrouwbaarheid status repliceren tussen replica's zich op verschillende knooppunten in het cluster. Replicatie wordt in wezen biedt redundantie voor de service moet beschikbaar, zelfs als een knooppunt in een cluster is mislukt. Een [service gepartitioneerd](service-fabric-concepts-partitioning.md) verdere verdeelt de status (en toegangspatronen op de bijbehorende status) tussen knooppunten in het cluster.

Het volgende diagram toont de relatie tussen toepassingen en service-exemplaren, partities en replica's.

![Partities en replica's binnen een service][cluster-application-instances]

> [!TIP]
> U kunt de indeling van toepassingen weergeven in een cluster met behulp van het Service Fabric Explorer hulpprogramma beschikbaar op http://&lt;yourclusteraddress&gt;: 19080/Explorer. Zie voor meer informatie [uw cluster visualiseren met Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
> 
> 

## <a name="describe-a-service"></a>Een service omschrijven
De servicemanifest definieert declaratief het servicetype en de versie. Hiermee wordt de metagegevens zoals servicetype, eigenschappen van health-taakverdeling metrische gegevens, de binaire bestanden en configuratiebestanden.  Anders gezegd, beschrijft de code, configuratie en gegevens pakketten waaruit een servicepakket ter ondersteuning van een of meer servicetypen. Dit is een eenvoudig voorbeeld servicemanifest:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
    <EnvironmentVariables>
      <EnvironmentVariable Name="MyEnvVariable" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentVariables>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
```

**Versie** kenmerken ongestructureerde tekenreeksen zijn en niet worden geparseerd door het systeem. Versie-kenmerken worden gebruikt voor de versie van elk onderdeel voor upgrades.

**ServiceTypes** wordt gedeclareerd welke servicetypen worden ondersteund door **CodePackages** in dit manifest. Wanneer een service met een van deze servicetypen wordt gestart, worden alle code pakketten die zijn gedefinieerd in deze manifest geactiveerd door het uitvoeren van hun toegangspunten. De resulterende processen wordt verwacht dat het registreren van de ondersteunde servicetypen tijdens runtime. Service-typen zijn gedeclareerd op het manifest niveau en niet de code-pakket. Dus als er meerdere code pakketten, worden alle geactiveerd wanneer het systeem wordt gezocht naar een van de gedeclareerde servicetypen.

**Entrypoint** is een bevoorrechte toegangspunt dat wordt uitgevoerd met dezelfde referenties als Service Fabric (meestal de *LocalSystem* account) voordat andere toegangspunt. Het uitvoerbare bestand door opgegeven **EntryPoint** is meestal de ServiceHost langlopende. De aanwezigheid van een toegangspunt voor de afzonderlijke instellingen zo voorkomt u dat de ServiceHost uitvoeren met hoge bevoegdheden voor langere tijd. Het uitvoerbare bestand door opgegeven **EntryPoint** wordt uitgevoerd na **entrypoint** is afgesloten. Als het proces wordt beëindigd ooit of vastloopt, het resulterende proces wordt bewaakt en opnieuw opgestart (opnieuw vanaf **entrypoint**).  

Typische scenario's voor het gebruik van **entrypoint** zijn wanneer u een uitvoerbaar bestand voordat u de service wordt gestart uitvoeren of het uitvoeren van een bewerking met verhoogde bevoegdheden. Bijvoorbeeld:

* In te stellen en het initialiseren van omgevingsvariabelen op die moet van het uitvoerbare bestand van de service. Dit is niet beperkt tot alleen uitvoerbare bestanden die zijn geschreven via de Service Fabric-programmeermodellen. Npm.exe moet bijvoorbeeld een aantal omgevingsvariabelen die is geconfigureerd voor het implementeren van een node.js-toepassing.
* Instellen van toegangsbeheer door beveiligingscertificaten te installeren.

Voor meer informatie over het configureren van de **entrypoint** Zie [het beleid voor een toegangspunt voor service-instellingen configureren](service-fabric-application-runas-security.md)

**Omgevingsvariabelen** geeft een lijst van omgevingsvariabelen die zijn ingesteld voor dit codepakket. Environmentment variabelen kunnen worden overschreven in de `ApplicationManifest.xml` naar verschillende waarden voor verschillende service-exemplaren. 

**Gegevenspakket** declareert een map met de naam door de **naam** kenmerk met willekeurige statische gegevens om te worden verbruikt door het proces tijdens runtime.

**ConfigPackage** declareert een map met de naam door de **naam** kenmerk dat bevat een *Settings.xml* bestand. Het bestand met instellingen bevat secties van de gebruiker gedefinieerde, sleutel / waarde-paar-instellingen die het proces tijdens de uitvoering leest. Tijdens een upgrade als alleen de **ConfigPackage** **versie** is gewijzigd, en vervolgens het proces dat wordt uitgevoerd niet opnieuw wordt opgestart. In plaats daarvan waarschuwt een retouraanroep het proces dat configuratie-instellingen hebt gewijzigd zodat ze opnieuw kunnen worden geladen dynamisch. Hier volgt een voorbeeld *Settings.xml* bestand:

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

> [!NOTE]
> Een servicemanifest kan meerdere code, configuratie en gegevenspakketten bevatten. Elk van deze kan worden samengesteld onafhankelijk.
> 
> 

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Een toepassing beschrijven
Het toepassingsmanifest beschrijving declaratief van het toepassingstype en -versie. Deze geeft service samenstelling metagegevens zoals stabiele namen, partitioneren schema, exemplaar count/replicatie factor/isolatie-beleid, plaatsingsbeperkingen, configuratie onderdrukkingen en samenstellende servicetypen. De loadbalancer-domeinen waarin de toepassing is geplaatst, worden ook beschreven.

Een toepassingsmanifest is dus worden de elementen op toepassingsniveau beschreven en verwijst naar een of meer service-manifesten om op te stellen van een toepassingstype. Dit is een eenvoudig voorbeeld toepassingsmanifest:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
    <ConfigOverrides/>
    <EnvironmentOverrides CodePackageRef="MyCode"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
```

Service manifesten, zoals **versie** kenmerken ongestructureerde tekenreeksen zijn en niet worden geparseerd door het systeem. Versie-kenmerken worden ook gebruikt voor de versie van elk onderdeel voor upgrades.

**ServiceManifestImport** bevat verwijzingen naar service manifesten waaruit dit toepassingstype. Geïmporteerde service manifesten bepalen welke servicetypen zijn geldig binnen dit toepassingstype. Binnen de ServiceManifestImport overschrijven u configuratiewaarden in Settings.xml en omgevingsvariabelen in ServiceManifest.xml bestanden. 


**DefaultServices** declareert service-exemplaren die automatisch worden gemaakt wanneer een toepassing wordt gestart op basis van dit toepassingstype. Standaardservices zijn alleen gemakkelijker te maken en zich gedragen als normale services in elke opzicht nadat ze zijn gemaakt. Deze samen met andere services in het toepassingsexemplaar zijn bijgewerkt en kunnen ook worden verwijderd.

> [!NOTE]
> Een toepassingsmanifest kan meerdere service manifest invoer en standaardservices bevatten. Elke manifestimport service kan onafhankelijk samengestelde zijn.
> 
> 

Zie voor informatie over het onderhouden van de andere toepassing en serviceparameters voor afzonderlijke omgevingen, [beheren voor omgevingen met meerdere parameters voor de toepassing](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Volgende stappen
[Een toepassing van het pakket](service-fabric-package-apps.md) en klaar om te implementeren.

[Implementeren en toepassingen verwijderen] [ 10] wordt beschreven hoe u PowerShell gebruiken om exemplaren van een toepassing beheren.

[Het beheren van parameters voor de toepassing voor omgevingen met meerdere] [ 11] wordt beschreven hoe u parameters en variabelen voor exemplaren van een andere toepassing.

[Beveiligingsbeleid voor uw toepassing configureert] [ 12] wordt beschreven hoe u services onder het beveiligingsbeleid om toegang te beperken.

[Toepassing hosten modellen] [ 13] beschrijven de relatie tussen de replica's (of exemplaren) van een geïmplementeerde service en de ServiceHost proces.

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
[13]: service-fabric-hosting-model.md
