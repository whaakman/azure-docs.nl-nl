---
title: Een Azure Service Fabric-container-app maken | Microsoft Docs
description: Maak uw eerste container-app met Azure Service Fabric.  Bouw een Docker-installatiekopie met uw app, push de installatiekopie naar een containerregister, bouw en implementeer een Service Fabric-container-app.
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
ms.date: 05/08/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: acb68b274228aa647dc7be5d36b2b077bd213c1b
ms.contentlocale: nl-nl
ms.lasthandoff: 05/10/2017


---

# <a name="create-your-first-service-fabric-container-app"></a>Uw eerste Service Fabric-container-app maken
Er zijn geen wijzigingen in uw app vereist om een bestaande toepassing in een Windows-container uit te voeren op een Service Fabric-cluster. In deze Quick Start leert u stapsgewijs hoe u een Docker-installatiekopie maakt die een web-app bevat, de nieuwe installatiekopie naar Azure Container Registry pusht, een Service Fabric-container-app maakt, en deze container-app implementeert op een Service Fabric-cluster.  In dit artikel wordt ervan uitgegaan dat u de basisbeginselen kent van Docker. Meer informatie over Docker kunt u lezen in het [Docker-overzicht](https://docs.docker.com/engine/understanding-docker/).

## <a name="prerequisites"></a>Vereisten
Een ontwikkelcomputer waarop wordt uitgevoerd:
* Visual Studio 2015 of Visual Studio 2017.
* [Service Fabric SDK en hulpprogramma's](service-fabric-get-started.md).
*  Docker voor Windows.  [Download Docker CE voor Windows (stabiel)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Nadat u Docker hebt geïnstalleerd en gestart, klikt u met de rechtermuisknop op het systeemvakpictogram en selecteert u **Overschakelen naar Windows-containers**. Dit is vereist voor het uitvoeren van Docker-installatiekopieën op basis van Windows.

Een Windows-cluster met drie of meer knooppunten die worden uitgevoerd op Windows Server 2016 met containers - [Een cluster maken](service-fabric-get-started-azure-cluster.md) of [Service Fabric gratis uitproberen](http://tryazureservicefabric.westus.cloudapp.azure.com/). 

Een register in Azure Container Registry - [Een containerregister maken](../container-registry/container-registry-get-started-portal.md) in uw Azure-abonnement. 

## <a name="create-a-simple-web-app"></a>Een eenvoudige web-app maken
Verzamel alle assets die u in een Docker-installatiekopie moet laden, op één plek. Voor deze Quick Start maakt u op de ontwikkelcomputer een web-app Hallo wereld.

1. Maak een map, bijvoorbeeld *c:\temp\helloworldapp*.
2. Maak een supmap *c:\temp\helloworldapp\content*.
3. Maak een bestand *index.html* in *c:\temp\helloworldapp\content*.
4. Bewerk *index.html* en voeg de volgende regel toe:
    ```
    <h1>Hello World!</h1>
    ```
5. Sla de wijzigingen in *index.html* op.

## <a name="build-the-docker-image"></a>De Docker-installatiekopie bouwen
Bouw een installatiekopie op basis van de [microsoft/iis-installatiekopie](https://hub.docker.com/r/microsoft/iis/) die zich bevindt op de Docker-hub. De microsoft/iis-installatiekopie is afgeleid van de Windows Server Core OS-basisinstallatiekopie en bevat IIS (Internet Information Services).  Als u deze installatiekopie uitvoert in de container worden IIS automatisch gestart en websites geïnstalleerd.

Definieer de Docker-installatiekopie in een Dockerfile. Het Dockerfile bevat instructies voor het bouwen van de installatiekopie en het laden van de app die u wilt uitvoeren. Het Dockerfile is de invoer van de opdracht ```docker build``` waarmee de installatiekopie wordt gemaakt. 

1. Maak een *Dockerfile*-bestand (zonder bestandsextensie) in *c:\temp\helloworldapp* en voeg het volgende toe:

    ```
    # The `FROM` instruction specifies the base image. You are
    # extending the `microsoft/iis` image.
    FROM microsoft/iis

    # Create a directory to hold the web app in the container.
    RUN mkdir C:\site

    # Create a new IIS site.
    RUN powershell -NoProfile -Command \
        Import-module IISAdministration; \
        New-IISSite -Name "Site" -PhysicalPath C:\site -BindingInformation "*:8000:"

    # Opens port 8000 on the container.
    EXPOSE 8000

    # The final instruction copies the web app you created earlier into the container.
    ADD content/ /site
    ```

    Dit Dockerfile bevat geen opdracht ```ENTRYPOINT```. U hebt geen opdracht nodig. Als Windows Server met IIS wordt uitgevoerd, is het IIS-proces het beginpunt dat is geconfigureerd om te starten in de basisinstallatiekopie.

    Lees het [Dockerfile-referentiemateriaal](https://docs.docker.com/engine/reference/builder/) voor meer informatie.

2. Voer de opdracht ```docker build``` uit om de installatiekopie te maken waarmee de web-app wordt uitgevoerd. Open een PowerShell-venster en ga naar *c:\temp\helloworldapp*. Voer de volgende opdracht uit:

    ```
    docker build -t helloworldapp .
    ```
    Met deze opdracht wordt de nieuwe installatiekopie gebouwd met behulp van de instructies in het Dockerfile, en krijgt de installatiekopie de naam (-t tagging) `hallowereldapp´. Bij het bouwen van een installatiekopie wordt de basisinstallatiekopie uit de Docker-hub gehaald en wordt een nieuwe installatiekopie gemaakt waarmee de app wordt toegevoegd boven op de basisinstallatiekopie.  De [microsoft/iis-installatiekopie](https://hub.docker.com/r/microsoft/iis/) en OS-basisinstallatiekopieën zijn 10,5 GB. Het downloaden en uitpakken ervan op de ontwikkelcomputer kost tijd.  Overweeg om lunchpauze te nemen of een kopje koffie te gaan drinken.  Downloaden kost minder tijd als u de OS-basisinstallatiekopie al eerder naar de ontwikkelcomputer hebt verplaatst.

3. Nadat de buildopdracht is voltooid, voert u de opdracht `docker images` uit om de gegevens van de nieuwe installatiekopie te bekijken:

    ```
    docker images
    
    REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
    helloworldapp              latest              86838648aab6        2 minutes ago       10.1 GB
    ```

## <a name="verify-the-image-runs-locally"></a>Controleer of de installatiekopie lokaal wordt uitgevoerd
Controleer de installatiekopie eerst lokaal voordat u deze naar het containerregister pusht.  

1. Start de container met ```docker run```:

    ```
    docker run -d -p 8000:8000 --name my-web-site helloworldapp
    ```

    Bij *naam* kunt de actieve container een naam geven (in plaats van de container-id).

2. Zodra de container is gestart, zoekt u het bijbehorende IP-adres zodat u vanuit een browser verbinding kunt maken met de actieve container:
    ```
    docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
    ```

3. Maak verbinding met de actieve container.  Open een webbrowser en verwijs naar het IP-adres dat is geretourneerd op poort 8000 (bijvoorbeeld: http://172.31.194.61:8000). Als het goed is, ziet u de koptekst Hallo wereld! weergegeven in de browser.

4. Als u de container wilt stoppen, voert u dit uit:

    ```
    docker stop my-web-site
    ```

5. De container verwijderen van de ontwikkelcomputer:

    ```
    docker rm my-web-site
    ```

## <a name="push-the-image-to-the-container-registry"></a>De installatiekopie naar het containerregister pushen
Nadat u hebt gecontroleerd of de container actief is op de ontwikkelcomputer, pusht u de installatiekopie naar het register in Azure Container Registry.

1. Voer ``docker login`` uit om u bij uw containerregister aan te melden met uw [registerreferenties](../container-registry/container-registry-authentication.md).

    In het volgende voorbeeld worden de id en het wachtwoord van een [service-principal](../active-directory/active-directory-application-objects.md) van Azure Active Directory doorgegeven. U hebt bijvoorbeeld een service-principal aan uw register toegewezen voor een automatiseringsscenario.

    ```
    docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
    ```

2. Met de volgende opdracht maakt u een tag (of alias) van de installatiekopie met een volledig gekwalificeerd pad naar uw register. In dit voorbeeld wordt de installatiekopie in de naamruimte ```samples``` geplaatst om overbodige items in de hoofdmap van het register te voorkomen.

    ```
    docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
    ```

3.  De installatiekopie naar het containerregister pushen:

    ```
    docker push myregistry.azurecr.io/samples/helloworldapp
    ```

## <a name="create-and-package-the-containerized-service-in-visual-studio"></a>De beperkte service maken en verpakken in Visual Studio
De Service Fabric SDK en hulpprogramma's bieden een servicesjabloon waarmee u een container kunt implementeren in een Service Fabric-cluster.

1. Start Visual Studio.  Selecteer **Bestand** > **Nieuw** > **Project**.
2. Selecteer **Service Fabric-toepassing**, geef deze de naam MyFirstContainer en klik op **OK**.
3. Selecteer **Gastcontainer** in de lijst met **servicesjablonen**.
4. Voer bij **Naam van installatiekopie** het volgende in: myregistry.azurecr.io/samples/helloworldapp. Dit is de installatiekopie die u naar uw containeropslagplaats hebt gepusht. 
5. Geef de service een naam en klik op **OK**.
6. Als de beperkte service een eindpunt voor communicatie nodig heeft, kunt u nu het protocol, de poort en het type toevoegen aan een ```Endpoint``` in het bestand ServiceManifest.xml. Voor deze Quick Start luistert de beperkte service naar poort 80: 

    ```xml
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    ```
    Door ```UriScheme``` op te geven wordt het eindpunt van de container automatisch geregistreerd bij de Service Fabric Naming-service voor meer zichtbaarheid. Een volledig voorbeeld van een ServiceManifest.xml-bestand vindt u aan het einde van dit artikel. 
7. Configureer de poorttoewijzing poort-naar-host voor de container door een ```PortBinding```-beleid op te geven in ```ContainerHostPolicies``` van het bestand ApplicationManifest.xml.  Voor deze Quick Start geldt: ```ContainerPort``` is 8000 (de container gebruikt poort 8000, zoals opgegeven in het Dockerfile) en ```EndpointRef``` is Guest1TypeEndpoint (het eindpunt dat is gedefinieerd in het servicemanifest).  Binnenkomende aanvragen naar de service op poort 80 worden toegewezen aan poort 8000 op de container.  Als de container moet verifiëren bij een persoonlijke privéopslagplaats, voegt u ```RepositoryCredentials``` toe.  Voeg voor deze Quick Start de accountnaam en het wachtwoord in voor het containerregister myregistry.azurecr.io. 

    ```xml
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
            <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
            <PortBinding ContainerPort="8000" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ```

    Een volledig voorbeeld van een ApplicationManifest.xml-bestand vindt u aan het einde van dit artikel.
8. Configureer het eindpunt voor de clusterverbinding zodat u de app op het cluster kunt publiceren.  U vindt het eindpunt voor de clientverbinding op de blade Overzicht voor het cluster in [Azure Portal](https://portal.azure.com). Open in Solution Explorer *Cloud.xml* onder **MyFirstContainer**->**PublishProfiles**.  Voeg de clusternaam en verbindingspoort toe aan **ClusterConnectionParameters**.  Bijvoorbeeld:
    ```xml
    <ClusterConnectionParameters ConnectionEndpoint="containercluster.westus2.cloudapp.azure.com:19000" />
    ```
    
9. Sla alle bestanden op en bouw uw project.  

10. Klik met de rechtermuisknop in Solution Explorer op **MyFirstContainer** en selecteer **Package**. 

## <a name="deploy-the-container-app"></a>De container-app implementeren
1. Klik met de rechtermuisknop in Solution Explorer op **MyFirstContainer** en selecteer **Publish**.

2. [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is een webhulpprogramma voor het inspecteren en beheren van toepassingen en knooppunten in een Service Fabric-cluster. Open een browser, ga naar http://containercluster.westus2.cloudapp.azure.com:19080/Explorer/ en volg de app-implementatie.  De app wordt geïmplementeerd, maar heeft een foutstatus totdat de installatiekopie is gedownload op de clusterknooppunten (wat enige tijd kan duren, afhankelijk van de grootte van de installatiekopie):  ![Fout][1]

3. De app is gereed bij een ```Ready```-status:  ![Gereed][2]

4. Open een browser en ga naar http://containercluster.westus2.cloudapp.azure.com. Als het goed is, ziet u de koptekst Hallo wereld! weergegeven in de browser.

## <a name="clean-up"></a>Opruimen
Zolang het cluster actief is, worden er kosten in rekening gebracht. Overweeg daarom [het cluster te verwijderen](service-fabric-get-started-azure-cluster.md#remove-the-cluster).  [Party-clusters](http://tryazureservicefabric.westus.cloudapp.azure.com/) worden na een paar uur automatisch verwijderd.

Nadat u de installatiekopie naar het containerregister hebt gepusht, kunt u de lokale installatiekopie op de ontwikkelcomputer verwijderen:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Volledig voorbeeld van de manifesten voor de Fabric Service-toepassing en -service
Dit zijn de volledige manifesten voor de service en toepassing die worden gebruikt in deze Quick Start.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Guest1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="Guest1Type" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
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
      <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
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
    <Parameter Name="Guest1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="8000" EndpointRef="Guest1TypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Guest1">
      <StatelessService ServiceTypeName="Guest1Type" InstanceCount="[Guest1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het uitvoeren van [containers in Service Fabric](service-fabric-containers-overview.md).
* Meer informatie over de [levenscyclus](service-fabric-application-lifecycle.md) van de Service Fabric-toepassing.
* Bekijk [voorbeelden van Service Fabric-containercode op GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers).

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png

