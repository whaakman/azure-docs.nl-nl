---
title: Azure IoT Edge-apparaat automatisch inrichten met DPS - Linux | Microsoft Docs
description: Een gesimuleerd TPM gebruiken op een Linux-VM voor het testen van apparaat inrichten voor Azure IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 2416d70e3c9d0f66ff755d8049e78e0275c89258
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232066"
---
# <a name="create-and-provision-an-edge-device-with-a-virtual-tpm-on-a-linux-virtual-machine"></a>Een Edge-apparaat met een virtuele TPM op een Linux-machine maken en inrichten

Azure IoT Edge-apparaten kunnen worden automatisch kan worden ingericht met behulp van de [Device Provisioning Service](../iot-dps/index.yml) net als bij apparaten die niet Microsoft edge-functionaliteit. Als u niet bekend met het proces van automatische inrichting bent, raadpleegt u de [concepten voor automatische inrichting](../iot-dps/concepts-auto-provisioning.md) voordat u doorgaat. 

Dit artikel leest u hoe voor het testen van automatische inrichting op een gesimuleerd Edge-apparaat met de volgende stappen uit: 

* Maak een virtuele Linux virtuele machine (VM) in Hyper-V met een gesimuleerde Trusted Platform Module (TPM) voor hardwarebeveiliging.
* Maak een exemplaar van de IoT Hub Device Provisioning Service (DPS).
* Maken van een afzonderlijke inschrijving voor het apparaat
* Installeren van de IoT Edge-runtime en verbind het apparaat met IoT Hub

De stappen in dit artikel zijn bedoeld voor testdoeleinden.

## <a name="prerequisites"></a>Vereisten

* Een Windows-ontwikkelcomputer met [Hyper-V ingeschakeld](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v). In dit artikel maakt gebruik van Windows 10 waarop een Ubuntu-Server-VM wordt uitgevoerd. 
* Een actieve IoT-Hub. 

## <a name="create-a-linux-virtual-machine-with-a-virtual-tpm"></a>Een virtuele Linux-machine maken met een virtuele TPM

In deze sectie maakt u een nieuwe virtuele machine voor Linux op Hyper-V met een gesimuleerd TPM, zodat u deze gebruiken kunt voor het testen hoe automatische inrichting werkt met IoT Edge. 

### <a name="create-a-virtual-switch"></a>Een virtuele switch maken

Een virtuele switch kunt uw virtuele machine verbinding maken met een fysiek netwerk.

1. Open Hyper-V op uw windows-computer. 

2. In de **acties** in het menu **Virtual Switch Manager**. 

3. Kies een **externe** virtuele switch, en selecteer vervolgens **virtuele Switch maken**. 

4. Geef uw nieuwe virtuele switch een naam, bijvoorbeeld **EdgeSwitch**. Zorg ervoor dat het verbindingstype is ingesteld op **extern netwerk**en selecteer vervolgens **Ok**.

5. Een pop-upvenster waarschuwt u dat de verbinding met het netwerk kan worden onderbroken. Selecteer **Ja** om door te gaan. 

Als u fouten ziet tijdens het maken van de nieuwe virtuele switch, zorg ervoor dat er geen andere switches van de ethernet-adapter gebruikmaakt, en dat er geen andere switches dezelfde naam gebruiken. 

### <a name="create-virtual-machine"></a>Virtuele machine maken

1. Download een schijfimage-bestand wilt gebruiken voor uw virtuele machine en deze lokaal opslaat. Bijvoorbeeld, [Ubuntu server](https://www.ubuntu.com/download/server). 

2. Hiermee opent u Hyper-V opnieuw. In de **acties** in het menu **nieuw** > **virtuele Machine**.

3. Voltooi de **Wizard Nieuwe virtuele Machine** met de volgende specifieke configuraties:

   1. **Generatie opgeven**: Selecteer **van de 2e generatie**.
   2. **Configureren van netwerken**: Stel de waarde van **verbinding** aan de virtuele switch die u in de vorige sectie hebt gemaakt. 
   3. **Installatie Oprtions**: Selecteer **een besturingssysteem installeren vanaf een opstartbare installatiekopie-bestand** en blader naar het schijfimage-bestand dat u lokaal hebt opgeslagen.

Het duurt een weergave-minuten om de nieuwe VM te maken. 

### <a name="enable-virtual-tpm"></a>Virtuele TPM inschakelen

1. Als uw virtuele machine is gemaakt, opent u de instellingen. 
2. Navigeer naar **Security**. 
3. Schakel het selectievakje **beveiligd opstarten inschakelen**.
4. Controleer **inschakelen Trusted Platform Module**. 
5. Klik op **OK**.  

### <a name="start-the-virtual-machine-and-collect-tpm-data"></a>Start de virtuele machine en TPM-gegevens verzamelen

In de virtuele machine, bouwt u een C-SDK-hulpprogramma dat u gebruiken kunt om op te halen van het apparaat **registratie-ID** en **Endorsement Key**. 

1. Start de virtuele machine en maak verbinding met het voltooien van het installatieproces. 

2. Volg de stappen in in uw virtuele machine, [een Linux-ontwikkelomgeving instellen](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux) te installeren en het bouwen van de Azure IoT-device-SDK voor c 

3. Voer de volgende opdrachten voor het bouwen van een C-SDK-hulpprogramma dat uw device provisioning-gegevens worden opgehaald. 

   ```bash
   cd azure-iot-sdk-c/cmake
   cmake -Duse_prov_client:BOOL=ON ..
   cd provisioning_client/tools/tpm_device_provision
   make
   sudo ./tpm_device_provision
   ```

3. Kopieer de waarden voor **registratie-ID** en **Endorsement Key**. Deze waarden kunt u een afzonderlijke inschrijving voor uw apparaat in DPS maken. 

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>De IoT Hub Device Provisioning Service instellen

Maak een nieuw exemplaar van de IoT Hub Device Provisioning Service in Azure en een koppeling naar uw IoT hub. U kunt de instructies in [instellen van de IoT Hub-DPS](../iot-dps/quick-setup-auto-provision.md).

Nadat u de Device Provisioning Service die wordt uitgevoerd hebt, kopieert u de waarde van **ID-bereik** van de overzichtspagina. U gebruikt deze waarde bij het configureren van de IoT Edge-runtime. 

## <a name="create-a-dps-enrollment"></a>Maken van een DPS-inschrijving

Ophalen van de Inrichtingsgegevens van uw virtuele machine en maakt u een afzonderlijke inschrijving in Device Provisioning Service. 

Wanneer u een inschrijving in DPS maakt, hebt u de mogelijkheid om te declareren een **oorspronkelijke Apparaatdubbelstatus**. U kunt in de apparaatdubbel labels instellen om apparaten te groeperen door elke meetwaarde die u nodig hebt in uw oplossing, zoals regio, omgeving, locatie, of het apparaat. Deze tags zijn gebruikt voor het maken [automatische implementaties](how-to-deploy-monitor.md). 


1. In de [Azure-portal](https://portal.azure.com), en navigeer naar uw IoT Hub Device Provisioning Service-exemplaar. 

2. Onder **instellingen**, selecteer **registraties beheren**. 

3. Selecteer **afzonderlijke registratie toevoegen** voltooit u de volgende stappen om de registratie te configureren:  

   1. Voor **mechanisme**, selecteer **TPM**. 
   2. Plaats de **goedkeuringssleutel** en **registratie-ID** die u hebt gekopieerd uit uw virtuele machine.
   3. Selecteer **inschakelen** op te geven dat deze virtuele machine een IoT Edge-apparaat is. 
   4. Kies de gekoppelde **IoT-Hub** dat u wilt verbinding maken met uw apparaat. 
   5. Als u wilt, moet u een ID opgeven voor uw apparaat. Apparaat-id's kunt u richten op een afzonderlijk apparaat voor de implementatie van beleidsmodule. 
   6. Een tagwaarde toevoegen aan de **oorspronkelijke Apparaatdubbelstatus** als u wilt. U kunt tags voor doelgroepen van apparaten gebruiken voor de implementatie van beleidsmodule. 
   7. Selecteer **Opslaan**. 


## <a name="install-the-iot-edge-runtime"></a>IoT Edge-runtime installeren

De IoT Edge-runtime wordt op alle IoT Edge-apparaten geïmplementeerd. De onderdelen in containers worden uitgevoerd, en kunnen u extra containers implementeren op het apparaat, zodat u kunt de code aan de rand uitvoeren. IoT Edge-runtime installeren op uw virtuele machine. 

Kent uw DPS **ID-bereik** en apparaat **registratie-ID** voordat u begint met het artikel die overeenkomt met uw apparaattype. Als u het voorbeeld van de Ubuntu-server hebt geïnstalleerd, gebruikt u de **x64** instructies. Zorg ervoor dat u de IoT Edge-runtime voor het inrichten van automatische, niet handmatig configureren. 

* [Linux (x64)](how-to-install-iot-edge-linux.md)
* [Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md)

## <a name="give-iot-edge-access-to-the-tpm"></a>IoT Edge toegang geven tot de TPM

In de volgorde voor de IoT Edge-runtime voor het automatisch inrichten van uw apparaat, moet deze toegang hebben tot de TPM. 

Gebruik de volgende stappen uit om TPM-toegang te geven. U kunt ook kunt u hetzelfde uitvoeren door te overschrijven de instellingen voor systemd zodat de *iotedge* service kan worden uitgevoerd als root. 

1. Het pad naar de TPM-hardware-module vinden op uw apparaat en sla deze op als een lokale variabele. 

   ```bash
   tpm=$(sudo find /sys -name dev -print | fgrep tpm | sed 's/.\{4\}$//')
   ```

2. Maak een nieuwe regel waarmee wordt de IoT Edge runtime toegang tot tpm0. 

   ```bash
   sudo touch /etc/udev/rules.d/tpmaccess.rules
   ```

3. Open het regelbestand. 

   ```bash
   sudo nano /etc/udev/rules.d/tpmaccess.rules
   ```

4. Kopieer de volgende toegang tot gegevens in het regelbestand. 

   ```input 
   # allow iotedge access to tpm0
   KERNEL=="tpm0", SUBSYSTEM=="tpm", GROUP="iotedge", MODE="0660"
   ```

5. Opslaan en sluiten van het bestand. 

6. Activeer de udev-systeem om te evalueren van de nieuwe regel. 

   ```bash
   /bin/udevadm trigger $tpm
   ```

7. Controleer of dat de regel is toegepast.

   ```bash
   ls -l /dev/tpm0
   ```

   Geslaagde uitvoer ziet er als volgt uit:

   ```output
   crw------- 1 root root 10, 224 Jun 28 22:34 /dev/tpm0
   ```

8. Open de IoT Edge-runtime overschrijft bestand. 

   ```bash
   sudo systemctl edit iotedge.service
   ```

9. Voeg de volgende code voor het maken van een TPM-omgevingsvariabele.

   ```input
   [Service]
   Environment=IOTEDGE_USE_TPM_DEVICE=ON
   ```

9. Controleren of de onderdrukking geslaagd is.

   ```bash
   sudo systemctl cat iotedge.service
   ```

   Geslaagde uitvoer wordt weergegeven de **iotedge** standaard variabelen voor de service en geeft u vervolgens de omgevingsvariabele op de dat u instelt **override.conf**. 

12. De instellingen voor laden.

   ```bash
   sudo systemctl daemon-reload
   ```

## <a name="restart-the-iot-edge-runtime"></a>Opnieuw opstarten van de IoT Edge-runtime

IoT Edge-runtime opnieuw zodat deze neemt alle configuratiewijzigingen die u hebt aangebracht op het apparaat. 

   ```bash
   sudo systemctl restart iotedge
   ```

Controleer of de IoT Edge-runtime wordt uitgevoerd. 

   ```bash
   sudo systemctl status iotedge
   ```

Als u fouten bij het inrichten ziet, kan het zijn dat de wijzigingen in de configuratie zijn niet doorgevoerd nog. Probeer het opnieuw opstarten van de IoT Edge-daemon krijgen. 

   ```bash
   sudo systemctl daemon-reload
   ```
   
Of probeer het opnieuw opstarten van uw virtuele machine om te zien als de wijzigingen van kracht op een nieuwe start. 

## <a name="verify-successful-installation"></a>Controleer of geslaagde installatie

Als de runtime is gestart, kunt u met ingang van uw IoT-Hub en Zie dat het nieuwe apparaat automatisch is ingericht en gereed is voor de IoT Edge-modules uitvoeren. 

Controleer de status van de IoT Edge-Daemon.

```cmd/sh
systemctl status iotedge
```

Controleer de daemon-Logboeken.

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Lijst met modules.

```cmd/sh
iotedge list
```


## <a name="next-steps"></a>Volgende stappen

Het inschrijvingsproces Device Provisioning Service kunt u instellen de apparaat-ID en het apparaat dubbele tags op hetzelfde moment als u het nieuwe apparaat inrichten. Deze waarden kunt u afzonderlijke apparaten of groepen van apparaten met behulp van automatische Apparaatbeheer. Meer informatie over het [implementeren en te bewaken IoT Edge-modules op schaal met Azure portal](how-to-deploy-monitor.md) of [met behulp van Azure CLI](how-to-deploy-monitor-cli.md)
