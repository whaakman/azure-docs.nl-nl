---
title: Een versie die voorjaar opstarten toepassing implementeert naar de Azure App Service | Microsoft Docs
description: Deze zelfstudie leidt ontwikkelaars door de stappen voor het implementeren van de versie die voorjaar opstarten aan de slag web-app in Azure App Service.
services: app-service\web
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
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.openlocfilehash: 8776142d5452bf5057990702c89aa1a541382ffc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-a-spring-boot-application-to-the-azure-app-service"></a>Een Spring Boot-toepassing implementeren in Azure App Service

De  **[Spring Framework]**  is een open source-oplossing waarmee ontwikkelaars van Java op bedrijfsniveau toepassingen maken en een van de meer populaire projecten die is gebaseerd op dat platform is[ Spring Boot], waarmee u een vereenvoudigde benadering voor het maken van zelfstandige Java-toepassingen.

Deze zelfstudie leert u echter het voorbeeld Spring opstarten aan de slag web-app te maken en implementeren van deze [Azure App Service].

### <a name="prerequisites"></a>Vereisten

U moet de stappen in deze zelfstudie hebt voltooid, hebt u de volgende:

* Een Azure-abonnement; Als u nog geen Azure-abonnement hebt, kunt u activeren uw [voordelen als MSDN-abonnee] of zich aanmelden voor een [gratis Azure-account].
* Een up-to-date [Java Developer Kit (JDK)].
* Apache van [Maven] bouwen tool (versie 3).
* Een [Git] client.

## <a name="create-the-spring-boot-getting-started-web-app"></a>De versie die voorjaar opstarten aan de slag web-app maken

De volgende stappen begeleidt u door de stappen die nodig zijn voor het maken van een eenvoudige webtoepassing voor Spring opstarten en lokaal testen.

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

1. Kloon de [Spring opstarten aan de slag] voorbeeldproject naar de map die u zojuist hebt gemaakt, bijvoorbeeld:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot.git
   ```

1. Wijzig de directory aan het project voltooide; bijvoorbeeld:
   ```
   cd gs-spring-boot
   cd complete
   ```

1. Maken van de JAR-bestand met behulp van Maven; bijvoorbeeld:
   ```
   mvn package
   ```

1. Wanneer de web-app is gemaakt, map wijzigen naar het JAR-bestand en de web-app starten bijvoorbeeld:
   ```
   cd target
   java -jar gs-spring-boot-0.1.0.jar
   ```

1. De web-app testen door te bladeren naar http://localhost: 8080 met een webbrowser, of gebruik de syntaxis op het volgende voorbeeld hebt u curl beschikbaar:
   ```
   curl http://localhost:8080
   ```

1. U ziet het volgende bericht weergegeven: **begroetingen vanaf Spring opstart!**

   ![Voorbeeld-App bladeren][SB01]

## <a name="create-an-azure-web-app-for-use-with-java"></a>Maken van een Azure-web-app voor gebruik met Java

De volgende stappen begeleidt u bij de stappen voor het maken van een Azure-Web-App en configureer de referenties van uw FTP-Configureer de vereiste instellingen voor Java.

1. Blader naar de [Azure-portal] en zich aanmelden.

1. Nadat u hebt geregistreerd bij uw account in de Azure portal, klikt u op het menupictogram voor **App Services**:
   
   ![Azure Portal][AZ01]

1. Wanneer de **App Services** pagina wordt weergegeven, klikt u op **+ toevoegen** voor het maken van een nieuwe App Service.

   ![App Service maken][AZ02]

1. Wanneer de lijst met sjablonen voor web-app wordt weergegeven, klikt u op de koppeling voor de eenvoudige Web-App van Microsoft.

   ![Sjablonen voor Web-App][AZ03]

1. Wanneer de pagina gegevens voor de Web-App-sjabloon wordt weergegeven, klikt u op **maken**.

   ![Web-app maken][AZ04]

1. Geef een unieke naam voor uw web-app en geef eventuele aanvullende instellingen en vervolgens **maken**.

   ![Instellingen voor Web-App maken][AZ05]

1. Nadat uw web-app is gemaakt, klikt u op het menupictogram voor **App Services**, en klik vervolgens op uw nieuwe web-app:

   ![Lijst met Web-Apps][AZ06]

1. Wanneer uw web-app wordt weergegeven, moet u de Java-versie opgeven met behulp van de volgende stappen uit:

   a. Klik op de **toepassingsinstellingen** menu-item.

   b. Kies **Java 8** voor de Java-versie.

   c. Kies **nieuwste** voor de secundaire Java-versie.

   d. Kies **nieuwste Tomcat 8.5** voor de webcontainer. (Deze container niet daadwerkelijk wordt gebruikt; Azure wordt de container van uw toepassing Spring opstartinstallatiekopie gebruiken.)

   e. Klik op **Opslaan**.

   ![Toepassingsinstellingen][AZ07]

1. Geef de referenties voor uw FTP-implementatie met behulp van de volgende stappen uit:

   a. Klik op de **Implementatiereferenties** menu-item.

   b. Geef uw gebruikersnaam en wachtwoord.

   c. Klik op **Opslaan**.

   ![Geef referenties voor implementatie][AZ08]

1. De FTP-verbindingsgegevens worden opgehaald met behulp van de volgende stappen uit:

   a. Klik op de **Implementatiereferenties** menu-item.

   b. Uw volledige FTP-gebruikersnaam en de URL kopiëren en opslaan voor de volgende sectie van deze zelfstudie.

   ![FTP-URL en referenties][AZ09]

## <a name="deploy-your-spring-boot-web-app-to-azure"></a>Uw Spring opstarten web-app implementeren in Azure

De volgende stappen begeleidt u bij de stappen voor het implementeren van uw Spring opstarten web-app in Azure.

1. Open een teksteditor zoals Kladblok van Windows en plak de volgende tekst in een nieuw document en sla het bestand als *web.config*:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
       </handlers>
       <httpPlatform processPath="%JAVA_HOME%\bin\java.exe"
           arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\gs-spring-boot-0.1.0.jar&quot;">
       </httpPlatform>
     </system.webServer>
   </configuration>
   ```

1. Nadat u hebt opgeslagen de *web.config* van het bestand in uw systeem, verbinding maken met uw web-app via de FTP-URL, gebruikersnaam en wachtwoord van de voorgaande sectie van deze zelfstudie gebruiken. Bijvoorbeeld:
   ```
   ftp
   open waws-prod-sn0-000.ftp.azurewebsites.windows.net
   user wingtiptoys-springboot\wingtiptoysuser
   pass ********
   ```

1. Wijzigen van de externe map in de hoofdmap van uw web-app (die is op */site/wwwroot*), kopieert u het JAR-bestand van uw toepassing Spring opstart en de *web.config* van eerder. Bijvoorbeeld:
   ```
   cd site/wwwroot
   put gs-spring-boot-0.1.0.jar
   put web.config
   ```

1. Nadat u uw JAR hebt geïmplementeerd en *web.config* bestanden naar uw web-app, moet u opnieuw opstarten van uw web-app met Azure portal:

   ![][AZ10]

1. De web-app testen door te bladeren naar uw web-app-URL met een webbrowser, of gebruik de syntaxis op het volgende voorbeeld hebt u curl beschikbaar:
   ```
   curl http://wingtiptoys-springboot.azurewebsites.net/
   ```

1. U ziet het volgende bericht weergegeven: **begroetingen vanaf Spring opstart!**

   ![Voorbeeld-App bladeren][SB02]

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over het gebruik van opstarten Spring toepassingen in Azure:

* [Een toepassing Spring opstarten op Linux in de Azure Containerservice implementeren](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md)

* [Een toepassing Spring opstarten op een Cluster Kubernetes in de Azure Containerservice implementeren](../container-service/kubernetes/container-service-deploy-spring-boot-app-on-kubernetes.md)

Voor meer informatie over het gebruik van Azure met Java raadpleegt u het [Azure Java-ontwikkelaarscentrum] en de [Java Tools voor Visual Studio Team Services].

Zie voor meer informatie over depoying WebApps in Azure met behulp van FTP [uw app implementeren in Azure App Service met behulp van FTP/S].

Zie voor meer informatie over het voorbeeldproject Spring Boot [Spring opstarten aan de slag].

Zie voor meer informatie over aan de slag met uw eigen toepassingen Spring opstartinstallatiekopie, de **Spring Initializr** op https://start.spring.io/.

Zie voor meer informatie over het configureren van aanvullende instellingen voor uw web-app [web-apps configureren in Azure App Service].

<!-- URL List -->

[Azure App Service]: https://azure.microsoft.com/services/app-service/
[Azure Container Service]: https://azure.microsoft.com/services/container-service/
[Azure Java-ontwikkelaarscentrum]: https://azure.microsoft.com/develop/java/
[Azure-portal]: https://portal.azure.com/
[web-apps configureren in Azure App Service]: /azure/app-service/web-sites-configure
[uw app implementeren in Azure App Service met behulp van FTP/S]: https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp
[gratis Azure-account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools voor Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[voordelen als MSDN-abonnee]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[ Spring Boot]: http://projects.spring.io/spring-boot/
[Spring opstarten aan de slag]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB01.png
[SB02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/SB02.png

[AZ01]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ01.png
[AZ02]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ02.png
[AZ03]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ03.png
[AZ04]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ04.png
[AZ05]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ05.png
[AZ06]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ06.png
[AZ07]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ07.png
[AZ08]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ08.png
[AZ09]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ09.png
[AZ10]: ./media/app-service-deploy-spring-boot-web-app-on-azure/AZ10.png
