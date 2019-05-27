---
title: 'Zelfstudie: Een Azure Time Series Insights-omgeving maken | Microsoft Docs'
description: Informatie over het maken van een Time Series Insights-omgeving die gevuld met gegevens uit de gesimuleerde apparaten.
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 04/26/2019
ms.author: dpalled
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: b8b46db043113f29f559ad44855d19f0d6ca73c3
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244162"
---
# <a name="tutorial-create-an-azure-time-series-insights-environment"></a>Zelfstudie: Een Azure Time Series Insights-omgeving maken

In deze zelfstudie leidt u door het proces voor het maken van een Azure Time Series Insights-omgeving die gevuld met gegevens uit de gesimuleerde apparaten. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een Time Series Insights-omgeving.
> * Maak een simulatie van apparaten met een IoT-hub.
> * De Time Series Insights-omgeving verbinden met de IoT-hub.
> * Een apparaatsimulatie uitvoeren om gegevens te streamen in de Time Series Insights-omgeving.
> * Controleer of de gesimuleerde telemetrie-gegevens.

## <a name="video"></a>Video

### <a name="learn-how-to-use-an-azure-iot-solution-accelerator-to-generate-data-and-get-started-with-time-series-insights-br"></a>Informatie over het gebruik van een Azure IoT-oplossingsversnellers voor het genereren van gegevens en aan de slag met Time Series Insights. </br>

> [!VIDEO https://www.youtube.com/embed/6ehNf6AJkFo]

## <a name="prerequisites"></a>Vereisten

* Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/).
* Uw Azure-aanmelden-account ook lid zijn van van het abonnement moet worden **eigenaar** rol. Zie voor meer informatie, [toegang beheren met behulp van op rollen gebaseerd toegangsbeheer en Azure portal](/azure/role-based-access-control/role-assignments-portal).

## <a name="overview"></a>Overzicht

De Time Series Insights-omgeving is waar de apparaatgegevens worden verzameld en opgeslagen. Nadat de gegevens worden opgeslagen, de [Azure Time Series Insights explorer](time-series-quickstart.md) en [Time Series Insights Query-API](/rest/api/time-series-insights/ga-query-api) kan worden gebruikt om te zoeken en analyseren van de gegevens. Azure IoT Hub is het verbindingspunt wordt gebruikt door alle apparaten (gesimuleerde of fysieke) veilig verbinding maken met en gegevens verzenden naar de Azure-cloud. De [Time Series Insights-overzicht](time-series-insights-overview.md) opmerkingen bij de dat Azure IoT Hub ook als een gebeurtenisbron fungeert voor het streamen van gegevens naar een Time Series Insights-omgeving. In deze zelfstudie wordt een [IoT-oplossingsversnellers](/azure/iot-accelerators/) te genereren en het streamen van voorbeeldtelemetrie die naar IoT Hub.

>[!TIP]
> IoT-oplossingsversnellers bieden geavanceerde vooraf geconfigureerde oplossingen die u gebruiken kunt om de ontwikkeling van aangepaste IoT-oplossingen.

## <a name="create-an-environment"></a>Een omgeving maken

Maak eerst een Time Series Insights-omgeving in uw Azure-abonnement.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van de account van uw Azure-abonnement. 
1. Selecteer linksboven **+ Een resource maken**. 
1. Selecteer de categorie **Internet of Things** en vervolgens **Time Series Insights**. 

   [![Selecteer de Time Series Insights-omgevingsresource](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png#lightbox)

1. Op de **Time Series Insights-omgeving** pagina, vult u de vereiste parameters.

   Parameter|Description
   ---|---
   **Omgevingsnaam** | Kies een unieke naam voor de Time Series Insights-omgeving. De namen worden gebruikt door de Time Series Insights explorer en de Query-API's.
   **Abonnement** | Abonnementen zijn containers voor Azure-resources. Kies een abonnement om de Time Series Insights-omgeving te maken.
   **Resourcegroep** | Een resourcegroep is een container voor Azure-resources. Kies een bestaande resourcegroep of maak een nieuw wachtwoord voor de resource van Time Series Insights-omgeving.
   **Locatie** | Kies een regio data center voor uw Time Series Insights-omgeving. Om te voorkomen dat extra bandbreedtekosten en latentie, houden met de Time Series Insights-omgeving in dezelfde regio als andere IoT-resources.
   **SKU-prijzen** | Kies de benodigde doorvoer. Selecteer voor de laagste kosten en starter capaciteit, `S1`.
   **Capaciteit** | Capaciteit is de vermenigvuldigingsfactor die wordt toegepast op de invoersnelheid, opslagcapaciteit en kosten die aan de geselecteerde SKU zijn verbonden. U kunt de capaciteit wijzigen nadat de is gemaakt. Voor de laagste kosten, selecteert u een capaciteit van 1.

   Wanneer u klaar bent, selecteert u **maken** om te beginnen met het inrichtingsproces.

   [![Een resource van Time Series Insights-omgeving maken](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png#lightbox)

1. Controleer de **meldingen** deelvenster voor het bewaken van de implementatie is voltooid. 

   [![Time Series Insights-omgeving de implementatie is voltooid](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png#lightbox)

## <a name="create-a-device-simulation"></a>Een apparaatsimulatie maken

Maak vervolgens de oplossing voor het simuleren van apparaten, waardoor testgegevens om in te vullen uw Time Series Insights-omgeving worden gegenereerd.

1. In een apart venster of tabblad, gaat u naar [azureiotsolutions.com](https://www.azureiotsolutions.com). Meld u aan met behulp van hetzelfde account Azure-abonnement en selecteer de **Apparaatsimulatie** accelerator.

   [![Voer de Apparaatsimulatie-accelerator](media/tutorial-create-populate-tsi-environment/sa-main.png)](media/tutorial-create-populate-tsi-environment/sa-main.png#lightbox)

1. Voer de vereiste parameters op de **maken Apparaatsimulatie-oplossing** pagina.

   Parameter|Description
   ---|---
   **Naam van de oplossing** | Deze unieke waarde wordt gebruikt om een nieuwe resourcegroep te maken. De vermelde Azure-resources worden gemaakt en toegewezen aan de resourcegroep.
   **Abonnement** | Geef hetzelfde abonnement als dat is gebruikt voor het maken van uw Time Series Insights-omgeving in de vorige sectie.
   **Regio** | Geef dezelfde regio die is gebruikt voor het maken van uw Time Series Insights-omgeving in de vorige sectie.
   **Optionele Azure-resources implementeren** | Laat **IoT-Hub** gecontroleerd. De gesimuleerde apparaten gebruiken om verbinding te maken voor het streamen van gegevens.

   Wanneer u klaar bent, selecteert u **oplossing maken** voor het inrichten van de oplossing Azure-resources. Het duurt 6-7 minuten om dit proces te voltooien.

   [![De simulatie apparaten inrichten](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png#lightbox)

1. Nadat het inrichten is voltooid, verandert de tekst boven de nieuwe oplossing van **Provisioning** naar **gereed**.

   >[!IMPORTANT]
   > Selecteer niet **starten** nog! Houd deze webpagina geopend, omdat u keert terug naar het later opnieuw.

   [![Simulatie van apparaten inrichten is voltooid](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png#lightbox)

1. Nu gaat u terug naar de Azure-portal en controleren van de zojuist gemaakte resources in uw abonnement. In de portal **resourcegroepen** pagina, ziet u dat een nieuwe resourcegroep is gemaakt met behulp van de **oplossingsnaam** opgegeven in de vorige stap. Let ook op alle resources die zijn gemaakt ter ondersteuning van de oplossing voor het simuleren van apparaten.

   [![Apparaatsimulatieoplossingsresources](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png)](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png#lightbox)

## <a name="connect-the-environment-to-the-iot-hub"></a>Verbinding maken met de omgeving naar de IoT hub

Op dit punt hebt u geleerd hoe u twee sets resources kunt maken, elk in hun eigen resourcegroep:

- Een lege Time Series Insights-omgeving.
- Simulatie oplossing apparaatbronnen, waaronder een IoT-hub, die worden gegenereerd door een oplossingsversnellers.

Bedenk dat de gesimuleerde apparaten verbinding moeten maken met een IoT-hub om apparaatgegevens te streamen. Als u wilt laten doorlopen de gegevens in de Time Series Insights-omgeving, moet u configuratiewijzigingen aanbrengt aan zowel uw IoT-hub en de Time Series Insights-omgeving.

### <a name="iot-hub-configuration-define-a-consumer-group"></a>Configuratie van de IoT-hub: Een consumergroep definiëren

IoT Hub biedt verschillende eindpunten voor het delen van de functionaliteit voor andere actoren. Het eindpunt '' gebeurtenissen biedt een manier voor andere toepassingen gebruiken voor gegevens zoals deze wordt gestreamd naar een IoT hub-instantie. Met name bieden "consumentengroepen" een mechanisme voor toepassingen om te luisteren en gegevens ophalen uit de IoT-hub.

Vervolgens definieert u een nieuwe **consumergroep** eigenschap op het apparaat simulatie oplossing van IoT-hub **eindpunt gebeurtenissen**.

1. In de Azure-portal, gaat u naar de **overzicht** pagina van de resourcegroep die u hebt gemaakt voor de oplossing voor het simuleren van apparaten. Selecteer de IoT hub-resource.

   [![Apparaatsimulatieoplossingsresourcegroep](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png#lightbox)

   Noteer de **naam** van de IoT Hub-resource die is gegenereerd voor de oplossing. U kunt hier later naar verwijzen.

1. Schuif omlaag en selecteer de **eindpunten** pagina en selecteer vervolgens de **gebeurtenissen** eindpunt. Op het eindpunt **eigenschappen** pagina, voer een unieke naam voor uw eindpunt onder de consumergroep '$Default'. Selecteer **Opslaan**.

   [![IoT-hub-eindpunten apparaatsimulatieoplossing](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png#lightbox)

### <a name="environment-configuration-define-an-event-source"></a>Configuratie van de omgeving: een gebeurtenisbron definiëren

Nu verbinding maken met de nieuwe IoT hub **consumergroep** gebeurtenis eindpunt Time Series Insights-omgeving als een **gebeurtenisbron**.

1. Ga naar de **overzicht** pagina van de resourcegroep die u hebt gemaakt voor de Time Series Insights-omgeving. Selecteer de Time Series Insights-omgeving.

   [![Resourcegroep voor Time Series Insights-omgeving en de omgeving](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png#lightbox)

1. Selecteer op de pagina van de Time Series Insights-omgeving **gebeurtenisbronnen**. Selecteer vervolgens **+ toevoegen**.

   [![Overzicht van Time Series Insights-omgeving](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png#lightbox)

1. Voer de vereiste parameters op de **nieuwe gebeurtenisbron** pagina.

   Parameter|Description
   ---|---
   **Naam van gebeurtenisbron** | Vereist een unieke waarde, die wordt gebruikt om de gebeurtenisbron een naam te geven.
   **Bron** | Selecteer **IoT-Hub**.
   **Importoptie** | Selecteer de standaard `Use IoT hub from available subscriptions`. Deze optie zorgt ervoor dat de volgende vervolgkeuzelijst om te worden ingevuld met de beschikbare abonnementen.
   **Abonnement** | Selecteer het hetzelfde abonnement waarin u de Time Series Insights-omgeving en de Apparaatsimulatie-resources gemaakt.
   **Naam van de IoT-hub** | Als het goed is, wordt standaard de naam van de IoT-hub die u eerder hebt genoteerd ingevuld. Als dat niet het geval is, selecteert u de juiste IoT-hub.
   **Naam van het IoT-hub-beleid** | Selecteer **iothubowner**.
   **IoT-hub-consumentengroep** | Als het goed is, wordt standaard de naam van de IoT-hub-consumentengroep ingevuld die u eerder hebt gemaakt. Als dat niet het geval is, selecteert u de naam van de juiste consumentengroep.
   **Serialisatie-indeling voor gebeurtenissen** | Behoud de rolledover waarde van `JSON`.
   **Naam van de timestamp-eigenschap** | Geef als `timestamp`.

   Wanneer u klaar bent, selecteert u **maken** om toe te voegen bron van de gebeurtenis. Als u terugkeert naar de resourcegroep **overzicht** pagina, samen met uw omgeving Time Series Insights-resource, ziet u een nieuwe 'Time Series Insights-gebeurtenisbron'-resource.

   [![Een time Series Insights-omgeving nieuwe gebeurtenisbron](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png#lightbox)

## <a name="run-device-simulation-to-stream-data"></a>Apparaatsimulatie uitvoeren om gegevens te streamen

Nu dat al het configuratiewerk is voltooid, is het tijd voor het vullen van de Time Series Insights-omgeving met voorbeeldgegevens uit de gesimuleerde apparaten.

U kunt intrekken van de [maakt u een apparaat simuleren-sectie](#create-a-device-simulation), verschillende Azure-resources zijn gemaakt door de accelerator voor de ondersteuning van de oplossing. Samen met de eerder besproken IoT-hub is een Azure App Service-webtoepassing gegenereerd om telemetrie voor gesimuleerde apparaten te maken en te verzenden.

1. Ga terug naar het dashboard [Oplossingsverbeteringen](https://www.azureiotsolutions.com/Accelerators#dashboard). Meld u opnieuw, indien nodig, met behulp van de dezelfde Azure-account die u hebt gebruikt in deze zelfstudie. Nu kunt u **starten** onder uw oplossing 'Apparaatsimulatie'.

     [![Dashboard van de oplossing accelerators](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png#lightbox)

1. De apparaat-simulatie web app wordt gestart op dit punt en het kunnen enkele seconden bij het initiële laden duren. U wordt ook gevraagd om toestemming te verlenen de web-App in de 'aanmelden en uw profiel te lezen' machtiging. Deze machtiging kan de toepassing om op te halen die nodig zijn voor de ondersteuning van de werking van de toepassing van gebruikersprofielgegevens.

     [![Apparaat simulatie web toepassing toestemming](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png)](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png#lightbox)

1. Na de **simulatie setup** pagina laadt, voer de vereiste parameters.

   Parameter|Description
   ---|---
   **Doel-IoT-hub** | Selecteer **Gebruik vooraf IoT Hub wordt ingericht**.
   **Apparaatmodel** | Selecteer **Koelunit**.
   **Aantal apparaten**  | Voer `1000` onder **bedrag**.
   **Telemetriefrequentie** | Voer `10` seconden.
   **Simulatieduur** | Selecteer **eindigen op:** en voer `5` minuten.

   Wanneer u klaar bent, selecteert u **simulatie starten**. De simulatie wordt uitgevoerd voor een totaal van vijf minuten. Deze gegevens gegenereerd van 1000 gesimuleerde apparaten elke 10 seconden. 

   [![Simulatie van de installatie van apparaat](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png#lightbox)

1. Terwijl de simulatie wordt uitgevoerd, ziet u dat de **totaal aantal berichten** en **berichten per seconde** velden bijwerken, ongeveer elke 10 seconden. De simulatie wordt beëindigd na ongeveer 5 minuten en gaat u terug naar **simulatie setup**.

   [![Apparaatsimulatie uitgevoerd](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png#lightbox)

## <a name="verify-the-telemetry-data"></a>De telemetriegegevens verifiëren

In deze laatste sectie controleert u dat de telemetrische gegevens is gegenereerd en in de Time Series Insights-omgeving opgeslagen. Voor het verifiëren van de gegevens gebruikt u de verkenner van Time Series Insights, die wordt gebruikt voor het opvragen en analyseren van telemetriegegevens.

1. Ga terug naar de resourcegroep van de Time Series Insights-omgeving **overzicht** pagina. Selecteer de Time Series Insights-omgeving.

   [![Resourcegroep voor Time Series Insights-omgeving en de omgeving](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png#lightbox)

1. In de Time Series Insights-omgeving **overzicht** weergeeft, schakelt de **Time Series Insights explorer-URL** om de Time Series Insights explorer te openen.

   [![Time Series Insights explorer](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png#lightbox)

1. De Time Series Insights explorer wordt geladen en verifieert met behulp van uw account voor Azure portal. Na de eerste weergave ziet u in het grafiekgebied dat de Time Series Insights-omgeving is ingevuld met gegevens van de gesimuleerde telemetrie. Om te filteren smaller tal van tijd, selecteer de vervolgkeuzelijst in de linkerbovenhoek. Geef een tijdsbereik die groot genoeg is voor de duur van de apparaatsimulatie omvatten. Selecteer vervolgens het Vergrootglas zoeken.

   [![Time Series Insights explorer filter tijdsbereik](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png#lightbox)

1. Het tijdsbereik verkleinen, kunt de grafiek om in te zoomen op de afzonderlijke pieken van gegevensoverdracht naar de IoT-hub en de Time Series Insights-omgeving. Ook ziet u de **Streaming is voltooid** tekst in de rechterbovenhoek, geeft het totale aantal gebeurtenissen gevonden. U kunt ook slepen de **intervalgrootte** schuifregelaar voor het beheren van de granulatie van de grafiek in de grafiek.

   [![Time Series Insights explorer tijdsbereik gefilterde weergave](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png#lightbox)

1. Ten slotte kunt u ook een regio om te filteren op een bereik klikken. Maak vervolgens met de rechtermuisknop op en gebruik **gebeurtenissen onderzoeken** om weer te geven van de details van gebeurtenis in de tabellaire **gebeurtenissen** weergeven.

   [![Time Series Insights explorer tijdsbereik weergeven en gebeurtenissen gefilterd](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png#lightbox)

## <a name="clean-up-resources"></a>Resources opschonen

Deze zelfstudie maakt u meerdere actief Azure-services ter ondersteuning van de Time Series Insights-omgeving en apparaat simulatie-oplossing. Als u wilt afbreken of uitstellen van uw werk op in deze reeks zelfstudies, verwijdert u alle resources om te voorkomen dat er onnodige kosten in rekening.

In het menu aan de linkerkant in Azure portal:

1. Selecteer de **resourcegroepen** pictogram. Selecteer vervolgens de resourcegroep die u hebt gemaakt voor de Time Series Insights-omgeving. Aan de bovenkant van de pagina, selecteer **resourcegroep verwijderen**, voer de naam van de resourcegroep en selecteer **verwijderen**.

1. Selecteer de **resourcegroepen** pictogram. Selecteer vervolgens de resourcegroep die is gemaakt door de oplossingsversneller voor simulatie van apparaat. Aan de bovenkant van de pagina, selecteer **resourcegroep verwijderen**, voer de naam van de resourcegroep en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Maak een Time Series Insights-omgeving.
> * Maak een simulatie van apparaten met een IoT-hub.
> * De Time Series Insights-omgeving verbinden met de IoT-hub.
> * Een apparaatsimulatie uitvoeren om gegevens te streamen in de Time Series Insights-omgeving.
> * Controleer of de gesimuleerde telemetrie-gegevens.

Nu dat u hoe u uw eigen Time Series Insights-omgeving maken weet, informatie over het bouwen van een webtoepassing die u gegevens van een Time Series Insights-omgeving gebruikt:

> [!div class="nextstepaction"]
> [Een web-app van Azure Time Series Insights met één pagina maken](tutorial-create-tsi-sample-spa.md)
