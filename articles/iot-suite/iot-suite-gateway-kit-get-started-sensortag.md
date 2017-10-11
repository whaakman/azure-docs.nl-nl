---
title: Verbinding maken met een gateway met een Intel NUC van Azure IoT Suite | Microsoft Docs
description: "Gebruik de Microsoft IoT commerciële Gateway Kit en de vooraf geconfigureerde oplossing voor externe controle. De Azure IoT Edge-gateway gebruiken om in te schakelen van een SensorTag-apparaat verbinding maakt met de oplossing voor externe controle, verzenden van telemetrie naar de cloud en reageren op de methoden die worden aangeroepen vanuit het dashboard van oplossing."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: dobett
ms.openlocfilehash: bda16be1094276fcecef1e708f9d7db307d94a89
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-azure-iot-edge-gateway-to-the-remote-monitoring-preconfigured-solution-and-send-telemetry-from-a-sensortag"></a>Verbinding maken met uw Azure-IoT-Edge gateway naar de vooraf geconfigureerde oplossing voor externe controle en verzend telemetrie vanuit een SensorTag

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

Deze zelfstudie laat zien hoe u met Azure IoT rand temperatuur en vochtigheid gegevens van SensorTag apparaat verzenden naar de vooraf geconfigureerde oplossing voor externe controle. De SensorTag verbindt met de Intel NUC gateway via Bluetooth. De zelfstudie wordt gebruikt:

- Azure IoT-rand voor het implementeren van een voorbeeld-gateway.
- IoT Suite remote monitoring vooraf geconfigureerde oplossing als de cloud-gebaseerde back-end.

## <a name="overview"></a>Overzicht

In deze zelfstudie maakt uitvoeren u de volgende stappen:

- Implementeer een exemplaar van de vooraf geconfigureerde oplossing voor externe controle op uw Azure-abonnement. Deze stap implementeert automatisch en meerdere Azure-services configureert.
- Uw gatewayapparaat Intel NUC instellen om te communiceren met uw computer en de oplossing voor externe controle.
- Instellen van uw gateway Intel NUC telemetrie van een apparaat SensorTag ontvangen en verzenden naar het dashboard voor externe controle.

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[Texas instrumenten uitschakelen SensorTag][lnk-sensortag]. Deze zelfstudie opgehaald telemetrische gegevens via Bluetooth uit het SensorTag-apparaat.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> De oplossing voor externe controle levert een set van Azure-services in uw Azure-abonnement. De implementatie duidt op een echte enterprise-architectuur. Om te voorkomen dat een Azure-verbruik onnodige kosten, verwijdert u uw exemplaar van de vooraf geconfigureerde oplossing op azureiotsuite.com wanneer u klaar bent met het. Als u de vooraf geconfigureerde oplossing meer nodig hebt, kunt u het eenvoudig opnieuw. Zie voor meer informatie over het verbruik verminderen terwijl de oplossing voor externe controle wordt uitgevoerd, [configureren van Azure IoT Suite vooraf geconfigureerde oplossingen voor demonstratiedoeleinden][lnk-demo-config].

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

## <a name="configure-bluetooth-connectivity"></a>Bluetooth-connectiviteit configureren

Bluetooth configureren op de Intel NUC zodat het apparaat SensorTag verbinding maken en verzenden van telemetrie.

### <a name="find-the-mac-address-of-the-sensortag"></a>Het MAC-adres van de SensorTag vinden

1. Voer de volgende opdracht om de blokkering van de Bluetooth-service in de shell op de Intel NUC:

    ```bash
    sudo rfkill unblock bluetooth
    ```

1. Voer de volgende opdrachten de Bluetooth-service starten op de Intel NUC en voer de Bluetooth-shell:

    ```bash
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Voer de volgende opdracht met power op de Bluetooth-controller:

    ```bash
    power on
    ```

    Als de domeincontroller ingeschakeld is, ziet u een bericht **power wijzigen op geslaagd**.

1. Voer de volgende opdracht om te scannen op in de buurt Bluetooth-apparaten:

    ```bash
    scan on
    ```

1. Druk op de knop op de SensorTag detecteerbaar. De groene LED knippert.

1. Wanneer er een bericht in de shell dat de controller de SensorTag heeft gedetecteerd, noteer het MAC-adres van het apparaat. Het MAC-adres ziet eruit als **A0:E6:F8:B5:F6:00**. U moet het MAC-adres verderop in de zelfstudie als u de gateway configureren.

1. Voer de volgende opdracht Bluetooth scannen uitschakelen:

    ```bash
    scan off
    ```

1. Voer de volgende opdracht om te controleren of u verbinding kunt maken met het SensorTag-apparaat:

    ```bash
    connect <SensorTag MAC address>
    ```

    Als u verbinding is, ziet u het bericht de shell **verbinding tot stand gebracht** en informatie over het apparaat SensorTag afgedrukt. Als u geen verbinding maken, controleert u dat de SensorTag nog steeds is ingeschakeld.

1. U kunt nu de SensorTag verbreken en de Bluetooth-shell afsluiten met de volgende opdrachten:

    ```bash
    disconnect
    exit
    ```

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-the-custom-iot-edge-module"></a>De aangepaste rand van de IoT-module maken

Nu kunt u de aangepaste module IoT rand waarmee de gateway om berichten te verzenden naar de oplossing voor externe controle. Zie voor meer informatie over het configureren van een gateway en de rand van de IoT-modules [Azure IoT rand concepten][lnk-gateway-concepts].

De broncode voor de aangepaste rand van de IoT-modules downloaden van GitHub met de volgende opdrachten:

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

De aangepaste rand van de IoT-module met de volgende opdrachten maken:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

Het script build libsensor2remotemonitoring.so aangepaste IoT Edge-module in de build-map geplaatst.

## <a name="configure-and-run-the-iot-edge-gateway"></a>Configureren en uitvoeren van de rand van de IoT-gateway

U kunt nu de rand van de IoT-gateway voor het verzenden van telemetrie van uw apparaat SensorTag naar uw dashboard voor externe controle configureren. Zie voor meer informatie over het configureren van een gateway en de rand van de IoT-modules [Azure IoT rand concepten][lnk-gateway-concepts].

> [!TIP]
> In deze zelfstudie gebruikt u de standaard `vi` teksteditor op de Intel NUC. Als u niet hebt gebruikt `vi` , moet u een inleidende zelfstudie, zoals uitvoeren [Unix - de vi zelfstudie Editor] [ lnk-vi-tutorial] om vertrouwd te raken met deze editor. U kunt ook installeren de gebruikersvriendelijker [nano](https://www.nano-editor.org/) editor met de opdracht `smart install nano -y`.

Open het voorbeeldconfiguratiebestand in de **vi** editor met de volgende opdracht:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic/remote_monitoring.json
```

Zoek de volgende regels in de configuratie voor de IoTHub-module:

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

Vervang de tijdelijke aanduiding voor waarden met de IoT Hub informatie u gemaakt en opgeslagen aan het begin van deze zelfstudie. De waarde voor IoTHubName eruit **yourrmsolution37e08**, en is meestal de waarde voor IoTSuffix **azure devices.net**.

Zoek de volgende regels in de configuratie voor de module toewijzing:

```json
args": [
  {
    "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

Vervang de **macAddress** aanduiding voor items met de MAC-adres van uw SensorTag die u eerder hebt genoteerd. Vervang de **deviceID** en **deviceKey** tijdelijke aanduidingen door de id's en sleutels voor de twee apparaten die u eerder in de oplossing voor externe controle hebt gemaakt.

Zoek de volgende regels in de configuratie voor de SensorTag-module:

```json
"args": {
  "controller_index": 0,
  "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
  ...
}
```

Vervang de **apparaat\_mac\_adres** aanduiding voor items met de MAC-adres van uw SensorTag die u eerder hebt genoteerd.

Sla uw wijzigingen op.

U kunt nu de gateway met de volgende opdrachten uitvoeren:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
/usr/share/azureiotgatewaysdk/samples/ble_gateway/ble_gateway remote_monitoring.json
```

De gateway aan de rand van de IoT begint op de Intel NUC en verzendt telemetrie van de SensorTag naar de oplossing voor externe controle:

![Rand IoT gateway verzendt telemetrie van de SensorTag][img-telemetry]

Druk op **Ctrl-C** om af te sluiten van het programma op elk gewenst moment.

## <a name="view-the-telemetry"></a>De telemetrie weergeven

De gateway verzendt nu telemetrie vanaf het SensorTag-apparaat aan de oplossing voor externe controle. U kunt de telemetrie weergeven op het dashboard van oplossing. U kunt ook opdrachten verzenden naar uw SensorTag-apparaat via de gateway van het dashboard van de oplossing.

- Ga naar het dashboard van de oplossing.
- Selecteer het apparaat dat u hebt geconfigureerd in de gateway die voor de SensorTag in vertegenwoordigt de **apparaat naar de weergave** vervolgkeuzelijst.
- De telemetrie van het apparaat SensorTag wordt weergegeven op het dashboard.

![Telemetrie weergegeven van de SensorTag-apparaten][img-telemetry-display]

> [!WARNING]
> Als u de oplossing voor externe controle uitgevoerd in uw Azure-account laat, wordt u gefactureerd voor de tijd die wordt uitgevoerd. Zie voor meer informatie over het verbruik verminderen terwijl de oplossing voor externe controle wordt uitgevoerd, [configureren van Azure IoT Suite vooraf geconfigureerde oplossingen voor demonstratiedoeleinden][lnk-demo-config]. De vooraf geconfigureerde oplossing verwijderen uit uw Azure-account wanneer u klaar bent met het gebruik van maken.


## <a name="next-steps"></a>Volgende stappen

Ga naar de [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) voor meer voorbeelden en documentatie over Azure IoT.

[img-telemetry]: ./media/iot-suite-gateway-kit-get-started-sensortag/appoutput.png


[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-sensortag/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-sensortag]: http://www.ti.com/ww/en/wireless_connectivity/sensortag/
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started