---
title: Het gebruik van de invoegtoepassing Maven voor Web-App voor Containers voor een beperkte Spring Boot-app implementeren in Azure
description: Informatie over het gebruik van de Maven-invoegtoepassing voor Web-App voor Containers naar een versie die voorjaar Boot-app implementeren in Azure.
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: 0329aa9b88c7542ab3235a104a0652cd217ff872
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="how-to-use-the-maven-plugin-for-web-app-for-containers-to-deploy-a-containerized-spring-boot-app-to-azure"></a>Het gebruik van de invoegtoepassing Maven voor Web-App voor Containers voor een beperkte Spring Boot-app implementeren in Azure

De [Maven-invoegtoepassing voor Web-App voor Containers](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) voor [Apache Maven](http://maven.apache.org/) biedt naadloze integratie van Azure App Service met Maven-projecten en stroomlijnt het proces voor ontwikkelaars voor web-apps te implementeren Azure App Service.

In dit artikel wordt gedemonstreerd met behulp van de invoegtoepassing Maven voor Web-App voor Containers voor het implementeren van een voorbeeldtoepassing Spring opstarten in een Docker-container in Web-App voor Containers.

> [!NOTE]
>
> De Maven-invoegtoepassing voor Web-App voor Containers is momenteel beschikbaar als een voorbeeld. Alleen FTP-publicatie wordt nu ondersteund, maar aanvullende functies zijn gepland voor de toekomst.
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

## <a name="clone-the-sample-spring-boot-application"></a>Klonen van de voorbeeldtoepassing Spring opstarten

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

1. Klonen van het voorbeeldproject [Spring Boot met Docker] naar de map die u hebt gemaakt; bijvoorbeeld:
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot-docker
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

## <a name="create-an-azure-service-principal"></a>Een Azure-service-principal maken

In deze sectie maakt u een Azure service-principal die de invoegtoepassing Maven gebruikt bij het implementeren van de container in Azure.

1. Open een opdrachtprompt.

1. Meld u aan bij uw Azure-account met behulp van de Azure CLI:
   ```shell
   az login
   ```
   Volg de instructies om het proces aanmelden te voltooien.

1. Een Azure-service-principal maken:
   ```shell
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

## <a name="configure-maven-to-use-your-azure-service-principal"></a>Maven voor het gebruik van uw Azure-service-principal configureren

In deze sectie gebruikt u de waarden van uw Azure service-principal voor het configureren van de verificatie die Maven wordt gebruikt bij het implementeren van de container in Azure.

1. Open uw Maven `settings.xml` bestand in een teksteditor; dit bestand is mogelijk in een pad, zoals de volgende voorbeelden:
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. Toevoegen van uw Azure-service-principal instellingen uit het vorige gedeelte van deze zelfstudie voor het `<servers>` verzameling in de *settings.xml* bestand; bijvoorbeeld:

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
   `<environment>` | Definieert de doel-Azure-cloud-omgeving, die `AZURE` in dit voorbeeld. (Een volledige lijst met omgevingen is beschikbaar in de [Maven-invoegtoepassing voor Web-App voor Containers] documentatie)

1. Sla op en sluit de *settings.xml* bestand.

## <a name="optional-deploy-your-local-docker-file-to-docker-hub"></a>Optioneel: Implementeer uw lokale Docker-bestand naar de Docker-Hub

Als u een Docker-account hebt, kunt u uw Docker-container image lokaal bouwen en dit doorgeven aan de Docker-Hub. Gebruik de volgende stappen uit om dit te doen.

1. Open de `pom.xml` bestand voor uw toepassing Spring opstarten in een teksteditor.

1. Zoek de `<imageName>` onderliggend element van de `<containerSettings>` element.

1. Update de `${docker.image.prefix}` waarde met de naam van uw Docker:
   ```xml
   <containerSettings>
      <imageName>mydockeraccountname/${project.artifactId}</imageName>
   </containerSettings>
   ```

1. Kies een van de volgende implementatiemethoden gebruiken:

   * Uw installatiekopie container lokaal bouwen met Maven en vervolgens met Docker uw container push naar Docker-Hub:
      ```shell
      mvn clean package docker:build
      docker push
      ```

   * Als u hebt de [Docker-invoegtoepassing voor Maven] geïnstalleerd, kunt u automatisch maken en een installatiekopie van de container met Docker-Hub met behulp van de `-DpushImage` parameter:
      ```shell
      mvn clean package docker:build -DpushImage
      ```

## <a name="optional-customize-your-pomxml-before-deploying-your-container-to-azure"></a>Optioneel: Uw pom.xml aanpassen voordat u de container implementeren naar Azure

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
         <resourceGroup>maven-plugin</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
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

Er zijn verschillende waarden die u voor de invoegtoepassing Maven wijzigen kunt en een gedetailleerde beschrijving in voor elk van deze elementen is beschikbaar in de [Maven-invoegtoepassing voor Web-App voor Containers] documentatie. Die wordt aangegeven, er zijn verschillende waarden die waard markeren in dit artikel zijn:

Element | Beschrijving
---|---|---
`<version>` | Hiermee geeft u de versie van de [Maven-invoegtoepassing voor Web-App voor Containers]. Controleer de versie vermeld in de [Maven centrale opslagplaats](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) om ervoor te zorgen dat u van de meest recente versie gebruikmaakt.
`<authentication>` | Hiermee geeft u de verificatie-informatie voor Azure, die in dit voorbeeld bevat een `<serverId>` element met `azure-auth`; Maven gebruikt dat de waarde voor het opzoeken van de Azure service-principal waarden in uw Maven *settings.xml* bestand, dat u hebt gedefinieerd in een eerdere sectie van dit artikel.
`<resourceGroup>` | Hiermee geeft u de doelresourcegroep is `maven-plugin` in dit voorbeeld. De resourcegroep wordt gemaakt tijdens de implementatie als deze niet al bestaat.
`<appName>` | Hiermee geeft u de naam van het doel voor uw web-app. In dit voorbeeld wordt de naam van het doel is `maven-linux-app-${maven.build.timestamp}`, waarbij de `${maven.build.timestamp}` achtervoegsel wordt toegevoegd in dit voorbeeld om conflicten te voorkomen. (De tijdstempel is optioneel, kunt u een unieke tekenreeks voor de naam van de app.)
`<region>` | Hiermee geeft u het de doelregio die in dit voorbeeld is `westus`. (Een volledige lijst is in de [Maven-invoegtoepassing voor Web-App voor Containers] documentatie.)
`<appSettings>` | Hiermee geeft u een unieke instellingen voor Maven te gebruiken bij het implementeren van uw web-app in Azure. In dit voorbeeld wordt een `<property>` element bevat een naam/waarde-paar van onderliggende elementen die de poort voor uw app opgeven.

> [!NOTE]
>
> De instellingen te wijzigen van het poortnummer in dit voorbeeld zijn alleen nodig als u bij het wijzigen van de poort van de standaardwaarde.
>

## <a name="build-and-deploy-your-container-to-azure"></a>Bouwen en implementeren van de container in Azure

Zodra u alle instellingen in de voorgaande secties in dit artikel hebt geconfigureerd, bent u klaar voor het implementeren van de container in Azure. Volg hiervoor de volgende stappen:

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
> ```bash
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

Zie voor meer informatie over de verschillende technologieën die in dit artikel wordt besproken, de volgende artikelen:

* [Maven-invoegtoepassing voor Web-App voor Containers]

* [Meld u aan bij Azure met Azure CLI](/azure/xplat-cli-connect)

* [Het gebruik van de invoegtoepassing Maven voor Web-App voor Containers naar een versie die voorjaar Boot-app implementeren in Azure App Service op Linux](../app-service-web-deploy-spring-boot-app-with-maven-plugin.md)

* [Een Azure-service-principal maken met Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Naslaginformatie over maven](https://maven.apache.org/settings.html)

* [Docker-invoegtoepassing voor Maven]

<!-- URL List -->

[Azure-opdrachtregelinterface (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portal]: https://portal.azure.com/
[Docker]: https://www.docker.com/
[Docker-invoegtoepassing voor Maven]: https://github.com/spotify/docker-maven-plugin
[gratis Azure-account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[voordelen als MSDN-abonnee]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[Maven-invoegtoepassing voor Web-App voor Containers]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP02.png
