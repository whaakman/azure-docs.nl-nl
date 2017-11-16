---
title: IoT hub berichten opslaan in Azure gegevensopslag | Microsoft Docs
description: Gebruik IoT Hub berichtroutering opslaan van uw IoT hub berichten naar uw Azure-blobopslag. De IoT hub berichten bevatten informatie, zoals sensorgegevens dat wordt verzonden door uw IoT-apparaat.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT-gegevensopslag, gegevensopslag iot-temperatuursensor
ms.assetid: 62fd14fd-aaaa-4b3d-8367-75c1111b6269
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/04/2017
ms.author: xshi
ms.openlocfilehash: 5419f0fb86f2a7b051ffc7fda17c74cf15178a6b
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-blob-storage"></a>IoT hub berichten met sensorgegevens naar uw Azure blob storage opslaan

![End-to-end-diagram](media/iot-hub-store-data-in-azure-table-storage/1_route-to-storage.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Wat u leert

U informatie over het maken van een Azure storage-account en een Azure-functie-app voor het opslaan van IoT hub berichten in de blob-opslag.

## <a name="what-you-do"></a>Wat u doet

- Een Azure-opslagaccount maken.
- Bereid uw IoT-hub te routeren van berichten naar de opslag.

## <a name="what-you-need"></a>Wat u nodig hebt

- [Instellen van uw apparaat](iot-hub-raspberry-pi-kit-node-get-started.md) omvatten de volgende vereisten:
  - Een actief Azure-abonnement
  - Een IoT-hub in uw abonnement 
  - Een actieve toepassing die berichten naar uw IoT-hub verzendt

## <a name="create-an-azure-storage-account"></a>Een Azure-opslagaccount maken

1. In de [Azure-portal](https://portal.azure.com/), klikt u op **nieuw** > **opslag** > **opslagaccount**  >   **Maak**.

2. Voer de benodigde gegevens voor het opslagaccount:

   ![Een opslagaccount maken in de Azure portal](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * **Naam**: de naam van het opslagaccount. De naam moet wereldwijd uniek zijn.

   * **Resourcegroep**: gebruik dezelfde resourcegroep die gebruikmaakt van uw IoT-hub.

   * **Vastmaken aan dashboard**: selecteer deze optie voor eenvoudige toegang tot uw IoT-hub vanuit het dashboard.

3. Klik op **Create**.

## <a name="prepare-your-iot-hub-to-route-messages-to-storage"></a>Voorbereiden van uw IoT-hub om berichten te routeren naar opslag

IoT Hub biedt systeemeigen ondersteuning voor routeren van berichten naar Azure-opslag als blobs.

### <a name="add-storage-as-a-custom-endpoint"></a>Opslag toevoegen als een aangepaste eindpunt

Navigeer naar uw IoT-hub in de Azure portal. Klik op **eindpunten** > **toevoegen**. De naam van het eindpunt en selecteer **Azure Storage-Container** als het eindpunttype. Gebruik de objectkiezer selecteren van het opslagaccount dat u in de vorige sectie hebt gemaakt. Een opslagcontainer maken, selecteert u deze en klikt u op **OK**.

  ![Maak een aangepaste eindpunt in IoT-Hub](media\iot-hub-store-data-in-azure-table-storage\2_custom-storage-endpoint.png)

### <a name="add-a-route-to-route-data-to-storage"></a>De gegevens van een route-route toevoegen aan de opslag

Klik op **Routes** > **toevoegen** en voer een naam voor de route. Selecteer **apparaat-berichten** als de gegevensbron en selecteert u het opslag-eindpunt dat u zojuist hebt gemaakt als het eindpunt in de route. Voer `true` als de queryreeks, klik vervolgens op **opslaan**.

  ![Een route in IoT Hub maken](media\iot-hub-store-data-in-azure-table-storage\3_create-route.png)
  
### <a name="add-a-route-for-hot-path-telemetry-optional"></a>Voeg een route voor hot pad telemetrie (optioneel)

IoT Hub routeert standaard alle berichten die niet overeenkomt met een andere deze routes voor het eindpunt van de ingebouwde. Aangezien alle telemetrieberichten nu de regel die de berichten worden doorgestuurd naar de opslag overeenkomen, moet u een andere route voor berichten die worden geschreven naar het ingebouwde eindpunt toevoegen. Er zijn geen extra kosten om berichten te routeren naar meerdere eindpunten.

> [!NOTE]
> U kunt deze stap overslaan als u geen extra verwerking op de telemetrieberichten.

Klik op **toevoegen** in het deelvenster Routes en voer een naam voor de route. Selecteer **apparaat-berichten** als de gegevensbron en **gebeurtenissen** als het eindpunt. Voer `true` als de queryreeks, klik vervolgens op **opslaan**.

  ![Een route hot pad in IoT Hub maken](media\iot-hub-store-data-in-azure-table-storage\4_hot-path-route.png)

## <a name="verify-your-message-in-your-storage-container"></a>Controleer of het bericht in uw storage-container

1. Voer de voorbeeldtoepassing op uw apparaat om berichten te verzenden naar uw IoT-hub.

2. [Download en installeer Azure Opslagverkenner](http://storageexplorer.com/).

3. Open Opslagverkenner, klik op **een Azure-Account toevoegen** > **aanmelden**, en vervolgens weer aanmelden bij uw Azure-account.

4. Klik op uw Azure-abonnement > **Opslagaccounts** > uw storage-account > **Blob-Containers** > uw container.

   U ziet de berichten die naar uw IoT-hub die is vastgelegd in de blob-container van het apparaat wordt verzonden.

## <a name="next-steps"></a>Volgende stappen

U hebt gemaakt uw Azure storage-account en gerouteerde berichten uit IoT Hub naar een blob-container in dit opslagaccount.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
