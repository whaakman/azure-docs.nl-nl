---
title: Externe controle IoT en meldingen met Azure Logic Apps | Microsoft Docs
description: Azure Logic Apps gebruiken voor het bewaken van IoT temperatuur op uw IoT-hub en automatisch e-mailmeldingen te verzenden naar uw postvak voor elke gedetecteerde afwijkingen.
author: robinsh
keywords: IOT controlemeldingen, iot, iot temperatuur controleren
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: robinsh
ms.openlocfilehash: 5d5b1d1579600767153fcf5ad751e1224631d611
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59262513"
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>Externe controle IoT en meldingen met Azure Logic Apps die gebruikmaken van uw IoT-hub en Postvak

![Diagram voor end-to-end](media/iot-hub-monitoring-notifications-with-azure-logic-apps/iot-hub-e2e-logic-apps.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Met Azure Logic Apps biedt een manier om processen te automatiseren als een reeks stappen. Een logische app kan verbinding maken in verschillende services en protocollen. Deze begint met een trigger, zoals 'Wanneer een account wordt toegevoegd', gevolgd door een combinatie van acties, zoals het verzenden van pushmeldingen. Deze functie kunnen Logic Apps een perfecte IoT-oplossing voor IoT bewaking, zoals de waarschuwing voor afwijkingen, onder andere gebruiksscenario's blijven.

## <a name="what-you-learn"></a>Wat u leert

U informatie over het maken van een logische app waarmee uw IoT-hub en uw postvak in voor het controleren van de temperatuur en meldingen. Wanneer de temperatuur hoger dan 30 C is, de clienttoepassing markeert `temperatureAlert = "true"` in het bericht wordt verzonden naar uw IoT hub. Het bericht activeert de logische app voor het verzenden van een e-mailmelding.

## <a name="what-you-do"></a>Wat u allemaal doen

* Maak een service bus-naamruimte en een wachtrij aan toe te voegen.
* Een eindpunt en een regel voor doorsturen toevoegen aan uw IoT-hub.
* Maken, configureren en testen van een logische app.

## <a name="what-you-need"></a>Wat u nodig hebt

* Zelfstudie [instellen van uw apparaat](iot-hub-raspberry-pi-kit-node-get-started.md) voltooid die voorziet in de volgende vereisten:

  * Een actief Azure-abonnement.
  * Een Azure IoT-hub in uw abonnement.
  * Een clienttoepassing die berichten naar uw Azure-IoT-hub verzendt.

## <a name="create-service-bus-namespace-and-add-a-queue-to-it"></a>Service bus-naamruimte maken en een wachtrij aan toe te voegen

### <a name="create-a-service-bus-namespace"></a>Een service bus-naamruimte maken

1. Op de [Azure-portal](https://portal.azure.com/), selecteer **een resource maken** > **bedrijfsintegratie** > **Service Bus**.

2. Geef de volgende informatie op:

   **Naam**: De naam van de servicebus.

   **Prijscategorie**: Selecteer **Basic** > **Selecteer**. De Basic-laag is voldoende voor deze zelfstudie.

   **Resourcegroep**: Gebruik dezelfde resourcegroep bevinden die gebruikmaakt van uw IoT-hub.

   **Locatie**: Gebruik de dezelfde locatie die gebruikmaakt van uw IoT-hub.

3. Selecteer **Maken**.

   ![Een service bus-naamruimte maken in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a>Een service bus-wachtrij toevoegen

1. Open de service bus-naamruimte en selecteer vervolgens **+ wachtrij**.

1. Voer een naam voor de wachtrij en selecteer vervolgens **maken**.

1. Open de service bus-wachtrij en selecteer vervolgens **beleid voor gedeelde toegang** > **+ toevoegen**.

1. Voer een naam voor het beleid, controle **beheren**, en selecteer vervolgens **maken**.

   ![Een service bus-wachtrij toevoegen in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-query-to-your-iot-hub"></a>Een eindpunt en een routering query toevoegen aan uw IoT-hub

Een eindpunt en een routering query toevoegen aan uw Iot-hub.

### <a name="add-an-endpoint"></a>Een eindpunt toevoegen

1. Open uw IoT-hub, selecteer **eindpunten** > **+ toevoegen**.

1. Voer de volgende informatie in:

   **Naam**: De naam van het eindpunt.

   **Eindpunt van het type**: Selecteer een **Service Bus-wachtrij**.

   **Service Bus-naamruimte**: Selecteer de naamruimte die u hebt gemaakt.

   **Service Bus-wachtrij**: Selecteer de wachtrij die u hebt gemaakt.

3. Selecteer **OK**.

   ![Een eindpunt toevoegen aan uw IoT-hub in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a>Een routeringsregel toevoegen

1. Selecteer in uw IoT-hub **Routes** > **+ toevoegen**.

2. Voer de volgende informatie in:

   **Naam**: De naam van de regel voor doorsturen.

   **Gegevensbron**: Selecteer **DeviceMessages**.

   **Eindpunt**: Selecteer het eindpunt dat u hebt gemaakt.

   **Querytekenreeks**: Voer `temperatureAlert = "true"` in.

3. Selecteer **Opslaan**.

   ![Een regel voor doorsturen toevoegen in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a>Een logische app maken en configureren

Vervolgens maken en configureren van een logische app.

### <a name="create-a-logic-app"></a>Een logische app maken

1. In de [Azure-portal](https://portal.azure.com/), selecteer **een resource maken** > **bedrijfsintegratie** > **logische App**.

2. Voer de volgende informatie in:

   **Naam**: De naam van de logische app.

   **Resourcegroep**: Gebruik dezelfde resourcegroep bevinden die gebruikmaakt van uw IoT-hub.

   **Locatie**: Gebruik de dezelfde locatie die gebruikmaakt van uw IoT-hub.

3. Selecteer **Maken**.

### <a name="configure-the-logic-app"></a>Configureren van de logische app

1. Open de logische app die wordt geopend in de ontwerper van logische Apps.

2. Selecteer in de ontwerper van logische Apps **lege logische App**.

   ![Beginnen met een lege, logische app in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

3. Selecteer **Service Bus**.

   ![Selecteer de Service Bus om te beginnen met het maken van uw logische app in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

4. Selecteer **Service Bus-wanneer een of meer berichten in een wachtrij (automatisch voltooien aankomen)**.

5. Maak een service bus-verbinding.

   1. Voer de naam van een verbinding.

   2. Selecteer de service bus-naamruimte > de service bus-beleid > **maken**.

      ![Een service bus-verbinding voor uw logische app maken in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   3. Selecteer **doorgaan** nadat de service bus-verbinding is gemaakt.

   4. Selecteer de wachtrij die u hebt gemaakt en voer `175` voor **bericht maximumaantal**.

      ![Geef het maximum aantal berichten voor de service bus-verbinding in uw logische app](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)

   5. Selecteer de knop ' opslaan' de wijzigingen op te slaan.

6. Maak een SMTP-service-verbinding.

   1. Selecteer **nieuwe stap** > **een actie toevoegen**.

   2. Type `SMTP`, selecteer de **SMTP** service in de zoekresultaten en selecteer vervolgens **SMTP - e-mailbericht verzenden**.

      ![Maak een SMTP-verbinding in uw logische app in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   3. Geef de SMTP-gegevens van uw postvak in en selecteer vervolgens **maken**.

      ![SMTP-verbindingsgegevens invoeren in uw logische app in Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      Haal de SMTP-informatie voor [Hotmail/Outlook.com](https://support.office.com/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), en [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).

   4. Voer uw e-mailadres voor **van** en **naar**, en `High temperature detected` voor **onderwerp** en **hoofdtekst**.

   5. Selecteer **Opslaan**.

De logische app is in staat wanneer u deze opslaat.

## <a name="test-the-logic-app"></a>Test de logische app

1. Start de clienttoepassing die u implementeert op uw apparaat bij [ESP8266 verbinding maken met Azure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md).

2. Verhoog de temperatuur omgeving rond de SensorTag aan meer dan 30 C. Bijvoorbeeld, een kaars rond uw SensorTag licht.

3. U ontvangt een e-mailmelding verzonden door de logische app.

   > [!NOTE]
   > Uw e-serviceprovider mogelijk om te controleren of de identiteit van de afzender om ervoor te zorgen dat het ligt aan jullie die het e-mailbericht verzendt.

## <a name="next-steps"></a>Volgende stappen

U hebt gemaakt een logische app waarmee uw IoT-hub en uw postvak in voor het controleren van de temperatuur en meldingen.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
