---
title: Realtime gegevensvisualisatie van sensorgegevens uit Azure IoT hub-Web-Apps | Microsoft Docs
description: Gebruik de functie Web Apps van Microsoft Azure App Service om temperatuur en vochtigheid gegevens die worden verzameld van de sensor en verzonden naar uw Iot-hub te visualiseren.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: robinsh
ms.openlocfilehash: 070f37a969411cfc4caf5f2d2b089ccfae759ca2
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59683190"
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-the-web-apps-feature-of-azure-app-service"></a>Het visualiseren van realtime-sensorgegevens uit Azure IoT hub met behulp van de functie Web Apps van Azure App Service

![Diagram voor end-to-end](./media/iot-hub-live-data-visualization-in-web-apps/1_iot-hub-end-to-end-diagram.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Wat u leert

In deze zelfstudie leert u hoe u voor het visualiseren van realtime-sensorgegevens die uw IoT-hub ontvangt door het uitvoeren van een webtoepassing die wordt gehost op een web-app. Als u wilt proberen om te visualiseren van de gegevens in uw IoT-hub met behulp van Power BI, Zie [gebruik Power BI voor het visualiseren van realtime-sensorgegevens uit Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).

## <a name="what-you-do"></a>Wat u allemaal doen

* Een web-app maken in Azure portal.
* Bereid u voor uw IoT-hub voor toegang tot gegevens door toe te voegen een consumergroep.
* De web-app om te lezen van sensorgegevens uit IoT hub configureren.
* Een web-App moet worden gehost door de web-app uploaden.
* Open de web-app om te zien van realtime temperatuur en vochtigheid gegevens van uw IoT-hub.

## <a name="what-you-need"></a>Wat u nodig hebt

* Voltooi de [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md) zelfstudie of een van de apparaat-zelfstudies, bijvoorbeeld [Raspberry Pi met node.js](iot-hub-raspberry-pi-kit-node-get-started.md). Deze betrekking hebben op de volgende vereisten:

  * Een actief Azure-abonnement
  * Een Iot-hub in uw abonnement
  * Een clienttoepassing waarmee berichten worden verzonden naar uw Iot hub

* [Git downloaden](https://www.git-scm.com/downloads)

## <a name="create-a-web-app"></a>Een webtoepassing maken

1. In de [Azure-portal](https://portal.azure.com/), klikt u op **een resource maken** > **Web en mobiel** > **Web-App**.

2. Voer een unieke Taaknaam, Controleer of het abonnement, geeft u een resourcegroep en een locatie op, selecteer **vastmaken aan dashboard**, en klik vervolgens op **maken**.

   Het is raadzaam dat u de dezelfde locatie als uw resourcegroep selecteren. 
   
   ![Een webtoepassing maken](./media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-the-web-app-to-read-data-from-your-iot-hub"></a>De web-app voor het lezen van gegevens van uw IoT-hub configureren

1. Open de web-app die u zojuist hebt ingericht.

2. Klik op **toepassingsinstellingen**, en klik vervolgens onder **App-instellingen**, voeg de volgende sleutel/waarde-paren toe:

   | Sleutel                                   | Value                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | Azure.IoT.IoTHub.ConnectionString     | Verkregen van Azure CLI                                      |
   | Azure.IoT.IoTHub.ConsumerGroup        | De naam van de consumergroep die u aan uw IoT-hub toevoegt  |
   | WEBSITE_NODE_DEFAULT_VERSION          | 8.9.4                                                        |

   ![Instellingen toevoegen aan uw web-app met sleutel/waarde-paren](./media/iot-hub-live-data-visualization-in-web-apps/3_web-app-settings-key-value-azure.png)

3. Klik op **toepassingsinstellingen**onder **algemene instellingen**, in-/ uitschakelen de **Web sockets** optie en klik vervolgens op **opslaan**.

   ![Stel de Web sockets-optie](./media/iot-hub-live-data-visualization-in-web-apps/4_toggle_web_sockets.png)

## <a name="upload-a-web-application-to-be-hosted-by-the-web-app"></a>Een web-App moet worden gehost door de web-app uploaden

Op GitHub, hebben we aangebracht beschikbaar een webtoepassing die wordt weergegeven van realtime-sensorgegevens uit IoT hub. U hoeft alleen is de web-app voor het werken met een Git-opslagplaats, de web-App downloaden uit GitHub en vervolgens te uploaden naar Azure voor de web-app naar host configureren.

1. Klik in de web-app op **implementatieopties** > **bron kiezen** > **lokale Git-opslagplaats**, en klik vervolgens op **OK**.

   ![Configureren van de implementatie van uw web-app voor het gebruik van de lokale Git-opslagplaats](./media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. Klik op **Implementatiereferenties**, maak een gebruikersnaam en wachtwoord moet worden gebruikt verbinding maken met de Git-opslagplaats in Azure, en klik vervolgens op **opslaan**.

3. Klik op **overzicht**, en noteer de waarde van **Git kloon-url**.

   ![Ophalen van de Git-kloon-URL van uw web-app](./media/iot-hub-live-data-visualization-in-web-apps/6_web-app-git-clone-url-azure.png)

4. Open een opdracht of een terminalvenster op uw lokale computer.

5. De web-app downloaden uit GitHub en dit uploaden naar Azure voor de web-app op de host. Voer de volgende opdrachten uit om dit te doen:

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > \<GIT-kloon-URL\> is de URL van de Git-opslagplaats gevonden op de **overzicht** pagina van de web-app.

## <a name="open-the-web-app-to-see-real-time-temperature-and-humidity-data-from-your-iot-hub"></a>Open de web-app om te zien van realtime temperatuur en vochtigheid gegevens van uw IoT-hub

Op de **overzicht** pagina van uw web-app, klikt u op de URL voor het openen van de web-app.

![De URL van uw web-app ophalen](./media/iot-hub-live-data-visualization-in-web-apps/7_web-app-url-azure.png)

U ziet de realtime temperatuur en vochtigheid gegevens uit uw IoT-hub.

![Web-app-pagina met realtime temperatuur en vochtigheid](./media/iot-hub-live-data-visualization-in-web-apps/8_web-app-page-show-real-time-temperature-humidity-azure.png)

> [!NOTE]
> Controleer of dat de voorbeeldtoepassing wordt uitgevoerd op uw apparaat. Als u niet het geval is, ontvangt u een lege grafiek, kunt u verwijzen naar de zelfstudies onder [instellen van uw apparaat](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="next-steps"></a>Volgende stappen

U hebt uw web-app is gebruikt voor het visualiseren van realtime-sensorgegevens uit uw IoT-hub.

Zie voor een alternatieve manier om gegevens van Azure IoT Hub te visualiseren, [gebruik Power BI voor het visualiseren van realtime-sensorgegevens uit IoT hub](iot-hub-live-data-visualization-in-power-bi.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]