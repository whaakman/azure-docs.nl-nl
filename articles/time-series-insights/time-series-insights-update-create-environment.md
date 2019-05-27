---
title: 'Zelfstudie: Een Azure Time Series Insights Preview-omgeving instellen | Microsoft Docs'
description: Informatie over hoe u een omgeving instelt in Azure Time Series Insights Preview.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 04/25/2019
ms.custom: seodec18
ms.openlocfilehash: 77b7b90b63ffebc14498183fc179b9c8ae76a722
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237850"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Zelfstudie: Een Time Series Insights Preview-omgeving instellen

In deze zelfstudie wordt u door het creatieproces van een Azure Time Series Insights Preview-omgeving op basis van betalen per gebruik geleid. 

In deze zelfstudie leert u het volgende:

* Een Time Series Insights Preview-omgeving maken.
* De Azure Time Series Insights Preview-omgeving verbinden met een Event Hub in Azure Event Hubs.
* Een voorbeeldoplossingsverbetering uitvoeren om gegevens te streamen naar de Azure Time Series Insights Preview-omgeving.
* Voer een eenvoudige analyse van de gegevens uit.
* Een type Time Series Model en een hiërarchie definiëren en deze koppelen aan uw exemplaren.

## <a name="create-a-device-simulation"></a>Een apparaatsimulatie maken

In deze sectie maakt u drie gesimuleerde apparaten die gegevens naar een exemplaar van Azure IoT Hub verzenden.

1. Ga naar [de pagina Azure IoT-oplossingsverbeteringen](https://www.azureiotsolutions.com/Accelerators). Op de pagina worden enkele vooraf gedefinieerde voorbeelden weergegeven. Meld u aan met uw Azure-account. Selecteer vervolgens **Apparaatsimulatie**.

   [![Azure IoT-oplossing accelerators pagina](media/v2-update-provision/device-one-accelerator.png)](media/v2-update-provision/device-one-accelerator.png#lightbox)

   Selecteer **Nu uitproberen**.

1. Op de **maken Apparaatsimulatie-oplossing** pagina, stelt u de volgende parameters:

    | Parameter | Bewerking |
    | --- | --- |
    | **Naam van de oplossing** | Voer een unieke waarde voor een nieuwe resourcegroep. De vermelde Azure-resources worden gemaakt en toegewezen aan de resourcegroep. |
    | **Abonnement** | Selecteer het abonnement dat u gebruikt om uw Time Series Insights-omgeving te maken. |
    | **Regio** | Selecteer de regio die u hebt gebruikt om uw Time Series Insights-omgeving te maken. |
    | **Optionele Azure-resources implementeren** | Laat de **IoT-Hub** selectievakje is ingeschakeld. De gesimuleerde apparaten gebruikt IoT-Hub verbinding maken met en gegevens streamen. |
 
    Selecteer **oplossing maken**. Wacht 10-15 minuten totdat uw oplossing is geïmplementeerd.

    [![De pagina van de Apparaatsimulatie-oplossing maken](media/v2-update-provision/device-two-create.png)](media/v2-update-provision/device-two-create.png#lightbox)

1. Selecteer in het oplossingsdashboard accelerator **starten**:

    [![De simulatie apparaat oplossing starten](media/v2-update-provision/device-three-launch.png)](media/v2-update-provision/device-three-launch.png#lightbox)

1. U wordt omgeleid naar de pagina **Microsoft Azure IoT-apparaatsimulatie**. Selecteer in de rechterbovenhoek van de pagina **nieuwe simulatie**.

    [![Pagina van Azure IoT-simulatie](media/v2-update-provision/device-four-iot-sim-page.png)](media/v2-update-provision/device-four-iot-sim-page.png#lightbox)

1. In de **simulatie setup** in het deelvenster de volgende parameters instellen:

    | Parameter | Bewerking |
    | --- | --- |
    | **Naam** | Voer een unieke naam voor een simulator in. |
    | **Beschrijving** | Voer een definitie in. |
    | **Simulatieduur** | Stel de simulatieduur in op **Voor onbepaalde tijd uitvoeren**. |
    | **Apparaatmodel** | **Naam**: Voer **Koelunit** in. <br />**Aantal**: Voer **3** in. |
    | **Doel-IoT-hub** | Stel **Vooraf ingerichte IoT-hub gebruiken** in. |

    [![Parameters instellen](media/v2-update-provision/device-five-params.png)](media/v2-update-provision/device-five-params.png#lightbox)

    Selecteer **simulatie starten**.

    Houd er rekening mee de informatie die wordt weergegeven voor in het dashboard van de simulatie apparaat **actieve apparaten** en **berichten per seconde**.

    [![Dashboard van Azure IoT-simulatie](media/v2-update-provision/device-seven-dashboard.png)](media/v2-update-provision/device-seven-dashboard.png#lightbox)

## <a name="list-device-simulation-properties"></a>Eigenschappen van apparaatsimulatie vermelden

Voordat u een Azure Time Series Insights-omgeving maakt, moet u de namen van uw IoT-hub, uw abonnement en de resourcegroep.

1. Ga naar de solution accelerator-dashboard. Meld u aan met behulp van hetzelfde account Azure-abonnement. Zoek de apparaatsimulatie die u in de voorgaande sectie hebt gemaakt.

1. Selecteer uw apparaatsimulator, en selecteer vervolgens **starten**. Selecteer in de simulator solution accelerator in het deelvenster apparaat aan de rechterkant, het **Azure Management Portal** optie.

    [![Aanbiedingen met Simulator](media/v2-update-provision/device-six-listings.png)](media/v2-update-provision/device-six-listings.png#lightbox)

1. Noteer de namen van de IoT hub-abonnement en resource-groep.

    [![Azure portal simulator dashboard Apparaatdetails](media/v2-update-provision/device-eight-portal.png)](media/v2-update-provision/device-eight-portal.png#lightbox)

## <a name="create-a-time-series-insights-preview-payg-environment"></a>Een Time Series Insights Preview-omgeving op basis van betalen per gebruik maken

In deze sectie wordt beschreven hoe u een Azure Time Series Insights Preview-omgeving maakt door de [Azure-portal](https://portal.azure.com/).

1. Meld u aan bij de Azure-portal met uw Azure-abonnementsaccount.

1. Selecteer **een resource maken** > **Internet of Things** > **Time Series Insights**.

   [![Internet of Things selecteren en selecteer vervolgens de Time Series Insights](media/v2-update-provision/payg-one-azure.png)](media/v2-update-provision/payg-one-azure.png#lightbox)

1. In de **maken Time Series Insights-omgeving** deelvenster op de **basisbeginselen** tabblad, stelt u de volgende parameters:

    | Parameter | Bewerking |
    | --- | ---|
    | **Omgevingsnaam** | Voer een unieke naam voor de Azure Time Series Insights Preview-omgeving. |
    | **Abonnement** | Voer het abonnement waarin u wilt maken van de Azure Time Series Insights Preview-omgeving. Een best practice is het gebruik van hetzelfde abonnement als de rest van de IoT-resources die zijn gemaakt door de apparaatsimulator. |
    | **Resourcegroep** | Selecteer een bestaande resourcegroep of maak een nieuwe resourcegroep voor de resource van Azure Time Series Insights Preview-omgeving. Een resourcegroep is een container voor Azure-resources. Een best practice is het gebruik van dezelfde resourcegroep bevinden als de andere IoT-resources die zijn gemaakt door de apparaatsimulator. |
    | **Locatie** | Selecteer de regio van een datacenter voor uw Azure Time Series Insights Preview-omgeving. Om te voorkomen dat extra bandbreedtekosten en latentie, is het raadzaam te maken van uw Azure Time Series Insights Preview-omgeving in dezelfde regio als uw andere IoT-resources. |
    | **Laag** |  Selecteer **PAYG** (*betalen per gebruik*). Dit is de SKU voor het Azure Time Series Insights Preview-product. |
    | **Eigenschaps-ID** | Voer een waarde die een unieke identificatie van uw exemplaar van de reeks tijd. De waarde die u in de **eigenschaps-ID** vak is onveranderbaar. U niet meer deze later wijzigen. Voer voor deze zelfstudie **iothub-verbinding-apparaat-id**. Zie voor meer informatie over Time Series-ID, [aanbevolen procedures voor het kiezen van een Time Series-ID](./time-series-insights-update-how-to-id.md). |
    | **Naam van opslagaccount** | Voer een unieke naam voor een nieuw opslagaccount te maken. |
   
   Selecteer **Volgende: Bron van gebeurtenis**.

   [![Deelvenster voor het maken van een Time Series Insights-omgeving](media/v2-update-provision/payg-two-create.png)](media/v2-update-provision/payg-two-create.png#lightbox)

1. Op de **gebeurtenisbron** tabblad, stelt u de volgende parameters:

   | Parameter | Bewerking |
   | --- | --- |
   | **Een gebeurtenisbron maken** | Selecteer **Ja**.|
   | **Naam** | Voer een unieke waarde voor de naam van de gebeurtenis. |
   | **Brontype** | Selecteer **IoT-Hub**. |
   | **Selecteer een hub** | Kies **Selecteer een bestaande**. |
   | **Abonnement** | Selecteer het abonnement dat u hebt gebruikt voor de apparaatsimulator. |
   | **Naam van de IoT Hub** | Selecteer de naam van de IoT-hub die u hebt gemaakt voor de apparaatsimulator. |
   | **IoT Hub-toegangsbeleid** | Selecteer **iothubowner**. |
   | **IoT Hub-consumentengroep** | Selecteer **Nieuw**, voer een unieke naam in en selecteer vervolgens **Toevoegen**. De consumentengroep moet een unieke waarde in Azure Time Series Insights Preview. |
   | **Timestamp-eigenschap** | Deze waarde wordt gebruikt om te identificeren de **Timestamp** eigenschap in de binnenkomende telemetriegegevens. Voor deze zelfstudie laat u dit vak leeg zijn. In deze simulator wordt de binnenkomende timestamp van IoT Hub gebruikt waarop Time Series Insights standaard is ingesteld. |

   Selecteer **Controleren + maken**.

   [![Een gebeurtenisbron configureren](media/v2-update-provision/payg-five-event-source.png)](media/v2-update-provision/payg-five-event-source.png#lightbox)

1. Op de **beoordelen en maken** tabblad Controleer uw selecties en selecteer vervolgens **maken**.

    [![Beoordelen en pagina maken met de knop maken](media/v2-update-provision/payg-six-review.png)](media/v2-update-provision/payg-six-review.png#lightbox)

    Hier ziet u de status van uw implementatie:

    [![Melding dat de implementatie voltooid is](media/v2-update-provision/payg-seven-deploy.png)](media/v2-update-provision/payg-seven-deploy.png#lightbox)

1. U hebt toegang tot uw Azure Time Series Insights Preview-omgeving als u de tenant eigenaar bent. Doe het volgende om ervoor te zorgen dat u toegang hebt:

   1. Zoek de resourcegroep en selecteer vervolgens uw Azure Time Series Insights Preview-omgeving:

      [![Geselecteerde omgeving](media/v2-update-provision/payg-eight-environment.png)](media/v2-update-provision/payg-eight-environment.png#lightbox)

   1. Selecteer op de pagina Azure Time Series Insights Preview **beleid voor gegevenstoegang**:

      [![Beleid voor gegevenstoegang](media/v2-update-provision/payg-nine-data-access.png)](media/v2-update-provision/payg-nine-data-access.png#lightbox)

   1. Controleer of dat uw referenties worden weergegeven:

      [![Vermeld referenties](media/v2-update-provision/payg-ten-verify.png)](media/v2-update-provision/payg-ten-verify.png#lightbox)

   Als uw referenties worden niet weergegeven, moet u uzelf machtiging voor toegang tot de omgeving verlenen. Raadpleeg [Gegevenstoegang verlenen](./time-series-insights-data-access.md) voor meer informatie over het instellen van machtigingen.

## <a name="analyze-data-in-your-environment"></a>Gegevens analyseren in uw omgeving

In deze sectie voert u een eenvoudige analyse uit op uw tijdreeksgegevens met de [verkenner van Azure Time Series Insights Preview](./time-series-insights-update-explorer.md).

1. Ga naar de verkenner van Azure Time Series Insights Preview door de URL te selecteren op de resourcepagina in [Azure Portal](https://portal.azure.com/).

    [![De Time Series Insights Preview explorer-URL](media/v2-update-provision/analyze-one-portal.png)](media/v2-update-provision/analyze-one-portal.png#lightbox)

1. Selecteer in de Verkenner, de **Time Series-instanties** knooppunt om te zien van alle exemplaren van een Azure Time Series Insights Preview in de omgeving.

    [![Lijst met niet-bovenliggende instanties](media/v2-update-provision/analyze-two-unparented.png)](media/v2-update-provision/analyze-two-unparented.png#lightbox)

1. Selecteer het eerste exemplaar van de time-series. Selecteer **druk te verlichten weergeven**.

    [![Time series exemplaar met de opdracht om weer te geven van gemiddelde druk te verlichten geselecteerd](media/v2-update-provision/analyze-three-show-pressure.png)](media/v2-update-provision/analyze-three-show-pressure.png#lightbox)

    Grafiek met een reeks wordt weergegeven. Wijzig de **Interval** naar **gates 15**.

    [![Grafiek-serie](media/v2-update-provision/analyze-four-chart.png)](media/v2-update-provision/analyze-four-chart.png#lightbox)

1. Herhaal stap 3 met de andere twee time series-instanties. U kunt alle exemplaren van time series, bekijken, zoals wordt weergegeven in deze grafiek:

    [![Grafiek voor alle tijdreeksen](media/v2-update-provision/analyze-five-chart.png)](media/v2-update-provision/analyze-five-chart.png#lightbox)

1. In de **tijdsbestek** box optie, wijzigt u het tijdsbereik om te zien van time series trends in het afgelopen uur:

    [![Stel het tijdsbereik tot een uur](media/v2-update-provision/analyze-six-time.png)](media/v2-update-provision/analyze-six-time.png#lightbox)

## <a name="define-and-apply-a-model"></a>Een model definiëren en toepassen

In dit gedeelte past u een model toe om uw gegevens te structureren. U definieert typen, hiërarchieën en exemplaren om het model te voltooien. Zie voor meer informatie over het modelleren van de gegevens, [Tijdreeksmodel](./time-series-insights-update-tsm.md).

1. Selecteer het tabblad **Model** in de verkenner:

   [![Model-tabblad in de Verkenner](media/v2-update-provision/define-one-model.png)](media/v2-update-provision/define-one-model.png#lightbox)

1. Selecteer **toevoegen** een type toevoegen:

   [![De knop toevoegen voor typen](media/v2-update-provision/define-two-add.png)](media/v2-update-provision/define-two-add.png#lightbox)

1. Vervolgens definieert u drie variabelen voor het type: *druk te verlichten*, *temperatuur*, en *vochtigheid*. In de **toevoegen van een Type** in het deelvenster de volgende parameters instellen:

    | Parameter | Bewerking |
    | --- | ---|
    | **Naam** | Voer **Koelunit** in. |
    | **Beschrijving** | Voer **Dit is een typedefinitie van Koelunit** in. |

   * Voor het definiëren van *druk te verlichten*onder **variabelen**, de volgende parameters instellen:

     | Parameter | Bewerking |
     | --- | ---|
     | **Naam** | Voer **Gemiddelde druk** in. |
     | **Waarde** | Selecteer **druk (dubbel)** . Het kan enkele minuten duren voordat **waarde** automatisch wordt gevuld na het starten van Azure Time Series Insights Preview er gebeurtenissen worden ontvangen. |
     | **Aggregatiebewerking** | Selecteer **GEMIDDELDE**. |

      [![Selecties voor het definiëren van druk te verlichten](media/v2-update-provision/define-three-variable.png)](media/v2-update-provision/define-three-variable.png#lightbox)

      Selecteer om de volgende variabele toe **variabele toevoegen**.

   * Definieer *temperatuur*:

     | Parameter | Bewerking |
     | --- | ---|
     | **Naam** | Voer **Gemiddelde temperatuur** in. |
     | **Waarde** | Selecteer **temperatuur (dubbel)** . Het kan enkele minuten duren voordat **waarde** automatisch wordt gevuld na het starten van Azure Time Series Insights Preview er gebeurtenissen worden ontvangen. |
     | **Aggregatiebewerking** | Selecteer **GEMIDDELDE**.|

      [![Selecties voor het definiëren van temperatuur](media/v2-update-provision/define-four-avg.png)](media/v2-update-provision/define-four-avg.png#lightbox)

      Selecteer om de volgende variabele toe **variabele toevoegen**.

   * Definieer *vochtigheid*:

      | | |
      | --- | ---|
      | **Naam** | Voer **Max vochtigheid.** |
      | **Waarde** | Selecteer **vochtigheid (dubbel)** . Het kan enkele minuten duren voordat **waarde** automatisch wordt gevuld na het starten van Azure Time Series Insights Preview er gebeurtenissen worden ontvangen. |
      | **Aggregatiebewerking** | Selecteer **MAXIMUM**.|

      [![Selecties voor het definiëren van temperatuur](media/v2-update-provision/define-five-humidity.png)](media/v2-update-provision/define-five-humidity.png#lightbox)

    Selecteer **Maken**.

    Hier ziet u het type dat u hebt toegevoegd:

    [![Informatie over het type toegevoegd](media/v2-update-provision/define-six-type.png)](media/v2-update-provision/define-six-type.png#lightbox)

1. De volgende stap is om een hiërarchie toe te voegen. Onder **hiërarchieën**, selecteer **toevoegen**:

    [![Tabblad hiërarchieën met knop toevoegen](media/v2-update-provision/define-seven-hierarchy.png)](media/v2-update-provision/define-seven-hierarchy.png#lightbox)

1. In de **hiërarchie bewerken** in het deelvenster de volgende parameters instellen:

   | Parameter | Bewerking |
   | --- | ---|
   | **Naam** | Voer **Locatiehiërarchie** in. |
   | **Niveau 1** | Voer **Land** in. |
   | **Niveau 2** | Voer **Plaats** in. |
   | **Niveau 3** | Voer **Gebouw** in. |

   Selecteer **Opslaan**.

    [![Hiërarchievelden met de knop maken](media/v2-update-provision/define-eight-add-hierarchy.png)](media/v2-update-provision/define-eight-add-hierarchy.png#lightbox)

   U kunt de gemaakte hiërarchie bekijken:

    [![Informatie over de hiërarchie](media/v2-update-provision/define-nine-created.png)](media/v2-update-provision/define-nine-created.png#lightbox)

1. Selecteer **exemplaren**. Selecteer het eerste exemplaar en selecteer vervolgens **bewerken**:

    [![De knop bewerken voor een exemplaar selecteren](media/v2-update-provision/define-ten-edit.png)](media/v2-update-provision/define-ten-edit.png#lightbox)

1. In de **exemplaren bewerken** in het deelvenster de volgende parameters instellen:

    | Parameter | Bewerking |
    | --- | --- |
    | **Type** | Selecteer **Koelunit**. |
    | **Beschrijving** | Voer **Exemplaar voor Koelunit-01.1** in. |
    | **Hiërarchieën** | Selecteer **locatie hiërarchie**. |
    | **Land** | Voer **USA** in. |
    | **Plaats** | Voer **Seattle** in. |
    | **Bouwen** | Voer **Space Needle** in. |

    [![Exemplaarvelden met de knop Opslaan](media/v2-update-provision/define-eleven-chiller.png)](media/v2-update-provision/define-eleven-chiller.png#lightbox)

   Selecteer **Opslaan**.

1. Herhaal de vorige stap voor de andere sensoren. Werk de volgende waarden bij:

   * Voor Koelunit 01.2:

     | Parameter | Bewerking |
     | --- | --- |
     | **Type** | Selecteer **Koelunit**. |
     | **Beschrijving** | Voer **Exemplaar voor Koelunit-01.2** in. |
     | **Hiërarchieën** | Selecteer **locatie hiërarchie**. |
     | **Land** | Voer **USA** in. |
     | **Plaats** | Voer **Seattle** in. |
     | **Bouwen** | Voer **Pacific Science Center** in. |

   * Voor Koelunit 01.3:

     | Parameter | Bewerking |
     | --- | --- |
     | **Type** | Selecteer **Koelunit**. |
     | **Beschrijving** | Voer **-exemplaar voor Koelunit 01.3**. |
     | **Hiërarchieën** | Selecteer **locatie hiërarchie**. |
     | **Land** | Voer **USA** in. |
     | **Plaats** | Voer **New York** in. |
     | **Bouwen** | Voer **Empire State Building** in. |

1. Selecteer de **analyseren** tabblad en vernieuw de pagina. Onder **locatie hiërarchie**, vouw alle hiërarchieniveaus om de time series-instanties weer te geven:

   [![Het tabblad analyseren](media/v2-update-provision/define-twelve.png)](media/v2-update-provision/define-twelve.png#lightbox)

1. Als u wilt verkennen de time series-instanties in het afgelopen uur, wijzigen **snelle tijden** naar **afgelopen uur**:

    [![Het vak snelle tijden met afgelopen uur geselecteerd](media/v2-update-provision/define-thirteen-explore.png)](media/v2-update-provision/define-thirteen-explore.png#lightbox)

1. Onder **Pacific Science Center**, selecteer het exemplaar van de reeks tijd, en selecteer vervolgens **weergeven Max vochtigheid**.

    [![Geselecteerde reeks exemplaar en de menu-optie Max vochtigheid weergeven](media/v2-update-provision/define-fourteen-show-max.png)](media/v2-update-provision/define-fourteen-show-max.png#lightbox)

1. De tijdreeksen voor **Max vochtigheid** met de intervalgrootte van een van **1 minuut** wordt geopend. Als u wilt filteren op een bereik, selecteer een regio. Voor het analyseren van gebeurtenissen in het tijdsbestek, met de rechtermuisknop op de grafiek en selecteer vervolgens **zoomen**:

   [![Geselecteerde bereik met de opdracht Inzoomen op een snelmenu](media/v2-update-provision/define-fifteen-filter.png)](media/v2-update-provision/define-fifteen-filter.png#lightbox)

1. Zie de details van gebeurtenis, selecteer een regio en vervolgens met de rechtermuisknop op de grafiek:

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
> [Azure Time Series Insights Preview opslag en inkomend](./time-series-insights-update-storage-ingress.md)

Meer informatie over Time Series-modellen:

> [!div class="nextstepaction"]
> [Azure Time Series Insights Preview-gegevensmodellering](./time-series-insights-update-tsm.md)