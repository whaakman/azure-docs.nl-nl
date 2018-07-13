---
title: Een Spring Boot-app maken in Service Fabric in Azure | Microsoft Docs
description: In deze snelstart implementeert u een Spring Boot-toepassing voor Azure Service Fabric met behulp van een Spring Boot-voorbeeldtoepassing.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/23/2017
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: 612c397900d496e7f2b92e70f1a1521741ba84e9
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112232"
---
# <a name="quickstart-deploy-a-java-spring-boot-application-to-service-fabric"></a>Snelstart: een Java Spring Boot-toepassing in Azure implementeren

Azure Service Fabric is een platform voor gedistribueerde systemen voor het implementeren en distribueren van microservices en containers.

Deze snelstart laat zien hoe u een Spring Boot-toepassing implementeert in Service Fabric. Deze snelstart gebruikt het [Aan de slag](https://spring.io/guides/gs/spring-boot/)-voorbeeld van de Spring-website. Met gebruik van vertrouwde opdrachtregelprogramma’s wordt u in deze snelstart stapsgewijs begeleid bij het implementeren van het Spring Boot-voorbeeld als een Service Fabric-toepassing. Als u klaar bent, werkt het Spring Boot Aan de slag-voorbeeld in Service Fabric.

![Schermafbeelding van de toepassing](./media/service-fabric-quickstart-java-spring-boot/springbootsflocalhost.png)

In deze snelstart leert u de volgende zaken:

* Een Spring Boot-toepassing implementeren in Service Fabric
* De toepassing implementeren in het lokale cluster
* De toepassing implementeren in een cluster in Azure
* De toepassing uitschalen over meerdere knooppunten
* Failover van de service uitvoeren met geen beschikbaarheid

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze snelstart:

1. Service Fabric-SDK en Service Fabric CLI (opdrachtregelinterface) installeren

    a. [Mac](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cli#cli-mac)
    
    b. [Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods)

2. [Git installeren](https://git-scm.com/)
3. Yeoman installeren

    a. [Mac](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-mac#create-your-application-on-your-mac-by-using-yeoman)

    b. [Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-yeoman-generators-for-containers-and-guest-executables)
4. Java-omgeving instellen

    a. [Mac](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-mac#create-your-application-on-your-mac-by-using-yeoman)
    
    b.  [Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development)

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Voer in een terminalvenster de volgende opdracht uit om het Spring Boot Aan de slag-voorbeeld te klonen op de lokale computer.

```bash
git clone https://github.com/spring-guides/gs-spring-boot.git
```

## <a name="build-the-spring-boot-application"></a>De Spring Boot-toepassing compileren 
1. Voer in de directory `gs-spring-boot/complete` de onderstaande opdracht uit om de toepassing te compileren 

    ```bash
    ./gradlew build
    ``` 

## <a name="package-the-spring-boot-application"></a>De Spring Boot-toepassing inpakken 
1. Voer in de `gs-spring-boot`-map van uw kloon de opdracht `yo azuresfguest` uit. 

2. Voer de volgende details in voor elke prompt.

    ![Yeoman-vermeldingen](./media/service-fabric-quickstart-java-spring-boot/yeomanspringboot.png)

3. Maak in de map `SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/code` een bestand met de naam `entryPoint.sh`. Voeg het volgende toe aan het bestand `entryPoint.sh`. 

    ```bash
    #!/bin/bash
    BASEDIR=$(dirname $0)
    cd $BASEDIR
    java -jar gs-spring-boot-0.1.0.jar
    ```

4. Voeg de resource **Eindpunten** toe aan het bestand `gs-spring-boot/SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/ServiceManifest.xml`

    ```xml 
        <Resources>
          <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="8080" />
          </Endpoints>
       </Resources>
    ```

    De **ServiceManifest.xml** ziet er nu als volgt uit: 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceManifest Name="SpringGettingStartedPkg" Version="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

       <ServiceTypes>
          <StatelessServiceType ServiceTypeName="SpringGettingStartedType" UseImplicitHost="true">
       </StatelessServiceType>
       </ServiceTypes>

       <CodePackage Name="code" Version="1.0.0">
          <EntryPoint>
             <ExeHost>
                <Program>entryPoint.sh</Program>
                <Arguments></Arguments>
                <WorkingFolder>CodePackage</WorkingFolder>
             </ExeHost>
          </EntryPoint>
       </CodePackage>
        <Resources>
          <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="8080" />
          </Endpoints>
       </Resources>
     </ServiceManifest>
    ```

In dit stadium hebt u een Service Fabric-toepassing voor het Spring Boot Aan de slag-voorbeeld gemaakt die u kunt implementeren in Service Fabric.

## <a name="run-the-application-locally"></a>De toepassing lokaal uitvoeren

1. Start het lokale cluster op Ubuntu-computers door de volgende opdracht uit te voeren:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

    Als u een Mac gebruikt, start u het lokale cluster vanuit de Docker-installatiekopie (hierbij wordt verondersteld dat is voldaan aan de [vereisten](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-mac#create-a-local-container-and-set-up-service-fabric) voor het instellen van het lokale cluster voor Mac). 

    ```bash
    docker run --name sftestcluster -d -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 -p 8080:8080 mysfcluster
    ```

    Het starten van het lokale cluster kan enige tijd duren. Open Service Fabric Explorer op **http://localhost:19080** om te controleren of het cluster volledig naar behoren functioneert. Als de vijf knooppunten in orde zijn, is het lokale cluster actief. 
    
    ![Lokaal cluster is in orde](./media/service-fabric-quickstart-java-spring-boot/sfxlocalhost.png)

2. Navigeer naar de map `gs-spring-boot/SpringServiceFabric`.
3. Voer de volgende opdracht uit om verbinding te maken met het lokale cluster.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```
4. Voer het `install.sh`-script uit.

    ```bash
    ./install.sh
    ```

5. Open uw favoriete webbrowser en open de toepassing op **http://localhost:8080**.

    ![Front-end van de toepassing voor Local](./media/service-fabric-quickstart-java-spring-boot/springbootsflocalhost.png)

U hebt nu toegang tot de Spring Boot-toepassing die werd geïmplementeerd in een Service Fabric-cluster.

## <a name="deploy-the-application-to-azure"></a>De toepassing implementeren in Azure

### <a name="set-up-your-azure-service-fabric-cluster"></a>Het Azure Service Fabric-cluster instellen

Maak uw eigen cluster om de toepassing te implementeren in een cluster in Azure.

Party-clusters zijn kosteloze, tijdelijke Service Fabric-clusters die worden gehost in Azure en worden uitgevoerd door het Service Fabric-team. U kunt party-clusters gebruiken om toepassingen te implementeren en meer te weten te komen over het platform. Het cluster gebruikt één zelfondertekend certificaat voor beveiliging van knooppunt-naar-knooppunt en client-naar-knooppunt.

Meld u aan en neem deel aan een [Linux-cluster](http://aka.ms/tryservicefabric). Download het PFX-certificaat naar uw computer door op de koppeling **PFX** te klikken. Klik op de koppeling **Leesmij** om het certificaatwachtwoord te vinden en instructies te krijgen over het configureren van verschillende omgevingen voor gebruik van het certificaat. Laat de pagina's **Welkom** en **Leesmij** geopend, want u hebt enkele instructies in de volgende stappen nodig.

> [!Note]
> Er zijn per uur een beperkt aantal party-clusters beschikbaar. Als er een fout optreedt wanneer u zich probeert aan te melden voor een party-cluster, kunt u een bepaalde tijd wachten en het opnieuw proberen, maar u kunt ook deze stappen in [Een Service Fabric-cluster in Azure maken](service-fabric-tutorial-create-vnet-and-linux-cluster.md) volgen om een cluster in uw abonnement te maken.
>
> De Spring Boot-service is geconfigureerd om naar binnenkomend verkeer te luisteren op poort 8080. Zorg ervoor dat de poort is geopend in het cluster. Als u een cluster van derden gebruikt, is deze poort geopend.
>

Service Fabric biedt verschillende hulpmiddelen waarmee u een cluster en de bijbehorende toepassingen kunt beheren:

* Service Fabric Explorer, een op een browser gebaseerd hulpprogramma.
* Service Fabric Command Line Interface (CLI), die wordt uitgevoerd boven op Azure CLI 2.0.
* PowerShell-opdrachten.

In deze snelstart gebruikt u de Service Fabric CLI en Service Fabric Explorer.

Om de CLI te kunnen gebruiken, moet u een PEM-bestand maken op basis van het PFX-bestand dat u hebt gedownload. Als u het bestand wilt converteren, gebruikt u de volgende opdracht. (Voor party-clusters kunt u een voor uw PFX-bestand specifieke opdracht uit de instructies op de pagina **Leesmij** kopiëren.)

```bash
openssl pkcs12 -in party-cluster-1486790479-client-cert.pfx -out party-cluster-1486790479-client-cert.pem -nodes -passin pass:1486790479
``` 

Als u Service Fabric Explorer wilt gebruiken, moet u het PFX-certificaatbestand dat u hebt gedownload van de Party Cluster-website importeren in uw certificaatarchief (Windows of Mac) of in de browser zelf (Ubuntu). U hebt het wachtwoord voor uw persoonlijke PFX-sleutel nodig, die te vinden is op-de pagina **Leesmij**.

Gebruik de methode die u het handigst vindt om het certificaat naar uw systeem te importeren. Bijvoorbeeld:

* Voor Windows: dubbelklik op het PFX-bestand en volg de aanwijzingen om het certificaat in uw persoonlijke archief, `Certificates - Current User\Personal\Certificates`, te installeren. U kunt ook de PowerShell-opdracht in de **Leesmij**-instructies gebruiken.
* Voor Mac: dubbelklik op het PFX-bestand en volg de aanwijzingen om het certificaat in uw Sleutelhanger te installeren.
* Voor Ubuntu: Mozilla Firefox is de standaardbrowser in Ubuntu 16.04. U kunt het certificaat importeren in Firefox door te klikken op de menuknop in de rechterbovenhoek van uw browser en te klikken op **Opties**. Op de pagina **Voorkeuren** gebruikt u het zoekvak om te zoeken naar 'certificaten'. Klik op **Certificaten bekijken**, selecteer het tabblad **Uw certificaten**, klik op **Importeren** en volg de aanwijzingen om het certificaat te importeren.

   ![Certificaat installeren in Firefox](./media/service-fabric-quickstart-java-spring-boot/install-cert-firefox.png)

### <a name="deploy-the-application-using-cli"></a>De toepassing implementeren met behulp van CLI

Nu de toepassing en het cluster gereed zijn, kunt u deze rechtstreeks vanuit de opdrachtregel implementeren in het cluster.

1. Navigeer naar de map `gs-spring-boot/SpringServiceFabric`.
2. Voer de volgende opdracht uit om verbinding te maken met het Azure-cluster.

    ```bash
    sfctl cluster select --endpoint https://<ConnectionIPOrURL>:19080 --pem <path_to_certificate> --no-verify
    ```
3. Voer het `install.sh`-script uit.

    ```bash
    ./install.sh
    ```

4. Open uw webbrowser en open de toepassing op **http://\<ConnectionIPOrUrl>:8080**.

    ![Front-end van de toepassing voor Local](./media/service-fabric-quickstart-java-spring-boot/springbootsfazure.png)

U kunt de Spring Boot-toepassing die wordt uitgevoerd in een Service Fabric-cluster op Azure nu benaderen.

## <a name="scale-applications-and-services-in-a-cluster"></a>Toepassingen en services voor schalen in een cluster

Services kunnen eenvoudig worden geschaald in een cluster om een wijziging in de belasting voor de services aan te kunnen. U schaalt een service door het aantal exemplaren te wijzigen dat wordt uitgevoerd in het cluster. Er zijn veel manieren waarop u services kunt schalen. U kunt bijvoorbeeld scripts of opdrachten van Service Fabric CLI (sfctl) gebruiken. In de volgende stappen wordt Service Fabric Explorer gebruikt.

Service Fabric Explorer kan worden uitgevoerd in alle Service Fabric-clusters en is toegankelijk door vanuit een browser te bladeren naar de HTTP-beheerpoort (19080) van het cluster, bijvoorbeeld `http://localhost:19080`.

Voer de volgende stappen uit om de web-front-endservice te schalen:

1. Open Service Fabric Explorer in het cluster - bijvoorbeeld: `http://localhost:19080`.
2. Klik op het beletselteken (drie punten) naast het knooppunt **fabric:/SpringServiceFabric/SpringGettingStarted** in de structuurweergave en kies **Service schalen**.

    ![De Service Fabric Explorer-service schalen](./media/service-fabric-quickstart-java-spring-boot/sfxscaleservicehowto.png)

    U kunt er nu voor kiezen om het aantal exemplaren van de service te schalen.

3. Wijzig het aantal in **3** en klik op **Service schalen**.

    Een alternatieve manier om de service te schalen met behulp van de opdrachtregel gaat als volgt.

    ```bash
    # Connect to your local cluster
    sfctl cluster select --endpoint https://<ConnectionIPOrURL>:19080 --pem <path_to_certificate> --no-verify

    # Run Bash command to scale instance count for your service
    sfctl service update --service-id 'SpringServiceFabric~SpringGettingStarted' --instance-count 3 --stateless 
    ``` 

4. Klik op het knooppunt **fabric:/SpringServiceFabric/SpringGettingStartedt** in de structuurweergave en vouw het partitieknooppunt uit (vertegenwoordigd door een GUID).

    ![Schalen van Service Fabric Explorer-service is voltooid](./media/service-fabric-quickstart-java-spring-boot/sfxscaledservice.png)

    De service heeft drie exemplaren en de structuurweergave laat zien op welke knooppunten de exemplaren worden uitgevoerd.

Met deze eenvoudige beheertaak hebt u het aantal beschikbare resources voor het verwerken van gebruikersbelasting voor de front-endservice verdubbeld. Het is belangrijk te weten dat u niet meerdere exemplaren van een service nodig hebt om ervoor te zorgen dat deze op betrouwbare wijze wordt uitgevoerd. Als de service mislukt, wordt in Service Fabric een nieuw exemplaar van de service uitgevoerd in het cluster.

## <a name="fail-over-services-in-a-cluster"></a>Failoverservices in een cluster

Het opnieuw opstarten van een knooppunt kan worden gesimuleerd met behulp van Service Fabric Explorer om failover van de service te demonstreren. Zorg ervoor dat maar één exemplaar van de service wordt uitgevoerd.

1. Open Service Fabric Explorer in het cluster - bijvoorbeeld: `http://localhost:19080`.
2. Klik op het beletselteken (drie punten) naast het knooppunt waarop het service-exemplaar wordt uitgevoerd, en start het knooppunt opnieuw.

    ![Knooppunt opnieuw starten in Service Fabric Explorer](./media/service-fabric-quickstart-java-spring-boot/sfxhowtofailover.png)
3. Het service-exemplaar wordt naar een ander knooppunt verplaatst en er treedt geen downtime op voor de toepassing.

    ![Knooppunt opnieuw starten in Service Fabric Explorer is voltooid](./media/service-fabric-quickstart-java-spring-boot/sfxfailedover.png)

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u de volgende zaken geleerd:

* Een Spring Boot-toepassing implementeren in Service Fabric
* De toepassing implementeren in het lokale cluster
* De toepassing implementeren in een cluster in Azure
* De toepassing uitschalen over meerdere knooppunten
* Failover van de service uitvoeren met geen beschikbaarheid

Meer informatie over het werken met Java-apps in Service Fabric vindt u in de zelfstudie voor Java-apps.

> [!div class="nextstepaction"]
> [Een Java-app implementeren](./service-fabric-tutorial-create-java-app.md)