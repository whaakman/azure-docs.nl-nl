---
title: bestand opnemen
description: bestand opnemen
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 12/7/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 1a750a97cdc940c0f0a3d7e33d6be0d33f811425
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53108019"
---
Een van de belangrijkste mogelijkheden van Azure IoT Edge is dat u er modules voor uw IoT Edge-apparaten mee kunt implementeren vanuit de cloud. Een IoT Edge-module is een uitvoerbaar pakket dat is geïmplementeerd als container. In deze sectie wordt een vooraf samengestelde module geïmplementeerd vanuit de [IoT Edge-Modules-sectie van de Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules). Deze module genereert telemetrie voor uw gesimuleerde apparaat.

1. Voer in Azure Portal `Simulated Temperature Sensor` in het zoekvak in en open het resultaat van Marketplace.

   ![Gesimuleerde temperatuursensor in Azure Portal zoeken](./media/iot-edge-deploy-module/search-for-temperature-sensor.png)

2. In het veld **Abonnement** selecteert u het abonnement met de IoT Hub die u gebruikt, als dit nog niet is geselecteerd.

3. In het veld **IoT Hub** selecteert u de naam van de IoT Hub die u gebruikt, als deze nog niet is geselecteerd.

4. Klik op **Apparaat vinden**, selecteer uw IoT Edge-apparaat (met de naam `myEdgeDevice`) en selecteer vervolgens **Maken**.

5. In de stap **Modules toevoegen** van de wizard, klikt u op de module **SimulatedTemperatureSensor** om de configuratie-instellingen te controleren, klikt u op **Opslaan** en selecteert u **Volgende**.

6. In de stap **Routes opgeven** van de wizard controleert u of de routes correct zijn ingesteld met de standaardroute waarmee alle berichten van alle modules worden verzonden naar IoT Hub (`$upstream`). Voeg anders de volgende code toe en selecteer **Volgende**.

   ```json
    {
    "routes": {
        "route": "FROM /messages/* INTO $upstream"
        }
    }
   ```

7. Selecteer in de stap **Implementatie beoordelen** van de wizard de optie **Verzenden**.

8. Ga terug naar de detailpagina van het apparaat en selecteer **Vernieuwen**. Naast de module edgeAgent, die u hebt gemaakt toen u de service voor het eerst startte, worden ook een andere runtimemodule met de naam **edgeHub** en de module **SimulatedTemperatureSensor** in de lijst weergegeven.

   Het kan enkele minuten duren voordat de nieuwe modules worden weergegeven. Op het IoT Edge-apparaat moeten de nieuw implementatiegegevens worden opgehaald uit de cloud, de containers worden gestart, en de nieuwe status weer worden gemeld bij IoT Hub. 

   ![Weergave SimulatedTemperatureSensor in de lijst met geïmplementeerde modules](./media/iot-edge-deploy-module/deployed-modules-marketplace-temp.png)
