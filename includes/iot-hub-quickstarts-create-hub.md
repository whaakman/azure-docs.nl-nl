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
ms.openlocfilehash: 63acf0297a694ff442d56e67d52fd9b4e49f812d
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008583"
---
De eerste stap is Azure Portal te gebruiken om een IoT-hub te maken in uw abonnement. Met de IoT-hub kunt u grote hoeveelheden telemetrie naar de cloud opnemen vanaf een groot aantal apparaten. De hub stelt één of meerdere back-end diensten in de cloud in staat om die telemetrie te lezen en te verwerken.

1. Meld u aan bij [Azure Portal](http://portal.azure.com).

1. Selecteer **Een resource maken** > **Internet of Things** > **IoT Hub**.

    ![Selecteren om IoT Hub te installeren][1]

1. Voer in het deelvenster **IoT Hub** deelvenster de volgende informatie in voor uw IoT-hub:

   * **Abonnement**: kies het abonnement dat u wilt gebruiken om deze IoT-hub te maken.
   * **Resourcegroep**: maak een nieuwe resourcegroep voor de IoT-hub of gebruik een bestaande. Door alle gerelateerde resources samen in een groep te plaatsen, zoals **TestResources**, kunt u ze samen beheren. Als u bijvoorbeeld de resourcegroep verwijdert, worden alle resources verwijderd die deze groep bevat. Raadpleeg voor meer informatie [Resourcegroepen gebruiken om Azure-resources te beheren][lnk-resource-groups].
   * **Regio**: selecteer de locatie die het dichtst bij uw apparaten in de buurt is.
   * **Naam**: bedenk een unieke naam voor uw IoT-hub. Als de door u opgegeven naam beschikbaar is, verschijnt er een groen vinkje.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![IoT Hub-basisinformatievenster][2]

2. Selecteer **Volgende: grootte en schaal** om verder te gaan met het maken van uw IoT-hub. 

3. Kies uw **prijs- en schaalcategorie**. Selecteer voor dit artikel de categorie **F1 - Gratis** als deze nog beschikbaar is voor uw abonnement. Zie [Prijs- en schaalniveau][lnk-pricing] voor meer informatie.

   ![Venster met grootte en schaal van IoT Hub][3]

4. Selecteer **Controleren + maken**.

1. Controleer de informatie van uw IoT-hub en klik vervolgens op **Maken**. Het maken van de IoT-hub kan een paar minuten duren. U kunt de voortgang bewaken via het deelvenster **Meldingen**.


<!-- Images -->
[1]: ./media/iot-hub-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-create-hub/create-iot-hub3.png
<!-- Links -->
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md