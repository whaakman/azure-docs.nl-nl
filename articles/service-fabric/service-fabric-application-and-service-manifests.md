---
title: Met een beschrijving van Azure Service Fabric-apps en -services | Microsoft Docs
description: Hierin wordt beschreven hoe manifesten worden gebruikt om Service Fabric-toepassingen en services te beschrijven.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/19/2018
ms.author: ryanwi
ms.openlocfilehash: fa3c94854f8f67da9caa2955f5c62581843e39c8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55156102"
---
# <a name="service-fabric-application-and-service-manifests"></a>Service Fabric-toepassing- en servicemanifesten
Dit artikel wordt beschreven hoe Service Fabric-toepassingen en services worden gedefinieerd en is samengesteld met behulp van de bestanden ApplicationManifest.xml en ServiceManifest.xml.  Zie voor meer voorbeelden van gedetailleerde [toepassing en service manifest van de voorbeelden](service-fabric-manifest-examples.md).  Het XML-schema voor deze manifestbestanden wordt gedocumenteerd in [ServiceFabricServiceModel.xsd schemadocumentatie](service-fabric-service-model-schema.md).

> [!WARNING]
> Het manifest schema van de XML-bestand dwingt de juiste volgorde van de onderliggende elementen.  Als een gedeeltelijke oplossing "C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd" in Visual Studio te openen tijdens het ontwerpen of wijzigen van een van de Service Fabric-manifesten. Hiermee kunt u om te controleren of de volgorde van de onderliggende elementen en biedt beschikt.

## <a name="describe-a-service-in-servicemanifestxml"></a>Een service in ServiceManifest.xml beschrijven
Het servicemanifest definieert declaratief het servicetype en de versie. Het geeft de metagegevens zoals servicetype, health eigenschappen, taakverdeling metrische gegevens, de binaire bestanden en -configuratiebestanden.  Anders gezegd, beschrijft de code, configuratie en gegevens pakketten waaruit een servicepakket ter ondersteuning van een of meer servicetypen. Een servicemanifest kan bevatten meerdere code, configuratie en gegevenspakketten die onafhankelijk is samengesteld kunnen zijn. Dit is een servicemanifest voor de ASP.NET Core web-front-endservice van de [stemmen voorbeeldtoepassing](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart) (en hier volgen enkele [meer gedetailleerde voorbeelden van](service-fabric-manifest-examples.md)):

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

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an 
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

**Versie** kenmerken zijn tekenreeksen met niet-gestructureerde en niet worden geparseerd door het systeem. Versie-kenmerken worden gebruikt voor de versie van elk onderdeel voor upgrades.

**ServiceTypes** wordt gedeclareerd welke servicetypen worden ondersteund door **CodePackages** in dit manifest. Wanneer een service wordt gestart op basis van een van deze servicetypen, worden alle pakketten gedefinieerd in deze manifest geactiveerd door het uitvoeren van hun toegangspunten. De resulterende processen worden verwacht voor het registreren van de typen ondersteunde service tijdens runtime. Service-typen zijn gedefinieerd op het niveau van het manifest en niet het niveau van de code-pakket. Dus wanneer er meerdere pakketten, worden alle geactiveerd wanneer er wordt gezocht naar een van de gedeclareerde servicetypen.

Het uitvoerbare bestand opgegeven door **EntryPoint** is doorgaans de ServiceHost langlopende. **SetupEntryPoint** is een privileged ingangspunt gebruikt dat wordt uitgevoerd met dezelfde referenties als Service Fabric (meestal de *LocalSystem* account) voordat u een ander toegangspunt.  De aanwezigheid van een toegangspunt voor de afzonderlijke instellingen zo voorkomt u dat de ServiceHost met hoge bevoegdheden voor langere tijd worden uitgevoerd. Het uitvoerbare bestand opgegeven door **EntryPoint** wordt uitgevoerd na **SetupEntryPoint** is afgesloten. Als het proces wordt beëindigd ooit of vastloopt, het resulterende proces wordt bewaakt en opnieuw opgestart (opnieuw vanaf **SetupEntryPoint**).  

Typische scenario's voor het gebruik van **SetupEntryPoint** zijn wanneer u een uitvoerbaar bestand voordat u de service wordt gestart uitvoeren of u een bewerking met verhoogde bevoegdheden uit te voeren. Bijvoorbeeld:

* Instellen van en het initialiseren van omgevingsvariabelen die het uitvoerbare bestand van de service nodig heeft. Dit is niet beperkt tot alleen uitvoerbare bestanden die zijn geschreven via de Service Fabric-programmeermodellen. Npm.exe moet bijvoorbeeld enkele omgevingsvariabelen die is geconfigureerd voor het implementeren van een node.js-toepassing.
* Toegangsbeheer instellen door het installeren van beveiligingscertificaten.

Zie voor meer informatie over het configureren van de SetupEntryPoint [het beleid voor een toegangspunt voor service-instellingen configureren](service-fabric-application-runas-security.md)

**Omgevingsvariabelen** (niet ingesteld in het vorige voorbeeld) bevat een lijst van omgevingsvariabelen die zijn ingesteld voor dit codepakket. Omgevingsvariabelen kunnen worden overschreven in de `ApplicationManifest.xml` voor verschillende waarden voor verschillende service-exemplaren. 

**Gegevenspakket** (niet ingesteld in het vorige voorbeeld) declareert een map met de naam van de **naam** kenmerk met willekeurige statische gegevens om te worden verbruikt door het proces tijdens runtime.

**ConfigPackage** declareert u een map met de naam van de **naam** kenmerk dat bevat een *Settings.xml* bestand. Het instellingenbestand bevat secties van de gebruiker gedefinieerde, sleutel / waarde-paar instellingen die het proces weer tijdens runtime leest. Tijdens een upgrade wordt als alleen de **ConfigPackage** **versie** is gewijzigd, en vervolgens het proces dat wordt uitgevoerd niet opnieuw wordt opgestart. In plaats daarvan een retouraanroep hoogte gebracht van het proces dat configuratie-instellingen hebt gewijzigd, zodat ze kunnen dynamisch worden geladen. Hier volgt een voorbeeld *Settings.xml* bestand:

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

**Resources**, zoals eindpunten die worden gebruikt door de service worden gedeclareerd/gewijzigd zonder de gecompileerde code te wijzigen.  Toegang tot de resources die zijn opgegeven in het servicemanifest kan worden beheerd via de **Toewijzingsmodule** in het toepassingsmanifest.  Wanneer een **eindpunt** resource is gedefinieerd in het servicemanifest, Service Fabric poorten van het gereserveerde bereik toegewezen wanneer een poort niet expliciet is opgegeven.  Meer informatie over [op te geven of het vervangen van eindpunt resources](service-fabric-service-manifest-resources.md).


<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application-in-applicationmanifestxml"></a>Een toepassing in ApplicationManifest.xml beschrijven
Het toepassingsmanifest beschrijving declaratief van het toepassingstype en de versie. Het geeft de metagegevens van samenstelling zoals stabiele namen, schema, exemplaar, aantal/replicatiefactor, isolatie/beleid, plaatsingsbeperkingen, configuratie van onderdrukkingen en samenstellende servicetypen partitioneren. De load-balancing domeinen waarin de toepassing is geplaatst, worden ook beschreven.

Manifest van de toepassing worden elementen op toepassingsniveau en dus verwijst naar een of meer servicemanifesten voor het opstellen van een toepassingstype. Hier volgt een manifest voor de toepassing voor de [stemmen voorbeeldtoepassing](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart) (en hier volgen enkele [meer gedetailleerde voorbeelden van](service-fabric-manifest-examples.md)):

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

Servicemanifesten, zoals **versie** kenmerken zijn tekenreeksen met niet-gestructureerde en niet worden geparseerd door het systeem. Versie-kenmerken worden ook gebruikt voor de versie van elk onderdeel voor upgrades.

**Parameters** definieert de parameters gebruikt in het toepassingsmanifest. De waarden van deze parameters kunnen worden opgegeven wanneer de toepassing wordt gestart en toepassing of service configuratie-instellingen overschrijven kunt.  De standaardwaarde voor de parameter wordt gebruikt als de waarde niet is gewijzigd tijdens instantiëring van toepassing. Zie voor informatie over het onderhouden van andere toepassing en serviceparameters voor afzonderlijke omgevingen, [parameters voor de toepassing voor meerdere omgevingen beheren](service-fabric-manage-multiple-environment-app-configuration.md).

**ServiceManifestImport** bevat verwijzingen naar servicemanifesten waaruit deze toepassingstype. Manifest van de toepassing kan meerdere service manifest invoer bevatten, elk kan worden bijgehouden onafhankelijk van elkaar. Geïmporteerde servicemanifesten te bepalen welke servicetypen zijn geldig binnen dit toepassingstype. Binnen de ServiceManifestImport overschrijven u configuratiewaarden in Settings.xml en omgevingsvariabelen in ServiceManifest.xml bestanden. **Beleid** (niet ingesteld in het voorgaande voorbeeld) voor het eindpunt binding, beveiliging en toegang en pakket delen kan worden ingesteld voor geïmporteerde servicemanifesten.  Zie voor meer informatie, [beveiligingsbeleid configureren voor uw toepassing](service-fabric-application-runas-security.md).

**DefaultServices** declareert de service-exemplaren die automatisch worden gemaakt wanneer een toepassing wordt geïnstantieerd op basis van dit toepassingstype. Standaard-services zijn alleen uw gemak en zich gedraagt als normale services in alle opzichten nadat ze zijn gemaakt. Ze worden bijgewerkt, samen met eventuele andere services in een exemplaar van de toepassing en kunnen ook worden verwijderd. Manifest van de toepassing kan meerdere standaardservices bevatten.

**Certificaten** (niet ingesteld in het vorige voorbeeld) verklaart de certificaten voor het [HTTPS-eindpunten instellen](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service) of [versleutelen geheimen in het toepassingsmanifest](service-fabric-application-secret-management.md).

**Beleid** (niet ingesteld in het vorige voorbeeld) beschrijft de logboekverzameling [standaard run as](service-fabric-application-runas-security.md), [health](service-fabric-health-introduction.md#health-policies), en [beveiligingstoegang](service-fabric-application-runas-security.md) beleid om in te stellen op de niveau van de toepassing.

**Beveiligings-principals** (niet ingesteld in het voorgaande voorbeeld) de beveiligings-principals (gebruikers of groepen) vereist om te beschrijven [uitvoeren services en bronnen van veilige service](service-fabric-application-runas-security.md).  Beveiligings-principals zijn waarnaar wordt verwezen in de **beleid** secties.





<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Volgende stappen
- [Een toepassing verpakken](service-fabric-package-apps.md) en geef deze klaar om te implementeren.
- [Implementeren en verwijderen van toepassingen](service-fabric-deploy-remove-applications.md).
- [Configureren van de parameters en variabelen voor exemplaren van een andere toepassing](service-fabric-manage-multiple-environment-app-configuration.md).
- [Beveiligingsbeleid configureren voor uw toepassing](service-fabric-application-runas-security.md).
- [Setup-HTTPS-eindpunten](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service).
- [Geheimen in het toepassingsmanifest versleutelen](service-fabric-application-secret-management.md)

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png



