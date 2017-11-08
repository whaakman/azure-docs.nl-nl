---
title: Een versie die voorjaar opstarten Web-App op Linux in Azure Containerservice implementeren | Microsoft Docs
description: Deze zelfstudie wordt u echter de stappen voor het implementeren van een toepassing Spring opstarten als een Linux-web-app in Microsoft Azure.
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
ms.openlocfilehash: fd071dc8ce568602db3d96d4bcaa5f421bd16513
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="deploy-a-spring-boot-application-on-linux-in-the-azure-container-service"></a>Een toepassing Spring opstarten op Linux in de Azure Container Service implementeren

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

De  **[Spring Framework]**  is een open source-oplossing waarmee ontwikkelaars van Java op bedrijfsniveau toepassingen maken. Een van de meer populaire projecten die is gebaseerd op deze platform is [Spring Boot], waarmee u een vereenvoudigde benadering voor het maken van zelfstandige Java-toepassingen.

**[Docker]**  is open source-oplossingen waarmee ontwikkelaars automatiseren implementatie, schaalbaarheid en beheer van hun toepassingen die worden uitgevoerd in containers.

Deze zelfstudie leert u met behulp van Docker te ontwikkelen en een versie die voorjaar opstarten toepassing implementeert naar een Linux-host in de [Azure Container Service (ACS)].

## <a name="prerequisites"></a>Vereisten

U moet de stappen in deze zelfstudie hebt voltooid, hebt de volgende vereisten:

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

De volgende stappen maakt u de stappen die nodig zijn voor het maken van een eenvoudige webtoepassing voor Spring opstarten en lokaal testen.

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

1. Kloon de [Spring opstarten op het Docker aan de slag] voorbeeldproject naar de map die u hebt gemaakt, bijvoorbeeld:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Wijzig de directory aan het project voltooide; bijvoorbeeld:
   ```
   cd gs-spring-boot-docker/complete
   ```

1. Maken van de JAR-bestand met behulp van Maven; bijvoorbeeld:
   ```
   mvn package
   ```

1. Wanneer de web-app is gemaakt, wijzig directory in de `target` directory waar het JAR-bestand zich bevindt en start de web-app; bijvoorbeeld:
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. De web-app testen door te bladeren naar lokaal met behulp van een webbrowser. Bijvoorbeeld, als u curl beschikbaar en u geconfigureerd de Tomcat-server worden uitgevoerd op poort 80:
   ```
   curl http://localhost
   ```

1. U ziet het volgende bericht weergegeven: **Docker HelloWorld!**

   ![Lokaal voorbeeld-App bladeren][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>Maken van een Azure Container register om te gebruiken als een persoonlijke Docker-register

De volgende stappen maakt u via de Azure portal gebruiken voor het maken van een Azure Container-register.

> [!NOTE]
>
> Als u de Azure CLI gebruiken in plaats van de Azure-portal wilt, volg de stappen in [maken van een persoonlijke Docker-container register met behulp van de Azure CLI 2.0](../../container-registry/container-registry-get-started-azure-cli.md).
>

1. Blader naar de [Azure-portal] en meld u aan.

   Zodra u zich hebt geregistreerd bij uw account in de Azure portal, kunt u de stappen in de [maken van een persoonlijke Docker-container register met de Azure portal] artikel, dat in de volgende stappen voor het sake van zijn paraphrased proces.

1. Klik op het menupictogram voor **+ nieuw**, klikt u vervolgens op **Containers**, en klik vervolgens op **Azure Container register**.
   
   ![Maak een nieuwe Azure-Container register][AR01]

1. Wanneer de pagina gegevens voor de sjabloon Azure Container register wordt weergegeven, klikt u op **maken**. 

   ![Maak een nieuwe Azure-Container register][AR02]

1. Wanneer de **maken container register** pagina wordt weergegeven, voert u uw **naam van Routeringsregister** en **resourcegroep**, kies **inschakelen** voor de **Gebruiker met beheerdersrechten**, en klik vervolgens op **maken**.

   ![Azure-Container registerinstellingen configureren][AR03]

1. Zodra het register van de container is gemaakt, gaat u naar het register van de container in Azure portal en klik vervolgens op **toegangstoetsen**. Noteer de gebruikersnaam en het wachtwoord voor de volgende stappen.

   ![Azure Container toegang registersleutels][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a>Maven voor het gebruik van de toegangssleutels van uw Azure-Container register configureren

1. Ga naar de configuratiemap voor uw installatie Maven en open de *settings.xml* bestand met een teksteditor.

1. Uw toegang tot Azure Container registerinstellingen toevoegen uit het vorige gedeelte van deze zelfstudie voor het `<servers>` verzameling in de *settings.xml* bestand; bijvoorbeeld:

   ```xml
   <settings>
       <pluginGroups>
           <pluginGroup>com.spotify</pluginGroup>
       </pluginGroups>
   </settings>
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
         <configuration>
            <email>foo@foo.bar</email>
         </configuration>
      </server>
   </servers>
   ```

1. Ga naar de map voltooid project voor uw toepassing Spring opstarten (bijvoorbeeld: '*C:\SpringBoot\gs-spring-boot-docker\complete*"of"*/users/robert/SpringBoot/gs-spring-boot-docker/complete* '), en open de *pom.xml* bestand met een teksteditor.

1. Update de `<properties>` verzameling in de *pom.xml* -bestand met de waarde van de server aanmelding voor uw Azure-Container registersleutel uit het vorige gedeelte van deze zelfstudie; bijvoorbeeld:

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Update de `<plugins>` verzameling in de *pom.xml* bestand zodat de `<plugin>` bevat van de server en het register aanmeldingsnaam voor uw Azure-Container registersleutel uit het vorige gedeelte van deze zelfstudie. Bijvoorbeeld:

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>${project.basedir}</dockerDirectory>
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

1. Ga naar de map voltooid project voor uw toepassing Spring opstarten en voer de volgende opdracht om de toepassing bouwen en de container push naar uw Azure-Container register:

   ```
   mvn package docker:build -DpushImage 
   ```

> [!NOTE]
>
> Wanneer u uw Docker-container worden gepusht naar Azure, verschijnt een foutbericht weergegeven dat vergelijkbaar is met een van de volgende zelfs al uw Docker-container is gemaakt:
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> Als dit gebeurt, moet u mogelijk aan te melden bij uw Azure-account vanaf de opdrachtregel Docker; bijvoorbeeld:
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> U kunt vervolgens de container push vanaf de opdrachtregel; bijvoorbeeld:
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a>Een web-app maken in Linux op Azure App Service met behulp van de installatiekopie van de container

1. Blader naar de [Azure-portal] en meld u aan.

1. Klik op het menupictogram voor **+ nieuw**, klikt u vervolgens op **Web en mobiel**, en klik vervolgens op **Web-App op Linux**.
   
   ![Een nieuwe WebApp maken in de Azure portal][LX01]

1. Wanneer de **Web-App op Linux** pagina wordt weergegeven, voert u de volgende informatie:

   a. Geef een unieke naam voor de **appnaam**; bijvoorbeeld: '*wingtiptoyslinux*. "

   b. Kies uw **abonnement** uit de vervolgkeuzelijst.

   c. Kies een bestaande **resourcegroep**, of geef een naam om een nieuwe resourcegroep te maken.

   d. Klik op **configureren container** en voer de volgende informatie:

      * Kies **persoonlijke register**.

      * **Installatiekopie en optionele tag**: Geef de containernaam van uw van eerder; bijvoorbeeld: '*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*'

      * **Server-URL**: Geef de URL van het register van eerder; bijvoorbeeld: '*https://wingtiptoysregistry.azurecr.io*'

      * **Gebruikersnaam voor aanmelding** en **wachtwoord**: Geef de referenties van uw **toegangstoetsen** die u gebruikt in de vorige stappen.
   
   e. Nadat u alle bovenstaande gegevens hebt ingevoerd, klikt u op **OK**.

   ![Instellingen voor web-app configureren][LX02]

1. Klik op **Create**.

> [!NOTE]
>
> Azure internetaanvragen automatisch toegewezen aan ingesloten Tomcat-server die wordt uitgevoerd op de standaard poorten 80 of 8080. Als uw ingesloten Tomcat-server worden uitgevoerd op een aangepaste poort is geconfigureerd, moet u een omgevingsvariabele toevoegen aan uw web-app die de poort voor de ingesloten Tomcat-server definieert. Volg hiervoor de volgende stappen:
>
> 1. Blader naar de [Azure-portal] en meld u aan.
> 
> 2. Klik op het pictogram voor **App Services**. (Zie artikel #1 in de onderstaande afbeelding.)
>
> 3. Selecteer de web-app in de lijst. (Item #2 in de onderstaande afbeelding.)
>
> 4. Klik op **toepassingsinstellingen**. (Item #3 in de onderstaande afbeelding.)
>
> 5. In de **appinstellingen** sectie, het toevoegen van een nieuwe omgevingsvariabele met de naam **poort** en voer het nummer van uw aangepaste poort voor de waarde. (Item #4 in de onderstaande afbeelding.)
>
> 6. Klik op **Opslaan**. (Item #5 in de onderstaande afbeelding.)
>
> ![Een aangepast poortnummer opslaan in de Azure portal][LX03]
>

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over het gebruik van opstarten Spring toepassingen in Azure:

* [Een toepassing Spring opstart in de Azure App Service implementeren](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Een toepassing Spring opstarten op een Cluster Kubernetes in de Azure Containerservice implementeren](container-service-deploy-spring-boot-app-on-kubernetes.md)

Voor meer informatie over het gebruik van Azure met Java raadpleegt u het [Azure Java-ontwikkelaarscentrum] en de [Java Tools voor Visual Studio Team Services].

Zie voor meer informatie over het opstarten van de versie die voorjaar op Docker-voorbeeldproject [Spring opstarten op het Docker aan de slag].

Zie voor meer informatie over aan de slag met uw eigen toepassingen Spring opstartinstallatiekopie, de **Spring Initializr** op https://start.spring.io/.

Zie voor meer informatie over aan de slag met het maken van een eenvoudige Spring Boot-toepassing, de versie die voorjaar Initializr op https://start.spring.io/.

Zie voor meer voorbeelden voor het aangepaste Docker-installatiekopieën gebruiken met Azure [met een aangepaste Docker-installatiekopie voor Azure-Web-App op Linux].

<!-- URL List -->

[Azure-opdrachtregelinterface (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java-ontwikkelaarscentrum]: https://azure.microsoft.com/develop/java/
[Azure-portal]: https://portal.azure.com/
[maken van een persoonlijke Docker-container register met de Azure portal]: /azure/container-registry/container-registry-get-started-portal
[met een aangepaste Docker-installatiekopie voor Azure-Web-App op Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[gratis Azure-account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools voor Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[voordelen als MSDN-abonnee]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring opstarten op het Docker aan de slag]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-linux/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-linux/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-linux/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-linux/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-linux/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-linux/AR04.png

[LX01]: ./media/container-service-deploy-spring-boot-app-on-linux/LX01.png
[LX02]: ./media/container-service-deploy-spring-boot-app-on-linux/LX02.png
[LX03]: ./media/container-service-deploy-spring-boot-app-on-linux/LX03.png
