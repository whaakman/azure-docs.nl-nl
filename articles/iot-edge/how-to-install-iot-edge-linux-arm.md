---
title: Over het installeren van Azure IoT Edge op Linux | Microsoft Docs
description: Azure IoT Edge-installatie-instructies voor Linux op ARM32
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: e814785af7041ba762f7c383a0cfdc434fce9dce
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213869"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Azure IoT Edge-runtime installeren in Linux (ARM32v7/armhf)

De Azure IoT Edge-runtime wordt geïmplementeerd op alle IoT Edge-apparaten. Deze bevat drie onderdelen. De **IoT Edge security daemon** biedt en onderhoudt beveiligingsstandaarden op het Edge-apparaat. De daemon begint op elke keer opstarten en bootstrapt van het apparaat door de IoT Edge-agent wordt gestart. De **IoT Edge agent** vergemakkelijkt de implementatie en bewaking van modules op het Edge-apparaat, met inbegrip van de IoT Edge hub. Met de **IoT Edge-hub** wordt de communicatie tussen modules op het IoT Edge-apparaat en tussen het apparaat en IoT Hub beheerd.

In dit artikel bevat de stappen voor het installeren van de Azure IoT Edge-runtime op een Linux ARM32v7/armhf Edge-apparaat (bijvoorbeeld Raspberry Pi).

>[!NOTE]
>Pakketten in de opslagplaatsen met Linux-software zijn afhankelijk van de licentievoorwaarden die zich in elk pakket (/ usr/delen/Docs/*pakketnaam*). Lees de licentievoorwaarden voordat u het pakket. De installatie en het gebruik van het pakket wordt verstaan onder uw acceptatie van deze voorwaarden. Als u niet akkoord met de licentievoorwaarden gaat, moet u het pakket niet gebruiken.

## <a name="install-the-container-runtime"></a>De container-runtime installeren

Azure IoT Edge is afhankelijk van een [OCI-compatibele] [ lnk-oci] container-runtime. Voor productiescenario's, het is raadzaam gebruikt u de [Moby gebaseerde] [ lnk-moby] engine hieronder. Dit is de enige container-engine officieel ondersteund met Azure IoT Edge. Containerinstallatiekopieën met docker CE/EE zijn compatibel met de runtime op basis van Moby.

Onderstaande opdrachten installeren de Moby gebaseerde engine en de opdrachtregelinterface (CLI). De CLI is handig voor het ontwikkelen van, maar is optioneel voor productie-implementaties.

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

## <a name="install-the-iot-edge-security-daemon"></a>De IoT Edge Security-Daemon installeren

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

## <a name="configure-the-azure-iot-edge-security-daemon"></a>De Daemon van de beveiliging met Azure IoT Edge configureren


De daemon kan worden geconfigureerd met behulp van het configuratiebestand op `/etc/iotedge/config.yaml`. Het bestand is tegen schrijven beveiligd standaard, moet u mogelijk de verhoogde machtigingen om deze te bewerken.

```bash
sudo nano /etc/iotedge/config.yaml
```

Het edge-apparaat kan worden geconfigureerd handmatig met behulp van een [apparaatverbindingsreeks] [ lnk-dcs] of [automatisch via Device Provisioning Service] [ lnk-dps].

* Handmatige configuratie, verwijder opmerkingen bij de **handmatige** Inrichtingsmethode. Werk de waarde van **device_connection_string** door de verbindingsreeks van uw IoT Edge-apparaat.

   ```yaml
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # provisioning: 
   #   source: "dps"
   #   global_endpoint: "https://global.azure-devices-provisioning.net"
   #   scope_id: "{scope_id}"
   #   registration_id: "{registration_id}"
   ```

* Voor automatische configuratie, verwijder de opmerking bij de **dps** Inrichtingsmethode. De waarden voor **scope_id** en **registration_id** door de waarden van uw IoT Hub-DPS-exemplaar en uw IoT Edge-apparaat met TPM. 

   ```yaml
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   provisioning: 
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     registration_id: "{registration_id}"
   ```

Sla het bestand op en sluit het. 

   `CTRL + X`, `Y`, `Enter`

Na het invoeren van de Inrichtingsgegevens in de configuratie, start u de daemon opnieuw:

```cmd/sh
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Controleer of geslaagde installatie

Als u gebruikt de **handmatige configuratie** stappen in de vorige sectie, IoT Edge-runtime moet is ingericht en wordt uitgevoerd op uw apparaat. Als u gebruikt de **automatische configuratie** stappen, moet u enkele extra stappen uitvoeren zodat de runtime uw apparaat met uw IoT-hub uit uw naam registreren kan. Zie voor de volgende stappen [maken en inrichten van een gesimuleerd TPM-Edge-apparaat op een Linux-machine](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

U kunt de status van het gebruik van de IoT Edge-Daemon controleren:

```cmd/sh
systemctl status iotedge
```

Controleer de daemon-logboeken met behulp van:

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

En lijst met modules met:

```cmd/sh
sudo iotedge list
```
>[!NOTE]
>Voor de resource beperkte apparaten zoals RaspberryPi, het is raadzaam dat *OptimizeForPerformance* omgevingsvariabele is ingesteld op *false* volgens de instructies in de [ gids voor probleemoplossing.][lnk-trouble]


## <a name="next-steps"></a>Volgende stappen

Als u problemen ondervindt met het Edge-runtime installeren goed, bekijk de [probleemoplossing] [ lnk-trouble] pagina.

<!-- Links -->
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-linux.md
[lnk-trouble]: https://docs.microsoft.com/azure/iot-edge/troubleshoot#stability-issues-on-resource-constrained-devices
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
