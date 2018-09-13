---
title: Azure Service Fabric reliable services-toepassing manifest voorbeelden | Microsoft Docs
description: Meer informatie over het configureren van de toepassing en service-manifest instellingen voor een reliable services-Service Fabric-toepassing.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: xml
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/11/2018
ms.author: ryanwi
ms.openlocfilehash: c90715608b5d35520605c504b5cebb5e7a3ec021
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35644253"
---
# <a name="reliable-services-application-and-service-manifest-examples"></a>Voorbeelden van toepassings- en servicemanifesten voor betrouwbare services
Hier volgen enkele voorbeelden van de toepassing en service manifesten voor een Service Fabric-toepassing met een ASP.NET Core web-front-end en een stateful back-end. Het doel van deze voorbeelden is om weer te geven welke instellingen zijn beschikbaar en het gebruik ervan. Deze toepassing en service manifesten zijn gebaseerd op de [Snelstartgids voor Service Fabric .NET](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) manifesten.

De volgende functies worden weergegeven:
|Manifest|Functies|
|---|---|
|[Toepassingsmanifest](#application-manifest)| [resourcebeheer](service-fabric-resource-governance.md), [een service uitvoeren als een lokaal Administrator-account](service-fabric-application-runas-security.md), [een standaardbeleid toepassen op alle code servicepakketten](service-fabric-application-runas-security.md#apply-a-default-policy-to-all-service-code-packages), [maken van gebruiker en groep-principals](service-fabric-application-runas-security.md), een gegevenspakket tussen service-exemplaren delen [overschrijven van de service-eindpunten](service-fabric-service-manifest-resources.md#overriding-endpoints-in-servicemanifestxml)| 
|[FrontEndService servicemanifest](#frontendservice-service-manifest)| [Een script uitvoeren bij het opstarten van de service](service-fabric-run-script-at-service-startup.md), [een HTTPS-eindpunt definiëren](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#define-an-https-endpoint-in-the-service-manifest) | 
|[BackEndService servicemanifest](#backendservice-service-manifest)| [Declareer een configuratiepakket](service-fabric-application-and-service-manifests.md), [declareert een gegevenspakket](service-fabric-application-and-service-manifests.md), [een eindpunt configureren](service-fabric-service-manifest-resources.md)| 

Zie [manifest elementen van de toepassing](#application-manifest-elements), [VotingWeb-service-manifest elementen](#votingweb-service-manifest-elements), en [VotingData service manifest elementen](#votingdata-service-manifest-elements) voor meer informatie over specifieke XML elementen.

## <a name="application-manifest"></a>Manifest van de toepassing

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="VotingData_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingData_PartitionCount" DefaultValue="1" />
    <Parameter Name="VotingData_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingWeb_InstanceCount" DefaultValue="-1" />
    <Parameter Name="CpuCores" DefaultValue="2" />
    <Parameter Name="Memory" DefaultValue="4084" />
    <Parameter Name="BlockIOWeight" DefaultValue="200" />
    <Parameter Name="MaximumIOBandwidth" DefaultValue="1024" />
    <Parameter Name="MemoryReservationInMB" DefaultValue="1024" />
    <Parameter Name="MemorySwapInMB" DefaultValue="4084"/>
    <Parameter Name="Port" DefaultValue="8081" />
    <Parameter Name="Protocol" DefaultValue="tcp" />
    <Parameter Name="Type" DefaultValue="internal" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingDataPkg" ServiceManifestVersion="1.0.0" />
    <!-- Override endpoints declared in the service manifest. -->
    <ResourceOverrides>
      <Endpoints>
        <Endpoint Name="DataEndpoint" Port="[Port]" Protocol="[Protocol]" Type="[Type]" />
      </Endpoints>
    </ResourceOverrides>

    <!-- Policies to be applied to the imported service manifest. -->
    <Policies>
      
      <!-- Set resource governance at the service package level. -->
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]" MemoryInMB="[Memory]"/>

      <!-- Set resource governance at the code package level. -->
      <ResourceGovernancePolicy CodePackageRef="Code" CpuPercent="10" MemoryInMB="[Memory]" BlockIOWeight="[BlockIOWeight]" 
                                MaximumIOBandwidth="[MaximumIOBandwidth]" MaximumIOps="[MaximumIOps]" MemoryReservationInMB="[MemoryReservationInMB]" 
                                MemorySwapInMB="[MemorySwapInMB]"/>

      <!-- Share the data package across multiple instances of the VotingData service-->
      <PackageSharingPolicy PackageRef="VotingDataPkg.Data"/>

      <!-- Give read rights on the "DataEndpoint" endpoint to the Customer2 account.-->
      <SecurityAccessPolicy GrantRights="Read" PrincipalRef="Customer2" ResourceRef="DataEndpoint" ResourceType="Endpoint"/>         
    </Policies>
  </ServiceManifestImport>
  
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="1.0.0" />
    
    <!-- Policies to be applied to the imported service manifest. -->
    <Policies>
      <!-- Run the setup entry point (defined in the imported service manifest) as the SetupAdminUser account 
      (declared in the following Principals section). -->
      <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      
    </Policies>

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
  <!-- Define users and groups required to run the services and access resources.  Principals are used in the Policies section(s). -->
  <Principals>
    <!-- Declare a set of groups as security principals, which can be referenced in policies. Groups are useful if there are multiple users 
    for different service entry points and they need to have certain common privileges that are available at the group level. -->
    <Groups>
      <!-- Create a group that has administrator privileges. -->
      <Group Name="LocalAdminGroup">
        <Membership>
          <SystemGroup Name="Administrators" />
        </Membership>
      </Group>
    </Groups>
    <Users>
      <!-- Declare a user and add the user to the Administrators system group. The SetupAdminUser account is used to run the 
      setup entry point of the VotingWebPkg code package (described in the preceding Policies section).-->
      <User Name="SetupAdminUser">
        <MemberOf>
          <SystemGroup Name="Administrators" />
        </MemberOf>
      </User>
      <!-- Create a user. Local user accounts are created on the machines where the application is deployed. By default, these accounts 
      do not have the same names as those specified here. Instead, they are dynamically generated and have random passwords. -->
      <User Name="Customer1" >
        <MemberOf>
          <!-- Add the user to the local administrators group.-->
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
      <!-- Create a user as a local user with the specified account name and password. Local user accounts are created on the machines 
      where the application is deployed. -->
      <User Name="Customer2" AccountType="LocalUser" AccountName="Customer1" Password="MyPassword">
        <MemberOf>
          <!-- Add the user to the local administrators group.-->
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
      <!-- Create a user as NetworkService. -->
      <User Name="MyDefaultAccount" AccountType="NetworkService" />      
    </Users>
  </Principals>
  <!-- Policies applied at the application level. -->
  <Policies>
    <!-- Specify a default user account for all code packages that don’t have a specific RunAsPolicy defined in 
    the ServiceManifestImport section(s). -->
    <DefaultRunAsPolicy UserRef="MyDefaultAccount" />
    
  </Policies>
</ApplicationManifest>

```

## <a name="votingweb-service-manifest"></a>VotingWeb-servicemanifest

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
    <!-- A privileged entry point that by default runs with the same credentials as Service Fabric (typically the NetworkService account) before 
    any other entry point. Use the setup entry point to set system environment variables, give the account running the service (NETWORK SERVICE, by default) 
    access to a certificate's private key, or perform other setup tasks. In the application manifest, you can change the security permissions to run the startup script 
    under a local system account or an administrator account.  -->
    <SetupEntryPoint>
      <ExeHost>
        <!-- The setup script to run. -->
        <Program>Setup.bat</Program>
        <!-- Pass arguments to the script when it runs.-->
        <Arguments>MyValue</Arguments>
        <!-- The working directory for the process in the code package on the node where the application is deployed. CodePackage sets the working directory to be 
        the root of the code package regardless of where the EXE is defined in the code package directory. This is where the processes can write the data. Writing data 
        in the code package or code base is not recommended as those folders could be shared between different application instances and may get deleted.-->
        <WorkingFolder>CodePackage</WorkingFolder>
        <!-- Warning! Do not use console rediriction in a production application, only use it for local development and debugging. Redirects console output from the startup
        script to an output file in the application folder called "log" on the cluster node where the application is deployed and run. Also set the number of output files
        to retain and the maximum file size (in KB). -->
        <ConsoleRedirection FileRetentionCount="10" FileMaxSizeInKb="20480"/>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <!-- The working directory for the process in the code package on the node where the application is deployed. CodePackage sets the working directory to be 
        the root of the code package regardless of where the EXE is defined in the code package directory. This is where the processes can write the data. Writing data 
        in the code package or code base is not recommended as those folders could be shared between different application instances and may get deleted.-->
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- Configure a HTTPS endpoint on port 443. This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="https" Name="EndpointHttps" Type="Input" Port="443" />
    </Endpoints>
  </Resources>
</ServiceManifest>

```

## <a name="votingdata-service-manifest"></a>VotingData-servicemanifest

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingDataPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType. 
         This name must match the string used in RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="VotingDataType"  HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingData.exe</Program>
        <!-- The working directory for the process in the code package on the node where the application is deployed. CodePackage sets the working directory to be 
        the root of the code package regardless of where the EXE is defined in the code package directory. This is where the processes can write the data. Writing data 
        in the code package or code base is not recommended as those folders could be shared between different application instances and may get deleted.-->
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Declares a folder, named by the Name attribute, under PackageRoot that contains a Settings.xml file. This file contains sections of user-defined, 
  key-value pair settings that the process can read back at run time. During an upgrade, if only the ConfigPackage version has changed, 
  then the running process is not restarted. Instead, a callback notifies the process that configuration settings have changed so they can be reloaded dynamically. -->
  <ConfigPackage Name="Config" Version="1.0.0" />
  
  <!-- Declares a folder, named by the Name attribute, under PackageRoot which contains static data files to be consumed by the process at run time. -->
  <DataPackage Name="Data" Version="1.0.0"/>

  <Resources>
    <Endpoints>
      <!-- Define an internal (used for intra-application communication) TCP endpoint. Since no port is specified, one is created and assigned dynamically
           to the service.-->
      <Endpoint Name="DataEndpoint" Protocol="tcp" Type="Internal" />
    </Endpoints>
  </Resources>

</ServiceManifest>

```

## <a name="application-manifest-elements"></a>Manifest elementen van de toepassing
### <a name="applicationmanifest-element"></a>ApplicationManifest-Element
Declaratief beschrijving van het toepassingstype en versie. Een of meer servicemanifesten van de bijbehorende services wordt voor het opstellen van een toepassingstype verwezen. Configuratie-instellingen van de bijbehorende services kunnen worden genegeerd met geparameteriseerde toepassingsinstellingen. Standaard services, servicesjablonen, beveiligings-principals, beleidsregels, diagnostische gegevens over configuratie en certificaten kan ook worden gedefinieerd op het toepassingsniveau van de. Zie voor meer informatie, [ApplicationManifest-Element](service-fabric-service-model-schema-elements.md#ApplicationManifestElementApplicationManifestTypeComplexType)

### <a name="parameters-element"></a>Parameters-Element
Verklaart de parameters die worden gebruikt in het manifest van deze toepassing. De waarde van deze parameters kan worden opgegeven wanneer de toepassing wordt gestart en kan worden gebruikt voor de onderdrukking van de toepassing of service configuratie-instellingen. Zie voor meer informatie, [Parameters-Element](service-fabric-service-model-schema-elements.md#ParametersElementanonymouscomplexTypeComplexTypeDefinedInApplicationManifestTypecomplexType)

### <a name="parameter-element"></a>Parameter-Element
Een parameter van de toepassing moet worden gebruikt in dit manifest. De waarde van parameter kan worden gewijzigd tijdens instantiëring van toepassing, of, als er geen waarde is opgegeven de standaardwaarde wordt gebruikt. Zie voor meer informatie, [Parameter-Element](service-fabric-service-model-schema-elements.md#ParameterElementanonymouscomplexTypeComplexTypeDefinedInParameterselement)

### <a name="servicemanifestimport-element"></a>ServiceManifestImport-Element
Hiermee importeert u een servicemanifest die zijn gemaakt door de ontwikkelaar van de service. Een servicemanifest moet voor elke samenstellende service in de toepassing worden geïmporteerd. Configuratie overschreven en beleid kunnen worden gedeclareerd voor het servicemanifest. Zie voor meer informatie, [ServiceManifestImport-Element](service-fabric-service-model-schema-elements.md#ServiceManifestImportElementanonymouscomplexTypeComplexTypeDefinedInApplicationManifestTypecomplexType)

### <a name="servicemanifestref-element"></a>ServiceManifestRef-Element
Hiermee importeert u het servicemanifest met verwijzing. Op dit moment moet het manifestbestand van de service (ServiceManifest.xml) aanwezig zijn in de build-pakket. Zie voor meer informatie, [ServiceManifestRef-Element](service-fabric-service-model-schema-elements.md#ServiceManifestRefElementServiceManifestRefTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="resourceoverrides-element"></a>ResourceOverrides-Element
Hiermee geeft u een resource onderdrukkingen voor eindpunten die zijn gedeclareerd in het servicemanifest-resources. Zie voor meer informatie, [ResourceOverrides-Element](service-fabric-service-model-schema-elements.md#ResourceOverridesElementResourceOverridesTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="endpoints-element"></a>Eindpunten-Element
De eindpunt(en) om op te heffen. Zie voor meer informatie, [eindpunten-Element](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourceOverridesTypecomplexType)

### <a name="endpoint-element"></a>Eindpuntelement
Het eindpunt, gedefinieerd in het servicemanifest, om op te heffen. Zie voor meer informatie, [Endpoint-Element](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)

### <a name="policies-element"></a>Beleid-Element
Beschrijving van het beleid (eindpunt binding, delen, run as-pakket en beveiliging toegang) moet worden toegepast op het geïmporteerde servicemanifest. Zie voor meer informatie, [beleid-Element](service-fabric-service-model-schema-elements.md#PoliciesElementServiceManifestImportPoliciesTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="servicepackageresourcegovernancepolicy-element"></a>ServicePackageResourceGovernancePolicy-Element
Hiermee definieert u de resource governance-beleid dat wordt toegepast op het niveau van de hele service-pakket. Zie voor meer informatie, [ServicePackageResourceGovernancePolicy-Element](service-fabric-service-model-schema-elements.md#ServicePackageResourceGovernancePolicyElementServicePackageResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInServicePackageTypecomplexType)

### <a name="resourcegovernancepolicy-element"></a>ResourceGovernancePolicy-Element
Hiermee geeft u voor een codepackage. Zie voor meer informatie, [ResourceGovernancePolicy-Element](service-fabric-service-model-schema-elements.md#ResourceGovernancePolicyElementResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelementDefinedInDigestedEndpointelement)

### <a name="packagesharingpolicy-element"></a>PackageSharingPolicy-Element
Hiermee wordt aangegeven als een pakket code, configuratie of gegevens moet worden verdeeld over service-exemplaren van hetzelfde servicetype. Zie voor meer informatie, [PackageSharingPolicy-Element](service-fabric-service-model-schema-elements.md#PackageSharingPolicyElementPackageSharingPolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexType)

### <a name="securityaccesspolicy-element"></a>SecurityAccessPolicy-Element
Verleent toegang tot machtigingen aan een principal voor een bron (zoals een eindpunt) die zijn gedefinieerd in een servicemanifest. Normaal gesproken is het handig om te beheren en beperken de toegang van services voor verschillende bronnen om te voorkomen dat beveiligingsrisico. Dit is vooral belangrijk wanneer de toepassing is gemaakt van een verzameling van vanuit een marketplace-services die zijn ontwikkeld door andere ontwikkelaars. Zie voor meer informatie, [SecurityAccessPolicy-Element](service-fabric-service-model-schema-elements.md#SecurityAccessPolicyElementSecurityAccessPolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInSecurityAccessPolicieselementDefinedInDigestedEndpointelement)

### <a name="runaspolicy-element"></a>RunAsPolicy-Element
Hiermee geeft u op de lokale gebruiker of het lokale systeemaccount een servicecodepakket wordt uitgevoerd. Domeinaccounts worden ondersteund op Windows Server-implementaties waar Azure Active Directory beschikbaar is. Toepassingen worden standaard uitgevoerd onder het account dat de Fabric.exe-proces wordt uitgevoerd onder. Toepassingen kunnen ook worden uitgevoerd als andere accounts, die moeten worden gedeclareerd in de sectie beveiligings-Principals. Als u een Run as-beleid op een service toepassen en het servicemanifest verklaart eindpunt resources met de HTTP-protocol, u ook een SecurityAccessPolicy om ervoor te zorgen opgeven moet dat de poorten toegewezen aan zijn deze eindpunten correct toegangsbeheer die worden weergegeven voor het uitvoeren als het gebruikersaccount dat de service wordt uitgevoerd onder. Voor een HTTPS-eindpunt hebt u ook voor het definiëren van een EndpointBindingPolicy om aan te geven van de naam van het certificaat om terug te keren naar de client. Zie voor meer informatie, [RunAsPolicy-Element](service-fabric-service-model-schema-elements.md#RunAsPolicyElementRunAsPolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="defaultservices-element"></a>DefaultServices-Element
Verklaart de exemplaren van de service die automatisch worden gemaakt wanneer een toepassing wordt geïnstantieerd op basis van dit toepassingstype. Zie voor meer informatie, [DefaultServices-Element](service-fabric-service-model-schema-elements.md#DefaultServicesElementDefaultServicesTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInApplicationInstanceTypecomplexType)

### <a name="service-element"></a>Service-Element
Declareert een service die automatisch worden gemaakt wanneer de toepassing wordt gestart. Zie voor meer informatie, [Service-Element](service-fabric-service-model-schema-elements.md#ServiceElementanonymouscomplexTypeComplexTypeDefinedInDefaultServicesTypecomplexType)

### <a name="statefulservice-element"></a>StatefulService-Element
Hiermee definieert u een stateful service. Zie voor meer informatie, [StatefulService-Element](service-fabric-service-model-schema-elements.md#StatefulServiceElementStatefulServiceTypeComplexTypeDefinedInServiceTemplatesTypecomplexTypeDefinedInServiceelement)

### <a name="statelessservice-element"></a>StatelessService-Element
Hiermee definieert u een stateless service. Zie voor meer informatie, [StatelessService-Element](service-fabric-service-model-schema-elements.md#StatelessServiceElementStatelessServiceTypeComplexTypeDefinedInServiceTemplatesTypecomplexTypeDefinedInServiceelement)

### <a name="principals-element"></a>Beveiligings-principals-Element
Beschrijving van de beveiligings-principals (gebruikers, groepen) vereist voor deze toepassing services en veilige resources uit te voeren. Beveiligings-principals zijn waarnaar wordt verwezen in de sectie beleid. Zie voor meer informatie, [Principals-Element](service-fabric-service-model-schema-elements.md#PrincipalsElementSecurityPrincipalsTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInEnvironmentTypecomplexType)

### <a name="groups-element"></a>Groepen-Element
Een set met groepen verklaart als beveiligings-principals, deze kunnen worden verwezen in het beleid. Groepen kunnen nuttig zijn als er meerdere gebruikers voor verschillende toegangspunten toegepast en ze moeten bepaalde algemene rechten die beschikbaar op het groepeerniveau van de zijn hebt. Zie voor meer informatie, [groepen-Element](service-fabric-service-model-schema-elements.md#GroupsElementanonymouscomplexTypeComplexTypeDefinedInSecurityPrincipalsTypecomplexType)

### <a name="group-element"></a>Element Group
Geeft aan een groep als een beveiligings-principal die kan worden verwezen in het beleid. Zie voor meer informatie, [Groepelement](service-fabric-service-model-schema-elements.md#GroupElementanonymouscomplexTypeComplexTypeDefinedInGroupselement)

### <a name="membership-element"></a>Lidmaatschap van Element
 Zie voor meer informatie, [lidmaatschap-Element](service-fabric-service-model-schema-elements.md#MembershipElementanonymouscomplexTypeComplexTypeDefinedInGroupelement)

### <a name="systemgroup-element"></a>SystemGroup-Element
 Zie voor meer informatie, [SystemGroup-Element](service-fabric-service-model-schema-elements.md#SystemGroupElementanonymouscomplexTypeComplexTypeDefinedInMembershipelement)

### <a name="users-element"></a>Gebruikers-Element
Een set gebruikers verklaart als beveiligings-principals, deze kunnen worden verwezen in het beleid. Zie voor meer informatie, [gebruikers Element](service-fabric-service-model-schema-elements.md#UsersElementanonymouscomplexTypeComplexTypeDefinedInSecurityPrincipalsTypecomplexType)

### <a name="user-element"></a>Gebruiker-Element
Geeft aan een gebruiker als beveiligings-principal, die kan worden verwezen in het beleid. Zie voor meer informatie, [User-Element](service-fabric-service-model-schema-elements.md#UserElementanonymouscomplexTypeComplexTypeDefinedInUserselement)

### <a name="memberof-element"></a>MemberOf-Element
Gebruikers kunnen worden toegevoegd aan een bestaande lidmaatschapsgroep, zodat het alle eigenschappen en de beveiligingsinstellingen van die lidmaatschapsgroep overnemen kan. De lidmaatschapsgroep kan worden gebruikt voor het beveiligen van externe bronnen die moeten worden geopend door andere services of dezelfde service (op een andere computer). Zie voor meer informatie, [MemberOf-Element](service-fabric-service-model-schema-elements.md#MemberOfElementanonymouscomplexTypeComplexTypeDefinedInUserelement)

### <a name="systemgroup-element"></a>SystemGroup-Element
De systeemgroep om toe te voegen van de gebruiker.  De groep van het systeem moet worden gedefinieerd in de sectie groepen. Zie voor meer informatie, [SystemGroup-Element](service-fabric-service-model-schema-elements.md#SystemGroupElementanonymouscomplexTypeComplexTypeDefinedInMemberOfelement)

### <a name="group-element"></a>Element Group
De groep toe te voegen van de gebruiker.  De groep moet worden gedefinieerd in de sectie groepen. Zie voor meer informatie, [Groepelement](service-fabric-service-model-schema-elements.md#GroupElementanonymouscomplexTypeComplexTypeDefinedInMemberOfelement)

### <a name="policies-element"></a>Beleid-Element
Beschrijving van het beleid (logboekverzameling, standaard run as, status en beveiligingstoegang) moet worden toegepast op het toepassingsniveau van de. Zie voor meer informatie, [beleid-Element](service-fabric-service-model-schema-elements.md#PoliciesElementApplicationPoliciesTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInEnvironmentTypecomplexType)

### <a name="defaultrunaspolicy-element"></a>DefaultRunAsPolicy-Element
Geef een standaardgebruikersaccount voor alle pakketten van de service-code die een specifieke RunAsPolicy is gedefinieerd in de sectie ServiceManifestImport geen hebben. Zie voor meer informatie, [DefaultRunAsPolicy-Element](service-fabric-service-model-schema-elements.md#DefaultRunAsPolicyElementanonymouscomplexTypeComplexTypeDefinedInApplicationPoliciesTypecomplexType)




## <a name="votingweb-service-manifest-elements"></a>VotingWeb-service-manifest elementen
### <a name="servicemanifest-element"></a>ServiceManifest-Element
Declaratief beschrijving van het servicetype en de versie. Geeft de onafhankelijk kan worden geüpgraded code, configuratie en gegevens-pakketten die samen een servicepakket ter ondersteuning van een of meer servicetypen opstellen. Resources, instellingen voor diagnostische gegevens en servicemetagegevens van de, zoals servicetype, status-eigenschappen en taakverdeling metrische gegevens ook worden opgegeven. Zie voor meer informatie, [ServiceManifest-Element](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType)

### <a name="servicetypes-element"></a>ServiceTypes-Element
Hiermee definieert u welke servicetypen worden ondersteund door een CodePackage in dit manifest. Wanneer een service wordt gestart op basis van een van deze servicetypen, worden alle pakketten gedefinieerd in deze manifest geactiveerd door het uitvoeren van hun toegangspunten. Service-typen zijn gedefinieerd op het niveau van het manifest en niet het niveau van de code-pakket. Zie voor meer informatie, [ServiceTypes-Element](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="statelessservicetype-element"></a>StatelessServiceType-Element
Beschrijft een type stateless service. Zie voor meer informatie, [StatelessServiceType-Element](service-fabric-service-model-schema-elements.md#StatelessServiceTypeElementStatelessServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>CodePackage-Element
Beschrijft een codepakket die ondersteuning biedt voor een gedefinieerde servicetype. Wanneer een service wordt gestart op basis van een van deze servicetypen, worden alle pakketten gedefinieerd in deze manifest geactiveerd door het uitvoeren van hun toegangspunten. De resulterende processen worden verwacht voor het registreren van de typen ondersteunde service tijdens runtime. Wanneer er meerdere pakketten, worden alle geactiveerd wanneer er wordt gezocht naar een van de gedeclareerde servicetypen. Zie voor meer informatie, [CodePackage-Element](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="setupentrypoint-element"></a>SetupEntryPoint-Element
Een bevoegde toegangspunt dat standaard wordt uitgevoerd met dezelfde referenties als Service Fabric (meestal het NETWORKSERVICE-account) voordat u een ander toegangspunt. Het uitvoerbare bestand opgegeven door EntryPoint is meestal de ServiceHost langlopende. De aanwezigheid van een toegangspunt voor de afzonderlijke instellingen zo voorkomt u dat de ServiceHost met hoge bevoegdheden voor langere tijd worden uitgevoerd. Zie voor meer informatie, [SetupEntryPoint-Element](service-fabric-service-model-schema-elements.md#SetupEntryPointElementanonymouscomplexTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="exehost-element"></a>ExeHost-Element
 Zie voor meer informatie, [ExeHost-Element](service-fabric-service-model-schema-elements.md#ExeHostElementExeHostEntryPointTypeComplexTypeDefinedInSetupEntryPointelement)

### <a name="program-element"></a>Programma-Element
De naam van uitvoerbaar bestand.  Bijvoorbeeld, "MySetup.bat" of 'MyServiceHost.exe'. Zie voor meer informatie, [programma-Element](service-fabric-service-model-schema-elements.md#ProgramElementxs:stringComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="arguments-element"></a>Argumenten-Element
 Zie voor meer informatie, [argumenten-Element](service-fabric-service-model-schema-elements.md#ArgumentsElementxs:stringComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="workingfolder-element"></a>WorkingFolder-Element
De werkmap voor het proces in het codepakket op het clusterknooppunt waarop de toepassing wordt geïmplementeerd. U kunt drie waarden opgeven: werk (de standaardinstelling), CodePackage of codebasis. CodeBase geeft aan dat de werkmap is ingesteld op de map waarin het exe-bestand is gedefinieerd in het codepakket. CodePackage Hiermee stelt u de werkmap naar de hoofdmap van het codepakket, ongeacht waar het exe-bestand is gedefinieerd in de pakketmap code worden. Werk Hiermee stelt u de werkmap in een unieke map gemaakt op het knooppunt.  Deze map is hetzelfde voor exemplaar van de gehele toepassing. De werkmap van alle processen in de toepassing is standaard ingesteld op de map van het werk. Dit is waar de gegevens op de processen kunnen schrijven. Schrijven van gegevens in het codepakket of codebasis wordt niet aanbevolen omdat deze mappen kunnen worden gedeeld tussen verschillende toepassingsinstanties en mogelijk verwijderd. Zie voor meer informatie, [WorkingFolder-Element](service-fabric-service-model-schema-elements.md#WorkingFolderElementanonymouscomplexTypeComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="consoleredirection-element"></a>ConsoleRedirection-Element
Waarschuwing! Console rediriction gebruiken in een productietoepassing, alleen gebruiken voor lokale ontwikkeling en foutopsporing niet. Leidt de console-uitvoer van het opstartscript naar een uitvoerbestand in de map met de naam 'log' op het clusterknooppunt waarop de toepassing wordt geïmplementeerd en uitgevoerd. Zie voor meer informatie, [ConsoleRedirection-Element](service-fabric-service-model-schema-elements.md#ConsoleRedirectionElementanonymouscomplexTypeComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="entrypoint-element"></a>EntryPoint-Element
Het uitvoerbare bestand opgegeven door EntryPoint is meestal de ServiceHost langlopende. De aanwezigheid van een toegangspunt voor de afzonderlijke instellingen zo voorkomt u dat de ServiceHost met hoge bevoegdheden voor langere tijd worden uitgevoerd. Het uitvoerbare bestand opgegeven door EntryPoint wordt uitgevoerd nadat de SetupEntryPoint is afgesloten. De resulterende proces wordt bewaakt en opnieuw opgestart (begin opnieuw met SetupEntryPoint) als dit ooit wordt beëindigd of vastloopt. Zie voor meer informatie, [EntryPoint-Element](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="exehost-element"></a>ExeHost-Element
 Zie voor meer informatie, [ExeHost-Element](service-fabric-service-model-schema-elements.md#ExeHostElementanonymouscomplexTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType)

### <a name="configpackage-element"></a>ConfigPackage-Element
Declareert een map met de naam van het kenmerk naam onder PackageRoot die een bestand Settings.xml bevat. Dit bestand bevat secties van de gebruiker gedefinieerde, sleutel / waarde-paar instellingen die het proces tijdens de uitvoering lezen kan. Tijdens een upgrade als alleen de ConfigPackage-versie is gewijzigd, klikt u vervolgens het proces dat wordt uitgevoerd is niet opnieuw opgestart. In plaats daarvan een retouraanroep hoogte gebracht van het proces dat configuratie-instellingen hebt gewijzigd, zodat ze kunnen dynamisch worden geladen. Zie voor meer informatie, [ConfigPackage-Element](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="resources-element"></a>Bronnen-Element
Beschrijving van de resources die worden gebruikt door deze service, dat kan worden verklaard zonder gecompileerde code te wijzigen en gewijzigd wanneer de service is geïmplementeerd. Toegang tot deze bronnen wordt geregeld via de secties beveiligings-Principals en het beleid van het toepassingsmanifest. Zie voor meer informatie, [Resources-Element](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="endpoints-element"></a>Eindpunten-Element
Hiermee definieert u eindpunten voor de service. Zie voor meer informatie, [eindpunten-Element](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Eindpuntelement
Het eindpunt, gedefinieerd in het servicemanifest, om op te heffen. Zie voor meer informatie, [Endpoint-Element](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)



## <a name="votingdata-service-manifest-elements"></a>VotingData service manifest-elementen
### <a name="servicemanifest-element"></a>ServiceManifest-Element
Declaratief beschrijving van het servicetype en de versie. Geeft de onafhankelijk kan worden geüpgraded code, configuratie en gegevens-pakketten die samen een servicepakket ter ondersteuning van een of meer servicetypen opstellen. Resources, instellingen voor diagnostische gegevens en servicemetagegevens van de, zoals servicetype, status-eigenschappen en taakverdeling metrische gegevens ook worden opgegeven. Zie voor meer informatie, [ServiceManifest-Element](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType)

### <a name="servicetypes-element"></a>ServiceTypes-Element
Hiermee definieert u welke servicetypen worden ondersteund door een CodePackage in dit manifest. Wanneer een service wordt gestart op basis van een van deze servicetypen, worden alle pakketten gedefinieerd in deze manifest geactiveerd door het uitvoeren van hun toegangspunten. Service-typen zijn gedefinieerd op het niveau van het manifest en niet het niveau van de code-pakket. Zie voor meer informatie, [ServiceTypes-Element](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="statefulservicetype-element"></a>StatefulServiceType-Element
Hierin wordt beschreven in een stateful servicetype. Zie voor meer informatie, [StatefulServiceType-Element](service-fabric-service-model-schema-elements.md#StatefulServiceTypeElementStatefulServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>CodePackage-Element
Beschrijft een codepakket die ondersteuning biedt voor een gedefinieerde servicetype. Wanneer een service wordt gestart op basis van een van deze servicetypen, worden alle pakketten gedefinieerd in deze manifest geactiveerd door het uitvoeren van hun toegangspunten. De resulterende processen worden verwacht voor het registreren van de typen ondersteunde service tijdens runtime. Wanneer er meerdere pakketten, worden alle geactiveerd wanneer er wordt gezocht naar een van de gedeclareerde servicetypen. Zie voor meer informatie, [CodePackage-Element](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="entrypoint-element"></a>EntryPoint-Element
Het uitvoerbare bestand opgegeven door EntryPoint is meestal de ServiceHost langlopende. De aanwezigheid van een toegangspunt voor de afzonderlijke instellingen zo voorkomt u dat de ServiceHost met hoge bevoegdheden voor langere tijd worden uitgevoerd. Het uitvoerbare bestand opgegeven door EntryPoint wordt uitgevoerd nadat de SetupEntryPoint is afgesloten. De resulterende proces wordt bewaakt en opnieuw opgestart (begin opnieuw met SetupEntryPoint) als dit ooit wordt beëindigd of vastloopt. Zie voor meer informatie, [EntryPoint-Element](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="exehost-element"></a>ExeHost-Element
 Zie voor meer informatie, [ExeHost-Element](service-fabric-service-model-schema-elements.md#ExeHostElementanonymouscomplexTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType)

### <a name="program-element"></a>Programma-Element
De naam van uitvoerbaar bestand.  Bijvoorbeeld, "MySetup.bat" of 'MyServiceHost.exe'. Zie voor meer informatie, [programma-Element](service-fabric-service-model-schema-elements.md#ProgramElementxs:stringComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="workingfolder-element"></a>WorkingFolder-Element
De werkmap voor het proces in het codepakket op het clusterknooppunt waarop de toepassing wordt geïmplementeerd. U kunt drie waarden opgeven: werk (de standaardinstelling), CodePackage of codebasis. CodeBase geeft aan dat de werkmap is ingesteld op de map waarin het exe-bestand is gedefinieerd in het codepakket. CodePackage Hiermee stelt u de werkmap naar de hoofdmap van het codepakket, ongeacht waar het exe-bestand is gedefinieerd in de pakketmap code worden. Werk Hiermee stelt u de werkmap in een unieke map gemaakt op het knooppunt.  Deze map is hetzelfde voor exemplaar van de gehele toepassing. De werkmap van alle processen in de toepassing is standaard ingesteld op de map van het werk. Dit is waar de gegevens op de processen kunnen schrijven. Schrijven van gegevens in het codepakket of codebasis wordt niet aanbevolen omdat deze mappen kunnen worden gedeeld tussen verschillende toepassingsinstanties en mogelijk verwijderd. Zie voor meer informatie, [WorkingFolder-Element](service-fabric-service-model-schema-elements.md#WorkingFolderElementanonymouscomplexTypeComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="configpackage-element"></a>ConfigPackage-Element
Declareert een map met de naam van het kenmerk naam onder PackageRoot die een bestand Settings.xml bevat. Dit bestand bevat secties van de gebruiker gedefinieerde, sleutel / waarde-paar instellingen die het proces tijdens de uitvoering lezen kan. Tijdens een upgrade als alleen de ConfigPackage-versie is gewijzigd, klikt u vervolgens het proces dat wordt uitgevoerd is niet opnieuw opgestart. In plaats daarvan een retouraanroep hoogte gebracht van het proces dat configuratie-instellingen hebt gewijzigd, zodat ze kunnen dynamisch worden geladen. Zie voor meer informatie, [ConfigPackage-Element](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="datapackage-element"></a>Gegevenspakket-Element
Declareert een map met de naam van het kenmerk naam onder PackageRoot die bestanden om te worden verbruikt door het proces tijdens runtime statische gegevens bevat. Service Fabric wordt gerecycled alle exe- en DLLHOSTs opgegeven in de pakketten host en ondersteuning wanneer een van de gegevenspakketten die worden vermeld in het servicemanifest worden bijgewerkt. Zie voor meer informatie, [gegevenspakket-Element](service-fabric-service-model-schema-elements.md#DataPackageElementDataPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedDataPackageelement)

### <a name="resources-element"></a>Bronnen-Element
Beschrijving van de resources die worden gebruikt door deze service, dat kan worden verklaard zonder gecompileerde code te wijzigen en gewijzigd wanneer de service is geïmplementeerd. Toegang tot deze bronnen wordt geregeld via de secties beveiligings-Principals en het beleid van het toepassingsmanifest. Zie voor meer informatie, [Resources-Element](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="endpoints-element"></a>Eindpunten-Element
Hiermee definieert u eindpunten voor de service. Zie voor meer informatie, [eindpunten-Element](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Eindpuntelement
Het eindpunt, gedefinieerd in het servicemanifest, om op te heffen. Zie voor meer informatie, [Endpoint-Element](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)

