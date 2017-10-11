---
title: Een App Spring opstarten op Kubernetes in Azure Containerservice implementeren | Microsoft Docs
description: Deze zelfstudie leert u Hoewel de stappen voor het implementeren van een toepassing Spring opstarten in een Kubernetes cluster op Microsoft Azure.
services: container-service
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.custom: mvc
ms.openlocfilehash: 7f726436b2d459b8c16abb02e07de099abfd8974
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-spring-boot-application-on-a-kubernetes-cluster-in-the-azure-container-service"></a>Een Spring Boot-toepassing implementeren op een Kubernetes-cluster in Azure Container Service

De  **[Spring Framework]**  is een populaire open-source framework waarmee ontwikkelaars van Java web, mobiel en API-toepassingen maken. In deze zelfstudie wordt een voorbeeld-app gemaakt met behulp van [Spring Boot], een conventie voor gestuurde benadering voor het gebruik van Spring snel aan de slag.

**[Kubernetes]**  en  **[Docker]**  zijn open source-oplossingen die ontwikkelaars helpen bij het automatiseren van de implementatie, schaalbaarheid en beheer van hun toepassingen die worden uitgevoerd containers.

Deze zelfstudie wordt u al het combineren van deze twee populaire open-source technologieën voor het ontwikkelen en implementeren van een toepassing Spring opstarten naar Microsoft Azure. Meer specifiek, gebruikt u  *[Spring Boot]*  voor toepassingsontwikkeling,  *[Kubernetes]*  voor implementatie van de container en de [ Azure Container Service (ACS)] voor het hosten van uw toepassing.

### <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement; Als u nog geen Azure-abonnement hebt, kunt u activeren uw [voordelen als MSDN-abonnee] of zich aanmelden voor een [gratis Azure-account].
* De [Azure-opdrachtregelinterface (CLI)].
* Een up-to-date [Java Developer Kit (JDK)].
* Apache van [Maven] bouwen tool (versie 3).
* Een [Git] client.
* Een [Docker] client.

> [!NOTE]
>
> Vanwege de virtualisatie-vereisten van deze zelfstudie, kan niet u de stappen in dit artikel volgen op een virtuele machine; u moet een fysieke computer gebruiken met virtualisatiefuncties ingeschakeld.
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>Maken van de opstartinstallatiekopie Spring op web-app Docker aan de slag

De volgende stappen maakt u bij het bouwen van een webtoepassing Spring opstarten en lokaal te testen.

1. Open een opdrachtprompt en maak een lokale map houdt uw toepassing en wijzig in die map; bijvoorbeeld:
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- of--
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Kloon de [Spring opstarten op het Docker aan de slag] voorbeeldproject naar de map.
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Wijzig de directory aan het voltooide project.
   ```
   cd gs-spring-boot-docker
   cd complete
   ```

1. Maven gebruiken om te bouwen en uitvoeren van de voorbeeld-app.
   ```
   mvn package spring-boot:run
   ```

1. De web-app testen door te bladeren naar http://localhost: 8080 of met de volgende `curl` opdracht:
   ```
   curl http://localhost:8080
   ```

1. U ziet het volgende bericht weergegeven: **Docker HelloWorld**

   ![Lokaal voorbeeld-App bladeren][SB01]

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>Maken van een Azure Container register met de Azure CLI

1. Open een opdrachtprompt.

1. Aanmelden bij uw Azure-account:
   ```azurecli
   az login
   ```

1. Maak een resourcegroep voor de Azure-resources in deze zelfstudie gebruikt.
   ```azurecli
   az group create --name=wingtiptoys-kubernetes --location=eastus
   ```

1. Maak een register persoonlijke Azure-container in de resourcegroep. De zelfstudie duwt de voorbeeld-app als een Docker-installatiekopie in dit register in latere stappen. Vervang `wingtiptoysregistry` met een unieke naam voor het register.
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoys-kubernetes--location eastus \
    --name wingtiptoysregistry --sku Basic
   ```

## <a name="push-your-app-to-the-container-registry"></a>Uw app in het register van de container push

1. Navigeer naar de configuratiemap voor uw installatie Maven (standaard ~/.m2/ of C:\Users\username\.m2) en open de *settings.xml* bestand met een teksteditor.

1. Het wachtwoord voor uw register container ophalen met de Azure CLI.
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```

   ```json
   {
  "name": "password",
  "value": "AbCdEfGhIjKlMnOpQrStUvWxYz"
   }
   ```

1. Uw Azure-Container register-id en wachtwoord toevoegen aan een nieuwe `<server>` verzameling in de *settings.xml* bestand.
De `id` en `username` zijn de naam van het register. Gebruik de `password` waarde uit de vorige opdracht (zonder aanhalingstekens).

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. Ga naar de map voltooid project voor uw toepassing Spring opstarten (bijvoorbeeld '*C:\SpringBoot\gs-spring-boot-docker\complete*"of"*/users/robert/SpringBoot/gs-spring-boot-docker/complete* '), en open de *pom.xml* bestand met een teksteditor.

1. Update de `<properties>` verzameling in de *pom.xml* -bestand met de waarde van de server aanmelding voor uw Azure-Container-register.

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Update de `<plugins>` verzameling in de *pom.xml* bestand zodat de `<plugin>` bevat van de server en het register aanmeldingsnaam voor uw Azure-Container-register.

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. Ga naar de map voltooid project voor uw toepassing Spring opstarten en voer de volgende opdracht voor het bouwen van de Docker-container en de installatiekopie van het push naar het register:

   ```
   mvn package docker:build -DpushImage
   ```

> [!NOTE]
>
>  Er wordt een foutbericht weergegeven dat vergelijkbaar is met een van de volgende wanneer u Maven duwt de installatiekopie naar Azure:
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> Als u deze fout krijgt, meld u aan bij Azure vanaf de opdrachtregel Docker.
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> Vervolgens push uw container:
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`

## <a name="create-a-kubernetes-cluster-on-acs-using-the-azure-cli"></a>Maak een Kubernetes Cluster op ACS met de Azure CLI

1. Maak een Kubernetes-cluster in Azure Container Service. De volgende opdracht maakt u een *kubernetes* -cluster in de *wingtiptoys kubernetes* resource groep met *wingtiptoys containerservice* als de naam van het cluster, en *wingtiptoys kubernetes* als de DNS-voorvoegsel:
   ```azurecli
   az acs create --orchestrator-type=kubernetes --resource-group=wingtiptoys-kubernetes \ 
    --name=wingtiptoys-containerservice --dns-prefix=wingtiptoys-kubernetes
   ```
   Met deze opdracht kan even duren om te voltooien.

1. Installeer `kubectl` met de Azure CLI. Linux-gebruikers hebben mogelijk tot het voorvoegsel van deze opdracht met `sudo` sinds deze de CLI Kubernetes naar implementeert `/usr/local/bin`.
   ```azurecli
   az acs kubernetes install-cli
   ```

1. Downloaden van de configuratie-informatie van het cluster zodat u uw cluster vanuit de webinterface Kubernetes beheren kunt en `kubectl`. 
   ```azurecli
   az acs kubernetes get-credentials --resource-group=wingtiptoys-kubernetes  \ 
    --name=wingtiptoys-containerservice
   ```

## <a name="deploy-the-image-to-your-kubernetes-cluster"></a>De installatiekopie implementeren op uw cluster Kubernetes

Deze zelfstudie implementeert u de app met behulp `kubectl`, klikt u vervolgens kunt u de implementatie via de webinterface Kubernetes verkennen.

### <a name="deploy-with-the-kubernetes-web-interface"></a>Met de webinterface Kubernetes implementeren

1. Open een opdrachtprompt.

1. Open de website van de configuratie voor uw cluster Kubernetes in de browser:
   ```
   az acs kubernetes browse --resource-group=wingtiptoys-kubernetes --name=wingtiptoys-containerservice
   ```

1. Als de website van de configuratie Kubernetes in uw browser wordt geopend, klikt u op de koppeling naar **een beperkte app implementeren**:

   ![Kubernetes configuratie Website][KB01]

1. Wanneer de **een beperkte app implementeren** pagina wordt weergegeven, geeft u de volgende opties:

   a. Selecteer **appdetails hieronder opgeven**.

   b. Voer de naam van uw Spring Boot-toepassing voor de **appnaam**; bijvoorbeeld: '*gs-spring-boot-docker*'.

   c. Voer uw aanmelding server en de container afbeelding van eerder voor de **Container installatiekopie**; bijvoorbeeld: '*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*'.

   d. Kies **externe** voor de **Service**.

   e. Geef uw interne en externe poorten in de **poort** en **poort doel** tekstvakken.

   ![Kubernetes configuratie Website][KB02]


1. Klik op **implementeren** voor het implementeren van de container.

   ![Container implementeren][KB05]

1. Wanneer uw toepassing is geïmplementeerd, ziet u uw toepassing Spring Boot is vermeld in **Services**.

   ![Kubernetes Services][KB06]

1. Als u op de koppeling voor **externe eindpunten**, ziet u uw toepassing Spring opstarten op Azure.

   ![Kubernetes Services][KB07]

   ![Voorbeeld-App in Azure bladeren][SB02]


### <a name="deploy-with-kubectl"></a>Met kubectl implementeren

1. Open een opdrachtprompt.

1. De container in het cluster Kubernetes uitvoeren met behulp van de `kubectl run` opdracht. Geef een servicenaam voor uw app in Kubernetes en naam van de volledige installatiekopie. Bijvoorbeeld:
   ```
   kubectl run gs-spring-boot-docker --image=wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest
   ```
   In deze opdracht:

   * De containernaam `gs-spring-boot-docker` is opgegeven onmiddellijk na de `run` opdracht

   * De `--image` parameter geeft u de gecombineerde server- en afbeeldingsbestanden aanmeldingsnaam als`wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest`

1. Uw cluster Kubernetes blootstellen extern met behulp van de `kubectl expose` opdracht. Geef de servicenaam van uw, de openbare TCP-poort gebruikt voor toegang tot de app en de interne doelpoort die uw app luistert op. Bijvoorbeeld:
   ```
   kubectl expose deployment gs-spring-boot-docker --type=LoadBalancer --port=80 --target-port=8080
   ```
   In deze opdracht:

   * De containernaam `gs-spring-boot-docker` is opgegeven onmiddellijk na de `expose deployment` opdracht

   * De `--type` parameter geeft u op dat de cluster maakt gebruik van load balancer

   * De `--port` parameter geeft u de openbare TCP-poort 80. U toegang tot de app op deze poort.

   * De `--target-port` parameter geeft u op de interne TCP-poort van 8080. De load balancer verzendt aanvragen naar uw app op deze poort.

1. Zodra de app is geïmplementeerd met het cluster, het externe IP-adres zoeken en openen in uw webbrowser:

   ```
   kubectl get services -o jsonpath={.items[*].status.loadBalancer.ingress[0].ip} --namespace=${namespace}
   ```

   ![Voorbeeld-App in Azure bladeren][SB02]


## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over het gebruik van opstarten Spring op Azure:

* [Een toepassing Spring opstart in de Azure App Service implementeren](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Een toepassing Spring opstarten op Linux in de Azure Container Service implementeren](container-service-deploy-spring-boot-app-on-linux.md)

Voor meer informatie over het gebruik van Azure met Java raadpleegt u het [Azure Java-ontwikkelaarscentrum] en de [Java Tools voor Visual Studio Team Services].

Zie voor meer informatie over het opstarten van de versie die voorjaar op Docker-voorbeeldproject [Spring opstarten op het Docker aan de slag].

De volgende koppelingen vindt aanvullende informatie over het maken van opstarten Spring toepassingen:

* Zie voor meer informatie over het maken van een eenvoudige Spring Boot-toepassing de Initializr Spring op https://start.spring.io/.

De volgende koppelingen vindt u meer informatie over het gebruik van Kubernetes met Azure:

* [Aan de slag met een cluster Kubernetes in Container Service](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough)
* [Met behulp van de webgebruikersinterface Kubernetes met Azure Container Service](https://docs.microsoft.com/azure/container-service/container-service-kubernetes-ui)

Meer informatie over het gebruik van de opdrachtregelinterface Kubernetes is beschikbaar in de **kubectl** gebruikershandleiding op <https://kubernetes.io/docs/user-guide/kubectl/>.

De website Kubernetes heeft verschillende artikelen waarin met afbeeldingen in persoonlijke registers worden besproken:

* [Configureren van Service-Accounts voor het gehele product]
* [Naamruimten]
* [Binnenhalen van een installatiekopie van een persoonlijke register]

Zie voor meer voorbeelden voor het aangepaste Docker-installatiekopieën gebruiken met Azure [met een aangepaste Docker-installatiekopie voor Azure-Web-App op Linux].

<!-- URL List -->

[Azure-opdrachtregelinterface (CLI)]: /cli/azure/overview
[ Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java-ontwikkelaarscentrum]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[met een aangepaste Docker-installatiekopie voor Azure-Web-App op Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[gratis Azure-account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools voor Visual Studio Team Services]: https://java.visualstudio.com/
[Kubernetes]: https://kubernetes.io/
[Kubernetes Command-Line Interface (kubectl)]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[Maven]: http://maven.apache.org/
[voordelen als MSDN-abonnee]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring opstarten op het Docker aan de slag]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[Configureren van Service-Accounts voor het gehele product]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/
[Naamruimten]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[Binnenhalen van een installatiekopie van een persoonlijke register]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/AR04.png

[KB01]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB01.png
[KB02]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB02.png
[KB03]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB03.png
[KB04]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB04.png
[KB05]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB05.png
[KB06]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB06.png
[KB07]: ./media/container-service-deploy-spring-boot-app-on-kubernetes/KB07.png
