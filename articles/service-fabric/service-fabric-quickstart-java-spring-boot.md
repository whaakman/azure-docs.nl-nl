---
title: Een Spring Boot-toepassing implementeren in Azure Service Fabric | Microsoft Docs
description: Een Spring Boot-toepassing implementeren in Azure Service Fabric met behulp van de Spring Boot Aan de slag.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/23/2017
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: 544f189e79733c6476bf71e9ce39ab5f35e3d032
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="deploy-a-spring-boot-application"></a>Een Spring Boot-toepassing implementeren
Azure Service Fabric is een platform voor gedistribueerde systemen voor het implementeren en distribueren van microservices en containers. 

In deze quickstart wordt gedemonstreerd hoe u een Spring Boot-toepassing implementeert in Service Fabric. Deze quickstart gebruikt het [Aan de slag](https://spring.io/guides/gs/spring-boot/)-voorbeeld van de Spring-website. Met gebruik van vertrouwde opdrachtregelprogramma’s wordt u in deze quickstart stapsgewijs begeleid bij het implementeren van het Spring Boot-voorbeeld als een Service Fabric-toepassing. Als u klaar bent, werkt het Spring Boot Aan de slag-voorbeeld in Service Fabric. 

![Schermafbeelding van de toepassing](./media/service-fabric-quickstart-java-spring-boot/springbootsflocalhost.png)

In deze snelstartgids leert u de volgende zaken:

> [!div class="checklist"]
> * Een Spring Boot-toepassing implementeren in Service Fabric
> * De toepassing implementeren in het lokale cluster 
> * De toepassing implementeren in een cluster in Azure
> * De toepassing uitschalen over meerdere knooppunten
> * Failover van de service uitvoeren met geen beschikbaarheid

## <a name="prerequisites"></a>Vereisten
Dit zijn de vereisten voor het voltooien van deze Quickstart:
1. [Service Fabric-SDK en Service Fabric CLI (opdrachtregelinterface) installeren](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods)
2. [Git installeren](https://git-scm.com/)
3. [Yeoman installeren](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-yeoman-generators-for-containers-and-guest-executables)
4. [Java-omgeving instellen](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development)

## <a name="download-the-sample"></a>Het voorbeeld downloaden
Voer in een opdrachtvenster de volgende opdracht uit om het Spring Boot Aan de slag-voorbeeld te klonen op de lokale computer.
```
git clone https://github.com/spring-guides/gs-spring-boot.git
```

## <a name="package-the-spring-boot-application"></a>De Spring Boot-toepassing inpakken 
1. Voer in de gekloonde `gs-spring-boot`-map de opdracht `yo azuresfguest` uit. 

2. Voer de volgende details in voor elke prompt. 

    ![Yeoman-vermeldingen](./media/service-fabric-quickstart-java-spring-boot/yeomanspringboot.png)

3. Maak in de map `SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/code` een bestand met de naam `entryPoint.sh`. Voeg het volgende toe aan het bestand. 

    ```bash
    #!/bin/bash
    BASEDIR=$(dirname $0)
    cd $BASEDIR
    java -jar gs-spring-boot-0.1.0.jar
    ```

In dit stadium hebt u een Service Fabric-toepassing voor het Spring Boot Aan de slag-voorbeeld gemaakt die u kunt implementeren in Service Fabric.

## <a name="run-the-application-locally"></a>De toepassing lokaal uitvoeren
1. Start het lokale cluster door de volgende opdracht uit te voeren:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
    Het starten van het lokale cluster kan enige tijd duren. Open Service Fabric Explorer op **http://localhost:19080** om te controleren of het cluster helemaal naar behoren functioneert. Als de vijf knooppunten in orde zijn, is het lokale cluster actief. 
    
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

Clusters van derden zijn gratis tijdelijke Service Fabric-clusters die worden gehost in Azure. Ze worden beheerd door het Service Fabric-team. Iedereen kan hier toepassingen implementeren en informatie krijgen over het platform. [Volg de instructies](http://aka.ms/tryservicefabric) om toegang te krijgen tot een cluster van derden. 

Zie [Een Service Fabric-cluster maken op Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md) voor meer informatie over het maken van uw eigen cluster.

> [!Note]
> De Spring Boot-service is geconfigureerd om naar binnenkomend verkeer te luisteren op poort 8080. Zorg ervoor dat de poort is geopend in het cluster. Als u een cluster van derden gebruikt, is deze poort geopend.
>

### <a name="deploy-the-application-using-cli"></a>De toepassing implementeren met behulp van CLI
Nu de toepassing en het cluster gereed zijn, kunt u deze rechtstreeks vanuit de opdrachtregel implementeren in het cluster.

1. Navigeer naar de map `gs-spring-boot/SpringServiceFabric`.
2. Voer de volgende opdracht uit om verbinding te maken met het Azure-cluster. 

    ```bash
    sfctl cluster select --endpoint http://<ConnectionIPOrURL>:19080
    ```
    
    Als het cluster is beveiligd met een zelfondertekend certificaat, is dit de opdracht die u uitvoert: 

    ```bash
    sfctl cluster select --endpoint https://<ConnectionIPOrURL>:19080 --pem <path_to_certificate> --no-verify
    ```
3. Voer het `install.sh`-script uit. 

    ```bash
    ./install.sh
    ```

4. Open uw favoriete webbrowser en open de toepassing op **http://\<ConnectionIPOrUrl>:8080**. 

    ![Front-end van de toepassing voor Local](./media/service-fabric-quickstart-java-spring-boot/springbootsfazure.png)
    
U hebt nu toegang tot de Spring Boot-toepassing die werd geïmplementeerd in een Service Fabric-cluster.  
    
## <a name="scale-applications-and-services-in-a-cluster"></a>Toepassingen en services voor schalen in een cluster
Services kunnen eenvoudig worden geschaald in een cluster om een wijziging in de belasting voor de services aan te kunnen. U schaalt een service door het aantal exemplaren te wijzigen dat wordt uitgevoerd in het cluster. Er zijn meerdere manieren waarop u services kunt schalen. U kunt hiervoor scripts of opdrachten van Service Fabric-CLI (sfctl) gebruiken. In dit voorbeeld wordt Service Fabric Explorer gebruikt.

Service Fabric Explorer kan worden uitgevoerd in alle Service Fabric-clusters en is toegankelijk door vanuit een browser naar de HTTP-beheerpoort (19080) te bladeren, bijvoorbeeld `http://localhost:19080`.

Voer de volgende stappen uit om de web-front-endservice te schalen:

1. Open Service Fabric Explorer in het cluster - bijvoorbeeld: `http://localhost:19080`.
2. Klik op het beletselteken (drie punten) naast het knooppunt **fabric:/SpringServiceFabric/SpringGettingStarted** in de structuurweergave en kies **Service schalen**.

    ![De Service Fabric Explorer-service schalen](./media/service-fabric-quickstart-java-spring-boot/sfxscaleservicehowto.png)

    U kunt er nu voor kiezen om het aantal exemplaren van de service te schalen.

3. Wijzig het aantal in **3** en klik op **Service schalen**.

    Een alternatieve manier om de service te schalen met behulp van de opdrachtregel gaat als volgt.

    ```bash 
    # Connect to your local cluster
    sfctl cluster select --endpoint http://localhost:19080

    # Run Bash command to scale instance count for your service
    sfctl service update --service-id 'SpringServiceFabric~SpringGettingStarted` --instance-count 3 --stateless 
    ``` 

4. Klik op het knooppunt **fabric:/SpringServiceFabric/SpringGettingStartedt** in de structuurweergave en vouw het partitieknooppunt uit (vertegenwoordigd door een GUID).

    ![Schalen van Service Fabric Explorer-service is voltooid](./media/service-fabric-quickstart-java-spring-boot/sfxscaledservice.png)

    De service heeft drie exemplaren en de structuurweergave laat zien op welke knooppunten de exemplaren worden uitgevoerd.

Met deze eenvoudige beheertaak wordt het aantal resources verhoogd dat beschikbaar is voor de Spring-service voor het verwerken van gebruikersbelasting. Het is belangrijk te weten dat u niet meerdere exemplaren van een service nodig hebt om ervoor te zorgen dat deze op betrouwbare wijze wordt uitgevoerd. Als de service mislukt, wordt in Service Fabric een nieuw exemplaar van de service uitgevoerd in het cluster.

## <a name="fail-over-services-in-a-cluster"></a>Failoverservices in een cluster 
Het opnieuw opstarten van een knooppunt kan worden gesimuleerd met behulp van Service Fabric Explorer om failover van de service te demonstreren. Zorg ervoor dat maar één exemplaar van de service wordt uitgevoerd. 

1. Open Service Fabric Explorer in het cluster - bijvoorbeeld: `http://localhost:19080`.
2. Klik op het beletselteken (drie punten) naast het knooppunt waarop het service-exemplaar wordt uitgevoerd, en start het knooppunt opnieuw. 

    ![Knooppunt opnieuw starten in Service Fabric Explorer](./media/service-fabric-quickstart-java-spring-boot/sfxhowtofailover.png)
3. Het service-exemplaar wordt naar een ander knooppunt verplaatst en er treedt geen downtime op voor de toepassing. 

    ![Knooppunt opnieuw starten in Service Fabric Explorer is voltooid](./media/service-fabric-quickstart-java-spring-boot/sfxfailedover.png)

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u de volgende zaken geleerd:

> [!div class="checklist"]
> * Een Spring Boot-toepassing implementeren in Service Fabric
> * De toepassing implementeren in het lokale cluster 
> * De toepassing implementeren in een cluster in Azure
> * De toepassing uitschalen over meerdere knooppunten
> * Failover van de service uitvoeren met geen beschikbaarheid

* Meer informatie krijgen over het [bouwen van Java-microservices met behulp van Service Fabric-programmeermodellen](service-fabric-quickstart-java-reliable-services.md)
* Meer informatie over [doorlopende integratie en implementatie instellen met behulp van Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md)
* Andere [Java-voorbeelden](https://github.com/Azure-Samples/service-fabric-java-getting-started) bekijken