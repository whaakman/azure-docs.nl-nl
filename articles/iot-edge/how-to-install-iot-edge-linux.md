---
title: Azure IoT Edge installeren in Linux | Microsoft Docs
description: Azure IoT Edge-installatie-instructies op Linux AMD64-apparaten met Ubuntu
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 398b984f4d97005fdc4d749f3fe072423cc5bbd7
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57309295"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>De Azure IoT Edge-runtime installeren in Linux (x64)

De Azure IoT Edge-runtime is wat een apparaat verandert in een IoT Edge-apparaat. De runtime kan worden geïmplementeerd op apparaten als klein is als een Raspberry Pi of even groot zijn als een industrie-server. Wanneer een apparaat is geconfigureerd met de IoT Edge-runtime, kun u bedrijfslogica toe vanuit de cloud implementeren.

Zie voor meer informatie, [inzicht in de Azure IoT Edge-runtime en de bijbehorende architectuur](iot-edge-runtime.md).

In dit artikel vindt u de stappen voor het installeren van de Azure IoT Edge-runtime op uw Linux x64 (Intel/AMD) IoT Edge-apparaat. Raadpleeg [ondersteuning voor Azure IoT Edge](support.md#operating-systems) voor een lijst van ondersteunde AMD64-besturingssystemen.

> [!NOTE]
> Pakketten in de opslagplaatsen met Linux-software zijn afhankelijk van de licentievoorwaarden die zich in elk pakket (/ usr/delen/Docs/*pakketnaam*). Lees de licentievoorwaarden voordat u het pakket. De installatie en het gebruik van het pakket wordt verstaan onder uw acceptatie van deze voorwaarden. Als u niet akkoord met de licentievoorwaarden gaat, moet u het pakket niet gebruiken.

## <a name="register-microsoft-key-and-software-repository-feed"></a>Feed voor Microsoft-sleutel en software-opslagplaats registreren

Uw apparaat voorbereiden voor de IoT-Edge runtime-installatie.


Installeer de configuratie van de opslagplaats. Vervang **\<release\>** met **16.04** of **18.04** die geschikt zijn voor uw versie van Ubuntu.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/<release>/prod.list > ./microsoft-prod.list
   ```

Kopieer de gegenereerde lijst.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

Openbare Microsoft GPG-sleutel installeren

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

## <a name="install-the-container-runtime"></a>De container-runtime installeren

Azure IoT Edge is afhankelijk van een [OCI-compatibele](https://www.opencontainers.org/) container-runtime. Voor productiescenario's, raden we u ook de [Moby gebaseerde](https://mobyproject.org/) engine hieronder. Dit is de enige container-engine officieel ondersteund met Azure IoT Edge. Containerinstallatiekopieën met docker CE/EE zijn compatibel met de runtime Moby.

Apt-update uitvoeren.

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

De **IoT Edge security daemon** biedt en onderhoudt beveiligingsstandaarden op het IoT Edge-apparaat. De daemon begint op elke keer opstarten en het apparaat bootstrapt op basis van de rest van de IoT Edge-runtime.

De installatieopdracht installeert ook de standaardversie van de **iothsmlib** als dit nog niet geïnstalleerd.

Apt-update uitvoeren.

   ```bash
   sudo apt-get update
   ```

De daemon beveiliging installeren. Het pakket is geïnstalleerd op `/etc/iotedge/`.

   ```bash
   sudo apt-get install iotedge
   ```

## <a name="configure-the-azure-iot-edge-security-daemon"></a>De Daemon van de beveiliging met Azure IoT Edge configureren

Configureer de IoT Edge-runtime voor het koppelen van uw fysieke apparaat met een apparaat-id die deel uitmaakt van een Azure-IoT-hub.

De daemon kan worden geconfigureerd met behulp van het configuratiebestand op `/etc/iotedge/config.yaml`. Het bestand is tegen schrijven beveiligd standaard, moet u mogelijk de verhoogde machtigingen om deze te bewerken.

Een enkele IoT Edge-apparaat kan worden ingericht handmatig met behulp van een apparaat verbindingen tekenreeks opgegeven door de IoT Hub. Of u de Device Provisioning Service kunt gebruiken voor het automatisch inrichten van apparaten, dit is handig wanneer er veel apparaten zijn om in te richten. Afhankelijk van inrichting keuze, kiest u het script voor de juiste installatie.

### <a name="option-1-manual-provisioning"></a>Optie 1: Handmatige inrichting

Voor het handmatig inrichten van een apparaat, moet u opgeven met een [apparaatverbindingsreeks](how-to-register-device-portal.md) dat u maken kunt met een nieuwe apparaat wordt geregistreerd bij uw IoT-hub.

Open het configuratiebestand.

```bash
sudo nano /etc/iotedge/config.yaml
```

De inrichting-sectie van het bestand vinden. Verwijder de opmerking bij de **handmatige** inrichtingsmodus en zorg ervoor dat de dps inrichtingsmodus is opgenomen als opmerking. Werk de waarde van **device_connection_string** door de verbindingsreeks van uw IoT Edge-apparaat.

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

De inrichting-sectie van het bestand vinden. Verwijder de opmerking bij de **dps** modus en uitcommentarieer de handmatige sectie wordt ingericht. De waarden voor **scope_id** en **registration_id** door de waarden van uw IoT Hub Device Provisioning Service en uw IoT Edge-apparaat met TPM.

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

Als u gebruikt de **handmatige configuratie** stappen in de vorige sectie, IoT Edge-runtime moet is ingericht en wordt uitgevoerd op uw apparaat. Als u gebruikt de **automatische configuratie** stappen, moet u enkele extra stappen uitvoeren zodat de runtime uw apparaat met uw IoT-hub uit uw naam registreren kan. Zie voor de volgende stappen [een gesimuleerd TPM IoT Edge-apparaat op een Linux-machine maken en inrichten](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

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

Op apparaten met beperkte resource, het is raadzaam dat u instellen de *OptimizeForPerformance* omgevingsvariabele *false* volgens de instructies in de [problemen oplossen met ](troubleshoot.md).

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

Als u hebt met de IoT Edge-runtime niet goed geïnstalleerd problemen, bekijk de [probleemoplossing](troubleshoot.md) pagina.

Als u wilt een bestaande installatie bijwerken naar de nieuwste versie van IoT Edge, Zie [bijwerken van de IoT Edge security-daemon en de runtime](how-to-update-iot-edge.md).
