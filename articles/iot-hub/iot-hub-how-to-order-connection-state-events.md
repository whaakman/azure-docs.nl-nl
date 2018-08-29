---
title: Order apparaat connection-gebeurtenissen van Azure IoT Hub met behulp van Azure Cosmos DB | Microsoft Docs
description: In dit artikel wordt beschreven hoe u bestellen en het apparaat verbindingsgebeurtenissen van Azure IoT Hub met behulp van Azure Cosmos DB te houden van de status van de meest recente verbinding opnemen
services: iot-hub
documentationcenter: ''
author: ash2017
manager: briz
editor: ''
ms.service: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2018
ms.author: asrastog
ms.openlocfilehash: dd3c750e93646624e46c46afd871ef75af905bf0
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43145870"
---
# <a name="order-device-connection-events-from-azure-iot-hub-using-azure-cosmos-db"></a>Order apparaat connection-gebeurtenissen van Azure IoT Hub met behulp van Azure Cosmos DB

Azure Event Grid helpt u bij het bouwen van toepassingen op basis van gebeurtenissen en IoT-gebeurtenissen in uw zakelijke oplossingen eenvoudig integreren. Dit artikel begeleidt u bij een set van die kunnen worden gebruikt bij te houden en de status van de meest recente apparaat verbinding opslaan in Cosmos DB. We gebruiken het volgnummer beschikbaar in de gebeurtenissen apparaat verbonden en het apparaat niet verbonden en de meest recente status opslaan in Cosmos DB. We gaan een opgeslagen procedure, die de toepassingslogica van een die wordt uitgevoerd op basis van een verzameling in Cosmos DB gebruiken.

Het volgnummer wordt een tekenreeksweergave van een hexadecimaal nummer. Tekenreeks vergelijken kunt u het grotere aantal identificeren. Als u converteert de tekenreeks die moet worden hexadecimale, wordt het getal een 256-bits getal zijn. Het volgnummer strikt toeneemt en de meest recente gebeurtenis heeft een hoger getal dan andere gebeurtenissen. Dit is handig als u regelmatig apparaat verbinding maakt en de verbinding verbreekt en om te controleren of dat alleen de meest recente gebeurtenis wordt gebruikt voor het activeren van een downstream-actie, zoals Azure Event Grid biedt geen ondersteuning voor ordening van gebeurtenissen.

## <a name="prerequisites"></a>Vereisten

* Een actief Azure-account. Als u nog geen account hebt, kunt u [een gratis account aanmaken](http://azure.microsoft.com/pricing/free-trial/).
* Een actief Azure Cosmos DB SQL API-account. Als u een nog niet hebt gemaakt, raadpleegt u [een databaseaccount maken](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-dotnet#create-a-database-account) voor een overzicht.
* Een verzameling in uw database. Zie [toevoegen van een verzameling](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-dotnet#add-a-collection) voor een overzicht.
* Een IoT-Hub in Azure. Als u nog geen hub hebt gemaakt, leest u [Get started with IoT Hub](../iot-hub/iot-hub-csharp-csharp-getstarted.md) (Aan de slag met IoT Hub) voor stapsgewijze instructies. 

## <a name="create-a-stored-procedure"></a>Een opgeslagen procedure maken

Eerst maakt u een opgeslagen procedure en stel deze tot een logica die wordt vergeleken volgnummers van binnenkomende gebeurtenissen en de meest recente gebeurtenis per apparaat wordt geregistreerd in de database worden uitgevoerd.

1. Selecteer in uw Cosmos DB SQL API biedt **Data Explorer** > **Items** > **nieuwe opgeslagen Procedure**.

   ![Opgeslagen procedure maken](./media/iot-hub-how-to-order-connection-state-events/create-stored-procedure.png)

2. Voer een opgeslagen procedure-id in en plak het volgende in de 'hoofdtekst van de opgeslagen Procedure'. Houd er rekening mee dat deze code geen bestaande code in de hoofdtekst van de opgeslagen procedure moet vervangen. Deze code onderhoudt een rij per apparaat-ID en de meest recente verbindingsstatus van deze apparaat-id met het vaststellen van het hoogste volgnummer registreert. 

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

1. In de [Azure-portal](https://portal.azure.com), selecteer **nieuw** > **integratie** > **logische App**.

   ![Logische app maken](./media/iot-hub-how-to-order-connection-state-events/select-logic-app.png)

2. Geef een naam op voor de logische app die uniek is in uw abonnement en selecteer vervolgens het abonnement, de resourcegroep en de locatie van uw IoT-hub. 
3. Als u klaar bent, selecteert u **Vastmaken aan dashboard** en kiest u **Maken**.

   U hebt nu een Azure-resource voor uw logische app gemaakt. Nadat de logische app is geïmplementeerd, toont de ontwerper van logische apps sjablonen voor algemene patronen, zodat u sneller aan de slag kunt.

   > [!NOTE] 
   > Als u **Vastmaken aan Dashboard** selecteert, wordt de logische app automatisch geopend in de functie Ontwerper van logische apps. Anders kunt u de logische app handmatig zoeken en openen.

4. Kies in Ontwerper van logische apps onder **Sjablonen** de optie **Lege logische app**, zodat u de logische app helemaal zelf kunt ontwerpen.

### <a name="select-a-trigger"></a>Een trigger selecteren

Een trigger is een specifieke gebeurtenis waarmee uw logische app wordt gestart. Voor deze zelfstudie is de trigger voor het activeren van de werkstroom het ontvangen van een aanvraag via HTTP.  

1. Typ **HTTP** in de zoekbalk voor connectors en triggers.
2. Selecteer **Aanvraag - Wanneer een HTTP-aanvraag is ontvangen** als de trigger. 

   ![Selecteer de trigger Aanvraag - Wanneer een HTTP-aanvraag is ontvangen](./media/iot-hub-how-to-order-connection-state-events/http-request-trigger.png)

3. Selecteer **Voorbeeldnettolading om een schema te genereren**. 

   ![Selecteer de trigger Aanvraag - Wanneer een HTTP-aanvraag is ontvangen](./media/iot-hub-how-to-order-connection-state-events/sample-payload.png)

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

5. Er kan een melding worden weergegeven dat u niet moet vergeten **in uw aanvraag een header Content-type op te nemen die is ingesteld op application/json**. U kunt deze suggestie zonder problemen negeren en verdergaan met de volgende sectie. 

### <a name="create-a-condition"></a>Een voorwaarde maken

Voorwaarden help specifieke acties uitvoeren na het slagen van een bepaalde voorwaarde, u kunt de werkstroom voor logische Apps. Als de voorwaarde is voldaan, kan een gewenste actie kan worden gedefinieerd. Voor deze zelfstudie is de voorwaarde om te controleren of type gebeurtenis is verbonden apparaat of apparaat is verbroken. De actie worden de opgeslagen procedure uitvoeren in uw database. 

1. Selecteer **nieuwe stap** vervolgens **dient te worden** en **voorwaarde**. 

2. Vul de voorwaarde, zoals hieronder wordt weergegeven om uit te voeren dit alleen voor apparaten die zijn verbonden en apparaat verbroken gebeurtenissen:
  * Een waarde kiezen: **type gebeurtenis**
  * Wijzigen is gelijk aan ' naar **eindigt met**
  * Een waarde kiezen: **nected**

   ![Opvulling van voorwaarde](./media/iot-hub-how-to-order-connection-state-events/condition-detail.png)

3. Als de voorwaarde waar is, klikt u op **een actie toevoegen**.
  
   ![Actie toevoegen als de waarde true](./media/iot-hub-how-to-order-connection-state-events/action-if-true.png)

4. Zoeken naar Cosmos DB en klik op **Azure Cosmos DB - opgeslagen procedure uitvoeren**

   ![Zoeken naar CosmosDB](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-search.png)

5. Vul het formulier voor uitvoeren die zijn opgeslagen door waarden te selecteren uit de database verkrijgen. Voer de waarde voor de partitiesleutel en de parameters zoals hieronder wordt weergegeven. 

   ![actie voor logische app vullen](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure.png)

6. Sla uw logische app op. 

### <a name="copy-the-http-url"></a>HTTP-URL kopiëren

Voordat u de functie Ontwerper van logische apps verlaat, kopieert u de URL waarnaar uw logische app luistert voor een trigger. U gebruikt deze URL voor het configureren van Event Grid. 

1. Vouw het configuratievak **Wanneer een HTTP-aanvraag is ontvangen** uit door erop te klikken. 
2. Kopieer de waarde van **URL voor HTTP POST** door de knop URL kopiëren ernaast te selecteren. 

   ![De URL voor HTTP POST kopiëren](./media/iot-hub-how-to-order-connection-state-events/copy-url.png)

3. Sla deze URL op zodat u ernaar kunt verwijzen in de volgende sectie. 

## <a name="configure-subscription-for-iot-hub-events"></a>Abonnement voor IoT Hub-gebeurtenissen configureren

In deze sectie configureert u de IoT-hub voor het publiceren van gebeurtenissen op het moment dat deze optreden. 

1. Ga in Azure Portal naar uw IoT-hub. 
2. Selecteer **Gebeurtenissen**.

   ![Details van gebeurtenisraster weergeven](./media/iot-hub-how-to-order-connection-state-events/event-grid.png)

3. Selecteer **Gebeurtenisabonnement**. 

   ![Nieuw gebeurtenisabonnement maken](./media/iot-hub-how-to-order-connection-state-events/event-subscription.png)

4. Maak het gebeurtenisabonnement met de volgende waarden: 
   * **Gebeurtenistype**: Schakel het selectievakje abonneren op alle gebeurtenistypen en selecteer **apparaat aangesloten** en **apparaat losgekoppeld** in het menu.
   * **Details van eindpunt**: Endpoint-Type selecteren als **Webhook** en klik op selecteren eindpunt en plak de URL die u hebt gekopieerd uit uw logische app en Bevestig de selectie.

   ![eindpunt-url selecteren](./media/iot-hub-how-to-order-connection-state-events/endpoint-url.png)

   * **Details van gebeurtenis abonnement**: Geef een beschrijvende naam op en selecteer **gebeurtenisschema in het raster** wanneer u klaar bent, het formulier moet eruitzien als het volgende voorbeeld: 

   ![Voorbeeld van formulier voor gebeurtenisabonnement](./media/iot-hub-how-to-order-connection-state-events/subscription-form.png)

5. Selecteer **Maken** om het gebeurtenisabonnement op te slaan.

## <a name="observe-events"></a>Gebeurtenissen bekijken
Nu het gebeurtenisabonnement is ingesteld, gaan we testen door verbinding te maken van een apparaat.

### <a name="register-a-device-in-iot-hub"></a>Registreer een apparaat in IoT Hub

1. Selecteer **IoT-apparaten** in de IoT-hub. 
2. Selecteer **Toevoegen**.
3. Geef `Demo-Device-1` op voor **Apparaat-ID**.
4. Selecteer **Opslaan**. 
5. U kunt meerdere apparaten met verschillende apparaat-id's toevoegen.

   ![Hoe uitslag](./media/iot-hub-how-to-order-connection-state-events/AddIoTDevice.png)

6. Kopieer de **verbindingsreeks - primaire sleutel** voor later gebruik.

   ![Hoe uitslag](./media/iot-hub-how-to-order-connection-state-events/DeviceConnString.png)

### <a name="start-raspberry-pi-simulator"></a>Starten van de simulator Raspberry Pi

1. We gebruiken de Raspberry Pi-websimulator apparaatverbinding simuleren.

[Starten van de simulator Raspberry Pi](https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted)

### <a name="run-a-sample-applciation-on-the-raspberry-pi-web-simulator"></a>Een voorbeeld-toepassing uitvoeren op de Raspberry Pi-websimulator
Hiermee wordt een verbonden apparaat-gebeurtenis geactiveerd.

1. In de code, kunt u de tijdelijke aanduiding in regel 15 vervangen door de verbindingsreeks van uw Azure IoT Hub-apparaat.

   ![Hoe uitslag](./media/iot-hub-how-to-order-connection-state-events/raspconnstring.png)

2. De toepassing uitvoeren door te klikken op **uitvoeren**.

Hier ziet u de volgende uitvoer ziet u de sensorgegevens en de berichten die worden verzonden naar uw IoT-hub.

   ![Hoe uitslag](./media/iot-hub-how-to-order-connection-state-events/raspmsg.png)

   Klik op **stoppen** stoppen van de simulator en het activeren een **apparaat losgekoppeld** gebeurtenis.

U hebt nu een voorbeeld van toepassing voor het verzamelen van gegevens en verzenden naar uw IoT-hub uitvoeren. 

### <a name="observe-events-in-cosmos-db"></a>Bekijk de gebeurtenissen in Cosmos DB

Resultaten van de uitgevoerde opgeslagen procedure ziet u in uw Cosmos DB-document. Hier ziet u hoe het eruit. Houd er rekening mee dat elke rij bevat de meest recente status van het apparaat verbinding per apparaat

   ![Hoe uitslag](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-outcome.png)

## <a name="use-the-azure-cli"></a>Azure CLI gebruiken

In plaats van Azure Portal te gebruiken, kunt u de stappen voor IoT Hub ook uitvoeren met de Azure CLI. Zie voor meer informatie, de Azure CLI-pagina's voor [het maken van een gebeurtenisabonnement](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) en [het maken van een IoT-apparaat](https://docs.microsoft.com/cli/azure/iot/device).

## <a name="clean-up-resources"></a>Resources opschonen

In deze zelfstudie zijn resources gebruikt die kosten voor uw Azure-abonnement met zich meebrengen. Wanneer u klaar bent met de zelfstudie en het testen van de resultaten, moet u daarom de resources uitschakelen of verwijderen die u niet wilt behouden. 

Als u het werk aan uw logische app wilt behouden, kunt u de app uitschakelen in plaats van verwijderen. 

1. Ga naar uw logische app.
2. Selecteer **Verwijderen** of **Uitschakelen** op de blade **Overzicht**. 

Elk abonnement biedt toegang tot één gratis IoT-hub. Als u een gratis hub hebt gemaakt voor deze zelfstudie, hoeft u deze niet te verwijderen om te voorkomen dat er kosten in rekening worden gebracht.

1. Ga naar uw IoT-hub. 
2. Selecteer **Verwijderen** op de blade **Overzicht**. 

Zelfs als u uw IoT-hub wilt behouden, kunt u het gebeurtenisabonnement verwijderen dat u hebt gemaakt. 

1. Selecteer hiervoor **Event Grid** in uw IoT-hub.
2. Selecteer het gebeurtenisabonnement dat u wilt verwijderen. 
3. Selecteer **Verwijderen**. 

Een Azure Cosmos DB-account verwijderen uit de Azure portal, kopiëren en klik op de accountnaam en klik op **-account verwijderen**. Zie voor gedetailleerde instructies [verwijderen van een Azure Cosmos DB-account](https://docs.microsoft.com/azure/cosmos-db/manage-account#delete).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [reageren op gebeurtenissen van IoT-Hub met behulp van Event Grid om acties starten](../iot-hub/iot-hub-event-grid.md)
* [Raadpleeg de zelfstudie IoT Hub-gebeurtenissen](../event-grid/publish-iot-hub-events-to-logic-apps.md)
* Meer informatie over wat u kunt doen met [Event Grid](../event-grid/overview.md)


