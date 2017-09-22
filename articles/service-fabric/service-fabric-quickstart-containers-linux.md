---
title: Een Azure Service Fabric-containertoepassing maken in Linux | Microsoft Docs
description: Maak uw eerste Linux-containertoepassing in Azure Service Fabric.  Bouw een Docker-installatiekopie met uw toepassing, push de installatiekopie naar een containerregister, en bouw en implementeer een Service Fabric-containertoepassing.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/05/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: 78306672e812745fd1902ae264c2adea196ab721
ms.contentlocale: nl-nl
ms.lasthandoff: 09/07/2017

---

# <a name="deploy-a-service-fabric-linux-container-application-on-azure"></a>Een Service Fabric Linux-containertoepassing implementeren in Azure
Azure Service Fabric is een platform voor gedistribueerde systemen waarmee u schaalbare en betrouwbare microservices en containers implementeert en beheert. 

Er zijn geen wijzigingen in uw toepassing vereist om een bestaande toepassing in een Linux-container uit te voeren in een Service Fabric-cluster. In deze snelstartgids ziet u hoe u een vooraf gedefinieerde Docker-containerinstallatiekopie in een Service Fabric-toepassing implementeert. Wanneer u klaar bent, hebt u een actieve nginx-container gemaakt.  In deze snelstartgids wordt beschreven hoe u een Linux-container implementeert. Lees [deze snelstartgids](service-fabric-quickstart-containers.md) als u een Windows-container wilt implementeren.

![Nginx][nginx]

In deze snelstartgids leert u de volgende zaken:
> [!div class="checklist"]
> * Een Docker-containerinstallatiekopie verpakken
> * Communicatie configureren
> * De Service Fabric-toepassing bouwen en inpakken
> * De containertoepassing implementeren naar Azure

## <a name="prerequisites"></a>Vereisten
Installeer de [Service Fabric SDK, Service Fabric CLI en de sjabloongenerators van Service Fabric yeoman](service-fabric-get-started-linux.md).
  
## <a name="package-a-docker-image-container-with-yeoman"></a>Een Docker-containerinstallatiekopie verpakken met Yeoman
De Service Fabric-SDK voor Linux bevat een [Yeoman](http://yeoman.io/)-generator waarmee u gemakkelijk uw eerste toepassing kunt maken en een containerinstallatiekopie kunt toevoegen. 

Om een Service Fabric-containertoepassing te maken, opent u een terminalvenster en voert u `yo azuresfcontainer` uit.  

Geef uw toepassing de naam 'MyFirstContainer' en de toepassingsservice de naam 'MyContainerService'.

Geef de containerinstallatiekopie de naam 'nginx:latest' (de [nginx-containerinstallatiekopie](https://hub.docker.com/r/_/nginx/) op Docker Hub). 

Deze installatiekopie heeft een gedefinieerd invoerpunt voor de werkbelasting, dus u moet expliciet de invoeropdrachten opgeven. 

Geef '1' exemplaar op.

![Service Fabric Yeoman-generator voor containers][sf-yeoman]

## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>Communicatie en poort-naar-host poorttoewijzing van de container configureren
Een HTTP-eindpunt configureren zodat clients met uw service kunnen communiceren.  Open het bestand *./MyFirstContainer/MyContainerServicePkg/ServiceManifest.xmll* en declareer een eindpuntbron in het element **ServiceManifest**.  Voeg het protocol, de poort en de naam toe. In deze snelstartgids gebruikt de service poort 80: 

```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
    <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
  </Endpoints>
</Resources>

```
Door `UriScheme` op te geven wordt het eindpunt van de container automatisch geregistreerd bij de Service Fabric Naming-service voor meer zichtbaarheid. Een volledig voorbeeld van een ServiceManifest.xml-bestand vindt u aan het einde van dit artikel. 

Wijs een containerpoort toe aan een `Endpoint`-service met behulp van een `PortBinding`-beleid in `ContainerHostPolicies` van het bestand ApplicationManifest.xml.  Voor deze snelstartgids geldt: `ContainerPort` is 80 (de container gebruikt poort 80) en `EndpointRef` is 'myserviceTypeEndpoint' (het eindpunt dat is gedefinieerd in het servicemanifest).  Binnenkomende aanvragen naar de service op poort 80 worden toegewezen aan poort 80 in de container.  

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-the-service-fabric-application"></a>De Service Fabric-toepassing bouwen en inpakken
De Service Fabric Yeoman-sjablonen bevatten een bouwscript voor [Gradle](https://gradle.org/), dat u kunt gebruiken om de toepassing via de terminal te maken. Sla al uw wijzigingen op.  Ga als volgt te werk om de toepassing te maken en in te pakken:

```bash
cd MyFirstContainer
gradle
```
## <a name="create-a-cluster"></a>Een cluster maken
U kunt voor het implementeren van de toepassing naar een cluster in Azure uw eigen cluster maken of een cluster van derden gebruiken.

Clusters van derden zijn gratis, tijdelijke Service Fabric-clusters die worden gehost op Azure en uitgevoerd door het Service Fabric-team. Iedereen kan hier toepassingen implementeren en meer te weten komen over het platform. [Volg de instructies](http://aka.ms/tryservicefabric) om toegang te krijgen tot een cluster van derden.  

Zie voor meer informatie over het maken van uw eigen cluster [Uw eerste Service Fabric-cluster maken op Azure](service-fabric-get-started-azure-cluster.md).

Let op het verbindingseindpunt, u gaat dit in de volgende stap gebruiken.

## <a name="deploy-the-application-to-azure"></a>De toepassing implementeren in Azure
Als de toepassing is gemaakt, kunt u deze met behulp van de Service Fabric CLI implementeren in het Azure-cluster.

Maak verbinding met het Service Fabric-cluster in Azure.

```bash
sfctl cluster select --endpoint http://lnxt10vkfz6.westus.cloudapp.azure.com:19080
```

Gebruik het installatiescript dat is opgegeven in de sjabloon om het toepassingspakket te kopiëren naar de installatiekopieopslag van het cluster, het toepassingstype te registreren en een exemplaar van de toepassing te maken.

```bash
./install.sh
```

Open een browser en ga naar Service Fabric Explorer op http://lnxt10vkfz6.westus.cloudapp.azure.com:19080/Explorer. Vouw het knooppunt Toepassingen uit. U ziet dat er nu een vermelding is voor uw toepassingstype en nog een voor het eerste exemplaar van dat type.

![Service Fabric Explorer][sfx]

Maak verbinding met de actieve container.  Open een webbrowser en verwijs naar het IP-adres dat is geretourneerd op poort 80, bijvoorbeeld 'lnxt10vkfz6.westus.cloudapp.azure.com:80'. De welkomstpagina van nginx wordt weergegeven in de browser.

![Nginx][nginx]

## <a name="clean-up"></a>Opruimen
Gebruik het uninstall-script dat is opgegeven in de sjabloon om het toepassingsexemplaar te verwijderen uit het cluster en de registratie van het toepassingstype op te heffen.

```bash
./uninstall.sh
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Volledig voorbeeld van de manifesten voor de Fabric Service-toepassing en -service
Dit zijn de volledige manifesten voor de service en toepassing die worden gebruikt in deze snelstartgids.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyContainerServicePkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="MyContainerServiceType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>
   
   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>nginx:latest</ImageName>
            <Commands></Commands>
         </ContainerHost>
      </EntryPoint>
      <EnvironmentVariables> 
      </EnvironmentVariables> 
   </CodePackage>
<Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>
 </ServiceManifest>

```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest  ApplicationTypeName="MyFirstContainerType" ApplicationTypeVersion="1.0.0"
                      xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
   
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyContainerServicePkg" ServiceManifestVersion="1.0.0" />
   <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
</ServiceManifestImport>
   
   <DefaultServices>
      <Service Name="MyContainerService">
        <!-- On a local development cluster, set InstanceCount to 1.  On a multi-node production 
        cluster, set InstanceCount to -1 for the container service to run on every node in 
        the cluster.
        -->
        <StatelessService ServiceTypeName="MyContainerServiceType" InstanceCount="1">
            <SingletonPartition />
        </StatelessService>
      </Service>
   </DefaultServices>
   
</ApplicationManifest>

```

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids leert u de volgende zaken:
> [!div class="checklist"]
> * Een Docker-containerinstallatiekopie verpakken
> * Communicatie configureren
> * De Service Fabric-toepassing bouwen en inpakken
> * De containertoepassing implementeren naar Azure

* Meer informatie over het uitvoeren van [containers in Service Fabric](service-fabric-containers-overview.md).
* Lees de zelfstudie [Een .NET-toepassing implementeren in een container](service-fabric-host-app-in-a-container.md).
* Meer informatie over de [levenscyclus](service-fabric-application-lifecycle.md) van de Service Fabric-toepassing.
* Bekijk [voorbeelden van Service Fabric-containercode op GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers).

[sfx]: ./media/service-fabric-quickstart-containers-linux/SFX.png
[nginx]: ./media/service-fabric-quickstart-containers-linux/nginx.png
[sf-yeoman]: ./media/service-fabric-quickstart-containers-linux/YoSF.png

