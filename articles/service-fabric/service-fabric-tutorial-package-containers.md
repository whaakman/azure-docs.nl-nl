---
title: Verpakken en distribueren van een Service Fabric-toepassing voor containers | Microsoft Docs
description: Informatie over het genereren van de definitie van een Azure Service Fabric-toepassing met behulp van Yeoman en de toepassing van het pakket.
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: Docker, Containers, Microservices, Service Fabric, Azure
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: caa7f58860c4540fa6914b1c0f0cfcba437468fa
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="package-and-deploy-containers-as-a-service-fabric-application"></a>Het pakket en containers implementeren als een Service Fabric-toepassing

Deze zelfstudie maakt deel uit twee in een reeks. In deze zelfstudie wordt een sjabloon generator-hulpprogramma (Yeoman) gebruikt voor het genereren van de definitie van een Service Fabric-toepassing. Deze toepassing kan vervolgens worden gebruikt voor het implementeren van containers naar Service Fabric. In deze zelfstudie leert u het volgende: 

> [!div class="checklist"]
> * Yeoman installeren  
> * Maken van een toepassingspakket met Yeoman
> * Instellingen configureren in het toepassingspakket voor gebruik met containers
> * De toepassing bouwen  
> * Implementeren en uitvoeren van de toepassing 
> * Opschonen van de toepassing

## <a name="prerequisites"></a>Vereisten

- Installatiekopieën van de container in Azure Container register gemaakt in gepusht [Part 1](service-fabric-tutorial-create-container-images.md) van deze zelfstudie reeksen worden gebruikt.
- Linux-ontwikkelomgeving is [instellen](service-fabric-tutorial-create-container-images.md).

## <a name="install-yeoman"></a>Yeoman installeren
Service fabric bevat steigers hulpmiddelen voor het maken van toepassingen van definitieve Yeoman met generator van de sjabloon. De volgende stappen om te controleren of u hebt de Yeoman generator van de sjabloon. 

1. Nodejs en NPM installeren op uw computer. Houd er rekening mee dat Mac OS x-gebruikers moeten de package manager Homebrew gebruiken

    ```bash
    sudo apt-get install npm && sudo apt install nodejs-legacy
    ```
2. Installeer Yeoman sjabloon generator op uw computer uit NPM 

    ```bash
    sudo npm install -g yo
    ```
3. De container-generator voor Yeoman voor Service Fabric installeren

    ```bash 
    sudo npm install -g generator-azuresfcontainer
    ```

## <a name="package-a-docker-image-container-with-yeoman"></a>Een Docker-containerinstallatiekopie verpakken met Yeoman

1. Als u wilt maken van een container Service Fabric-toepassing in de container-tutorial-map van de gekloonde opslagplaats, voer de volgende opdracht.

    ```bash
    yo azuresfcontainer
    ```
2. Typ 'TestContainer' als naam voor uw toepassing
3. Typ 'azurevotefront' als naam voor uw toepassingsservice.
4. Geef op het pad van de container-installatiekopie in ACR voor de opslagplaats frontend - bijvoorbeeld '\<acrName >.azurecr.io / azure-stem-voorzijde: v1'. De \<acrName > veld moet gelijk zijn aan de waarde die is gebruikt in de vorige zelfstudie.
5. Druk op Enter om te laten de opdrachten sectie leeg.
6. Geef een aantal exemplaren van 1.

Hieronder vindt u de invoer en uitvoer van het uitvoeren de yo opdracht:

```bash
? Name your application TestContainer
? Name of the application service: azurevotefront
? Input the Image Name: <acrName>.azurecr.io/azure-vote-front:v1
? Commands: 
? Number of instances of guest container application: 1
   create TestContainer/TestContainer/ApplicationManifest.xml
   create TestContainer/TestContainer/azurevotefrontPkg/ServiceManifest.xml
   create TestContainer/TestContainer/azurevotefrontPkg/config/Settings.xml
   create TestContainer/TestContainer/azurevotefrontPkg/code/Dummy.txt
   create TestContainer/install.sh
   create TestContainer/uninstall.sh
```

Als een andere containerservice toevoegen aan een toepassing die al is gemaakt met behulp van Yeoman, moet u de volgende stappen uitvoeren:

1. Eén niveau van de directory te wijzigen de **TestContainer** directory, bijvoorbeeld *. / TestContainer*
2. Voer `yo azuresfcontainer:AddService` uit. 
3. Naam van de service azurevoteback
4. Geef het pad van de container-installatiekopie voor Redis - ' alpine: redis'
5. Druk op Enter om te laten de opdrachten sectie leeg
6. Geef '1' exemplaar op.

De vermeldingen voor het toevoegen van de service die gebruikt worden alle weergegeven:

```bash
? Name of the application service: azurevoteback
? Input the Image Name: alpine:redis
? Commands: 
? Number of instances of guest container application: 1
   create TestContainer/azurevotebackPkg/ServiceManifest.xml
   create TestContainer/azurevotebackPkg/config/Settings.xml
   create TestContainer/azurevotebackPkg/code/Dummy.txt
```

Voor de rest van deze zelfstudie we werken de **TestContainer** directory. Bijvoorbeeld: *./TestContainer/TestContainer*. De inhoud van deze map moet als volgt.
```bash
$ ls
ApplicationManifest.xml azurevotefrontPkg azurevotebackPkg
```

## <a name="configure-the-application-manifest-with-credentials-for-azure-container-registry"></a>Het toepassingsmanifest met referenties voor Azure Container register configureren
Service Fabric voor het ophalen van de installatiekopieën van de container in Azure Container register, moet de referenties in de **ApplicationManifest.xml**. 

Aanmelden bij uw ACR-exemplaar. Gebruik de **az acr aanmelding** opdracht om de bewerking te voltooien. Geef de unieke naam krijgen tot het register van de container wanneer deze is gemaakt.

```bash
az acr login --name <acrName>
```

De opdracht retourneert een **aanmelding geslaagd** bericht eenmaal is voltooid.

Voer vervolgens de volgende opdracht om op te halen van het wachtwoord van het register van de container. Dit wachtwoord wordt gebruikt door de Service Fabric voor verificatie met ACR voor het ophalen van de installatiekopieën van de container.

```bash
az acr credential show -n <acrName> --query passwords[0].value
```

In de **ApplicationManifest.xml**, Voeg het volgende codefragment onder de **ServiceManifestImport** element voor de front-end-service. Voeg uw **acrName** voor de **AccountName** veld en het wachtwoord dat wordt geretourneerd door de vorige opdracht wordt gebruikt voor de **wachtwoord** veld. Een volledige **ApplicationManifest.xml** aan het einde van dit document wordt geleverd. 

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <RepositoryCredentials AccountName="<acrName>" Password="<password>" PasswordEncrypted="false"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>Communicatie en poort-naar-host poorttoewijzing van de container configureren

### <a name="configure-communication-port"></a>Communicatiepoort configureren

Een HTTP-eindpunt configureren zodat clients met uw service kunnen communiceren. Open de *./TestContainer/azurevotefrontPkg/ServiceManifest.xml* bestands- en declareert een endpoint-bron in de **ServiceManifest** element.  Voeg het protocol, de poort en de naam toe. Voor deze zelfstudie wordt de service luistert op poort 80. Het volgende fragment wordt geplaatst in de *ServiceManifest* tag op in de resource.
  
```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
            listen. Please note that if your service is partitioned, this port is shared with 
            replicas of different partitions that are placed in your code. -->
    <Endpoint Name="azurevotefrontTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
  </Endpoints>
</Resources>

```
  
Op dezelfde manier wijzigen Service Manifest voor de back-endservice. Open de *./TestContainer/azurevotebackPkg/ServiceManifest.xml* en declareert een endpoint-bron in de **ServiceManifest** element. Voor deze zelfstudie wordt de standaardwaarde redis van 6379 bijgehouden. Het volgende fragment wordt geplaatst in de *ServiceManifest* tag op in de resource.

```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
            listen. Please note that if your service is partitioned, this port is shared with 
            replicas of different partitions that are placed in your code. -->
    <Endpoint Name="azurevotebackTypeEndpoint" Port="6379" Protocol="tcp"/>
  </Endpoints>
</Resources>
```
Geven de **UriScheme**automatisch wordt geregistreerd, het eindpunt van de container met de naamgeving van Service Fabric-service voor detectie. Een volledige ServiceManifest.xml voorbeeld-bestand voor de back-endservice is verstrekt aan het einde van dit artikel als voorbeeld. 

### <a name="map-container-ports-to-a-service"></a>Container poorten toewijzen aan een service
Als u wilt weergeven van de containers in het cluster, moeten we ook maakt u een poortbinding in de 'ApplicationManifest.xml'. De **PortBinding** beleid verwijst naar de **eindpunten** is gedefinieerd in de **ServiceManifest.xml** bestanden. Binnenkomende aanvragen naar deze eindpunten toegewezen aan de container-poorten die worden geopend en wordt begrensd hier. In de **ApplicationManifest.xml** bestand, voeg de volgende code om poort 80 en 6379 binden aan de eindpunten. Een volledige **ApplicationManifest.xml** is beschikbaar op het einde van dit document. 
  
```xml
<ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="azurevotefrontTypeEndpoint"/>
</ContainerHostPolicies>
```

```xml
<ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="6379" EndpointRef="azurevotebackTypeEndpoint"/>
</ContainerHostPolicies>
```

### <a name="add-a-dns-name-to-the-backend-service"></a>Een DNS-naam toevoegen aan de back-endservice
  
Voor Service Fabric deze DNS-naam toewijzen aan de back-endservice, de naam moet worden opgegeven de **ApplicationManifest.xml**. Voeg de **ServiceDnsName** kenmerk de **Service** element zoals wordt weergegeven: 
  
```xml
<Service Name="azurevoteback" ServiceDnsName="redisbackend.testapp">
  <StatelessService ServiceTypeName="azurevotebackType" InstanceCount="1">
    <SingletonPartition/>
  </StatelessService>
</Service>
```

De frontend-service leest een omgevingsvariabele als u wilt weten van de DNS-naam van het Redis-exemplaar. Deze omgevingsvariabele is al gedefinieerd in de Dockerfile dat is gebruikt voor het genereren van de installatiekopie van het Docker en er is geen actie moet worden ondernomen hier.
  
```Dockerfile
ENV REDIS redisbackend.testapp
```
  
Het volgende codefragment ziet u hoe de front-Python-code de omgevingsvariabele beschreven in de Dockerfile opneemt. Er is geen actie moet worden ondernomen hier. 

```python
# Get DNS Name
redis_server = os.environ['REDIS'] 

# Connect to the Redis store
r = redis.StrictRedis(host=redis_server, port=6379, db=0)
```

Op dit moment in de zelfstudie is de sjabloon voor een pakket met Service-toepassing beschikbaar voor implementatie naar een cluster. In de volgende zelfstudie deze toepassing wordt geïmplementeerd en uitgevoerd in een Service Fabric-cluster.

## <a name="create-a-service-fabric-cluster"></a>Een Service Fabric-cluster maken
Gebruik uw eigen cluster of een cluster van derden om de toepassing te implementeren in een cluster in Azure.

Clusters van derden zijn gratis tijdelijke Service Fabric-clusters die worden gehost in Azure. Deze wordt beheerd door het Service Fabric-team waar iedereen toepassingen implementeren en meer informatie over het platform. [Volg de instructies](http://aka.ms/tryservicefabric) om toegang te krijgen tot een cluster van derden. 

Zie [Een Service Fabric-cluster maken op Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md) voor meer informatie over het maken van uw eigen cluster.

## <a name="build-and-deploy-the-application-to-the-cluster"></a>Bouw en implementeer de toepassing aan het cluster
De Azure-cluster met behulp van de Service Fabric-CLI kunt u de toepassing implementeren. Als Service Fabric CLI op uw computer niet is geïnstalleerd, voert u de instructies [hier](service-fabric-get-started-linux.md#set-up-the-service-fabric-cli) om deze te installeren. 

Maak verbinding met het Service Fabric-cluster in Azure. Het eindpunt van de tijdelijke aanduiding vervangt door uw eigen. Het eindpunt moet een volledige lijkt op de onderstaande URL.

```bash
sfctl cluster select --endpoint <http://lin4hjim3l4.westus.cloudapp.azure.com:19080>
```

Gebruik het script voor installatie is opgegeven in de **TestContainer** directory voor het kopiëren van het toepassingspakket naar het archief van de installatiekopie van het cluster, het toepassingstype registreren en geen exemplaar maken van de toepassing.

```bash
./install.sh
```

Open een browser en Ga naar Service Fabric Explorer op http://lin4hjim3l4.westus.cloudapp.azure.com:19080/Explorer. Vouw het knooppunt toepassingen en houd er rekening mee dat er een vermelding voor het toepassingstype van uw en een andere voor het exemplaar.

![Service Fabric Explorer][sfx]

Open een webbrowser om verbinding met de actieve toepassing, en Ga naar de url van het cluster - bijvoorbeeld http://lin0823ryf2he.cloudapp.azure.com:80. Hier ziet u de toepassing Voting in de web-UI.

![votingapp][votingapp]

## <a name="clean-up"></a>Opruimen
Gebruik het uninstall-script dat is opgegeven in de sjabloon om het toepassingsexemplaar te verwijderen uit het cluster en de registratie van het toepassingstype op te heffen. Met deze opdracht neemt enige tijd opschonen van het exemplaar en de opdracht 'install.sh' onmiddellijk na dit script kan niet worden uitgevoerd. 

```bash
./uninstall.sh
```

## <a name="examples-of-completed-manifests"></a>Voorbeelden van voltooide manifesten

### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ApplicationManifest ApplicationTypeName="TestContainerType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="azurevotefrontPkg" ServiceManifestVersion="1.0.0"/>
    <Policies> 
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myaccountname" Password="<password>" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="azurevotefrontTypeEndpoint"/>
    </ContainerHostPolicies>
        </Policies>
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="azurevotebackPkg" ServiceManifestVersion="1.0.0"/>
      <Policies> 
        <ContainerHostPolicies CodePackageRef="Code">
          <PortBinding ContainerPort="6379" EndpointRef="azurevotebackTypeEndpoint"/>
        </ContainerHostPolicies>
      </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="azurevotefront">
      <StatelessService ServiceTypeName="azurevotefrontType" InstanceCount="1">
        <SingletonPartition/>
      </StatelessService>
    </Service>
    <Service Name="azurevoteback" ServiceDnsName="redisbackend.testapp">
      <StatelessService ServiceTypeName="azurevotebackType" InstanceCount="1">
        <SingletonPartition/>
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

### <a name="front-end-servicemanifestxml"></a>Front-ServiceManifest.xml 
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="azurevotefrontPkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="azurevotefrontType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>
   
   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>acrName.azurecr.io/azure-vote-front:v1</ImageName>
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
      <Endpoint Name="azurevotefrontTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>

 </ServiceManifest>
```

### <a name="redis-servicemanifestxml"></a>ServiceManifest.xml redis
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="azurevotebackPkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="azurevotebackType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>
   
   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>alpine:redis</ImageName>
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
      <Endpoint Name="azurevotebackTypeEndpoint" Port="6379" Protocol="tcp"/>
    </Endpoints>
  </Resources>
 </ServiceManifest>
```
## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie zijn meerdere containers in een Service Fabric-toepassing met behulp van Yeoman verpakt. Deze toepassing is vervolgens geïmplementeerd en uitgevoerd op een Service Fabric-cluster. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * Yeoman installeren  
> * Maken van een toepassingspakket met Yeoman
> * Instellingen configureren in het toepassingspakket voor gebruik met containers
> * De toepassing bouwen  
> * Implementeren en uitvoeren van de toepassing 
> * Opschonen van de toepassing

Ga naar de volgende zelfstudie voor meer informatie over failover en schalen van de toepassing in Service Fabric.

> [!div class="nextstepaction"]
> [Meer informatie over failover en toepassingen vergroten/verkleinen](service-fabric-tutorial-containers-failover.md)

[votingapp]: ./media/service-fabric-tutorial-deploy-run-containers/votingapp.png
[sfx]: ./media/service-fabric-tutorial-deploy-run-containers/containerspackagetutorialsfx.png


