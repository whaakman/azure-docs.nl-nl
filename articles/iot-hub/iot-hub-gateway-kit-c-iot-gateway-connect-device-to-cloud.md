---
title: Een IoT-gateway gebruiken voor verbinding van een apparaat met Azure IoT Hub | Microsoft Docs
description: Informatie over het gebruik van Intel NUC als een IoT-gateway verbinding maken met een SensorTag TI en sensorgegevens verzenden naar Azure IoT Hub in de cloud.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT gateway verbinding maken met het apparaat in de cloud
ms.assetid: cb851648-018c-4a7e-860f-b62ed3b493a5
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: 4fb77ed0241d15338c2574fd22828507c3e40cb3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="use-iot-gateway-to-connect-things-to-the-cloud---sensortag-to-azure-iot-hub"></a>Gebruik IoT gateway dingen verbinding met de cloud - SensorTag met Azure IoT Hub

> [!NOTE]
> Voordat u deze zelfstudie begint, controleert u of u hebt voltooid [Intel NUC als een IoT-gateway instellen](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md). In [Intel NUC als een IoT-gateway instellen](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), instellen van het apparaat Intel NUC als een IoT-gateway.

## <a name="what-you-will-learn"></a>Wat u leert

U informatie over het gebruik van een IoT-gateway verbinding maken met een Texas instrumenten SensorTag (CC2650STK) Azure IoT Hub. De IoT-gateway verzendt temperatuur en vochtigheid gegevens verzameld uit de SensorTag naar Azure IoT Hub.

## <a name="what-you-will-do"></a>Wat u doet

- Een iothub maken.
- Een apparaat in de iothub voor de SensorTag registreren.
- De verbinding tussen de IoT-gateway en de SensorTag inschakelen.
- Voer een voorbeeldtoepassing uitschakelen SensorTag gegevens verzenden naar uw IoT-hub.

## <a name="what-you-need"></a>Wat u nodig hebt

- Zelfstudie [Intel NUC als een IoT-gateway instellen](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) voltooid in die u Intel NUC als een IoT-gateway instelt.
- * Een actief Azure-abonnement. Als u een Azure-account geen [maken van een gratis Azure-proefaccount](https://azure.microsoft.com/free/) over een paar minuten.
- Een SSH-client die wordt uitgevoerd op de hostcomputer. PuTTY wordt aanbevolen voor Windows. Linux- en Mac OS al worden geleverd met een SSH-client.
- Het IP-adres en de gebruikersnaam en wachtwoord voor toegang tot de gateway van de SSH-client.
- Een internetverbinding.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

> [!NOTE]
> Hier registreren u dit nieuwe apparaat voor uw SensorTag

## <a name="enable-the-connection-between-the-iot-gateway-and-the-sensortag"></a>De verbinding tussen de IoT-gateway en de SensorTag inschakelen

In deze sectie kunt u de volgende taken uitvoeren:

- De MAC-adres van de SensorTag voor Bluetooth-verbinding niet ophalen.
- Een Bluetooth-verbinding van de IoT gateway om de SensorTag te starten.

### <a name="get-the-mac-address-of-the-sensortag-for-bluetooth-connection"></a>De MAC-adres van de SensorTag voor Bluetooth-verbinding ophalen

1. De SSH-client wordt uitgevoerd op de hostcomputer en maak verbinding met de IoT-gateway.
1. De blokkering opheffen Bluetooth met de volgende opdracht:

   ```bash
   sudo rfkill unblock bluetooth
   ```

1. Start de Bluetooth-service op de IoT-gateway en voert u een Bluetooth-shell Bluetooth configureren met de volgende opdrachten:

   ```bash
   sudo systemctl start bluetooth
   bluetoothctl
   ```

1. De Bluetooth-controller door de volgende opdracht in de Bluetooth-shell inschakelen:

   ```bash
   power on
   ```

   ![de Bluetooth-controller hebben op de IoT-gateway met bluetoothctl inschakelen](./media/iot-hub-iot-gateway-connect-device-to-cloud/8_power-on-bluetooth-controller-at-bluetooth-shell-bluetoothctl.png)

1. Start scannen op in de buurt Bluetooth-apparaten met de volgende opdracht:

   ```bash
   scan on
   ```

   ![In de buurt Bluetooth-apparaten met bluetoothctl scannen](./media/iot-hub-iot-gateway-connect-device-to-cloud/9_start-scan-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

1. Klik op koppelen op de SensorTag. De groene geleid op de flitsen SensorTag.
1. Op de Bluetooth-shell ziet u dat de SensorTag is gevonden. Noteer het MAC-adres van de SensorTag. In dit voorbeeld wordt het MAC-adres van de SensorTag is `24:71:89:C0:7F:82`.
1. De scan uitschakelen met de volgende opdracht:

   ```bash
   scan off
   ```

   ![In de buurt Bluetooth-apparaten met bluetoothctl scannen stopzetten](./media/iot-hub-iot-gateway-connect-device-to-cloud/10_stop-scanning-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

### <a name="initiate-a-bluetooth-connection-from-the-iot-gateway-to-the-sensortag"></a>Een Bluetooth-verbinding van de IoT-gateway naar de SensorTag starten

1. Verbinding maken met de SensorTag met de volgende opdracht:

   ```bash
   connect <MAC address>
   ```

   ![Verbinding maken met de SensorTag met bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/11_connect-to-sensortag-at-bluetooth-shell-bluetoothctl.png)

1. Verbreek de verbinding tussen de SensorTag en de Bluetooth-shell afsluiten met de volgende opdrachten:

   ```bash
   disconnect
   exit
   ```

   ![Verbreek de verbinding tussen de SensorTag met bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/12_disconnect-from-sensortag-at-bluetooth-shell-bluetoothctl.png)

U hebt de verbinding tussen de SensorTag en de IoT-gateway is ingeschakeld.

## <a name="run-a-ble-sample-application-to-send-sensortag-data-to-your-iot-hub"></a>Voer een voorbeeldtoepassing uitschakelen SensorTag gegevens verzenden naar uw IoT-hub

De voorbeeldtoepassing Bluetooth laag energieverbruik (uitschakelen) wordt verstrekt door Azure IoT rand. De voorbeeldtoepassing verzamelt gegevens van verbinding uitschakelen en de gegevens naar u IoT-hub te verzenden. Als u wilt de voorbeeldtoepassing uitvoeren, moet u:

1. Configureer de voorbeeldtoepassing.
1. De voorbeeldtoepassing uitvoeren op de IoT-gateway.

### <a name="configure-the-sample-application"></a>De voorbeeldtoepassing configureren

1. Ga naar de map van de voorbeeldtoepassing met de volgende opdracht:

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples/ble_gateway
   ```

1. Open het configuratiebestand met de volgende opdracht:

   ```bash
   vi ble_gateway.json
   ```

1. Vul in het configuratiebestand in de volgende waarden:

   **IoTHubName**: de naam van uw IoT-hub.

   **IoTHubSuffix**: IoTHubSuffix ophalen uit de primaire sleutel van de verbindingsreeks van het apparaat dat u hebt genoteerd omlaag. Zorgen ervoor dat u de primaire sleutel van de verbindingsreeks apparaat, niet de primaire sleutel van de verbindingsreeks van uw IoT-hub. De primaire sleutel van de verbindingsreeks van het apparaat zich in de indeling van `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.

   **Transport**: de standaardwaarde is `amqp`. Deze waarde wordt het protocol tijdens transpotation weergegeven. Kan de zijn `http`, `amqp`, of `mqtt`.

   **macAddress**: de MAC-adres van de SensorTag die u hebt genoteerd omlaag.

   **deviceID**: ID van het apparaat dat u hebt gemaakt in uw IoT-hub.

   **deviceKey**: de primaire sleutel van de verbindingsreeks van het apparaat.

   ![Voltooien van het configuratiebestand van de voorbeeldtoepassing uitschakelen](./media/iot-hub-iot-gateway-connect-device-to-cloud/13_edit-config-file-of-ble-sample.png)

1. Druk op `ESC` en het type `:wq` het bestand wilt opslaan.

### <a name="run-the-sample-application"></a>De voorbeeldtoepassing uitvoeren

1. Zorg ervoor dat de SensorTag is ingeschakeld.
1. Voer de volgende opdracht uit:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Volgende stappen

[Gebruik IoT gateway voor gegevenstransformatie sensor met Azure IoT rand](iot-hub-gateway-kit-c-use-iot-gateway-for-data-conversion.md)
