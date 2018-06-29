---
title: Het installeren van Azure IoT Edge op Linux | Microsoft Docs
description: Azure IoT rand installatie-instructies op Linux
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: kgremban
ms.openlocfilehash: d1df27300ac2e7f4732d32f54b2a4a4803f421f2
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37100439"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>Azure IoT rand runtime installeren op Linux (x64)

De runtime Azure IoT-rand wordt geïmplementeerd op alle rand van de IoT-apparaten. Er zijn drie onderdelen gedefinieerd. De **IoT rand beveiliging daemon** biedt en onderhoudt beveiligingsstandaarden op het apparaat aan de rand. De daemon begint op elke keer opstarten en het apparaat bootstraps door de rand van de IoT-agent wordt gestart. De **IoT rand agent** vereenvoudigt de implementatie en controle van modules op het apparaat aan de rand, met inbegrip van de rand van de IoT-hub. Met de **IoT Edge-hub** wordt de communicatie tussen modules op het IoT Edge-apparaat en tussen het apparaat en IoT Hub beheerd.

In dit artikel vindt u de stappen voor het installeren van de rand van Azure IoT-runtime op uw Linux x64 (Intel/AMD) randapparaat.

>[!NOTE]
>Pakketten in de Linux-software-opslagplaatsen zijn onderworpen aan de licentievoorwaarden die zich in elk pakket (/ usr/delen doc/*pakketnaam*). Lees de licentievoorwaarden voordat het pakket. De installatie en het gebruik van het pakket wordt verstaan onder de bevestiging van deze voorwaarden. Als u niet akkoord met de licentievoorwaarden gaat, gebruik niet het pakket.

## <a name="register-microsoft-key-and-software-repository-feed"></a>Microsoft sleutel en software-opslagplaats feed registreren

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```cmd/sh
# Install repository configuration
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# Install Microsoft GPG public key
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

### <a name="ubuntu-1804"></a>Ubuntu 18.04

```cmd/sh
# Install repository configuration
curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# Install Microsoft GPG public key
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

### <a name="debian-9"></a>Debian 9

```cmd/sh
# Install repository configuration
curl https://packages.microsoft.com/config/debian/9/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

# Install Microsoft GPG public key
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

## <a name="install-the-container-runtime"></a>Installeren van de container-runtime 

Azure IoT-rand is afhankelijk van een [OCI-compatibele] [ lnk-oci] container runtime (bijvoorbeeld Docker). Als u al Docker CE/EE geïnstalleerd op de Edge-apparaat hebt, kunt u blijven gebruiken voor ontwikkeling en tests met Azure IoT rand. 

Voor productiescenario's is het raadzaam u gebruikt de [Moby gebaseerde] [ lnk-moby] engine hieronder. Dit is de enige container-engine officieel ondersteund met Azure IoT rand. Docker CE/EE container afbeeldingen zijn volledig compatibel is met de runtime Moby.

*Onderstaande instructies installeren moby-engine en opdrachtregelinterface (CLI). De CLI is nuttig voor ontwikkeling, maar is optioneel voor productie-implementaties.*

```cmd/sh
sudo apt-get update
sudo apt-get install moby-engine
sudo apt-get install moby-cli
```

## <a name="install-the-azure-iot-edge-security-daemon"></a>De Daemon van de beveiliging Azure IoT-rand installeren

Onderstaande opdrachten installeert ook de standaardversie van de **iothsmlib** als dat niet al aanwezig zijn.

```cmd/sh
sudo apt-get update
sudo apt-get install iotedge
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
sudo iotedge list
```

## <a name="next-steps"></a>Volgende stappen

Als u problemen ondervindt met de installatie naar behoren afhandeling Edge-runtime de [probleemoplossing] [ lnk-trouble] pagina.

<!-- Links -->
[lnk-dcs]: ../iot-hub/quickstart-send-telemetry-dotnet.md#register-a-device
[lnk-dps]: how-to-simulate-dps-tpm.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
