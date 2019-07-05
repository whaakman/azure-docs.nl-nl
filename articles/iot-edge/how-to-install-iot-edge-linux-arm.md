---
title: Azure IoT Edge installeren op Linux ARM32 | Microsoft Docs
description: Azure IoT Edge installatie-instructies voor Linux op ARM32 apparaten, zoals een Raspberry PI
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: kgremban
ms.openlocfilehash: f7004edf2bab0e22d4d1e4c1200d6e8b8ef729b3
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485953"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Azure IoT Edge-runtime installeren in Linux (ARM32v7/armhf)

De Azure IoT Edge-runtime is wat een apparaat verandert in een IoT Edge-apparaat. De runtime kan worden geïmplementeerd op apparaten als klein is als een Raspberry Pi of even groot zijn als een industrie-server. Wanneer een apparaat is geconfigureerd met de IoT Edge-runtime, kun u bedrijfslogica toe vanuit de cloud implementeren. 

Zie voor meer informatie over de werking van de IoT Edge-runtime en welke onderdelen zijn opgenomen, [inzicht in de Azure IoT Edge-runtime en de bijbehorende architectuur](iot-edge-runtime.md).

In dit artikel bevat de stappen voor het installeren van de Azure IoT Edge-runtime op een Linux ARM32v7/armhf IoT Edge-apparaat. Bijvoorbeeld, zou moeten als volgt werken voor Raspberry Pi-apparaten. Zie voor een lijst van ondersteunde besturingssystemen voor ARM32 [Azure IoT Edge ondersteunde systemen](support.md#operating-systems). 

>[!NOTE]
>Pakketten in de opslagplaatsen met Linux-software zijn afhankelijk van de licentievoorwaarden die zich in elk pakket (/ usr/delen/Docs/*pakketnaam*). Lees de licentievoorwaarden voordat u het pakket. De installatie en het gebruik van het pakket wordt verstaan onder uw acceptatie van deze voorwaarden. Als u niet akkoord met de licentievoorwaarden gaat, moet u het pakket niet gebruiken.

## <a name="install-the-latest-version"></a>Installeer de nieuwste versie

Gebruik de volgende secties voor het installeren van de meest recente versie van de service Azure IoT Edge op uw Linux-ARM-apparaten. 

### <a name="install-the-container-runtime"></a>De container-runtime installeren

Azure IoT Edge is afhankelijk van een [OCI-compatibele](https://www.opencontainers.org/) container-runtime. Voor productiescenario's, het is raadzaam gebruikt u de [Moby gebaseerde](https://mobyproject.org/) engine hieronder. Dit is de enige container-engine officieel ondersteund met Azure IoT Edge. Containerinstallatiekopieën met docker CE/EE zijn compatibel met de runtime op basis van Moby.

De volgende opdrachten installeren de Moby gebaseerde engine en de opdrachtregelinterface (CLI). De CLI is handig voor het ontwikkelen van, maar is optioneel voor productie-implementaties.

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

### <a name="install-the-iot-edge-security-daemon"></a>De IoT Edge Security-Daemon installeren

De **IoT Edge security daemon** biedt en onderhoudt beveiligingsstandaarden op het IoT Edge-apparaat. De daemon begint op elke keer opstarten en het apparaat bootstrapt op basis van de rest van de IoT Edge-runtime. 


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

Zodra de IoT Edge is geïnstalleerd, de uitvoer u gevraagd om bij te werken van het configuratiebestand. Volg de stappen in de [configureren van de Azure IoT Edge security-daemon](#configure-the-azure-iot-edge-security-daemon) sectie klaar bent met het inrichten van uw apparaat. 

## <a name="install-a-specific-version"></a>Een specifieke versie te installeren

Als u wilt voor het installeren van een specifieke versie van Azure IoT Edge, kunt u de onderdeelbestanden rechtstreeks vanuit de IoT Edge-GitHub-opslagplaats kunt richten. Gebruik dezelfde `curl` opdrachten die worden vermeld in de vorige secties om op te halen van alle van de IoT Edge onderdelen op uw apparaat: de engine Moby en CLI, de libiothsm en ten slotte de IoT Edge-daemon voor beveiliging. Het enige verschil is dat u vervangen de **aka.ms** URL's met koppelingen die rechtstreeks verwijst naar de versie van elk onderdeel dat u wilt gebruiken.

Navigeer naar de [releases van Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases), en zoek de versie die u wilt targeten. Vouw de **activa** voor de versie uit en kies de bestanden die overeenkomen met uw IoT-Edge-architectuur van het apparaat. Elke IoT Edge-release bevat **iotedge** en **libiothsm** bestanden. Niet alle releases bevatten onder meer **moby-engine** of **moby cli**. Als u geen al de Moby container engine moet zijn geïnstalleerd, bekijken via de oudere versies totdat u een met de Moby-onderdelen. 

Zodra de IoT Edge is geïnstalleerd, de uitvoer u gevraagd om bij te werken van het configuratiebestand. Volg de stappen in de volgende sectie voor het voltooien van de inrichting van uw apparaat. 

## <a name="configure-the-azure-iot-edge-security-daemon"></a>De daemon van de Azure IoT Edge-beveiliging configureren

Configureer de IoT Edge-runtime voor het koppelen van uw fysieke apparaat met een apparaat-id die deel uitmaakt van een Azure-IoT-hub. 

De daemon kan worden geconfigureerd met behulp van het configuratiebestand op `/etc/iotedge/config.yaml`. Het bestand is beveiligd tegen schrijven standaard, dus moet u verhoogde machtigingen om deze te bewerken.

Een enkele IoT Edge-apparaat kan worden ingericht handmatig met behulp van een apparaat verbindingen tekenreeks opgegeven door de IoT Hub. Of u de Device Provisioning Service kunt gebruiken voor het automatisch inrichten van apparaten, dit is handig wanneer er veel apparaten zijn om in te richten. Afhankelijk van inrichting keuze, kiest u het script voor de juiste installatie. 

### <a name="option-1-manual-provisioning"></a>Optie 1: Handmatige inrichting

Voor het handmatig inrichten van een apparaat, moet u opgeven met een [apparaatverbindingsreeks](how-to-register-device-portal.md) dat u maken kunt met een nieuwe IoT Edge-apparaat wordt geregistreerd bij uw IoT-hub.

Open het configuratiebestand. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Ga naar het inrichtingsproces gedeelte van het bestand en verwijder opmerkingen bij de **handmatige** Inrichtingsmethode. Werk de waarde van **device_connection_string** door de verbindingsreeks van uw IoT Edge-apparaat.

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

Sla het bestand op en sluit het. 

`CTRL + X`, `Y`, `Enter`

Na het invoeren van de Inrichtingsgegevens in het configuratiebestand, start u de daemon opnieuw:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>Optie 2: Automatische inrichting

Voor het automatisch inrichten van een apparaat, [Device Provisioning Service instellen en ophalen van uw apparaat registratie-ID](how-to-auto-provision-simulated-device-linux.md). Automatische inrichting werkt alleen met apparaten met een chip Trusted Platform Module (TPM). Bijvoorbeeld, Raspberry Pi apparaten niet afkomstig zijn met TPM standaard. 

Open het configuratiebestand. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Ga naar het inrichtingsproces gedeelte van het bestand en verwijder opmerkingen bij de **dps** Inrichtingsmethode. De waarden voor **scope_id** en **registration_id** door de waarden van uw IoT Hub Device Provisioning service en uw IoT Edge-apparaat met TPM. 

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

Na het invoeren van de Inrichtingsgegevens in het configuratiebestand, start u de daemon opnieuw:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Controleer of geslaagde installatie

Als u gebruikt de **handmatige configuratie** stappen in de vorige sectie, IoT Edge-runtime moet is ingericht en wordt uitgevoerd op uw apparaat. Of, als u gebruikt de **automatische configuratie** stappen, moet u enkele extra stappen uitvoeren zodat de runtime uw apparaat met uw IoT-hub uit uw naam registreren kan. Zie voor de volgende stappen [een gesimuleerd TPM IoT Edge-apparaat op een Linux-machine maken en inrichten](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

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

## <a name="uninstall-iot-edge"></a>Verwijderen van IoT Edge

Als u de installatie van de IoT Edge van uw Linux-apparaat verwijderen wilt, gebruikt u de volgende opdrachten uit vanaf de opdrachtregel. 

Verwijder de IoT Edge-runtime. 

```bash
sudo apt-get remove --purge iotedge
```

Wanneer de IoT Edge-runtime wordt verwijderd, wordt de container die deze gemaakt zijn gestopt maar nog steeds aanwezig zijn op uw apparaat. Alle containers om te zien welke blijven weergeven. 

```bash
sudo docker ps -a
```

Verwijder de containers van uw apparaat, met inbegrip van de twee runtimecontainers. 

```bash
sudo docker rm -f <container name>
```

Ten slotte de container-runtime van uw apparaat verwijderd. 

```bash 
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT Edge-apparaat dat is ingericht met de runtime geïnstalleerd hebt, kunt u [IoT Edge-modules implementeren](how-to-deploy-modules-portal.md).

Als u problemen met de IoT Edge-runtime niet goed geïnstalleerd hebt, raadpleegt u de [probleemoplossing](troubleshoot.md#stability-issues-on-resource-constrained-devices) pagina.

Als u wilt een bestaande installatie bijwerken naar de nieuwste versie van IoT Edge, Zie [bijwerken van de IoT Edge security-daemon en de runtime](how-to-update-iot-edge.md).
