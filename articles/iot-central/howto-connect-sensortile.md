---
title: Een apparaat SensorTile.box verbinden met uw Azure IoT Central-toepassing | Microsoft Docs
description: Leer hoe u een apparaat SensorTile.box verbinden met uw Azure IoT Central-toepassing als een apparaat-ontwikkelaar.
author: sarahhubbard
ms.author: sahubbar
ms.date: 04/24/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: sandeep.pujar
ms.openlocfilehash: 8c1b4a4ab834b2203a7e0b6e4e9e366c3fc38774
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472193"
---
# <a name="connect-sensortilebox-device-to-your-azure-iot-central-application"></a>SensorTile.box apparaat verbinden met uw Azure IoT Central-toepassing

Dit artikel wordt beschreven hoe u, als een apparaat-ontwikkelaar, een SensorTile.box apparaat verbinden met uw Microsoft Azure IoT Central-toepassing.

## <a name="before-you-begin"></a>Voordat u begint

Als u wilt de stappen in dit artikel hebt voltooid, moet u de volgende bronnen:

* Een apparaat SensorTile.box. Zie voor meer informatie, [SensorTile.box](https://www.st.com/content/st_com/en/products/evaluation-tools/product-evaluation-tools/mems-motion-sensor-eval-boards/steval-mksbox1v1.html).
* De ST BLE Sensor-app op uw Android-apparaat is geïnstalleerd, kunt u [downloaden vanaf hier](https://play.google.com/store/apps/details?id=com.st.bluems). Voor meer informatie naar: [ST KELEN Sensor](https://www.st.com/stblesensor)
* Een Azure IoT Central-toepassing gemaakt op basis van de **DevKits** toepassingssjabloon. Zie voor meer informatie de [snelstart over het maken van een toepassing](quick-deploy-iot-central.md).
* Toevoegen de **SensorTile.box** apparaat sjabloon in uw IoT Central-toepassing, recentst de **Apparaatsjablonen** pagina en klik op **+ nieuw**, en selecteer de **SensorTile.box** sjabloon.

### <a name="get-your-device-connection-details"></a>Uw apparaat Verbindingsdetails ophalen

Voeg in uw Azure IoT Central-toepassing een echt apparaat uit de **SensorTile.box** apparaat sjabloon en maak een notitie van de verbindingsgegevens van apparaat: **Scope-ID**, **apparaat-ID**, en **primaire sleutel**:

1. Een apparaat uit Device Explorer toevoegen. Selecteer **+ Nieuw > echte** om toe te voegen een echt apparaat.

    * Voer een kleine **apparaat-ID**, of gebruik de voorgestelde **apparaat-ID**.
    * Voer een **apparaatnaam**, of de voorgestelde naam gebruiken

    ![Apparaat toevoegen](media/howto-connect-sensortile/real-device.png)

1. Het apparaat verbinding om informatie te krijgen, **bereik-ID**, **apparaat-ID**, en **primaire sleutel**, selecteer **Connect** op de Apparaatpagina.

    ![Verbindingsdetails](media/howto-connect-sensortile/connect-device.png)

1. Maak een notitie van de verbindingsgegevens. U bent tijdelijk niet verbonden met het internet wanneer u uw apparaat DevKit in de volgende stap voorbereiden.

## <a name="set-up-the-sensortilebox-with-the-mobile-application"></a>De SensorTile.box met de mobiele toepassing instellen

In deze sectie leert u hoe u de firmware van toepassing op het apparaat te pushen. U vervolgens de apparaatgegevens naar IoT Central verzenden via de mobiele app van ST BLE Sensor connectiviteit van de Bluetooth-laag energieverbruik (inschakelen).

1. Open de app ST BLE Sensor en druk op de **een nieuwe app maken** knop.

    ![App maken](media/howto-connect-sensortile/create-app.png)

1. Selecteer de **Barometer** toepassing.
1. Druk op de knop voor uploaden.

    ![Barometer uploaden](media/howto-connect-sensortile/barometer-upload.png)

1. Druk op de knop afspelen die zijn gekoppeld aan uw SensorTile.box.
1. Wanneer het proces voltooid is, streamt de SensorTile.box de temperatuur, druk te verlichten en vochtigheid via inschakelen.

## <a name="connect-the-sensortilebox-to-the-cloud"></a>Verbinding maken met de SensorTile.box naar de cloud

In deze sectie leert u hoe u de SensorTile.box verbinden met de mobiele toepassing, en verbinding maken met de mobiele toepassing in de cloud.

1. Gebruik het menu links, selecteer de **Cloud logboekregistratie** knop.

    ![Cloud-logboekregistratie](media/howto-connect-sensortile/cloud-logging.png)

1. Selecteer **Azure IoT Central** als cloudprovider.
1. Plaats de apparaat-ID en Scope-ID die eerder zijn vastgesteld.

    ![Referenties](media/howto-connect-sensortile/credentials.png)

1. Selecteer de **Toepassingssleutel** keuzerondje.
1. Klik op **Connect** en selecteert u de telemetrische gegevens die u wilt uploaden.
1. Na een paar seconden, de gegevens worden weergegeven op het dashboard IoT Central.

## <a name="sensortilebox-device-template-details"></a>SensorTile.box sjabloon Apparaatdetails

Een toepassing gemaakt op basis van de sjabloon SensorTile.box apparaat met de volgende kenmerken:

### <a name="telemetry"></a>Telemetrie

| Veldnaam     | Eenheden  | Minimum | Maximum | Aantal decimalen |
| -------------- | ------ | ------- | ------- | -------------- |
| vochtigheid       | %      | 30       | 90     | 1              |
| temp           | °C     | 0     | 40     | 1              |
| pressure       | mbar    | 900     | 1100    | 2              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| accelerometerX | mg     | -2000   | 2000    | 0              |
| accelerometerY | mg     | -2000   | 2000    | 0              |
| accelerometerZ | mg     | -2000   | 2000    | 0              |
| gyroscopeX     | dps   | -3276   | 3276    | 1              |
| gyroscopeY     | dps   | -3276   | 3276    | 1              |
| gyroscopeZ     | dps   | -3276   | 3276    | 1              |
| FFT_X     |    |    |     |               |
| FFT_Y     |    |    |     |               |
| FFT_Z     |    |    |     |               |

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een SensorTile.box verbinden met uw Azure IoT Central-toepassing, de voorgestelde volgende stap is te leren [over het instellen van een sjabloon aangepast apparaat](howto-set-up-template.md) voor uw eigen IoT-apparaat.
