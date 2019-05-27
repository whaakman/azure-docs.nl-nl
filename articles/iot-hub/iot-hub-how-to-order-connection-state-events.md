---
title: Order apparaat connection-gebeurtenissen van Azure IoT Hub met behulp van Azure Cosmos DB | Microsoft Docs
description: In dit artikel wordt beschreven hoe u bestellen en het apparaat verbindingsgebeurtenissen van Azure IoT Hub met behulp van Azure Cosmos DB te houden van de status van de meest recente verbinding opnemen
services: iot-hub
ms.service: iot-hub
author: ash2017
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: asrastog
ms.openlocfilehash: ff8f8c6656c4cd095749b3e048c72572d113f1ad
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "66015260"
---
# <a name="order-device-connection-events-from-azure-iot-hub-using-azure-cosmos-db"></a>Order apparaat connection-gebeurtenissen van Azure IoT Hub met behulp van Azure Cosmos DB

Azure Event Grid helpt u bij het bouwen van toepassingen op basis van gebeurtenissen en IoT-gebeurtenissen in uw zakelijke oplossingen eenvoudig integreren. Dit artikel begeleidt u bij een installatie die kan worden gebruikt bij te houden en de status van de meest recente apparaat verbinding opslaan in Cosmos DB. We gebruiken het volgnummer beschikbaar in de gebeurtenissen apparaat verbonden en het apparaat niet verbonden en de meest recente status opslaan in Cosmos DB. We gaan een opgeslagen procedure, die de toepassingslogica van een die wordt uitgevoerd op basis van een verzameling in Cosmos DB gebruiken.

Het volgnummer wordt een tekenreeksweergave van een hexadecimaal nummer. Tekenreeks vergelijken kunt u het grotere aantal identificeren. Als u converteert de tekenreeks die moet worden hexadecimale, wordt het getal een 256-bits getal zijn. Het volgnummer strikt toeneemt en de meest recente gebeurtenis heeft een hoger getal dan andere gebeurtenissen. Dit is handig als u regelmatig apparaat verbinding maakt en de verbinding verbreekt en om te controleren of dat alleen de meest recente gebeurtenis wordt gebruikt voor het activeren van een downstream-actie, zoals Azure Event Grid biedt geen ondersteuning voor ordening van gebeurtenissen.

## <a name="prerequisites"></a>Vereisten

* Een actief Azure-account. Als u nog geen account hebt, kunt u [een gratis account aanmaken](https://azure.microsoft.com/pricing/free-trial/).

* Een actief Azure Cosmos DB SQL API-account. Als u een nog niet hebt gemaakt, raadpleegt u [een databaseaccount maken](../cosmos-db/create-sql-api-dotnet.md#create-an-azure-cosmos-db-account) voor een overzicht.

* Een verzameling in uw database. Zie [toevoegen van een verzameling](../cosmos-db/create-sql-api-dotnet.md#add-a-database-and-a-collection) voor een overzicht. Wanneer u uw verzameling maakt, gebruikt u `/id` voor de partitiesleutel.

* Een IoT Hub in Azure. Als u nog geen hub hebt gemaakt, leest u [Get started with IoT Hub](iot-hub-csharp-csharp-getstarted.md) (Aan de slag met IoT Hub) voor stapsgewijze instructies.

## <a name="create-a-stored-procedure"></a>Een opgeslagen procedure maken

Eerst maakt u een opgeslagen procedure en stel deze tot een logica die wordt vergeleken volgnummers van binnenkomende gebeurtenissen en de meest recente gebeurtenis per apparaat wordt geregistreerd in de database worden uitgevoerd.

1. Selecteer in uw Cosmos DB SQL API biedt **Data Explorer** > **Items** > **nieuwe opgeslagen Procedure**.

   ![Opgeslagen procedure maken](./media/iot-hub-how-to-order-connection-state-events/create-stored-procedure.png)

2. Voer **LatestDeviceConnectionState** voor de opgeslagen procedure-ID en plak het volgende in de **hoofdtekst van de opgeslagen Procedure**. Houd er rekening mee dat deze code geen bestaande code in de hoofdtekst van de opgeslagen procedure moet vervangen. Deze code onderhoudt een rij per apparaat-ID en de meest recente verbindingsstatus van deze apparaat-ID met het vaststellen van het hoogste volgnummer registreert.

    ```javascript
    // SAMPLE STORED PROCEDURE
    function UpdateDevice(deviceId, moduleId, hubName, connectionState, connectionStateUpdatedTime, sequenceNumber) {
      var collection = getContext().getCollection();
      var response = {};

      var docLink = getDocumentLink(deviceId, moduleId);

      var isAccepted = collection.readDocument(docLink, function(err, doc) {
        if (err) {
          console.log('Cannot find device ' + docLink + ' - ');
          createDocument();
        } else {
          console.log('Document Found - ');
          replaceDocument(doc);
        }
      });

      function replaceDocument(document) {
        console.log(
          'Old Seq :' +
            document.sequenceNumber +
            ' New Seq: ' +
            sequenceNumber +
            ' - '
        );
        if (sequenceNumber > document.sequenceNumber) {
          document.connectionState = connectionState;
          document.connectionStateUpdatedTime = connectionStateUpdatedTime;
          document.sequenceNumber = sequenceNumber;

          console.log('replace doc - ');

          isAccepted = collection.replaceDocument(docLink, document, function(
            err,
            updated
          ) {
            if (err) {
              getContext()
                .getResponse()
                .setBody(err);
            } else {
              getContext()
                .getResponse()
                .setBody(updated);
            }
          });
        } else {
          getContext()
            .getResponse()
            .setBody('Old Event - current: ' + document.sequenceNumber + ' Incoming: ' + sequenceNumber);
        }
      }
      function createDocument() {
        document = {
          id: deviceId + '-' + moduleId,
          deviceId: deviceId,
          moduleId: moduleId,
          hubName: hubName,
          connectionState: connectionState,
          connectionStateUpdatedTime: connectionStateUpdatedTime,
          sequenceNumber: sequenceNumber
        };
        console.log('Add new device - ' + collection.getAltLink());
        isAccepted = collection.createDocument(
          collection.getAltLink(),
          document,
          function(err, doc) {
            if (err) {
              getContext()
                .getResponse()
                .setBody(err);
            } else {
              getContext()
                .getResponse()
                .setBody(doc);
            }
          }
        );
      }

      function getDocumentLink(deviceId, moduleId) {
        return collection.getAltLink() + '/docs/' + deviceId + '-' + moduleId;
      }
    }
    ```

3. Sla de opgeslagen procedure:

    ![opslaan van de opgeslagen procedure](./media/iot-hub-how-to-order-connection-state-events/save-stored-procedure.png)

## <a name="create-a-logic-app"></a>Een logische app maken

U gaat eerst een logische app maken en een trigger voor Event Grid toevoegen die de resourcegroep voor uw virtuele machine bewaakt.

### <a name="create-a-logic-app-resource"></a>Een logische app maken

1. In de [Azure-portal](https://portal.azure.com), selecteer **+ een resource maken**, selecteer **integratie** en vervolgens **logische App**.

   ![Logische app maken](./media/iot-hub-how-to-order-connection-state-events/select-logic-app.png)

2. Geef een naam op voor de logische app die uniek is in uw abonnement en selecteer vervolgens het abonnement, de resourcegroep en de locatie van uw IoT-hub.

   ![Nieuwe logische app](./media/iot-hub-how-to-order-connection-state-events/new-logic-app.png)

3. Selecteer **maken** te maken van de logische app.

   U hebt nu een Azure-resource voor uw logische app gemaakt. Nadat de logische app is geïmplementeerd, toont de ontwerper van logische apps sjablonen voor algemene patronen, zodat u sneller aan de slag kunt.

   > [!NOTE]
   > Als u wilt zoeken en opnieuw openen van uw logische app, selecteer **resourcegroepen** en selecteer de resourcegroep die u voor deze instructies gebruikt. Selecteer vervolgens de nieuwe logische app. Hiermee opent u de ontwerper van logische App.

4. In de Logic App Designer, schuif naar rechts tot u veelgebruikte triggers. Onder **sjablonen**, kiest u **lege logische App** zodat u uw logische app helemaal kunt bouwen.

### <a name="select-a-trigger"></a>Een trigger selecteren

Een trigger is een specifieke gebeurtenis waarmee uw logische app wordt gestart. Voor deze zelfstudie is de trigger voor het activeren van de werkstroom het ontvangen van een aanvraag via HTTP.

1. Typ in de connectors en triggers zoekbalk **HTTP** en druk op Enter.

2. Selecteer **Aanvraag - Wanneer een HTTP-aanvraag is ontvangen** als de trigger.

   ![Selecteer de trigger Aanvraag - Wanneer een HTTP-aanvraag is ontvangen](./media/iot-hub-how-to-order-connection-state-events/http-request-trigger.png)

3. Selecteer **Voorbeeldnettolading om een schema te genereren**.

   ![Voorbeeld van payload gebruiken voor het genereren van een schema](./media/iot-hub-how-to-order-connection-state-events/sample-payload.png)

4. Plak de volgende voorbeeldcode van JSON in het tekstvak en selecteer vervolgens **Gereed**:

   ```json
   [{
    "id": "fbfd8ee1-cf78-74c6-dbcf-e1c58638ccbd",
    "topic":
      "/SUBSCRIPTIONS/DEMO5CDD-8DAB-4CF4-9B2F-C22E8A755472/RESOURCEGROUPS/EGTESTRG/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/MYIOTHUB",
    "subject": "devices/Demo-Device-1",
    "eventType": "Microsoft.Devices.DeviceConnected",
    "eventTime": "2018-07-03T23:20:11.6921933+00:00",
    "data": {
      "deviceConnectionStateEventInfo": {
        "sequenceNumber":
          "000000000000000001D4132452F67CE200000002000000000000000000000001"
      },
      "hubName": "MYIOTHUB",
      "deviceId": "48e44e11-1437-4907-83b1-4a8d7e89859e",
      "moduleId": ""
    },
    "dataVersion": "1",
    "metadataVersion": "1"
   }]
   ```

   ![Voorbeeld van JSON-nettolading plakken](./media/iot-hub-how-to-order-connection-state-events/paste-sample-payload.png)

5. Er kan een melding worden weergegeven dat u niet moet vergeten **in uw aanvraag een header Content-type op te nemen die is ingesteld op application/json**. U kunt deze suggestie zonder problemen negeren en verdergaan met de volgende sectie.

### <a name="create-a-condition"></a>Een voorwaarde maken

In uw werkstroom voor logische Apps, helpen voorwaarden bij het uitvoeren van specifieke acties na het slagen van die specifieke voorwaarde. Als de voorwaarde is voldaan, kan een gewenste actie kan worden gedefinieerd. Voor deze zelfstudie is de voorwaarde om te controleren of type gebeurtenis is verbonden apparaat of apparaat is verbroken. De actie worden de opgeslagen procedure uitvoeren in uw database.

1. Selecteer **+ nieuwe stap** vervolgens **ingebouwde**, zoek en selecteer **voorwaarde**. Klik in **een waarde kiezen** en een vak weergegeven waarin de dynamische inhoud--de velden die kunnen worden geselecteerd. Vul de velden, zoals hieronder wordt weergegeven om uit te voeren dit alleen voor apparaten die zijn verbonden en apparaat verbroken gebeurtenissen:

   * Een waarde kiezen: **type gebeurtenis** --Selecteer deze optie in de velden in de dynamische inhoud die worden weergegeven wanneer u dit veld op.
   * Wijziging 'is gelijk aan' naar **eindigt**.
   * Een waarde kiezen: **nected**.

     ![Opvulling van voorwaarde](./media/iot-hub-how-to-order-connection-state-events/condition-detail.png)

2. In de **als de waarde true** dialoogvenster, klikt u op **een actie toevoegen**.
  
   ![Actie toevoegen als de waarde true](./media/iot-hub-how-to-order-connection-state-events/action-if-true.png)

3. Zoek naar Cosmos DB en selecteer **Azure Cosmos DB - opgeslagen procedure uitvoeren**

   ![Zoeken naar CosmosDB](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-search.png)

4. Vul in **cosmosdb-connection** voor de **verbindingsnaam** en selecteer de vermelding in de tabel en selecteer vervolgens **maken**. U ziet de **opgeslagen procedure uitvoeren** deelvenster. Voer de waarden voor de velden:

   **Database-ID**: Takenlijst

   **Verzamelings-ID**: Items

   **ID van opgeslagen procedure**: LatestDeviceConnectionState

5. Selecteer **toevoegen van nieuwe parameter**. In de vervolgkeuzelijst die wordt weergegeven, schakel de selectievakjes naast **partitiesleutel** en **Parameters voor de opgeslagen procedure**, klik vervolgens ergens anders zijn op het scherm op; deze wordt toegevoegd een veld voor de waarde voor de partitiesleutel en een veld voor parameters voor de opgeslagen procedure.

   ![actie voor logische app vullen](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure.png)

6. Voer nu de waarde voor de partitiesleutel en de parameters zoals hieronder wordt weergegeven. Zorg ervoor dat in de vierkante haken en dubbele aanhalingstekens zoals plaatsen. Mogelijk moet u klikt u op **dynamische inhoud toevoegen** om op te halen van de geldige waarden die u hier kunt gebruiken.

   ![actie voor logische app vullen](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure-2.png)

7. Aan de bovenkant van het deelvenster waarin de status **voor elk**onder **een uitvoer selecteren uit de vorige stappen**, zorg ervoor dat het **hoofdtekst** is geselecteerd.

   ![logische app voor elke vullen](./media/iot-hub-how-to-order-connection-state-events/logicapp-foreach-body.png)

8. Sla uw logische app op.

### <a name="copy-the-http-url"></a>HTTP-URL kopiëren

Voordat u de Logic Apps Designer verlaat, Kopieer de URL die uw logische app om te naar een trigger luisteren is. U gebruikt deze URL voor het configureren van Event Grid.

1. Vouw het configuratievak **Wanneer een HTTP-aanvraag is ontvangen** uit door erop te klikken.

2. Kopieer de waarde van **URL voor HTTP POST** door de knop URL kopiëren ernaast te selecteren.

   ![De URL voor HTTP POST kopiëren](./media/iot-hub-how-to-order-connection-state-events/copy-url.png)

3. Sla deze URL op zodat u ernaar kunt verwijzen in de volgende sectie.

## <a name="configure-subscription-for-iot-hub-events"></a>Abonnement voor IoT Hub-gebeurtenissen configureren

In deze sectie configureert u de IoT-hub voor het publiceren van gebeurtenissen op het moment dat deze optreden.

1. Ga in Azure Portal naar uw IoT-hub.

2. Selecteer **Gebeurtenissen**.

   ![Details van gebeurtenisraster weergeven](./media/iot-hub-how-to-order-connection-state-events/event-grid.png)

3. Selecteer **+ gebeurtenisabonnement**.

   ![Nieuw gebeurtenisabonnement maken](./media/iot-hub-how-to-order-connection-state-events/event-subscription.png)

4. Vul in **abonnement Gebeurtenisdetails**: Geef een beschrijvende naam op en selecteer **gebeurtenisschema in het raster**.

5. Vul de **gebeurtenistypen** velden. Schakel het selectievakje **abonneren op alle gebeurtenistypen** en selecteer **apparaat aangesloten** en **apparaat losgekoppeld** in het menu.

   ![Gebeurtenistypen instellen om te zoeken](./media/iot-hub-how-to-order-connection-state-events/set-event-types.png)

6. Voor **eindpuntdetails**, selecteert u Eindpunttype als **Webhook** en klik op selecteren eindpunt en plak de URL die u hebt gekopieerd uit uw logische app en Bevestig de selectie.

   ![eindpunt-url selecteren](./media/iot-hub-how-to-order-connection-state-events/endpoint-url.png)

7. Het formulier moet er nu uitzien zoals in het volgende voorbeeld:

   ![Voorbeeld van formulier voor gebeurtenisabonnement](./media/iot-hub-how-to-order-connection-state-events/subscription-form.png)

   Selecteer **Maken** om het gebeurtenisabonnement op te slaan.

## <a name="observe-events"></a>Gebeurtenissen bekijken

Nu het gebeurtenisabonnement is ingesteld, gaan we testen door verbinding te maken van een apparaat.

### <a name="register-a-device-in-iot-hub"></a>Registreer een apparaat in IoT Hub

1. Selecteer **IoT-apparaten** in de IoT-hub.

2. Selecteer **+ toevoegen** aan de bovenkant van het deelvenster.

3. Geef `Demo-Device-1` op voor **Apparaat-ID**.

4. Selecteer **Opslaan**.

5. U kunt meerdere apparaten met verschillende apparaat-id's toevoegen.

   ![Apparaten die zijn toegevoegd aan de hub](./media/iot-hub-how-to-order-connection-state-events/AddIoTDevice.png)

6. Klik op het apparaat opnieuw; nu worden de verbindingsreeksen en sleutels ingevuld. Kopieer de **verbindingsreeks - primaire sleutel** voor later gebruik.

   ![ConnectionString voor apparaat](./media/iot-hub-how-to-order-connection-state-events/DeviceConnString.png)

HostName=test-eventgrid-hub.azure-devices.net;DeviceId=Demo-Device-1;SharedAccessKey=cv8uPNixe7E2R9EHtimoY/PlJfBV/lOYCMajVOp/Cuw=

### <a name="start-raspberry-pi-simulator"></a>Starten van de simulator Raspberry Pi

1. We gebruiken de Raspberry Pi-websimulator apparaatverbinding simuleren.

[Starten van de simulator Raspberry Pi](https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted)

### <a name="run-a-sample-application-on-the-raspberry-pi-web-simulator"></a>Een voorbeeld-App uitvoeren op de Raspberry Pi-websimulator

Hiermee wordt een verbonden apparaat-gebeurtenis geactiveerd.

1. In de code, kunt u de tijdelijke aanduiding in regel 15 vervangen door de verbindingsreeks van uw Azure IoT Hub-apparaat dat u aan het einde van de vorige sectie hebt opgeslagen.

   ![Plak in de verbindingsreeks van apparaat](./media/iot-hub-how-to-order-connection-state-events/raspconnstring.png)

2. De toepassing wordt uitgevoerd door het selecteren van **uitvoeren**.

U ziet er ongeveer uitzien als in de volgende uitvoer ziet u de sensorgegevens en de berichten die worden verzonden naar uw IoT-hub.

   ![De toepassing uitvoeren](./media/iot-hub-how-to-order-connection-state-events/raspmsg.png)

   Klik op **stoppen** stoppen van de simulator en het activeren een **apparaat losgekoppeld** gebeurtenis.

U hebt nu een voorbeeld van toepassing voor het verzamelen van gegevens en verzenden naar uw IoT-hub uitvoeren.

### <a name="observe-events-in-cosmos-db"></a>Bekijk de gebeurtenissen in Cosmos DB

Resultaten van de uitgevoerde opgeslagen procedure ziet u in uw Cosmos DB-document. Hier ziet u hoe dat eruitziet. Elke rij bevat de status van de meest recente apparaat verbinding per apparaat.

   ![Hoe uitslag](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-outcome.png)

## <a name="use-the-azure-cli"></a>Azure CLI gebruiken

In plaats van de [Azure-portal](https://portal.azure.com), kunt u de stappen van de IoT Hub met behulp van de Azure CLI uitvoeren. Zie voor meer informatie, de Azure CLI-pagina's voor [het maken van een gebeurtenisabonnement](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) en [het maken van een IoT-apparaat](/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity#ext-azure-cli-iot-ext-az-iot-hub-device-identity-create).

## <a name="clean-up-resources"></a>Resources opschonen

In deze zelfstudie zijn resources gebruikt die kosten voor uw Azure-abonnement met zich meebrengen. Wanneer u klaar bent met de zelfstudie en het testen van de resultaten, moet u daarom de resources uitschakelen of verwijderen die u niet wilt behouden.

Als u het werk aan uw logische app wilt behouden, kunt u de app uitschakelen in plaats van verwijderen.

1. Ga naar uw logische app.

2. Op de **overzicht** Selecteer **verwijderen** of **uitschakelen**.

    Elk abonnement biedt toegang tot één gratis IoT-hub. Als u een gratis hub hebt gemaakt voor deze zelfstudie, hoeft u deze niet te verwijderen om te voorkomen dat er kosten in rekening worden gebracht.

3. Ga naar uw IoT-hub.

4. Op de **overzicht** Selecteer **verwijderen**.

    Zelfs als u uw IoT-hub wilt behouden, kunt u het gebeurtenisabonnement verwijderen dat u hebt gemaakt.

5. Selecteer hiervoor **Event Grid** in uw IoT-hub.

6. Selecteer het gebeurtenisabonnement dat u wilt verwijderen.

7. Selecteer **Verwijderen**.

Als u wilt verwijderen van een Azure Cosmos DB-account vanuit Azure portal, met de rechtermuisknop op de accountnaam en klik op **-account verwijderen**. Zie voor gedetailleerde instructies [verwijderen van een Azure Cosmos DB-account](https://docs.microsoft.com/azure/cosmos-db/manage-account).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [reageren op gebeurtenissen van IoT-Hub met behulp van Event Grid om acties starten](../iot-hub/iot-hub-event-grid.md)

* [Raadpleeg de zelfstudie IoT Hub-gebeurtenissen](../event-grid/publish-iot-hub-events-to-logic-apps.md)

* Meer informatie over wat u kunt doen met [Event Grid](../event-grid/overview.md)