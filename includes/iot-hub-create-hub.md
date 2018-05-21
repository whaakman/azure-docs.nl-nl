---
title: bestand opnemen
description: bestand opnemen
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 05/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 9a29406b92f7d2e2ce8171974efb5a264e112d1d
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
1. Meld u aan bij [Azure Portal][lnk-portal].
1. Selecteer **Een resource maken** > **Internet of Things** > **IoT Hub**.
   
    ![Schermopname van Azure portal navigatie naar IoT Hub][1]

1. Voer in het deelvenster **IoT Hub** deelvenster de volgende informatie in voor uw IoT-hub:

   * **Abonnement**: Kies het abonnement dat u gebruiken wilt voor het maken van deze iothub.

   * **Resourcegroep**: maak een resourcegroep voor het hosten van de IoT-hub of gebruik een bestaande. Zie voor meer informatie [resourcegroepen gebruiken voor het beheren van uw Azure-resources][lnk-resource-groups].

   * **Regio**: de dichtstbijzijnde locatie om u te selecteren.

   * **Naam**: verzin een naam voor uw IoT-hub. Als u de naam opgeven beschikbaar is, wordt er een groen vinkje weergegeven.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![IoT Hub basisbeginselen venster][2]

2. Selecteer **volgende: grootte en schaal** om door te gaan maken van uw IoT-hub. 

3. Kies uw **prijs-en schaalniveau**. Selecteer voor dit artikel de **F1 - vrije** servicetier als deze nog steeds beschikbaar is op uw abonnement. Zie [Prijs- en schaalniveau][lnk-pricing] voor meer informatie.

   ![De grootte en schaal venster IoT-Hub][3]

4. Selecteer **revisie + maken**.

1. Lees de informatie van uw IoT hub en klik vervolgens op **maken**. Het maken van de IoT-hub kan een paar minuten duren. U kunt de voortgang bewaken via het deelvenster **Meldingen**.
<!-- Images -->
[1]: ./media/iot-hub-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-create-hub/create-iot-hub3.png
<!-- Links -->
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md