---
title: Service Fabric service-eindpunten op te geven | Microsoft Docs
description: Hoe om te beschrijven van de eindpunt-resources in een servicemanifest, met inbegrip van het HTTPS-eindpunten instellen
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: da36cbdb-6531-4dae-88e8-a311ab71520d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 8707a9cb90afe1bf72f3aef6377f8ada409a1c64
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60837756"
---
# <a name="specify-resources-in-a-service-manifest"></a>Resources specificeren in een servicemanifest
## <a name="overview"></a>Overzicht
Het servicemanifest kan resources die worden gebruikt door de service worden gedeclareerd/gewijzigd zonder de gecompileerde code te wijzigen. Azure Service Fabric biedt ondersteuning voor configuratie van eindpunt resources voor de service. De toegang tot de resources die zijn opgegeven in het servicemanifest kan worden beheerd via de Toewijzingsmodule in het toepassingsmanifest. De declaratie van resources kunt deze resources worden gewijzigd tijdens de implementatie, wat betekent dat de service niet hoeft te introduceren een nieuwe configuratie-mechanisme. De schemadefinitie voor het bestand ServiceManifest.xml is geïnstalleerd met de Service Fabric SDK en hulpprogramma's voor *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

## <a name="endpoints"></a>Eindpunten
Wanneer een eindpuntresource in het servicemanifest wordt gedefinieerd, wijst Service Fabric poorten van het gereserveerde bereik toe wanneer een poort niet expliciet is opgegeven. Zoek op het eindpunt *ServiceEndpoint1* opgegeven in het manifest codefragment na dit lid. Services kunnen bovendien ook aanvragen voor een specifieke poort in een resource. Service-replica's die worden uitgevoerd op andere knooppunten kunnen worden toegewezen worden andere poortnummers, terwijl de poort voor replica's van een service die wordt uitgevoerd op hetzelfde knooppunt delen. De service-replica's kunnen vervolgens deze poorten gebruiken indien nodig voor de replicatie en luisteren naar aanvragen van clients.

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

Als er meerdere pakketten in een pakket met één service zijn, wordt het codepakket ook moet worden verwezen de **eindpunten** sectie.  Bijvoorbeeld, als **ServiceEndpoint2a** en **ServiceEndpoint2b** eindpunten van het pakket met dezelfde service verwijst naar een andere codepakketten, het codepakket overeenkomt met elk eindpunt is Er wordt duidelijk gemaakt als volgt:

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint2a" Protocol="http" Port="802" CodePackageRef="Code1"/>
    <Endpoint Name="ServiceEndpoint2b" Protocol="http" Port="801" CodePackageRef="Code2"/>
  </Endpoints>
</Resources>
```

Raadpleeg [stateful Reliable Services configureren](service-fabric-reliable-services-configuration.md) lezen meer informatie over die verwijzen naar eindpunten van de configuratie-instellingen voor pakketten (settings.xml)-bestand.

## <a name="example-specifying-an-http-endpoint-for-your-service"></a>Voorbeeld: een HTTP-eindpunt voor uw service opgeven
Het volgende servicemanifest definieert een TCP-endpoint-resource en twee HTTP-eindpunt resources in de &lt;Resources&gt; element.

HTTP-eindpunten worden automatisch dat ACL zouden op Service Fabric.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         This name must match the string used in the RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by the replicator for replicating the state of your service.
           This endpoint is configured through the ReplicatorSettings config section in the Settings.xml
           file under the ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a>Voorbeeld: op te geven van een HTTPS-eindpunt voor uw service
Het HTTPS-protocol biedt verificatie van de server en wordt ook gebruikt voor het versleutelen van communicatie tussen de client-server. U HTTPS hebt ingeschakeld voor uw Service Fabric-service, geef het protocol in de *eindpunten-Resources > Endpoint ->* gedeelte van het servicemanifest, zoals eerder besproken voor het eindpunt *ServiceEndpoint3*.

> [!NOTE]
> Een service-protocol kan niet worden gewijzigd tijdens de upgrade van de toepassing. Als deze wordt gewijzigd tijdens de upgrade, is een belangrijke wijziging.
> 

> [!WARNING] 
> Wanneer u HTTPS gebruikt, gebruik niet de dezelfde poort en het certificaat dat voor andere service-exemplaren (onafhankelijk van de toepassing) is geïmplementeerd op hetzelfde knooppunt. Upgraden van twee verschillende services met behulp van dezelfde poort in verschillende toepassingsinstanties resulteert in een upgrade mislukt. Zie voor meer informatie, [bijwerken van meerdere toepassingen met HTTPS-eindpunten ](service-fabric-application-upgrade.md#upgrading-multiple-applications-with-https-endpoints).
>

Hier volgt een voorbeeld ApplicationManifest die u nodig hebt om in te stellen voor HTTPS. De vingerafdruk van het certificaat moet worden opgegeven. De EndpointRef is een verwijzing naar EndpointResource in ServiceManifest, waarvoor u het HTTPS-protocol hebt ingesteld. U kunt meer dan één EndpointCertificate toevoegen.  

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_ ]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```

Voor Linux-clusters, de **MY** standaard ingesteld op de map opslaan **/var/lib/sfcerts**.


## <a name="overriding-endpoints-in-servicemanifestxml"></a>Eindpunten in ServiceManifest.xml overschreven

In de ApplicationManifest toevoegen een sectie ResourceOverrides, die zich op hetzelfde niveau ConfigOverrides sectie. In deze sectie kunt u de onderdrukking voor de eindpunten in de sectie met resources in het servicemanifest opgegeven. Onderdrukken van eindpunten wordt ondersteund in runtime 5.7.217/SDK 2.7.217 en hoger.

Als u wilt overschrijven eindpunt in het ServiceManifest ApplicationParameters wijzigen de ApplicationManifest gebruiken als volgt:

Voeg een nieuwe sectie 'ResourceOverrides' in de sectie ServiceManifestImport.

```xml
<ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <ResourceOverrides>
      <Endpoints>
        <Endpoint Name="ServiceEndpoint" Port="[Port]" Protocol="[Protocol]" Type="[Type]" />
        <Endpoint Name="ServiceEndpoint1" Port="[Port1]" Protocol="[Protocol1] "/>
      </Endpoints>
    </ResourceOverrides>
        <Policies>
           <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint"/>
        </Policies>
  </ServiceManifestImport>
```

In de Parameters die hieronder worden toevoegen:

```xml
  <Parameters>
    <Parameter Name="Port" DefaultValue="" />
    <Parameter Name="Protocol" DefaultValue="" />
    <Parameter Name="Type" DefaultValue="" />
    <Parameter Name="Port1" DefaultValue="" />
    <Parameter Name="Protocol1" DefaultValue="" />
  </Parameters>
```

Tijdens het implementeren van de toepassing kunt u deze waarden als ApplicationParameters doorgeven.  Bijvoorbeeld:

```powershell
PS C:\> New-ServiceFabricApplication -ApplicationName fabric:/myapp -ApplicationTypeName "AppType" -ApplicationTypeVersion "1.0.0" -ApplicationParameter @{Port='1001'; Protocol='https'; Type='Input'; Port1='2001'; Protocol='http'}
```

Opmerking: Als de waarden voor de ApplicationParameters leeg is, teruggaan we naar de standaardwaarde die is opgegeven in het bestand servicemanifest.XML voor de bijbehorende EndPointName.

Bijvoorbeeld:

Als in het bestand servicemanifest.XML die u hebt opgegeven

```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint1" Protocol="tcp"/>
    </Endpoints>
  </Resources>
```

En de waarde Port1 en Protocol1 voor parameters voor de toepassing is null of leeg zijn. De poort wordt nog steeds bepaald door de service fabric. En de TCP-Protocol wordt.

Stel dat u een onjuiste waarde opgeven. Net als voor de poort u hebt opgegeven een string-waarde "Foo" in plaats van een int.  Nieuwe ServiceFabricApplication opdracht mislukt met fout: De parameter overschrijven met de naam 'ServiceEndpoint1' kenmerk 'Port1' in de sectie 'ResourceOverrides' is ongeldig. De opgegeven waarde is 'Foo' en 'integer' vereist is.
