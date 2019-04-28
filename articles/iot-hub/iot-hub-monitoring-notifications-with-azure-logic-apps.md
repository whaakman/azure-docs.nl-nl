---
title: Externe controle IoT en meldingen met Azure Logic Apps | Microsoft Docs
description: Azure Logic Apps gebruiken voor het bewaken van IoT temperatuur op uw IoT-hub en automatisch e-mailmeldingen te verzenden naar uw postvak voor elke gedetecteerde afwijkingen.
author: robinsh
keywords: IOT controlemeldingen, iot, iot temperatuur controleren
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/19/2019
ms.author: robinsh
ms.openlocfilehash: 26637468f44e12f7ad66f907e0f6be3d907e578f
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126083"
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>Externe controle IoT en meldingen met Azure Logic Apps die gebruikmaken van uw IoT-hub en Postvak

![Diagram voor end-to-end](media/iot-hub-monitoring-notifications-with-azure-logic-apps/iot-hub-e2e-logic-apps.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[Met Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/) kunt u werkstromen indelen in on-premises en cloud-services, een of meer ondernemingen, en tussen verschillende protocollen. Een logische app begint met een trigger, die wordt gevolgd door een of meer acties die kunnen worden geordend met ingebouwde besturingselementen, zoals de voorwaarden en iterators. Deze flexibiliteit maakt Logic Apps een ideale IoT-oplossing voor controle IoT-scenario's. Bijvoorbeeld, kan de aankomst van telemetriegegevens van een apparaat op een IoT Hub-eindpunt logic app workflows datawarehouse van de gegevens in een Azure Storage-blob, het verzenden van e-mailwaarschuwingen waarschuwen bij gegevens afwijkingen, plannen dat een technicus bezoek als een apparaat een fout gemeld initiëren , enzovoort.

## <a name="what-you-learn"></a>Wat u leert

U informatie over het maken van een logische app waarmee uw IoT-hub en uw postvak in voor het controleren van de temperatuur en meldingen.

De clientcode die wordt uitgevoerd op uw apparaat wordt een eigenschap van de toepassing, `temperatureAlert`op elke telemetrie bericht wordt verzonden naar uw IoT hub. Als de clientcode een temperatuur hoger dan 30 C detecteert, wordt deze eigenschap ingesteld op `true`; anders wordt de eigenschap ingesteld op `false`.

In dit onderwerp stelt u routering op uw IoT-hub voor het verzenden van berichten waarin `temperatureAlert = true` naar een Service Bus-eindpunt, en u een logische app instelt die wordt geactiveerd op de berichten die binnenkomen in de Service Bus-eindpunt en verzendt u een e-mailmelding.

## <a name="what-you-do"></a>Wat u allemaal doen

* Een Service Bus-naamruimte maken en een Service Bus-wachtrij aan toe te voegen.
* Een aangepast eindpunt en een regel voor het doorsturen naar uw IoT-hub te routeren van berichten met een waarschuwing temperatuur naar de Service Bus-wachtrij toevoegen.
* Maken, configureren en testen van een logische app voor het gebruiken van berichten van uw Service Bus-wachtrij en e-mailmeldingen verzenden naar een gewenste ontvanger.

## <a name="what-you-need"></a>Wat u nodig hebt

* Voltooi de [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md) zelfstudie of een van de apparaat-zelfstudies, bijvoorbeeld [Raspberry Pi met node.js](iot-hub-raspberry-pi-kit-node-get-started.md). Deze betrekking hebben op de volgende vereisten:

  * Een actief Azure-abonnement.
  * Een Azure IoT-hub in uw abonnement.
  * Een clienttoepassing uitvoert op het apparaat dat telemetrieberichten naar uw Azure IoT-hub verzendt.

## <a name="create-service-bus-namespace-and-queue"></a>Service Bus-naamruimte en wachtrij maken

Maak een Service Bus-naamruimte en -wachtrij. Verderop in dit onderwerp maakt u een regel voor doorsturen in uw IoT-hub kunt u direct berichten met een waarschuwing temperatuur aan de Service Bus-wachtrij, waar ze opgepikt door een logische app en het verzenden van een e-mailmelding activeren.

### <a name="create-a-service-bus-namespace"></a>Een Service Bus-naamruimte maken

1. Op de [Azure-portal](https://portal.azure.com/), selecteer **+ een resource maken** > **integratie** > **Service Bus**.

1. Op de **-naamruimte maken** in het deelvenster de volgende informatie:

   **Naam**: De naam van de service bus-naamruimte. De naamruimte moet uniek zijn in Azure.

   **Prijscategorie**: Selecteer **Basic** uit de vervolgkeuzelijst. De Basic-laag is voldoende voor deze zelfstudie.

   **Resourcegroep**: Gebruik dezelfde resourcegroep bevinden die gebruikmaakt van uw IoT-hub.

   **Locatie**: Gebruik de dezelfde locatie die gebruikmaakt van uw IoT-hub.

   ![Een service bus-naamruimte maken in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1-create-service-bus-namespace-azure-portal.png)

1. Selecteer **Maken**. Wacht tot de installatie is voltooid voordat u doorgaat met de volgende stap.

### <a name="add-a-service-bus-queue-to-the-namespace"></a>Een Service Bus-wachtrij toevoegen aan de naamruimte

1. Open de Service Bus-naamruimte. De eenvoudigste manier om te gaan naar de Service Bus-naamruimte is om te selecteren **resourcegroepen** in het deelvenster resource, selecteer de resourcegroep en selecteer vervolgens de Service Bus-naamruimte uit de lijst met resources.

1. Op de **Service Bus Namespace** venster **+ wachtrij**.

1. Voer een naam voor de wachtrij en selecteer vervolgens **maken**. Wanneer de wachtrij is gemaakt, de **wachtrij maken** deelvenster wordt gesloten.

   ![Een service bus-wachtrij toevoegen in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/create-service-bus-queue.png)

1. Terug op de **Service Bus Namespace** deelvenster onder **entiteiten**, selecteer **wachtrijen**. Open de Service Bus-wachtrij in de lijst en selecteer vervolgens **beleid voor gedeelde toegang** > **+ toevoegen**.

1. Voer een naam voor het beleid, controle **beheren**, en selecteer vervolgens **maken**.

   ![Toevoegen van een service bus-wachtrij-beleid in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2-add-service-bus-queue-azure-portal.png)

## <a name="add-a-custom-endpoint-and-routing-rule-to-your-iot-hub"></a>Een aangepast eindpunt en een regel voor het doorsturen naar uw IoT-hub toevoegen

Een aangepast eindpunt voor de Service Bus-wachtrij toevoegen aan uw IoT-hub en maak een regel om de berichten met een waarschuwing temperatuur naar dit eindpunt, waar ze opgepikt door uw logische app voor het doorsturen van bericht. De routeringsregel gebruikmaakt van een query routering, `temperatureAlert = "true"`, voor het doorsturen van berichten op basis van de waarde van de `temperatureAlert` eigenschap van de toepassing ingesteld door de clientcode die wordt uitgevoerd op het apparaat. Zie voor meer informatie, [Message routing query die is gebaseerd op berichteigenschappen](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#message-routing-query-based-on-message-properties).

### <a name="add-a-custom-endpoint"></a>Een aangepast eindpunt toevoegen

1. Open uw IoT-hub. De eenvoudigste manier om te gaan naar de IoT-hub is om te selecteren **resourcegroepen** in het deelvenster resource, selecteer de resourcegroep en selecteer vervolgens uw IoT-hub uit de lijst met resources.

1. Onder **Messaging**, selecteer **berichtroutering**. Op de **berichtroutering** venster de **aangepaste eindpunten** tabblad en selecteer vervolgens **+ toevoegen**. Selecteer in de vervolgkeuzelijst **Service bus-wachtrij**.

   ![Een eindpunt toevoegen aan uw IoT-hub in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/select-iot-hub-custom-endpoint.png)

1. Op de **toevoegen van een service bus-eindpunt** deelvenster, voer de volgende informatie:

   **Naam van het eindpunt**: De naam van het eindpunt.

   **Service bus-naamruimte**: Selecteer de naamruimte die u hebt gemaakt.

   **Service bus-wachtrij**: Selecteer de wachtrij die u hebt gemaakt.

   ![Een eindpunt toevoegen aan uw IoT-hub in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3-add-iot-hub-endpoint-azure-portal.png)

1. Selecteer **Maken**. Nadat het eindpunt is gemaakt, gaat u verder met de volgende stap.

### <a name="add-a-routing-rule"></a>Een routeringsregel toevoegen

1. Terug op de **berichtroutering** venster de **Routes** tabblad en selecteer vervolgens **+ toevoegen**.

1. Op de **een route toevoegen** deelvenster, voer de volgende informatie:

   **Naam**: De naam van de regel voor doorsturen.

   **Eindpunt**: Selecteer het eindpunt dat u hebt gemaakt.

   **Gegevensbron**: Selecteer **apparaat Telemetrieberichten**.

   **Routeringsquery**: Voer `temperatureAlert = "true"` in.

   ![Een regel voor doorsturen toevoegen in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4-add-routing-rule-azure-portal.png)

1. Selecteer **Opslaan**. Kunt u sluiten de **berichtroutering** deelvenster.

## <a name="create-and-configure-a-logic-app"></a>Een logische App maken en configureren

In de vorige sectie, moet u uw IoT-hub te routeren van berichten met een waarschuwing temperatuur naar uw Service Bus-wachtrij instellen. Nu instellen van een logische app om te controleren van de Service Bus-wachtrij en een e-mailmelding verzenden wanneer er een bericht wordt toegevoegd aan de wachtrij.

### <a name="create-a-logic-app"></a>Een logische app maken

1. Selecteer **een resource maken** > **integratie** > **logische App**.

1. Voer de volgende informatie in:

   **Naam**: De naam van de logische app.

   **Resourcegroep**: Gebruik dezelfde resourcegroep bevinden die gebruikmaakt van uw IoT-hub.

   **Locatie**: Gebruik de dezelfde locatie die gebruikmaakt van uw IoT-hub.

   ![Een logische app maken in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/create-a-logic-app.png)

1. Selecteer **Maken**.

### <a name="configure-the-logic-app-trigger"></a>De logische app-trigger configureren

1. Open de logische app. De eenvoudigste manier om aan de logische app is om te selecteren **resourcegroepen** in het deelvenster resource, selecteer de resourcegroep en selecteer vervolgens uw logische app uit de lijst met resources. Wanneer u de logische app selecteert, wordt de ontwerper van logische Apps geopend.

1. In de ontwerper van logische Apps, schuif omlaag naar **sjablonen** en selecteer **lege logische App**.

   ![Beginnen met een lege, logische app in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5-start-with-blank-logic-app-azure-portal.png)

1. Selecteer de **alle** tabblad en selecteer vervolgens **Service Bus**.

   ![Selecteer de Service Bus om te beginnen met het maken van uw logische app in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6-select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. Onder **Triggers**, selecteer **als een of meer berichten aankomen in een wachtrij (automatisch voltooien)**.

   ![Selecteer de trigger voor uw logische app in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/select-service-bus-trigger.png)

1. Maak een service bus-verbinding.
   1. Voer de naam van een verbinding en selecteer uw Service Bus-naamruimte in de lijst. Het volgende scherm wordt geopend.

      ![Een service bus-verbinding voor uw logische app maken in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/create-service-bus-connection-1.png)

   1. Selecteer de service bus-beleid (RootManageSharedAccessKey). Selecteer vervolgens **maken**.

      ![Een service bus-verbinding voor uw logische app maken in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7-create-service-bus-connection-in-logic-app-azure-portal.png)

   1. Op het laatste scherm voor **wachtrijnaam**, selecteert u de wachtrij die u hebt gemaakt in de vervolgkeuzelijst. Voer `175` voor **bericht maximumaantal**.

      ![Geef het maximum aantal berichten voor de service bus-verbinding in uw logische app](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8-specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)

   1. Selecteer **opslaan** in het menu aan de bovenkant van de ontwerper van logische Apps uw wijzigingen op te slaan.

### <a name="configure-the-logic-app-action"></a>De actie voor logische app configureren

1. Maak een SMTP-service-verbinding.

   1. Selecteer **Nieuwe stap**. In **een actie kiezen**, selecteer de **alle** tabblad.

   1. Type `smtp` in het zoekvak, selecteer de **SMTP** service in de zoekresultaten en selecteer vervolgens **E-mail verzenden**.

      ![Maak een SMTP-verbinding in uw logische app in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9-create-smtp-connection-logic-app-azure-portal.png)

   1. Voer de SMTP-gegevens voor uw postvak in en selecteer vervolgens **maken**.

      ![SMTP-verbindingsgegevens invoeren in uw logische app in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10-enter-smtp-connection-info-logic-app-azure-portal.png)

      Haal de SMTP-informatie voor [Hotmail/Outlook.com](https://support.office.com/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), en [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).

      > [!NOTE]
      > U moet mogelijk uitschakelen SSL verbinding te maken. Als dit het geval is en u wilt dat SSL opnieuw inschakelen nadat de verbinding tot stand is gebracht, controleert u de optionele stap aan het einde van deze sectie.

   1. Uit de **toevoegen van nieuwe parameter** vervolgkeuzelijst op de **E-mail verzenden** stap, selecteer **van**, **naar**, **onderwerp**en **hoofdtekst**. Klik of tik op het scherm om te sluiten van het selectiekader.

      ![SMTP-e-mailbericht Verbindingsvelden kiezen](media/iot-hub-monitoring-notifications-with-azure-logic-apps/smtp-connection-choose-fields.png)

   1. Voer uw e-mailadres voor **van** en **naar**, en `High temperature detected` voor **onderwerp** en **hoofdtekst**. Als de **Voeg dynamische inhoud toe van de apps en connectors in deze stroom** dialoogvenster wordt geopend, selecteert u **verbergen** om deze te sluiten. U gebruik geen dynamische inhoud in deze zelfstudie.

      ![Invulvelden SMTP verbinding e-mailadres](media/iot-hub-monitoring-notifications-with-azure-logic-apps/fill-in-smtp-connection-fields.png)

   1. Selecteer **opslaan** om op te slaan van de SMTP-verbinding.

1. (Optioneel) Als u had om uit te schakelen SSL om een verbinding maken met uw e-mailprovider en wilt dat deze opnieuw inschakelen, volgt u deze stappen:

   1. Op de **logische app** deelvenster onder **ontwikkeltools**, selecteer **API-verbindingen**.

   1. In de lijst van API-verbindingen, selecteert u de SMTP-verbinding.

   1. Op de **smtp API-verbinding** deelvenster onder **algemene**, selecteer **bewerken API-verbinding**.

   1. Op de **API-verbinding bewerken** venster **SSL inschakelen?**, voer het wachtwoord voor uw e-mailaccount en selecteert u **opslaan**.

      ![SMTP-API-verbinding in uw logische app in Azure portal bewerken](media/iot-hub-monitoring-notifications-with-azure-logic-apps/re-enable-smtp-connection-ssl.png)

Uw logische app is nu gereed om te verwerken temperatuur waarschuwingen van de Service Bus-wachtrij en meldingen naar uw e-mailaccount verzenden.

## <a name="test-the-logic-app"></a>Test de logische app

1. De clienttoepassing op uw apparaat te starten.

1. Als u een fysiek apparaat, zorgvuldig Breng een heatmap-bron in de buurt van de heatmap-sensor totdat de temperatuur hoger is dan 30 graden C. Als u de online simulator, uitvoer de clientcode telemetrieberichten die groter zijn dan 30 C. willekeurig

1. U moet beginnen met het ontvangen van e-mailmeldingen verzonden door de logische app.

   > [!NOTE]
   > Uw e-serviceprovider mogelijk om te controleren of de identiteit van de afzender om ervoor te zorgen dat het ligt aan jullie die het e-mailbericht verzendt.

## <a name="next-steps"></a>Volgende stappen

U hebt gemaakt een logische app waarmee uw IoT-hub en uw postvak in voor het controleren van de temperatuur en meldingen.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
