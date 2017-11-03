---
title: Het gebruik van de Maven-invoegtoepassing voor Azure-Web-Apps voor een versie die voorjaar Boot-app implementeren in Azure
description: Informatie over het gebruik van de Maven-invoegtoepassing voor Azure-Web-Apps voor een versie die voorjaar Boot-app implementeren in Azure.
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
ms.openlocfilehash: f22089349e149d8e71df88d5c173869d2a5abad7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-to-azure"></a>Het gebruik van de Maven-invoegtoepassing voor Azure-Web-Apps voor een versie die voorjaar Boot-app implementeren in Azure

De [Maven-invoegtoepassing voor Azure-Web-Apps](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) voor [Apache Maven](http://maven.apache.org/) biedt naadloze integratie van Azure App Service met Maven-projecten en stroomlijnt het proces voor ontwikkelaars voor web-apps implementeren in Azure-App De service.

In dit artikel wordt gedemonstreerd met behulp van de Maven-invoegtoepassing voor Azure-Web-Apps voor het implementeren van een voorbeeldtoepassing Spring opstarten naar Azure App Services.

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

## <a name="clone-the-sample-spring-boot-web-app"></a>Klonen van de steekproef Spring opstarten web-app

In dit gedeelte klonen van een voltooide Spring Boot-toepassing en lokaal testen.

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

1. Kloon de [Spring opstarten aan de slag] voorbeeldproject naar de map die u hebt gemaakt, bijvoorbeeld:
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot
   ```

1. Wijzig de directory aan het project voltooide; bijvoorbeeld:
   ```shell
   cd gs-spring-boot/complete
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

1. U ziet het volgende bericht weergegeven: **begroetingen vanaf Spring opstart!**

## <a name="create-an-azure-service-principal"></a>Een Azure-service-principal maken

In deze sectie maakt u een Azure service-principal die de invoegtoepassing Maven gebruikt bij het implementeren van uw web-app in Azure.

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
   > U gebruikt de waarden van deze JSON-antwoord bij het configureren van de Maven-invoegtoepassing voor het implementeren van uw web-app in Azure. De `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, en `tttttttt` tijdelijke aanduiding voor waarden die in dit voorbeeld worden gebruikt om het gemakkelijker deze waarden worden toegewezen aan hun respectieve elementen bij het configureren van uw Maven `settings.xml` bestand in de volgende sectie .
   >
   >

## <a name="configure-maven-to-use-your-azure-service-principal"></a>Maven voor het gebruik van uw Azure-service-principal configureren

In deze sectie gebruikt u de waarden van uw Azure service-principal voor het configureren van de verificatie met behulp van Maven bij het implementeren van uw web-app in Azure.

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
   `<environment>` | Definieert de doel-Azure-cloud-omgeving, die `AZURE` in dit voorbeeld. (Een volledige lijst met omgevingen is beschikbaar in de [Maven-invoegtoepassing voor Azure-Web-Apps] documentatie)

1. Sla op en sluit de *settings.xml* bestand.

## <a name="optional-customize-your-pomxml-before-deploying-your-web-app-to-azure"></a>Optioneel: Uw pom.xml aanpassen voordat u uw web-app implementeren naar Azure

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
         <appName>maven-web-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <javaVersion>1.8</javaVersion>
         <deploymentType>ftp</deploymentType>
         <resources>
            <resource>
               <directory>${project.basedir}/target</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>*.jar</include>
               </includes>
            </resource>
            <resource>
               <directory>${project.basedir}</directory>
               <targetPath>/</targetPath>
               <includes>
                  <include>web.config</include>
               </includes>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```

Er zijn verschillende waarden die u voor de invoegtoepassing Maven wijzigen kunt en een gedetailleerde beschrijving in voor elk van deze elementen is beschikbaar in de [Maven-invoegtoepassing voor Azure-Web-Apps] documentatie. Die wordt aangegeven, er zijn verschillende waarden die waard markeren in dit artikel zijn:

Element | Beschrijving
---|---|---
`<version>` | Hiermee geeft u de versie van de [Maven-invoegtoepassing voor Azure-Web-Apps]. Controleer de versie vermeld in de [Maven centrale opslagplaats](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) om ervoor te zorgen dat u van de meest recente versie gebruikmaakt.
`<authentication>` | Hiermee geeft u de verificatie-informatie voor Azure, die in dit voorbeeld bevat een `<serverId>` element met `azure-auth`; Maven gebruikt dat de waarde voor het opzoeken van de Azure service-principal waarden in uw Maven *settings.xml* bestand, dat u hebt gedefinieerd in een eerdere sectie van dit artikel.
`<resourceGroup>` | Hiermee geeft u de doelresourcegroep is `maven-plugin` in dit voorbeeld. De resourcegroep wordt gemaakt tijdens de implementatie als deze niet al bestaat.
`<appName>` | Hiermee geeft u de naam van het doel voor uw web-app. In dit voorbeeld wordt de naam van het doel is `maven-web-app-${maven.build.timestamp}`, waarbij de `${maven.build.timestamp}` achtervoegsel wordt toegevoegd in dit voorbeeld om conflicten te voorkomen. (De tijdstempel is optioneel, kunt u een unieke tekenreeks voor de naam van de app.)
`<region>` | Hiermee geeft u het de doelregio die in dit voorbeeld is `westus`. (Een volledige lijst is in de [Maven-invoegtoepassing voor Azure-Web-Apps] documentatie.)
`<javaVersion>` | Hiermee geeft u de Java runtime-versie voor uw web-app. (Een volledige lijst is in de [Maven-invoegtoepassing voor Azure-Web-Apps] documentatie.)
`<deploymentType>` | Hiermee geeft u een implementatietype voor uw web-app. Op dit moment alleen `ftp` wordt ondersteund, maar ondersteuning voor andere implementatietypen in ontwikkeling is.
`<resources>` | Hiermee geeft u resources en doel-doelen die Maven gebruikt bij het implementeren van uw web-app in Azure. In dit voorbeeld twee `<resource>` elementen opgeven dat Maven het JAR-bestand voor uw web-app implementeert en de *web.config* bestand van het project Spring opstarten.

## <a name="build-and-deploy-your-web-app-to-azure"></a>Bouwen en implementeren van uw web-app in Azure

Zodra u alle instellingen in de voorgaande secties in dit artikel hebt geconfigureerd, bent u klaar voor het implementeren van uw web-app in Azure. Volg hiervoor de volgende stappen:

1. Opnieuw vanaf de opdrachtprompt of terminalvenster die u eerder hebt gebruikt, het JAR-bestand met behulp van Maven, als u wijzigingen aangebracht in de *pom.xml* bestand; bijvoorbeeld:
   ```shell
   mvn clean package
   ```

1. Uw web-app implementeren in Azure met behulp van Maven; bijvoorbeeld:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven implementeert uw web-app naar Azure. Als de web-app niet al bestaat, wordt deze gemaakt.

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

* [Maven-invoegtoepassing voor Azure-Web-Apps]

* [Meld u aan bij Azure met Azure CLI](/azure/xplat-cli-connect)

* [Het gebruik van de Maven-invoegtoepassing voor Azure-Web-Apps voor een beperkte Spring Boot-app implementeren in Azure](containers/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin.md)

* [Een Azure-service-principal maken met Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Naslaginformatie over maven](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure-opdrachtregelinterface (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure-portal]: https://portal.azure.com/
[gratis Azure-account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[voordelen als MSDN-abonnee]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring opstarten aan de slag]: https://github.com/microsoft/gs-spring-boot
[Spring Framework]: https://spring.io/
[Maven-invoegtoepassing voor Azure-Web-Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-with-maven-plugin/AP02.png
