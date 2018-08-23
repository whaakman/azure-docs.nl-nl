---
title: Over het installeren van Azure IoT Edge op Linux | Microsoft Docs
description: Azure IoT Edge-installatie-instructies op Linux
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/14/2018
ms.author: kgremban
ms.openlocfilehash: 5cd12d4fab97f295cad1e0ea06112fc53e376b12
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42060151"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>De Azure IoT Edge-runtime installeren in Linux (x64)

De Azure IoT Edge-runtime wordt geïmplementeerd op alle IoT Edge-apparaten. Deze bevat drie onderdelen. De **IoT Edge security daemon** biedt en onderhoudt beveiligingsstandaarden op het Edge-apparaat. De daemon begint op elke keer opstarten en bootstrapt van het apparaat door de IoT Edge-agent wordt gestart. De **IoT Edge agent** vergemakkelijkt de implementatie en bewaking van modules op het Edge-apparaat, met inbegrip van de IoT Edge hub. Met de **IoT Edge-hub** wordt de communicatie tussen modules op het IoT Edge-apparaat en tussen het apparaat en IoT Hub beheerd.

In dit artikel vindt u de stappen voor het installeren van de Azure IoT Edge-runtime op uw Linux x64 (Intel/AMD) Edge-apparaat.

>[!NOTE]
>Pakketten in de opslagplaatsen met Linux-software zijn afhankelijk van de licentievoorwaarden die zich in elk pakket (/ usr/delen/Docs/*pakketnaam*). Lees de licentievoorwaarden voordat u het pakket. De installatie en het gebruik van het pakket wordt verstaan onder uw acceptatie van deze voorwaarden. Als u niet akkoord met de licentievoorwaarden gaat, moet u het pakket niet gebruiken.

## <a name="register-microsoft-key-and-software-repository-feed"></a>Feed voor Microsoft-sleutel en software-opslagplaats registreren

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

# Perform apt upgrade
sudo apt-get upgrade
```

## <a name="install-the-container-runtime"></a>De container-runtime installeren 

Azure IoT Edge is afhankelijk van een [OCI-compatibele] [ lnk-oci] container-runtime. Voor productiescenario's, het is raadzaam gebruikt u de [Moby gebaseerde] [ lnk-moby] engine hieronder. Dit is de enige container-engine officieel ondersteund met Azure IoT Edge. Containerinstallatiekopieën met docker CE/EE zijn compatibel met de runtime Moby.

Apt-get bijwerken.

```bash
sudo apt-get update
```

De engine Moby installeren. 

```bash
sudo apt-get install moby-engine
```

Installeer de Moby-opdrachtregelinterface (CLI). De CLI is handig voor het ontwikkelen van, maar is optioneel voor productie-implementaties.

```bash
sudo apt-get install moby-cli
```

## <a name="install-the-azure-iot-edge-security-daemon"></a>De Daemon van de beveiliging met Azure IoT Edge installeren

De standaardversie van zal ook installeren door onderstaande opdrachten de **iothsmlib** als dit nog niet geïnstalleerd.

```bash
sudo apt-get update
sudo apt-get install iotedge
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

Na het invoeren van de Inrichtingsgegevens in de configuratie, start u de daemon opnieuw:

```cmd/sh
sudo systemctl restart iotedge
```

>[!TIP]
>U moet verhoogde bevoegdheden hebben om uit te voeren `iotedge` opdrachten. Nadat u uw computer afmelden en meld u opnieuw aan de eerste keer na de installatie van de IoT Edge-runtime, worden de machtigingen automatisch bijgewerkt. Gebruik tot die tijd kun **sudo** vóór de opdrachten. 

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

## <a name="next-steps"></a>Volgende stappen

Als u hebt met het Edge-runtime niet goed geïnstalleerd problemen, bekijk de [probleemoplossing] [ lnk-trouble] pagina.

<!-- Links -->
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-linux.md
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
[lnk-trouble]: troubleshoot.md
