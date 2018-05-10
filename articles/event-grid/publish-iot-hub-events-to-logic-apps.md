---
title: Gebeurtenissen van de IoT-Hub aan trigger Azure Logic Apps gebruiken | Microsoft Docs
description: Maak met de gebeurtenis routing-service van Azure Event raster geautomatiseerde processen op basis van gebeurtenissen van de IoT Hub Azure Logic Apps-bewerkingen uit te voeren.
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.service: iot-hub
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2018
ms.author: kgremban
ms.openlocfilehash: 4fed42a45f8d291bd3ba1e4fd5d636b7d0b0fbfc
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="send-email-notifications-about-azure-iot-hub-events-using-logic-apps"></a>Verzenden van e-mailmeldingen over gebeurtenissen van Azure IoT Hub met Logic Apps

Azure Event raster kunt u om te reageren op gebeurtenissen in IoT Hub door acties in uw downstream business-toepassingen activeren.

Dit artikel begeleidt bij een voorbeeldconfiguratie die gebruikmaakt van IoT Hub en Event raster. Door het end hebt u een Azure logic app instellen op een e-mailmelding verzenden telkens wanneer een apparaat wordt toegevoegd aan uw IoT-hub. 

## <a name="prerequisites"></a>Vereisten

* Een e-mailaccount van een e-provider die wordt ondersteund door Azure Logic Apps zoals Outlook van Office 365, Outlook.com of Gmail. Dit e-mailaccount wordt gebruikt om de gebeurtenismeldingen te verzenden. Zie voor een volledige lijst met ondersteunde logische App-connectors, de [Connectors-overzicht](https://docs.microsoft.com/connectors/)
* Een actief Azure-account. Als u niet hebt, kunt u [een gratis account maken](http://azure.microsoft.com/pricing/free-trial/).
* Een Iot-hub in Azure. Als u een nog niet hebt gemaakt, Zie [aan de slag met IoT Hub](../iot-hub/iot-hub-csharp-csharp-getstarted.md) voor een overzicht. 

## <a name="create-a-logic-app"></a>Een logische app maken

Eerst een logische app maken en toevoegen van een trigger voor de gebeurtenis raster dat de resourcegroep voor uw virtuele machine bewaakt. 

### <a name="create-a-logic-app-resource"></a>Maak een logische app-resource


1. In de [Azure-portal](https://portal.azure.com), selecteer **nieuw** > **Enterprise Integration** > **logische App**.

   ![Logische app maken](./media/publish-iot-hub-events-to-logic-apps/select-logic-app.png)

2. Geef een naam die uniek is in uw abonnement op uw logische app in en selecteer hetzelfde abonnement, resourcegroep en locatie als uw IoT-hub. 
3. Wanneer u klaar bent, selecteert u **vastmaken aan dashboard**, en kies **maken**.

   U hebt nu een Azure-resource voor uw logische app gemaakt. Nadat de logische app is geïmplementeerd, toont de ontwerper van logische apps sjablonen voor algemene patronen, zodat u sneller aan de slag kunt.

   > [!NOTE] 
   > Wanneer u selecteert **vastmaken aan dashboard**, uw logische app automatisch wordt geopend in Logic Apps Designer. U kunt anders handmatig zoeken en open uw logische app.

4. In de ontwerpfunctie voor Logic App onder **sjablonen**, kies **lege logische App** zodat u uw logische app helemaal kunt samenstellen.

## <a name="select-a-trigger"></a>Selecteer een trigger

Een trigger is een specifieke gebeurtenis die uw logische app wordt gestart. Voor deze zelfstudie is de trigger die uit de werkstroom ingesteld ontvangst van een aanvraag via HTTP.  

1. Typ in de connectors en triggers zoekbalk **HTTP**.
2. Selecteer **aanvraag: wanneer een HTTP-aanvraag wordt ontvangen** als de trigger. 

   ![Selecteer HTTP-aanvraag trigger](./media/publish-iot-hub-events-to-logic-apps/http-request-trigger.png)

3. Selecteer **gebruik voorbeeld nettolading voor het genereren van schema**. 

   ![Selecteer HTTP-aanvraag trigger](./media/publish-iot-hub-events-to-logic-apps/sample-payload.png)

4. Plak de volgende voorbeeldcode JSON in het tekstvak in en selecteer vervolgens **gedaan**:

   ```json
   [{
     "id": "56afc886-767b-d359-d59e-0da7877166b2",
     "topic": "/SUBSCRIPTIONS/<Subscription ID>/RESOURCEGROUPS/<Resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<IoT hub name>",
     "subject": "devices/LogicAppTestDevice",
     "eventType": "Microsoft.Devices.DeviceCreated",
     "eventTime": "2018-01-02T19:17:44.4383997Z",
     "data": {
       "twin": {
         "deviceId": "LogicAppTestDevice",
         "etag": "AAAAAAAAAAE=",
         "status": "enabled",
         "statusUpdateTime": "0001-01-01T00:00:00",
         "connectionState": "Disconnected",
         "lastActivityTime": "0001-01-01T00:00:00",
         "cloudToDeviceMessageCount": 0,
         "authenticationType": "sas",
         "x509Thumbprint": {
           "primaryThumbprint": null,
           "secondaryThumbprint": null
         },
         "version": 2,
         "properties": {
           "desired": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           },
           "reported": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           }
         }
       },
       "hubName": "egtesthub1",
       "deviceId": "LogicAppTestDevice",
       "operationTimestamp": "2018-01-02T19:17:44.4383997Z",
       "opType": "DeviceCreated"
     },
     "dataVersion": "",
     "metadataVersion": "1"
   }]
   ```
5. Krijgt u een pop-upbericht dat **onthouden om op te nemen van een header Content-Type is ingesteld op application/json in uw aanvraag.** U kunt veilig negeren deze suggestie en gaat u verder met de volgende sectie. 


### <a name="create-an-action"></a>Een actie maken

Acties zijn stappen die worden uitgevoerd nadat de trigger de logic app-werkstroom wordt gestart. Voor deze zelfstudie wordt de actie is een e-mailmelding verzenden vanuit uw e-mailprovider. 

1. Selecteer **nieuwe stap** vervolgens **een actie toevoegen**. 

   ![Nieuwe stap, een actie toevoegen](./media/publish-iot-hub-events-to-logic-apps/new-step.png)

2. Zoeken naar **e**. 
3. Zoek en selecteer de bijbehorende connector op basis van uw e-mailprovider. Deze zelfstudie wordt gebruikgemaakt van **Outlook van Office 365**. De stappen voor andere e-mailproviders lijken. 

   ![Selecteer e provider-connector](./media/publish-iot-hub-events-to-logic-apps/o365-outlook.png)

4. Selecteer de **e-mailbericht verzenden** in te grijpen. 
5. Als u wordt gevraagd, moet u zich aanmelden bij uw e-mailaccount. 
6. Uw e-mailsjabloon maken. 
   * **Naar**: Voer het e-mailadres voor het ontvangen van de e-mailmeldingen. Gebruik een e-mailaccount dat u toegang hebt tot voor het testen voor deze zelfstudie. 
   * **Onderwerp** en **hoofdtekst**: de tekst voor uw e-mailadres schrijven. Selecteer JSON-eigenschappen in het hulpprogramma selector om op te nemen van dynamische inhoud op basis van gegevens van gebeurtenissen.  

   Uw e-mailsjabloon eruit als in dit voorbeeld:

   ![Vul informatie per e-mail](./media/publish-iot-hub-events-to-logic-apps/email-content.png)

7. Sla uw logische app op. 

### <a name="copy-the-http-url"></a>Kopieer de HTTP-URL

Voordat u de ontwerpfunctie van Logic Apps laat, Kopieer de URL waarnaar uw logische apps luistert voor een trigger. U deze URL gebruiken voor het configureren van de gebeurtenis raster. 

1. Vouw de **wanneer een HTTP-aanvraag wordt ontvangen** trigger configuratie vak door erop te klikken. 
2. Kopieer de waarde van **HTTP POST-URL** door het selecteren van de knop kopiëren ernaast. 

   ![Kopieer de HTTP POST-URL](./media/publish-iot-hub-events-to-logic-apps/copy-url.png)

3. Deze URL opslaan zodat u naar deze in de volgende sectie verwijzen kunt. 

## <a name="publish-an-event-from-iot-hub"></a>Een gebeurtenis uit IoT Hub publiceren

In deze sectie configureert u uw IoT-Hub voor het publiceren van gebeurtenissen, als deze problemen optreden. 

1. Navigeer naar uw IoT-hub in de Azure portal. 
2. Selecteer **gebeurtenissen**.

   ![De details van gebeurtenis raster openen](./media/publish-iot-hub-events-to-logic-apps/event-grid.png)

3. Selecteer **gebeurtenisabonnement**. 

   ![Nieuwe gebeurtenisabonnement maken](./media/publish-iot-hub-events-to-logic-apps/event-subscription.png)

4. De gebeurtenisabonnement maken met de volgende waarden: 
   * **Naam**: Geef een beschrijvende naam.
   * **Abonneren op alle gebeurtenistypen**: Schakel het selectievakje uit.
   * **Gebeurtenistypen**: Selecteer **DeviceCreated**.
   * **Abonnee type**: Selecteer **Web haakje**.
   * **Abonnee eindpunt**: plak de URL die u hebt gekopieerd uit uw logische app. 

   U kan het gebeurtenisabonnement hier opslaat en meldingen ontvangen voor elk apparaat dat is gemaakt in uw IoT-hub. Voor deze zelfstudie echter we gebruiken de optionele velden om te filteren op specifieke apparaten: 

   * **Filter het voorvoegsel**: Voer `devices/Building1_` kunt filteren op apparaatgebeurtenissen in het gebouw 1.
   * **Achtervoegsel filter**: Voer `_Temperature` kunt filteren op apparaatgebeurtenissen met betrekking tot temperatuur.

   Wanneer u bent klaar, moet de vorm moet eruitzien als in het volgende voorbeeld: 

   ![Voorbeeld gebeurtenis abonnement formulier](./media/publish-iot-hub-events-to-logic-apps/subscription-form.png)

5. Selecteer **maken** om op te slaan van het gebeurtenisabonnement.

## <a name="create-a-new-device"></a>Een nieuw apparaat maken

Uw logische app testen door het maken van een nieuw apparaat om te activeren van een gebeurtenis meldingse-mail. 

1. Selecteer in uw IoT-hub **IoT-apparaten**. 
2. Selecteer **Toevoegen**.
3. Voor **apparaat-ID**, voer `Building1_Floor1_Room1_Temperature`.
4. Selecteer **Opslaan**. 
5. U kunt meerdere apparaten met verschillende apparaat-id's voor het testen van de gebeurtenis abonnementsfilters toevoegen. Probeer deze voorbeelden: 
   * Building1_Floor1_Room1_Light
   * Building1_Floor2_Room2_Temperature
   * Building2_Floor1_Room1_Temperature
   * Building2_Floor1_Room1_Light

Zodra u enkele apparaten aan uw IoT-hub hebt toegevoegd, Controleer je e-mail om te zien welke u de logische app geactiveerd. 

## <a name="use-the-azure-cli"></a>Azure CLI gebruiken

In plaats van de Azure-portal, kunt u de stappen van de IoT Hub met de Azure CLI uitvoeren. Zie voor meer informatie, de Azure CLI-pagina's voor [maken van een gebeurtenisabonnement](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) en [maken van een IoT-apparaat](https://docs.microsoft.com/cli/azure/iot/device)

## <a name="clean-up-resources"></a>Resources opschonen

Deze zelfstudie gebruikt bronnen die op uw Azure-abonnement kosten. Wanneer u klaar bent uitproberen van de zelfstudie en het testen van uw resultaten, uitschakelen of verwijderen van de resources die u niet wilt behouden. 

Als u niet dat het werk op uw logische app verliest wilt, kunt u deze in plaats van het verwijderen van deze uitschakelen. 

1. Navigeer naar uw logische app.
2. Op de **overzicht** blade Selecteer **verwijderen** of **uitschakelen**. 

Elk abonnement kan één gratis IoT-hub hebben. Als u een gratis hub voor deze zelfstudie hebt gemaakt, moet u niet om te voorkomen dat de kosten te verwijderen.

1. Navigeer naar uw IoT-hub. 
2. Op de **overzicht** blade Selecteer **verwijderen**. 

Zelfs als u uw IoT-hub wilt behouden, kunt u mogelijk wilt verwijderen van het abonnement op gebeurtenissen die u hebt gemaakt. 

1. Selecteer in uw IoT-hub **gebeurtenis raster**.
2. Selecteer het gebeurtenisabonnement die u wilt verwijderen. 
3. Selecteer **Verwijderen**. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [reageert op gebeurtenissen van de IoT Hub met behulp van de gebeurtenis raster acties starten](../iot-hub/iot-hub-event-grid.md).

Meer informatie over wat u kunt doen met [gebeurtenis raster](overview.md).


