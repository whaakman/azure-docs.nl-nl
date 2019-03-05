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
ms.openlocfilehash: d607d6cd813b23051e1676153cbb134261bcf5bc
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960604"
---
# <a name="quickstart-create-a-java-spring-app-with-app-configuration"></a>Quickstart: een Java Spring-app maken met App Configuration

Azure App Configuration is een beheerde configuratieservice in Azure. Hiermee kunt u eenvoudig alle toepassingsinstellingen opslaan en beheren op één plek die gescheiden is van uw code. Deze quickstart laat zien hoe u de service kunt opnemen in een Java Spring-app.

U kunt elke code-editor gebruiken om de stappen in deze snelstart uit te voeren. [Visual Studio Code](https://code.visualstudio.com/) is een uitstekende optie die beschikbaar is op de Windows-, macOS- en Linux-platforms.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze quickstart installeert u een ondersteunde [Java Development Kit (JDK)](https://aka.ms/azure-jdks) van versie 8 en [Apache Maven](http://maven.apache.org/) van versie 3.0 of hoger.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Een app-configuratiearchief maken

1. Als u een nieuw configuratiearchief voor de app wilt maken, moet u zich eerst aanmelden bij de [Azure-portal](https://aka.ms/azconfig/portal). Selecteer **+ Een resource maken** linksboven van de pagina. Typ in het tekstvak **Marketplace doorzoeken** de tekst **App Configuration** en druk op **Enter**.

    ![Zoeken naar App Configuration](./media/quickstarts/azure-app-configuration-new.png)

2. Klik in de lijst met zoekresultaten op **App Configuration** en vervolgens op **Maken**.

3. Voer op de pagina **App Configuration** > **Maken** de volgende instellingen in:

    | Instelling | Voorgestelde waarde | Beschrijving |
    |---|---|---|
    | **Resourcenaam** | Wereldwijd unieke naam | Voer een unieke resourcenaam in voor de resource van het app-configuratiearchief. De naam moet een tekenreeks zijn van 1 tot 63 tekens die alleen cijfers, letters en het teken `-` mag bevatten. De naam mag niet beginnen of eindigen met het teken `-`, en opeenvolgende `-`-tekens zijn niet toegestaan.  |
    | **Abonnement** | Uw abonnement | Selecteer het Azure-abonnement dat u wilt gebruiken om App Configuration te testen. Als uw account maar één abonnement heeft, wordt dit automatisch geselecteerd en wordt de vervolgkeuzelijst **Abonnement** niet weergegeven. |
    | **Resourcegroep** | *AppConfigTestResources* | Selecteer of maak een resourcegroep voor de resource van het app-configuratiearchief. Deze groep is handig voor het ordenen van meerdere resources die u mogelijk op een bepaald moment wilt verwijderen door resourcegroep te verwijderen. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) voor meer informatie. |
    | **Locatie** | *US - centraal* | Gebruik **Locatie** om de geografische locatie op te geven waar SignalR-resource wordt gehost. Voor de beste prestaties wordt u aangeraden om de resource te maken in dezelfde regio als de andere onderdelen van uw toepassing. |

    ![Een app-configuratiearchief maken](./media/quickstarts/azure-app-configuration-create.png)

4. Klik op **Create**. De implementatie kan enkele minuten duren.

5. Wanneer de implementatie is voltooid, klikt u op **Instellingen** > **Toegangssleutels**. Noteer de verbindingsreeks van de primaire alleen-lezen- of lees/schrijf-sleutel. U hebt deze later nodig voor de configuratie van uw toepassing, zodat deze kan communiceren met het app-configuratiearchief dat u hebt gemaakt. Deze verbindingsreeks heeft de volgende indeling:

        Endpoint=<your_endpoint>;Id=<your_id>;Secret=<your_secret>

    U moet de gehele tekenreeks in uw toepassing gebruiken.

6. Klik op **Sleutel-waardeverkenner** en **+ Maken** om de volgende sleutel-waardeparen toe te voegen:

    | Sleutel | Waarde |
    |---|---|
    | /application/config.message | Hello |

    U kunt **Label** en **Inhoudstype** leeg laten.

## <a name="create-a-spring-boot-app"></a>Een Spring Boot-app maken

U gebruikt de [Spring Initializr](https://start.spring.io/) om een nieuw Spring Boot-project te maken.

1. Blader naar <https://start.spring.io/>.

2. Geef de volgende opties op:

   * Genereer een **Maven**-project met **Java**.
   * Geef een **Spring Boot**-versie op van 2.0 of hoger.
   * Geef de namen voor **Groep** en **Artefact** voor uw toepassing op.
   * Voeg de afhankelijkheid **Web** toe.

3. Wanneer u de bovenstaande opties hebt opgegeven, klikt u op **Project genereren**. Wanneer u hierom wordt gevraagd, downloadt u het project naar een pad op uw lokale computer.

## <a name="connect-to-app-configuration-store"></a>Verbinding maken met een app-configuratiearchief

1. Nadat u de bestanden op uw lokale systeem hebt uitgepakt, is uw eenvoudige Spring Boot-toepassing gereed om te bewerken. Zoek het bestand *pom.xml* in de hoofdmap van uw app.

2. Open het bestand *pom.xml* in een teksteditor en voeg de starter Spring Cloud Azure Config toe aan de lijst van `<dependencies>`:

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-appconfiguration-config</artifactId>
        <version>1.1.0.RC2</version>
    </dependency>
    ```

3. Maak een nieuw Java-bestand met de naam *MessageProperties.java* in de pakketmap van uw app. Voeg de volgende regels toe.

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

4. Maak een nieuw Java-bestand met de naam *HelloController.java* in de pakketmap van uw app. Voeg de volgende regels toe.

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

6. Maak een nieuw bestand met de naam `bootstrap.yaml` in de resourcemap van uw app en voeg de volgende regels toe aan het bestand. Vervang ook de voorbeeldwaarden door de gewenste eigenschappen voor het app-configuratiearchief.

    ```yaml
    spring:
        cloud:
            azure:
                config:
                    stores:
                        - connection-string: [your-connection-string]
    ```

## <a name="build-and-run-the-app-locally"></a>De app lokaal compileren en uitvoeren

1. Maak de Spring Boot-toepassing met Maven en voer deze uit; bijvoorbeeld:

    ```shell
    mvn clean package
    mvn spring-boot:run
    ```
2. Zodra uw toepassing wordt uitgevoerd, kunt u *curl* gebruiken om uw toepassing te testen; bijvoorbeeld:

      ```shell
      curl -X GET http://localhost:8080/
      ```
    Er hoort een bericht te worden weergegeven dat u zich in het app-configuratiearchief bevindt.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een nieuw app-configuratiearchief gemaakt en deze gebruikt met een Java Spring-app. Ga naar [Spring op de Azure-startpagina](https://docs.microsoft.com/java/azure/spring-framework/) voor meer informatie.

Als u meer wilt weten over het gebruik van App-configuratie, gaat u verder met de volgende zelfstudie, waarin verificatie wordt behandeld.

> [!div class="nextstepaction"]
> [Beheerde identiteiten voor integratie met Azure-resources](./integrate-azure-managed-service-identity.md)