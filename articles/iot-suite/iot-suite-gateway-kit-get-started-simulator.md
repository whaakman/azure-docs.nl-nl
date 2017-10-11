---
title: Verbinding maken met een gateway met een Intel NUC van Azure IoT Suite | Microsoft Docs
description: "Gebruik de Microsoft IoT commerciële Gateway Kit en de vooraf geconfigureerde oplossing voor externe controle. Gebruik de rand van Azure IoT gateway verbinding maken met de oplossing voor externe controle, gesimuleerde telemetrie verzenden naar de cloud en reageren op de methoden die worden aangeroepen vanuit het dashboard van oplossing."
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
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 9ed57d3c23e2adbd42c054f33c8ed46e3d6c9792
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-azure-iot-edge-gateway-to-the-remote-monitoring-preconfigured-solution-and-send-simulated-telemetry"></a>Verbinding maken met uw Azure-IoT-Edge gateway naar de vooraf geconfigureerde oplossing voor externe controle en gesimuleerde telemetrie verzenden

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

Deze zelfstudie laat zien hoe u met Azure IoT rand temperatuur en vochtigheid gegevens worden verzonden naar de vooraf geconfigureerde oplossing voor externe controle te simuleren. De zelfstudie wordt gebruikt:

- Azure IoT-rand voor het implementeren van een voorbeeld-gateway.
- IoT Suite remote monitoring vooraf geconfigureerde oplossing als de cloud-gebaseerde back-end.

## <a name="overview"></a>Overzicht

In deze zelfstudie maakt uitvoeren u de volgende stappen:

- Implementeer een exemplaar van de vooraf geconfigureerde oplossing voor externe controle op uw Azure-abonnement. Deze stap implementeert automatisch en meerdere Azure-services configureert.
- Uw gatewayapparaat Intel NUC instellen om te communiceren met uw computer en de oplossing voor externe controle.
- De rand van de IoT-gateway voor het verzenden van de gesimuleerde telemetrie die u op het dashboard van de oplossing weergeven kunt configureren.

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> De oplossing voor externe controle levert een set van Azure-services in uw Azure-abonnement. De implementatie duidt op een echte enterprise-architectuur. Om te voorkomen dat een Azure-verbruik onnodige kosten, verwijdert u uw exemplaar van de vooraf geconfigureerde oplossing op azureiotsuite.com wanneer u klaar bent met het. Als u de vooraf geconfigureerde oplossing meer nodig hebt, kunt u het eenvoudig opnieuw. Zie voor meer informatie over het verbruik verminderen terwijl de oplossing voor externe controle wordt uitgevoerd, [configureren van Azure IoT Suite vooraf geconfigureerde oplossingen voor demonstratiedoeleinden][lnk-demo-config].

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

Herhaal de vorige stappen voor het toevoegen van een tweede apparaat met een apparaat-ID, zoals **device02**. Het voorbeeld verzendt de gegevens van twee gesimuleerde apparaten in de gateway naar de oplossing voor externe controle.

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

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
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

Het script build libsimulator.so aangepaste IoT Edge-module in de build-map geplaatst.

## <a name="configure-and-run-the-iot-edge-gateway"></a>Configureren en uitvoeren van de rand van de IoT-gateway

U kunt nu de rand van de IoT-gateway voor het gesimuleerde telemetrie verzenden naar uw dashboard voor externe controle configureren. Zie voor meer informatie over het configureren van een gateway en de rand van de IoT-modules [Azure IoT rand concepten][lnk-gateway-concepts].

> [!TIP]
> In deze zelfstudie gebruikt u de standaard `vi` teksteditor op de Intel NUC. Als u niet hebt gebruikt `vi` , moet u een inleidende zelfstudie, zoals uitvoeren [Unix - de vi zelfstudie Editor] [ lnk-vi-tutorial] om vertrouwd te raken met deze editor. U kunt ook installeren de gebruikersvriendelijker [nano](https://www.nano-editor.org/) editor met de opdracht `smart install nano -y`.

Open het voorbeeldconfiguratiebestand in de **vi** editor met de volgende opdracht:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator/remote_monitoring.json
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
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  },
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

Vervang de **deviceID** en **deviceKey** tijdelijke aanduidingen door de id's en sleutels voor de twee apparaten die u eerder in de oplossing voor externe controle hebt gemaakt.

Sla uw wijzigingen op.

U kunt nu de rand van de IoT-gateway met de volgende opdrachten uitvoeren:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
/usr/share/azureiotgatewaysdk/samples/simulated_device_cloud_upload/simulated_device_cloud_upload remote_monitoring.json
```

De gateway wordt gestart op de Intel NUC en gesimuleerde telemetrie verzendt naar de oplossing voor externe controle:

![Gateway aan de rand van de IoT gesimuleerde telemetrie genereert][img-simulated telemetry]

Druk op **Ctrl-C** om af te sluiten van het programma op elk gewenst moment.

## <a name="view-the-telemetry"></a>De telemetrie weergeven

De rand van de IoT-gateway is nu gesimuleerde telemetrie verzenden naar de oplossing voor externe controle. U kunt de telemetrie weergeven op het dashboard van oplossing.

- Ga naar het dashboard van de oplossing.
- Selecteer een van de twee apparaten die u hebt geconfigureerd in de gateway in de **apparaat naar de weergave** vervolgkeuzelijst.
- De telemetrie van de gatewayapparaten wordt weergegeven op het dashboard.

![Telemetrie weergegeven van de gesimuleerde gatewayapparaten][img-telemetry-display]

> [!WARNING]
> Als u de oplossing voor externe controle uitgevoerd in uw Azure-account laat, wordt u gefactureerd voor de tijd die wordt uitgevoerd. Zie voor meer informatie over het verbruik verminderen terwijl de oplossing voor externe controle wordt uitgevoerd, [configureren van Azure IoT Suite vooraf geconfigureerde oplossingen voor demonstratiedoeleinden][lnk-demo-config]. De vooraf geconfigureerde oplossing verwijderen uit uw Azure-account wanneer u klaar bent met het gebruik van maken.

## <a name="next-steps"></a>Volgende stappen

Ga naar de [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) voor meer voorbeelden en documentatie over Azure IoT.

[img-simulated telemetry]: ./media/iot-suite-gateway-kit-get-started-simulator/appoutput.png

[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-simulator/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started