---
title: Beheer van Azure IoT Hub cloud apparaat met iothub explorer messaging | Microsoft Docs
description: Informatie over het gebruik van de iothub-explorer CLI tool monitor apparaat cloud-berichten (D2C) en het verzenden van cloud naar apparaat (C2D)-berichten in Azure IoT Hub.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: iothub explorer cloud apparaat messaging, iot hub cloud naar apparaat, cloud device Messaging
ms.assetid: 04521658-35d3-4503-ae48-51d6ad3c62cc
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: xshi
ms.openlocfilehash: 30151b7bdc544bc36e959cc3528d37897198fc7e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-iothub-explorer-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Iothub-explorer gebruiken om te verzenden en ontvangen van berichten tussen het apparaat en IoT-Hub

![End-to-end-diagram](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[iothub explorer](https://github.com/azure/iothub-explorer) heeft een aantal opdrachten die IoT Hub beheer vergemakkelijkt. Deze zelfstudie is gericht op het iothub-explorer gebruiken om te verzenden en ontvangen van berichten tussen het apparaat en uw IoT-hub.

## <a name="what-you-will-learn"></a>Wat u leert

U informatie over het iothub-explorer gebruiken om te controleren van apparaat-naar-cloud-berichten en berichten van cloud naar apparaat verzenden. Apparaat-naar-cloud-berichten kunnen worden sensorgegevens die uw apparaat verzamelt en verzendt vervolgens naar uw IoT-hub. Cloud-naar-apparaat-berichten kunnen worden opdrachten die uw IoT-hub naar uw apparaat verzendt knipperen een LED die is verbonden met uw apparaat.

## <a name="what-you-will-do"></a>Wat u doet

- Gebruik iothub-explorer om apparaat-naar-cloud-berichten worden gecontroleerd.
- Gebruik de iothub-explorer cloud naar apparaat verzenden.

## <a name="what-you-need"></a>Wat u nodig hebt

- Zelfstudie [instellen van uw apparaat](iot-hub-raspberry-pi-kit-node-get-started.md) voltooid die wordt ingegaan op de volgende vereisten:
  - Een actief Azure-abonnement.
  - Een Azure-IoT-hub in uw abonnement.
  - Een clienttoepassing dat berichten naar uw Azure-IoT-hub verzendt.
- iothub-explorer. ([Iothub explorer installeren](https://github.com/azure/iothub-explorer))

## <a name="monitor-device-to-cloud-messages"></a>Apparaat-naar-cloud-berichten worden gecontroleerd

Volg deze stappen voor het bewaken van berichten die op uw apparaat worden verzonden naar uw IoT-hub:

1. Open een consolevenster.
1. Voer de volgende opdracht uit:

   ```bash
   iothub-explorer monitor-events <device-id> --login "<IoTHubConnectionString>"
   ```

   > [!Note]
   > Ophalen van `<device-id>` en `<IoTHubConnectionString>` uit uw IoT-hub. Zorg ervoor dat u de vorige zelfstudie hebt voltooid. Of u kunt proberen te gebruiken `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` hebt u `HostName`, `SharedAccessKeyName` en `SharedAccessKey`.

## <a name="send-cloud-to-device-messages"></a>Cloud-naar-apparaat-berichten verzenden

Als u wilt een bericht met uw IoT-hub verzendt naar uw apparaat, de volgende stappen uit:

1. Open een consolevenster.
1. Een sessie starten op uw IoT-hub met de volgende opdracht:

   ```bash
   iothub-explorer login `<IoTHubConnectionString>`
   ```

1. Een bericht verzenden naar uw apparaat met de volgende opdracht:

   ```bash
   iothub-explorer send <device-id> <message>
   ```

De opdracht knippert de LED die is verbonden met uw apparaat en verzendt het bericht naar uw apparaat.

> [!Note]
> Er is niet nodig voor het apparaat een afzonderlijke ack-opdracht terug naar uw IoT-hub bij ontvangst van het bericht te verzenden.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u apparaat-naar-cloud-berichten worden gecontroleerd en cloud-naar-apparaat-berichten verzenden tussen uw IoT-apparaat en de Azure IoT Hub.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
