---
title: bestand opnemen
description: bestand opnemen
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 08/14/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 983c65ba6e8b87f1dd66fcfdb50eac088ffab5d0
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49960310"
---
Een van de belangrijkste mogelijkheden van Azure IoT Edge is dat u er modules voor uw IoT Edge-apparaten mee kunt implementeren vanuit de cloud. Een IoT Edge-module is een uitvoerbaar pakket dat is geïmplementeerd als container. In deze sectie implementeert u een module die telemetrie genereert voor uw gesimuleerde apparaat.

1. Ga in Azure Portal naar uw IoT-hub.
1. Ga onder **Automatisch apparaatbeheer** naar **IoT Edge** en selecteer uw IoT Edge-apparaat.
1. Selecteer **Modules instellen**.
1. Klik in de sectie **Implementatiemodules** van de stap **Modules toevoegen** op **Toevoegen** en selecteer **IoT Edge-module**.
1. Voer in het veld **Naam** `tempSensor` in.
1. Voer in het veld **URI installatiekopie** `mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0` in.
1. Laat de overige instellingen ongewijzigd en selecteer **Opslaan**.

   ![Sla de IoT Edge-module op nadat u de URI van de naam en installatiekopie het ingevoerd](./media/iot-edge-deploy-module/name-image.png)

1. Terug in de stap **Modules toevoegen** selecteert u **Volgende**.
1. In de stap **Routes opgeven** moet u beschikken over een standaardroute waarmee alle berichten van alle modules naar IoT Hub worden verzonden. Voeg anders de volgende code toe en selecteer **Volgende**.

   ```json
   {
       "routes": {
           "route": "FROM /messages/* INTO $upstream"
       }
   }
   ```

1. Selecteer in de stap **Implementatie beoordelen** de optie **Verzenden**.
1. Ga terug naar de detailpagina van het apparaat en selecteer **Vernieuwen**. Naast de module edgeAgent, die u hebt gemaakt toen u de service voor het eerst startte, ziet u een andere runtimemodule met de naam **edgeHub** en de module **tempSensor** in de lijst.

   ![Geef tempSensor weer in een lijst met geïmplementeerde modules](./media/iot-edge-deploy-module/deployed-modules.png)
