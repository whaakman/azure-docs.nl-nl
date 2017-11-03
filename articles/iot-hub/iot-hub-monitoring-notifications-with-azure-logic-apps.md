---
title: Externe controle IoT en meldingen met Azure Logic Apps | Microsoft Docs
description: Azure Logic Apps gebruiken voor het bewaken van IoT temperatuur op uw IoT-hub en automatisch e-mailmeldingen te verzenden naar uw postvak voor eventuele afwijkingen gedetecteerd.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: bewaking, iot-meldingen IOT iot Temperatuurbewaking
ms.assetid: 43043067-2e1f-42c9-953d-e2dce8fd86df
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: xshi
ms.openlocfilehash: 7a611912ae55eb22103539dbba9f1a06aaa543b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>Externe controle IoT en meldingen met Azure Logic Apps die gebruikmaken van uw IoT-hub en Postvak

![End-to-end-diagram](media/iot-hub-get-started-e2e-diagram/7.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Logische Apps van Azure biedt een manier voor het automatiseren van processen als een reeks stappen. Een logische app kan verbinding maken tussen verschillende services en protocollen. Deze begint met een trigger, zoals 'Wanneer een account is toegevoegd', en gevolgd door een combinatie van acties zoals het verzenden van een pushmelding. Deze functie stelt Logic Apps een perfecte IoT-oplossing voor IoT bewaking, zoals de waarschuwing op afwijkingen is vereist, onder andere gebruiksscenario's bijwerkt.

## <a name="what-you-learn"></a>Wat u leert

U informatie over het maken van een logische app die verbinding maakt van uw IoT-hub en uw postvak voor het controleren van de temperatuur en meldingen. Wanneer de temperatuur hoger dan 30 C is, de clienttoepassing markeert `temperatureAlert = "true"` in het bericht naar uw IoT-hub verzendt. Het bericht de logische app voor het verzenden van een e-mailmelding wordt geactiveerd.

## <a name="what-you-do"></a>Wat u doet

* Maken van een service bus-naamruimte en een wachtrij aan toe te voegen.
* Een eindpunt en een regel voor doorsturen toevoegen aan uw IoT-hub.
* Maken, configureren en testen van een logische app.

## <a name="what-you-need"></a>Wat u nodig hebt

* Zelfstudie [instellen van uw apparaat](iot-hub-raspberry-pi-kit-node-get-started.md) voltooid die wordt ingegaan op de volgende vereisten:
  * Een actief Azure-abonnement.
  * Een Azure-IoT-hub in uw abonnement.
  * Een clienttoepassing dat berichten naar uw Azure-IoT-hub verzendt.

## <a name="create-service-bus-namespace-and-add-a-queue-to-it"></a>Service bus-naamruimte maken en toevoegen van een wachtrij met

### <a name="create-a-service-bus-namespace"></a>Een service bus-naamruimte maken

1. Op de [Azure-portal](https://portal.azure.com/), klikt u op **nieuw** > **Enterprise Integration** > **Service Bus**.
1. Geef de volgende informatie:

   **Naam**: de naam van de servicebus.

   **Prijscategorie**: klik op **Basic** > **Selecteer**. De laag Basic is voldoende voor deze zelfstudie.

   **Resourcegroep**: gebruik dezelfde resourcegroep die gebruikmaakt van uw IoT-hub.

   **Locatie**: gebruik van dezelfde locatie die gebruikmaakt van uw IoT-hub.
1. Klik op **Create**.

   ![Een service bus-naamruimte maken in de Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a>Toevoegen van een service bus-wachtrij

1. Open de service bus-naamruimte en klik vervolgens op **+ wachtrij**.
1. Voer een naam voor de wachtrij en klik vervolgens op **maken**.
1. Open de service bus-wachtrij en klik vervolgens op **gedeeld toegangsbeleid** > **+ toevoegen**.
1. Voer een naam voor het beleid, selectievakje **beheren**, en klik vervolgens op **maken**.

   ![Toevoegen van een service bus-wachtrij in de Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-rule-to-your-iot-hub"></a>Een eindpunt en een regel voor doorsturen toevoegen aan uw IoT-hub

### <a name="add-an-endpoint"></a>Een eindpunt toevoegen

1. Uw IoT-hub te openen, klikt u op eindpunten > + toevoegen.
1. Voer de volgende informatie in:

   **Naam**: de naam van het eindpunt.

   **Eindpunttype**: Selecteer **Service Bus-wachtrij**.

   **Service Bus-naamruimte**: Selecteer de naamruimte die u hebt gemaakt.

   **Service Bus-wachtrij**: Selecteer de wachtrij die u hebt gemaakt.
1. Klik op **OK**.

   ![Een eindpunt toevoegen aan uw IoT-hub in de Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a>Toevoegen van een regel voor doorsturen

1. Klik op in uw IoT-hub **Routes** > **+ toevoegen**.
1. Voer de volgende informatie in:

   **Naam**: de naam van de regel voor doorsturen.

   **Gegevensbron**: Selecteer **DeviceMessages**.

   **Eindpunt**: Selecteer het eindpunt dat u hebt gemaakt.

   **Querytekenreeks**: Voer `temperatureAlert = "true"`.
1. Klik op **Opslaan**.

   ![Toevoegen van een regel voor doorsturen in de Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a>Maken en configureren van een logische app

### <a name="create-a-logic-app"></a>Een logische app maken

1. In de [Azure-portal](https://portal.azure.com/), klikt u op **nieuw** > **Enterprise Integration** > **logische App**.
1. Voer de volgende informatie in:

   **Naam**: de naam van de logische app.

   **Resourcegroep**: gebruik dezelfde resourcegroep die gebruikmaakt van uw IoT-hub.

   **Locatie**: gebruik van dezelfde locatie die gebruikmaakt van uw IoT-hub.
1. Klik op **Create**.

### <a name="configure-the-logic-app"></a>De logische app configureren

1. Open de logische app dat wordt geopend in de ontwerpfunctie van Logic Apps.
1. Klik in de ontwerpfunctie voor Logic Apps **lege logische App**.

   ![Beginnen met een lege logische app in de Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

1. Klik op **Service Bus**.

   ![Selecteer de Service Bus om te beginnen met het maken van uw logische app in de Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. Klik op **Service Bus-bij een of meer in een wachtrij (automatisch aanvullen berichten)**.
1. Een service bus-verbinding maken.
   1. Voer een verbindingsnaam.
   1. Klik op de service bus-naamruimte > de service bus-beleid > **maken**.

      ![Een service bus-verbinding voor uw logische app maken in de Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   1. Klik op **doorgaan** nadat de service bus-verbinding is gemaakt.
   1. Selecteer de wachtrij die u hebt gemaakt en voer `175` voor **maximumaantal bericht**

      ![Geef het maximale aantal voor de service bus-verbinding in uw logische app](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)
   1. Klik op 'Opslaan' knop de wijzigingen wilt opslaan.

1. Maak een SMTP-service-verbinding.
   1. Klik op **nieuwe stap** > **een actie toevoegen**.
   1. Type `SMTP`, klikt u op de **SMTP** service in de zoekresultaten en klik vervolgens op **SMTP - e-mailbericht verzenden**.

      ![Een SMTP-verbinding maken in uw logische app in de Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   1. Voer de SMTP-gegevens van uw postvak in en klik vervolgens op **maken**.

      ![Voer SMTP-verbindingsgegevens in uw logische app in de Azure portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      De SMTP-informatie ophalen voor [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), en [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).
   1. Voer uw e-mailadres voor **van** en **naar**, en `High temperature detected` voor **onderwerp** en **hoofdtekst**.
   1. Klik op **Opslaan**.

De logische app is defect wanneer u deze opslaat.

## <a name="test-the-logic-app"></a>De logische app testen

1. Start de clienttoepassing die u implementeert op uw apparaat bij [ESP8266 verbinding maken met Azure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md).
1. Vergroot de temperatuur omgeving rond de SensorTag meer dan 30 C. Bijvoorbeeld, een kaars rond de SensorTag lichten.
1. U ontvangt een e-mailmelding verzonden door de logische app.

   > [!NOTE]
   > Uw e-provider moet mogelijk de identiteit van de afzender om ervoor te zorgen dat het is u die het e-mailbericht verzendt.

## <a name="next-steps"></a>Volgende stappen

U hebt gemaakt aan een logische app die verbinding maakt van uw IoT-hub en uw postvak voor het controleren van de temperatuur en meldingen.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
