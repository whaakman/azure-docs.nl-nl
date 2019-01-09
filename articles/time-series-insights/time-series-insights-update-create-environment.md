---
title: 'Zelfstudie: Een Azure Time Series Insights Preview-omgeving instellen | Microsoft Docs'
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
ms.openlocfilehash: 1b09c0e31b217d7d67f936aefe9045d190241389
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53633110"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Zelfstudie: Een Time Series Insights Preview-omgeving instellen

In deze zelfstudie wordt u door het creatieproces van een Azure Time Series Insights Preview-omgeving op basis van betalen per gebruik geleid. In deze zelfstudie leert u het volgende:

* Een Time Series Insights Preview-omgeving maken.
* De Azure Time Series Insights Preview-omgeving verbinden met een Event Hub in Azure Event Hubs.
* Een voorbeeldoplossingsverbetering uitvoeren om gegevens te streamen naar de Azure Time Series Insights Preview-omgeving.
* Voer een eenvoudige analyse van de gegevens uit.
* Een type Time Series Model en een hiërarchie definiëren en deze koppelen aan uw exemplaren.

# <a name="create-a-device-simulation"></a>Een apparaatsimulatie maken

In dit deel maakt u drie gesimuleerde apparaten die gegevens verzenden naar een IoT-hub.

1. Ga naar [de pagina Azure IoT-oplossingsverbeteringen](https://www.azureiotsolutions.com/Accelerators). Op de pagina worden enkele vooraf gedefinieerde voorbeelden weergegeven. Meld u aan met uw Azure-account. Selecteer vervolgens **Apparaatsimulatie**.

   ![De pagina Azure IoT-oplossingsverbeteringen][1]

   Selecteer **Nu uitproberen**.

1. Voer de vereiste parameters in op de pagina **Apparaatsimulatieoplossing maken**:

   | Parameter | Beschrijving |
   | --- | --- |
   | **Naam van de oplossing** |    Voer een unieke waarde in om een nieuwe resourcegroep te maken. De vermelde Azure-resources worden gemaakt en toegewezen aan de resourcegroep. |
   | **Abonnement** | Geef hetzelfde abonnement op dat is gebruikt voor het maken van uw Time Series Insights-omgeving. |
   | **Regio** |   Geef dezelfde regio op die is gebruikt voor het maken van uw Time Series Insights-omgeving. |
   | **Optionele Azure-resources implementeren**    | Laat IoT Hub aangevinkt, omdat de gesimuleerde apparaten het gebruiken om gegevens aan te sluiten en te streamen. |

   Selecteer vervolgens **Oplossing maken**. Wacht 10-15 minuten totdat uw oplossing is geïmplementeerd.

   ![De pagina Oplossing voor apparaatsimulatie maken][2]

1. In uw dashboard voor oplossingsverbeteringen selecteert u de knop **Starten**:

   ![Oplossing voor apparaatsimulatie starten][3]

1. U wordt omgeleid naar de pagina **Microsoft Azure IoT-apparaatsimulatie**. Selecteer **+ Nieuwe simulatie** in de rechterbovenhoek van de pagina.

   ![Pagina van Azure IoT-simulatie][4]

1.  Vul de vereiste parameters als volgt in:

    ![In te vullen parameters][5]

    |||
    | --- | --- |
    | **Naam** | Voer een unieke naam voor een simulator in. |
    | **Beschrijving** | Voer een definitie in. |
    | **Simulatieduur** | Stel de simulatieduur in op **Voor onbepaalde tijd uitvoeren**. |
    | **Apparaatmodel** | **Naam**: Voer **Koelunit** in. </br>**Aantal**: Voer **3** in. |
    | **Doel-IoT-hub** | Stel **Vooraf ingerichte IoT-hub gebruiken** in. |

    Selecteer vervolgens **Simulatie starten**.

1. In het dashboard van de apparaatsimulatie ziet u **Actieve apparaten** en **Berichten per seconde**.

    ![Dashboard van Azure IoT-simulatie][6]

## <a name="list-device-simulation-properties"></a>Eigenschappen van apparaatsimulatie vermelden

Voordat u een Azure Time Series Insights-omgeving maakt, hebt u de namen van uw IoT-hub, abonnement en resourcegroep nodig.

1. Ga naar het dashboard van de oplossingsverbetering en meld u aan met hetzelfde Azure-abonnementsaccount. Zoek de apparaatsimulatie die u in de vorige stappen hebt gemaakt.

1. Klik op uw apparaatsimulator en selecteer **Starten**. Selecteer de koppeling **Azure Management Portal** aan de rechterkant.

    ![Simulatorvermeldingen][7]

1. Noteer de namen van de IoT-hub, het abonnement en de resourcegroep.

    ![Azure Portal][8]

## <a name="create-a-time-series-insights-preview-payg-environment"></a>Een Time Series Insights Preview-omgeving op basis van betalen per gebruik maken

In deze sectie wordt beschreven hoe u een Azure Time Series Insights Preview-omgeving maakt door de [Azure-portal](https://portal.azure.com/).

1. Meld u aan bij de Azure-portal met uw Azure-abonnementsaccount.

1. Selecteer **Een resource maken**.

1. Selecteer de categorie **Internet of Things** en vervolgens **Time Series Insights**.

   ![Internet of Things en vervolgens Time Series Insights selecteren][9]

1. Vul de velden op de pagina als volgt in:

   | | |
   | --- | ---|
   | **Omgevingsnaam** | Kies een unieke naam voor de Azure Time Series Insights Preview-omgeving. |
   | **Abonnement** | Voer uw abonnement in waarin u uw Azure Time Series Insights Preview-omgeving wilt maken. Het is een best practice om hetzelfde abonnement te gebruiken als de rest van uw IoT-resources die door de apparaatsimulator worden gemaakt. |
   | **Resourcegroep** | Een resourcegroep is een container voor Azure-resources. Kies een bestaande resourcegroep voor de Azure Time Series Insights Preview-omgevingsresource of maak een nieuwe. Het is een best practice om dezelfde resourcegroep te gebruiken als de rest van uw IoT-resources die door de apparaatsimulator worden gemaakt. |
   | **Locatie** | Kies een datacentrumregio voor uw Azure Time Series Insights Preview-omgeving. Vermijd extra bandbreedtekosten en -latentie door de Azure Time Series Insights Preview-omgeving in dezelfde regio te bewaren als andere IoT-resources. |
   | **Laag** |  Selecteer **Betalen per gebruik**. Dit is de SKU voor het Azure Time Series Insights Preview-product. |
   | **Eigenschaps-ID** | Voer een unieke identificatie voor uw tijdreeks in. Houd er rekening mee dat dit veld onveranderbaar is en later niet meer kan worden gewijzigd. Voor deze zelfstudie gebruiken we **iothub-connection-device-id**. Lees [Een Time Series-ID kiezen](./time-series-insights-update-how-to-id.md) voor meer informatie over Time Series-ID. |
   | **Naam van opslagaccount** | Voer een wereldwijd unieke naam voor een nieuw te maken opslagaccount in. |

   Selecteer vervolgens **Volgende: Bron van gebeurtenis**.

   ![Pagina voor het maken van een Time Series Insights-omgeving][10]

1. Vul de velden op de pagina voor de gebeurtenisbron als volgt in:

   | | |
   | --- | --- |
   | **Een gebeurtenisbron maken** | Voer **Ja** in.|
   | **Naam** | Voer een unieke waarde in als naam voor de gebeurtenisbron.|
   | **Brontype** | Voer **IoT Hub** in. |
   | **Een hub selecteren** | Voer **Bestaande selecteren**. |
   | **Abonnement** | Voer het abonnement in dat u hebt gebruikt voor de apparaatsimulator. |
   | **Naam van de IoT Hub** | Voer de naam in van de IoT-hub die u hebt gebruikt voor de apparaatsimulator. |
   | **IoT Hub-toegangsbeleid** | Voer **iothubowner** in. |
   | **IoT Hub-consumentengroep** | U hebt een unieke consumentengroep nodig voor Azure Time Series Insights Preview. Selecteer **Nieuw**, voer een unieke naam in en selecteer vervolgens **Toevoegen**. |
   | **Timestamp-eigenschap** | Dit veld wordt gebruikt om de eigenschap timestamp te identificeren in uw binnenkomende telemetriegegevens. Vul het veld niet in voor deze zelfstudie. In deze simulator wordt de binnenkomende timestamp van IoT Hub gebruikt waarop Time Series Insights standaard is ingesteld.|

   Selecteer vervolgens **Controleren + maken**.

   ![Pagina voor het instellen van een gebeurtenisbron][13]

1. Controleer alle velden op de controlepagina en selecteer **Maken**.

   ![De pagina Controleren + maken met de knop Maken][14]

1. Hier ziet u de status van uw implementatie.

   ![Melding dat de implementatie voltooid is][15]

1. U zou toegang moeten krijgen tot uw Azure Time Series Insights Preview-omgeving als u eigenaar van de tenant bent. Doe het volgende om ervoor te zorgen dat u toegang hebt:

   a. Zoek uw resourcegroep en selecteer uw Azure Time Series Insights Preview-omgeving:

      ![Geselecteerde omgeving][16]

   b. Ga op de pagina Azure Time Series Insights Preview naar **Beleid voor gegevenstoegang**.

     ![Beleid voor gegevenstoegang][17]

   c. Controleer of uw referenties worden vermeld.

     ![Vermelde referenties][18]

   Als uw referenties niet worden vermeld, moet u uzelf toestemming geven voor toegang tot de omgeving. Raadpleeg [Gegevenstoegang verlenen](./time-series-insights-data-access.md) voor meer informatie over het instellen van machtigingen.

## <a name="analyze-data-in-your-environment"></a>Gegevens analyseren in uw omgeving

In deze sectie voert u een eenvoudige analyse uit op uw tijdreeksgegevens met de [verkenner van Azure Time Series Insights Preview](./time-series-insights-update-explorer.md).

1. Ga naar de verkenner van Azure Time Series Insights Preview door de URL te selecteren op de resourcepagina in [Azure Portal](https://portal.azure.com/).

   ![URL van verkenner van Time Series Insights Preview][19]

1. In de verkenner selecteert u het knooppunt onder **Niet-bovenliggende exemplaren** om alle exemplaren van Azure Time Series Insights Preview in de omgeving te bekijken.

   ![Lijst van niet-bovenliggende exemplaren][20]

1. In de weergegeven tijdreeks selecteert u het eerste exemplaar. Selecteer vervolgens **Gemiddelde druk weergeven**.

   ![Geselecteerd exemplaar met menuopdracht om de gemiddelde druk weer te geven][21]

   Er zou aan de rechterzijde een tijdreeksgrafiek kunnen verschijnen:

   ![Time Series-grafiek][22]

1. Herhaal stap 3 bij de andere twee tijdreeksen. Vervolgens kunt u alle tijdreeksen, bekijken, zoals wordt weergegeven in deze grafiek:

   ![Grafiek met alle tijdreeksen][23]

1. Wijzig het tijdsbereik om de trends in de tijdreeks van het afgelopen uur te bekijken. 

   a. Selecteer het optievakje **Vanaf**:

      ![Het optievakje Vanaf][24]

   b. Wijzig de tijd in het optievakje om de gebeurtenissen in het afgelopen uur weer te geven:

      ![Tijdcorrecties][25]

1. Vervolgens kunt u de druk van alle drie de apparaten in het afgelopen uur vergelijken:

   ![Vergelijking van drie apparaten][26]

## <a name="define-and-apply-a-model"></a>Een model definiëren en toepassen

In dit gedeelte past u een model toe om uw gegevens te structureren. U definieert typen, hiërarchieën en exemplaren om het model te voltooien. Ga naar [Time Series-model](./time-series-insights-update-tsm.md) voor meer informatie over gegevensmodellering.

1. Selecteer het tabblad **Model** in de verkenner:

   ![Het tabblad Model in de verkenner][27]

1. Selecteer **+ Toevoegen** om een type toe te voegen. Aan de rechterzijde wordt een type-editor geopend.

   ![De knop Toevoegen voor typen][28]

1. Definieer drie variabelen voor het type: druk, temperatuur en vochtigheid. Voer de volgende informatie in:

   | | |
   | --- | ---|
   | **Naam** | Voer **Koelunit** in. |
   | **Beschrijving** | Voer **Dit is een typedefinitie van Koelunit** in. |

   * Definieer Druk met drie variabelen:

      | | |
      | --- | ---|
      | **Naam** | Voer **Gemiddelde druk** in. |
      | **Waarde** | Selecteer **druk (dubbel)**. Het duurt een paar minuten voordat dit veld is ingevuld nadat Azure Time Series Insights Preview gebeurtenissen ontvangt. |
      | **Aggregatiebewerking** | Selecteer **GEMIDDELDE**. |

      ![Selecties voor het definiëren van druk][29]

      Selecteer **+ Variabele toevoegen** om de volgende variabele toe te voegen.

   * Definieer Temperatuur:

      | | |
      | --- | ---|
      | **Naam** | Voer **Gemiddelde temperatuur** in. |
      | **Waarde** | Selecteer **temperatuur (dubbel)**. Het duurt een paar minuten voordat dit veld is ingevuld nadat Azure Time Series Insights Preview gebeurtenissen ontvangt. |
      | **Aggregatiebewerking** | Selecteer **GEMIDDELDE**.|

      ![Selecties voor het definiëren van temperatuur][30]

   * Definieer Vochtigheid:

      | | |
      | --- | ---|
      | **Naam** | Voer **Maximale vochtigheid** in. |
      | **Waarde** | Selecteer **vochtigheid (dubbel)**. Het duurt een paar minuten voordat dit veld is ingevuld nadat Azure Time Series Insights Preview gebeurtenissen ontvangt. |
      | **Aggregatiebewerking** | Selecteer **MAXIMUM**.|

      ![Selecties voor het definiëren van temperatuur][31]

   Ten slotte selecteert u **Create**.

1. Hier ziet u uw toegevoegde type:

   ![Informatie over het toegevoegde type][32]

1. De volgende stap is om een hiërarchie toe te voegen. In het gedeelte **Hiërarchieën** selecteert u **+ Toevoegen**:

   ![Het tabblad Hiërarchieën met de knop Toevoegen][33]

1. Definieer de hiërarchie. Vul de velden als volgt in:

   | | |
   | --- | ---|
   | **Naam** | Voer **Locatiehiërarchie** in. |
   | **Niveau 1** | Voer **Land** in. |
   | **Niveau 2** | Voer **Plaats** in. |
   | **Niveau 3** | Voer **Gebouw** in. |

   Ten slotte selecteert u **Create**.

   ![Hiërarchievelden met de knop Maken][34]

1. U kunt de gemaakte hiërarchie bekijken:

   ![Informatie over de hiërarchie][35]

1. Selecteer **Exemplaren** aan de linkerkant. Als de exemplaren verschijnen, selecteert u het eerste exemplaar en vervolgens **Bewerken**:

   ![De knop Bewerken voor een exemplaar selecteren][36]

1. Aan de rechterzijde wordt een teksteditor geopend. Voeg de volgende informatie toe:

   | | |
   | --- | --- |
   | **Type** | Selecteer **Koelunit**. |
   | **Beschrijving** | Voer **Exemplaar voor Koelunit-01.1** in. |
   | **Hiërarchieën** | Schakel **Locatiehiërarchie** in. |
   | **Land** | Voer **USA** in. |
   | **Plaats** | Voer **Seattle** in. |
   | **Bouwen** | Voer **Space Needle** in. |

    Selecteer vervolgens **Opslaan**.

   ![Exemplaarvelden met de knop Opslaan][37]

1. Herhaal de vorige stap voor de overige sensoren. Gebruik de volgende velden:

   * Voor Koelunit 01.2:

     | | |
     | --- | --- |
     | **Type** | Selecteer **Koelunit**. |
     | **Beschrijving** | Voer **Exemplaar voor Koelunit-01.2** in. |
     | **Hiërarchieën** | Schakel **Locatiehiërarchie** in. |
     | **Land** | Voer **USA** in. |
     | **Plaats** | Voer **Seattle** in. |
     | **Bouwen** | Voer **Pacific Science Center** in. |

   * Voor Koelunit 01.3:

     | | |
     | --- | --- |
     | **Type** | Selecteer **Koelunit**. |
     | **Beschrijving** | Voer **Exemplaar voor Koelunit-01.1** in. |
     | **Hiërarchieën** | Schakel **Locatiehiërarchie** in. |
     | **Land** | Voer **USA** in. |
     | **Plaats** | Voer **New York** in. |
     | **Bouwen** | Voer **Empire State Building** in. |

1. Ga naar het tabblad **Analyseren** en vernieuw de pagina. Vouw alle hiërarchieniveaus uit om de tijdreeks te zoeken.

   ![Het tabblad Analyseren][38]

1. Wijzig **Snelle tijden** in **Afgelopen uur** om de tijdreeks van het afgelopen uur te bekijken:

   ![Het vak Snelle tijden met Afgelopen uur geselecteerd][39]

1. Klik op de tijdreeks onder **Pacific Science Center** en selecteer **Maximale vochtigheid weergeven**.

   ![Geselecteerde tijdreeks met menuopdracht Maximale vochtigheid weergeven geselecteerd][40]

1. De tijdsreeks voor **Maximale vochtigheid** met een intervalgrootte van 1 minuut wordt geopend. Selecteer een regio om te filteren op een bereik. Klik vervolgens met de rechtermuisknop op **Inzoomen** om gebeurtenissen in het tijdsbestek te analyseren:

   ![Bereik dat is geselecteerd met de opdracht Inzoomen in een snelmenu][41]

1. U kunt ook een regio selecteren en vervolgens met de rechtermuisknop klikken om details van een gebeurtenis te bekijken:

   ![Lijst met gebeurtenisdetails][44]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:  

* Een verbetering voor apparaatsimulatie maken en gebruiken.
* Een Time Series Insights Preview-omgeving op basis van betalen per gebruik maken.
* De Azure Time Series Insights Preview-omgeving verbinden met een Event Hub.
* Een voorbeeldoplossingsverbetering uitvoeren om gegevens te streamen naar de Azure Time Series Insights Preview-omgeving.
* Een eenvoudige analyse uitvoeren van de gegevens.
* Een type Time Series Model en een hiërarchie definiëren en deze koppelen aan uw exemplaren.

Nu u weet hoe u een eigen Azure Time Series Preview-omgeving maakt, is het tijd om meer te leren over de belangrijkste concepten in Azure Time Series Insights.

Meer informatie over de Azure Time Series Insights-opslagconfiguratie:

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
[6]: media/v2-update-provision/device-seven-dashboard.png
[7]: media/v2-update-provision/device-six-listings.png
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