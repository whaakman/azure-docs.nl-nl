---
title: Azure Time Series Insights Preview instellen - Een zelfstudie waarin u een Azure Time Series Insights Preview-omgeving instelt | Microsoft Docs
description: Informatie over hoe u een omgeving instelt in Azure Time Series Insights Preview.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 12/12/2018
ms.custom: seodec18
ms.openlocfilehash: 9ad957d6378b1279f1ca51939eb4802b0ce7d78f
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53322615"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Zelfstudie: Een Time Series Insights Preview-omgeving instellen

In deze zelfstudie wordt u door het creatieproces van een Azure Time Series Insights Preview-omgeving op basis van betalen per gebruik geleid. In deze zelfstudie leert u het volgende:

* Een Time Series Insights Preview-omgeving maken.
* De Azure Time Series Insights Preview-omgeving verbinden met een Event Hub in Azure Event Hubs.
* Een windparksimulatie uitvoeren om gegevens te streamen naar de Azure Time Series Insights Preview-omgeving.
* Voer een eenvoudige analyse van de gegevens uit.
* Een type Time Series Model en een hiërarchie definiëren en deze koppelen aan uw exemplaren.

# <a name="create-a-device-simulation"></a>Een apparaatsimulatie maken

In dit deel maakt u drie gesimuleerde apparaten die gegevens verzenden naar een IoT Hub.

1. Ga naar de startpagina van [Azure IoT-oplossingsverbeteringen](https://www.azureiotsolutions.com/Accelerators). Op de startpagina van Azure IoT-oplossingsverbeteringen worden verschillende vooraf gemaakte voorbeelden weergegeven. Meld u aan met uw Azure-account. Selecteer vervolgens **Apparaatsimulatie**.

   ![Startpagina Azure IoT-oplossingsverbeteringen][1]

   Klik ten slotte op **Nu proberen**.

1. Voer de vereiste parameters in op de pagina **Apparaatsimulatie maken**:

   | Parameter | Beschrijving |
   | --- | --- |
   | Naam van de oplossing |    Een unieke waarde die wordt gebruikt voor het maken van een nieuwe resourcegroep. De vermelde Azure-resources zijn | gemaakt en toegewezen aan de resourcegroep. |
   | Abonnement | Geef hetzelfde abonnement op dat is gebruikt voor het maken van uw TSI-omgeving |
   | Regio |   Geef dezelfde regio op die is gebruikt voor het maken van uw TSI. |
   | Optionele Azure-resources implementeren    | Laat IoT Hub aangevinkt, want de gesimuleerde apparaten gebruiken het om gegevens aan te sluiten/te streamen. |

   Klik nadat u de vereiste parameters hebt ingevoerd op **Oplossing maken**. Wacht ongeveer 10-15 minuten voor uw oplossing worden geïmplementeerd.

   ![Oplossing voor apparaatsimulatie maken][2]

1. In uw **Dashboard voor oplossingsverbeteringen** klikt u op de knop **Starten**:

   ![Oplossing voor apparaatsimulatie starten][3]

1. U wordt omgeleid naar de pagina **Microsoft Azure IoT-apparaatsimulatie**. Klik op **+ Nieuwe simulatie** rechtsboven in het scherm.

   ![Pagina van Azure IoT-simulatie][4]

1.  Vul de vereiste parameters als volgt in:

    ![In te vullen parameters][5]

    |||
    | --- | --- |
    | **Naam** | Voer een unieke naam voor een simulator |
    | **Beschrijving** | Voor een definitie in |
    | **Simulatieduur** | Ingesteld op `Run indefinitely` |
    | **Apparaatmodel** | **Naam**: Voer `Chiller` in. **Bedrag**: Voer `3` in |
    | **Doel-IoT-hub** | Ingesteld op `Use pre-provisioned IoT Hub` |

    Nadat u de vereiste parameters hebt ingesteld, klikt u op **Simulatie starten**.

1. In het dashboard van de apparaatsimulatie ziet u de **Actieve apparaten** en **Berichten per seconde**.

    ![Dashboard van Azure IoT-simulatie][6]

## <a name="list-device-simulation-properties"></a>Eigenschappen van apparaatsimulatie vermelden

Voordat u een Azure Time Series Insights-omgeving maakt, hebt u de namen van uw IoT Hub, abonnement en resourcegroepnaam nodig.

1. Ga naar het **Dashboard van de oplossingsverbetering** en meld u aan met hetzelfde Azure-abonnementsaccount. Zoek de apparaatsimulatie die u in de vorige stappen hebt gemaakt.

1. Klik op uw apparaatsimulator en klik op **Starten**. Klik op de link naar de **Azure Management Portal** die aan rechterzijde wordt weergegeven.

    ![Simulatorvermeldingen][7]

1. Noteer de namen van de IoT Hub, het abonnement en de resourcegroep.

    ![Azure Portal][8]

## <a name="create-a-time-series-insights-preview-payg-environment"></a>Een Time Series Insights Preview-omgeving op basis van betalen per gebruik maken

In deze sectie wordt beschreven hoe u een Azure Time Series Insights Preview-omgeving maakt door de [Azure-portal](https://portal.azure.com/).

1. Meld u aan bij de Azure-portal met uw Azure-abonnementsaccount.

1. Selecteer **Een resource maken**.

1. Selecteer de categorie **Internet of Things** en vervolgens **Time Series Insights**.

   ![Selecteer Een resource maken, dan Internet of Things en vervolgens Time Series Insights][9]

1. Vul de velden op de pagina als volgt in:

   | | |
   | --- | ---|
   | **Omgevingsnaam** | Kies een unieke naam voor de Azure Time Series Insights Preview-omgeving. |
   | **Abonnement** | Voer uw abonnement in waarin u uw Azure Time Series Insights Preview-omgeving wilt maken. Het is een best practice om hetzelfde abonnement te gebruiken als de rest van uw IoT-resources die door de apparaatsimulator worden gemaakt. |
   | **Resourcegroep** | Een resourcegroep is een container voor Azure-resources. Kies een bestaande resourcegroep voor de Azure Time Series Insights Preview-omgevingsresource of maak een nieuwe. Het is een best practice om dezelfde resourcegroep te gebruiken als de rest van uw IoT-resources die door de apparaatsimulator worden gemaakt. |
   | **Locatie** | Kies een datacentrumregio voor uw Azure Time Series Insights Preview-omgeving. Vermijd extra bandbreedtekosten en -latentie door de Azure Time Series Insights Preview-omgeving in dezelfde regio te bewaren als andere IoT-resources. |
   | **Laag** |  Selecteer `PAYG` die voor betalen per gebruik staat. Dit is de SKU voor Azure Time Series Insights Preview. |
   | **Eigenschaps-ID** | Unieke identificatie van uw tijdreeks. Houd er rekening mee dat dit veld onveranderbaar is en later niet kan worden gewijzigd. Voor deze zelfstudie is het veld ingesteld op `iothub-connection-device-id`. Lees [Een Time Series-ID kiezen](./time-series-insights-update-how-to-id.md) voor meer informatie over Time Series-ID. |
   | **Naam van opslagaccount** | Voer een wereldwijd unieke naam voor een nieuw te maken opslagaccount in. |

   Nadat u de bovenstaande velden hebt ingevoerd, klikt u op **Volgende: Bron van gebeurtenis**.

   ![Klik op Volgende: Bron van gebeurtenis][10]

1. Vul op de pagina de velden als volgt in:

   | | |
   | --- | --- |
   | **Een gebeurtenisbron maken** | Voer `Yes` in|
   | **Naam** | Vereist een unieke waarde, die wordt gebruikt om de gebeurtenisbron een naam te geven.|
   | **Brontype** | Voer `IoT Hub` in |
   | **Selecteer een Hub?** | Voer `Select Existing` in |
   | **Abonnement** | Voer het abonnement in dat u hebt gebruikt voor de apparaatsimulator. |
   | **Naam van de IoT Hub** | Voer de naam van de IoT Hub in die u hebt gebruikt voor de apparaatsimulator. |
   | **IoT Hub-toegangsbeleid** | Voer `iothubowner` in |
   | **IoT Hub-consumentengroep** | U hebt een unieke consumentengroep nodig voor een Azure Time Series Insights Preview. |
   | **Timestamp** | Dit veld wordt gebruikt om de eigenschap timestamp te identificeren in uw binnenkomende telemetriegegevens. Vul het veld tijdens deze zelfstudie niet in. In deze simulator wordt de binnenkomende timestamp van IoT Hub gebruikt waarop Time Series Insights standaard is ingesteld.|

   Doe het volgende om een unieke consumentengroep te maken:

   1. Klik op **Nieuw** naast het veld **IoT Hub-consumentengroep**:

      ![Klik op Volgende: Bron van gebeurtenis][11]

   1. Geef de consumentengroep een unieke naam en klik op **Toevoegen**:

      ![Klik op Add.][12]

   Nadat u de bovenstaande velden hebt ingevuld, klikt u op **Controleren en maken**.

      ![Controleren en maken][13]

1. Controleer alle velden op de controlepagina en klik op **Maken**.

   ![Maken][14]

1. Hier ziet u de status van uw implementatie.

   ![Implementatie voltooid][15]

1. U zou toegang moeten krijgen tot uw tijdreeksomgeving als u eigenaar van de tenant bent. Doe het volgende om ervoor te zorgen dat u toegang hebt:

   * Navigeer naar uw nieuw gemaakt Azure Time Series Insights Preview-omgeving. U doet dit door te zoeken naar uw resourcegroep. Klik vervolgens op uw tijdreeksomgeving:

      ![Implementatie voltooid][16]

   * Op de pagina Azure Time Series Insights Preview navigeert u naar **Toegangsbeleid voor gegevens**.

     ![Beleid voor gegevenstoegang][17]

   * Controleer of uw referenties worden vermeld.

     ![Uw referenties controleren][18]

   Als uw referenties niet worden vermeld, moet u uzelf toestemming geven voor toegang tot de omgeving. Raadpleeg [Gegevenstoegang](./time-series-insights-data-access.md) om meer te leren over machtigingen instellen.

## <a name="analyze-data-in-your-environment"></a>Gegevens analyseren in uw omgeving

In deze sectie voert u een eenvoudige analyse uit op uw tijdreeksgegevens met de [verkenner van Azure Time Series Insights Preview](./time-series-insights-update-explorer.md).

1. Ga naar de verkenner van Azure Time Series Insights Preview door te klikken op de URL op de resourcepagina in de [Azure-portal](https://portal.azure.com/).

   ![URL naar de verkenner van Time Series Insights][19]

1. In de verkenner selecteert u de knooppunten onder **Niet-bovenliggende exemplaren** om alle Azure Time Series Insights Preview in de omgeving te bekijken.

   ![Lijst van niet-bovenliggende exemplaren][20]

1. In de weergegeven tijdreeks klikt u op het eerste exemplaar. Klik vervolgens op **Gemiddelde druk weergeven**.

   ![Gemiddelde druk weergeven][21]

1. Er zou aan de rechterzijde een tijdreeksgrafiek kunnen verschijnen:

   ![Time Series-grafiek][22]

1. Herhaal **stap 3** bij de andere twee tijdreeksen. Alle tijdreeksen kunnen vervolgens worden weergegeven zoals hieronder:

   ![Grafiek met alle tijdreeksen][23]

1. Wijzig het **tijdsbereik** om de trends in de tijdreeks van het afgelopen uur te bekijken. Selecteer het optievakje **Vanaf** zoals hier beneden wordt weergegeven:

   ![Selecteer de optie Vanaf][24]

1. Wijzig de tijd binnen het optievakje **Vanaf** om gebeurtenissen weer te geven van het afgelopen uur:

   ![Selecteer de optie Vanaf][25]

1. Vervolgens kunt u de druk van alle drie de apparaten in het afgelopen uur vergelijken:

   ![Selecteer de optie Vanaf][26]

## <a name="define-and-apply-a-model"></a>Een model definiëren en toepassen

In deze sectie past u een model toe om uw gegevens te structureren. U definieert Typen, Hiërarchieën en Exemplaren om het model te voltooien. Ga naar [Time Series-modellen](./time-series-insights-update-tsm.md) voor meer informatie over gegevensmodellering.

1. Selecteer het tabblad **Model** in de verkenner:

   ![Selecteer het tabblad Model][27]

1. Klik vervolgens op **+ Toevoegen** om een type toe te voegen. Aan de rechterzijde wordt een typebewerker geopend.

   ![Klik op Add.][28]

1. Definieer vervolgens drie variabelen: Druk, Temperatuur en Vochtigheid in een Type. Voer de volgende velden in:

   | | |
   | --- | ---|
   | **Naam** | Voer `Chiller` in |
   | **Beschrijving** | Voer `This is a type definition of Chiller` in |

   * Definieer nu Druk met drie variabelen:

      | | |
      | --- | ---|
      | **Naam** | Voer `Avg Pressure` in |
      | **Waarde** | Selecteer **druk (dubbel)**. Onthoud dat het een paar minuten duurt voordat dit veld is ingevuld nadat Azure Time Series Insights gebeurtenissen ontvangt |
      | **Aggregatiebewerking** | Selecteer `AVG` |

      ![Een variabele toevoegen][29]

      Klik op **+ Variabele** om de volgende variabele toe te voegen.

   * Definieer nu Temperatuur:

      | | |
      | --- | ---|
      | **Naam** | Voer `Avg Temperature` in |
      | **Waarde** | Selecteer **temperatuur (dubbel)**. Onthoud dat het een paar minuten duurt voordat dit veld is ingevuld nadat Azure Time Series Insights gebeurtenissen ontvangt |
      | **Aggregatiebewerking** | Selecteer `AVG`|

      ![Definieer Temperatuur][30]

   * Definieer nu Vochtigheid:

      | | |
      | --- | ---|
      | **Naam** | Voer `Max Humidity` in |
      | **Waarde** | Selecteer **vochtigheid (dubbel)**. Onthoud dat het een paar minuten duurt voordat dit veld is ingevuld nadat Azure Time Series Insights gebeurtenissen ontvangt |
      | **Aggregatiebewerking** | Selecteer `MAX`|

      ![Definieer Temperatuur][31]

   Nadat u de variabelen hebt gedefinieerd, klikt u op **Maken**.

1. Hier ziet u uw toegevoegde Type:

   ![Bekijk uw toegevoegde type][32]

1. De volgende stap is om een Hiërarchie toe te voegen. In de sectie **Hiërarchieën** selecteert u **+ Toevoegen** om een nieuwe Hiërarchie te maken:

   ![Een hiërarchie toevoegen][33]

1. Definieer Hiërarchie. Vul de velden als volgt in:

   | | |
   | --- | ---|
   | **Naam** | Voer `Location Hierarchy` in |
   | **Niveau 1** | Voer `Country` in |
   | **Niveau 2** | Voer `City` in |
   | **Niveau 3** | Voer `Building` in |

   Nadat u de bovenstaande velden hebt ingevoerd, klikt u op **Maken**.

   ![Een Hiërarchie definiëren][34]

1. U kunt de gemaakte Hiërarchie bekijken:

   ![Bekijk uw Hiërarchie][35]

1. Nadat u uw Hiërarchie hebt gedefinieerd, klikt u aan de linkerzijde op **Exemplaren**. Nadat de exemplaren verschijnen, klikt u op het eerste exemplaar en selecteert u **Bewerken**:

   ![Een exemplaar bewerken][36]

1. Aan de rechterzijde wordt een teksteditor geopend. Voeg de volgende velden toe:

   | | |
   | --- | --- |
   | **Type** | Selecteer `Chiller` |
   | **Beschrijving** | Voer `Instance for Chiller-01.1` in |
   | **Hiërarchieën** | Schakel `Location Hierarchy` in |
   | **Land** | Voer `USA` in |
   | **Plaats** | Voer `Seattle` in |
   | **Bouwen** | Voer `Space Needle` in |

    Nadat u de bovenstaande velden hebt ingevuld, klikt u op **Opslaan**.

   ![Een koelunit opslaan][37]

1. Herhaal de vorige stap voor de overige sensoren. Gebruik de volgende velden:

   * Voor Koelunit 01.2:

     | | |
     | --- | --- |
     | **Type** | Selecteer `Chiller` |
     | **Beschrijving** | Voer `Instance for Chiller-01.2` in |
     | **Hiërarchieën** | Schakel `Location Hierarchy` in |
     | **Land** | Voer `USA` in |
     | **Plaats** | Voer `Seattle` in |
     | **Bouwen** | Voer `Pacific Science Center` in |

   * Voor Koelunit 01.3:

     | | |
     | --- | --- |
     | **Type** | Selecteer `Chiller` |
     | **Beschrijving** | Voer `Instance for Chiller-01.1` in |
     | **Hiërarchieën** | Schakel `Location Hierarchy` in |
     | **Land** | Voer `USA` in |
     | **Plaats** | Voer `New York` in |
     | **Bouwen** | Voer `Empire State Building` in |

1. Ga naar het tabblad **Analyseren** en vernieuw de pagina. Vouw alle Hiërarchieniveaus uit om de tijdreeks te zoeken.

   ![Het tabblad Analyseren bekijken][38]

1. Wijzig **Snelle tijden** in het afgelopen uur om de tijdreeks van het afgelopen uur te bekijken:

   ![Het afgelopen uur bekijken][39]

1. Klik op de tijdreeks onder **Pacific Science Center** en klik op **Maximale vochtigheid weergeven**.

   ![Maximale Vochtigheid weergeven][40]

1. De tijdsreeks voor **Maximale vochtigheid** met een intervalgrootte van 1 minuut wordt geopend. Klik op een regio om te filteren op een bereik. Klik vervolgens met de rechtermuisknop op gebeurtenissen in het tijdsbestek:

   ![Bekijken, filteren en zoomen][41]

   ![Bekijken, filteren en zoomen][42]

1. U kunt ook op een regio klikken en vervolgens met de rechtermuisknop klikken om details van een gebeurtenis te bekijken:

   ![Bekijken, filteren en zoomen][43]

   ![Bekijken, filteren en zoomen][44]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:  

* Een verbetering voor apparaatsimulatie maken en gebruiken.
* Een Time Series Insights Preview-omgeving op basis van betalen per gebruik maken.
* De Azure Time Series Insights Preview-omgeving verbinden met een Event Hub.
* Een windparksimulatie uitvoeren om gegevens te streamen naar de Azure Time Series Insights Preview-omgeving.
* Een eenvoudige analyse uitvoeren van de gegevens.
* Een type Time Series Model en een hiërarchie definiëren en deze koppelen aan uw exemplaren.

Nu u weet hoe u een eigen Azure Time Series Preview-omgeving maakt, is het tijd om meer te leren over de belangrijkste concepten in Azure Time Series Insights.

Meer informatie over Azure Time Series Insights-opslagconfiguratie:

> [!div class="nextstepaction"]
> [Azure Time Series Insights Preview-opslag en -opname](./time-series-insights-update-storage-ingress.md)

Meer informatie over Time Series-modellen:

> [!div class="nextstepaction"]
> [Azure Time Series Insights Preview-gegevensmodellering](./time-series-insights-update-tsm.md)

<!-- Images -->
[1]: media/v2-update-provision/device-one-accelerator.png
[2]: media/v2-update-provision/device-two-create.png
[3]: media/v2-update-provision/device-three-launch.png
[4]: media/v2-update-provision/device-four-iot-sim-page.png
[5]: media/v2-update-provision/device-five-params.png
[6]: media/v2-update-provision/device-six-listings.png
[7]: media/v2-update-provision/device-seven-dashboard.png
[8]: media/v2-update-provision/device-eight-portal.png

[9]: media/v2-update-provision/payg-one-azure.png
[10]: media/v2-update-provision/payg-two-create.png
[11]: media/v2-update-provision/payg-three-new.png
[12]: media/v2-update-provision/payg-four-add.png
[13]: media/v2-update-provision/payg-five-event-source.png
[14]: media/v2-update-provision/payg-six-review.png
[15]: media/v2-update-provision/payg-seven-deploy.png
[16]: media/v2-update-provision/payg-eight-environment.png
[17]: media/v2-update-provision/payg-nine-data-access.png
[18]: media/v2-update-provision/payg-ten-verify.png

[19]: media/v2-update-provision/analyze-one-portal.png
[20]: media/v2-update-provision/analyze-two-unparented.png
[21]: media/v2-update-provision/analyze-three-show-pressure.png
[22]: media/v2-update-provision/analyze-four-chart.png
[23]: media/v2-update-provision/analyze-five-chart.png
[24]: media/v2-update-provision/analyze-six-from.png
[25]: media/v2-update-provision/analyze-seven-change-from.png
[26]: media/v2-update-provision/analyze-eight-all.png

[27]: media/v2-update-provision/define-one-model.png
[28]: media/v2-update-provision/define-two-add.png
[29]: media/v2-update-provision/define-three-variable.png
[30]: media/v2-update-provision/define-four-avg.png
[31]: media/v2-update-provision/define-five-humidity.png
[32]: media/v2-update-provision/define-six-type.png
[33]: media/v2-update-provision/define-seven-hierarchy.png
[34]: media/v2-update-provision/define-eight-add-hierarchy.png
[35]: media/v2-update-provision/define-nine-created.png
[36]: media/v2-update-provision/define-ten-edit.png
[37]: media/v2-update-provision/define-eleven-chiller.png
[38]: media/v2-update-provision/define-twelve.png
[39]: media/v2-update-provision/define-thirteen-explore.png
[40]: media/v2-update-provision/define-fourteen-show-max.png
[41]: media/v2-update-provision/define-fifteen-filter.png
[42]: media/v2-update-provision/define-sixteen.png
[43]: media/v2-update-provision/define-seventeen.png
[44]: media/v2-update-provision/define-eighteen.png