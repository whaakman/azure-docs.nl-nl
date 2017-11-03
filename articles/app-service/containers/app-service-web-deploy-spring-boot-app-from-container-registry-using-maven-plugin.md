---
title: Het gebruik van de Maven-invoegtoepassing voor Azure-Web-Apps een Spring Boot-app in Azure Container register implementeren in Azure App Service
description: Deze zelfstudie leert u echter de stappen voor het implementeren van een toepassing Spring opstarten in het register van Azure-Container in Azure-Azure App Service met behulp van een Maven-invoegtoepassing.
services: 
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: b087003b3a1e236e4a306678904107b8bf99395e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-in-azure-container-registry-to-azure-app-service"></a>Het gebruik van de Maven-invoegtoepassing voor Azure-Web-Apps een Spring Boot-app in Azure Container register implementeren in Azure App Service

De  **[Spring Framework]**  is een populaire open-source framework waarmee ontwikkelaars van Java web, mobiel en API-toepassingen maken. In deze zelfstudie wordt een voorbeeld-app gemaakt met behulp van [Spring Boot], een conventie voor gestuurde benadering voor het gebruik van Spring snel aan de slag.

In dit artikel laat zien hoe een voorbeeldtoepassing Spring opstarten in Azure Container register implementeren en vervolgens de invoegtoepassing Maven voor Azure-Web-Apps gebruiken om uw toepassing in Azure App Service te implementeren.

> [!NOTE]
>
> De Maven-invoegtoepassing voor Azure-Web-Apps is momenteel beschikbaar als een voorbeeld. Alleen FTP-publicatie wordt nu ondersteund, maar aanvullende functies zijn gepland voor de toekomst.
>

## <a name="prerequisites"></a>Vereisten

U moet de stappen in deze zelfstudie hebt voltooid, hebt de volgende vereisten:

* Een Azure-abonnement; Als u nog geen Azure-abonnement hebt, kunt u activeren uw [voordelen als MSDN-abonnee] of zich aanmelden voor een [gratis Azure-account].
* De [Azure-opdrachtregelinterface (CLI)].
* Een up-to-date [Java Development Kit (JDK)], versie 1.7 of hoger.
* Apache van [Maven] bouwen tool (versie 3).
* Een [Git] client.
* Een [Docker] client.

> [!NOTE]
>
> Vanwege de virtualisatie-vereisten van deze zelfstudie, kan niet u de stappen in dit artikel volgen op een virtuele machine; u moet een fysieke computer gebruiken met virtualisatiefuncties ingeschakeld.
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a>Klonen van de steekproef Spring opstarten op Docker-web-app

In dit gedeelte klonen van een beperkte Spring Boot-toepassing en lokaal testen.

1. Open een opdrachtprompt of een terminalvenster en maak een lokale map houdt uw toepassing Spring opstarten en wijzigen in die map; bijvoorbeeld:
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- of--
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Kloon de [Spring opstarten op het Docker aan de slag] voorbeeldproject naar de map die u hebt gemaakt, bijvoorbeeld:
   ```shell
   git clone -b private-registry https://github.com/Microsoft/gs-spring-boot-docker
   ```

1. Wijzig de directory aan het project voltooide; bijvoorbeeld:
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. Maken van de JAR-bestand met behulp van Maven; bijvoorbeeld:
   ```shell
   mvn clean package
   ```

1. Wanneer de web-app is gemaakt, start u de web-app met behulp van Maven; bijvoorbeeld:
   ```shell
   mvn spring-boot:run
   ```

1. De web-app testen door te bladeren naar lokaal met behulp van een webbrowser. U kan bijvoorbeeld de volgende opdracht gebruiken, hebt u curl beschikbaar:
   ```shell
   curl http://localhost:8080
   ```

1. U ziet het volgende bericht weergegeven: **Docker HelloWorld**

   ![Lokaal voorbeeld-App bladeren][SB01]

## <a name="create-an-azure-service-principal"></a>Een Azure-service-principal maken

In deze sectie maakt u een Azure service-principal die de invoegtoepassing Maven gebruikt bij het implementeren van de container in Azure.

1. Open een opdrachtprompt.

1. Meld u aan bij uw Azure-account met behulp van de Azure CLI:
   ```azurecli
   az login
   ```
   Volg de instructies om het proces aanmelden te voltooien.

1. Een Azure-service-principal maken:
   ```azurecli
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   Waar `uuuuuuuu` is de gebruikersnaam en `pppppppp` is het wachtwoord voor de service-principal.

1. Azure reageert met JSON die lijkt op het volgende voorbeeld:
   ```json
   {
      "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "displayName": "uuuuuuuu",
      "name": "http://uuuuuuuu",
      "password": "pppppppp",
      "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

   > [!NOTE]
   >
   > U gebruikt de waarden van deze JSON-antwoord bij het configureren van de Maven-invoegtoepassing voor het implementeren van de container in Azure. De `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, en `tttttttt` tijdelijke aanduiding voor waarden die in dit voorbeeld worden gebruikt om het gemakkelijker deze waarden worden toegewezen aan hun respectieve elementen bij het configureren van uw Maven `settings.xml` bestand in de volgende sectie .
   >
   >

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>Maken van een Azure Container register met de Azure CLI

1. Open een opdrachtprompt.

1. Aanmelden bij uw Azure-account:
   ```azurecli
   az login
   ```

1. Maak een resourcegroep voor de Azure-resources die gaat u in dit artikel:
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   Vervang `wingtiptoysresources` in dit voorbeeld met een unieke naam voor de resourcegroep.

1. Maak een register persoonlijke Azure-container in de resourcegroep voor uw app Spring opstarten: 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   Vervang `wingtiptoysregistry` in dit voorbeeld met een unieke naam voor de container-register.

1. Het wachtwoord voor de container-register worden opgehaald:
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   Azure reageert met je wachtwoord; bijvoorbeeld:
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-to-your-maven-settings"></a>Uw Azure-container register en de Azure service-principal toevoegen aan uw Maven-instellingen

1. Open uw Maven `settings.xml` bestand in een teksteditor; dit bestand is mogelijk in een pad, zoals de volgende voorbeelden:
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. Uw toegang tot Azure Container registerinstellingen toevoegen uit de vorige sectie van dit artikel om de `<servers>` verzameling in de *settings.xml* bestand; bijvoorbeeld:

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   Waar:
   Element | Beschrijving
   ---|---|---
   `<id>` | Bevat de naam van het register persoonlijke Azure-container.
   `<username>` | Bevat de naam van het register persoonlijke Azure-container.
   `<password>` | Het wachtwoord die u hebt opgehaald in de vorige sectie van dit artikel bevat.

1. Toevoegen van uw Azure-service-principal-instellingen in een eerder gedeelte van dit artikel voor de `<servers>` verzameling in de *settings.xml* bestand; bijvoorbeeld:

   ```xml
   <servers>
      <server>
        <id>azure-auth</id>
         <configuration>
            <client>aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa</client>
            <tenant>tttttttt-tttt-tttt-tttt-tttttttttttt</tenant>
            <key>pppppppp</key>
            <environment>AZURE</environment>
         </configuration>
      </server>
   </servers>
   ```
   Waar:
   Element | Beschrijving
   ---|---|---
   `<id>` | Hiermee geeft u een unieke naam voor Maven gebruikt om te zoeken uw beveiligingsinstellingen wanneer u uw web-app in Azure implementeert.
   `<client>` | Bevat de `appId` waarde van uw service-principal.
   `<tenant>` | Bevat de `tenant` waarde van uw service-principal.
   `<key>` | Bevat de `password` waarde van uw service-principal.
   `<environment>` | Definieert de doel-Azure-cloud-omgeving, die `AZURE` in dit voorbeeld. (Een volledige lijst met omgevingen is beschikbaar in de [Maven-invoegtoepassing voor Azure-Web-Apps] documentatie)

1. Sla op en sluit de *settings.xml* bestand.

## <a name="build-your-docker-container-image-and-push-it-to-your-azure-container-registry"></a>Uw Docker-container-installatiekopie maken en dit doorgeven aan uw Azure-container-register

1. Ga naar de map voltooid project voor uw toepassing Spring opstarten (bijvoorbeeld) "*C:\SpringBoot\gs-spring-boot-docker\complete*"of"*/users/robert/SpringBoot/gs-spring-boot-docker/complete*'), en open de *pom.xml* bestand met een tekst Editor.

1. Update de `<properties>` verzameling in de *pom.xml* -bestand met de waarde van de server aanmelding voor uw Azure-Container registersleutel uit het vorige gedeelte van deze zelfstudie; bijvoorbeeld:

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   Waar:
   Element | Beschrijving
   ---|---|---
   `<azure.containerRegistry>` | Hiermee geeft u de naam van het register persoonlijke Azure-container.
   `<docker.image.prefix>` | Hiermee geeft u de URL van het register persoonlijke Azure-container, die is afgeleid door toe te voegen '. azurecr.io ' op de naam van het register privé-container.

1. Controleer `<plugin>` voor de Docker-invoegtoepassing in uw *pom.xml* -bestand bevat de juiste eigenschappen voor de server en het register aanmeldingsnaam van de vorige stap in deze zelfstudie. Bijvoorbeeld:

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <registryUrl>https://${docker.image.prefix}</registryUrl>
         <serverId>${azure.containerRegistry}</serverId>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```
   Waar:
   Element | Beschrijving
   ---|---|---
   `<serverId>` | Hiermee geeft u de eigenschap met de naam van het register persoonlijke Azure-container.
   `<registryUrl>` | Hiermee geeft u de eigenschap waarin de URL van uw persoonlijke Azure-container-register.

1. Ga naar de map voltooid project voor uw toepassing Spring opstarten en voer de volgende opdracht om de toepassing bouwen en de container push naar uw Azure-container register:

   ```
   mvn package docker:build -DpushImage 
   ```

1. Optioneel: Blader naar de [Azure-portal] en controleer of er een installatiekopie van Docker-container met de naam **gs-spring-boot-docker** in het register van de container.

   ![Controleer of de container in Azure-portal][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-to-azure"></a>Uw pom.xml aanpassen en vervolgens te bouwen en implementeren van de container in Azure

Open de `pom.xml` voor uw toepassing Spring opstarten in een teksteditor en zoek vervolgens de `<plugin>` element voor `azure-webapp-maven-plugin`. Dit element moet eruitzien als in het volgende voorbeeld:

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>wingtiptoysresources</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
            <registryUrl>https://${docker.image.prefix}</registryUrl>
            <serverId>${azure.containerRegistry}</serverId>
         </containerSettings>
         <appSettings>
            <property>
               <name>PORT</name>
               <value>8080</value>
            </property>
         </appSettings>
      </configuration>
   </plugin>
   ```

Er zijn verschillende waarden die u voor de invoegtoepassing Maven wijzigen kunt en een gedetailleerde beschrijving in voor elk van deze elementen is beschikbaar in de [Maven-invoegtoepassing voor Azure-Web-Apps] documentatie. Die wordt aangegeven, er zijn verschillende waarden die waard markeren in dit artikel zijn:

Element | Beschrijving
---|---|---
`<version>` | Hiermee geeft u de versie van de [Maven-invoegtoepassing voor Azure-Web-Apps]. Controleer de versie vermeld in de [Maven centrale opslagplaats](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) om ervoor te zorgen dat u van de meest recente versie gebruikmaakt.
`<authentication>` | Hiermee geeft u de verificatie-informatie voor Azure, die in dit voorbeeld bevat een `<serverId>` element met `azure-auth`; Maven gebruikt dat de waarde voor het opzoeken van de Azure service-principal waarden in uw Maven *settings.xml* bestand, dat u hebt gedefinieerd in een eerdere sectie van dit artikel.
`<resourceGroup>` | Hiermee geeft u de doelresourcegroep is `wingtiptoysresources` in dit voorbeeld. De resourcegroep wordt gemaakt tijdens de implementatie als deze niet al bestaat.
`<appName>` | Hiermee geeft u de naam van het doel voor uw web-app. In dit voorbeeld wordt de naam van het doel is `maven-linux-app-${maven.build.timestamp}`, waarbij de `${maven.build.timestamp}` achtervoegsel wordt toegevoegd in dit voorbeeld om conflicten te voorkomen. (De tijdstempel is optioneel, kunt u een unieke tekenreeks voor de naam van de app.)
`<region>` | Hiermee geeft u het de doelregio die in dit voorbeeld is `westus`. (Een volledige lijst is in de [Maven-invoegtoepassing voor Azure-Web-Apps] documentatie.)
`<containerSettings>` | Hiermee geeft u de eigenschappen met de naam en de URL van de container.
`<appSettings>` | Hiermee geeft u een unieke instellingen voor Maven te gebruiken bij het implementeren van uw web-app in Azure. In dit voorbeeld wordt een `<property>` element bevat een naam/waarde-paar van onderliggende elementen die de poort voor uw app opgeven.

> [!NOTE]
>
> De instellingen te wijzigen van het poortnummer in dit voorbeeld zijn alleen nodig als u bij het wijzigen van de poort van de standaardwaarde.
>

1. Opnieuw vanaf de opdrachtprompt of terminalvenster die u eerder hebt gebruikt, het JAR-bestand met behulp van Maven, als u wijzigingen aangebracht in de *pom.xml* bestand; bijvoorbeeld:
   ```shell
   mvn clean package
   ```

1. Uw web-app implementeren in Azure met behulp van Maven; bijvoorbeeld:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven implementeert uw web-app naar Azure. Als de web-app niet al bestaat, wordt deze gemaakt.

> [!NOTE]
>
> Als de regio die u opgeeft in de `<region>` element van uw *pom.xml* bestand beschikt niet over voldoende beschikbare servers wanneer u de implementatie start, ziet u mogelijk een foutbericht weergegeven dat vergelijkbaar is met het volgende voorbeeld:
>
> ```
> [INFO] Start deploying to Web App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed to execute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> Als dit gebeurt, kunt u een andere regio opgeven en de Maven-opdracht voor het implementeren van uw toepassing opnieuw uitvoeren.
>
>

Wanneer uw web-is geïmplementeerd, kunt u zich kunt beheren met de [Azure-portal].

* Uw web-app wordt weergegeven in **App Services**:

   ![Web-app die worden vermeld in Azure-portal App Services][AP01]

* En de URL voor uw web-app wordt weergegeven in de **overzicht** voor uw web-app:

   ![Bepalen van de URL voor uw web-app][AP02]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de verschillende technologieën die in dit artikel wordt besproken, de volgende artikelen:

* [Maven-invoegtoepassing voor Azure-Web-Apps]

* [Meld u aan bij Azure met Azure CLI](/azure/xplat-cli-connect)

* [Een Azure-service-principal maken met Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Naslaginformatie over maven](https://maven.apache.org/settings.html)

* [Maven docker-invoegtoepassing]

<!-- URL List -->

[Azure-opdrachtregelinterface (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portal]: https://portal.azure.com/
[Maven-invoegtoepassing voor Azure-Web-Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: tutorial-custom-docker-image.md
[Docker]: https://www.docker.com/
[Maven docker-invoegtoepassing]: https://github.com/spotify/docker-maven-plugin
[gratis Azure-account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[voordelen als MSDN-abonnee]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring opstarten op het Docker aan de slag]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP02.png
