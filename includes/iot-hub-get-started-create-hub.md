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
ms.openlocfilehash: d586ca18953b12045fbbaa4a656d78a7192eb88e
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34371232"
---
## <a name="create-an-iot-hub"></a>Een IoT Hub maken
Een IoT Hub maken waarmee uw gesimuleerde apparaat-app verbinding kan maken. De volgende stappen laten zien hoe u deze taak kunt uitvoeren met behulp van Azure Portal.

1. Meld u aan bij [Azure Portal][lnk-portal].

1. Selecteer **Een resource maken** > **Internet of Things** > **IoT Hub**.
   
    ![Snelbalk Azure Portal][1]

1. Voer in het deelvenster **IoT Hub** deelvenster de volgende informatie in voor uw IoT-hub:

   * **Abonnement**: kies het abonnement dat u wilt gebruiken om deze IoT-hub te maken.

   * **Resourcegroep**: maak een resourcegroep voor het hosten van de IoT-hub of gebruik een bestaande. Raadpleeg voor meer informatie [Resourcegroepen gebruiken om Azure-resources te beheren][lnk-resource-groups].

   * **Regio**: selecteer de locatie die het dichtst bij u in de buurt is.

   * **Naam**: verzin een naam voor uw IoT-hub. Als de door u opgegeven naam beschikbaar is, verschijnt er een groen vinkje.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![IoT Hub-basisinformatievenster][2]

2. Selecteer **Volgende: grootte en schaal** om verder te gaan met het maken van uw IoT-hub. 

3. Kies uw **prijs- en schaalcategorie**. Selecteer voor dit artikel de categorie **F1 - Gratis** als deze nog beschikbaar is voor uw abonnement. Zie [Prijs- en schaalniveau][lnk-pricing] voor meer informatie.

   ![Venster met grootte en schaal van IoT Hub][3]

4. Selecteer **Controleren + maken**.

1. Controleer de informatie van uw IoT-hub en klik vervolgens op **Maken**. Het maken van de IoT-hub kan een paar minuten duren. U kunt de voortgang bewaken via het deelvenster **Meldingen**.

1. Wanneer uw IoT-hub gereed is, klikt u op de tegel in Azure Portal om het eigenschappenvenster te openen. Nu u een IoT-hub hebt gemaakt, kunt u de belangrijke informatie zoeken die u gebruikt om apparaten en toepassingen te verbinden met uw IoT-hub. Klik op **Gedeeld toegangsbeleid**.
   
1. In **Gedeeld toegangsbeleid** selecteert u het beleid **iothubowner**. Kopieer de **Verbindingsreeks---primaire sleutel** van IoT Hub voor later gebruik. Zie voor meer informatie [Toegangsbeheer][lnk-access-control] in de Ontwikkelaarshandleiding voor IoT Hub.
   
    ![Gedeeld toegangsbeleid][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
