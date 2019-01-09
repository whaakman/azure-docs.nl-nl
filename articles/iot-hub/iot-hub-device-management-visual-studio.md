---
title: Azure IoT-Apparaatbeheer met Cloud Explorer voor Visual Studio | Microsoft Docs
description: De Cloud Explorer voor Visual Studio gebruiken voor Azure IoT Hub-Apparaatbeheer, met daarin de directe methoden en beheeropties van de Apparaatdubbel-gewenste eigenschappen.
author: shizn
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: xshi
ms.openlocfilehash: f0cb740f04970aeed4342511a046862f05aaa736
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54108200"
---
# <a name="use-cloud-explorer-for-visual-studio-for-azure-iot-hub-device-management"></a>Cloud Explorer voor Visual Studio gebruiken voor het beheer van Azure IoT Hub-apparaten

![Diagram voor end-to-end](media/iot-hub-get-started-e2e-diagram/2.png)

[Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) is een nuttig Visual Studio-uitbreiding waarmee u uw Azure-resources bekijken, hun eigenschappen inspecteren en voert u belangrijke ontwikkelaar acties op basis van Visual Studio. Het wordt geleverd met opties die u gebruiken kunt diverse taken uit te voeren.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Management-optie          | Taak                    |
|----------------------------|--------------------------------|
| Directe methoden             | Een apparaat fungeren, zoals starten of stoppen van berichten verzenden of opnieuw opstarten van het apparaat maken.                                        |
| Apparaatdubbel lezen           | Ophalen van de gerapporteerde status van een apparaat. Bijvoorbeeld, meldt het apparaat dat de LED nu knippert.                                    |
| Dubbele apparaat bijwerken         | Een apparaat in een bepaalde status, zoals een LED groen instellen of het instellen van het interval voor het verzenden van telemetrie tot 30 minuten plaatsen.         |
| Cloud-naar-apparaat-berichten   | Meldingen verzenden naar een apparaat. Bijvoorbeeld, "het is zeer waarschijnlijk regen vandaag nog. Vergeet niet om een overkoepelende."              |

Zie voor meer gedetailleerde uitleg over de verschillen en richtlijnen over het gebruik van deze opties, [richtlijnen voor communicatie van apparaat-naar-cloud](iot-hub-devguide-d2c-guidance.md) en [Cloud-naar-apparaat communicatie richtlijnen](iot-hub-devguide-c2d-guidance.md).

Apparaatdubbels zijn JSON-documenten waarin statusinformatie van een apparaat (metagegevens, configuraties en voorwaarden) zijn opgeslagen. IoT Hub is een apparaatdubbel voor elk apparaat dat is verbonden met het. Zie voor meer informatie over apparaatdubbels [aan de slag met apparaatdubbels](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Wat u leert

Leert u met Cloud Explorer voor Visual Studio met verschillende beheeropties voor op uw ontwikkelcomputer.

## <a name="what-you-do"></a>Wat u allemaal doen

Voer Cloud Explorer voor Visual Studio met verschillende opties.

## <a name="what-you-need"></a>Wat u nodig hebt

- Een actief Azure-abonnement.
- Een Azure-IoT-Hub in uw abonnement.
- Microsoft Visual Studio 2017 Update 8 of hoger
- Onderdeel van de cloud Explorer van Visual Studio Installer (standaard met de Azure Workload geselecteerd)

## <a name="update-cloud-explorer-to-latest-version"></a>Cloud Explorer naar de nieuwste versie bijwerken

Het onderdeel Cloud Explorer van Visual Studio Installer biedt alleen ondersteuning voor het bewaken van apparaat-naar-cloud en cloud-naar-apparaat-berichten. U moet downloaden en installeer de meest recente [Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) voor toegang tot de beheeropties.

## <a name="sign-in-to-access-your-iot-hub"></a>Meld u aan voor toegang tot uw IoT-Hub

1. In Visual Studio **Cloud Explorer** venster, klikt u op het pictogram accountbeheer. U kunt de Cloud Explorer-venster openen **weergave** > **Cloud Explorer** menu.

    ![Klik op Account beheren](media/iot-hub-visual-studio-cloud-device-messaging/click-account-management.png)

1. Klik op **Accounts beheren** in Cloud Explorer.
1. Klik op **een account toevoegen...**  in het nieuwe venster voor de eerste keer aanmelden bij Azure.
1. Nadat u zich hebt aangemeld, wordt de lijst met uw Azure-abonnement worden weergegeven. Selecteer het Azure-abonnementen weergeven en klik op het gewenste **toepassen**.
1. Vouw **uw abonnement** > **IoT-Hubs** > **uw IoT-Hub**, de lijst met apparaten worden weergegeven onder het knooppunt van uw IoT-Hub. Met de rechtermuisknop op een apparaat voor toegang tot de beheeropties.

    ![Opties voor beheer](media/iot-hub-device-management-visual-studio/management-options.png)

## <a name="direct-methods"></a>Directe methoden

1. Met de rechtermuisknop op uw apparaat en selecteer **Direct Apparaatmethode aanroepen**.
1. Voer de methodenaam en -nettolading in het invoervak.
1. Resultaten worden weergegeven de **IoT-Hub** deelvenster Uitvoer.

## <a name="read-device-twin"></a>Apparaatdubbel lezen

1. Met de rechtermuisknop op uw apparaat en selecteer **Apparaatdubbel bewerken**.
1. Een **azure-iot-device-twin.json** bestand wordt geopend met de inhoud van het dubbele apparaat.

## <a name="update-device-twin"></a>Dubbele apparaat bijwerken

1. Sommige wijzigingen van **tags** of **properties.desired** veld de **azure-iot-device-twin.json** bestand.
1. Druk op **Ctrl + S** bijwerken van het dubbele apparaat.
1. Resultaten worden weergegeven de **IoT-Hub** deelvenster Uitvoer.

## <a name="send-cloud-to-device-messages"></a>Cloud-naar-apparaat-berichten verzenden

Berichten van uw IoT-Hub worden verzonden naar uw apparaat, de volgende stappen uit:

1. Met de rechtermuisknop op uw apparaat en selecteer **C2D-bericht verzenden**.
1. Geef het bericht in het invoervak.
1. Resultaten worden weergegeven de **IoT-Hub** deelvenster Uitvoer.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u Cloud Explorer gebruiken voor Visual Studio met verschillende opties.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]