---
title: 'Zelfstudie: Gebeurtenissen uit de Azure Digital Twins-installatie analyseren | Microsoft Docs'
description: Leer hoe u met behulp van de stappen in deze zelfstudie gebeurtenissen uit Azure Digital Twins-ruimten kunt visualiseren en analyseren met Azure Time Series Insights.
services: digital-twins
author: alinamstanciu
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 12/18/2018
ms.author: alinast
ms.openlocfilehash: 3f6111457d3438b80ace8cd557747ab8c799efd3
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484743"
---
# <a name="tutorial-visualize-and-analyze-events-from-your-azure-digital-twins-spaces-by-using-time-series-insights"></a>Zelfstudie: Gebeurtenissen uit uw Azure Digital Twins-ruimten visualiseren en analyseren met Time Series Insights

Nadat u het Azure Digital Twins-exemplaar hebt geïmplementeerd, de ruimten hebt ingericht, en een aangepaste functie hebt geïmplementeerd om specifieke voorwaarden te bewaken, kunt u de gebeurtenissen en gegevens visualiseren die afkomstig zijn uit deze ruimten. Op deze manier kunt u eventuele trends en afwijkingen detecteren.

In [de eerste zelfstudie](tutorial-facilities-setup.md) hebt u de ruimtelijke grafiek van een denkbeeldig gebouw geconfigureerd, met hierin een ruimte met sensoren voor beweging, koolstofdioxide en temperatuur. in de [tweede zelfstudie](tutorial-facilities-udf.md) hebt u deze grafiek ingericht, samen met een door de gebruiker gedefinieerde functie. De functie controleert deze sensorwaarden en activeert meldingen voor de juiste voorwaarden. Dat wil zeggen dat de ruimte leeg is en de temperatuur en het koolstofdioxidegehalte normaal zijn.

In deze zelfstudie ziet u hoe u de meldingen en gegevens afkomstig van de Azure Digital Twins-installatie kunt integreren met behulp van Azure Time Series Insights. Vervolgens kunt u de sensorwaarden in de loop van de tijd visualiseren. U kunt trends zoeken, bijvoorbeeld welke ruimte het meest wordt gebruikt en wat de drukste tijdstippen op een dag zijn. U kunt ook afwijkingen detecteren, zoals welke ruimten bedompt of heet aanvoelen, of wanneer voor een gebied in het gebouw consistent hoge temperatuurwaarden worden verzonden, wat duidt op een defecte airconditioning.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Gegevens streamen met behulp van Azure Event Hubs.
> * Analyseren met Time Series Insights.

## <a name="prerequisites"></a>Vereisten

In deze zelfstudie wordt ervan uitgegaan dat u de Azure Digital Twins-installatie hebt [geconfigureerd](tutorial-facilities-setup.md) en [ingericht](tutorial-facilities-udf.md). Voordat u doorgaat, moet u ervoor zorgen voor dat u beschikt over:

- Een [Azure-account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Een actief exemplaar van Digital Twins.
- De gedownloade en uitgepakte [Digital Twins C#-voorbeelden](https://github.com/Azure-Samples/digital-twins-samples-csharp) op een werkcomputer.
- [.NET Core SDK-versie 2.1.403 of hoger](https://www.microsoft.com/net/download) op een ontwikkelcomputer om het voorbeeld uit te voeren. Voer `dotnet --version` uit om te controleren of de juiste versie is geïnstalleerd.

## <a name="stream-data-by-using-event-hubs"></a>Gegevens streamen met Event Hubs

Gebruik de service [Event Hubs](../event-hubs/event-hubs-about.md) om een pijplijn te maken om gegevens te streamen. In deze sectie ziet u hoe u een Event Hub kunt maken als de connector tussen een Azure Digital Twins- en een Time Series Insights-exemplaar.

### <a name="create-an-event-hub"></a>Een Event Hub maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer **Een resource maken** in het linkerdeelvenster.

1. Zoek en selecteer **Event Hubs**. Selecteer **Maken**.

1. Voer een **naam** in voor de naamruimte van de Event Hub. Kies **Standard** voor **Prijscategorie**, uw **Abonnement**, de **Resourcegroep** die u hebt gebruikt voor het Digital Twins-exemplaar, en de **Locatie**. Selecteer **Maken**.

1. Selecteer in de Event Hubs-naamruimte-implementatie, de **overzicht** in het deelvenster Selecteer **naar de resource gaan**.

    ![Event Hubs-naamruimte na implementatie](./media/tutorial-facilities-analyze/open-event-hub-ns.png)

1. Selecteer bovenaan in het deelvenster **Overzicht** van de Event Hubs-naamruimte de knop **Event Hub**.
    ![Knop Event Hub](./media/tutorial-facilities-analyze/create-event-hub.png)

1. Voer een **naam** in voor de Event Hub en selecteer **Maken**.

   Nadat de Event Hub is geïmplementeerd, wordt deze met de status **Actief** weergegeven in het deelvenster **Event Hubs** van de Event Hubs-naamruimte. Selecteer deze Event Hub om het bijbehorende deelvenster **Overzicht** te openen.

1. Selecteer bovenaan de knop **Consumentengroep** en voer een naam in voor de consumentengroep, bijvoorbeeld **tsievents**. Selecteer **Maken**.

    ![Event Hub-consumentengroep](./media/tutorial-facilities-analyze/event-hub-consumer-group.png)

   Nadat de consumenten groep is gemaakt, wordt deze weergegeven in de lijst onderaan het deelvenster **Overzicht** van de Event Hub.

1. Open het deelvenster **Beleid voor gedeelde toegang** voor de Event Hub en selecteer de knop **Toevoegen**. Voer **ManageSend** in als de naam van het beleid, controleer of alle selectievakjes zijn ingeschakeld en selecteer **Maken**.

    ![Event Hub-verbindingsreeksen](./media/tutorial-facilities-analyze/event-hub-connection-strings.png)

1. Open het beleid ManageSend dat u hebt gemaakt, en kopieer de waarden voor **Verbindingsreeks: primaire sleutel** en **Verbindingsreeks: secundaire sleutel** naar een tijdelijk bestand. U hebt deze waarden in de volgende sectie nodig om een eindpunt te maken voor de Event Hub.

### <a name="create-an-endpoint-for-the-event-hub"></a>Eindpunt maken voor de Event Hub

1. Zorg dat u in het opdrachtvenster de map **occupancy-quickstart\src** van het Azure Digital Twins-voorbeeld hebt geopend.

1. Open het bestand **actions\createEndpoints.yaml** in de editor. Vervang de inhoud door het volgende:

    ```yaml
    - type: EventHub
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hub
    - type: EventHub
      eventTypes:
      - DeviceMessage
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hub
    ```

1. Vervang de tijdelijke aanduidingen `Primary_connection_string_for_your_event_hub` door de waarde van **Verbindingsreeks: primaire sleutel** voor de Event Hub. Zorg ervoor dat de indeling van deze verbindingsreeks er als volgt uitziet:

   ```plaintext
   Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey1GUID;EntityPath=nameOfYourEventHub
   ```

1. Vervang de tijdelijke aanduidingen `Secondary_connection_string_for_your_event_hub` door de waarde van **Verbindingsreeks: secundaire sleutel** voor de Event Hub. Zorg ervoor dat de indeling van deze verbindingsreeks er als volgt uitziet: 

   ```plaintext
   Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey2GUID;EntityPath=nameOfYourEventHub
   ```

1. Vervang de tijdelijke aanduidingen `Name_of_your_Event_Hub` door de naam van uw Event Hub.

    > [!IMPORTANT]
    > Voer alle waarden in zonder aanhalingstekens. Zorg dat de dubbele punten in het YAML-bestand worden gevolgd door minstens één spatie. U kunt de inhoud van het YAML-bestand ook valideren met behulp van een YAML-onlinevalidatie, zoals [dit hulpprogramma](https://onlineyamltools.com/validate-yaml).

1. Sla het bestand op en sluit het. Voer de volgende opdracht uit in het opdrachtvenster en meld u aan met uw Azure-account wanneer u hierom wordt gevraagd.

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Hiermee worden twee eindpunten gemaakt voor de Event Hub.

   ![Eindpunten voor Event Hubs](./media/tutorial-facilities-analyze/dotnet-create-endpoints.png)

## <a name="analyze-with-time-series-insights"></a>Analyseren met Time Series Insights

1. Selecteer in het linkerdeelvenster van de [Azure-portal](https://portal.azure.com) de optie **Een resource maken**. 

1. Zoek en selecteer een nieuwe **Time Series Insights**-resource. Selecteer **Maken**.

1. Voer een **naam** in voor het Time Series Insights-exemplaar en selecteer vervolgens uw **abonnement**. Selecteer de **Resourcegroep** die u hebt gebruikt voor het Digital Twins-exemplaar, en uw **Locatie**. Selecteer **Volgende: De bron van gebeurtenis** knop of de **gebeurtenisbron** tabblad.

    ![Selecties voor het maken van een Time Series Insights-exemplaar](./media/tutorial-facilities-analyze/create-tsi.png)

1. In de **gebeurtenisbron** tabblad, voer een **naam**, selecteer **Event Hub** als de **gegevensbrontype**, en zorg ervoor dat de andere waarden zijn geselecteerd correct. Selecteer **ManageSend** voor **Event Hub-beleidsnaam**, en selecteer vervolgens de consumergroep die u hebt gemaakt in de vorige sectie voor **Event Hub-consumentengroep**. Selecteer **Controleren + maken**.

    ![Selecties voor het maken van een gebeurtenisbron](./media/tutorial-facilities-analyze/tsi-event-source.png)

1. In de **beoordelen en maken** deelvenster, Controleer de informatie die u hebt ingevoerd en selecteert u **maken**.

1. Selecteer in het deelvenster met implementatie, de Time Series Insights-resource die u zojuist hebt gemaakt. Het openen van de **overzicht** deelvenster voor uw Time Series Insights-omgeving.

1. Selecteer de **gaat u naar de omgeving** bovenaan op de knop. Als u een waarschuwing voor gegevenstoegang te zien krijgt, opent u het deelvenster **Beleid voor gegevenstoegang** voor het Time Series Insights-exemplaar en selecteert u **Toevoegen**. Vervolgens selecteert u de rol **Inzender** en selecteert u de juiste gebruiker.

1. Met de knop **Ga naar omgeving** wordt de [Time Series Insights-verkenner](../time-series-insights/time-series-insights-explorer.md) geopend. Als hier geen gebeurtenissen worden weergegeven, simuleert u apparaatgebeurtenissen door naar het project **device-connectivity** van het Digital Twins-voorbeeld te gaan en `dotnet run` uit te voeren.

1. Nadat een aantal gesimuleerde gebeurtenissen is gegenereerd, gaar u terug naar de Time Series Insights-verkenner en selecteert u bovenaan de knop Vernieuwen. Als het goed is, ziet u dat er nu analytische grafieken worden gemaakt voor de gesimuleerde sensorgegevens. 

    ![Grafiek met de Time Series Insights-verkenner](./media/tutorial-facilities-analyze/tsi-explorer.png)

1. In de Time Series Insights-verkenner kunt u vervolgens grafieken en heatmaps genereren voor verschillende gebeurtenissen en gegevens van de ruimten, sensoren en andere resources. Gebruik de vervolgkeuzelijsten **MEASURE** en **SPLIT BY** aan de linkerkant om uw eigen visualisaties te maken. 

   Selecteer bijvoorbeeld **Gebeurtenissen** voor **MEASURE** en **DigitalTwins-SensorHardwareId** voor **SPLIT BY** om een heatmap te genereren voor elke sensor. De heatmap ziet er ongeveer uit als op de volgende afbeelding:

   ![Heatmap in de Time Series Insights-verkenner](./media/tutorial-facilities-analyze/tsi-explorer-heatmap.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u Azure Digital Twins niet verder wilt verkennen, kunt u de resources die in deze zelfstudie zijn gemaakt, gerust verwijderen:

1. Klik in het linkermenu in de [Azure-portal](https://portal.azure.com) op **Alle resources**, selecteer de Digital Twins-resourcegroep en selecteer vervolgens **Verwijderen**.

    > [!TIP]
    > Als u problemen hebt bij het verwijderen van uw Digital Twins-exemplaar, is er een service-update met de oplossing hiervoor beschikbaar. Probeer opnieuw of u het exemplaar kunt verwijderen.

2. Verwijder zo nodig de voorbeeldtoepassingen van uw werkcomputer.

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over grafieken voor ruimtelijke intelligentie en objectmodellen in Azure Digital Twins.

> [!div class="nextstepaction"]
> [Begrip van objectmodellen en grafieken voor ruimtelijke intelligentie van Digital Twins](concepts-objectmodel-spatialgraph.md)
