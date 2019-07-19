---
title: Azure IoT Edge installeren in Linux | Microsoft Docs
description: Azure IoT Edge-installatie-instructies op Linux AMD64-apparaten met Ubuntu
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 822efe2534d49c0995a672232107cc322e547989
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "68227512"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>De Azure IoT Edge-runtime installeren in Linux (x64)

De Azure IoT Edge-runtime is wat een apparaat verandert in een IoT Edge-apparaat. De runtime kan worden geïmplementeerd op apparaten als klein is als een Raspberry Pi of even groot zijn als een industrie-server. Wanneer een apparaat is geconfigureerd met de IoT Edge-runtime, kun u bedrijfslogica toe vanuit de cloud implementeren.

Zie [inzicht krijgen in de Azure IOT Edge runtime en de bijbehorende architectuur](iot-edge-runtime.md)voor meer informatie.

In dit artikel worden de stappen beschreven voor het installeren van de Azure IoT Edge runtime op uw Ubuntu Linux x64-(Intel/AMD) IoT Edge apparaat. Raadpleeg [Azure IOT Edge ondersteunde systemen](support.md#operating-systems) voor een lijst met ondersteunde amd64-besturings systemen.

> [!NOTE]
> Pakketten in de opslagplaatsen met Linux-software zijn afhankelijk van de licentievoorwaarden die zich in elk pakket (/ usr/delen/Docs/*pakketnaam*). Lees de licentievoorwaarden voordat u het pakket. De installatie en het gebruik van het pakket wordt verstaan onder uw acceptatie van deze voorwaarden. Als u niet akkoord met de licentievoorwaarden gaat, moet u het pakket niet gebruiken.

## <a name="install-the-latest-version"></a>De nieuwste versie installeren

Gebruik de volgende secties om de meest recente versie van de Azure IoT Edge-service op uw apparaten te installeren. 

### <a name="register-microsoft-key-and-software-repository-feed"></a>Feed voor Microsoft-sleutel en software-opslagplaats registreren

Bereid uw apparaat voor op de installatie van IoT Edge runtime.


Installeer de configuratie van de opslag plaats. Kies naar wens het **16,04** -of **18,04** code fragment voor uw release van Ubuntu:

> [!NOTE]
> Zorg ervoor dat u het code fragment selecteert in het vak juiste code voor uw versie van Ubuntu.

* Voor **Ubuntu 16,04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   ```

* Voor **Ubuntu 18,04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > ./microsoft-prod.list
   ```
   
Kopieer de gegenereerde lijst.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

Open bare sleutel van micro soft GPG installeren

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-the-container-runtime"></a>De container-runtime installeren

Azure IoT Edge is afhankelijk van een [met OCI compatibele](https://www.opencontainers.org/) container runtime. Voor productie scenario's wordt u aangeraden de [op Moby gebaseerde](https://mobyproject.org/) engine te gebruiken die hieronder wordt beschreven. Dit is de enige container-engine officieel ondersteund met Azure IoT Edge. Containerinstallatiekopieën met docker CE/EE zijn compatibel met de runtime Moby.

Voer apt-update uit.

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

#### <a name="verify-your-linux-kernel-for-moby-compatibility"></a>De Linux-kernel voor Moby-compatibiliteit controleren

Veel fabrikanten van Inge sloten apparaten verzenden apparaat-installatie kopieën die aangepaste Linux-kernels bevatten waarvoor mogelijk functies ontbreken die vereist zijn voor compatibiliteit met container-runtime. Als u problemen ondervindt bij het installeren van de aanbevolen [Moby](https://github.com/moby/moby) -container runtime, kunt u mogelijk problemen met de configuratie van de Linux-kernel oplossen met behulp van het [controle-](https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh) configuratie script dat is opgegeven in de officiële [Moby github-opslag plaats](https://github.com/moby/moby) door de volgende opdrachten op het apparaat uit te voeren.

   ```bash
   curl -sSL https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh -o check-config.sh
   chmod +x check-config.sh
   ./check-config.sh
   ```

Dit geeft een gedetailleerde uitvoer die de status bevat van de kernel-functies die worden gebruikt door de Moby-runtime. U moet ervoor zorgen dat alle items onder `Generally Necessary` en `Network Drivers` worden ingeschakeld om ervoor te zorgen dat uw kernel volledig compatibel is met de Moby-runtime.  Als u ontbrekende onderdelen hebt geïdentificeerd, kunt u deze inschakelen door de kernel opnieuw te bouwen vanuit de bron en de bijbehorende modules te selecteren voor opname in de juiste kernel. config.  En als u een configuratie Generator van een kernel gebruikt, zoals defconfig of menuconfig, moet u de betreffende functies vinden en inschakelen en de kernel opnieuw samen stellen.  Nadat u de zojuist gewijzigde kernel hebt geïmplementeerd, voert u het script check-config opnieuw uit om te controleren of de geïdentificeerde functies correct zijn ingeschakeld.

### <a name="install-the-azure-iot-edge-security-daemon"></a>De Daemon van de beveiliging met Azure IoT Edge installeren

De **IOT Edge Security daemon** biedt en onderhoudt beveiligings standaarden op het IOT edge apparaat. De daemon begint op elke keer opstarten en het apparaat bootstrapt op basis van de rest van de IoT Edge-runtime.

De installatie opdracht installeert ook de standaard versie van de **libiothsm** als deze nog niet aanwezig is.

Voer apt-update uit.

   ```bash
   sudo apt-get update
   ```

De daemon beveiliging installeren. Het pakket is geïnstalleerd op `/etc/iotedge/`.

   ```bash
   sudo apt-get install iotedge
   ```

Zodra IoT Edge is geïnstalleerd, wordt u gevraagd het configuratie bestand bij te werken. Volg de stappen in de sectie [Configure the Azure IOT Edge Security daemon](#configure-the-azure-iot-edge-security-daemon) om de inrichting van uw apparaat te volt ooien. 

## <a name="install-a-specific-version"></a>Een specifieke versie installeren

Als u een specifieke versie van Azure IoT Edge wilt installeren, kunt u de onderdeel bestanden rechtstreeks vanuit de IoT Edge GitHub-opslag plaats richten. Gebruik de volgende stappen om alle IoT Edge onderdelen op uw apparaat op te halen: de Moby-engine en CLI, de libiothsm en ten slotte de IoT Edge Security daemon.

1. Ga naar de [Azure IOT Edge releases](https://github.com/Azure/azure-iotedge/releases)en zoek de release versie die u wilt richten. 

2. Vouw de sectie **assets** uit voor die versie.

3. Er kunnen al dan niet updates worden ontvangen voor de Moby-engine in een gegeven release. Als er bestanden worden weer geven die beginnen met **Moby-engine** en **Moby-cli**, gebruikt u de volgende opdrachten om deze onderdelen bij te werken. Als er geen Moby-bestanden worden weer gegeven en u Moby nog niet hebt geïnstalleerd op uw apparaat, gaat u terug door de oudere release-assets totdat u ze vindt. 

   1. Zoek het **Moby-engine** bestand dat overeenkomt met de architectuur van uw IOT edge-apparaat. Klik met de rechter muisknop op de bestands koppeling en kopieer het koppelings adres.

   2. Gebruik de gekopieerde koppeling in de volgende opdracht om die versie van de Moby-engine te installeren: 

      ```bash
      curl -L <moby-engine link> -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb
      ```

   3. Zoek het **Moby-cli-** bestand dat overeenkomt met de architectuur van uw IOT edge-apparaat. De Moby CLI is een optioneel onderdeel, maar kan handig zijn tijdens de ontwikkeling. Klik met de rechter muisknop op de bestands koppeling en kopieer het koppelings adres. 

   4. Gebruik de gekopieerde koppeling in de volgende opdracht om die versie van de Moby-CLI te installeren: 

      ```bash
      curl -L <moby-cli link> -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb
      ```

4. Elke release moet nieuwe bestanden hebben voor de IoT Edge Security daemon en de hsmlib. Gebruik de volgende opdrachten om deze onderdelen bij te werken. 

   1. Zoek het bestand **libiothsm-STD** dat overeenkomt met de architectuur van uw IOT edge-apparaat. Klik met de rechter muisknop op de bestands koppeling en kopieer het koppelings adres. 

   2. Gebruik de gekopieerde koppeling in de volgende opdracht om die versie van de hsmlib te installeren:

      ```bash
      curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
      ```
   
   3. Zoek het **iotedge** -bestand dat overeenkomt met de architectuur van uw IOT edge-apparaat. Klik met de rechter muisknop op de bestands koppeling en kopieer het koppelings adres. 

   4. Gebruik de gekopieerde koppeling in de volgende opdracht om die versie van de IoT Edge Security daemon te installeren. 

      ```bash
      curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
      ```

Zodra IoT Edge is geïnstalleerd, wordt u gevraagd het configuratie bestand bij te werken. Volg de stappen in de volgende sectie om het inrichten van uw apparaat te volt ooien. 

## <a name="configure-the-azure-iot-edge-security-daemon"></a>De Azure IoT Edge Security daemon configureren

Configureer de IoT Edge-runtime voor het koppelen van uw fysieke apparaat met een apparaat-id die deel uitmaakt van een Azure-IoT-hub.

De daemon kan worden geconfigureerd met behulp van het configuratiebestand op `/etc/iotedge/config.yaml`. Het bestand is tegen schrijven beveiligd standaard, moet u mogelijk de verhoogde machtigingen om deze te bewerken.

Een enkele IoT Edge-apparaat kan worden ingericht handmatig met behulp van een apparaat verbindingen tekenreeks opgegeven door de IoT Hub. Of u de Device Provisioning Service kunt gebruiken voor het automatisch inrichten van apparaten, dit is handig wanneer er veel apparaten zijn om in te richten. Afhankelijk van inrichting keuze, kiest u het script voor de juiste installatie.

### <a name="option-1-manual-provisioning"></a>Optie 1: Hand matig inrichten

Voor het handmatig inrichten van een apparaat, moet u opgeven met een [apparaatverbindingsreeks](how-to-register-device-portal.md) dat u maken kunt met een nieuwe apparaat wordt geregistreerd bij uw IoT-hub.

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

Als u gebruikt de **handmatige configuratie** stappen in de vorige sectie, IoT Edge-runtime moet is ingericht en wordt uitgevoerd op uw apparaat. Als u gebruikt de **automatische configuratie** stappen, moet u enkele extra stappen uitvoeren zodat de runtime uw apparaat met uw IoT-hub uit uw naam registreren kan. Zie voor de volgende stappen [een gesimuleerde TPM maken en inrichten IOT edge apparaat op een virtuele Linux-machine](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

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

Raadpleeg de pagina [probleem oplossing](troubleshoot.md) als u problemen ondervindt met het installeren van IOT Edge runtime.

Zie [de IOT Edge Security daemon en runtime bijwerken](how-to-update-iot-edge.md)als u een bestaande installatie wilt bijwerken naar de nieuwste versie van IOT Edge.
