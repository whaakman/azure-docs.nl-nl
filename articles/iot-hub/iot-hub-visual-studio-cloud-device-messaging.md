---
title: Berichten met Cloud Explorer voor Visual Studio Azure IoT Hub op cloudapparaten beheren | Microsoft Docs
description: Leer hoe u Cloud Explorer voor Visual Studio gebruiken om te controleren van apparaat naar cloud-berichten en cloud naar apparaat-berichten in Azure IoT Hub verzenden.
author: shizn
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/07/2018
ms.author: xshi
ms.openlocfilehash: bc955db4e851c1f59012dc1375170850e1b0604c
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54108396"
---
# <a name="use-cloud-explorer-for-visual-studio-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Cloud Explorer voor Visual Studio gebruiken om te verzenden en ontvangen van berichten tussen uw apparaat en IoT-Hub

![Diagram voor end-to-end](media/iot-hub-get-started-e2e-diagram/2.png)

[Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) is een nuttig Visual Studio-uitbreiding waarmee u uw Azure-resources bekijken, hun eigenschappen inspecteren en voert u belangrijke ontwikkelaar acties op basis van Visual Studio. In dit artikel richt zich op het Cloud Explorer gebruiken om te verzenden en ontvangen van berichten tussen uw apparaat en uw IoT-Hub.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="what-you-will-learn"></a>U leert

U leert hoe u Cloud Explorer voor Visual Studio gebruiken voor het bewaken van apparaat-naar-cloud-berichten en het verzenden van berichten van cloud-naar-apparaat. Apparaat-naar-cloud-berichten mogelijk sensorgegevens die uw apparaat verzamelt en verzendt vervolgens naar uw IoT-Hub. Cloud-naar-apparaat-berichten kunnen worden opdrachten die uw IoT-Hub worden verzonden naar uw apparaat. Bijvoorbeeld, knipperen een LED die is verbonden met uw apparaat.

## <a name="what-you-will-do"></a>Wat u doet

- Cloud Explorer voor Visual Studio gebruiken voor het bewaken van apparaat-naar-cloud-berichten.
- Cloud Explorer voor Visual Studio gebruiken om cloud-naar-apparaat-berichten te verzenden.

## <a name="what-you-need"></a>Wat u nodig hebt

- Een actief Azure-abonnement.
- Een Azure-IoT-Hub in uw abonnement.
- Microsoft Visual Studio 2017 Update 8 of hoger
- Onderdeel van de cloud Explorer van Visual Studio Installer (standaard met de Azure Workload geselecteerd)

## <a name="update-cloud-explorer-to-latest-version"></a>Cloud Explorer naar de nieuwste versie bijwerken

Het onderdeel Cloud Explorer van Visual Studio Installer biedt alleen ondersteuning voor het bewaken van apparaat-naar-cloud en cloud-naar-apparaat-berichten. Om te kunnen berichten verzenden naar het apparaat of in de cloud, downloadt en installeert u de meest recente [Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS).

## <a name="sign-in-to-access-your-iot-hub"></a>Meld u aan voor toegang tot uw IoT-Hub

1. In Visual Studio **Cloud Explorer** venster, klikt u op het pictogram accountbeheer. U kunt de Cloud Explorer-venster openen **weergave** > **Cloud Explorer** menu.

    ![Klik op Account beheren](media/iot-hub-visual-studio-cloud-device-messaging/click-account-management.png)

1. Klik op **Accounts beheren** in Cloud Explorer.
1. Klik op **een account toevoegen...**  in het nieuwe venster voor de eerste keer aanmelden bij Azure.
1. Nadat u zich hebt aangemeld, wordt de lijst met uw Azure-abonnement worden weergegeven. Selecteer het Azure-abonnementen weergeven en klik op het gewenste **toepassen**.
1. Vouw **uw abonnement** > **IoT-Hubs** > **uw IoT-Hub**, de lijst met apparaten worden weergegeven onder het knooppunt van uw IoT-Hub.

    ![Apparatenlijst](media/iot-hub-visual-studio-cloud-device-messaging/device-list.png)

## <a name="monitor-device-to-cloud-messages"></a>Bewaken van apparaat-naar-cloud-berichten

Volg deze stappen voor het bewaken van berichten die van uw apparaat worden verzonden naar uw IoT-Hub:

1. Met de rechtermuisknop op uw IoT-Hub of een apparaat en selecteer **Start Monitoring D2C Message**.

    ![Start Monitoring D2C Message](media/iot-hub-visual-studio-cloud-device-messaging/start-monitoring-d2c-message.png)

1. De bewaakte berichten worden weergegeven de **IoT-Hub** deelvenster Uitvoer.

    ![Monitoring D2C Message resultaat](media/iot-hub-visual-studio-cloud-device-messaging/monitor-d2c-message-result.png)

1. Om te stoppen bewaking, met de rechtermuisknop op elk IoT-Hub of het apparaat en selecteer **Stop Monitoring D2C Message**.

## <a name="send-cloud-to-device-messages"></a>Cloud-naar-apparaat-berichten verzenden

Berichten van uw IoT-Hub worden verzonden naar uw apparaat, de volgende stappen uit:

1. Met de rechtermuisknop op uw apparaat en selecteer **C2D-bericht verzenden**.

    ![C2D-bericht verzenden](media/iot-hub-visual-studio-cloud-device-messaging/send-c2d-message.png)

1. Geef het bericht in het invoervak.
1. Resultaten worden weergegeven de **IoT-Hub** deelvenster Uitvoer.

    ![Resultaat van C2D-bericht verzenden](media/iot-hub-visual-studio-cloud-device-messaging/send-c2d-message-result.png)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u bewaken van apparaat-naar-cloud-berichten en cloud-naar-apparaat-berichten tussen uw IoT-apparaat en Azure IoT Hub verzendt.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]