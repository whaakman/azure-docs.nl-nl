---
title: Het configureren van een app Spring opstarten initialisatiefunctie voor het gebruik van Redis-Cache
description: Informatie over het configureren van een toepassing Spring opstarten gemaakt met de versie die voorjaar Initializr Azure Redis-Cache gebruiken.
services: redis-cache
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
keywords: Spring, Spring opstarten Starter, Redis-Cache
ms.assetid: 
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: java
ms.topic: article
ms.date: 08/31/2017
ms.author: robmcm;zhijzhao;yidon
ms.openlocfilehash: 7a6ec549654d00975494bac8594a6777af5ec415
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-a-spring-boot-initializer-app-to-use-redis-cache"></a>Het configureren van een app Spring opstarten initialisatiefunctie voor het gebruik van Redis-Cache

## <a name="overview"></a>Overzicht

De  **[Spring Framework]**  is een open source-oplossing waarmee ontwikkelaars van Java op bedrijfsniveau toepassingen maken. Een van de meer populaire projecten die is gebaseerd op deze platform is [Spring Boot], waarmee u een vereenvoudigde benadering voor het maken van zelfstandige Java-toepassingen. Om te helpen aan de slag met Spring opstarten ontwikkelaars, verschillende voorbeeld Spring opstarten pakketten zijn beschikbaar op <https://github.com/spring-guides/>. Naast de kiezen uit de lijst met basic Spring opstarten projecten, de  **[Spring Initializr]**  kunnen softwareontwikkelaars aan de slag met het maken van aangepaste Spring Boot-toepassingen.

Dit artikel begeleidt u bij het maken van een Redis-cache met behulp van de Azure portal, vervolgens met de **Spring Initializr** voor het maken van een aangepaste toepassing, en maak vervolgens een Java-webtoepassing die worden opgeslagen en opgehaald van gegevens met behulp van uw Redis cache.

## <a name="prerequisites"></a>Vereisten

De volgende vereisten zijn vereist voor de stappen in dit artikel:

* Een Azure-abonnement; Als u nog geen Azure-abonnement hebt, kunt u activeren uw [voordelen als MSDN-abonnee] of zich aanmelden voor een [gratis Azure-account].

* Een [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), 1,7 of hoger.

* [Apache Maven](http://maven.apache.org/), versie 3.0 of hoger.

## <a name="create-a-redis-cache-on-azure"></a>Een Redis-cache maken op Azure

1. Blader naar de Azure portal op <https://portal.azure.com/> en klik op het item voor **+ nieuw**.

   ![Azure Portal][AZ01]

1. Klik op **Database**, en klik vervolgens op **Redis-Cache**.

   ![Azure Portal][AZ02]

1. Op de **nieuwe Redis-Cache** pagina, geeft u de volgende informatie:

   * Voer de **DNS-naam** voor uw cache.
   * Geef uw **abonnement**, **resourcegroep**, **locatie**, en **prijscategorie**.
   * Voor deze zelfstudie kiest **deblokkeren poort 6379**.

   > [!NOTE]
   >
   > U kunt SSL gebruiken met Redis-caches, maar u moet een andere Redis-client, zoals Jedis gebruikt. Zie voor meer informatie [Azure Redis-Cache gebruiken met Java][Redis Cache with Java].
   >

   Wanneer u deze opties hebt opgegeven, klikt u op **maken** om uw cache te maken.

   ![Azure Portal][AZ03]

1. Nadat uw cache is voltooid, ziet u het weergegeven in uw Azure **Dashboard**, ook als onder de **alle Resources**, en **Redis-Caches** pagina's. U kunt klikken op uw cache op een van deze locaties om de eigenschappenpagina voor uw cache te openen.

   ![Azure Portal][AZ04]

1. Wanneer de pagina met de lijst met eigenschappen voor uw cache wordt weergegeven, klikt u op **toegangssleutels** en kopieer uw toegangssleutels voor uw cache.

   ![Azure Portal][AZ05]

## <a name="create-a-custom-application-using-the-spring-initializr"></a>Een aangepaste toepassing maken met de versie die voorjaar Initializr

1. Blader naar <https://start.spring.io/>.

1. Opgeven dat u voor het genereren van een **Maven** project met **Java**, voer de **groep** en **Aritifact** namen voor uw toepassing en klik vervolgens op de koppeling naar **overschakelen naar de volledige versie** van de versie die voorjaar Initializr.

   ![Basic Spring Initializr opties][SI01]

   > [!NOTE]
   >
   > De versie die voorjaar Initializr gebruikt de **groep** en **Aritifact** namen voor het maken van de naam van het pakket; bijvoorbeeld: *com.contoso.myazuredemo*.
   >

1. Schuif omlaag naar de **Web** sectie en schakel het selectievakje voor **Web**, schuif vervolgens omlaag naar de **NoSQL** sectie en schakel het selectievakje voor **Redis**, Ga naar de onderkant van de pagina en klik op de knop **genereren Project**.

   ![Opties voor volledige Spring Initializr][SI02]

1. Wanneer u wordt gevraagd, download u het project naar een pad op de lokale computer.

   ![Aangepaste Spring Boot-project downloaden][SI03]

1. Nadat u de bestanden op uw lokale system hebt uitgepakt, zal uw aangepaste Spring Boot-toepassing gereed is voor het bewerken van zijn.

   ![Aangepaste Spring project opstartbestanden][SI04]

## <a name="configure-your-custom-spring-boot-to-use-your-redis-cache"></a>Uw aangepaste Spring opstarten om het gebruik van uw Redis-Cache configureren

1. Zoek de *application.properties* bestand de *resources* map van uw app, of het bestand te maken als deze niet al bestaat.

   ![Zoek het bestand application.properties][RE01]

1. Open de *application.properties* bestand in een teksteditor en voeg de volgende regels in het bestand en de voorbeeldwaarden vervangen door de juiste eigenschappen uit uw cache:

   ```yaml
   # Specify the DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify the port for your Redis cache.
   spring.redis.port=6379

   # Specify the access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![Bewerken van het bestand application.properties][RE02]

   > [!NOTE]
   >
   > Als u een andere Redis-client, zoals Jedis waarmee u SSL gebruikt, geeft u poort 6380 in uw *application.properties* bestand. Zie voor meer informatie [Azure Redis-Cache gebruiken met Java][Redis Cache with Java].
   >

1. Sla op en sluit de *application.properties* bestand.

1. Maak een map met de naam *controller* onder de bronmap voor het pakket; bijvoorbeeld:

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   -of-

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. Maak een nieuw bestand met de naam *HelloController.java* in de *controller* map. Open het bestand in een teksteditor en voeg de volgende code toe:

   ```java
   package com.contoso.myazuredemo;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.data.redis.core.StringRedisTemplate;
   import org.springframework.data.redis.core.ValueOperations;

   @RestController
   public class HelloController {
   
      @Autowired
      private StringRedisTemplate template;

      @RequestMapping("/")
      // Define the Hello World controller.
      public String hello() {
      
         ValueOperations<String, String> ops = this.template.opsForValue();

         // Add a Hello World string to your cache.
         String key = "greeting";
         if (!this.template.hasKey(key)) {
             ops.set(key, "Hello World!");
         }

         // Return the string from your cache.
         return ops.get(key);
      }
   }
   ```
   
   Waar moet u vervangen `com.contoso.myazuredemo` met de naam van het pakket voor uw project.

1. Sla op en sluit de *HelloController.java* bestand.

1. Uw toepassing Spring opstarten met Maven bouwen en uitvoeren. bijvoorbeeld:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. De web-app testen door te bladeren naar http://localhost: 8080 met een webbrowser, of gebruik de syntaxis op het volgende voorbeeld hebt u curl beschikbaar:

   ```shell
   curl http://localhost:8080
   ```

   U ziet nu de 'Hello World!" bericht ontvangen van uw voorbeeld-domeincontroller die wordt weergegeven, die dynamisch worden opgehaald uit de Redis-cache.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over het gebruik van opstarten Spring toepassingen in Azure:

* [Een toepassing Spring opstart in de Azure App Service implementeren](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [Spring opstarten toepassing uitvoert op een Cluster Kubernetes in de Azure Containerservice](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

Voor meer informatie over het gebruik van Azure met Java raadpleegt u het [Azure Java-ontwikkelaarscentrum] en de [Java Tools voor Visual Studio Team Services].

Zie voor meer informatie over het ophalen van de slag met Redis-Cache met behulp van Java in Azure, [Azure Redis-Cache gebruiken met Java][Redis Cache with Java].

<!-- URL List -->

[Azure Java-ontwikkelaarscentrum]: https://azure.microsoft.com/develop/java/
[gratis Azure-account]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools voor Visual Studio Team Services]: https://java.visualstudio.com/
[voordelen als MSDN-abonnee]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[Redis Cache with Java]: cache-java-get-started.md

<!-- IMG List -->

[AZ01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ01.png
[AZ02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ02.png
[AZ03]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ03.png
[AZ04]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ04.png
[AZ05]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ05.png

[SI01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI01.png
[SI02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI02.png
[SI03]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI03.png
[SI04]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI04.png

[RE01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/RE01.png
[RE02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/RE02.png
