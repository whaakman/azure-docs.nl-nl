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
ms.date: 10/04/2017
ms.author: ryanwi
ms.openlocfilehash: 3649cc2800e774f8dca1b88a1704744b4663a68d
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="create-your-first-service-fabric-container-application-on-linux"></a>Uw eerste Service Fabric-containertoepassing maken in Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Er zijn geen wijzigingen in uw toepassing vereist om een bestaande toepassing in een Linux-container uit te voeren in een Service Fabric-cluster. Dit artikel helpt u bij het maken van een Docker-installatiekopie met een Python [Flask](http://flask.pocoo.org/)-webtoepassing en het implementeren ervan in een Service Fabric-cluster.  U gaat uw containertoepassing ook delen via [Azure Container Registry](/azure/container-registry/).  In dit artikel wordt ervan uitgegaan dat u de basisbeginselen kent van Docker. Meer informatie over Docker kunt u lezen in het [Docker-overzicht](https://docs.docker.com/engine/understanding-docker/).

## <a name="prerequisites"></a>Vereisten
* Een ontwikkelcomputer waarop wordt uitgevoerd:
  * [Service Fabric SDK en hulpprogramma's](service-fabric-get-started-linux.md).
  * [Docker CE voor Linux](https://docs.docker.com/engine/installation/#prior-releases). 
  * [Service Fabric-CLI](service-fabric-cli.md)

* Een register in Azure Container Registry - [Een containerregister maken](../container-registry/container-registry-get-started-portal.md) in uw Azure-abonnement. 

## <a name="define-the-docker-container"></a>De Docker-container definiëren
Bouw een installatiekopie op basis van de [Python-installatiekopie](https://hub.docker.com/_/python/) die zich in de Docker-hub bevindt. 

Definieer uw Docker-container in een Dockerfile. Het bestand Dockerfile bevat instructies voor het instellen van de omgeving in de container, het laden van de toepassing die u wilt uitvoeren en het toewijzen van poorten. Het Dockerfile is de invoer van de opdracht `docker build` waarmee de installatiekopie wordt gemaakt. 

Maak een lege map en maak het bestand *Dockerfile* (zonder extensie). Voeg het volgende toe aan het bestand *Dockerfile* en sla de wijzigingen op:

```
# Use an official Python runtime as a base image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available outside this container
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
Voer de opdracht `docker build` uit om de installatiekopie te maken waarmee de webtoepassing wordt uitgevoerd. Open een PowerShell-venster en ga naar *c:\temp\helloworldapp*. Voer de volgende opdracht uit:

```bash
docker build -t helloworldapp .
```

Met deze opdracht wordt de nieuwe installatiekopie gebouwd met behulp van de instructies in het Dockerfile, en krijgt de installatiekopie de naam (-t tagging) `hallowereldapp´. Bij het bouwen van een installatiekopie wordt de basisinstallatiekopie uit de Docker-hub gehaald en wordt er een nieuwe installatiekopie gemaakt waarmee de toepassing wordt toegevoegd boven op de basisinstallatiekopie.  

Nadat de buildopdracht is voltooid, voert u de opdracht `docker images` uit om de gegevens van de nieuwe installatiekopie te bekijken:

```bash
$ docker images
    
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              86838648aab6        2 minutes ago       194 MB
```

## <a name="run-the-application-locally"></a>De toepassing lokaal uitvoeren
Controleer of de containertoepassing lokaal wordt uitgevoerd voordat u deze naar het containerregister pusht.  

Voer de toepassing uit en wijs poort 4000 van uw computer toe aan de door de container beschikbaar gestelde poort 80:

```bash
docker run -d -p 4000:80 --name my-web-site helloworldapp
```

Bij *naam* kunt de actieve container een naam geven (in plaats van de container-id).

Maak verbinding met de actieve container.  Open een webbrowser en verwijs naar het IP-adres dat is geretourneerd op poort 4000, bijvoorbeeld http://localhost:4000. Als het goed is, ziet u de koptekst Hallo wereld! weergegeven in de browser.

![Hallo wereld!][hello-world]

Als u de container wilt stoppen, voert u dit uit:

```bash
docker stop my-web-site
```

De container verwijderen van de ontwikkelcomputer:

```bash
docker rm my-web-site
```

## <a name="push-the-image-to-the-container-registry"></a>De installatiekopie naar het containerregister pushen
Nadat u hebt gecontroleerd of de toepassing wordt uitgevoerd in Docker, pusht u de installatiekopie naar het register in Azure Container Registry.

Voer `docker login` uit om u bij uw containerregister aan te melden met uw [registerreferenties](../container-registry/container-registry-authentication.md).

In het volgende voorbeeld worden de id en het wachtwoord van een [service-principal](../active-directory/active-directory-application-objects.md) van Azure Active Directory doorgegeven. U hebt bijvoorbeeld een service-principal aan uw register toegewezen voor een automatiseringsscenario.  Of u kunt zich aanmelden met uw gebruikersnaam en wachtwoord van het register.

```bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Met de volgende opdracht maakt u een tag (of alias) van de installatiekopie met een volledig gekwalificeerd pad naar uw register. In dit voorbeeld wordt de installatiekopie in de naamruimte `samples` geplaatst om overbodige items in de hoofdmap van het register te voorkomen.

```bash
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

De installatiekopie naar het containerregister pushen:

```bash
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="package-the-docker-image-with-yeoman"></a>De Docker-installatiekopie inpakken met Yeoman
De Service Fabric-SDK voor Linux bevat een [Yeoman](http://yeoman.io/)-generator waarmee u gemakkelijk uw eerste toepassing kunt maken en een containerinstallatiekopie kunt toevoegen. We gebruiken Yeoman om een toepassing te maken met een enkele Docker-container genaamd *SimpleContainerApp*.

Om een Service Fabric-containertoepassing te maken, opent u een terminalvenster en voert u `yo azuresfcontainer` uit.  

Geef uw toepassing een naam (bijvoorbeeld 'mycontainer') evenals de toepassingsservice (bijvoorbeeld 'myservice').

Geef voor de naam van de installatiekopie de URL op voor de containerinstallatiekopie in een containerregister (bijvoorbeeld: ´myregistry.azurecr.io/samples/helloworldapp´). 

Omdat voor deze installatiekopie een workloadinvoerpunt is gedefinieerd, hoeft u niet expliciet invoeropdrachten op te geven (opdrachten worden uitgevoerd in de container, zodat de container na het opstarten actief blijft). 

Geef '1' exemplaar op.

![Service Fabric Yeoman-generator voor containers][sf-yeoman]

## <a name="configure-port-mapping-and-container-repository-authentication"></a>Poorttoewijzing en containeropslagplaatsverificatie configureren
Uw containerservice heeft een eindpunt voor communicatie nodig.  Voeg nu het protocol en de poort toe, en typ onder de tag ´Resources´ een `Endpoint` in het bestand ServiceManifest.xml. Voor deze snelstartgids luistert de containerservice naar poort 4000: 

```xml

<Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="myServiceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
    </Endpoints>
  </Resources>
 ```
 
Door `UriScheme` op te geven wordt het eindpunt van de container automatisch geregistreerd bij de Service Fabric Naming-service voor meer zichtbaarheid. Een volledig voorbeeld van een ServiceManifest.xml-bestand vindt u aan het einde van dit artikel. 

Configureer de poorttoewijzing poort-naar-host voor de container door een `PortBinding`-beleid op te geven in `ContainerHostPolicies` van het bestand ApplicationManifest.xml.  Voor dit artikel geldt: `ContainerPort` is 80 (de container gebruikt poort 80, zoals opgegeven in het Dockerfile) en `EndpointRef` is myServiceTypeEndpoint (het eindpunt dat is gedefinieerd in het servicemanifest).  Binnenkomende aanvragen naar de service op poort 4000 worden toegewezen aan poort 80 op de container.  Als de container moet verifiëren bij een persoonlijke privéopslagplaats, voegt u `RepositoryCredentials` toe.  Voeg voor dit artikel de accountnaam en het wachtwoord in voor het containerregister myregistry.azurecr.io. Zorg ervoor dat het beleid wordt toegevoegd onder de tag ´ServiceManifestImport´ dat overeenkomt met het juiste servicepakket.

```xml
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServicePkg" ServiceManifestVersion="1.0.0" />
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myServiceTypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
   </ServiceManifestImport>
``` 

## <a name="build-and-package-the-service-fabric-application"></a>De Service Fabric-toepassing bouwen en inpakken
De Service Fabric Yeoman-sjablonen bevatten een bouwscript voor [Gradle](https://gradle.org/), dat u kunt gebruiken om de toepassing via de terminal te maken. Ga als volgt te werk om de toepassing te maken en in te pakken:

```bash
cd mycontainer
gradle
```

## <a name="deploy-the-application"></a>De toepassing implementeren
Als de toepassing is gemaakt, kunt u deze met behulp van de Service Fabric-CLI implementeren in het lokale cluster.

Maak verbinding met het lokale cluster van Service Fabric.

```bash
sfctl cluster select --endpoint http://localhost:19080
```

Gebruik het installatiescript dat is opgegeven in de sjabloon om het toepassingspakket te kopiëren naar de installatiekopieopslag van het cluster, het toepassingstype te registreren en een exemplaar van de toepassing te maken.

```bash
./install.sh
```

Open een browser en navigeer naar de Service Fabric Explorer op http://localhost:19080/Explorer (vervang localhost door het privé IP-adres van de virtuele machine als u Vagrant in Mac OS X gebruikt). Vouw het knooppunt Toepassingen uit. U ziet dat er nu een vermelding is voor uw toepassingstype en nog een voor het eerste exemplaar van dat type.

Maak verbinding met de actieve container.  Open een webbrowser en verwijs naar het IP-adres dat is geretourneerd op poort 4000, bijvoorbeeld http://localhost:4000. Als het goed is, ziet u de koptekst Hallo wereld! weergegeven in de browser.

![Hallo wereld!][hello-world]

## <a name="clean-up"></a>Opruimen
Gebruik het uninstall-script dat is opgegeven in de sjabloon om het toepassingsexemplaar te verwijderen uit het lokale ontwikkelomgevingscluster en de registratie van het toepassingstype op te heffen.

```bash
./uninstall.sh
```

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
<ServiceManifest Name="myservicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="myserviceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers 
      to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
        <Commands></Commands>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->
    
    <EnvironmentVariables>
      <!--
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
      -->
    </EnvironmentVariables>
    
  </CodePackage>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="myServiceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="mycontainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="myservicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myServiceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="myservice">
      <!-- On a local development cluster, set InstanceCount to 1.  On a multi-node production 
      cluster, set InstanceCount to -1 for the container service to run on every node in 
      the cluster.
      -->
      <StatelessService ServiceTypeName="myserviceType" InstanceCount="1">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```
## <a name="adding-more-services-to-an-existing-application"></a>Meer services toevoegen aan een bestaande toepassing

Voer de volgende stappen uit als u nog een containerservice wilt toevoegen aan een toepassing die al is gemaakt met yeoman:

1. Stel de directory in op de hoofdmap van de bestaande toepassing.  Bijvoorbeeld `cd ~/YeomanSamples/MyApplication` als `MyApplication` de toepassing is die is gemaakt door Yeoman.
2. Voer `yo azuresfcontainer:AddService` uit.

<a id="manually"></a>


## <a name="configure-time-interval-before-container-is-force-terminated"></a>Tijdsinterval configureren voor geforceerd beëindigen van container

U kunt een tijdsinterval configureren, zodat de runtime die tijd wacht voordat de container wordt verwijderd nadat het verwijderen van een service (of het verplaatsen naar een ander knooppunt) is gestart. Als u een tijdsinterval configureert, wordt de `docker stop <time in seconds>` opdracht verzonden naar de container.   Zie [docker stop](https://docs.docker.com/engine/reference/commandline/stop/) voor meer informatie. Het tijdsinterval dat moet worden gewacht, kunt u opgeven in de sectie `Hosting`. Het volgende fragment van een clustermanifest laat zien hoe u het wachtinterval instelt:

```xml
{
        "name": "Hosting",
        "parameters": [
          {
            "ContainerDeactivationTimeout": "10",
          ...
          }
        ]
}
```
Het standaardtijdsinterval is 10 seconden. Aangezien deze configuratie dynamisch is, wordt de time-out bijgewerkt bij een configuratie-upgrade van het cluster. 


## <a name="configure-the-runtime-to-remove-unused-container-images"></a>De runtime configureren voor het verwijderen van ongebruikte containerinstallatiekopieën

U kunt het Service Fabric-cluster configureren voor het verwijderen van ongebruikte containerinstallatiekopieën van het knooppunt. Met deze configuratie kunt u schijfruimte vrijmaken als er te veel containerinstallatiekopieën aanwezig zijn op het knooppunt.  Om deze functie in te schakelen, past u de sectie `Hosting` in het clustermanifest aan zoals wordt weergegeven in het volgende fragment: 


```xml
{
        "name": "Hosting",
        "parameters": [
          {
            "PruneContainerImages": “True”,
            "ContainerImagesToSkip": "microsoft/windowsservercore|microsoft/nanoserver|…",
          ...
          }
        ]
} 
```

De installatiekopieën die niet moeten worden verwijderd, kunt u opgeven met de parameter `ContainerImagesToSkip`. 


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het uitvoeren van [containers in Service Fabric](service-fabric-containers-overview.md).
* Lees de zelfstudie [Een .NET-toepassing implementeren in een container](service-fabric-host-app-in-a-container.md).
* Meer informatie over de [levenscyclus](service-fabric-application-lifecycle.md) van de Service Fabric-toepassing.
* Bekijk [voorbeelden van Service Fabric-containercode op GitHub](https://github.com/Azure-Samples/service-fabric-containers).

[hello-world]: ./media/service-fabric-get-started-containers-linux/HelloWorld.png
[sf-yeoman]: ./media/service-fabric-get-started-containers-linux/YoSF.png
