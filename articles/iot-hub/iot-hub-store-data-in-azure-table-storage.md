---
title: Uw IoT hub-berichten opslaan in Azure-gegevensopslag | Microsoft Docs
description: Gebruik IoT Hub-berichtroutering naar uw IoT hub-berichten opslaan in de Azure blob-opslag. De IoT hub-berichten bevatten informatie, zoals sensorgegevens, dat wordt verzonden door uw IoT-apparaat.
author: rangv
manager: ''
keywords: gegevensopslag voor IOT, iot sensor-gegevensopslag
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 84355453a5cb8d8f42abdcbde5432651c9c035b0
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856287"
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-blob-storage"></a>IoT hub-berichten met sensorgegevens naar uw Azure blob-opslag opslaan

![Diagram voor end-to-end](./media/iot-hub-store-data-in-azure-table-storage/1_route-to-storage.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Wat u leert

U informatie over het maken van een Azure storage-account en een Azure-functie-app voor het opslaan van IoT hub-berichten in Azure Blob-opslag.

## <a name="what-you-do"></a>Wat u allemaal doen

- Een Azure-opslagaccount maken.
- Instellen van uw IoT-hub te routeren van berichten naar de opslag.

## <a name="what-you-need"></a>Wat u nodig hebt

- [Instellen van uw apparaat](iot-hub-raspberry-pi-kit-node-get-started.md) om te kunnen krijgen van de volgende vereisten:
  - Een actief Azure-abonnement
  - Een IoT-hub in uw abonnement 
  - Uitvoeren van een toepassing waarmee berichten worden verzonden naar uw IoT hub

## <a name="create-an-azure-storage-account"></a>Een Azure-opslagaccount maken

1. In de [Azure-portal](https://portal.azure.com/), klikt u op **een resource maken** > **opslag** > **opslagaccount**.

2. Voer de benodigde gegevens voor de storage-account:

   ![Een storage-account maken in Azure portal](./media/iot-hub-store-data-in-azure-table-storage/1_azure-portal-create-storage-account.png)

   * **Naam**: de naam van het storage-account. De naam moet wereldwijd uniek zijn.

   * **Soort account**: kies `Storage (general purpose v1)`.

   * **Locatie**: Kies de locatie die gebruikmaakt van uw IoT-hub.

   * **Replicatie**: kies `Locally-redundant storage (LRS)`.

   * **Prestaties**: kies `Standard`.

   * **Veilige overdracht vereist**: kies `Disabled`.

   * **Abonnement**: selecteer uw Azure-abonnement.

   * **Resourcegroep**: gebruik dezelfde resourcegroep die gebruikmaakt van uw IoT-hub.

   * **Vastmaken aan dashboard**: selecteer deze optie voor eenvoudige toegang tot uw IoT-hub vanuit het dashboard.

3. Klik op **Create**.

## <a name="prepare-your-iot-hub-to-route-messages-to-storage"></a>Voorbereiden van uw IoT-hub te routeren van berichten naar opslag

IoT Hub biedt systeemeigen ondersteuning voor routeren van berichten naar Azure storage als blobs. Voor meer informatie over de Azure IoT Hub met aangepaste eindpunten, kunt u verwijzen naar [lijst van ingebouwde IoT Hub-eindpunten](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-endpoints#custom-endpoints).

### <a name="add-storage-as-a-custom-endpoint"></a>Opslag toevoegen als een aangepast eindpunt

1. Navigeer naar uw IoT-hub in Azure portal. 

2. Klik op **eindpunten** > **toevoegen**. 

3. De naam van het eindpunt en selecteer **Azure-Opslagcontainer** als het eindpunttype. 

4. Gebruik de kiezer om te selecteren van het opslagaccount dat u in de vorige sectie hebt gemaakt. Maken van een storage-container en selecteert u deze en klik vervolgens op **OK**.

   ![Een aangepast eindpunt in IoT-Hub maken](./media/iot-hub-store-data-in-azure-table-storage/2_custom-storage-endpoint.png)

### <a name="add-a-route-to-route-data-to-storage"></a>De gegevens van een route-route toevoegen aan opslag

1. Klik op **Routes** > **toevoegen** en voer een naam voor de route. 

2. Selecteer **Apparaatberichten** als de gegevensbron en selecteer het opslageindpunt die u zojuist hebt gemaakt als het eindpunt in de route. 

3. Voer `true` als de querytekenreeks en klik vervolgens op **opslaan**.

   ![Een route in IoT Hub maken](./media/iot-hub-store-data-in-azure-table-storage/3_create-route.png)
  
### <a name="add-a-route-for-hot-path-telemetry-optional"></a>Toevoegen van een route voor het dynamische pad telemetrie (optioneel)

Standaard routeert IoT-Hub alle berichten die niet overeenkomen met alle andere routes met het ingebouwde eindpunt. Omdat alle berichten over telemetrie is nu overeenkomt met de regel die de berichten worden doorgestuurd naar de opslag, moet u een andere route voor berichten die worden geschreven naar het ingebouwde eindpunt toevoegen. Er zijn geen extra kosten voor het routeren van berichten naar meerdere eindpunten.

> [!NOTE]
> U kunt deze stap overslaan als u geen extra verwerking van berichten over uw telemetrie.

1. Klik op **toevoegen** in het deelvenster Routes en voer een naam voor de route. 

2. Selecteer **Apparaatberichten** als de gegevensbron en **gebeurtenissen** als het eindpunt. 

3. Voer `true` als de querytekenreeks en klik vervolgens op **opslaan**.

  ![Een route hot-pad in IoT-Hub maken](./media/iot-hub-store-data-in-azure-table-storage/4_hot-path-route.png)

## <a name="verify-your-message-in-your-storage-container"></a>Controleer of het bericht in uw opslagcontainer

1. Voer de voorbeeldtoepassing op uw apparaat berichten te verzenden naar uw IoT-hub.

2. [Download en installeer Azure Storage Explorer](http://storageexplorer.com/).

3. Storage Explorer openen, klikt u op **een Azure-Account toevoegen** > **aanmelden**, en meldt u zich aan bij uw Azure-account.

4. Klik op uw Azure-abonnement > **Opslagaccounts** > uw storage-account > **Blobcontainers** > uw container.

   Hier ziet u berichten die naar uw IoT-hub die is vastgelegd in de blob-container van het apparaat wordt verzonden.

## <a name="clean-up-resources"></a>Resources opschonen 

In deze zelfstudie kunt u een storage-account toegevoegd, en vervolgens Routering voor berichten van de IoT-Hub worden geschreven naar het opslagaccount dat wordt toegevoegd. Voor het opschonen van de resources die u hebt gemaakt, verwijdert u de gegevens van de Routering en verwijder vervolgens het opslagaccount. 

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

2. Klik op **resourcegroepen** en selecteer de resourcegroep die u hebt gebruikt. De lijst met resources in de groep wordt weergegeven. 

   > [!NOTE]
   > Als u alle resources in de resourcegroep te verwijderen wilt, klikt u op **verwijderen** als u wilt verwijderen van de resourcegroep, volg de aanwijzingen. Hiermee verwijdert u alles in die resourcegroep, zodat u bent klaar opschonen van de resources en u kunt doorgaan met de volgende sectie.

3. Klik op de IoT-hub die u voor deze zelfstudie hebt gebruikt. 

4. Klik in het deelvenster IoT-Hub **Routes**. Klik op het selectievakje naast de routeringsregel u toegevoegd en klik vervolgens op **verwijderen**. Wanneer u wordt gevraagd als u zeker dat u wilt verwijderen die route bent, klikt u op **Ja**.

   ![Verwijderen van de regel voor doorsturen](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-routing.png)

   Sluit het deelvenster routering. U keert terug naar het deelvenster van de resourcegroep.

5. Klik opnieuw op de IoT-hub. 

6. Klik in het deelvenster IoT-Hub **eindpunten**. Klik op het selectievakje naast het eindpunt dat u hebt toegevoegd voor de storage-container en klik vervolgens op **verwijderen**. Wanneer u wordt gevraagd als u zeker dat u wilt verwijderen van het eindpunt van de geselecteerde bent, klikt u op **Ja**.

    ![Eindpunt verwijderen](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-endpoint.png)

    Sluit het deelvenster eindpunten. U keert terug naar het deelvenster van de resourcegroep. 

7.  Klik op het opslagaccount dat u voor deze zelfstudie hebt ingesteld. 

8.  Klik in het deelvenster met Storage-account op **verwijderen** de storage-account te verwijderen. U gaat naar de **opslagaccount verwijderen** deelvenster.

   ![Opslagaccount verwijderen](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-storageaccount.png)

8.  Typ in de naam van het opslagaccount en klik vervolgens op **verwijderen** aan de onderkant van het deelvenster. 

## <a name="next-steps"></a>Volgende stappen

U hebt uw Azure storage-account en gemaakt gerouteerde berichten uit IoT Hub naar een blobcontainer in het storage-account.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
