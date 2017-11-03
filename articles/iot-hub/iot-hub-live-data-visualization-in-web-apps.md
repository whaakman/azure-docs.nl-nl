---
title: "Realtime gegevensvisualisatie van sensorgegevens uit uw Azure-IoT-hub – Web-Apps | Microsoft Docs"
description: Gebruik de functie Web Apps van Microsoft Azure App Service voor het visualiseren van temperatuur en vochtigheid gegevens die worden verzameld van de sensor en verzonden naar uw Iot-hub.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: realtime gegevensvisualisatie, live gegevensvisualisatie, sensor gegevensvisualisatie
ms.assetid: e42b07a8-ddd4-476e-9bfb-903d6b033e91
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: e037f5c29cabf8e5d0d3e7ded187280a0652d5c3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-the-web-apps-feature-of-azure-app-service"></a>Realtime-sensorgegevens uit uw Azure-IoT-hub visualiseren met behulp van de functie Web Apps van Azure App Service

![End-to-end-diagram](media/iot-hub-get-started-e2e-diagram/5.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Wat u leert

In deze zelfstudie leert u hoe voor het visualiseren van realtime-sensorgegevens die uw IoT-hub ontvangt door het uitvoeren van een webtoepassing die wordt gehost op een web-app. Als u wilt proberen de gegevens in uw IoT-hub visualiseren met behulp van Power BI, Zie [gebruik Power BI voor het visualiseren van realtime-sensorgegevens uit Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).

## <a name="what-you-do"></a>Wat u doet

- Een web-app maken in de Azure-portal.
- Bereid uw IoT-hub voor toegang tot gegevens door een consumergroep toe te voegen.
- Configureer de web-app voor het lezen van sensorgegevens uit uw IoT-hub.
- Upload een webtoepassing worden gehost door de web-app.
- Open de web-app om realtime temperatuur en vochtigheid gegevens uit uw IoT-hub te bekijken.

## <a name="what-you-need"></a>Wat u nodig hebt

- [Instellen van uw apparaat](iot-hub-raspberry-pi-kit-node-get-started.md), die wordt ingegaan op de volgende vereisten:
  - Een actief Azure-abonnement
  - Een Iot-hub in uw abonnement
  - Een clienttoepassing die berichten naar uw Iot-hub verzendt
- [Git downloaden](https://www.git-scm.com/downloads)

## <a name="create-a-web-app"></a>Een webtoepassing maken

1. In de [Azure-portal](https://ms.portal.azure.com/), klikt u op **nieuw** > **Web en mobiel** > **Web-App**.
2. Voer een unieke naam, Controleer of het abonnement, geeft u een resourcegroep en een locatie, selecteer **vastmaken aan dashboard**, en klik vervolgens op **maken**.

   Het is raadzaam dat u dezelfde locatie als die van de resourcegroep selecteren. In dat geval helpt bij het verwerken van de snelheid en beperkt de kosten van de gegevensoverdracht.

   ![Een webtoepassing maken](media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-the-web-app-to-read-data-from-your-iot-hub"></a>De web-app voor het lezen van gegevens uit uw IoT-hub configureren

1. Open de web-app die u zojuist hebt ingericht.
2. Klik op **toepassingsinstellingen**, en klik vervolgens onder **appinstellingen**, voeg de volgende sleutel-waardeparen:

   | Sleutel                                   | Waarde                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | Azure.IoT.IoTHub.ConnectionString     | Verkregen van iothub explorer                                |
   | Azure.IoT.IoTHub.ConsumerGroup        | De naam van de consumergroep die u aan uw IoT-hub toevoegt  |

   ![Instellingen toevoegen aan uw web-app met sleutel-waardeparen](media/iot-hub-live-data-visualization-in-web-apps/4_web-app-settings-key-value-azure.png)

3. Klik op **toepassingsinstellingen**onder **algemene instellingen**, schakelen tussen de **Web-sockets** optie en klik vervolgens op **opslaan**.

   ![Stel de optie Web-sockets](media/iot-hub-live-data-visualization-in-web-apps/10_toggle_web_sockets.png)

## <a name="upload-a-web-application-to-be-hosted-by-the-web-app"></a>Een webtoepassing worden gehost door de web-app uploaden

Op GitHub, hebben we aangebracht beschikbaar een webtoepassing die wordt weergegeven van realtime-sensorgegevens uit uw IoT-hub. U hoeft alleen is de web-app voor het werken met een Git-opslagplaats, de webtoepassing vanuit GitHub downloaden en vervolgens uploaden naar Azure voor de web-app naar host configureren.

1. Klik in de web-app op **implementatieopties** > **bron kiezen** > **lokale Git-opslagplaats**, en klik vervolgens op **OK**.

   ![Configureren van uw web-app-implementatie voor het gebruik van de lokale Git-opslagplaats](media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. Klik op **Implementatiereferenties**, maak een gebruikersnaam en wachtwoord om verbinding maken met de Git-opslagplaats in Azure, en klik vervolgens op **opslaan**.

3. Klik op **overzicht**, en noteer de waarde van **Git kloon-url**.

   ![Ophalen van de Git-kloon-URL van uw web-app](media/iot-hub-live-data-visualization-in-web-apps/7_web-app-git-clone-url-azure.png)

4. Open een opdracht of een terminalvenster op de lokale computer.

5. De web-app vanuit GitHub downloaden en dit uploaden naar Azure voor de web-app naar host. Voer de volgende opdrachten om dit te doen:

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > \<GIT kloon-URL\> is de URL van de Git-opslagplaats gevonden op de **overzicht** pagina van de web-app.

## <a name="open-the-web-app-to-see-real-time-temperature-and-humidity-data-from-your-iot-hub"></a>Open de web-app om realtime temperatuur en vochtigheid gegevens uit uw IoT-hub te bekijken

Op de **overzicht** pagina van uw web-app klikt u op de URL voor het openen van de web-app.

![Ophalen van de URL van uw web-app](media/iot-hub-live-data-visualization-in-web-apps/8_web-app-url-azure.png)

U ziet de real-time temperatuur en vochtigheid gegevens uit uw IoT-hub.

![Web-app-pagina met realtime temperatuur en vochtigheid](media/iot-hub-live-data-visualization-in-web-apps/9_web-app-page-show-real-time-temperature-humidity-azure.png)

> [!NOTE]
> Controleer dat de voorbeeldtoepassing op uw apparaat wordt uitgevoerd. Als u niet het geval is, ontvangt u een lege grafiek, raadpleegt u de zelfstudies onder [instellen van uw apparaat](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="next-steps"></a>Volgende stappen
U hebt uw web-app is gebruikt voor het visualiseren van realtime-sensorgegevens uit uw IoT-hub.

Zie voor een andere manier om gegevens uit Azure IoT Hub te visualiseren [gebruik Power BI voor het visualiseren van realtime-sensorgegevens uit uw IoT-hub](iot-hub-live-data-visualization-in-power-bi.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
