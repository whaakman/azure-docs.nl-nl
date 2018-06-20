---
title: Een Azure Time Series Insights-omgeving maken
description: Leer hoe u een Time Series Insights-omgeving kunt maken, gevuld met gegevens van gesimuleerde apparaten.
services: time-series-insights
author: BryanLa
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 06/04/2018
ms.author: bryanla
ms.openlocfilehash: 434f9f7890b909746fd1a0f72a2dea5d7bb27e92
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301351"
---
# <a name="tutorial-create-an-azure-time-series-insights-environment"></a>Zelfstudie: Een Azure Time Series Insights-omgeving maken

Deze zelfstudie begeleidt u bij het maken van een TSI-omgeving (Time Series Insight), gevuld met gegevens van gesimuleerde apparaten. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een TSI-omgeving maken 
> * Een apparaatsimulatieoplossing met een IoT Hub maken
> * De TSI-omgeving verbinden met de IoT-hub
> * Een apparaatsimulatie uitvoeren om gegevens naar de TSI-omgeving te streamen
> * De gesimuleerde telemetrie-gegevens verifiëren

## <a name="prerequisites"></a>Vereisten

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint. 

Uw Azure-aanmeldingsaccount moet ook lid zijn van de rol 'Eigenaar' van het abonnement. Zie [Toevoegen of wijzigen van de beheerders van een Azure-abonnement](/azure/billing/billing-add-change-azure-subscription-administrator)voor meer informatie

## <a name="overview"></a>Overzicht

In de TSI-omgeving worden apparaatgegevens verzameld en opgeslagen. Wanneer ze zijn opgeslagen in de TSI-omgeving kunt u de [TSI-verkenner](time-series-quickstart.md) en [TSI Query-API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) gebruiken om de gegevens op te vragen en te analyseren.

Net als alle apparaten, gesimuleerd of fysiek, is IoT Hub het verbindingspunt dat door apparaten wordt gebruikt om veilig verbinding te maken en gegevens naar de Azure-cloud te verzenden. Zoals besproken in het [TSI-overzicht](time-series-insights-overview.md), dient IoT Hub ook als een gebeurtenisbron voor het streamen van gegevens in de TSI-omgeving. 

Deze zelfstudie maakt ook gebruik van een [IoT-oplossingsverbetering](/azure/iot-accelerators/)om voorbeeldtelemetriegegevens te genereren en te streamen naar IoT Hub. IoT-oplossingsverbeteringen bieden vooraf geconfigureerde oplossingen van ondernemingsklasse, waarmee u de ontwikkeling van aangepaste IoT-oplossingen kunt versnellen. 

## <a name="create-a-tsi-environment"></a>Een TSI-omgeving maken

Maak eerst een TSI-omgeving in uw Azure-abonnement:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met uw Azure-abonnementsaccount.  
2. Selecteer linksboven **+ Een resource maken**.  
3. Selecteer de categorie **Internet of Things** en vervolgens **Time Series Insights**.  
   
   [![Selecteer de Time Series Insights-omgevingsresource](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png#lightbox)

4. Vul de vereiste parameters in op de pagina **Time Series Insights**:
   
   Parameter|Beschrijving
   ---|---
   **Omgevingsnaam** | Kies een unieke naam voor de TSI-omgeving. De naam wordt gebruikt door TSI-verkenner en de API-Query.
   **Abonnement** | Abonnementen zijn containers voor Azure-resources. Kies het abonnement waar u de TSI-omgeving wilt maken.
   **Resourcegroep** | Een resourcegroep is een container voor Azure-resources. Kies een bestaande resourcegroep voor de TSI-omgevingsresource of maak een nieuwe.
   **Locatie** | Kies een datacenterregio voor uw TSI-omgeving. Om extra bandbreedtekosten en latentie te voorkomen, is het het beste om de TSI-omgeving in dezelfde regio te houden als andere IoT-bronnen.
   **SKU-prijzen** | Kies de benodigde doorvoer. Selecteer S1 voor de laagste kosten en startcapaciteit.
   **Capaciteit** | Capaciteit is de vermenigvuldigingsfactor die wordt toegepast op de invoersnelheid, opslagcapaciteit en kosten die aan de geselecteerde SKU zijn verbonden.  U kunt de capaciteit van een omgeving wijzigen nadat deze is gemaakt. Selecteer een capaciteit van 1 voor de laagste kosten. 

   Als u klaar bent, klikt u op **Maken** om het inrichtingsproces te starten.

   ![Een Time Series Insights-omgevingsresource maken](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png)

5. U kunt het paneel **​​Meldingen** controleren om de voltooiing van de implementatie te controleren. Dit zou minder dan een minuut moeten duren:  

   ![Time Series Insights omgeving implementatie is voltooid](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png)

## <a name="create-a-device-simulation"></a>Een apparaatsimulatie maken

Maak vervolgens de oplossing voor apparaatsimulatie, waarmee testgegevens worden gegenereerd om uw TSI-omgeving te vullen:

1. Ga in een afzonderlijk venster/tabblad naar https://www.azureiotsolutions.com, log in met hetzelfde Azure-abonnementsaccount en selecteer de oplosssingsverbetering Apparaatsimulatie:

   ![De oplossingsverbetering Apparaatsimulatie uitvoeren](media/tutorial-create-populate-tsi-environment/sa-main.png)

2. Voer de vereiste parameters in op de pagina **Apparaatsimulatieoplossing maken**:

   Parameter|Beschrijving
   ---|---
   **Naam van de oplossing** | Een unieke waarde die wordt gebruikt voor het maken van een nieuwe resourcegroep. De vermelde Azure-resources worden gemaakt en toegewezen aan de resourcegroep.
   **Abonnement** | Geef hetzelfde abonnement op dat is gebruikt voor het maken van uw TSI-omgeving in de vorige sectie.
   **Regio** | Geef dezelfde regio op die is gebruikt voor het maken van uw TSI-omgeving in de vorige sectie. 
   **Optionele Azure-resources implementeren** | Laat **IoT Hub** aangevinkt, want de gesimuleerde apparaten gebruiken het om gegevens aan te sluiten/te streamen.

   Als u klaar bent, klikt u op **Oplossing maken** om de Azure-resources van de oplossing in te richten. Dit duurt ongeveer 6-7 minuten:

   ![De apparaatsimulatieoplossing inrichten](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png)

3. Zodra het inrichten is voltooid, verandert de tekst boven uw nieuwe oplossing van 'Inrichten...' in 'Gereed':

   >[!IMPORTANT]
   > Klik nog niet op de knop **Starten**! Houd deze webpagina wel open, omdat u er later naar terugkeert.

   ![Het inrichten van de apparaatsimulatieoplossing is voltooid](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png)

4. Ga nu terug naar de Azure-portal en bekijk de nieuw gemaakte resources in uw abonnement. Op de pagina **Resourcegroepen** pagina van de portal ziet u dat er een nieuwe resourcegroep is gemaakt met behulp van de **Oplossingsnaam** in de laatste stap. Merk ook alle resources op die zijn gemaakt om de apparaatsimulatieoplossing te ondersteunen:

   [![Apparaatsimulatieoplossingsresources](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png)](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png#lightbox)

## <a name="connect-the-tsi-environment-to-the-iot-hub"></a>De TSI-omgeving verbinden met de IoT-hub 

Op dit punt hebt u geleerd hoe u twee sets resources kunt maken, elk in hun eigen resourcegroep: 
- Een lege TSI-omgeving 
- Apparaatsimulatieoplossingsresources, inclusief een IoT-hub, gegenereerd door een oplossingsverbetering

Bedenk dat de gesimuleerde apparaten verbinding moeten maken met een IoT-hub om apparaatgegevens te streamen. Voor het doorsturen van de gegevens naar de TSI-omgeving moet u configuratiewijzigingen aanbrengen in zowel uw IoT-hub als de TSI-omgeving. 

### <a name="iot-hub-configuration-define-a-consumer-group"></a>IoT-hubconfiguratie: een consumentengroep definiëren

IoT Hub biedt verschillende eindpunten om functionaliteit te delen met andere actoren. Het eindpunt ‘Gebeurtenissen’ biedt een manier voor andere toepassingen om gegevens te gebruiken, omdat deze worden gestreamd naar een IoT-hubinstantie. Met name 'consumentengroepen' bieden een mechanisme voor toepassingen om te luisteren en gegevens van de IoT Hub te halen. 

Vervolgens definieert u een nieuwe ‘consumentengroep’-eigenschap op het eindpunt "Gebeurtenissen" van de IoT-hub van de apparaatsimulatieoplossing. 

1. Ga in de Azure-portal naar de pagina **Overzicht** van de resourcegroep die u hebt gemaakt voor de apparaatsimulatieoplossing en selecteer vervolgens de IoT Hub-resource:

   [![Apparaatsimulatieoplossingsresourcegroep](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png#lightbox)

   Noteer ook de **Naam** van de IoT Hub-resource die voor de oplossing is gegenereerd, want u hebt deze later nodig.

2. Blader omlaag en selecteer de pagina **Eindpunten** en selecteer vervolgens het eindpunt **Gebeurtenissen**. Voer op de eindpuntpagina **Eigenschappen** een unieke naam in voor uw eindpunt onder de consumentengroep '$Default' en klik vervolgens op **Opslaan**:

   [![IoT-hub-eindpunten apparaatsimulatieoplossing](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png#lightbox)

### <a name="tsi-environment-configuration-define-an-event-source"></a>TSI-omgevingsconfiguratie: een gebeurtenisbron definiëren

Verbind nu het nieuwe eindpunt van de gebeurtenisgroep ‘consumentengroep’ van de IoT hub met de TSI-omgeving als een ‘gebeurtenisbron’.

1. Ga naar de pagina **Overzicht** van de resourcegroep die u hebt gemaakt voor de TSI-omgeving en selecteer vervolgens de TSI-omgeving:

   [![TSI-omgevingsresourcegroep en omgeving](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png#lightbox)

2. Selecteer **Gebeurtenisbronnen**op de TSI-omgevingspagina en klik vervolgens op **+ Toevoegen**:

   ![Overzicht van TSI-omgeving](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png)

3. Voer de vereiste parameters in op de pagina **Nieuwe gebeurtenisbron**: 

   Parameter|Beschrijving
   ---|---
   **Naam van gebeurtenisbron** | Vereist een unieke waarde, die wordt gebruikt om de gebeurtenisbron een naam te geven.
   **Bron** | Selecteer ‘IoT Hub’.
   **Importoptie** | Gebruik de standaardinstelling ‘IoT Hub uit de beschikbare abonnementen gebruiken’. Deze optie zorgt ervoor dat de volgende vervolgkeuzelijst wordt gevuld met de beschikbare abonnementen.
   **Abonnement** | Selecteer hetzelfde abonnement waarin u de TSI-omgeving en Device Simulation-resources hebt gemaakt.
   **Naam van de IoT-hub** | Als het goed is, wordt standaard de naam van de IoT-hub die u eerder hebt genoteerd ingevuld. Als dat niet het geval is, selecteert u de juiste IoT-hub.
   **Naam van het IoT-hub-beleid** | Laat de standaardwaarde 'iothubowner' staan.
   **IoT-hub-consumentengroep** | Als het goed is, wordt standaard de naam van de IoT-hub-consumentengroep ingevuld die u eerder hebt gemaakt. Als dat niet het geval is, selecteert u de naam van de juiste consumentengroep. 
   **Serialisatie-indeling voor gebeurtenissen** | Laat de standaardwaarde 'JSON' staan.
   **Naam van de timestamp-eigenschap** | Geef 'timestamp' op.

   Klik als u klaar bent op **Maken** om de gebeurtenisbron toe te voegen. Wanneer u terugkeert naar de pagina **Overzicht** van de resourcegroep, ziet u bij uw TSI-omgevingsresource de nieuwe resource ‘Gebeurtenisbron voor Time Series Insights’.

   ![Nieuwe gebeurtenisbron van TSI omgeving](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png)

## <a name="run-a-device-simulation-to-stream-data-into-the-tsi-environment"></a>Een apparaatsimulatie uitvoeren om gegevens naar de TSI-omgeving te streamen

Nu alle configuratiewerkzaamheden zijn voltooid, is het tijd om de TSI-omgeving te vullen met voorbeeldgegevens van de gesimuleerde apparaten.

U zult zich herinneren dat in het gedeelte [Een apparaatsimulatie maken](#create-a-device-simulation) verschillende Azure-bronnen zijn gemaakt met de oplossingsverbetering om de oplossing te ondersteunen. Samen met de eerder besproken IoT-hub is een Azure App Service-webtoepassing gegenereerd om telemetrie voor gesimuleerde apparaten te maken en te verzenden.

1. Ga terug naar het dashboard [Oplossingsverbeteringen](https://www.azureiotsolutions.com/Accelerators#dashboard). Meld u indien nodig opnieuw aan met hetzelfde Azure-account dat u in deze zelfstudie hebt gebruikt. Nu kunt u op de knop **Starten** onder uw oplossing ‘Apparaatsimulatie’ klikken:

     ![Dashboard Oplossingsverbeteringen](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png)

2. Op dit punt wordt de web-app voor apparaatsimulatie gestart. Bij de eerste keer laden kan dit enkele seconden duren. U wordt ook gevraagd om de webtoepassing de machtiging ‘Aanmelden en uw profiel lezen’ te verlenen. Met deze machtiging kan de toepassing de gebruikersprofielinformatie ophalen die nodig is om de werking van de toepassing te ondersteunen:

     ![Toestemming voor webtoepassing voor apparaatsimulatie](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png)

3. Nadat de pagina **Simulatie-instellingen** is geladen, voert u de vereiste parameters in: 

   Parameter|Beschrijving
   ---|---
   **Doel-IoT-hub** | Selecteer ‘Vooraf ingerichte IoT-hub gebruiken’.
   **Apparaatmodel** | Selecteer ‘Koelinstallatie’.
   **Aantal apparaten**  | Voer 1000 in onder **Aantal**.
   **Telemetriefrequentie** | Voer 10 seconden in.
   **Simulatieduur** | Selecteer **Eindigen over:** en voer 5 minuten in.

   Wanneer u klaar bent, klikt u op **Simulatie starten**. De simulatie wordt gedurende een totaal van 5 minuten uitgevoerd en genereert elke 10 seconden gegevens van 1000 gesimuleerde apparaten.  

   ![Apparaatsimulatie instellen](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png)

4. Terwijl de simulatie wordt uitgevoerd, ziet u dat de velden **Totaal berichten** en **Berichten per seconde** worden bijgewerkt, ongeveer om de 10 seconden. De simulatie eindigt na ongeveer 5 minuten en keert terug naar **Simulatie-instellingen**.

   ![Apparaatsimulatie wordt uitgevoerd](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png)

## <a name="verify-the-telemetry-data"></a>De telemetriegegevens verifiëren

In dit laatste gedeelte verifieert u dat de telemetriegegevens zijn gegenereerd en opgeslagen in de TSI-omgeving. Voor het verifiëren van de gegevens gebruikt u de verkenner van Time Series Insights, die wordt gebruikt voor het opvragen en analyseren van telemetriegegevens.

1. Ga terug naar de pagina **Overzicht** van de resourcegroep van de TSI-omgeving en selecteer de TSI-omgeving opnieuw:

   [![TSI-omgevingsresourcegroep en omgeving](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png#lightbox)

2. Klik op de pagina **Overzicht** van de TSI-omgeving op de **URL voor Time Series Insights Explorer** om de TSI-verkenner te openen:

   [![TSI-verkenner](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png#lightbox)

3. De TSI-verkenner wordt geladen en geverifieerd met uw Azure-portalaccount. Bij de eerste weergave ziet u in het grafiekgebied dat de TSI-omgeving inderdaad is gevuld met gesimuleerde telemetriegegevens. Als u een kleiner tijdsbereik wilt filteren, selecteert u de vervolgkeuzelijst linksboven. Voer vervolgens een tijdsbereik in dat groot genoeg is om de duur van de apparaatsimulatie te overbruggen. Klik vervolgens op het vergrootglas voor zoeken:

   [![Tijdfilter TSI-verkenner](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png#lightbox)

4. Door het tijdsbereik te toe te spitsen kan de grafiek inzoomen op de afzonderlijke bursts van gegevensoverdracht naar de IoT-hub en de TSI-omgeving. Let ook op de tekst **Streaming voltooid** in de rechterbovenhoek, waar het totale aantal gevonden gebeurtenissen wordt weergegeven. U kunt ook de schuifregelaar **Intervalgrootte** slepen om de granulariteit van de grafiek te bepalen:

   [![TSI-verkenner met gefilterd tijdsbereik](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png#lightbox)

5. Ten slotte kunt u ook met de linkermuisknop op een regio klikken om een ​​bereik te filteren en vervolgens met de rechtermuisknop klikken om **Gebeurtenissen verkennen** te gebruiken om gebeurtenisdetails weer te geven in de tabelweergave **Gebeurtenissen**:

   [![TSI-verkenner met gefilterd tijdsbereik en gebeurtenissen](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png#lightbox)

## <a name="clean-up-resources"></a>Resources opschonen

In deze zelfstudie worden verschillende werkende Azure-services gemaakt om de TSI-omgeving en de apparaatsimulatie-oplossing te ondersteunen. Als u deze zelfstudiereeks wilt afbreken en/of de voltooiing ervan wilt uitstellen, raden we u aan alle resources te verwijderen, om onnodige kosten te voorkomen. 

In het linkermenu van de Azure-portal:

1. Klik op het pictogram **Resourcegroepen** en selecteer de resourcegroep die u hebt gemaakt voor de TSI-omgeving. Klik bovenaan de pagina op **Resourcegroep verwijderen**, voer de naam van de resourcegroep in en klik op **Verwijderen**. 
2. Klik op het pictogram **Resourcegroepen** en selecteer de resourcegroep die is gemaakt met de oplossingsverbetering voor apparaatsimulatie. Klik bovenaan de pagina op **Resourcegroep verwijderen**, voer de naam van de resourcegroep in en klik op **Verwijderen**. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een TSI-omgeving maken 
> * Een apparaatsimulatieoplossing met een IoT Hub maken
> * De TSI-omgeving verbinden met de IoT-hub
> * Een apparaatsimulatie uitvoeren om gegevens naar de TSI-omgeving te streamen
> * De gesimuleerde telemetrie-gegevens verifiëren

Nu u weet hoe u uw eigen TSI-omgeving kunt maken, kunt u meer te weten komen over het planningsproces door naar het volgende artikel te gaan:

> [!div class="nextstepaction"]
> [Uw Azure Time Series Insights-omgeving plannen](time-series-insights-environment-planning.md)


