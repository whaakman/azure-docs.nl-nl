---
title: Met een beschrijving van Azure Service Fabric-apps en services | Microsoft Docs
description: Hierin wordt beschreven hoe manifesten worden gebruikt om Service Fabric-toepassingen en services te beschrijven.
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
ms.date: 12/07/2017
ms.author: ryanwi
ms.openlocfilehash: 8e0cf78aef7e973188ce9581ec94f012f6ecde90
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="service-fabric-application-and-service-manifests"></a>Service Fabric-toepassing en service manifesten
Dit artikel wordt beschreven hoe Service Fabric-toepassingen en services worden gedefinieerd en samengestelde met behulp van de bestanden ApplicationManifest.xml en ServiceManifest.xml.  Het XML-schema voor deze manifestbestanden wordt beschreven in [ServiceFabricServiceModel.xsd schemadocumentatie](service-fabric-service-model-schema.md).

## <a name="describe-a-service-in-servicemanifestxml"></a>Een service in ServiceManifest.xml beschrijven
De servicemanifest definieert declaratief het servicetype en de versie. Hiermee wordt de metagegevens zoals servicetype, eigenschappen van health-taakverdeling metrische gegevens, de binaire bestanden en configuratiebestanden.  Anders gezegd, beschrijft de code, configuratie en gegevens pakketten waaruit een servicepakket ter ondersteuning van een of meer servicetypen. Een servicemanifest kan meerdere code, configuratie en gegevenspakketten, die samengesteld onafhankelijk worden kunnen bevatten. Hier volgt een servicemanifest voor de service ASP.NET Core web-front-end van de [stemmen voorbeeldtoepassing](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart):

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType. 
         This name must match the string used in RegisterServiceType call in Program.cs. -->
    <StatelessServiceType ServiceTypeName="VotingWebType" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

**Versie** kenmerken ongestructureerde tekenreeksen zijn en niet worden geparseerd door het systeem. Versie-kenmerken worden gebruikt voor de versie van elk onderdeel voor upgrades.

**ServiceTypes** wordt gedeclareerd welke servicetypen worden ondersteund door **CodePackages** in dit manifest. Wanneer een service met een van deze servicetypen wordt gestart, worden alle code pakketten die zijn gedefinieerd in deze manifest geactiveerd door het uitvoeren van hun toegangspunten. De resulterende processen wordt verwacht dat het registreren van de ondersteunde servicetypen tijdens runtime. Service-typen zijn gedeclareerd op het manifest niveau en niet de code-pakket. Dus als er meerdere code pakketten, worden alle geactiveerd wanneer het systeem wordt gezocht naar een van de gedeclareerde servicetypen.

Het uitvoerbare bestand door opgegeven **EntryPoint** is meestal de ServiceHost langlopende. **Entrypoint** is een bevoorrechte toegangspunt dat wordt uitgevoerd met dezelfde referenties als Service Fabric (meestal de *LocalSystem* account) voordat andere toegangspunt.  De aanwezigheid van een toegangspunt voor de afzonderlijke instellingen zo voorkomt u dat de ServiceHost uitvoeren met hoge bevoegdheden voor langere tijd. Het uitvoerbare bestand door opgegeven **EntryPoint** wordt uitgevoerd na **entrypoint** is afgesloten. Als het proces wordt beëindigd ooit of vastloopt, het resulterende proces wordt bewaakt en opnieuw opgestart (opnieuw vanaf **entrypoint**).  

Typische scenario's voor het gebruik van **entrypoint** zijn wanneer u een uitvoerbaar bestand voordat u de service wordt gestart uitvoeren of het uitvoeren van een bewerking met verhoogde bevoegdheden. Bijvoorbeeld:

* In te stellen en het initialiseren van omgevingsvariabelen op die moet van het uitvoerbare bestand van de service. Dit is niet beperkt tot alleen uitvoerbare bestanden die zijn geschreven via de Service Fabric-programmeermodellen. Npm.exe moet bijvoorbeeld een aantal omgevingsvariabelen die is geconfigureerd voor het implementeren van een node.js-toepassing.
* Instellen van toegangsbeheer door beveiligingscertificaten te installeren.

Voor meer informatie over het configureren van de **entrypoint**, Zie [het beleid voor een toegangspunt voor service-instellingen configureren](service-fabric-application-runas-security.md)

**Omgevingsvariabelen** (niet ingesteld in het voorgaande voorbeeld) bevat een lijst van omgevingsvariabelen die zijn ingesteld voor dit codepakket. Omgevingsvariabelen kunnen worden overschreven in de `ApplicationManifest.xml` naar verschillende waarden voor verschillende service-exemplaren. 

**Gegevenspakket** (niet ingesteld in het voorgaande voorbeeld) declareert een map met de naam door de **naam** kenmerk met willekeurige statische gegevens om te worden verbruikt door het proces tijdens runtime.

**ConfigPackage** declareert een map met de naam door de **naam** kenmerk dat bevat een *Settings.xml* bestand. Het bestand met instellingen bevat secties van de gebruiker gedefinieerde, sleutel / waarde-paar-instellingen die het proces tijdens de uitvoering leest. Tijdens een upgrade als alleen de **ConfigPackage** **versie** is gewijzigd, en vervolgens het proces dat wordt uitgevoerd niet opnieuw wordt opgestart. In plaats daarvan waarschuwt een retouraanroep het proces dat configuratie-instellingen hebt gewijzigd zodat ze opnieuw kunnen worden geladen dynamisch. Hier volgt een voorbeeld *Settings.xml* bestand:

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

**Resources**, zoals-eindpunten die worden gebruikt door de service worden gedeclareerd of gewijzigd zonder de gecompileerde code te wijzigen.  Toegang tot de resources die zijn opgegeven in het servicemanifest kan worden beheerd via de **beveiligingsgroep** in het toepassingsmanifest.  Wanneer een **eindpunt** resource is gedefinieerd in het servicemanifest, Service Fabric toegewezen poorten van de gereserveerde toepassingspoortbereik wanneer een poort expliciet is niet opgegeven.  Lees meer over [opgeven of het eindpunt resources overschrijven](service-fabric-service-manifest-resources.md).


<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application-in-applicationmanifestxml"></a>Een toepassing in ApplicationManifest.xml beschrijven
Het toepassingsmanifest beschrijving declaratief van het toepassingstype en -versie. Deze geeft service samenstelling metagegevens zoals stabiele namen, partitioneren schema, exemplaar count/replicatie factor/isolatie-beleid, plaatsingsbeperkingen, configuratie onderdrukkingen en samenstellende servicetypen. De loadbalancer-domeinen waarin de toepassing is geplaatst, worden ook beschreven.

Een toepassingsmanifest is dus worden de elementen op toepassingsniveau beschreven en verwijst naar een of meer service-manifesten om op te stellen van een toepassingstype. Hier wordt het toepassingsmanifest voor de [stemmen voorbeeldtoepassing](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart):

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="VotingData_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingData_PartitionCount" DefaultValue="1" />
    <Parameter Name="VotingData_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingWeb_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingDataPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="VotingData">
      <StatefulService ServiceTypeName="VotingDataType" TargetReplicaSetSize="[VotingData_TargetReplicaSetSize]" MinReplicaSetSize="[VotingData_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[VotingData_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    <Service Name="VotingWeb" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="VotingWebType" InstanceCount="[VotingWeb_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

Service manifesten, zoals **versie** kenmerken ongestructureerde tekenreeksen zijn en niet worden geparseerd door het systeem. Versie-kenmerken worden ook gebruikt voor de versie van elk onderdeel voor upgrades.

**Parameters** definieert de parameters die in het toepassingsmanifest wordt gebruikt. De waarden van deze parameters kunnen worden opgegeven wanneer de toepassing instatiated is en toepassing of service-configuratie-instellingen kunt wijzigen.  De standaardwaarde voor de parameter wordt gebruikt als de waarde niet is gewijzigd tijdens instantiëring van toepassing. Zie voor informatie over het onderhouden van de andere toepassing en serviceparameters voor afzonderlijke omgevingen, [beheren voor omgevingen met meerdere parameters voor de toepassing](service-fabric-manage-multiple-environment-app-configuration.md).

**ServiceManifestImport** bevat verwijzingen naar service manifesten waaruit dit toepassingstype. Een toepassingsmanifest kan meerdere service manifest invoer bevatten, elk criterium kan samengestelde onafhankelijk. Geïmporteerde service manifesten bepalen welke servicetypen zijn geldig binnen dit toepassingstype. Binnen de ServiceManifestImport overschrijven u configuratiewaarden in Settings.xml en omgevingsvariabelen in ServiceManifest.xml bestanden. **Beleid** (niet ingesteld in het voorgaande voorbeeld) voor het eindpunt binding, beveiliging en toegang en pakket delen kan worden ingesteld op een geïmporteerde service manifest.  Zie voor meer informatie [beveiligingsbeleid voor uw toepassing configureren](service-fabric-application-runas-security.md).

**DefaultServices** declareert service-exemplaren die automatisch worden gemaakt wanneer een toepassing wordt gestart op basis van dit toepassingstype. Standaardservices zijn alleen gemakkelijker te maken en zich gedragen als normale services in elke opzicht nadat ze zijn gemaakt. Deze samen met andere services in het toepassingsexemplaar zijn bijgewerkt en kunnen ook worden verwijderd. Een toepassingsmanifest kan meerdere standaardservices bevatten.

**Certificaten** (niet ingesteld in het voorgaande voorbeeld) wordt aangegeven dat de certificaten voor [setup-HTTPS-eindpunten](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service) of [versleutelen geheimen in het toepassingsmanifest](service-fabric-application-secret-management.md).

**Beleid** (niet ingesteld in het voorgaande voorbeeld) beschrijft de logboekverzameling [standaard run as](service-fabric-application-runas-security.md), [health](service-fabric-health-introduction.md#health-policies), en [beveiligingstoegang](service-fabric-application-runas-security.md) beleid instellen op het niveau van de toepassing.

**Principals** (niet ingesteld in het voorgaande voorbeeld) beschrijven de beveiligings-principals (gebruikers of groepen) vereist voor het [uitvoeren services en beveiligde bronnen](service-fabric-application-runas-security.md).  Principals wordt verwezen in de **beleid** secties.





<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Volgende stappen
- [Een toepassing van het pakket](service-fabric-package-apps.md) en klaar om te implementeren.
- [Implementeren en verwijderen van toepassingen](service-fabric-deploy-remove-applications.md).
- [Configureer de parameters en variabelen voor exemplaren van een andere toepassing](service-fabric-manage-multiple-environment-app-configuration.md).
- [Beveiligingsbeleid voor uw toepassing configureren](service-fabric-application-runas-security.md).
- [Setup-HTTPS-eindpunten](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service).
- [Geheimen in het toepassingsmanifest versleutelen](service-fabric-application-secret-management.md)

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png



