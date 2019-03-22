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
ms.openlocfilehash: 050a7a3718cb7c9eb864b7ed6ea0787c079e31b9
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226314"
---
# <a name="quickstart-create-a-java-spring-app-with-app-configuration"></a>Quickstart: een Java Spring-app maken met App Configuration

Azure-appconfiguratie is een beheerde configuratieservice in Azure. U kunt deze eenvoudig opslaan en beheren van alle instellingen van de toepassing op één plek dat gescheiden van uw code. Deze quickstart laat zien hoe u de service kunt opnemen in een Java Spring-app.

Een code-editor kunt u de stappen in deze Quick Start. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie beschikbaar is op Windows, macOS en Linux-platforms.

## <a name="prerequisites"></a>Vereisten

Om te doen in deze Quick Start, installeert u een ondersteunde [Java Development Kit (JDK)](https://aka.ms/azure-jdks) versie 8 en [Apache Maven](https://maven.apache.org/) met versie 3.0 of hoger.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Een app-configuratiearchief maken

1. Voor het maken van een nieuwe app-configuratiearchief, moet u zich aanmelden bij de [Azure-portal](https://aka.ms/azconfig/portal). Selecteer in de linkerbovenhoek van de pagina **+ een resource maken**. In de **Marketplace doorzoeken** Voer **Appconfiguratie** en druk op Enter.

    ![Zoeken naar App Configuration](./media/quickstarts/azure-app-configuration-new.png)

2. Selecteer **Appconfiguratie** uit de zoekresultaten, en selecteer vervolgens **maken**.

3. Op de **Appconfiguratie** > **maken** pagina, voer de volgende instellingen.

    | Instelling | Voorgestelde waarde | Description |
    |---|---|---|
    | **Resourcenaam** | Wereldwijd unieke naam | Voer een unieke resourcenaam in voor de resource van het app-configuratiearchief. De naam moet een tekenreeks zijn van 1 tot 63 tekens die alleen cijfers, letters en het teken `-` mag bevatten. De naam mag niet beginnen of eindigen met de `-` teken en opeenvolgende `-` tekens zijn niet geldig.  |
    | **Abonnement** | Uw abonnement | Selecteer het Azure-abonnement dat u wilt gebruiken om App Configuration te testen. Als uw account slechts één abonnement heeft, wordt deze automatisch geselecteerd en de **abonnement** vervolgkeuzelijst wordt niet weergegeven. |
    | **Resourcegroep** | *AppConfigTestResources* | Selecteer of maak een resourcegroep voor de resource van het app-configuratiearchief. Deze groep is handig voor het ordenen van meerdere resources die u mogelijk wilt verwijderen op hetzelfde moment door de resourcegroep te verwijderen. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) voor meer informatie. |
    | **Locatie** | *US - centraal* | Gebruik **Locatie** om de geografische locatie op te geven waar SignalR-resource wordt gehost. Voor de beste prestaties, de resource te maken in dezelfde regio als andere onderdelen van uw toepassing. |

    ![Een app-configuratiearchief maken](./media/quickstarts/azure-app-configuration-create.png)

4. Selecteer **Maken**. De implementatie kan een paar minuten langer duren.

5. Nadat de implementatie is voltooid, selecteert u **instellingen** > **toegangssleutels**. Maak een notitie van een van beide de alleen-lezen of een primaire Lees-/ verbindingsreeks primaire sleutel. Met deze verbindingsreeks later kunt u uw toepassing configureren voor het communiceren met de app-configuratie-store die u hebt gemaakt. Deze verbindingsreeks heeft de volgende indeling:

        Endpoint=<your_endpoint>;Id=<your_id>;Secret=<your_secret>

    Gebruik de gehele tekenreeks in uw toepassing.

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
        <version>1.1.0.M1</version>
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
> [Beheerde identiteiten voor de integratie van Azure-resources](./integrate-azure-managed-service-identity.md)