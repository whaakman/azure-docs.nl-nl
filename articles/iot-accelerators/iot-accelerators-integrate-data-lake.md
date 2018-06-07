---
title: De externe controle-oplossing integreren met Azure Data Lake Store | Microsoft Docs
description: Ontdek hoe u de externe controle-oplossing integreren met Azure Data Lake Store met behulp van een Azure Stream Analytics-taak.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 04/29/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 3bd29e348fd067c12def8ca36fbdc1d7e35b2874
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627583"
---
# <a name="integrate-the-remote-monitoring-solution-with-azure-data-lake-store"></a>De externe controle-oplossing integreren met Azure Data Lake Store

U hebt mogelijk analytics vereisten afgezien van wat wordt aangeboden in de oplossing voor externe controle geavanceerde. Azure Data Lake Store is ideaal voor deze toepassing omdat kunt opslaan van gegevens van grootschalige en diverse gegevenssets evenals integreren met Azure Data Lake Analytics voor analyse op aanvraag.

In deze instructies gebruikt u een Azure Stream Analytics-taak gegevens van de stroom van de IoT-hub in uw oplossing voor externe controle met een Azure Data Lake Store.

## <a name="prerequisites"></a>Vereisten

Als u deze instructies, moet u het volgende:

* [Implementeren van de oplossing voor externe controle accelerator](iot-accelerators-remote-monitoring-deploy.md).
  * De oplossing voor externe controle implementeert de iothub en Azure Stream Analytics-taak die in dit artikel wordt gebruikt in uw Azure-abonnement.
* [Een Azure Data Lake Store implementeren](../data-lake-store/data-lake-store-get-started-portal.md)
  * Uw Data Lake Store moeten worden geïmplementeerd op dezelfde regio bevinden als uw oplossing voor externe controle.
  * [Maak een map](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) met de naam 'streaming' in uw account.

## <a name="create-a-consumer-group"></a>Een consumergroep maken

Een speciale klantengroep maken in de IoT-hub van uw oplossing voor externe controle. Dit wordt gebruikt door de Stream Analytics-taak voor streaming gegevens naar uw Data Lake Store.

> [!NOTE]
> Consumer-groepen worden gebruikt door toepassingen voor het ophalen van gegevens uit Azure IoT Hub. U moet een nieuwe consumergroep maken voor elke vijf uitvoer consumenten. U kunt maximaal 32 consumergroepen maken.

1. Meld u aan bij Azure Portal.

1. Klik in de Azure-portal op de **Cloud Shell** knop.

    ![Pictogram Portal starten](./media/iot-accelerators-integrate-data-lake/portal-launch-icon.png)

1. Deze opdracht voor het maken van een nieuwe consumergroep wordt uitgevoerd:

```azurecli-interactive
az iot hub consumer-group create --hub-name contoso-rm30263 --name streamanalyticsjob --resource-group contoso-rm
```

> [!NOTE]
> Gebruik de resourcegroep en de namen van de IoT hub van uw oplossing voor externe controle.

## <a name="create-stream-analytics-job"></a>Stream Analytics-taak maken

Maak een Azure Stream Analytics-taak voor het streamen van de gegevens van uw IoT-hub naar uw Azure Data Lake store.

1. Klik op **maken van een resource**Internet der dingen selecteert in de Marketplace en klikt u op **Stream Analytics-taak**.

    ![Nieuwe Stream Analytics-taak](./media/iot-accelerators-integrate-data-lake/new-stream-analytics-job.png)

1. Geef een taaknaam op en selecteer de gewenste abonnement en de Resource-groep.

1. Selecteer een locatie in de nabije- of in dezelfde regio bevinden als uw Data Lake Store. We gebruiken hier VS-Oost.

1. Zorg ervoor dat als u wilt laten de hostomgeving als standaardwaarde **Cloud**.

1. Klik op **Create**.

    ![Stream Analytics-taak maken](./media/iot-accelerators-integrate-data-lake/create-stream-analytics-job.png)

## <a name="configure-the-stream-analytics-job"></a>De Stream Analytics-taak configureren

1. Ga naar de **Stream Analytics-taak** in uw resourcegroep oplossing voor externe controle.

1. Klik op de pagina overzicht **invoer**.

    ![Overzichtspagina](./media/iot-accelerators-integrate-data-lake/stream-analytics-overview.png)

1. Klik op **Stroominvoer toevoegen** en selecteer **IoT Hub** in de vervolgkeuzelijst.

    ![Invoer toevoegen](./media/iot-accelerators-integrate-data-lake/stream-analytics-add-input.png)

1. Voer op het tabblad voor de nieuwe invoer een alias voor invoer van **IoTHub**.

1. Selecteer de consumergroep die u eerder hebt gemaakt in de consument groep vervolgkeuzelijst. We gebruiken hier **streamanalyticsjob**.

    ![Selecteer invoer](./media/iot-accelerators-integrate-data-lake/stream-analytics-new-input.png)

1. Klik op **Opslaan**.

1. Klik op de pagina overzicht **uitvoer**.

    ![Data Lake Store toevoegen](./media/iot-accelerators-integrate-data-lake/stream-analytics-overview-2.png)

1. Klik op **toevoegen** en selecteer **Data Lake Store** in de vervolgkeuzelijst.

    ![Uitvoer toevoegen](./media/iot-accelerators-integrate-data-lake/stream-analytics-output.png)

1. Voer op het nieuwe tabblad uitvoer een uitvoeralias van **DataLakeStore**.

1. Selecteer het Data Lake Store-account dat u in de vorige stappen hebt gemaakt en geef mapstructuur stroom gegevens naar de store.

1. Voer in het veld van de indeling datum **/streaming/ {date} / {time}**. Laat de standaarddatumnotatie van jjjj/MM/DD- en tijdnotatie uu.

    ![Mapstructuur bieden](./media/iot-accelerators-integrate-data-lake/stream-analytics-new-output.png)

1. Klik op **autoriseren**.

    U moet geautoriseerd met Data Lake Store de Stream analytics-taak schrijftoegang geven tot het bestandssysteem.

    ![Stream Analytics naar Data Lake Store toestaan](./media/iot-accelerators-integrate-data-lake/stream-analytics-out-authorize.png)

    U ziet een pop-up en zodra het pop-upvenster wordt gesloten autoriseren knop grijs weergegeven nadat de autorisatie is voltooid.

    > [!NOTE]
    > Als er een fout in het pop-upvenster, open een nieuw browservenster in de modus Incognito en probeer het opnieuw.

1. Klik op **Opslaan**.

## <a name="edit-the-stream-analytics-query"></a>De Stream Analytics query bewerken

Azure Stream Analytics maakt gebruik van een SQL-achtige query-taal voor het opgeven van een invoerbron die streams gegevens en transformatie van die gegevens als de gewenste- en uitvoergegevens voor tal van opslag- of -doelen.

1. Klik op het tabblad Overzicht **query bewerken**.

    ![Query bewerken](./media/iot-accelerators-integrate-data-lake/stream-analytics-edit-query.png)

1. Vervang in de Query-editor de [YourOutputAlias] en [YourInputAlias] tijdelijke aanduidingen door de waarden die u eerder hebt gedefinieerd.

    ```sql
    SELECT
        *, System.Timestamp as time
    INTO
        DataLakeStore
    FROM
        IoTHub
    ```

    ![Stream Analytics Query](./media/iot-accelerators-integrate-data-lake/stream-analytics-query.png)

1. Klik op **Opslaan**.
1. Klik op **Ja** om de wijzigingen te accepteren.

## <a name="start-the-stream-analytics-job"></a>De Stream Analytics-taak starten

1. Klik op het tabblad Overzicht **Start**.

    ![Stream Analytics-taak starten](./media/iot-accelerators-integrate-data-lake/stream-analytics-start.png)

1. Klik op het tabblad Start taak **aangepaste**.

1. Stel aangepaste tijd om terug te gaan een paar uur om op te halen gegevens uit wanneer het apparaat streaming is gestart.

1. Klik op **Start**.

    ![Aangepaste datum kiezen](./media/iot-accelerators-integrate-data-lake/stream-analytics-start-custom.png)

    Wacht totdat de taak wordt uitgevoerd, als er fouten van uw query zijn kan Controleer of de syntaxis juist is.

    ![Actieve taak](./media/iot-accelerators-integrate-data-lake/stream-analytics-running.png)

    De streaming-taak wordt gestart om te lezen van gegevens uit uw IoT-Hub en opslaan van de gegevens in uw Data Lake Store. Duurt enkele minuten duren voordat de gegevens moeten worden weergegeven in uw Data Lake Store beginnen.

## <a name="explore-the-streaming-data"></a>Verken de gegevens van streaming

1. Ga naar uw Data Lake Store.

1. Klik op het tabblad Overzicht **Gegevensverkenner**.

1. In de Data explorer Inzoomen op de **/ -streaming** map. Hier ziet u mappen die zijn gemaakt met de indeling JJJJ/MM/DD/uu.

    ![Streaming gegevens verkennen](./media/iot-accelerators-integrate-data-lake/data-lake-store-data-explorer.png)

    Hier ziet u json-bestanden met één bestand per uur.

    ![Streaming gegevens verkennen](./media/iot-accelerators-integrate-data-lake/data-lake-store-file-preview.png)

## <a name="next-steps"></a>Volgende stappen

Azure Data Lake Analytics kan worden gebruikt voor het uitvoeren van big data-analyse op uw Data Lake Store-gegevenssets. Meer informatie over de [Data Lake Analytics-documentatie](https://docs.microsoft.com/en-us/azure/data-lake-analytics).
