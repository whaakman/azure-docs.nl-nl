---
title: Het gebruik van de opstartinstallatiekopie Spring Starter met een Azure-API voor DocumentDB Cosmos DB
description: Informatie over het configureren van een toepassing die is gemaakt met de versie die voorjaar opstarten initialisatiefunctie met de Azure-API voor DocumentDB Cosmos DB.
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
keywords: Spring, Spring opstarten Starter Cosmos DB
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/08/2017
ms.author: robmcm;yungez;kevinzha
ms.openlocfilehash: 273cc750857c5e466882060a38ac0f3475811e98
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-the-spring-boot-starter-with-azure-cosmos-db-documentdb-api"></a>De versie die voorjaar opstarten Starter gebruiken met Azure Cosmos DB DocumentDB-API

## <a name="overview"></a>Overzicht

De  **[Spring Framework]**  is een open source-oplossing waarmee ontwikkelaars van Java op bedrijfsniveau toepassingen maken. Een van de meer populaire projecten die is gebaseerd op deze platform is [Spring Boot], waarmee u een vereenvoudigde benadering voor het maken van zelfstandige Java-toepassingen. Om te helpen aan de slag met Spring opstarten ontwikkelaars, verschillende voorbeeld Spring opstarten pakketten zijn beschikbaar op <https://github.com/spring-guides/>. Naast de kiezen uit de lijst met basic Spring opstarten projecten, de  **[Spring Initializr]**  kunnen softwareontwikkelaars aan de slag met het maken van aangepaste Spring Boot-toepassingen.

Azure Cosmos-database is een globaal gedistribueerd database-service waarmee ontwikkelaars kunnen werken met gegevens met verschillende standaard API's, zoals DocumentDB, MongoDB, grafiek en tabel-API's. Microsoft versie die voorjaar opstarten Starter kunnen ontwikkelaars eenvoudig worden geïntegreerd met Azure Cosmos DB met behulp van DocumentDB APIs Spring opstarten toepassingen.

Dit artikel wordt beschreven voor het maken van een Cosmos Azure DB de Azure-portal gebruikt, is met behulp van de **Spring Initializr** een aangepaste java-toepassing maken en vervolgens de Spring opstarten Starter-functionaliteit toe te voegen aan uw aangepaste toepassing voor het opslaan van gegevens in en gegevens ophalen van de Cosmos Azure DB met behulp van de DocumentDB-API.

## <a name="prerequisites"></a>Vereisten

De volgende vereisten zijn vereist voor de stappen in dit artikel:

* Een Azure-abonnement; Als u nog geen Azure-abonnement hebt, kunt u activeren uw [voordelen als MSDN-abonnee] of zich aanmelden voor een [gratis Azure-account].

* Een [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), 1,7 of hoger.

* [Apache Maven](http://maven.apache.org/), versie 3.0 of hoger.

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>Een Azure-Cosmos-database maken met behulp van de Azure-portal

1. Blader naar de Azure portal op <https://portal.azure.com/> en klik op **+ nieuw**.

   ![Azure Portal][AZ01]

1. Klik op **Databases**, en klik vervolgens op **Azure Cosmos DB**.

   ![Azure Portal][AZ02]

1. Op de **Azure Cosmos DB** pagina, voer de volgende informatie:

   * Voer een unieke **ID**, gaat u als de URI voor uw database. Bijvoorbeeld: *wingtiptoysdata.documents.azure.com*.
   * Kies **SQL (Document DB)** voor de API.
   * Kies de **abonnement** u wilt gebruiken voor uw database.
   * Geef op of maak een nieuwe **resourcegroep** voor uw database, of kies een bestaande resourcegroep.
   * Geef de **locatie** voor uw database.
   
   Wanneer u deze opties hebt opgegeven, klikt u op **maken** om uw database te maken.

   ![Azure Portal][AZ03]

1. Wanneer uw database is gemaakt, wordt vermeld in uw Azure **Dashboard**, ook als onder de **alle Resources** en **Azure Cosmos DB** pagina's. U kunt klikken op de database op een van deze locaties om de eigenschappenpagina voor uw cache te openen.

   ![Azure Portal][AZ04]

1. Wanneer de eigenschappenpagina voor uw database wordt weergegeven, klikt u op **toegangssleutels** en kopieer uw URI en toegangssleutels voor uw database; u kunt deze waarden worden gebruikt in uw toepassing Spring opstarten.

   ![Azure Portal][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Een eenvoudige Spring Boot-toepassing maken met de versie die voorjaar Initializr

1. Blader naar <https://start.spring.io/>.

1. Opgeven dat u voor het genereren van een **Maven** project met **Java**, voer de **groep** en **artefact** namen voor uw toepassing en klik vervolgens op de knop **genereren Project**.

   ![Basic Spring Initializr opties][SI01]

   > [!NOTE]
   >
   > De versie die voorjaar Initializr gebruikt de **groep** en **artefact** namen voor het maken van de naam van het pakket; bijvoorbeeld: *com.example.wintiptoys*.
   >

1. Wanneer u wordt gevraagd, download u het project naar een pad op de lokale computer.

   ![Aangepaste Spring Boot-project downloaden][SI02]

1. Nadat u de bestanden op uw lokale system hebt uitgepakt, zal uw eenvoudige Spring Boot-toepassing gereed is voor het bewerken van zijn.

   ![Aangepaste Spring project opstartbestanden][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-azure-spring-boot-starter"></a>Configureer uw app Spring opstarten voor het gebruik van de Azure Spring opstarten Starter

1. Zoek de *pom.xml* bestand in de map van uw app; bijvoorbeeld:

   `C:\SpringBoot\wingtiptoys\pom.xml`

   -of-

   `/users/example/home/wingtiptoys/pom.xml`

   ![Zoek het bestand pom.xml][PM01]

1. Open de *pom.xml* bestand in een teksteditor en de volgende regels toevoegen aan lijst met `<dependencies>`:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![Het bestand pom.xml bewerken][PM02]

1. Sla op en sluit de *pom.xml* bestand.

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a>Configureer uw app Spring opstarten voor het gebruik van uw Azure-Cosmos-DB

1. Zoek de *application.properties* bestand de *resources* map van uw app; bijvoorbeeld:

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   -of-

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![Zoek het bestand application.properties][RE01]

1. Open de *application.properties* bestand in een teksteditor en de volgende regels toevoegen aan het bestand en de voorbeeldwaarden vervangen door de juiste eigenschappen voor uw database:

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![Bewerken van het bestand application.properties][RE02]

1. Sla op en sluit de *application.properties* bestand.

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>Voorbeeldcode voor het implementeren van eenvoudige databasefunctionaliteit toevoegen

In deze sectie maakt u twee Java-klassen voor het opslaan van gebruikersgegevens en wijzig vervolgens de hoofdtoepassingsklasse voor het maken van een exemplaar van de gebruikersklasse en sla deze op uw database.

### <a name="define-a-basic-class-for-storing-user-data"></a>Een eenvoudige klasse voor het opslaan van gebruikersgegevens definiëren

1. Maak een nieuw bestand met de naam *User.java* in dezelfde map als de hoofdtoepassing Java-bestand.

1. Open de *User.java* bestand in een teksteditor en voeg de volgende regels in het bestand voor het definiëren van een algemene gebruikersklasse die worden opgeslagen en ophalen van waarden in de database:

   ```java
   package com.example.wingtiptoys;

   public class User {
      private String id;
      private String firstName;
      private String lastName;
 
      public User(String id, String firstName, String lastName) {
         this.id = id;
         this.firstName = firstName;
         this.lastName = lastName;
      }
   
      public String getId() {
         return this.id;
      }

      public void setId(String id) {
         this.id = id;
      }

      public String getFirstName() {
         return firstName;
      }

      public void setFirstName(String firstName) {
         this.firstName = firstName;
      }

      public String getLastName() {
         return lastName;
      }

      public void setLastName(String lastName) {
         this.lastName = lastName;
      }

      @Override
      public String toString() {
         return String.format("User: %s %s", firstName, lastName);
      }
   }
   ```

1. Sla op en sluit de *User.java* bestand.

### <a name="define-a-data-repository-interface"></a>Definieer een interface-opslagplaats

1. Maak een nieuw bestand met de naam *UserRepository.java* in dezelfde map als de hoofdtoepassing Java-bestand.

1. Open de *UserRepository.java* bestand in een teksteditor en voeg de volgende regels in het bestand voor het definiëren van een gebruikersinterface van de opslagplaats die uitgebreider is dan de standaardinterface voor DocumentDB-opslagplaats:

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. Sla op en sluit de *UserRepository.java* bestand.

### <a name="modify-the-main-application-class"></a>Wijzigen van de hoofdtoepassing-klasse

1. Zoek het hoofdvenster van de toepassing Java-bestand in de pakketmap van uw app; bijvoorbeeld:

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   -of-

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![Zoek de Java-bestand van de toepassing][JV01]

1. Open het hoofdvenster van de toepassing Java-bestand in een teksteditor en voeg de volgende regels in het bestand:

   ```java
   package com.example.wingtiptoys;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class WingtiptoysApplication implements CommandLineRunner {

      @Autowired
      private UserRepository repository;
    
      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysApplication.class, args);
      }

      public void run(String... var1) throws Exception {
         final User testUser = new User("testId", "testFirstName", "testLastName");

         repository.deleteAll();
         repository.save(testUser);

         final User result = repository.findOne(testUser.getId());

         System.out.printf("\n\n%s\n\n",result.toString());
      }
   }
   ```

1. Opslaan en sluiten van de hoofdtoepassing Java-bestand.

## <a name="build-and-test-your-app"></a>Bouwen en testen van uw app

1. Open een opdrachtprompt en wijzig de map naar de map waar uw *pom.xml* bestand bevindt zich; bijvoorbeeld:

   `cd C:\SpringBoot\wingtiptoys`

   -of-

   `cd /users/example/home/wingtiptoys`

1. Uw toepassing Spring opstarten met Maven bouwen en uitvoeren. bijvoorbeeld:

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. Uw toepassing meerdere runtime-berichten worden weergegeven en u ziet het bericht `User: testFirstName testLastName` weergegeven om aan te geven waarden met succes is opgeslagen en opgehaald uit de database.

   ![Geslaagde uitvoer van de toepassing][JV02]

1. Optioneel: U kunt de Azure-portal de inhoud van uw Azure-Cosmos-DB uit de eigenschappenpagina voor uw database weergeven door te klikken op **documentverkenner**, en vervolgens te selecteren en item uit de vervolgkeuzelijst om de inhoud weer te geven.

   ![Met behulp van de documentverkenner om uw gegevens weer te geven][JV03]

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over het gebruik van Azure DB die Cosmos en Java:

* [Documentatie Azure Cosmos DB].

* [Azure Cosmos DB: Een DocumentDB-API-app met Java en de Azure portal maken][Build a DocumentDB API app with Java]

Zie de volgende artikelen voor meer informatie over het gebruik van opstarten Spring toepassingen in Azure:

* [Spring Boot DocumenDB Starter voor Azure](https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample)

* [Een toepassing Spring opstart in de Azure App Service implementeren](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [Spring opstarten toepassing uitvoert op een Cluster Kubernetes in de Azure Containerservice](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

Voor meer informatie over het gebruik van Azure met Java raadpleegt u het [Azure Java-ontwikkelaarscentrum] en de [Java Tools voor Visual Studio Team Services].

<!-- URL List -->

[Documentatie Azure Cosmos DB]: /azure/cosmos-db/
[Azure Java-ontwikkelaarscentrum]: https://azure.microsoft.com/develop/java/
[Build a DocumentDB API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-documentdb-java
[gratis Azure-account]: https://azure.microsoft.com/pricing/free-trial/
[Java Tools voor Visual Studio Team Services]: https://java.visualstudio.com/
[voordelen als MSDN-abonnee]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ01.png
[AZ02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ02.png
[AZ03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ03.png
[AZ04]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ04.png
[AZ05]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ05.png

[SI01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI01.png
[SI02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI02.png
[SI03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI03.png

[RE01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/RE01.png
[RE02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/RE02.png

[PM01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/PM01.png
[PM02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/PM02.png

[JV01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV01.png
[JV02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV02.png
[JV03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV03.png
