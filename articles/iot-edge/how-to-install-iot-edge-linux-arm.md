---
title: Het installeren van Azure IoT Edge op Linux | Microsoft Docs
description: Azure IoT rand installatie-instructies op Linux op ARM32
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: ad70fcc6b9779cb33772a3fce2fb11b4cec804ee
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062595"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Azure IoT rand runtime installeren op Linux (ARM32v7/armhf)

De runtime Azure IoT-rand wordt geïmplementeerd op alle rand van de IoT-apparaten. Er zijn drie onderdelen gedefinieerd. De **IoT rand beveiliging daemon** biedt en onderhoudt beveiligingsstandaarden op het apparaat aan de rand. De daemon begint op elke keer opstarten en het apparaat bootstraps door de rand van de IoT-agent wordt gestart. De **IoT rand agent** vereenvoudigt de implementatie en controle van modules op het apparaat aan de rand, met inbegrip van de rand van de IoT-hub. Met de **IoT Edge-hub** wordt de communicatie tussen modules op het IoT Edge-apparaat en tussen het apparaat en IoT Hub beheerd.

In dit artikel vindt u de stappen voor het installeren van de runtime Azure IoT Edge op een Linux ARM32v7/armhf Edge-apparaat (bijvoorbeeld frambozen Pi).

>[!NOTE]
>Pakketten in de Linux-software-opslagplaatsen zijn onderworpen aan de licentievoorwaarden die zich in elk pakket (/ usr/delen doc/*pakketnaam*). Lees de licentievoorwaarden voordat het pakket. De installatie en het gebruik van het pakket wordt verstaan onder de bevestiging van deze voorwaarden. Als u niet akkoord met de licentievoorwaarden gaat, gebruik niet het pakket.

## <a name="install-the-container-runtime"></a>Installeren van de container-runtime

Azure IoT-rand is afhankelijk van een [OCI-compatibele] [ lnk-oci] container runtime (bijvoorbeeld Docker). Als u al Docker CE/EE geïnstalleerd op de Edge-apparaat hebt, kunt u blijven gebruiken voor ontwikkeling en tests met Azure IoT rand. 

Voor productiescenario's is het raadzaam u gebruikt de [Moby gebaseerde] [ lnk-moby] engine hieronder. Dit is de enige container-engine officieel ondersteund met Azure IoT rand. Docker CE/EE container afbeeldingen zijn volledig compatibel is met de runtime Moby.

Onderstaande opdrachten installeren moby-engine en opdrachtregelinterface (CLI). De CLI is nuttig voor ontwikkeling, maar is optioneel voor productie-implementaties.

```cmd/sh

# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the moby-engine
curl -L https://aka.ms/moby-engine-armhf-latest -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb

# Download and install the moby-cli
curl -L https://aka.ms/moby-cli-armhf-latest -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb

# Run apt-get fix
sudo apt-get install -f

```

## <a name="install-the-iot-edge-security-daemon"></a>De Daemon van de beveiliging IoT rand installeren

```cmd/sh
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the standard libiothsm implementation
curl -L https://aka.ms/libiothsm-std-linux-armhf-latest -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb

# Download and install the IoT Edge Security Daemon
curl -L https://aka.ms/iotedged-linux-armhf-latest -o iotedge.deb && sudo dpkg -i ./iotedge.deb

# Run apt-get fix
sudo apt-get install -f
```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>De Daemon van Azure IoT-rand beveiliging configureren

De daemon kan worden geconfigureerd met behulp van het configuratiebestand op `/etc/iotedge/config.yaml` apparaat aan de rand kan worden geconfigureerd <!--[automatically via Device Provisioning Service][lnk-dps] or--> handmatig met een [apparaat verbindingsreeks][lnk-dcs].

Voer de apparaat-verbindingsreeks in voor de handmatige configuratie **inrichting** sectie van **config.yaml**

```yaml
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
```

*Het bestand is beveiligd tegen schrijven standaard moet u mogelijk gebruik van `sudo` om deze te bewerken. Bijvoorbeeld `sudo nano /etc/iotedge/config.yaml`*

Na het invoeren van de Inrichtingsgegevens in de configuratie, start u de daemon opnieuw:

```cmd/sh
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Controleer of geslaagde installatie

U kunt de status van het gebruik van de IoT rand Daemon controleren:

```cmd/sh
systemctl status iotedge
```

Controleer de daemon logboeken met behulp van:

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

En lijst met modules met:

```cmd/sh
iotedge list
```

## <a name="next-steps"></a>Volgende stappen

Als u problemen ondervindt met de installatie naar behoren afhandeling Edge-runtime de [probleemoplossing] [ lnk-trouble] pagina.

<!-- Links -->
[lnk-dcs]: ../iot-hub/quickstart-send-telemetry-dotnet.md#register-a-device
[lnk-dps]: how-to-simulate-dps-tpm.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md