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
ms.date: 04/25/2019
ms.custom: seodec18
ms.openlocfilehash: f83063a88207f51f9d481447923fd8a8498692a2
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64713910"
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

   [![Azure IoT-oplossing accelerators pagina](media/v2-update-provision/device-one-accelerator.png)](media/v2-update-provision/device-one-accelerator.png#lightbox)

   Selecteer **Nu uitproberen**.

1. Voer de vereiste parameters in op de pagina **Apparaatsimulatieoplossing maken**:

   | Parameter | Description |
   | --- | --- |
   | **Naam van de oplossing** |    Voer een unieke waarde in om een nieuwe resourcegroep te maken. De vermelde Azure-resources worden gemaakt en toegewezen aan de resourcegroep. |
   | **Abonnement** | Geef hetzelfde abonnement op dat is gebruikt voor het maken van uw Time Series Insights-omgeving. |
   | **Regio** |   Geef dezelfde regio op die is gebruikt voor het maken van uw Time Series Insights-omgeving. |
   | **Optionele Azure-resources implementeren**    | Laat IoT Hub aangevinkt, omdat de gesimuleerde apparaten het gebruiken om gegevens aan te sluiten en te streamen. |

   Selecteer vervolgens **Oplossing maken**. Wacht 10-15 minuten totdat uw oplossing is geïmplementeerd.

   [![De pagina van de Apparaatsimulatie-oplossing maken](media/v2-update-provision/device-two-create.png)](media/v2-update-provision/device-two-create.png#lightbox)

1. In uw dashboard voor oplossingsverbeteringen selecteert u de knop **Starten**:

   [![De simulatie apparaat oplossing starten](media/v2-update-provision/device-three-launch.png)](media/v2-update-provision/device-three-launch.png#lightbox)

1. U wordt omgeleid naar de pagina **Microsoft Azure IoT-apparaatsimulatie**. Selecteer **+ Nieuwe simulatie** in de rechterbovenhoek van de pagina.

   [![Pagina van Azure IoT-simulatie](media/v2-update-provision/device-four-iot-sim-page.png)](media/v2-update-provision/device-four-iot-sim-page.png#lightbox)

1. Vul de vereiste parameters als volgt in:

    [![Parameters invullen](media/v2-update-provision/device-five-params.png)](media/v2-update-provision/device-five-params.png#lightbox)

    |||
    | --- | --- |
    | **Naam** | Voer een unieke naam voor een simulator in. |
    | **Beschrijving** | Voer een definitie in. |
    | **Simulatieduur** | Stel de simulatieduur in op **Voor onbepaalde tijd uitvoeren**. |
    | **Apparaatmodel** | **Naam**: Voer `Chiller` in. </br>**Aantal**: Voer `3` in. |
    | **Doel-IoT-hub** | Stel **Vooraf ingerichte IoT-hub gebruiken** in. |

    Selecteer vervolgens **Simulatie starten**.

1. In het dashboard van de apparaatsimulatie ziet u **Actieve apparaten** en **Berichten per seconde**.

    [![Dashboard van Azure IoT-simulatie](media/v2-update-provision/device-seven-dashboard.png)](media/v2-update-provision/device-seven-dashboard.png#lightbox)

## <a name="list-device-simulation-properties"></a>Eigenschappen van apparaatsimulatie vermelden

Voordat u een Azure Time Series Insights-omgeving maakt, hebt u de namen van uw IoT-hub, abonnement en resourcegroep nodig.

1. Ga naar het dashboard van de oplossingsverbetering en meld u aan met hetzelfde Azure-abonnementsaccount. Zoek de apparaatsimulatie die u in de vorige stappen hebt gemaakt.

1. Klik op uw apparaatsimulator en selecteer **Starten**. Selecteer de koppeling **Azure Management Portal** aan de rechterkant.

    [![Aanbiedingen met Simulator](media/v2-update-provision/device-six-listings.png)](media/v2-update-provision/device-six-listings.png#lightbox)

1. Noteer de namen van de IoT-hub, het abonnement en de resourcegroep.

    [![Azure-portal](media/v2-update-provision/device-eight-portal.png)](media/v2-update-provision/device-eight-portal.png#lightbox)

## <a name="create-a-time-series-insights-preview-payg-environment"></a>Een Time Series Insights Preview-omgeving op basis van betalen per gebruik maken

In deze sectie wordt beschreven hoe u een Azure Time Series Insights Preview-omgeving maakt door de [Azure-portal](https://portal.azure.com/).

1. Meld u aan bij de Azure-portal met uw Azure-abonnementsaccount.

1. Selecteer **Een resource maken**.

1. Selecteer de categorie **Internet of Things** en vervolgens **Time Series Insights**.

   [![Internet of Things selecteren en selecteer vervolgens de Time Series Insights](media/v2-update-provision/payg-one-azure.png)](media/v2-update-provision/payg-one-azure.png#lightbox)

1. Vul de velden op de pagina als volgt in:

   | | |
   | --- | ---|
   | **Omgevingsnaam** | Kies een unieke naam voor de Azure Time Series Insights Preview-omgeving. |
   | **Abonnement** | Voer uw abonnement in waarin u uw Azure Time Series Insights Preview-omgeving wilt maken. Het is een best practice om hetzelfde abonnement te gebruiken als de rest van uw IoT-resources die door de apparaatsimulator worden gemaakt. |
   | **Resourcegroep** | Een resourcegroep is een container voor Azure-resources. Kies een bestaande resourcegroep voor de Azure Time Series Insights Preview-omgevingsresource of maak een nieuwe. Het is een best practice om dezelfde resourcegroep te gebruiken als de rest van uw IoT-resources die door de apparaatsimulator worden gemaakt. |
   | **Locatie** | Kies een datacentrumregio voor uw Azure Time Series Insights Preview-omgeving. Vermijd extra bandbreedtekosten en -latentie door de Azure Time Series Insights Preview-omgeving in dezelfde regio te bewaren als andere IoT-resources. |
   | **Laag** |  Selecteer **Betalen per gebruik**. Dit is de SKU voor het Azure Time Series Insights Preview-product. |
   | **Eigenschaps-ID** | Voer een unieke identificatie voor uw tijdreeks in. Houd er rekening mee dat dit veld onveranderbaar is en later niet meer kan worden gewijzigd. Voor deze zelfstudie gebruiken we `iothub-connection-device-id`. Lees [Een Time Series-ID kiezen](./time-series-insights-update-how-to-id.md) voor meer informatie over Time Series-ID. |
   | **Naam van opslagaccount** | Voer een wereldwijd unieke naam voor een nieuw te maken opslagaccount in. |

   Selecteer vervolgens **Volgende: Bron van gebeurtenis**.

   [![Pagina voor het maken van een Time Series Insights-omgeving](media/v2-update-provision/payg-two-create.png)](media/v2-update-provision/payg-two-create.png#lightbox)

1. Vul de velden op de pagina voor de gebeurtenisbron als volgt in:

   | | |
   | --- | --- |
   | **Een gebeurtenisbron maken** | Voer `Yes` in.|
   | **Naam** | Voer een unieke waarde in als naam voor de gebeurtenisbron.|
   | **Brontype** | Selecteer **IoT-Hub**. |
   | **Een hub selecteren** | Selecteer **Selecteer een bestaande**. |
   | **Abonnement** | Selecteer het abonnement dat u hebt gebruikt voor de apparaatsimulator. |
   | **Naam van de IoT Hub** | Selecteer de naam van de IoT-hub die u hebt gemaakt voor de apparaatsimulator. |
   | **IoT Hub-toegangsbeleid** | Selecteer **iothubowner**. |
   | **IoT Hub-consumentengroep** | U hebt een unieke consumentengroep nodig voor Azure Time Series Insights Preview. Selecteer **Nieuw**, voer een unieke naam in en selecteer vervolgens **Toevoegen**. |
   | **Timestamp-eigenschap** | Dit veld wordt gebruikt om de eigenschap timestamp te identificeren in uw binnenkomende telemetriegegevens. Vul het veld niet in voor deze zelfstudie. In deze simulator wordt de binnenkomende timestamp van IoT Hub gebruikt waarop Time Series Insights standaard is ingesteld.|

   Selecteer vervolgens **Controleren + maken**.

   [![Een gebeurtenisbron configureren](media/v2-update-provision/payg-five-event-source.png)](media/v2-update-provision/payg-five-event-source.png#lightbox)

1. Controleer alle velden op de controlepagina en selecteer **Maken**.

   [![Beoordelen en pagina maken met de knop maken](media/v2-update-provision/payg-six-review.png)](media/v2-update-provision/payg-six-review.png#lightbox)

1. Hier ziet u de status van uw implementatie.

   [![Melding dat de implementatie voltooid is](media/v2-update-provision/payg-seven-deploy.png)](media/v2-update-provision/payg-seven-deploy.png#lightbox)

1. U zou toegang moeten krijgen tot uw Azure Time Series Insights Preview-omgeving als u eigenaar van de tenant bent. Doe het volgende om ervoor te zorgen dat u toegang hebt:

   a. Zoek uw resourcegroep en selecteer uw Azure Time Series Insights Preview-omgeving:

      [![Geselecteerde omgeving](media/v2-update-provision/payg-eight-environment.png)](media/v2-update-provision/payg-eight-environment.png#lightbox)

   b. Ga op de pagina Azure Time Series Insights Preview naar **Beleid voor gegevenstoegang**.

     [![Beleid voor gegevenstoegang](media/v2-update-provision/payg-nine-data-access.png)](media/v2-update-provision/payg-nine-data-access.png#lightbox)

   c. Controleer of uw referenties worden vermeld.

     [![Vermeld referenties](media/v2-update-provision/payg-ten-verify.png)](media/v2-update-provision/payg-ten-verify.png#lightbox)

   Als uw referenties niet worden vermeld, moet u uzelf toestemming geven voor toegang tot de omgeving. Raadpleeg [Gegevenstoegang verlenen](./time-series-insights-data-access.md) voor meer informatie over het instellen van machtigingen.

## <a name="analyze-data-in-your-environment"></a>Gegevens analyseren in uw omgeving

In deze sectie voert u een eenvoudige analyse uit op uw tijdreeksgegevens met de [verkenner van Azure Time Series Insights Preview](./time-series-insights-update-explorer.md).

1. Ga naar de verkenner van Azure Time Series Insights Preview door de URL te selecteren op de resourcepagina in [Azure Portal](https://portal.azure.com/).

   [![De Time Series Insights Preview explorer-URL](media/v2-update-provision/analyze-one-portal.png)](media/v2-update-provision/analyze-one-portal.png#lightbox)

1. Selecteer in de Verkenner, de **Time Series-instanties** knooppunt om te zien van alle exemplaren van een Azure Time Series Insights Preview in de omgeving.

   [![Lijst met niet-bovenliggende instanties](media/v2-update-provision/analyze-two-unparented.png)](media/v2-update-provision/analyze-two-unparented.png#lightbox)

1. In de weergegeven tijdreeks selecteert u het eerste exemplaar. Selecteer vervolgens **Gemiddelde druk weergeven**.

   [![Geselecteerde exemplaar met de opdracht om weer te geven van gemiddelde druk](media/v2-update-provision/analyze-three-show-pressure.png)](media/v2-update-provision/analyze-three-show-pressure.png#lightbox)

   Grafiek met een reeks moet worden weergegeven aan de rechterkant. Pas de **Interval** naar `15s`.

   [![Grafiek-serie](media/v2-update-provision/analyze-four-chart.png)](media/v2-update-provision/analyze-four-chart.png#lightbox)

1. Herhaal **stap 3** bij de andere twee tijdreeksen. Vervolgens kunt u alle tijdreeksen, bekijken, zoals wordt weergegeven in deze grafiek:

   [![Grafiek voor alle tijdreeksen](media/v2-update-provision/analyze-five-chart.png)](media/v2-update-provision/analyze-five-chart.png#lightbox)

1. Wijzig het tijdsbereik om de trends in de tijdreeks van het afgelopen uur te bekijken.

   a. Selecteer de **tijdsbestek** vak:

      [![Stel het tijdsbereik tot een uur](media/v2-update-provision/analyze-six-time.png)](media/v2-update-provision/analyze-six-time.png#lightbox)

## <a name="define-and-apply-a-model"></a>Een model definiëren en toepassen

In dit gedeelte past u een model toe om uw gegevens te structureren. U definieert typen, hiërarchieën en exemplaren om het model te voltooien. Ga naar [Time Series-model](./time-series-insights-update-tsm.md) voor meer informatie over gegevensmodellering.

1. Selecteer het tabblad **Model** in de verkenner:

   [![Model-tabblad in de Verkenner](media/v2-update-provision/define-one-model.png)](media/v2-update-provision/define-one-model.png#lightbox)

1. Selecteer **+ Toevoegen** om een type toe te voegen. Aan de rechterzijde wordt een type-editor geopend.

   [![De knop toevoegen voor typen](media/v2-update-provision/define-two-add.png)](media/v2-update-provision/define-two-add.png#lightbox)

1. Definieer drie variabelen voor het type: druk, temperatuur en vochtigheid. Voer de volgende informatie in:

   | | |
   | --- | ---|
   | **Naam** | Voer `Chiller` in. |
   | **Beschrijving** | Voer `This is a type definition of Chiller` in. |

   * Definieer Druk met drie variabelen:

      | | |
      | --- | ---|
      | **Naam** | Voer `Avg Pressure` in. |
      | **Waarde** | Selecteer **druk (dubbel)**. Het duurt een paar minuten voordat dit veld is ingevuld nadat Azure Time Series Insights Preview gebeurtenissen ontvangt. |
      | **Aggregatiebewerking** | Selecteer **GEMIDDELDE**. |

      [![Selecties voor het definiëren van druk te verlichten](media/v2-update-provision/define-three-variable.png)](media/v2-update-provision/define-three-variable.png#lightbox)

      Selecteer **+ Variabele toevoegen** om de volgende variabele toe te voegen.

   * Definieer Temperatuur:

      | | |
      | --- | ---|
      | **Naam** | Voer `Avg Temperature` in. |
      | **Waarde** | Selecteer **temperatuur (dubbel)**. Het duurt een paar minuten voordat dit veld is ingevuld nadat Azure Time Series Insights Preview gebeurtenissen ontvangt. |
      | **Aggregatiebewerking** | Selecteer **GEMIDDELDE**.|

      [![Selecties voor het definiëren van temperatuur](media/v2-update-provision/define-four-avg.png)](media/v2-update-provision/define-four-avg.png#lightbox)

   * Definieer Vochtigheid:

      | | |
      | --- | ---|
      | **Naam** | Voer `Max Humidity` in |
      | **Waarde** | Selecteer **vochtigheid (dubbel)**. Het duurt een paar minuten voordat dit veld is ingevuld nadat Azure Time Series Insights Preview gebeurtenissen ontvangt. |
      | **Aggregatiebewerking** | Selecteer **MAXIMUM**.|

      [![Selecties voor het definiëren van temperatuur](media/v2-update-provision/define-five-humidity.png)](media/v2-update-provision/define-five-humidity.png#lightbox)

   Ten slotte selecteert u **Create**.

1. Hier ziet u uw toegevoegde type:

   [![Informatie over het type toegevoegd](media/v2-update-provision/define-six-type.png)](media/v2-update-provision/define-six-type.png#lightbox)

1. De volgende stap is om een hiërarchie toe te voegen. In het gedeelte **Hiërarchieën** selecteert u **+ Toevoegen**:

   [![Tabblad hiërarchieën met knop toevoegen](media/v2-update-provision/define-seven-hierarchy.png)](media/v2-update-provision/define-seven-hierarchy.png#lightbox)

1. Definieer de hiërarchie. Vul de velden als volgt in:

   | | |
   | --- | ---|
   | **Naam** | Voer `Location Hierarchy` in. |
   | **Niveau 1** | Voer `Country` in. |
   | **Niveau 2** | Voer `City` in. |
   | **Niveau 3** | Voer `Building` in. |

   Ten slotte selecteert u **Create**.

   [![Hiërarchievelden met de knop maken](media/v2-update-provision/define-eight-add-hierarchy.png)](media/v2-update-provision/define-eight-add-hierarchy.png#lightbox)

1. U kunt de gemaakte hiërarchie bekijken:

   [![Informatie over de hiërarchie](media/v2-update-provision/define-nine-created.png)](media/v2-update-provision/define-nine-created.png#lightbox)

1. Selecteer **Exemplaren** aan de linkerkant. Als de exemplaren verschijnen, selecteert u het eerste exemplaar en vervolgens **Bewerken**:

   [![De knop bewerken voor een exemplaar selecteren](media/v2-update-provision/define-ten-edit.png)](media/v2-update-provision/define-ten-edit.png#lightbox)

1. Aan de rechterzijde wordt een teksteditor geopend. Voeg de volgende informatie toe:

   | | |
   | --- | --- |
   | **Type** | Selecteer **Koelunit**. |
   | **Beschrijving** | Voer `Instance for Chiller-01.1` in. |
   | **Hiërarchieën** | Selecteer **locatie hiërarchie**. |
   | **Land** | Voer `USA` in. |
   | **Plaats** | Voer `Seattle` in. |
   | **Bouwen** | Voer `Space Needle` in. |

    Selecteer vervolgens **Opslaan**.

   [![Exemplaarvelden met knop Opslaan](media/v2-update-provision/define-eleven-chiller.png)](media/v2-update-provision/define-eleven-chiller.png#lightbox)

1. Herhaal de vorige stap voor de overige sensoren. Gebruik de volgende velden:

   * Voor Koelunit 01.2:

     | | |
     | --- | --- |
     | **Type** | Selecteer **Koelunit**. |
     | **Beschrijving** | Voer `Instance for Chiller-01.2` in. |
     | **Hiërarchieën** | Selecteer **locatie hiërarchie**. |
     | **Land** | Voer `USA` in. |
     | **Plaats** | Voer `Seattle` in. |
     | **Bouwen** | Voer `Pacific Science Center` in. |

   * Voor Koelunit 01.3:

     | | |
     | --- | --- |
     | **Type** | Selecteer **Koelunit**. |
     | **Beschrijving** | Voer `Instance for Chiller-01.3` in. |
     | **Hiërarchieën** | Selecteer **locatie hiërarchie**. |
     | **Land** | Voer `USA` in. |
     | **Plaats** | Voer `New York` in. |
     | **Bouwen** | Voer `Empire State Building` in. |

1. Ga naar het tabblad **Analyseren** en vernieuw de pagina. Vouw alle hiërarchieniveaus uit om de tijdreeks te zoeken.

   [![Het tabblad analyseren](media/v2-update-provision/define-twelve.png)](media/v2-update-provision/define-twelve.png#lightbox)

1. Wijzig **Snelle tijden** in **Afgelopen uur** om de tijdreeks van het afgelopen uur te bekijken:

    [![Het vak snelle tijden met afgelopen uur geselecteerd](media/v2-update-provision/define-thirteen-explore.png)](media/v2-update-provision/define-thirteen-explore.png#lightbox)

1. Klik op de tijdreeks onder **Pacific Science Center** en selecteer **Maximale vochtigheid weergeven**.

    [![Geselecteerde tijdreeks met menu-optie Max vochtigheid weergeven](media/v2-update-provision/define-fourteen-show-max.png)](media/v2-update-provision/define-fourteen-show-max.png#lightbox)

1. De tijdreeksen voor **Max vochtigheid** met de intervalgrootte van een van **1 minuut** wordt geopend. Selecteer een regio om te filteren op een bereik. Klik vervolgens met de rechtermuisknop op **Inzoomen** om gebeurtenissen in het tijdsbestek te analyseren:

   [![Geselecteerde bereik met de opdracht Inzoomen op een snelmenu](media/v2-update-provision/define-fifteen-filter.png)](media/v2-update-provision/define-fifteen-filter.png#lightbox)

1. U kunt ook een regio selecteren en vervolgens met de rechtermuisknop klikken om details van een gebeurtenis te bekijken:

   [![Gedetailleerde lijst met gebeurtenissen](media/v2-update-provision/define-eighteen.png)](media/v2-update-provision/define-eighteen.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:  

> [!div class="checklist"]
> * Een verbetering voor apparaatsimulatie maken en gebruiken.
> * Een Time Series Insights Preview-omgeving op basis van betalen per gebruik maken.
> * De Azure Time Series Insights Preview-omgeving verbinden met een Event Hub.
> * Een voorbeeldoplossingsverbetering uitvoeren om gegevens te streamen naar de Azure Time Series Insights Preview-omgeving.
> * Een eenvoudige analyse uitvoeren van de gegevens.
> * Een type Time Series Model en een hiërarchie definiëren en deze koppelen aan uw exemplaren.

Nu u weet hoe u een eigen Azure Time Series Preview-omgeving maakt, is het tijd om meer te leren over de belangrijkste concepten in Azure Time Series Insights.

Meer informatie over de Azure Time Series Insights-opslagconfiguratie:

> [!div class="nextstepaction"]
> [Azure Time Series Insights Preview-opslag en -opname](./time-series-insights-update-storage-ingress.md)

Meer informatie over Time Series-modellen:

> [!div class="nextstepaction"]
> [Azure Time Series Insights Preview-gegevensmodellering](./time-series-insights-update-tsm.md)