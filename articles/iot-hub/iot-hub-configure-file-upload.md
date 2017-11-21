---
title: De Azure portal gebruiken om te uploaden bestand configureren | Microsoft Docs
description: Het gebruik van de Azure-portal voor het configureren van uw IoT-hub zodat bestandsuploads van verbonden apparaten. Bevat informatie over het configureren van de doel-Azure storage-account.
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 915f1597-272d-4fd4-8c5b-a0ccb1df0d91
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: 2d875947297be5d47362369b96bc6ab0d90b3c93
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a>IoT Hub uploaden van bestanden met de Azure portal configureren

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a>Bestand uploaden

Gebruik de [bestand uploaden functionaliteit in IoT-Hub][lnk-upload], moet u eerst een Azure Storage-account koppelen met de hub. Selecteer **uploaden bestand** om weer te geven een lijst met eigenschappen van bestand laden voor de IoT-hub die wordt gewijzigd.

![IoT Hub-bestand voor de weergave instellingen in de portal uploaden][13]

**Storage-container**: de Azure portal gebruiken om te selecteren van een blob-container in een Azure Storage-account in uw huidige Azure-abonnement wilt koppelen aan uw IoT-Hub. Indien nodig, kunt u een Azure Storage-account op de **opslagaccounts** blade en blob-container op de **Containers** blade. IoT Hub genereert automatisch SAS URI's met schrijfmachtigingen in voor deze blob-container voor apparaten voor gebruik bij het uploaden van bestanden.

![Storage-containers voor uploaden van het bestand weergeven in de portal][14]

**Meldingen ontvangen voor de geüploade bestanden**: bestand uploaden meldingen via de wisselknop- of uitschakelen.

**SAS TTL**: deze instelling wordt de time-to-live van de SAS-URI's die aan het apparaat wordt geretourneerd door de IoT Hub. Standaard ingesteld op één uur, maar kan worden aangepast aan andere waarden met behulp van de schuifregelaar.

**Bestand notification instellingen standaard TTL**: de time-to-live van een bestand uploaden melding voordat het is verlopen. Standaard ingesteld op één dag, maar kan worden aangepast aan andere waarden met behulp van de schuifregelaar.

**Melding levering van het maximum aantal bestanden**: het aantal keren dat de IoT-Hub probeert een bestand uploaden een melding. Standaard ingesteld op 10, maar kan worden aangepast aan andere waarden met behulp van de schuifregelaar.

![Het uploaden van het IoT-Hub in de portal configureren][15]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de functies voor het uploaden van bestanden van IoT Hub [uploaden van bestanden van een apparaat] [ lnk-upload] in de IoT Hub developer guide.

Volg deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

* [Bulksgewijs IoT-apparaten beheren][lnk-bulk]
* [IoT Hub metrische gegevens][lnk-metrics]
* [Bewerkingen controleren][lnk-monitor]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]
* [AI implementeren op de edge-apparaten met Azure IoT rand][lnk-iotedge]
* [Beveiligen van uw IoT-oplossing bouwen up][lnk-securing]

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-securing]: iot-hub-security-ground-up.md
