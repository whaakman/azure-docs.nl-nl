---
title: Azure IoT Edge installeren op Linux ARM32 | Microsoft Docs
description: Installatie-instructies Azure IoT Edge op Linux op ARM32-apparaten, zoals een Raspberry PI
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: kgremban
ms.openlocfilehash: 72349597a2831a64cb11ff66b30e6948f34cc921
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "68224698"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Azure IoT Edge runtime installeren op Linux (ARM32v7/armhf)

De Azure IoT Edge-runtime is wat een apparaat verandert in een IoT Edge-apparaat. De runtime kan worden geïmplementeerd op apparaten als klein is als een Raspberry Pi of even groot zijn als een industrie-server. Wanneer een apparaat is geconfigureerd met de IoT Edge-runtime, kun u bedrijfslogica toe vanuit de cloud implementeren. 

Zie voor meer informatie over de werking van de IoT Edge-runtime en welke onderdelen zijn opgenomen, [inzicht in de Azure IoT Edge-runtime en de bijbehorende architectuur](iot-edge-runtime.md).

Dit artikel bevat de stappen voor het installeren van de Azure IoT Edge runtime op een Linux ARM32v7/armhf IoT Edge-apparaat. Deze stappen werken bijvoorbeeld voor Raspberry Pi-apparaten. Zie [Azure IOT Edge ondersteunde systemen](support.md#operating-systems)voor een lijst met ondersteunde ARM32-besturings systemen. 

>[!NOTE]
>Pakketten in de opslagplaatsen met Linux-software zijn afhankelijk van de licentievoorwaarden die zich in elk pakket (/ usr/delen/Docs/*pakketnaam*). Lees de licentievoorwaarden voordat u het pakket. De installatie en het gebruik van het pakket wordt verstaan onder uw acceptatie van deze voorwaarden. Als u niet akkoord met de licentievoorwaarden gaat, moet u het pakket niet gebruiken.

## <a name="install-the-latest-version"></a>De nieuwste versie installeren

Gebruik de volgende secties om de meest recente versie van de Azure IoT Edge-service op uw Linux ARM-apparaten te installeren. 

### <a name="install-the-container-runtime"></a>De container-runtime installeren

Azure IoT Edge is afhankelijk van een [met OCI compatibele](https://www.opencontainers.org/) container runtime. Voor productiescenario's, het is raadzaam gebruikt u de [Moby gebaseerde](https://mobyproject.org/) engine hieronder. Dit is de enige container-engine officieel ondersteund met Azure IoT Edge. Docker CE/EE container installatie kopieën zijn compatibel met de op Moby gebaseerde runtime.

Met de volgende opdrachten installeert u zowel de Moby-engine als de opdracht regel interface (CLI). De CLI is handig voor het ontwikkelen van, maar is optioneel voor productie-implementaties.

```bash
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the moby-engine
curl -L https://aka.ms/moby-engine-armhf-latest -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb

# Download and install the moby-cli
curl -L https://aka.ms/moby-cli-armhf-latest -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb

# Run apt-get fix
sudo apt-get install -f
```

### <a name="install-the-iot-edge-security-daemon"></a>Installeer de IoT Edge Security daemon

De **IOT Edge Security daemon** biedt en onderhoudt beveiligings standaarden op het IOT edge apparaat. De daemon begint op elke keer opstarten en het apparaat bootstrapt op basis van de rest van de IoT Edge-runtime. 


```bash
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the standard libiothsm implementation
curl -L https://aka.ms/libiothsm-std-linux-armhf-latest -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb

# Download and install the IoT Edge Security Daemon
curl -L https://aka.ms/iotedged-linux-armhf-latest -o iotedge.deb && sudo dpkg -i ./iotedge.deb

# Run apt-get fix
sudo apt-get install -f
```

Zodra IoT Edge is geïnstalleerd, wordt u gevraagd het configuratie bestand bij te werken. Volg de stappen in de sectie [Configure the Azure IOT Edge Security daemon](#configure-the-azure-iot-edge-security-daemon) om de inrichting van uw apparaat te volt ooien. 

## <a name="install-a-specific-version"></a>Een specifieke versie installeren

Als u een specifieke versie van Azure IoT Edge wilt installeren, kunt u de onderdeel bestanden rechtstreeks vanuit de IoT Edge GitHub-opslag plaats richten. Gebruik dezelfde `curl` opdrachten die worden vermeld in de vorige secties om alle IOT Edge onderdelen op uw apparaat op te halen: de Moby-engine en CLI, de libiothsm en ten slotte de IOT Edge Security daemon. Het enige verschil is dat u de **aka.MS** -url's vervangt door koppelingen die rechtstreeks verwijzen naar de versie van elk onderdeel dat u wilt gebruiken.

Ga naar de [Azure IOT Edge releases](https://github.com/Azure/azure-iotedge/releases)en zoek de release versie die u wilt richten. Vouw de sectie **assets** uit voor de versie en kies de bestanden die overeenkomen met de architectuur van uw IOT edge-apparaat. Elke IoT Edge versie bevat **iotedge** -en **libiothsm** -bestanden. Niet alle releases bevatten **Moby-engine** of **Moby-cli**. Als u de Moby-container Engine nog niet hebt geïnstalleerd, doorzoekt u de oudere versies totdat u er een hebt gevonden die de Moby-onderdelen bevat. 

Zodra IoT Edge is geïnstalleerd, wordt u gevraagd het configuratie bestand bij te werken. Volg de stappen in de volgende sectie om het inrichten van uw apparaat te volt ooien. 

## <a name="configure-the-azure-iot-edge-security-daemon"></a>De Azure IoT Edge Security daemon configureren

Configureer de IoT Edge-runtime voor het koppelen van uw fysieke apparaat met een apparaat-id die deel uitmaakt van een Azure-IoT-hub. 

De daemon kan worden geconfigureerd met behulp van het configuratiebestand op `/etc/iotedge/config.yaml`. Het bestand is standaard beveiligd tegen schrijven, dus u hebt mogelijk verhoogde machtigingen nodig om het te bewerken.

Een enkele IoT Edge-apparaat kan worden ingericht handmatig met behulp van een apparaat verbindingen tekenreeks opgegeven door de IoT Hub. Of u de Device Provisioning Service kunt gebruiken voor het automatisch inrichten van apparaten, dit is handig wanneer er veel apparaten zijn om in te richten. Afhankelijk van inrichting keuze, kiest u het script voor de juiste installatie. 

### <a name="option-1-manual-provisioning"></a>Optie 1: Hand matig inrichten

Als u een apparaat hand matig wilt inrichten, moet u het voorzien van een [apparaat Connection String](how-to-register-device-portal.md) dat u kunt maken door een nieuw IOT edge-apparaat te registreren in uw IOT-hub.

Open het configuratiebestand. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Zoek de inrichtings configuraties van het bestand en verwijder de sectie **hand matige inrichtings configuratie** . Werk de waarde van **device_connection_string** door de verbindingsreeks van uw IoT Edge-apparaat. Zorg ervoor dat alle andere inrichtings secties van commentaar zijn.

```yaml
# Manual provisioning configuration
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
# DPS TPM provisioning configuration
# provisioning:
#   source: "dps"
#   global_endpoint: "https://global.azure-devices-provisioning.net"
#   scope_id: "{scope_id}"
#   attestation:
#     method: "tpm"
#     registration_id: "{registration_id}"
```

Sla het bestand op en sluit het. 

`CTRL + X`, `Y`, `Enter`

Na het invoeren van de Inrichtingsgegevens in het configuratiebestand, start u de daemon opnieuw:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>Optie 2: Automatische inrichting

Voor het automatisch inrichten van een apparaat, [Device Provisioning Service instellen en ophalen van uw apparaat registratie-ID](how-to-auto-provision-simulated-device-linux.md). Er zijn een aantal Attestation-mechanismen die door IoT Edge worden ondersteund bij het gebruik van automatische inrichting, maar uw hardware-vereisten zijn ook van invloed op uw keuzes. Zo worden Raspberry Pi-apparaten standaard niet geleverd met een Trusted Platform Module TPM-chip.

Open het configuratiebestand. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Zoek de inrichtings configuraties van het bestand en verwijder de sectie die geschikt is voor uw Attestation-mechanisme. Bij het gebruik van TPM-Attestation kunt u bijvoorbeeld de waarden van **scope_id** en **registration_id** bijwerken met de waarden van uw IOT hub Device Provisioning-Service en uw IOT edge apparaat met TPM.

```yaml
   # Manual provisioning configuration
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # DPS TPM provisioning configuration
   provisioning:
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     attestation:
       method: "tpm"
       registration_id: "{registration_id}"
```

Sla het bestand op en sluit het. 

`CTRL + X`, `Y`, `Enter`

Na het invoeren van de Inrichtingsgegevens in het configuratiebestand, start u de daemon opnieuw:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Controleer of geslaagde installatie

Als u gebruikt de **handmatige configuratie** stappen in de vorige sectie, IoT Edge-runtime moet is ingericht en wordt uitgevoerd op uw apparaat. Of, als u de stappen voor **automatische configuratie** hebt gebruikt, moet u een aantal extra stappen uitvoeren zodat de runtime uw apparaat kan registreren bij uw IOT-hub namens u. Zie voor de volgende stappen [een gesimuleerde TPM maken en inrichten IOT edge apparaat op een virtuele Linux-machine](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

U kunt de status van het gebruik van de IoT Edge-Daemon controleren:

```bash
systemctl status iotedge
```

Controleer de daemon-logboeken met behulp van:

```bash
journalctl -u iotedge --no-pager --no-full
```

En lijst met modules met:

```bash
sudo iotedge list
```

## <a name="tips-and-suggestions"></a>Tips en suggesties

U hebt verhoogde bevoegdheden nodig om `iotedge`-opdrachten uit te voeren. Afmelden bij de computer na de installatie van de runtime en meld u opnieuw aan uw machtigingen automatisch bijwerken. Gebruik tot die tijd kun **sudo** vóór een `iotedge` de opdrachten.

Op apparaten met beperkte resource, het is raadzaam dat u instellen de *OptimizeForPerformance* omgevingsvariabele *false* volgens de instructies in de [problemen oplossen met ](troubleshoot.md#stability-issues-on-resource-constrained-devices).

Als uw netwerk met een proxyserver, voert u de stappen in [uw IoT Edge-apparaat om te communiceren via een proxyserver configureren](how-to-configure-proxy-support.md).

## <a name="uninstall-iot-edge"></a>IoT Edge verwijderen

Als u de IoT Edge-installatie van uw Linux-apparaat wilt verwijderen, gebruikt u de volgende opdrachten vanaf de opdracht regel. 

Verwijder de IoT Edge-runtime. 

```bash
sudo apt-get remove --purge iotedge
```

Wanneer de IoT Edge runtime wordt verwijderd, wordt de door u gemaakte container gestopt, maar nog steeds aanwezig op het apparaat. Bekijk alle containers om te zien welke er aanwezig zijn. 

```bash
sudo docker ps -a
```

Verwijder de containers van het apparaat, met inbegrip van de twee runtime-containers. 

```bash
sudo docker rm -f <container name>
```

Ten slotte verwijdert u de container runtime van het apparaat. 

```bash 
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT Edge-apparaat dat is ingericht met de runtime geïnstalleerd hebt, kunt u [IoT Edge-modules implementeren](how-to-deploy-modules-portal.md).

Raadpleeg de pagina [problemen oplossen](troubleshoot.md#stability-issues-on-resource-constrained-devices) als u problemen ondervindt met het installeren van de IOT Edge-runtime.

Zie [de IOT Edge Security daemon en runtime bijwerken](how-to-update-iot-edge.md)als u een bestaande installatie wilt bijwerken naar de nieuwste versie van IOT Edge.
