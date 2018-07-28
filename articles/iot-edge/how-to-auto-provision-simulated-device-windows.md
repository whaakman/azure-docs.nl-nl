---
title: Azure IoT Edge-apparaat automatisch inrichten met DPS - Windows | Microsoft Docs
description: Een gesimuleerd apparaat op uw Windows-computer gebruiken voor het testen van automatische apparaatinrichting voor Azure IoT Edge met Device Provisioning Service
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: e149886e1ade80d7751f58eb1f77031c4e432b75
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39307940"
---
# <a name="create-and-provision-a-simulated-tpm-edge-device-on-windows"></a>Maken en inrichten van een gesimuleerd TPM-Edge-apparaat op Windows

Azure IoT Edge-apparaten kunnen worden automatisch kan worden ingericht met behulp van de [Device Provisioning Service](../iot-dps/index.yml) net als bij apparaten die niet Microsoft edge-functionaliteit. Als u niet bekend met het proces van automatische inrichting bent, raadpleegt u de [concepten voor automatische inrichting](../iot-dps/concepts-auto-provisioning.md) voordat u doorgaat. 

Dit artikel leest u hoe voor het testen van automatische inrichting op een gesimuleerd Edge-apparaat met de volgende stappen uit: 

* Maak een exemplaar van de IoT Hub Device Provisioning Service (DPS).
* Een gesimuleerd apparaat maakt op uw Windows-machine met een gesimuleerde Trusted Platform Module (TPM) voor hardwarebeveiliging.
* Maak een afzonderlijke inschrijving voor het apparaat.
* Installeer de IoT Edge-runtime en verbind het apparaat met IoT Hub.

## <a name="prerequisites"></a>Vereisten

* Een Windows-ontwikkelcomputer. In dit artikel wordt gebruikgemaakt van Windows 10. 
* Een actieve IoT-Hub. 

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>De IoT Hub Device Provisioning Service instellen

Maak een nieuw exemplaar van de IoT Hub Device Provisioning Service in Azure en een koppeling naar uw IoT hub. U kunt de instructies in [instellen van de IoT Hub-DPS](../iot-dps/quick-setup-auto-provision.md).

Nadat u de Device Provisioning Service die wordt uitgevoerd hebt, kopieert u de waarde van **ID-bereik** van de overzichtspagina. U gebruikt deze waarde bij het configureren van de IoT Edge-runtime. 

## <a name="simulate-a-tpm-device"></a>TPM-apparaat simuleren

Maak een gesimuleerd TPM-apparaat op uw Windows-ontwikkelcomputer. Ophalen van de **registratie-ID** en **Endorsement Key** voor uw apparaat en deze gebruiken om u te maken van een afzonderlijke inschrijvingsvermelding in DPS. 

Wanneer u een inschrijving in DPS maakt, hebt u de mogelijkheid om te declareren een **oorspronkelijke Apparaatdubbelstatus**. U kunt in de apparaatdubbel labels instellen om apparaten te groeperen door elke meetwaarde die u nodig hebt in uw oplossing, zoals regio, omgeving, locatie, of het apparaat. Deze tags zijn gebruikt voor het maken [automatische implementaties](how-to-deploy-monitor.md). 

De SDK-taal die u gebruiken wilt voor het maken van het gesimuleerde apparaat kiezen en volg de stappen totdat u de afzonderlijke registratie maken. 

Wanneer u de afzonderlijke registratie maakt, selecteert u **inschakelen** op te geven dat deze virtuele machine is een **IoT Edge-apparaat**.

Gesimuleerd apparaat en handleidingen voor afzonderlijke inschrijving: 
* [C](../iot-dps/quick-create-simulated-device.md)
* [Java](../iot-dps/quick-create-simulated-device-tpm-java.md)
* [C#](../iot-dps/quick-create-simulated-device-tpm-csharp.md)
* [Node.js](../iot-dps/quick-create-simulated-device-tpm-node.md)
* [Python](../iot-dps/quick-create-simulated-device-tpm-python.md)

Na het maken van de afzonderlijke registratie, sla de waarde van de **registratie-ID**. U gebruikt deze waarde bij het configureren van de IoT Edge-runtime. 

## <a name="install-the-iot-edge-runtime"></a>IoT Edge-runtime installeren

De IoT Edge-runtime wordt op alle IoT Edge-apparaten geïmplementeerd. De onderdelen in containers worden uitgevoerd, en kunnen u extra containers implementeren op het apparaat, zodat u kunt de code aan de rand uitvoeren. Op apparaten waarop Windows wordt uitgevoerd, kunt u Windows-containers of Linux-containers gebruiken. Kies het type van de containers die u wilt gebruiken en volg de stappen. Zorg ervoor dat u de IoT Edge-runtime voor het inrichten van automatische, niet handmatig configureren. 

Volg de instructies voor het installeren van de IoT Edge-runtime op het apparaat dat het gesimuleerde TPM uit de vorige sectie wordt uitgevoerd. 

Kent uw DPS **ID-bereik** en apparaat **registratie-ID** voordat u begint met deze artikelen. 

* [Windows-containers](how-to-install-iot-edge-windows-with-windows.md)
* [Linux-containers](how-to-install-iot-edge-windows-with-linux.md)

## <a name="create-a-tpm-environment-variable"></a>Maken van een TPM-omgevingsvariabele

Wijzig op de machine waarop uw gesimuleerde apparaat wordt uitgevoerd, de **iotedge** registersleutels voor de service om in te stellen van een omgevingsvariabele.

1. Uit de **Start** menu open **regedit**. 
2. Navigeer naar **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\iotedge**. 
3. Selecteer **bewerken** > **nieuwe** > **waarde met meerdere tekenreeksen**. 
4. Voer de naam **omgeving**. 
5. Dubbelklik op de nieuwe variabele en stel de waardegegevens in op **IOTEDGE_USE_TPM_DEVICE = ON**. 
6. Klik op **OK** om uw wijzigingen op te slaan. 

## <a name="restart-the-iot-edge-runtime"></a>Opnieuw opstarten van de IoT Edge-runtime

IoT Edge-runtime opnieuw zodat deze neemt alle configuratiewijzigingen die u hebt aangebracht op het apparaat. 

```powershell
Stop-Service iotedge -NoWait
sleep 5
Start-Service iotedge
```

## <a name="verify-successful-installation"></a>Controleer of geslaagde installatie

Als de runtime is gestart, kunt u met ingang van uw IoT-Hub en Zie dat het nieuwe apparaat automatisch is ingericht en gereed is voor de IoT Edge-modules uitvoeren. 

Controleer de status van de IoT Edge-service.

```powershell
Get-Service iotedge
```

Bekijk Logboeken van de laatste 5 minuten.

```powershell
# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false} |
  format-table -autosize -wrap
```

Lijst met modules.

```powershell
iotedge list
```

## <a name="next-steps"></a>Volgende stappen

Het inschrijvingsproces Device Provisioning Service kunt u instellen de apparaat-ID en het apparaat dubbele tags op hetzelfde moment als u het nieuwe apparaat inrichten. Deze waarden kunt u afzonderlijke apparaten of groepen van apparaten met behulp van automatische Apparaatbeheer. Meer informatie over het [implementeren en te bewaken IoT Edge-modules op schaal met Azure portal](how-to-deploy-monitor.md) of [met behulp van Azure CLI](how-to-deploy-monitor-cli.md)