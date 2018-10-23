---
title: Gebeurtenissen uit de Azure Digital Twins-installatie analyseren | Microsoft Docs
description: Leer hoe u met behulp van de stappen in deze zelfstudie gebeurtenissen uit Azure Digital Twins-ruimten kunt visualiseren en analyseren met Azure Time Series Insights.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 1366cafe5d2c526e86905c108b9c7b865aac8f69
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323218"
---
# <a name="tutorial-visualize-and-analyze-events-from-your-azure-digital-twins-spaces-using-time-series-insights"></a>Zelfstudie: gebeurtenissen uit Azure Digital Twins-ruimten visualiseren en analyseren met Time Series Insights

Nadat u het Azure Digital Twins-exemplaar hebt geïmplementeerd, de ruimten hebt ingericht, en een aangepaste functie hebt geïmplementeerd om specifieke voorwaarden te bewaken, kunt u vervolgens de gebeurtenissen en gegevens visualiseren die afkomstig zijn uit deze ruimten. Op deze manier kunt u eventuele trends en afwijkingen detecteren. 

In [de eerste zelfstudie](tutorial-facilities-setup.md) hebt u de ruimtelijke grafiek van een denkbeeldig gebouw geconfigureerd, met hierin een ruimte met sensoren voor beweging, koolstofdioxide en temperatuur. in de [tweede zelfstudie](tutorial-facilities-udf.md) hebt u deze grafiek ingericht, samen met een door de gebruiker gedefinieerde functie. Met de functie worden deze sensorwaarden gecontroleerd en worden meldingen geactiveerd voor de juiste voorwaarden, dat wil zeggen: als de ruimte leeg is, en het temperatuur- en koolstofdioxideniveau normaal zijn. In deze zelfstudie ziet u hoe u de meldingen en gegevens afkomstig van de Digital Twins-installatie kunt integreren met behulp van Azure Time Series Insights. U kunt de sensorwaarden vervolgens gedurende een bepaalde tijd visualiseren en kijken of u trends kunt ontdekken, bijvoorbeeld welke ruimte het meeste wordt gebruikt, wat de drukste tijden van de dag zijn, enzovoort. U kunt ook afwijkingen detecteren, zoals welke ruimten bedompt of heet aanvoelen, of wanneer voor een gebied in het gebouw consistent hoge temperatuurwaarden worden verzonden, wat duidt op een defecte airconditioning.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Gegevens streamen met Event Hubs
> * Analyseren met Time Series Insights

## <a name="prerequisites"></a>Vereisten

In deze zelfstudie wordt ervan uitgegaan dat u de Azure Digital Twins-installatie hebt [geconfigureerd](tutorial-facilities-setup.md) en [ingericht](tutorial-facilities-udf.md). Zorg er, vóórdat u doorgaat, voor dat u beschikt over:
- Een [Azure-account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Een actief exemplaar van Digital Twins.
- De gedownloade en uitgepakte [Digital Twins C#-voorbeelden](https://github.com/Azure-Samples/digital-twins-samples-csharp) op een werkcomputer.
- [.NET Core SDK-versie 2.1.403 of hoger](https://www.microsoft.com/net/download) op een ontwikkelcomputer om het voorbeeld uit te voeren. `dotnet --version` hebt uitgevoerd om te controleren of de juiste versie is geïnstalleerd. 


## <a name="stream-data-using-event-hubs"></a>Gegevens streamen met Event Hubs
Met de service [Event Hubs](../event-hubs/event-hubs-about.md) kunt u een pijplijn maken om gegevens te streamen. In deze sectie ziet u hoe u een Event Hub kunt maken als de connector tussen een Digital Twins- en een TSI-exemplaar.

### <a name="create-an-event-hub"></a>Een Event Hub maken

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

1. Klik in het navigatiepaneel aan de linkerkant op **Resource maken**. 

1. Zoek en selecteer **Event Hubs**. Klik op **Create**.

1. Voer een **naam** in voor de Event Hubs-naamruimte, kies de *Standard*-**prijslaag**, uw **abonnement**, de **resourcegroep** die u hebt gebruikt voor het Digital Twins-exemplaar, en de **locatie**. Klik op **Create**. 

1. Ga, zodra de implementatie is voltooid, naar de *implementatie* van de Event Hubs-naamruimte en klik onder **RESOURCE** op de naamruimte.

    ![Event hub-naamruimte](./media/tutorial-facilities-analyze/open-event-hub-ns.png)


1. Klik in het deelvenster **Overzicht** van de Event Hubs-naamruimte op de knop **Event Hub** bovenaan. 
    ![Event Hub](./media/tutorial-facilities-analyze/create-event-hub.png)

1. Voer een **naam** in voor de Event Hub en klik op **Maken**. Zodra de implementatie is voltooid, wordt deze weergegeven in het deelvenster **Event Hubs** van de Event Hubs-naamruimte met de **STATUS** *Actief*. Klik op deze Event Hub om het bijbehorende deelvenster **Overzicht** te openen.

1. Klik bovenaan op de knop **Consumentengroep** en voer een naam in voor de consumentengroep, bijvoorbeeld *tsigebeurtenissen*. Klik op **Create**.
    ![Event Hub-consumentengroep](./media/tutorial-facilities-analyze/event-hub-consumer-group.png)

   Nadat de consumenten groep is gemaakt, wordt deze weergegeven in de lijst onderaan het deelvenster **Overzicht** van de Event Hub. 

1. Open het deelvenster **Beleid voor gedeelde toegang** voor de Event Hub en klik op de knop **Toevoegen**. **Maak** een beleid met de naam *ManageSend*, en zorg ervoor dat alle selectievakjes zijn ingeschakeld. 

    ![Event Hub-verbindingsreeksen](./media/tutorial-facilities-analyze/event-hub-connection-strings.png)

1. Open het beleid *ManageSend* dat u hebt gemaakt, en kopieer de waarden voor **Verbindingsreeks: primaire sleutel** en **Verbindingsreeks: secundaire sleutel** naar een tijdelijk bestand. U hebt deze waarden in de volgende sectie nodig om een eindpunt te maken voor de Event Hub.

### <a name="create-endpoint-for-the-event-hub"></a>Eindpunt maken voor de Event Hub

1. Zorg dat u in het opdrachtvenster de map **_occupancy-quickstart\src** van het Digital Twins-voorbeeld hebt geopend.

1. Open het bestand **_actions\createEndpoints.yaml_** in de editor. Vervang de inhoud door het volgende:

    ```yaml
    - type: EventHub
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hubs_namespace
    - type: EventHub
      eventTypes:
      - DeviceMessage
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hubs_namespace
    ```

1. Vervang de tijdelijke aanduidingen `Primary_connection_string_for_your_event_hub` door de waarde van de **Verbindingsreeks: primaire sleutel** voor de Event Hub. Zorg ervoor dat de indeling van deze verbindingsreeks er als volgt uitziet:
```
Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey1GUID;EntityPath=nameOfYourEventHub
```

1. Vervang de tijdelijke aanduidingen `Secondary_connection_string_for_your_event_hub` door de waarde van de **Verbindingsreeks: secundaire sleutel** voor de Event Hub. Zorg ervoor dat de indeling van deze verbindingsreeks er als volgt uitziet: 
```
Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey2GUID;EntityPath=nameOfYourEventHub
```

1. Vervang de tijdelijke aanduidingen `Name_of_your_Event_Hubs_namespace` door de naam van de Event Hubs-naamruimte.

    > [!IMPORTANT]
    > Voer alle waarden in zonder aanhalingstekens. Zorg dat de dubbele punten in het *YAML*-bestand worden gevolgd door minstens één spatie. U kunt de inhoud van het *YAML*-bestand ook valideren met behulp van een YAML-onlinevalidatie, zoals [dit hulpprogramma](https://onlineyamltools.com/validate-yaml).


1. Sla het bestand op en sluit het. Voer de volgende opdracht uit in het opdrachtvenster en meld u aan met uw Azure-account wanneer u hierom wordt gevraagd.

    ```cmd/sh
    dotnet run CreateEndpoints
    ```
   
   Hiermee worden twee eindpunten gemaakt voor de Event Hub.

   ![Eindpunten voor Event Hubs](./media/tutorial-facilities-analyze/dotnet-create-endpoints.png)

## <a name="analyze-with-time-series-insights"></a>Analyseren met Time Series Insights

1. Klik in de [Azure-portal](https://portal.azure.com) in het navigatiepaneel aan de linkerkant op **Een resource maken**. 

1. Zoek en selecteer een nieuwe **Time Series Insights**-resource. Klik op **Create**.

1. Voer een **naam** in voor het Time Series Insights-exemplaar en selecteer vervolgens uw **abonnement**. Selecteer de **resourcegroep** die u hebt gebruikt voor het Digital Twins-exemplaar, en uw **locatie**. Klik op **Create**.

    ![TSI maken](./media/tutorial-facilities-analyze/create-tsi.png)

1. Open, zodra de implementatie is voltooid, de Time Series Insights-omgeving en open vervolgens het bijbehorende deelvenster **Gebeurtenisbronnen**. Klik bovenaan op de knop **Toevoegen** om een consumentengroep toe te voegen.

1. Voer in het deelvenster **Nieuwe gebeurtenisbron** een **naam** in en controleer of de andere waarden juist zijn geselecteerd. Selecteer *ManageSend* als de **naam voor het Event Hub-beleid**, en selecteer vervolgens de *consumentengroep* die u hebt gemaakt in de vorige sectie, als de **Event Hub-consumentengroep**. Klik op **Create**.

    ![TSI-gebeurtenisbron](./media/tutorial-facilities-analyze/tsi-event-source.png)

1. Open het deelvenster **Overzicht** van de Time Series Insights-omgeving en klik bovenaan op de knop **Ga naar omgeving**. Als u een *waarschuwing voor gegevenstoegang* te zien krijgt, opent u het deelvenster **Beleid voor gegevenstoegang** voor het TSI-exemplaar en klikt u op **Toevoegen**. Vervolgens selecteert u de rol **Inzender** en selecteert u de juiste gebruiker.

1. Met de knop **Ga naar omgeving** wordt de [Time Series Insights-verkenner](../time-series-insights/time-series-insights-explorer.md) geopend. Als hier geen gebeurtenissen worden weergegeven, simuleert u apparaatgebeurtenissen door naar het project **_device-connectivity_** van het Digital Twins-voorbeeld te gaan en `dotnet run` uit te voeren.

1. Nadat een aantal gesimuleerde gebeurtenissen is gegenereerd, gaar u terug naar de Time Series Insights-verkenner en klikt u bovenaan op de knop Vernieuwen. Als het goed is, ziet u dat er nu analytische grafieken worden gemaakt voor de gesimuleerde sensorgegevens. 

    ![TSI-verkenner](./media/tutorial-facilities-analyze/tsi-explorer.png)

1. In de Time Series-verkenner kunt u vervolgens grafieken en heatmaps genereren voor verschillende gebeurtenissen en gegevens van de ruimten, sensoren en andere resources. Klik aan de linkerkant op de vakken van vervolgkeuzelijsten met de namen **MEASURE** en **SPLIT BY** om uw eigen visualisaties te maken. Selecteer bijvoorbeeld *Gebeurtenissen* bij **MEASURE** en *DigitalTwins-SensorHardwareId* bij **SPLIT BY** om een heatmap te genereren voor elke sensor, vergelijkbaar met de volgende afbeelding:

    ![TSI-verkenner](./media/tutorial-facilities-analyze/tsi-explorer-heatmap.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u Azure Digital Twins niet verder wilt verkennen, kunt u de resources die in deze zelfstudie zijn gemaakt, gerust verwijderen:

1. Klik in het linkermenu in de [Azure-portal](http://portal.azure.com) op **Alle resources**, selecteer de Digital Twins-resourcegroep en kies **Verwijderen**.
2. Als u wilt, kunt u ook de voorbeeldtoepassingen op de werkcomputer verwijderen. 


## <a name="next-steps"></a>Volgende stappen

Ga door met het volgende artikel voor meer informatie over grafieken voor ruimtelijke intelligentie en objectmodellen in Azure Digital Twins. 
> [!div class="nextstepaction"]
> [Begrip van objectmodellen en grafieken voor ruimtelijke intelligentie van Digital Twins](concepts-objectmodel-spatialgraph.md)

