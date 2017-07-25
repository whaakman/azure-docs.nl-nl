---
title: Een Azure Service Fabric-containertoepassing maken | Microsoft Docs
description: Maak uw eerste Windows-containertoepassing in Azure Service Fabric.  Bouw een Docker-installatiekopie met een Python-toepassing, push de installatiekopie naar een containerregister, en bouw en implementeer een Service Fabric-containertoepassing.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: 8c9d6c65666b5ffedf058e0a83d4fc41fff80235
ms.contentlocale: nl-nl
ms.lasthandoff: 07/15/2017

---

# <a name="create-your-first-service-fabric-container-application-on-windows"></a>Uw eerste Service Fabric-containertoepassing maken in Windows
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Er zijn geen wijzigingen in uw toepassing vereist om een bestaande toepassing in een Windows-container uit te voeren in een Service Fabric-cluster. Dit artikel helpt u bij het maken van een Docker-installatiekopie met een Python [Flask](http://flask.pocoo.org/)-webtoepassing en het implementeren ervan in een Service Fabric-cluster.  U gaat uw containertoepassing ook delen via [Azure Container Registry](/azure/container-registry/).  In dit artikel wordt ervan uitgegaan dat u de basisbeginselen kent van Docker. Meer informatie over Docker kunt u lezen in het [Docker-overzicht](https://docs.docker.com/engine/understanding-docker/).

## <a name="prerequisites"></a>Vereisten
Een ontwikkelcomputer waarop wordt uitgevoerd:
* Visual Studio 2015 of Visual Studio 2017.
* [Service Fabric SDK en hulpprogramma's](service-fabric-get-started.md).
*  Docker voor Windows.  [Download Docker CE voor Windows (stabiel)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Nadat u Docker hebt geïnstalleerd en gestart, klikt u met de rechtermuisknop op het systeemvakpictogram en selecteert u **Overschakelen naar Windows-containers**. Dit is vereist voor het uitvoeren van Docker-installatiekopieën op basis van Windows.

Een Windows-cluster met drie of meer knooppunten die worden uitgevoerd op Windows Server 2016 met containers - [Een cluster maken](service-fabric-cluster-creation-via-portal.md) of [Service Fabric gratis uitproberen](https://aka.ms/tryservicefabric). 

Een register in Azure Container Registry - [Een containerregister maken](../container-registry/container-registry-get-started-portal.md) in uw Azure-abonnement. 

## <a name="define-the-docker-container"></a>De Docker-container definiëren
Bouw een installatiekopie op basis van de [Python-installatiekopie](https://hub.docker.com/_/python/) die zich in de Docker-hub bevindt. 

Definieer uw Docker-container in een Dockerfile. Het bestand Dockerfile bevat instructies voor het instellen van de omgeving in de container, het laden van de toepassing die u wilt uitvoeren en het toewijzen van poorten. Het Dockerfile is de invoer van de opdracht `docker build` waarmee de installatiekopie wordt gemaakt. 

Maak een lege map en maak het bestand *Dockerfile* (zonder extensie). Voeg het volgende toe aan het bestand *Dockerfile* en sla de wijzigingen op:

```
# Use an official Python runtime as a base image
FROM python:2.7-windowsservercore

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

Lees het [Dockerfile-referentiemateriaal](https://docs.docker.com/engine/reference/builder/) voor meer informatie.

## <a name="create-a-simple-web-application"></a>Een eenvoudige webtoepassing maken
Maak een Flask-toepassing die luistert op poort 80 en 'Hallo Wereld!' retourneert.  Maak in dezelfde map het bestand *requirements.txt*.  Voeg het volgende toe en sla de wijzigingen op:
```
Flask
```

Maak ook het bestand *app.py* en voeg het volgende toe:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    
    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

## <a name="build-the-image"></a>De installatiekopie bouwen
Voer de opdracht `docker build` uit om de installatiekopie te maken waarmee de webtoepassing wordt uitgevoerd. Open een PowerShell-venster en navigeer naar de map met het bestand Dockerfile. Voer de volgende opdracht uit:

```
docker build -t helloworldapp .
```

Met deze opdracht wordt de nieuwe installatiekopie gebouwd met behulp van de instructies in het Dockerfile, en krijgt de installatiekopie de naam (-t tagging) `hallowereldapp´. Bij het bouwen van een installatiekopie wordt de basisinstallatiekopie uit de Docker-hub gehaald en wordt er een nieuwe installatiekopie gemaakt waarmee de toepassing wordt toegevoegd boven op de basisinstallatiekopie.  

Nadat de buildopdracht is voltooid, voert u de opdracht `docker images` uit om de gegevens van de nieuwe installatiekopie te bekijken:

```
$ docker images
    
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              8ce25f5d6a79        2 minutes ago       10.4 GB
```

## <a name="run-the-application-locally"></a>De toepassing lokaal uitvoeren
Controleer de installatiekopie eerst lokaal voordat u deze naar het containerregister pusht.  

Voer de toepassing uit:

```
docker run -d --name my-web-site helloworldapp
```

Bij *naam* kunt de actieve container een naam geven (in plaats van de container-id).

Zodra de container is gestart, zoekt u het bijbehorende IP-adres zodat u vanuit een browser verbinding kunt maken met de actieve container:
```
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
```

Maak verbinding met de actieve container.  Open een webbrowser en verwijs naar het IP-adres dat is geretourneerd, bijvoorbeeld http://172.31.194.61. Als het goed is, ziet u de koptekst Hallo wereld! weergegeven in de browser.

Als u de container wilt stoppen, voert u dit uit:

```
docker stop my-web-site
```

De container verwijderen van de ontwikkelcomputer:

```
docker rm my-web-site
```

## <a name="push-the-image-to-the-container-registry"></a>De installatiekopie naar het containerregister pushen
Nadat u hebt gecontroleerd of de container actief is op de ontwikkelcomputer, pusht u de installatiekopie naar het register in Azure Container Registry.

Voer ``docker login`` uit om u bij uw containerregister aan te melden met uw [registerreferenties](../container-registry/container-registry-authentication.md).

In het volgende voorbeeld worden de id en het wachtwoord van een [service-principal](../active-directory/active-directory-application-objects.md) van Azure Active Directory doorgegeven. U hebt bijvoorbeeld een service-principal aan uw register toegewezen voor een automatiseringsscenario. Of u kunt zich aanmelden met uw gebruikersnaam en wachtwoord van het register.

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Met de volgende opdracht maakt u een tag (of alias) van de installatiekopie met een volledig gekwalificeerd pad naar uw register. In dit voorbeeld wordt de installatiekopie in de naamruimte ```samples``` geplaatst om overbodige items in de hoofdmap van het register te voorkomen.

```
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

De installatiekopie naar het containerregister pushen:

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
6. Als de beperkte service een eindpunt voor communicatie nodig heeft, kunt u nu het protocol, de poort en het type toevoegen aan een ```Endpoint``` in het bestand ServiceManifest.xml. Voor deze snelstartgids luistert de containerservice naar poort 80: 

    ```xml
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
    ```
    Door ```UriScheme``` op te geven wordt het eindpunt van de container automatisch geregistreerd bij de Service Fabric Naming-service voor meer zichtbaarheid. Een volledig voorbeeld van een ServiceManifest.xml-bestand vindt u aan het einde van dit artikel. 
7. Configureer de poorttoewijzing poort-naar-host voor de container door een ```PortBinding```-beleid op te geven in ```ContainerHostPolicies``` van het bestand ApplicationManifest.xml.  Voor dit artikel geldt: ```ContainerPort``` is 8081 (de container gebruikt poort 80, zoals opgegeven in het bestand Dockerfile) en ```EndpointRef``` is Guest1TypeEndpoint (het eindpunt dat is gedefinieerd in het servicemanifest).  Binnenkomende aanvragen naar de service op poort 8081 worden toegewezen aan poort 80 in de container.  Als de container moet verifiëren bij een persoonlijke privéopslagplaats, voegt u ```RepositoryCredentials``` toe.  Voeg voor dit artikel de accountnaam en het wachtwoord in voor het containerregister myregistry.azurecr.io. 

    ```xml
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
            <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
            <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ```

    Een volledig voorbeeld van een ApplicationManifest.xml-bestand vindt u aan het einde van dit artikel.
8. Configureer het eindpunt voor de clusterverbinding, zodat u de toepassing in het cluster kunt publiceren.  U vindt het eindpunt voor de clientverbinding op de blade Overzicht voor het cluster in [Azure Portal](https://portal.azure.com). Open in Solution Explorer *Cloud.xml* onder **MyFirstContainer**->**PublishProfiles**.  Voeg de clusternaam en verbindingspoort toe aan **ClusterConnectionParameters**.  Bijvoorbeeld:
    ```xml
    <ClusterConnectionParameters ConnectionEndpoint="containercluster.westus2.cloudapp.azure.com:19000" />
    ```
    
9. Sla alle bestanden op en bouw uw project.  

10. Klik in Solution Explorer met de rechtermuisknop op **MyFirstContainer** en selecteer **Package** om uw toepassing in te pakken. 

## <a name="deploy-the-container-application"></a>De containertoepassing implementeren
Klik in Solution Explorer met de rechtermuisknop op **MyFirstContainer** en selecteer **Publish** om uw toepassing te publiceren.

[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is een webhulpprogramma voor het inspecteren en beheren van toepassingen en knooppunten in een Service Fabric-cluster. Open een browser, ga naar http://containercluster.westus2.cloudapp.azure.com:19080/Explorer/ en volg de implementatie van de toepassing.  De toepassing wordt geïmplementeerd, maar heeft een foutstatus totdat de installatiekopie is gedownload op de clusterknooppunten (wat enige tijd kan duren, afhankelijk van de grootte van de installatiekopie): ![Fout][1]

De toepassing is gereed bij een ```Ready```-status: ![Gereed][2]

Open een browser en ga naar http://containercluster.westus2.cloudapp.azure.com:8081. Als het goed is, ziet u de koptekst Hallo wereld! weergegeven in de browser.

## <a name="clean-up"></a>Opruimen
Zolang het cluster actief is, worden er kosten in rekening gebracht. Overweeg daarom [het cluster te verwijderen](service-fabric-get-started-azure-cluster.md#remove-the-cluster).  [Party-clusters](http://tryazureservicefabric.westus.cloudapp.azure.com/) worden na een paar uur automatisch verwijderd.

Nadat u de installatiekopie naar het containerregister hebt gepusht, kunt u de lokale installatiekopie op de ontwikkelcomputer verwijderen:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Volledig voorbeeld van de manifesten voor de Fabric Service-toepassing en -service
Dit zijn de volledige manifesten voor de service en toepassing die in dit artikel worden gebruikt.

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
      <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
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
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
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
* Lees de zelfstudie [Een .NET-toepassing implementeren in een container](service-fabric-host-app-in-a-container.md).
* Meer informatie over de [levenscyclus](service-fabric-application-lifecycle.md) van de Service Fabric-toepassing.
* Bekijk [voorbeelden van Service Fabric-containercode op GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers).

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png

