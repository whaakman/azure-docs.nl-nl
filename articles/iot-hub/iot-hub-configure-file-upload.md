---
title: De Azure portal gebruiken voor het uploaden van bestanden configureren | Microsoft Docs
description: Het gebruik van de Azure-portal naar uw IoT-hub zodat het uploaden van bestanden van verbonden apparaten configureren. Bevat informatie over het configureren van de bestemming Azure storage-account.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/03/2017
ms.author: robin.shahan
ms.openlocfilehash: 04f3f05c8c18a3356c282144cda62d1f980362fe
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57008438"
---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a>IoT Hub-bestand wordt geüpload met behulp van de Azure-portal configureren

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a>Bestand uploaden

Gebruik de [bestand functionaliteit voor het uploaden van IoT-Hub](iot-hub-devguide-file-upload.md), moet u eerst een Azure Storage-account koppelen met de hub. Selecteer **uploaden van het bestand** om weer te geven van een lijst met eigenschappen voor het uploaden van bestand voor de IoT-hub die wordt gewijzigd.

![IoT Hub-bestand voor de weergave-instellingen in de portal uploaden](./media/iot-hub-configure-file-upload/file-upload-settings.png)

* **Storage-container**: De Azure portal gebruiken voor het selecteren van een blob-container in een Azure Storage-account in uw huidige Azure-abonnement koppelen aan uw IoT-Hub. Indien nodig, kunt u een Azure Storage-account op de **opslagaccounts** blade en blob-container op de **Containers** blade. IoT Hub wordt automatisch gegenereerd SAS URI's met schrijfmachtigingen voor deze blob-container voor apparaten moet worden gebruikt bij het uploaden van bestanden.

   ![Storage-containers voor het uploaden van bestand weergeven in de portal](./media/iot-hub-configure-file-upload/file-upload-container-selection.png)

* **Meldingen over geüploade bestanden ontvangen**: In- of uitschakelen van bestand uploaden meldingen via de in-/ uitschakelen.

* **SAS TTL**: Deze instelling is de time-to-live van de SAS-URI's op het apparaat wordt geretourneerd door de IoT Hub. Standaard ingesteld op één uur, maar kan worden aangepast aan andere waarden met behulp van de schuifregelaar.

* **Melding instellingen standaard TTL-bestand**: De time-to-live van een bestand uploaden van melding voordat deze is verlopen. Standaard ingesteld op één dag, maar kan worden aangepast aan andere waarden met behulp van de schuifregelaar.

* **Maximumaantal leveringen voor melding bestand**: Het aantal keren dat die de IoT-Hub wil ervoor zorgen dat een bestand uploaden een melding. Standaard ingesteld op 10, maar kan worden aangepast aan andere waarden met behulp van de schuifregelaar.

   ![Uploaden van de IoT Hub-bestanden in de portal configureren](./media/iot-hub-configure-file-upload/file-upload-selected-container.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de functies voor het uploaden van bestanden van IoT-Hub, [uploaden van bestanden vanaf een apparaat](iot-hub-devguide-file-upload.md) in de Ontwikkelaarshandleiding voor IoT Hub.

Volg deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

* [IoT-apparaten bulksgewijs beheren](iot-hub-bulk-identity-mgmt.md)
* [Metrische gegevens van IoT Hub](iot-hub-metrics.md)
* [Controle van bewerkingen](iot-hub-operations-monitoring.md)

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md)
* [AI implementeren op edge-apparaten met Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
* [Beveiligen van uw IoT-oplossing vanaf het begin van](../iot-fundamentals/iot-security-ground-up.md)
