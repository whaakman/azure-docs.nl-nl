---
title: 'Quickstart: leren werken met Azure App Configuration | Microsoft Docs'
description: Een quickstart voor het gebruik van Azure App Configuration met Java Spring-apps.
services: azure-app-configuration
documentationcenter: ''
author: yidon
manager: jeffya
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: Spring
ms.workload: tbd
ms.date: 01/08/2019
ms.author: yidon
ms.openlocfilehash: d023c6ec9c3d24400fd2b7b9fcce9568aa851214
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60202227"
---
# <a name="quickstart-create-a-java-spring-app-with-app-configuration"></a>Quickstart: een Java Spring-app maken met App Configuration

Azure-appconfiguratie is een beheerde configuratieservice in Azure. U kunt deze eenvoudig opslaan en beheren van alle instellingen van de toepassing op één plek dat gescheiden van uw code. Deze quickstart laat zien hoe u de service kunt opnemen in een Java Spring-app.

Een code-editor kunt u de stappen in deze Quick Start. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie beschikbaar is op Windows, macOS en Linux-platforms.

## <a name="prerequisites"></a>Vereisten

Om te doen in deze Quick Start, installeert u een ondersteunde [Java Development Kit (JDK)](https://aka.ms/azure-jdks) versie 8 en [Apache Maven](https://maven.apache.org/) met versie 3.0 of hoger.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Een app-configuratiearchief maken

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Selecteer **sleutel/waarde-Explorer** > **+ maken** om toe te voegen van de volgende sleutel-waardeparen:

    | Sleutel | Value |
    |---|---|
    | /application/config.message | Hello |

    Laat **Label** en **inhoudstype** voorlopig leeg.

## <a name="create-a-spring-boot-app"></a>Een Spring Boot-app maken

U gebruikt de [Spring Initializr](https://start.spring.io/) om een nieuwe Spring Boot-project te maken.

1. Blader naar <https://start.spring.io/>.

2. Geef de volgende opties op:

   * Genereer een **Maven**-project met **Java**.
   * Geef een **Spring Boot** versie die gelijk is aan of groter is dan 2.0.
   * Geef de namen voor **Groep** en **Artefact** voor uw toepassing op.
   * Voeg de afhankelijkheid **Web** toe.

3. Nadat u de voorgaande opties hebt opgegeven, selecteert u **Project genereren**. Wanneer u hierom wordt gevraagd, downloadt u het project naar een pad op uw lokale computer.

## <a name="connect-to-an-app-configuration-store"></a>Verbinding maken met een app-configuratiearchief

1. Nadat u de bestanden op uw lokale systeem hebt uitgepakt, is de eenvoudig Spring Boot-toepassing gereed is voor het bewerken van. Zoek het bestand *pom.xml* in de hoofdmap van uw app.

2. Open het bestand *pom.xml* in een teksteditor en voeg de starter Spring Cloud Azure Config toe aan de lijst van `<dependencies>`:

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-appconfiguration-config</artifactId>
        <version>1.1.0.M3</version>
    </dependency>
    ```

3. Maak een nieuw Java-bestand met de naam *MessageProperties.java* in de pakketmap van uw app. Voeg de volgende regels:

    ```java
    @ConfigurationProperties(prefix = "config")
    public class MessageProperties {
        private String message;

        public String getMessage() {
            return message;
        }

        public void setMessage(String message) {
            this.message = message;
        }
    }
    ```

4. Maak een nieuw Java-bestand met de naam *HelloController.java* in de pakketmap van uw app. Voeg de volgende regels:

    ```java
    @RestController
    public class HelloController {
        private final MessageProperties properties;

        public HelloController(MessageProperties properties) {
            this.properties = properties;
        }

        @GetMapping
        public String getMessage() {
            return "Message: " + properties.getMessage();
        }
    }
    ```

5. Open het Java-bestand van de hoofdtoepassing en voeg `@EnableConfigurationProperties` toe om deze functie in te schakelen.

    ```java
    @SpringBootApplication
    @EnableConfigurationProperties(MessageProperties.class)
    public class AzureConfigApplication {
        public static void main(String[] args) {
            SpringApplication.run(AzureConfigApplication.class, args);
        }
    }
    ```

6. Maak een nieuw bestand met de naam `bootstrap.properties` in de map van de resources van uw app en voeg de volgende regels toe aan het bestand. Vervang de voorbeeldwaarden door de gewenste eigenschappen voor het opslaan van de app-configuratie.

    ```properties
    spring.cloud.azure.appconfiguration.stores[0].connection-string=[your-connection-string]
    ```

## <a name="build-and-run-the-app-locally"></a>De app lokaal compileren en uitvoeren

1. De Spring Boot-toepassing met Maven bouwen en uitvoeren, bijvoorbeeld:

    ```shell
    mvn clean package
    mvn spring-boot:run
    ```
2. Nadat uw toepassing wordt uitgevoerd, gebruikt u *curl* voor het testen van uw toepassing, bijvoorbeeld:

      ```shell
      curl -X GET http://localhost:8080/
      ```
    U ziet het bericht dat u hebt ingevoerd in het opslaan van de app-configuratie.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt gemaakt van een nieuwe app-configuratiearchief en deze gebruikt met een Java Spring-app. Zie voor meer informatie, [Spring op Azure](https://docs.microsoft.com/java/azure/spring-framework/).

Doorgaan naar de volgende zelfstudie waarin wordt gedemonstreerd verificatie voor meer informatie over het gebruik van App-configuratie.

> [!div class="nextstepaction"]
> [Integratie van beheerde identiteit](./howto-integrate-azure-managed-service-identity.md)
