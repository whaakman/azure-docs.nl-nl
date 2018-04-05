---
title: Een Azure Service Fabric Windows-containertoepassing maken | Microsoft Docs
description: In deze snelstart maakt uw eerste Windows-containertoepassing in Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/18
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: eb4de9d7781ae355e42a6fec9f7732ad67228e70
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="quickstart-deploy-a-service-fabric-windows-container-application-on-azure"></a>Snelstart: een Service Fabric Windows-containertoepassing implementeren in Azure
Azure Service Fabric is een platform voor gedistribueerde systemen waarmee u schaalbare en betrouwbare microservices en containers implementeert en beheert. 

Er zijn geen wijzigingen in uw toepassing vereist om een bestaande toepassing in een Windows-container uit te voeren in een Service Fabric-cluster. In deze snelstartgids ziet u hoe u een vooraf gebouwde Docker-containerinstallatiekopie in een Service Fabric-toepassing implementeert. Wanneer u klaar bent, hebt u een actieve Windows Server 2016 Nano Server en IIS-container. In deze snelstartgids wordt beschreven hoe u een Windows-container implementeert. Lees [deze snelstartgids](service-fabric-quickstart-containers-linux.md) als u een Linux-container wilt implementeren.

![IIS-standaardwebpagina][iis-default]

In deze snelstartgids leert u de volgende zaken:
> [!div class="checklist"]
> * Een Docker-containerinstallatiekopie verpakken
> * Communicatie configureren
> * De Service Fabric-toepassing bouwen en inpakken
> * De containertoepassing implementeren naar Azure

## <a name="prerequisites"></a>Vereisten
* Een Azure-abonnement (u kunt een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) maken).
* Een ontwikkelcomputer waarop wordt uitgevoerd:
  * Visual Studio 2015 of Visual Studio 2017.
  * [Service Fabric SDK en hulpprogramma's](service-fabric-get-started.md).

## <a name="package-a-docker-image-container-with-visual-studio"></a>Een Docker-containerinstallatiekopie verpakken met Visual Studio
De Service Fabric SDK en hulpprogramma's bieden een servicesjabloon waarmee u een container kunt implementeren in een Service Fabric-cluster.

Start Visual Studio als 'Beheerder'.  Selecteer **Bestand** > **Nieuw** > **Project**.

Selecteer **Service Fabric-toepassing**, geef deze de naam MyFirstContainer en klik op **OK**.

Selecteer **Container** in de lijst met sjablonen voor **gehoste containers en toepassingen**.

In **Naam van installatiekopie** voert u 'microsoft/iis:nanoserver' in, de [Windows Server Nano Server en IIS-basisinstallatiekopie](https://hub.docker.com/r/microsoft/iis/). 

Geef uw service de naam 'MyContainerService' en klik op **OK**.

## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>Communicatie en poort-naar-host poorttoewijzing van de container configureren
De service heeft een eindpunt voor communicatie nodig.  Voor deze Quick Start luistert de containerservice naar poort 80.  Open *MyFirstContainer/ApplicationPackageRoot/MyContainerServicePkg/ServiceManifest.xml* in Solution Explorer.  Werk de bestaande `Endpoint` in het bestand ServiceManifest.xml bij en voeg het protocol, de poort en het URI-schema toe: 

```xml
<Resources>
    <Endpoints>
        <Endpoint Name="MyContainerServiceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
   </Endpoints>
</Resources>
```
Door `UriScheme` op te geven wordt het eindpunt van de container automatisch geregistreerd bij de Service Fabric Naming-service voor meer zichtbaarheid. Een volledig voorbeeld van een ServiceManifest.xml-bestand vindt u aan het einde van dit artikel. 

Configureer de toewijzing van de containerpoort naar de hostpoort zó, dat binnenkomende aanvragen voor de service op poort 80 worden toegewezen aan poort 80 van de container.  Open *MyFirstContainer/ApplicationPackageRoot/ApplicationManifest.xml* in Solution Explorer en geef een `PortBinding`-beleid op in `ContainerHostPolicies`.  Voor deze snelstartgids is `ContainerPort` 80 en `EndpointRef` 'MyContainerServiceTypeEndpoint' (het eindpunt dat is gedefinieerd in het servicemanifest).    

```xml
<ServiceManifestImport>
...
  <ConfigOverrides />
  <Policies>
    <ContainerHostPolicies CodePackageRef="Code">
      <PortBinding ContainerPort="80" EndpointRef="MyContainerServiceTypeEndpoint"/>
    </ContainerHostPolicies>
  </Policies>
</ServiceManifestImport>
```

Een volledig voorbeeld van een ApplicationManifest.xml-bestand vindt u aan het einde van dit artikel.

## <a name="create-a-cluster"></a>Een cluster maken
U kunt voor het implementeren van de toepassing naar een cluster in Azure een cluster van derden gebruiken. Clusters van derden zijn gratis, tijdelijke Service Fabric-clusters die worden gehost op Azure en uitgevoerd door het Service Fabric-team. Iedereen kan hier toepassingen implementeren en meer te weten komen over het platform.  Het cluster gebruikt één zelfondertekend certificaat voor beveiliging van knooppunt-naar-knooppunt en client-naar-knooppunt. Party-clusters ondersteunen containers. Als u besluit uw eigen cluster in te stellen en te gebruiken, moet het cluster worden uitgevoerd op een SKU die ondersteuning biedt voor containers (zoals Windows Server 2016 Datacenter met Containers).

Meld u aan en [neem deel aan een Windows-cluster](http://aka.ms/tryservicefabric). Download het PFX-certificaat naar uw computer door op de koppeling **PFX** te klikken. Klik op de koppeling **Hoe kan ik verbinding maken met een beveiligd Party-cluster?** en kopieer het certificaatwachtwoord. Het certificaat, het certificaatwachtwoord en de waarde van het **verbindingseindpunt** worden in volgende stappen gebruikt.

![PFX en verbindingseindpunt](./media/service-fabric-quickstart-containers/party-cluster-cert.png)

> [!Note]
> Er zijn per uur een beperkt aantal Party-clusters beschikbaar. Als er een fout optreedt wanneer u zich probeert aan te melden voor een Party-cluster, kunt u een bepaalde tijd wachten en het opnieuw proberen, of kunt u deze stappen in de zelfstudie [Een .NET-app implementeren](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-app-to-party-cluster#deploy-the-sample-application) volgen om een Service Fabric-cluster in uw Azure-abonnement te maken en daarin de toepassing te implementeren. Het cluster dat is gemaakt met behulp van Visual Studio ondersteunt containers. Nadat u de toepassing in uw cluster hebt geïmplementeerd en geverifieerd, kunt u verdergaan naar [Volledig voorbeeld van de manifesten voor de Service Fabric-toepassing en -service](#complete-example-service-fabric-application-and-service-manifests) in deze quickstart. 
>

Op een Windows-computer installeert u het PFX-bestand in het certificaatarchief *CurrentUser\My*.

```powershell
PS C:\mycertificates> Import-PfxCertificate -FilePath .\party-cluster-873689604-client-cert.pfx -CertStoreLocation Cert:\CurrentUser\My -Password (ConvertTo-SecureString 873689604 -AsPlainText -Force)


  PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject
----------                                -------
3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.westus.cloudapp.azure.com
```

Onthoud de vingerafdruk voor de volgende stap.  

## <a name="deploy-the-application-to-azure-using-visual-studio"></a>De toepassing publiceren in Azure met Visual Studio
Nu de toepassing klaar is, kunt u deze rechtstreeks vanuit Visual Studio implementeren naar een cluster.

Klik met de rechtermuisknop op **MyFirstContainer** in Solution Explorer en kies **Publiceren**. Het dialoogvenster Publiceren wordt weergegeven.

Kopieer het **verbindingseindpunt** van het cluster van derden naar het veld **Verbindingseindpunt**. Bijvoorbeeld `zwin7fh14scd.westus.cloudapp.azure.com:19000`. Klik op **Geavanceerde verbindingsparameters** en controleer de informatie van de verbindingsparameters.  De waarden *FindValue* en *ServerCertThumbprint* moeten overeenkomen met de vingerafdruk van het certificaat dat in de vorige stap is geïnstalleerd. 

![Het dialoogvenster Publiceren](./media/service-fabric-quickstart-containers/publish-app.png)

Klik op **Publish**.

Elke toepassing in het cluster moet een unieke naam hebben.  Clusters van derden vormen echter een openbare, gedeelde omgeving en er kan een conflict met een bestaande toepassing optreden.  Als er een naamconflict is, wijzigt u de naam van het Visual Studio-project en voert u de implementatie opnieuw uit.

Open een browser en ga naar het **verbindingseindpunt** dat is opgegeven op de pagina met het cluster van een derde. U kunt de URL eventueel vooraf laten gaan door de schema-id, `http://`, en de poort, `:80`, aan het einde toevoegen. Bijvoorbeeld http://zwin7fh14scd.westus.cloudapp.azure.com:80. U ziet de IIS-standaardwebpagina: ![IIS-standaardwebpagina][iis-default]

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Volledig voorbeeld van de manifesten voor de Fabric Service-toepassing en -service
Dit zijn de volledige manifesten voor de service en toepassing die worden gebruikt in deze snelstartgids.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyContainerServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="MyContainerServiceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>microsoft/iis:nanoserver</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->
    <!--
    <EnvironmentVariables>
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
    </EnvironmentVariables>
    -->
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyContainerServiceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="MyFirstContainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="MyContainerService_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyContainerServicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <PortBinding ContainerPort="80" EndpointRef="MyContainerServiceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="MyContainerService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="MyContainerServiceType" InstanceCount="[MyContainerService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u de volgende zaken geleerd:
> [!div class="checklist"]
> * Een Docker-containerinstallatiekopie verpakken
> * Communicatie configureren
> * De Service Fabric-toepassing bouwen en inpakken
> * De containertoepassing implementeren naar Azure

* Meer informatie over het uitvoeren van [containers in Service Fabric](service-fabric-containers-overview.md).
* Lees de zelfstudie [Een .NET-toepassing implementeren in een container](service-fabric-host-app-in-a-container.md).
* Meer informatie over de [levenscyclus](service-fabric-application-lifecycle.md) van de Service Fabric-toepassing.
* Bekijk [voorbeelden van Service Fabric-containercode op GitHub](https://github.com/Azure-Samples/service-fabric-containers).

[iis-default]: ./media/service-fabric-quickstart-containers/iis-default.png
[publish-dialog]: ./media/service-fabric-quickstart-containers/publish-dialog.png
