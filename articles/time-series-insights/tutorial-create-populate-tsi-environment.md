---
title: 'Zelfstudie: Een Azure Time Series Insights-omgeving maken | Microsoft Docs'
description: Leer hoe u een Time Series Insights-omgeving kunt maken, gevuld met gegevens van gesimuleerde apparaten.
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 04/26/2019
ms.author: anshan
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: 3c097ec74f45248a41c6dd4df80cbbd927a9b5ed
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827447"
---
# <a name="tutorial-create-an-azure-time-series-insights-environment"></a>Zelfstudie: Een Azure Time Series Insights-omgeving maken

In deze zelfstudie leidt u door het proces voor het maken van een omgeving met Time Series Insight, ingevuld met gegevens uit de gesimuleerde apparaten. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Time Series Insights-omgeving maken 
> * Een apparaatsimulatieoplossing met een IoT Hub maken
> * Verbinding maken met de Time Series Insights-omgeving naar de IoT hub
> * Een apparaatsimulatie uitvoeren om gegevens te streamen in de Time Series Insights-omgeving
> * De gesimuleerde telemetrie-gegevens verifiëren

## <a name="video"></a>Video

### <a name="learn-how-to-use-an-azure-iot-solution-accelerator-to-generate-data-and-get-started-with-time-series-insights-br"></a>Informatie over het gebruik van een Azure IoT-oplossingsversnellers voor het genereren van gegevens en aan de slag met Time Series Insights. </br>

> [!VIDEO https://www.youtube.com/embed/6ehNf6AJkFo]

## <a name="prerequisites"></a>Vereisten

* Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/).

* Uw Azure-aanmelden-account moet ook lid zijn van van het abonnement **eigenaar** rol. Zie [toegang met RBAC en de Azure-portal beheren](/azure/role-based-access-control/role-assignments-portal) voor meer informatie.

## <a name="overview"></a>Overzicht

De Time Series Insights-omgeving is waar de apparaatgegevens worden verzameld en opgeslagen. Wanneer deze zijn opgeslagen in de Time Series Insights-omgeving, de [Time Series Insights Explorer](time-series-quickstart.md) en [Time Series Insights Query-API](/rest/api/time-series-insights/ga-query-api) kan worden gebruikt om te zoeken en analyseren van de gegevens. IoT Hub is het verbindingspunt wordt gebruikt door alle apparaten (gesimuleerde of fysieke) veilig verbinding maken met en gegevens verzenden naar de Azure-cloud. De [Time Series Insights-overzicht](time-series-insights-overview.md) opmerkingen bij de dat Azure IoT Hub ook als een gebeurtenisbron fungeert voor het streamen van gegevens naar een Time Series Insights-omgeving. In deze zelfstudie wordt een [IoT-oplossingsversnellers](/azure/iot-accelerators/) te genereren en het streamen van voorbeeldtelemetrie die naar IoT Hub.

>[!TIP]
> IoT-oplossingsversnellers bieden geavanceerde vooraf geconfigureerde oplossingen zodat u sneller kunt ontwikkelen van aangepaste IoT-oplossingen.

## <a name="create-an-environment"></a>Een omgeving maken

Maak eerst een Time Series Insights-omgeving in uw Azure-abonnement:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met uw Azure-abonnementsaccount.  
1. Selecteer linksboven **+ Een resource maken**.  
1. Selecteer de categorie **Internet of Things** en vervolgens **Time Series Insights**.  

   [![Selecteer de Time Series Insights-omgevingsresource](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png#lightbox)

1. Vul de vereiste parameters in op de pagina **Time Series Insights**:

   Parameter|Description
   ---|---
   **Omgevingsnaam** | Kies een unieke naam voor de Time Series Insights-omgeving. De naam van de door Time Series Insights Explorer en Query-API's gebruikt.
   **Abonnement** | Abonnementen zijn containers voor Azure-resources. Kies een abonnement te maken van de Time Series Insights-omgeving voor.
   **Resourcegroep** | Een resourcegroep is een container voor Azure-resources. Kies een bestaande resourcegroep of maak een nieuw wachtwoord voor de resource van Time Series Insights-omgeving.
   **Locatie** | Kies een regio data center voor uw Time Series Insights-omgeving. Om te voorkomen dat extra bandbreedtekosten en latentie, is het raadzaam te houden met de Time Series Insights-omgeving in dezelfde regio als andere IoT-resources.
   **SKU-prijzen** | Kies de benodigde doorvoer. Selecteer voor de laagste kosten en starter capaciteit, `S1`.
   **Capaciteit** | Capaciteit is de vermenigvuldigingsfactor die wordt toegepast op de invoersnelheid, opslagcapaciteit en kosten die aan de geselecteerde SKU zijn verbonden.  U kunt de capaciteit wijzigen nadat de is gemaakt. Voor de laagste kosten, selecteert u een capaciteit van 1.

   Als u klaar bent, klikt u op **Maken** om het inrichtingsproces te starten.

   [![Een resource van Time Series Insights-omgeving maken](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png#lightbox)

1. Controleer de **meldingen** deelvenster voor het bewaken van de implementatie is voltooid.  

   [![Time Series Insights-omgeving de implementatie is voltooid](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png#lightbox)

## <a name="create-a-device-simulation"></a>Een apparaatsimulatie maken

Maak vervolgens de oplossing voor het simulatie van apparaten, die wordt gegenereerd testgegevens om in te vullen uw Time Series Insights-omgeving:

1. In een afzonderlijk venster/tabblad, gaat u naar [azureiotsolutions.com](https://www.azureiotsolutions.com). Meld u aan met hetzelfde account Azure-abonnement en selecteer de **Apparaatsimulatie** accelerator.

   [![Voer de Apparaatsimulatie-accelerator](media/tutorial-create-populate-tsi-environment/sa-main.png)](media/tutorial-create-populate-tsi-environment/sa-main.png#lightbox)

1. Voer de vereiste parameters op de **maken Apparaatsimulatie-oplossing** pagina.

   Parameter|Description
   ---|---
   **Naam van de oplossing** | Een unieke waarde die wordt gebruikt voor het maken van een nieuwe resourcegroep. De vermelde Azure-resources worden gemaakt en toegewezen aan de resourcegroep.
   **Abonnement** | Geef het hetzelfde abonnement gebruikt voor het maken van uw Time Series Insights-omgeving, in de vorige sectie.
   **Regio** | Geef op de dezelfde regio die wordt gebruikt voor het maken van uw Time Series Insights-omgeving, in de vorige sectie.
   **Optionele Azure-resources implementeren** | Laat **IoT Hub** aangevinkt, want de gesimuleerde apparaten gebruiken het om gegevens aan te sluiten/te streamen.

   Wanneer u klaar bent, klikt u op **oplossing maken** voor het inrichten van de oplossing Azure-resources. Het duurt 6-7 minuten om dit proces te voltooien.

   [![De simulatie apparaten inrichten](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png#lightbox)

1. Als inrichting is voltooid, de tekst boven de nieuwe oplossing wordt gewijzigd van **inrichten...**  naar **gereed**:

   >[!IMPORTANT]
   > Klik nog niet op de knop **Starten**! Houd deze webpagina wel open, omdat u er later naar terugkeert.

   [![Simulatie van apparaten inrichten is voltooid](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png#lightbox)

1. Ga nu terug naar de Azure-portal en bekijk de nieuw gemaakte resources in uw abonnement. Op de pagina **Resourcegroepen** pagina van de portal ziet u dat er een nieuwe resourcegroep is gemaakt met behulp van de **Oplossingsnaam** in de laatste stap. Merk ook alle resources op die zijn gemaakt om de apparaatsimulatieoplossing te ondersteunen:

   [![Apparaatsimulatieoplossingsresources](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png)](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png#lightbox)

## <a name="connect-the-environment-to-the-iot-hub"></a>Verbinding maken met de omgeving naar de IoT hub

Op dit punt hebt u geleerd hoe u twee sets resources kunt maken, elk in hun eigen resourcegroep:

- Een lege Time Series Insights-omgeving.
- Simulatie oplossing apparaatbronnen, met inbegrip van een IoT-hub, die worden gegenereerd door een oplossingsversnellers.

Bedenk dat de gesimuleerde apparaten verbinding moeten maken met een IoT-hub om apparaatgegevens te streamen. Als u wilt laten doorlopen de gegevens in de Time Series Insights-omgeving, moet u configuratiewijzigingen aanbrengt aan uw IoT-hub en de Time Series Insights-omgeving.

### <a name="iot-hub-configuration-define-a-consumer-group"></a>IoT-hubconfiguratie: een consumentengroep definiëren

IoT Hub biedt verschillende eindpunten om functionaliteit te delen met andere actoren. Het eindpunt ‘Gebeurtenissen’ biedt een manier voor andere toepassingen om gegevens te gebruiken, omdat deze worden gestreamd naar een IoT-hubinstantie. Met name 'consumentengroepen' bieden een mechanisme voor toepassingen om te luisteren en gegevens van de IoT Hub te halen.

Vervolgens definieert u een nieuwe **consumergroep** eigenschap op het apparaat simulatie oplossing van IoT-hub **eindpunt gebeurtenissen**.

1. Ga in de Azure-portal naar de pagina **Overzicht** van de resourcegroep die u hebt gemaakt voor de apparaatsimulatieoplossing en selecteer vervolgens de IoT Hub-resource:

   [![Apparaatsimulatieoplossingsresourcegroep](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png#lightbox)

   Noteer ook de **Naam** van de IoT Hub-resource die voor de oplossing is gegenereerd, want u hebt deze later nodig.

1. Blader omlaag en selecteer de pagina **Eindpunten** en selecteer vervolgens het eindpunt **Gebeurtenissen**. Voer op de eindpuntpagina **Eigenschappen** een unieke naam in voor uw eindpunt onder de consumentengroep '$Default' en klik vervolgens op **Opslaan**:

   [![IoT-hub-eindpunten apparaatsimulatieoplossing](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png#lightbox)

### <a name="environment-configuration-define-an-event-source"></a>Configuratie van de omgeving: een gebeurtenisbron definiëren

Nu verbinding maken met de nieuwe IoT hub **consumergroep** gebeurtenis eindpunt, de Time Series Insights-omgeving, als een **gebeurtenisbron**.

1. Ga naar de **overzicht** pagina van de resourcegroep die u hebt gemaakt voor de Time Series Insights-omgeving, en vervolgens selecteert u de Time Series Insights-omgeving:

   [![Resourcegroep voor Time Series Insights-omgeving en de omgeving](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png#lightbox)

1. Selecteer op de pagina van de Time Series Insights-omgeving **gebeurtenisbronnen**, klikt u vervolgens op **+ toevoegen**.

   [![Overzicht van Time Series Insights-omgeving](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png#lightbox)

1. Voer de vereiste parameters op de **nieuwe gebeurtenisbron** pagina.

   Parameter|Description
   ---|---
   **Naam van gebeurtenisbron** | Vereist een unieke waarde, die wordt gebruikt om de gebeurtenisbron een naam te geven.
   **Bron** | Selecteer **IoT-Hub**.
   **Importoptie** | Selecteer de standaard `Use IoT hub from available subscriptions`. Deze optie zorgt ervoor dat de volgende vervolgkeuzelijst wordt gevuld met de beschikbare abonnementen.
   **Abonnement** | Selecteer het hetzelfde abonnement waarin u de Time Series Insights-omgeving en de Apparaatsimulatie-resources gemaakt.
   **Naam van de IoT-hub** | Als het goed is, wordt standaard de naam van de IoT-hub die u eerder hebt genoteerd ingevuld. Als dat niet het geval is, selecteert u de juiste IoT-hub.
   **Naam van het IoT-hub-beleid** | Selecteer **iothubowner**.
   **IoT-hub-consumentengroep** | Als het goed is, wordt standaard de naam van de IoT-hub-consumentengroep ingevuld die u eerder hebt gemaakt. Als dat niet het geval is, selecteert u de naam van de juiste consumentengroep.
   **Serialisatie-indeling voor gebeurtenissen** | Behoud de rolledover waarde van `JSON`.
   **Naam van de timestamp-eigenschap** | Geef als `timestamp`.

   Klik als u klaar bent op **Maken** om de gebeurtenisbron toe te voegen. Als u terugkeert naar de resourcegroep **overzicht** pagina, samen met uw omgeving Time Series Insights-resource, ziet u een nieuwe 'Time Series Insights-gebeurtenisbron'-resource.

   [![Een time Series Insights-omgeving nieuwe gebeurtenisbron](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png#lightbox)

## <a name="run-device-simulation-to-stream-data"></a>Apparaatsimulatie uitvoeren om gegevens te streamen

Nu dat al het configuratiewerk is voltooid, is het tijd voor het vullen van de Time Series Insights-omgeving met voorbeeldgegevens uit de gesimuleerde apparaten.

U zult zich herinneren dat in het gedeelte [Een apparaatsimulatie maken](#create-a-device-simulation) verschillende Azure-bronnen zijn gemaakt met de oplossingsverbetering om de oplossing te ondersteunen. Samen met de eerder besproken IoT-hub is een Azure App Service-webtoepassing gegenereerd om telemetrie voor gesimuleerde apparaten te maken en te verzenden.

1. Ga terug naar het dashboard [Oplossingsverbeteringen](https://www.azureiotsolutions.com/Accelerators#dashboard). Meld u indien nodig opnieuw aan met hetzelfde Azure-account dat u in deze zelfstudie hebt gebruikt. Nu kunt u klikken op de **starten** knop onder uw oplossing 'Apparaatsimulatie'.

     [![Dashboard van de oplossing accelerators](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png#lightbox)

1. Op dit punt wordt de web-app voor apparaatsimulatie gestart. Bij de eerste keer laden kan dit enkele seconden duren. U wordt ook gevraagd om de webtoepassing de machtiging ‘Aanmelden en uw profiel lezen’ te verlenen. Deze machtiging kan de toepassing om op te halen die nodig zijn voor de ondersteuning van de werking van de toepassing van gebruikersprofielgegevens.

     [![Apparaat simulatie web toepassing toestemming](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png)](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png#lightbox)

1. Zodra de **simulatie setup** pagina laadt, voer de vereiste parameters.

   Parameter|Description
   ---|---
   **Doel-IoT-hub** | Selecteer **Gebruik vooraf IoT Hub wordt ingericht**.
   **Apparaatmodel** | Selecteer **Koelunit**.
   **Aantal apparaten**  | Voer `1000` onder **bedrag**.
   **Telemetriefrequentie** | Voer `10` seconden.
   **Simulatieduur** | Selecteer **eindigen op:** en voer `5` minuten.

   Wanneer u klaar bent, klikt u op **Simulatie starten**. De simulatie wordt gedurende een totaal van 5 minuten uitgevoerd en genereert elke 10 seconden gegevens van 1000 gesimuleerde apparaten.  

   [![Simulatie van de installatie van apparaat](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png#lightbox)

1. Terwijl de simulatie wordt uitgevoerd, ziet u dat de velden **Totaal berichten** en **Berichten per seconde** worden bijgewerkt, ongeveer om de 10 seconden. De simulatie eindigt na ongeveer 5 minuten en keert terug naar **Simulatie-instellingen**.

   [![Apparaatsimulatie uitgevoerd](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png#lightbox)

## <a name="verify-the-telemetry-data"></a>De telemetriegegevens verifiëren

In deze laatste sectie controleert u dat de telemetrische gegevens is gegenereerd en in de Time Series Insights-omgeving opgeslagen. Voor het verifiëren van de gegevens gebruikt u de verkenner van Time Series Insights, die wordt gebruikt voor het opvragen en analyseren van telemetriegegevens.

1. Ga terug naar de resourcegroep van de Time Series Insights-omgeving **overzicht** pagina. Selecteer de Time Series Insights-omgeving.

   [![Resourcegroep voor Time Series Insights-omgeving en de omgeving](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png#lightbox)

1. In de Time Series Insights-omgeving **overzicht** pagina, klikt u op de **Time Series Insights explorer-URL** om de Time Series Insights explorer te openen.

   [![Time Series Insights explorer](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png#lightbox)

1. Time Series Insights explorer wordt geladen en verifiëren met behulp van uw account voor Azure portal. Na de eerste weergave ziet u in het grafiekgebied dat de Time Series Insights-omgeving inderdaad is ingevuld met gegevens van de gesimuleerde telemetrie. Als u een kleiner tijdsbereik wilt filteren, selecteert u de vervolgkeuzelijst linksboven. Voer vervolgens een tijdsbereik in dat groot genoeg is om de duur van de apparaatsimulatie te overbruggen. Klik vervolgens op het vergroten van de klasse zoeken.

   [![Time Series Insights explorer filter tijdsbereik](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png#lightbox)

1. Het tijdsbereik verkleinen, kunt de grafiek om in te zoomen op de afzonderlijke pieken van de gegevensoverdracht naar de IoT hub en de Time Series Insights-omgeving. Let ook op de tekst **Streaming voltooid** in de rechterbovenhoek, waar het totale aantal gevonden gebeurtenissen wordt weergegeven. U kunt ook slepen de **intervalgrootte** schuifregelaar voor het beheren van de granulatie van de grafiek in de grafiek.

   [![Time Series Insights explorer tijdsbereik gefilterde weergave](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png#lightbox)

1. Ten slotte kunt u ook een regio voor het filteren van een bereik, met de rechtermuisknop op en gebruik linkermuisknop **gebeurtenissen onderzoeken** om weer te geven van de details van gebeurtenis in de tabellaire **gebeurtenissen** weergeven.

   [![Time Series Insights explorer tijdsbereik weergeven en gebeurtenissen gefilterd](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png#lightbox)

## <a name="clean-up-resources"></a>Resources opschonen

Deze zelfstudie maakt u meerdere actief Azure-services, ter ondersteuning van de Time Series Insights-omgeving en apparaat simulatie-oplossing. Als u deze zelfstudiereeks wilt afbreken en/of de voltooiing ervan wilt uitstellen, raden we u aan alle resources te verwijderen, om onnodige kosten te voorkomen.

In het linkermenu van de Azure-portal:

1. Klik op de **resourcegroepen** pictogram en selecteer vervolgens de resourcegroep die u hebt gemaakt voor de Time Series Insights-omgeving. Klik bovenaan de pagina op **Resourcegroep verwijderen**, voer de naam van de resourcegroep in en klik op **Verwijderen**.

1. Klik op het pictogram **Resourcegroepen** en selecteer de resourcegroep die is gemaakt met de oplossingsverbetering voor apparaatsimulatie. Aan de bovenkant van de pagina, klikt u op **resourcegroep verwijderen**, voer de naam van de resourcegroep en klik vervolgens op **verwijderen**

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een Time Series Insights-omgeving maken 
> * Een apparaatsimulatieoplossing met een IoT Hub maken
> * Verbinding maken met de Time Series Insights-omgeving naar de IoT hub
> * Een apparaatsimulatie uitvoeren om gegevens te streamen in de Time Series Insights-omgeving
> * De gesimuleerde telemetrie-gegevens verifiëren

Nu dat u hoe u uw eigen Time Series Insights-omgeving maken weet, informatie over het bouwen van een webtoepassing die u gegevens van een Time Series Insights-omgeving gebruikt:

> [!div class="nextstepaction"]
> [Een web-app van Azure Time Series Insights met één pagina maken](tutorial-create-tsi-sample-spa.md)
