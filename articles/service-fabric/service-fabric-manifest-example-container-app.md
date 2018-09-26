---
title: Azure Service Fabric container application manifest voorbeelden | Microsoft Docs
description: Meer informatie over het configureren van de toepassing en service-manifest instellingen voor een Service Fabric-toepassing met meerdere containers.
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
ms.date: 06/08/2018
ms.author: ryanwi
ms.openlocfilehash: 6f538fa821e546d12c5a2bdb9585cc85871241fa
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47094149"
---
# <a name="multi-container-application-and-service-manifest-examples"></a>Voorbeelden van toepassings- en servicemanifesten voor toepassingen met meerdere containers
Hier volgen enkele voorbeelden van de toepassing en service manifesten voor een Service Fabric-toepassing met meerdere containers. Het doel van deze voorbeelden is om weer te geven welke instellingen zijn beschikbaar en het gebruik ervan. Deze toepassing en service manifesten zijn gebaseerd op de [voorbeeld van Windows Server 2016 Container](https://github.com/Azure-Samples/service-fabric-containers/tree/master/Windows) manifesten.

De volgende functies worden weergegeven:
|Manifest|Functies|
|---|---|
|[Toepassingsmanifest](#application-manifest)| [omgevingsvariabelen overschrijven](service-fabric-get-started-containers.md#configure-and-set-environment-variables), [toewijzing van de poort-naar-host configureren](service-fabric-get-started-containers.md#configure-container-port-to-host-port-mapping-and-container-to-container-discovery), [verificatie containerregister configureren](service-fabric-get-started-containers.md#configure-container-registry-authentication), [resourcebeheer](service-fabric-resource-governance.md), [set-isolatiemodus](service-fabric-get-started-containers.md#configure-isolation-mode), [OS build-specifieke containerinstallatiekopieën opgeven](service-fabric-get-started-containers.md#specify-os-build-specific-container-images)| 
|[FrontEndService servicemanifest](#frontendservice-service-manifest)| [Omgevingsvariabelen instellen](service-fabric-get-started-containers.md#configure-and-set-environment-variables), [een eindpunt configureert](service-fabric-get-started-containers.md#configure-communication), opdrachten doorgeven aan de container, [een certificaat importeren in een container](service-fabric-securing-containers.md)| 
|[BackEndService servicemanifest](#backendservice-service-manifest)|[Omgevingsvariabelen instellen](service-fabric-get-started-containers.md#configure-and-set-environment-variables), [een eindpunt configureert](service-fabric-get-started-containers.md#configure-communication), [volume stuurprogramma configureren](service-fabric-containers-volume-logging-drivers.md)| 

Zie [manifest elementen van de toepassing](#application-manifest-elements), [FrontEndService service manifest elementen](#frontendservice-service-manifest-elements), en [BackEndService service manifest elementen](#backendservice-service-manifest-elements) voor meer informatie over specifieke XML-elementen.

## <a name="application-manifest"></a>Manifest van de toepassing

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Container.ApplicationType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="BackEndService_InstanceCount" DefaultValue="-1" />
    <Parameter Name="FrontEndService_InstanceCount" DefaultValue="-1" />
    <Parameter Name="CpuCores" DefaultValue="2" />
    <Parameter Name="BlockIOWeight" DefaultValue="200" />
    <Parameter Name="MaximumIOBandwidth" DefaultValue="1024" />
    <Parameter Name="MemoryReservationInMB" DefaultValue="1024" />
    <Parameter Name="MemorySwapInMB" DefaultValue="4084"/>
    <Parameter Name="MaximumIOps" DefaultValue="20"/>
    <Parameter Name="MemoryFront" DefaultValue="4084" />
    <Parameter Name="MemoryBack" DefaultValue="2048" />
    <Parameter Name="CertThumbprint" DefaultValue=""/>
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="BackEndServicePkg" ServiceManifestVersion="1.0.0" />    
    
    <!-- Policies to be applied to the imported service manifest. -->
    <Policies>
      <!-- Set resource governance at the service package level. -->
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]" MemoryInMB="[MemoryFront]"/>

      <!-- Set resource governance at the code package level. -->
      <ResourceGovernancePolicy CodePackageRef="Code" CpuPercent="10" MemoryInMB="[MemoryFront]" BlockIOWeight="[BlockIOWeight]" MaximumIOBandwidth="[MaximumIOBandwidth]" MaximumIOps="[MaximumIOps]" MemoryReservationInMB="[MemoryReservationInMB]" MemorySwapInMB="[MemorySwapInMB]"/>
      
      <!-- Policies for activating container hosts. -->
      <ContainerHostPolicies CodePackageRef="Code" Isolation="process">
        
        <!-- Credentials for the repository hosting the container image.-->
        <RepositoryCredentials AccountName="sfsamples" Password="ENCRYPTED-PASSWORD" PasswordEncrypted="true"/>
        
        <!-- This binds the port the container is listening on (8905 in this sample) to an endpoint resource named "BackEndServiceTypeEndpoint", which is defined in the service manifest.  -->
        <PortBinding ContainerPort="8905" EndpointRef="BackEndServiceTypeEndpoint"/>
        
        <!-- Configure the Azure Files volume plugin.  Bind the source folder on the host VM or a remote share to the destination folder within the running container. -->
        <Volume Source="azfiles" Destination="c:\VolumeTest\Data" Driver="sfazurefile">
          <!-- Driver options to be passed to driver. The Azure Files volume plugin supports the following driver options:
            shareName (the Azure Files file share that provides the volume for the container), storageAccountName (the Azure storage account
            that contains the Azure Files file share), storageAccountKey (Access key for the Azure storage account that contains the Azure Files file share).
            These three driver options are required. -->
          <DriverOption Name="shareName" Value="" />
          <DriverOption Name="storageAccountName" Value="MY-STORAGE-ACCOUNT-NAME" />
          <DriverOption Name="storageAccountKey" Value="MY-STORAGE-ACCOUNT-KEY" />
        </Volume>
        
        <!-- Windows Server containers may not be compatible across different versions of the OS.  You can specify multiple OS images per container and tag 
        them with the build versions of the OS. Get the build version of the OS by running "winver" at a Windows command prompt. -->
        <ImageOverrides>
          <!-- If the underlying OS is build version 16299 (Windows Server version 1709), Service Fabric picks the container image tagged with Os="16299". -->
          <Image Name="sfsamples.azurecr.io/sfsamples/servicefabricbackendservice_1709" Os="16299" />
          
          <!-- An untagged container image is assumed to work across all versions of the OS and overrides the image specified in the service manifest. -->
          <Image Name="sfsamples.azurecr.io/sfsamples/servicefabricbackendservice_default" />          
        </ImageOverrides>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>

  <!-- Policies to be applied to the imported service manifest. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    
    <!-- This enables you to provide different values for environment variables when creating a FrontEndService
         Theses environment variables are declared in the FrontEndServiceType service manifest-->
    <EnvironmentOverrides CodePackageRef="Code">
      <EnvironmentVariable Name="BackendServiceName" Value="Container.Application/BackEndService"/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
      <EnvironmentVariable Name="IsContainer" Value="true"/>
    </EnvironmentOverrides>
    
    <!-- This policy maps the  port of the container (80) to the endpoint declared in the service, 
         FrontEndServiceTypeEndpoint which is exposed as port 80 on the host-->    
    <Policies>

      <!-- Set resource governance at the service package level. -->
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]" MemoryInMB="[MemoryBack]"/>

      <!-- Policies for activating container hosts. -->
      <ContainerHostPolicies CodePackageRef="Code" Isolation="process">

        <!-- Credentials for the repository hosting the container image.-->
        <RepositoryCredentials AccountName="sfsamples" Password="ENCRYPTED-PASSWORD" PasswordEncrypted="true"/>

        <!-- Binds an endpoint resource (declared in the service manifest) to the exposed container port. -->
        <PortBinding ContainerPort="80" EndpointRef="FrontEndServiceTypeEndpoint"/>

        <!-- Import a certificate into the container.  The certificate must be installed in the LocalMachine store of all the cluster nodes.
          When the application starts, the runtime reads the certificate and generates a PFX file and password (on Windows) or a PEM file (on Linux).
          The PFX file and password are accessible in the container using the Certificates_ServicePackageName_CodePackageName_CertName_PFX and 
          Certificates_ServicePackageName_CodePackageName_CertName_Password environment variables. The PEM file is accessible in the container using the 
          Certificates_ServicePackageName_CodePackageName_CertName_PEM and Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey environment variables.-->
        <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[CertThumbprint]" />

        <!-- If the certificate is already in PFX or PEM form, you can create a data package inside your application and reference that certificate here. -->
        <CertificateRef Name="MyCert2" DataPackageRef="Data" DataPackageVersion="1.0.0" RelativePath="MyCert2.PFX" Password="ENCRYPTED-PASSWORD" IsPasswordEncrypted="true"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
        
    <Service Name="FrontEndService" >
      <StatelessService ServiceTypeName="FrontEndServiceType" InstanceCount="[FrontEndService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
        <Service Name="BackEndService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="BackEndServiceType" InstanceCount="[BackEndService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="frontendservice-service-manifest"></a>FrontEndService servicemanifest

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="FrontEndServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="FrontEndServiceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ContainerHost>
        <!--The repo and image on https://hub.docker.com or Azure Container Registry. -->
        <ImageName>sfsamples.azurecr.io/sfsamples/servicefabricfrontendservice:v1</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container or exe.  These variables are overridden in the application manifest. -->
    <EnvironmentVariables>
      <EnvironmentVariable Name="BackendServiceName" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
      <EnvironmentVariable Name="IsContainer" Value=""/>
    </EnvironmentVariables>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />
  
  <!-- Data package is the contents of the Data directory under PackageRoot that contains an 
       independently-updateable and versioned static data that's consumed by the process at runtime. -->
  <DataPackage Name="Data" Version="1.0.0"/>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. For a guest executable is used to register with the NamingService at its REST endpoint
           with http scheme and port 80 -->
      <Endpoint Name="FrontEndServiceTypeEndpoint" UriScheme="http" Port="80"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="backendservice-service-manifest"></a>BackEndService servicemanifest

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="BackEndServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="BackEndServiceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ContainerHost>
        <!--The repo and image on https://hub.docker.com or Azure Container Registry. -->
        <ImageName>sfsamples.azurecr.io/sfsamples/servicefabricbackendservice:v1</ImageName>
        
        <!-- Pass comma delimited commands to your container. -->
        <Commands> dotnet, myproc.dll, 5 </Commands>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container. These variables are overridden in the application manifest. -->
    <EnvironmentVariables>
      <EnvironmentVariable Name="IsContainer" Value="true"/>
    </EnvironmentVariables>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the host port on which to 
           listen. For a guest executable is used to register with the NamingService at its REST endpoint
           with http scheme. In this case since no port is specified, one is created and assigned dynamically
           to the service. This dynamically assigned host port is mapped to the container port (8905 in this sample),
            which was specified in the application manifest.-->
      <Endpoint Name="BackEndServiceTypeEndpoint" UriScheme="http" />
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

### <a name="policies-element"></a>Beleid-Element
Beschrijving van het beleid (eindpunt binding, delen, run as-pakket en beveiliging toegang) moet worden toegepast op het geïmporteerde servicemanifest. Zie voor meer informatie, [beleid-Element](service-fabric-service-model-schema-elements.md#PoliciesElementServiceManifestImportPoliciesTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="servicepackageresourcegovernancepolicy-element"></a>ServicePackageResourceGovernancePolicy-Element
Hiermee definieert u de resource governance-beleid dat wordt toegepast op het niveau van de hele service-pakket. Zie voor meer informatie, [ServicePackageResourceGovernancePolicy-Element](service-fabric-service-model-schema-elements.md#ServicePackageResourceGovernancePolicyElementServicePackageResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInServicePackageTypecomplexType)

### <a name="resourcegovernancepolicy-element"></a>ResourceGovernancePolicy-Element
Hiermee geeft u voor een codepakket. Zie voor meer informatie, [ResourceGovernancePolicy-Element](service-fabric-service-model-schema-elements.md#ResourceGovernancePolicyElementResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelementDefinedInDigestedEndpointelement)

### <a name="containerhostpolicies-element"></a>ContainerHostPolicies-Element
Hiermee geeft u een beleid voor het activeren van de hosts van de container. Zie voor meer informatie, [ContainerHostPolicies-Element](service-fabric-service-model-schema-elements.md#ContainerHostPoliciesElementContainerHostPoliciesTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="repositorycredentials-element"></a>RepositoryCredentials-Element
Referenties voor de container-installatiekopieopslagplaats naar pull installatiekopieën uit. Zie voor meer informatie, [RepositoryCredentials-Element](service-fabric-service-model-schema-elements.md#RepositoryCredentialsElementRepositoryCredentialsTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="portbinding-element"></a>PortBinding-Element
Hiermee geeft u op welke eindpuntresource verbinding maken met de beschikbaar gemaakte container-poort. Zie voor meer informatie, [PortBinding-Element](service-fabric-service-model-schema-elements.md#PortBindingElementPortBindingTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="volume-element"></a>Volume-Element
Hiermee geeft u het volume worden gebonden aan de container. Zie voor meer informatie, [Volume-Element](service-fabric-service-model-schema-elements.md#VolumeElementContainerVolumeTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="driveroption-element"></a>DriverOption-Element
Stuurprogrammaopties moeten worden doorgegeven aan het stuurprogramma. Zie voor meer informatie, [DriverOption-Element](service-fabric-service-model-schema-elements.md#DriverOptionElementDriverOptionTypeComplexTypeDefinedInContainerLoggingDriverTypecomplexTypeDefinedInContainerVolumeTypecomplexType)

### <a name="imageoverrides-element"></a>ImageOverrides-Element
Windows Server-containers zijn mogelijk niet in verschillende versies van het besturingssysteem compatibel is.  U kunt opgeven van meerdere besturingssysteemkopieën per container en ze te labelen met de build-versie van het besturingssysteem. De build-versie van het besturingssysteem ophalen door het uitvoeren van 'winver' achter de opdrachtprompt van Windows. Als het onderliggende besturingssysteem build 16299 (Windows Server versie 1709) versie is, kiest Service Fabric de containerinstallatiekopie getagd met Os = '16299'. Een containerinstallatiekopie wordt aangenomen dat het werk in alle versies van het besturingssysteem en de installatiekopie die wordt opgegeven in het servicemanifest onderdrukkingen. Zie voor meer informatie, [ImageOverrides-Element](service-fabric-service-model-schema-elements.md#ImageOverridesElementImageOverridesTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="image-element"></a>Image-Element
De containerinstallatiekopie die overeenkomt met de OS-buildversienummer moet worden gestart. Als het kenmerk besturingssysteem niet is opgegeven, wordt de containerinstallatiekopie wordt aangenomen dat het werk in alle versies van het besturingssysteem en vervangt de afbeelding die is opgegeven in het servicemanifest. Zie voor meer informatie, [Image-Element](service-fabric-service-model-schema-elements.md#ImageElementImageTypeComplexTypeDefinedInImageOverridesTypecomplexType)

### <a name="environmentoverrides-element"></a>EnvironmentOverrides-Element
 Zie voor meer informatie, [EnvironmentOverrides-Element](service-fabric-service-model-schema-elements.md#EnvironmentOverridesElementEnvironmentOverridesTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="environmentvariable-element"></a>EnvironmentVariable-Element
Omgevingsvariabele. Zie voor meer informatie, [EnvironmentVariable-Element](service-fabric-service-model-schema-elements.md#EnvironmentVariableElementEnvironmentVariableTypeComplexTypeDefinedInEnvironmentOverridesTypecomplexTypeDefinedInEnvironmentVariablesTypecomplexType)

### <a name="certificateref-element"></a>CertificateRef-Element
Hiermee geeft u informatie over een X509 certificaat dat moet worden blootgesteld aan de containeromgeving. Het certificaat moet worden geïnstalleerd in het LocalMachine-archief van alle clusterknooppunten.
Wanneer de toepassing wordt gestart, wordt de runtime leest het certificaat en genereert een PFX-bestand en het wachtwoord (op Windows) of een PEM-bestand (op Linux).
De PFX-bestand en het wachtwoord zijn toegankelijk in de container met behulp van de omgevingsvariabelen Certificates_ServicePackageName_CodePackageName_CertName_PFX en Certificates_ServicePackageName_CodePackageName_CertName_Password. Het PEM-bestand is beschikbaar in de container met behulp van de omgevingsvariabelen Certificates_ServicePackageName_CodePackageName_CertName_PEM en Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey. Zie voor meer informatie, [CertificateRef-Element](service-fabric-service-model-schema-elements.md#CertificateRefElementContainerCertificateTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="defaultservices-element"></a>DefaultServices-Element
Verklaart de exemplaren van de service die automatisch worden gemaakt wanneer een toepassing wordt geïnstantieerd op basis van dit toepassingstype. Zie voor meer informatie, [DefaultServices-Element](service-fabric-service-model-schema-elements.md#DefaultServicesElementDefaultServicesTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInApplicationInstanceTypecomplexType)

### <a name="service-element"></a>Service-Element
Declareert een service die automatisch worden gemaakt wanneer de toepassing wordt gestart. Zie voor meer informatie, [Service-Element](service-fabric-service-model-schema-elements.md#ServiceElementanonymouscomplexTypeComplexTypeDefinedInDefaultServicesTypecomplexType)

### <a name="statelessservice-element"></a>StatelessService-Element
Hiermee definieert u een stateless service. Zie voor meer informatie, [StatelessService-Element](service-fabric-service-model-schema-elements.md#StatelessServiceElementStatelessServiceTypeComplexTypeDefinedInServiceTemplatesTypecomplexTypeDefinedInServiceelement)


## <a name="frontendservice-service-manifest-elements"></a>FrontEndService service manifest-elementen
### <a name="servicemanifest-element"></a>ServiceManifest-Element
Declaratief beschrijving van het servicetype en de versie. Geeft de onafhankelijk kan worden geüpgraded code, configuratie en gegevens-pakketten die samen een servicepakket ter ondersteuning van een of meer servicetypen opstellen. Resources, instellingen voor diagnostische gegevens en servicemetagegevens van de, zoals servicetype, status-eigenschappen en taakverdeling metrische gegevens ook worden opgegeven. Zie voor meer informatie, [ServiceManifest-Element](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType)

### <a name="servicetypes-element"></a>ServiceTypes-Element
Hiermee definieert u welke servicetypen worden ondersteund door een CodePackage in dit manifest. Wanneer een service wordt gestart op basis van een van deze servicetypen, worden alle pakketten gedefinieerd in deze manifest geactiveerd door het uitvoeren van hun toegangspunten. Service-typen zijn gedefinieerd op het niveau van het manifest en niet het niveau van de code-pakket. Zie voor meer informatie, [ServiceTypes-Element](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="statelessservicetype-element"></a>StatelessServiceType-Element
Beschrijft een type stateless service. Zie voor meer informatie, [StatelessServiceType-Element](service-fabric-service-model-schema-elements.md#StatelessServiceTypeElementStatelessServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>CodePackage-Element
Beschrijft een codepakket die ondersteuning biedt voor een gedefinieerde servicetype. Wanneer een service wordt gestart op basis van een van deze servicetypen, worden alle pakketten gedefinieerd in deze manifest geactiveerd door het uitvoeren van hun toegangspunten. De resulterende processen worden verwacht voor het registreren van de typen ondersteunde service tijdens runtime. Wanneer er meerdere pakketten, worden alle geactiveerd wanneer er wordt gezocht naar een van de gedeclareerde servicetypen. Zie voor meer informatie, [CodePackage-Element](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="entrypoint-element"></a>EntryPoint-Element
Het uitvoerbare bestand opgegeven door EntryPoint is meestal de ServiceHost langlopende. De aanwezigheid van een toegangspunt voor de afzonderlijke instellingen zo voorkomt u dat de ServiceHost met hoge bevoegdheden voor langere tijd worden uitgevoerd. Het uitvoerbare bestand opgegeven door EntryPoint wordt uitgevoerd nadat de SetupEntryPoint is afgesloten. De resulterende proces wordt bewaakt en opnieuw opgestart (begin opnieuw met SetupEntryPoint) als dit ooit wordt beëindigd of vastloopt. Zie voor meer informatie, [EntryPoint-Element](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="containerhost-element"></a>ContainerHost-Element
 Zie voor meer informatie, [ContainerHost-Element](service-fabric-service-model-schema-elements.md#ContainerHostElementContainerHostEntryPointTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType)

### <a name="imagename-element"></a>ImageName-Element
De opslagplaats en de installatiekopie op https://hub.docker.com of Azure Container Registry. Zie voor meer informatie, [ImageName-Element](service-fabric-service-model-schema-elements.md#ImageNameElementxs:stringComplexTypeDefinedInContainerHostEntryPointTypecomplexType)

### <a name="environmentvariables-element"></a>Omgevingsvariabelen Element
Omgevingsvariabelen doorgeven aan de container of exe-bestand.  Zie voor meer informatie, [omgevingsvariabelen Element](service-fabric-service-model-schema-elements.md#EnvironmentVariablesElementEnvironmentVariablesTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="environmentvariable-element"></a>EnvironmentVariable-Element
Omgevingsvariabele. Zie voor meer informatie, [EnvironmentVariable-Element](service-fabric-service-model-schema-elements.md#EnvironmentVariableElementEnvironmentVariableTypeComplexTypeDefinedInEnvironmentOverridesTypecomplexTypeDefinedInEnvironmentVariablesTypecomplexType)

### <a name="configpackage-element"></a>ConfigPackage-Element
Declareert u een map met de naam van het kenmerk Name, die een Settings.xml bestand bevat. Dit bestand bevat secties van de gebruiker gedefinieerde, sleutel / waarde-paar instellingen die het proces tijdens de uitvoering lezen kan. Tijdens een upgrade als alleen de ConfigPackage-versie is gewijzigd, klikt u vervolgens het proces dat wordt uitgevoerd is niet opnieuw opgestart. In plaats daarvan een retouraanroep hoogte gebracht van het proces dat configuratie-instellingen hebt gewijzigd, zodat ze kunnen dynamisch worden geladen. Zie voor meer informatie, [ConfigPackage-Element](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="datapackage-element"></a>Gegevenspakket-Element
Declareert een map met de naam van het kenmerk Name, die bestanden statische gegevens bevat. Service Fabric wordt gerecycled alle exe- en DLLHOSTs opgegeven in de pakketten host en ondersteuning wanneer een van de gegevenspakketten die worden vermeld in het servicemanifest worden bijgewerkt. Zie voor meer informatie, [gegevenspakket-Element](service-fabric-service-model-schema-elements.md#DataPackageElementDataPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedDataPackageelement)

### <a name="resources-element"></a>Bronnen-Element
Beschrijving van de resources die worden gebruikt door deze service, dat kan worden verklaard zonder gecompileerde code te wijzigen en gewijzigd wanneer de service is geïmplementeerd. Toegang tot deze bronnen wordt geregeld via de secties beveiligings-Principals en het beleid van het toepassingsmanifest. Zie voor meer informatie, [Resources-Element](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="endpoints-element"></a>Eindpunten-Element
Hiermee definieert u eindpunten voor de service. Zie voor meer informatie, [eindpunten-Element](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Eindpuntelement
Zie voor meer informatie, [Endpoint-Element](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)


## <a name="backendservice-service-manifest-elements"></a>BackEndService service manifest-elementen
### <a name="servicemanifest-element"></a>ServiceManifest-Element
Declaratief beschrijving van het servicetype en de versie. Geeft de onafhankelijk kan worden geüpgraded code, configuratie en gegevens-pakketten die samen een servicepakket ter ondersteuning van een of meer servicetypen opstellen. Resources, instellingen voor diagnostische gegevens en servicemetagegevens van de, zoals servicetype, status-eigenschappen en taakverdeling metrische gegevens ook worden opgegeven. Zie voor meer informatie, [ServiceManifest-Element](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType)

### <a name="servicetypes-element"></a>ServiceTypes-Element
Hiermee definieert u welke servicetypen worden ondersteund door een CodePackage in dit manifest. Wanneer een service wordt gestart op basis van een van deze servicetypen, worden alle pakketten gedefinieerd in deze manifest geactiveerd door het uitvoeren van hun toegangspunten. Service-typen zijn gedefinieerd op het niveau van het manifest en niet het niveau van de code-pakket. Zie voor meer informatie, [ServiceTypes-Element](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="statelessservicetype-element"></a>StatelessServiceType-Element
Beschrijft een type stateless service. Zie voor meer informatie, [StatelessServiceType-Element](service-fabric-service-model-schema-elements.md#StatelessServiceTypeElementStatelessServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>CodePackage-Element
Beschrijft een codepakket die ondersteuning biedt voor een gedefinieerde servicetype. Wanneer een service wordt gestart op basis van een van deze servicetypen, worden alle pakketten gedefinieerd in deze manifest geactiveerd door het uitvoeren van hun toegangspunten. De resulterende processen worden verwacht voor het registreren van de typen ondersteunde service tijdens runtime. Wanneer er meerdere pakketten, worden alle geactiveerd wanneer er wordt gezocht naar een van de gedeclareerde servicetypen. Zie voor meer informatie, [CodePackage-Element](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="entrypoint-element"></a>EntryPoint-Element
Het uitvoerbare bestand opgegeven door EntryPoint is meestal de ServiceHost langlopende. De aanwezigheid van een toegangspunt voor de afzonderlijke instellingen zo voorkomt u dat de ServiceHost met hoge bevoegdheden voor langere tijd worden uitgevoerd. Het uitvoerbare bestand opgegeven door EntryPoint wordt uitgevoerd nadat de SetupEntryPoint is afgesloten. De resulterende proces wordt bewaakt en opnieuw opgestart (begin opnieuw met SetupEntryPoint) als dit ooit wordt beëindigd of vastloopt. Zie voor meer informatie, [EntryPoint-Element](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="containerhost-element"></a>ContainerHost-Element
Zie voor meer informatie, [ContainerHost-Element](service-fabric-service-model-schema-elements.md#ContainerHostElementContainerHostEntryPointTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType)

### <a name="imagename-element"></a>ImageName-Element
De opslagplaats en de installatiekopie op https://hub.docker.com of Azure Container Registry. Zie voor meer informatie, [ImageName-Element](service-fabric-service-model-schema-elements.md#ImageNameElementxs:stringComplexTypeDefinedInContainerHostEntryPointTypecomplexType)

### <a name="commands-element"></a>Opdrachten-Element
Een door komma's gescheiden lijst met opdrachten doorgeven aan de container. Zie voor meer informatie, [opdrachten-Element](service-fabric-service-model-schema-elements.md#CommandsElementxs:stringComplexTypeDefinedInContainerHostEntryPointTypecomplexType)

### <a name="environmentvariables-element"></a>Omgevingsvariabelen Element
Omgevingsvariabelen doorgeven aan de container of exe-bestand.  Zie voor meer informatie, [omgevingsvariabelen Element](service-fabric-service-model-schema-elements.md#EnvironmentVariablesElementEnvironmentVariablesTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="environmentvariable-element"></a>EnvironmentVariable-Element
Omgevingsvariabele. Zie voor meer informatie, [EnvironmentVariable-Element](service-fabric-service-model-schema-elements.md#EnvironmentVariableElementEnvironmentVariableTypeComplexTypeDefinedInEnvironmentOverridesTypecomplexTypeDefinedInEnvironmentVariablesTypecomplexType)

### <a name="configpackage-element"></a>ConfigPackage-Element
Declareert u een map met de naam van het kenmerk Name, die een Settings.xml bestand bevat. Dit bestand bevat secties van de gebruiker gedefinieerde, sleutel / waarde-paar instellingen die het proces tijdens de uitvoering lezen kan. Tijdens een upgrade als alleen de ConfigPackage-versie is gewijzigd, klikt u vervolgens het proces dat wordt uitgevoerd is niet opnieuw opgestart. In plaats daarvan een retouraanroep hoogte gebracht van het proces dat configuratie-instellingen hebt gewijzigd, zodat ze kunnen dynamisch worden geladen. Zie voor meer informatie, [ConfigPackage-Element](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="resources-element"></a>Bronnen-Element
Beschrijving van de resources die worden gebruikt door deze service, dat kan worden verklaard zonder gecompileerde code te wijzigen en gewijzigd wanneer de service is geïmplementeerd. Toegang tot deze bronnen wordt geregeld via de secties beveiligings-Principals en het beleid van het toepassingsmanifest. Zie voor meer informatie, [Resources-Element](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="endpoints-element"></a>Eindpunten-Element
Hiermee definieert u eindpunten voor de service. Zie voor meer informatie, [eindpunten-Element](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Eindpuntelement
 Zie voor meer informatie, [Endpoint-Element](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)

