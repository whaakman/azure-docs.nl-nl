---
title: bestand opnemen
description: bestand opnemen
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 02/26/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 8d8f8021925ac9c9e427dd6571ecaa1a30c85497
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670979"
---
<!-- This is the instructions for creating a simulated device you can use for testing routing.-->

Maak vervolgens een apparaat-id en sla de bijbehorende sleutel op voor later gebruik. Deze apparaat-id wordt gebruikt door de simulatietoepassing om berichten naar de IoT Hub te verzenden. Deze mogelijkheid is niet beschikbaar zijn in PowerShell of bij het gebruik van een Azure Resource Manager-sjabloon. De volgende stappen uitgelegd hoe u het maken van het gesimuleerde apparaat met de [Azure-portal](https://portal.azure.com).

1. Open [Azure Portal](https://portal.azure.com) en meld u aan bij uw Azure-account.

2. Selecteer **resourcegroepen** en kies vervolgens de resourcegroep. In deze zelfstudie wordt gebruikgemaakt van **ContosoResources**.

3. Selecteer uw IoT-hub in de lijst met resources. In deze zelfstudie wordt gebruikgemaakt van **ContosoTestHub**. Selecteer **IoT-apparaten** in het deelvenster Hub.

4. Selecteer **+ Toevoegen**. Vul in het deelvenster Apparaat toevoegen de apparaat-id in. In deze zelfstudie wordt gebruikgemaakt van **Contoso-testapparaat**. Laat het item voor sleutels leeg en schakel het selectievakje **Automatisch sleutels genereren** in. Zorg ervoor dat **Apparaat verbinden met IoT Hub** is ingeschakeld. Selecteer **Opslaan**.

   ![Het scherm toevoegen-apparaat](./media/iot-hub-include-create-simulated-device-portal/add-device.png)

5. Nu dat deze is gemaakt, selecteert u het apparaat om te zien van de gegenereerde sleutels. Selecteer het pictogram voor kopiëren op de primaire sleutel en sla het op een locatie zoals Kladblok voor de testfase van deze zelfstudie.

   ![De details van het apparaat, met inbegrip van de sleutels](./media/iot-hub-include-create-simulated-device-portal/device-details.png)