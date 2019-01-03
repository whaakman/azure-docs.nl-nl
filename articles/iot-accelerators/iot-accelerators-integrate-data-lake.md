---
title: De oplossing voor externe controle worden geïntegreerd met Data Lake Store - Azure | Microsoft Docs
description: Meer informatie over het integreren van de oplossing voor externe controle met Azure Data Lake Store met behulp van een Azure Stream Analytics-taak.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 04/29/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 021f18f588613817110539d408f9260fb9247895
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53603926"
---
# <a name="integrate-the-remote-monitoring-solution-with-azure-data-lake-store"></a>De oplossing voor externe controle integreren met Azure Data Lake Store

U hebt mogelijk analytics vereisten dan wat wordt aangeboden in de oplossing voor externe controle geavanceerde. Azure Data Lake Store is ideaal voor deze toepassing omdat deze kunt opslaan van gegevens uit de enorme en diverse gegevenssets, evenals integreren met Azure Data Lake Analytics voor analyse op aanvraag.

In deze instructies gebruikt u een Azure Stream Analytics-taak om gegevens te streamen vanaf de IoT-hub in uw oplossing voor externe controle naar een Azure Data Lake Store.

## <a name="prerequisites"></a>Vereisten

Als u wilt deze procedures hebt voltooid, moet u het volgende:

* [De oplossingsverbetering voor externe bewaking implementeren](quickstart-remote-monitoring-deploy.md).
  * De oplossing voor externe controle implementeert het IoT-hub en Azure Stream Analytics-taak die in dit artikel wordt gebruikt in uw Azure-abonnement.
* [Implementeren van een Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md)
  * Uw Data Lake Store moet worden geïmplementeerd in dezelfde regio als uw oplossing voor externe controle.
  * [Maak een map](../data-lake-store/data-lake-store-get-started-portal.md#createfolder) met de naam 'streaming' in uw account.

## <a name="create-a-consumer-group"></a>Een consumentengroep maken

Maak een speciale klantengroep in de IoT-hub van uw oplossing voor externe controle. Dit wordt gebruikt door de Stream Analytics-taak voor streaming-gegevens naar uw Data Lake Store.

> [!NOTE]
> Consumer-groepen worden gebruikt door toepassingen voor het ophalen van gegevens van Azure IoT Hub. U moet een nieuwe consumergroep maken voor elke consument vijf uitvoer. U kunt maximaal 32 consumergroepen maken.

1. Meld u aan bij Azure Portal.

1. Klik in de Azure-portal op de **Cloud Shell** knop.

    ![Pictogram van de portal starten](./media/iot-accelerators-integrate-data-lake/portal-launch-icon.png)

1. Deze opdracht om te maken van een nieuwe consumergroep wordt uitgevoerd:

```azurecli-interactive
az iot hub consumer-group create --hub-name contoso-rm30263 --name streamanalyticsjob --resource-group contoso-rm
```

> [!NOTE]
> Gebruik de resourcegroep en IoT hub-namen van uw oplossing voor externe controle.

## <a name="create-stream-analytics-job"></a>Stream Analytics-taak maken

Maak een Azure Stream Analytics-taak voor het streamen van de gegevens van uw IoT-hub met uw Azure Data Lake store.

1. Klik op **een resource maken**, Internet of Things selecteren in de Marketplace en klikt u op **Stream Analytics-taak**.

    ![Nieuwe Stream Analytics-taak](./media/iot-accelerators-integrate-data-lake/new-stream-analytics-job.png)

1. Voer een taaknaam en selecteer het juiste abonnement en Resource.

1. Selecteer een locatie in de nabije- of in dezelfde regio als uw Data Lake Store. We gebruiken hier VS-Oost.

1. Zorg ervoor dat u de Hosting-omgeving als de standaard laat **Cloud**.

1. Klik op **Create**.

    ![Stream Analytics-taak maken](./media/iot-accelerators-integrate-data-lake/create-stream-analytics-job.png)

## <a name="configure-the-stream-analytics-job"></a>De Stream Analytics-taak configureren

1. Ga naar de **Stream Analytics-taak** in uw resourcegroep oplossing voor externe controle.

1. Klik op de pagina overzicht **invoer**.

    ![Overzichtspagina](./media/iot-accelerators-integrate-data-lake/stream-analytics-overview.png)

1. Klik op **Stroominvoer toevoegen** en selecteer **IoT-Hub** in de vervolgkeuzelijst.

    ![Invoer toevoegen](./media/iot-accelerators-integrate-data-lake/stream-analytics-add-input.png)

1. Voer op het tabblad voor de nieuwe invoer een alias van de invoer van **IoTHub**.

1. In de consument groep vervolgkeuzelijst, selecteer de consumergroep die u eerder hebt gemaakt. We gebruiken hier **streamanalyticsjob**.

    ![Invoerselectie](./media/iot-accelerators-integrate-data-lake/stream-analytics-new-input.png)

1. Klik op **Opslaan**.

1. Klik op de pagina overzicht **uitvoer**.

    ![Data Lake Store toevoegen](./media/iot-accelerators-integrate-data-lake/stream-analytics-overview-2.png)

1. Klik op **toevoegen** en selecteer **Data Lake Store** in de vervolgkeuzelijst.

    ![Uitvoer toevoegen](./media/iot-accelerators-integrate-data-lake/stream-analytics-output.png)

1. Voer op het nieuwe tabblad van de uitvoer een uitvoeralias van **DataLakeStore**.

1. Selecteer het Data Lake Store-account dat u in de vorige stappen hebt gemaakt en geef mapstructuur om gegevens te streamen naar de store.

1. Voer in het veld van de indeling datum **/streaming/ {date} / {time}**. Laat de standaarddatumnotatie jjjj/MM/DD- en tijdnotatie, uu.

    ![Mapstructuur bieden](./media/iot-accelerators-integrate-data-lake/stream-analytics-new-output.png)

1. Klik op **autoriseren**.

    U moet autoriseren met Data Lake Store de Stream analytics-taak schrijven toegang geven tot het bestandssysteem.

    ![Stream Analytics kunt u Data Lake Store toestaan](./media/iot-accelerators-integrate-data-lake/stream-analytics-out-authorize.png)

    Ziet u een pop-upvenster en zodra het pop-upvenster wordt gesloten autoriseren knop wordt grijs weergegeven nadat de autorisatie is voltooid.

    > [!NOTE]
    > Als u een fout in het pop-upvenster ziet, opent u een nieuw browservenster in de Incognito-modus en probeer het opnieuw.

1. Klik op **Opslaan**.

## <a name="edit-the-stream-analytics-query"></a>De Stream Analytics-query bewerken

Azure Stream Analytics maakt gebruik van een SQL-achtige querytaal voor het opgeven van een invoerbron die de gegevens streamt en transformeren van gegevens op als de gewenste- en uitvoergegevens voor een verscheidenheid aan opslag of verwerking door bestemmingen.

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

    ![Stream Analytics-Query](./media/iot-accelerators-integrate-data-lake/stream-analytics-query.png)

1. Klik op **Opslaan**.
1. Klik op **Ja** om de wijzigingen te accepteren.

## <a name="start-the-stream-analytics-job"></a>De Stream Analytics-taak starten

1. Klik op het tabblad Overzicht **Start**.

    ![Stream Analytics-taak starten](./media/iot-accelerators-integrate-data-lake/stream-analytics-start.png)

1. Klik op het tabblad van de taak Start **aangepaste**.

1. Aangepaste tijd om een paar uur terug om op te halen gegevens uit wanneer het apparaat is gestart streaming instellen.

1. Klik op **Starten**.

    ![Aangepaste datum kiezen](./media/iot-accelerators-integrate-data-lake/stream-analytics-start-custom.png)

    Wacht totdat de taak wordt uitgevoerd, als er fouten mogelijk uit uw query controleren om te controleren of de syntaxis juist is.

    ![Taak die wordt uitgevoerd](./media/iot-accelerators-integrate-data-lake/stream-analytics-running.png)

    De streaming-taak wordt begint met het lezen van gegevens van uw IoT-Hub en de gegevens opslaan in uw Data Lake Store. Het duurt een paar minuten voor de gegevens om te worden weergegeven in uw Data Lake Store.

## <a name="explore-the-streaming-data"></a>De streaming-gegevens verkennen

1. Ga naar uw Data Lake Store.

1. Klik op het tabblad Overzicht **Data explorer**.

1. In Data explorer, zoomt u in op de **/ streaming** map. Hier ziet u mappen die zijn gemaakt met de indeling JJJJ/MM/DD/HH.

    ![Streaming-gegevens verkennen](./media/iot-accelerators-integrate-data-lake/data-lake-store-data-explorer.png)

    U ziet json-bestanden met één bestand per uur.

    ![Streaming-gegevens verkennen](./media/iot-accelerators-integrate-data-lake/data-lake-store-file-preview.png)

## <a name="next-steps"></a>Volgende stappen

Azure Data Lake Analytics kan worden gebruikt om big data-analyse uitvoeren op uw Data Lake Store-gegevenssets. Meer informatie over de [documentatie over Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics).
