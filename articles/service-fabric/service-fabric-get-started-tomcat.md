---
title: Een Azure Service Fabric-container voor Apache Tomcat-server maken in Linux | Microsoft Docs
description: Linux-container als u een toepassing die wordt uitgevoerd op Apache Tomcat-server op Azure Service Fabric beschikbaar wilt maken. Bouw een Docker-installatiekopie met uw toepassing en de Apache Tomcat-server, de installatiekopie naar een containerregister pushen, bouwen en implementeren van een Service Fabric-containertoepassing.
services: service-fabric
documentationcenter: .net
author: JimacoMS2
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/08/2018
ms.author: v-jamebr
ms.openlocfilehash: 5ae2ca352c6d3cbe02b659a97fe3147c1a31128f
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58664570"
---
# <a name="create-service-fabric-container-running-apache-tomcat-server-on-linux"></a>Service Fabric-container die Apache Tomcat-server op Linux maken
Apache Tomcat is een populaire, open-source implementatie van de Java Servlet- en Java-Server-technologieën. Dit artikel ziet u hoe u een container met Apache Tomcat en een eenvoudige webtoepassing bouwen, de container geïmplementeerd naar een Service Fabric-cluster waarop Linux wordt uitgevoerd en verbinding maken met de Web-App.  

Zie voor meer informatie over de Apache Tomcat, de [Apache Tomcat-startpagina](https://tomcat.apache.org/). 

## <a name="prerequisites"></a>Vereisten
* Een ontwikkelcomputer waarop wordt uitgevoerd:
  * [Service Fabric SDK en hulpprogramma's](service-fabric-get-started-linux.md).
  * [Docker CE voor Linux](https://docs.docker.com/engine/installation/#prior-releases). 
  * [Service Fabric-CLI](service-fabric-cli.md)

* Een containerregister in Azure Container Registry. U kunt een containerregister maken in uw Azure-abonnement met [de Azure-portal](../container-registry/container-registry-get-started-portal.md) of [de Azure CLI](./service-fabric-tutorial-create-container-images.md#deploy-azure-container-registry). 

## <a name="build-a-tomcat-image-and-run-it-locally"></a>Bouw een installatiekopie Tomcat en lokaal uitvoeren
Volg de stappen in deze sectie om een Docker-installatiekopie op basis van een installatiekopie van een Apache Tomcat en een eenvoudige Web-app bouwen en uitvoeren in een container in uw lokale systeem. 
 
1. Kloon de [Service Fabric-aan de slag met Java](https://github.com/Azure-Samples/service-fabric-java-getting-started) opslagplaats met voorbeelden op uw ontwikkelcomputer.

   ```bash
   git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
   ```

1. Wijzig de mappen in de map voorbeeld van Apache Tomcat-server (*service-fabric-java-getting-started/container-apache-tomcat-web-server-sample*):

   ```bash
   cd service-fabric-java-getting-started/container-apache-tomcat-web-server-sample
   ```

1. Maak een Docker-bestand op basis van de officiële [Tomcat installatiekopie](https://hub.docker.com/_/tomcat/) zich op Docker Hub en voorbeeld van de Tomcat-server. In de *service-fabric-java-getting-started/container-apache-tomcat-web-server-sample* directory, maak een bestand met de naam *Dockerfile* (met zonder extensie). Voeg het volgende toe aan het bestand *Dockerfile* en sla de wijzigingen op:

   ```
   FROM library/tomcat

   EXPOSE 8080

   COPY ./ApacheTomcat /usr/local/tomcat
   ```

   Zie de [Dockerfile-referentiemateriaal](https://docs.docker.com/engine/reference/builder/) voor meer informatie.


4. Voer de `docker build` opdracht voor het maken van de installatiekopie die de webtoepassing wordt uitgevoerd:

   ```bash
   docker build . -t tomcattest
   ```

   Met deze opdracht maakt de nieuwe installatiekopie van de instructies in de Dockerfile naming (-t tagging) de installatiekopie van het `tomcattest`. Voor het bouwen van een containerinstallatiekopie, de basisinstallatiekopie is eerst gedownload vanaf Docker Hub en de toepassing wordt toegevoegd aan het. 

   Nadat de buildopdracht is voltooid, voert u de opdracht `docker images` uit om de gegevens van de nieuwe installatiekopie te bekijken:

   ```bash
   $ docker images
    
   REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
   tomcattest                    latest              86838648aab6        2 minutes ago       194 MB
   ```

5. Controleer of dat uw containertoepassing lokaal voordat deze het containerregister pushen wordt uitgevoerd:
 
   ```bash
   docker run -itd --name tomcat-site -p 8080:8080 tomcattest.
   ```
   
   * `--name` namen van de container, zodat u kunt verwijzen met behulp van een beschrijvende naam in plaats van de ID.
   * `-p` Hiermee geeft u de poorttoewijzing tussen de container en het hostbesturingssysteem. 

   > [!Note]
   > De poort die u kunt met openen de `-p` parameter moet de poort die uw toepassing Tomcat naar aanvragen luistert. In het huidige voorbeeld is een Connector is geconfigureerd de *ApacheTomcat/conf/server.xml* bestand om te luisteren op poort 8080 voor HTTP-aanvragen. Deze poort wordt toegewezen aan poort 8080 op de host. 

   Zie voor meer informatie over andere parameters, de [documentatie: Docker run](https://docs.docker.com/engine/reference/commandline/run/).

1. Als u wilt uw container testen, open een browser en voer een van de volgende URL's. U ziet een variant van de "Hello World!" welkomstscherm wordt weergegeven voor elke URL.

   - `http://localhost:8080/hello` 
   - `http://localhost:8080/hello/sayhello` 
   - `http://localhost:8080/hello/sayhi` 

   ![Hello world /sayhi](./media/service-fabric-get-started-tomcat/hello.png)

2. De container stoppen en deze op uw ontwikkelcomputer verwijderen:

   ```bash
   docker stop tomcat-site
   docker rm tomcat-site
   ```

## <a name="push-the-tomcat-image-to-your-container-registry"></a>De Tomcat-installatiekopie naar het containerregister pushen
Nu dat u hebt gecontroleerd dat de Tomcat-installatiekopie wordt uitgevoerd in een container op uw ontwikkelcomputer, kunt u deze naar een opslagplaats in een containerregister pushen. In dit artikel wordt Azure Container Registry voor het opslaan van de installatiekopie, maar met enkele aanpassingen van de stappen, kunt u een containerregister die u kiest. In dit artikel de naam van het containerregister wordt ervan uitgegaan dat *myregistry* en is de naam van het volledige containerregister myregistry.azurecr.io. Deze op de juiste wijze voor uw scenario wijzigen. 

1. Voer `docker login` uit om u bij uw containerregister aan te melden met uw [registerreferenties](../container-registry/container-registry-authentication.md).

   In het volgende voorbeeld worden de id en het wachtwoord van een [service-principal](../active-directory/develop/app-objects-and-service-principals.md) van Azure Active Directory doorgegeven. U hebt bijvoorbeeld een service-principal aan uw register toegewezen voor een automatiseringsscenario. Of u kunt zich aanmelden met uw gebruikersnaam en wachtwoord van het register.

   ```bash
   docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
   ```

2. Met de volgende opdracht maakt u een tag (of alias) van de installatiekopie met een volledig gekwalificeerd pad naar uw register. In dit voorbeeld wordt de installatiekopie in de naamruimte `samples` geplaatst om overbodige items in de hoofdmap van het register te voorkomen.

   ```bash
   docker tag tomcattest myregistry.azurecr.io/samples/tomcattest
   ```

3. De installatiekopie naar het containerregister pushen:

   ```bash
   docker push myregistry.azurecr.io/samples/tomcattest
   ```

## <a name="build-and-deploy-the-service-fabric-container-application"></a>Bouw en implementeer de Service Fabric-containertoepassing
Nu dat u hebt de Tomcat-installatiekopie naar een containerregister hebt gepusht, kunt u deze kunt bouwen en implementeren van een Service Fabric-containertoepassing die de Tomcat-installatiekopie ophaalt uit het register en wordt deze uitgevoerd als een beperkte service in uw cluster. 

1. Maak een nieuwe map buiten de lokale kloon (buiten de *service-fabric-java-getting-started* directory-structuur). Ga naar het en Yeoman gebruiken voor het maken van een scaffold voor een toepassing met containers: 

   ```bash
   yo azuresfcontainer 
   ```
   Voer de volgende waarden wanneer hierom wordt gevraagd:

   * Geef uw toepassing: ServiceFabricTomcat
   * Naam van de toepassingsservice: TomcatService
   * Voer de naam van de installatiekopie: Geef de URL voor de installatiekopie van de container in het containerregister; bijvoorbeeld, myregistry.azurecr.io/samples/tomcattest.
   * Opdrachten: Laat dit leeg. Omdat voor deze installatiekopie een workloadinvoerpunt is gedefinieerd, hoeft u niet expliciet invoeropdrachten op te geven (opdrachten worden uitgevoerd in de container, zodat de container na het opstarten actief blijft).
   * Het aantal exemplaren van de Gast-containertoepassing: 1

   ![Service Fabric Yeoman-generator voor containers](./media/service-fabric-get-started-tomcat/yo-generator.png)

10. In het servicemanifest (*ServiceFabricTomcat/ServiceFabricTomcat/TomcatServicePkg/ServiceManifest.xml*), voeg de volgende XML-code in de basismap **ServiceManfest** tag om de poort te openen uw toepassing luistert naar aanvragen. De **eindpunt** tag worden gedeclareerd voor het protocol en poort voor het eindpunt. In dit artikel wordt de beperkte service luistert op poort 8080: 

   ```xml
   <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
       listen. Please note that if your service is partitioned, this port is shared with 
       replicas of different partitions that are placed in your code. -->
      <Endpoint Name="endpointTest" Port="8080" Protocol="tcp"/>
    </Endpoints>
   </Resources>
   ```

11. In het toepassingsmanifest (*ServiceFabricTomcat/ServiceFabricTomcat/ApplicationManifest.xml*), onder de **ServiceManifestImport** taggen, het volgende XML-bestand toevoegen. Vervang de **AccountName** en **wachtwoord** in de **RepositoryCredentials** tag met de naam van het containerregister en het wachtwoord is vereist voor aanmelding bij deze.

   ```xml
   <Policies>
    <ContainerHostPolicies CodePackageRef="Code">
      <PortBinding ContainerPort="8080" EndpointRef="endpointTest"/>
      <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
    </ContainerHostPolicies>
   </Policies>
   ```

   De **ContainerHostPolicies** tag Hiermee geeft u het beleid voor het activeren van de hosts van de container.
    
   * De **PortBinding** tag configureert u het beleid van de toewijzing van container-poort-naar-host. De **ContainerPort** kenmerk is ingesteld op 8080 omdat de container poort 8080, zoals opgegeven in de Dockerfile gebruikt. De **EndpointRef** kenmerk is ingesteld op 'endpointTest', het eindpunt dat is gedefinieerd in het servicemanifest in de vorige stap. Dus, binnenkomende aanvragen naar de service op poort 8080 worden toegewezen aan poort 8080 voor de container. 
   * De **RepositoryCredentials** tag Hiermee geeft u de referenties die de container moet worden geverifieerd bij de (privé) waar deze wordt opgehaald van een installatiekopie van-opslagplaats. U hoeft niet op dit beleid als de afbeelding wordt worden opgehaald uit een openbare opslagplaats.
    

12. In de *ServiceFabricTomcat* map, verbinding maken met uw service fabric-cluster. 

   * Voor verbinding met het lokale Service Fabric-cluster, voert u de volgende uit:

     ```bash
     sfctl cluster select --endpoint http://localhost:19080
     ```
    
   * Voor verbinding met een beveiligde Azure-cluster, zorg ervoor dat het clientcertificaat is aanwezig is als een .pem-bestand in de *ServiceFabricTomcat* Active directory en uitvoeren: 

     ```bash
     sfctl cluster select --endpoint https://PublicIPorFQDN:19080 -pem your-certificate.pem -no-verify
     ```
     Vervang in de voorgaande opdracht `your-certificate.pem` met de naam van uw client-certificaatbestand. In de ontwikkel- en testomgevingen, wordt het clustercertificaat dat is vaak gebruikt als het clientcertificaat. Als uw certificaat zelf niet is ondertekend, laat de `-no-verify` parameter. 
       
     Clustercertificaten zijn meestal lokaal gedownload als pfx-bestanden. Als u het certificaat nog in de PEM-indeling hebt, kunt u de volgende opdracht om een PEM-bestand maken vanuit een pfx-bestand uitvoeren:

     ```bash
     openssl pkcs12 -in your-certificate.pfx -out your-certificate.pem -nodes -passin pass:your-pfx-password
     ```

     Als uw pfx-bestand niet beveiligd met een wachtwoord is, gebruikt u `-passin pass:` voor de laatste parameter zijn.


13. Voer het installatiescript dat is opgegeven in de sjabloon om de toepassing in uw cluster te implementeren. Het script het toepassingspakket worden gekopieerd naar de installatiekopieopslag van het cluster, het toepassingstype registreert en maakt u een exemplaar van de toepassing.

     ```bash
     ./install.sh
     ```

   Nadat u het installatiescript hebt uitgevoerd, open een browser en Ga naar Service Fabric Explorer:
    
   * Gebruik op een lokaal cluster `http://localhost:19080/Explorer` (Vervang *localhost* met het particuliere IP-adres van de virtuele machine als u Vagrant in Mac OS X).
   * Gebruik op een beveiligde Azure-cluster, `https://PublicIPorFQDN:19080/Explorer`. 
    
   Vouw de **toepassingen** knooppunt en Let op: er nu een vermelding voor uw toepassingstype is **ServiceFabricTomcatType**, en een andere voor het eerste exemplaar van dat type. Het kan enkele minuten duren voor de toepassing volledig implementeren, worden dus even geduld.

   ![Service Fabric Explorer](./media/service-fabric-get-started-tomcat/service-fabric-explorer.png)


1. Voor toegang tot de toepassing op de Tomcat-server, open een browservenster en voer een van de volgende URL's. Als u met het lokale cluster geïmplementeerd, gebruikt u *localhost* voor *PublicIPorFQDN*. U ziet een variant van de "Hello World!" welkomstscherm wordt weergegeven voor elke URL.

   * http://PublicIPorFQDN:8080/hello  
   * http://PublicIPorFQDN:8080/hello/sayhello
   * http://PublicIPorFQDN:8080/hello/sayhi

## <a name="clean-up"></a>Opruimen
Gebruik het uninstall-script dat is opgegeven in de sjabloon te verwijderen van exemplaar van de toepassing van het cluster en de registratie van het toepassingstype.

```bash
./uninstall.sh
```

Nadat u de installatiekopie naar het containerregister hebt gepusht, kunt u de lokale installatiekopie op de ontwikkelcomputer verwijderen:

```
docker rmi tomcattest
docker rmi myregistry.azurecr.io/samples/tomcattest
```

## <a name="next-steps"></a>Volgende stappen
* Lees voor snelle stappen voor aanvullende functies voor Linux-container [uw eerste Service Fabric-containertoepassing maken in Linux](service-fabric-get-started-containers-linux.md).
* Lees voor meer stappen voor het Linux-containers gedetailleerde, de [maken van een Linux-container-app-zelfstudie](service-fabric-tutorial-create-container-images.md) zelfstudie.
* Meer informatie over het uitvoeren van [containers in Service Fabric](service-fabric-containers-overview.md).


